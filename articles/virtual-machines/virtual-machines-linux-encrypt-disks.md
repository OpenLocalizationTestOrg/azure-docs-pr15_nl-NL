<properties
   pageTitle="Versleutelen schijven op een VM Linux | Microsoft Azure"
   description="Coderen van schijven op een Linux VM gebruik van de Azure CLI en het implementatiemodel resourcemanager"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Schijven op een Linux VM gebruik van de Azure CLI versleutelen
Voor uitgebreide virtuele machine (VM) beveiliging en naleving, worden virtuele schijven in Azure wordt aangegeven in rust gecodeerd. Schijven zijn versleuteld met cryptografische toetsen die zijn beveiligd in een Azure-toets kluis. U deze cryptografische toetsen control en het gebruik ervan kunt controleren. Dit artikel wordt uitgelegd hoe u virtuele schijven op een Linux VM gebruik van de Azure CLI en het implementatiemodel resourcemanager versleutelen.


## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende informatie in de sectie snel het grondtal de opdrachten voor het coderen van virtuele schijven op uw VM. De rest van het document, [beginnend hier](#overview-of-disk-encryption)kunnen vindt u meer gedetailleerde informatie en context voor elke stap.

Moet u de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en aangemeld met de modus resourcemanager als volgt:

```
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Parameternamen voorbeeld opnemen `myResourceGroup`, `myKeyVault`, en `myVM`.

Eerst de provider Azure-toets kluis binnen uw Azure abonnement inschakelen en een resourcegroep maken. Het volgende voorbeeld wordt de naam van een resource-groep `myResourceGroup` in de `WestUS` locatie:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Maak een Azure belangrijke kluis. Het volgende voorbeeld wordt een toets kluis met de naam `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Een cryptografische sleutel maken in uw sleutel kluis en deze inschakelen voor Schijfopruiming versleuteling. Het volgende voorbeeld wordt een sleutel met de naam `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Een eindpunt met behulp van Azure Active Directory voor de verificatie voor de verwerking en uitwisseling van cryptografische sleutels uit kluis sleutel maken. De `--home-page` en `--identifier-uris` hoeft niet te werkelijke geschikt adres. Voor het hoogste niveau van beveiliging, moeten de client geheimen worden gebruikt in plaats van wachtwoorden. De CLI Azure kan geen momenteel client geheimen genereren. Client geheimen kunnen alleen worden gegenereerd in de portal van Azure. Het volgende voorbeeld wordt een Azure Active Directory-eindpunt met de naam `myAADApp` en gebruikt u een wachtwoord van `myPassword`. Uw eigen wachtwoord als volgt opgeven:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Opmerking de `applicationId` weergegeven in de uitvoer van de voorgaande opdracht. Deze toepassings-ID wordt gebruikt in de volgende stappen uit:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Een gegevensschijf toevoegen aan een bestaande VM. Het volgende voorbeeld wordt een gegevensschijf voor een VM met de naam `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Controleer de details voor uw sleutel kluis en de sleutel die u hebt gemaakt. U moet de sleutel kluis-ID, URI en toets URL in de laatste stap. Het volgende voorbeeld reviseert de details voor een toets kluis met de naam `myKeyVault` en van toetsen met de naam `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Coderen van uw schijven als volgt uw eigen parameternamen overal in te voeren:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

De CLI Azure biedt uitgebreide fouten niet tijdens het versleuteling. Raadpleeg voor meer informatie over probleemoplossing, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Als de voorgaande opdracht verschillende variabelen heeft en ziet u mogelijk niet veel aangegeven waarom het proces is mislukt, is een voorbeeld van de opdracht voltooid als volgt:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Ten slotte de status versleuteling nogmaals om te bevestigen dat uw virtuele schijven nu zijn gecodeerd bekijken. Het volgende voorbeeld wordt de status van een VM met de naam `myVM` in de `myResourceGroup` resourcegroep:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Overzicht van schijfversleuteling
Virtuele schijven op Linux VMs zijn versleuteld in rust [dm-crypt](https://wikipedia.org/wiki/Dm-crypt)gebruiken. Er zijn geen kosten voor het coderen van virtuele schijven in Azure wordt aangegeven. Cryptografische sleutels zijn opgeslagen in Azure-toets kluis met software-te beveiligen, of u kunt importeren of uw sleutels in Hardware beveiligingsmodules (HSM's) gecertificeerd voor FIPS 140-2 niveau 2 standaarden genereren. U de besturing van deze cryptografische toetsen behouden en het gebruik ervan kunt controleren. Deze cryptografische toetsen worden gebruikt om te versleutelen en virtuele schijven die zijn bijgevoegd bij uw VM ontsleutelen. Een eindpunt Azure Active Directory biedt een veilige methode voor deze cryptographic toetsen uitgifte, zoals VMs zijn ingeschakeld en uitgeschakeld.

Het proces voor het coderen van een VM is als volgt:

1. Maak een cryptografische sleutel in een Azure-toets kluis.
2. Configureer de cryptografische sleutel worden gebruikt voor het coderen van schijven.
3. Als u wilt lezen de cryptografische sleutel uit de kluis Azure-toets, door een eindpunt Azure Active Directory gebruikt met de juiste machtigingen te maken.
4. Hiermee geeft u de opdracht voor het coderen van uw virtuele schijven, die de Azure Active Directory-eindpunt en een juiste cryptografische sleutel moet worden gebruikt.
5. Het eindpunt van de Azure Active Directory vraagt de vereiste cryptografische sleutel van Azure-toets kluis.
6. Het virtuele schijven zijn versleuteld met de meegeleverde cryptographic toets.


## <a name="supporting-services-and-encryption-process"></a>Ondersteunende services en codering proces
Schijfversleuteling is afhankelijk van de volgende extra onderdelen:

- **Azure toets kluis** - gebruikt ter bescherming van cryptografische sleutels en geheimen gebruikt voor het proces van de ontsleutelen/schijf. 
  - Als dit bestaat, kunt u een bestaande Azure-toets kluis. U hoeft niet te reserveren van een sleutel kluis voor het coderen van schijven.
  - Als u wilt scheiden administratieve grenzen en belangrijke zichtbaarheid, kunt u een speciale toets kluis.
- **Azure Active Directory** - omgaat met de veilige uitwisseling van vereiste cryptografische sleutels en verificatie voor de gevraagde acties. 
  - U kunt een bestaand Azure Active Directory-exemplaar meestal gebruiken waarin uw toepassing. 
  - De toepassing is meer van een eindpunt voor de sleutel kluis en VM services aanvragen en krijgen de juiste cryptografische sleutels uitgegeven. U ontwikkelt niet een werkelijke-toepassing die wordt geïntegreerd met Azure Active Directory.


## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
Ondersteunde scenario's en vereisten voor Schijfopruiming versleuteling:

- De volgende Linux server SKU's - Ubuntu, CentOS SUSE en SUSE Linux Enterprise Server (SLES) en rood rol Enterprise Linux.
- Alle resources (zoals toets kluis, opslag-account en VM) moeten zich in de dezelfde Azure regiocode en het abonnement.
- Standaard A, D, DS, G en GS reeks VMs.

Schijfversleuteling wordt momenteel niet ondersteund in de volgende scenario's:

- Eenvoudige laag VMs.
- VMs gemaakt met behulp van het model Klassiek implementatie.
- OS schijf codering op Linux VMs uitschakelen.
- De cryptografische toetsen op een al versleutelde Linux VM bijwerken.


## <a name="create-the-azure-key-vault-and-keys"></a>Maken van de Azure-toets kluis en sleutels
Als u wilt de rest van deze handleiding hebt voltooid, moet u de [Meest recente Azure CLI](../xplat-cli-install.md) geïnstalleerd en aangemeld met de modus resourcemanager als volgt:

```
azure config mode arm
```

Vervang alle voorbeeldparameters met uw eigen namen, locatie en sleutelwaarden overal in de voorbeelden opdracht. De volgende voorbeelden gebruikt een overeenkomst van `myResourceGroup`, `myKeyVault`, `myAADApp`, enz.

De eerste stap is het opzetten van een kluis Azure-toets om op te slaan uw cryptografische sleutels. Azure-toets kluis kunt opslaan toetsen, geheimen of wachtwoorden waarmee u kunt ze veilig implementeren in uw toepassingen en -services. Voor virtuele schijf codering, kunt u toets kluis gebruiken voor het opslaan van een cryptografische sleutel die wordt gebruikt om te versleutelen of ontsleutelen uw virtuele schijven. 

De provider Azure-toets kluis in uw Azure abonnement inschakelen, en vervolgens een resourcegroep maken. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

De Azure-toets kluis met de cryptografische sleutels en de bijbehorende berekeningscluster bronnen zoals opslagcapaciteit en de VM zelf moet zich bevinden in dezelfde regio. Het volgende voorbeeld wordt een Azure-toets kluis met de naam `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

U kunt cryptografische sleutels met software of Hardware beveiliging Model (HSM) beveiliging opslaan. Een HSM gebruikt, is een premium-toets kluis vereist. Er is een extra kosten voor het maken van een premium-toets kluis in plaats van standaard toets kluis die worden opgeslagen toetsen software is beveiligd. Als u wilt maken van een premium-toets kluis, in de vorige stap toevoegen `--sku Premium` aan de opdracht. Het volgende voorbeeld wordt de toetsen software-beveiliging omdat we hebben gemaakt een standaard-toets kluis. 

Voor beide beveiligingsmodellen moet het Azure platform toegang aanvragen van de cryptografische sleutels wanneer de VM opgestart naar de virtuele schijven ontsleutelen is verleend. Maken van een versleutelingssleutel binnen uw sleutel kluis en instellen voor gebruik met virtuele schijfversleuteling. Het volgende voorbeeld wordt een sleutel met de naam `myKey` en dit vervolgens voor schijfversleuteling heeft ingeschakeld:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Maken van de Azure Active Directory-toepassing
Wanneer virtuele schijven worden versleuteld of ontsleuteld, kunt u een eindpunt gebruiken om de verificatie en uitwisseling van cryptografische sleutels uit kluis-toets te verwerken. Dit eindpunt, een Azure Active Directory-toepassing, kunt het Azure platform aanvragen van de juiste cryptografische sleutels namens de VM. Een standaardexemplaar Azure Active Directory is beschikbaar in uw abonnement, hoewel veel organisaties Azure Active Directory-mappen hebt toegewezen.

Als u niet een volledige Azure Active Directory-toepassing, maakt het `--home-page` en `--identifier-uris` parameters in het volgende voorbeeld hoeft niet te werkelijke geschikt adres. Het volgende voorbeeld bevat ook een wachtwoord gebaseerde geheim in plaats van genereren sleutels van binnen de Azure-portal. Als dit scenario genereren van sleutels is niet mogelijk uit de CLI Azure. 

Maak uw Azure Active Directory-toepassing. Het volgende voorbeeld wordt een toepassing met de naam `myAADApp` en gebruikt u een wachtwoord van `myPassword`. Uw eigen wachtwoord als volgt opgeven:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Noteer de `applicationId` die wordt geretourneerd in de uitvoer van de voorgaande opdracht. Deze toepassings-ID wordt gebruikt in enkele van de overige stappen. Maak vervolgens een servicenaam (principal name) zodat de toepassing toegankelijk zijn vanuit uw omgeving. Om deel te versleutelen of ontsleutelen virtuele schijven, moeten de machtigingen voor de cryptografische sleutel die zijn opgeslagen in de sleutel kluis zijn ingesteld op toestaan dat de Azure Active Directory-toepassing te lezen van de toetsen. 

Maak de name en stel de juiste machtigingen als volgt:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Voeg een virtuele schijf en versleuteling status controleren
Werkelijk versleutelen sommige virtuele schijven, kunt u een schijf toevoegen aan een bestaande VM. Als volgt een schijf van de gegevens 5Gb toevoegen aan een bestaande VM:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Het virtuele schijven zijn momenteel niet versleuteld. De huidige status van de versleuteling van uw VM als volgt controleren:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuele schijven versleutelen
Als u wilt versleutelen nu de virtuele schijven, brengen u samen de eerdere onderdelen:

1. Geef de Azure Active Directory-toepassing en het wachtwoord.
2. Geef op de toets kluis om op te slaan de metagegevens van de versleutelde schijven.
3. Geef de cryptografische sleutels moet worden gebruikt voor de werkelijke versleutelen en ontsleutelen.
4. Geef op of u de schijf OS, de schijf gegevens of alle versleutelen.

U kunt de details bekijken voor uw kluis Azure-toets en de sleutel die u hebt gemaakt, als u de sleutel kluis-ID, URI, nodig vervolgens belangrijke URL in de laatste stap:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Coderen van uw virtuele schijven met de uitvoer van de `azure keyvault show` en `azure keyvault key show` opdrachten als volgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Aangezien de voorgaande opdracht verschillende variabelen heeft, is het volgende voorbeeld de volledige opdracht voor verwijzing:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

De CLI Azure biedt uitgebreide fouten niet tijdens het versleuteling. Raadpleeg voor meer informatie over probleemoplossing, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` op de VM comprimeren.

Ten slotte kunt de status versleuteling nogmaals om te bevestigen dat uw virtuele schijven nu zijn gecodeerd controleren:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Extra gegevensschijven toevoegen
Zodra u hebt uw gegevensschijven gecodeerd, kunt u extra virtuele schijven later toevoegen aan uw VM en ze ook worden versleuteld. Bij het uitvoeren van de `azure vm enable-disk-encryption` opdracht, verhoogd de sequentie versie gebruikt de `--sequence-version` parameter. Deze parameter volgorde-versie kunt u herhaalde bewerkingen uitvoeren op de dezelfde VM.

U kunt bijvoorbeeld een tweede virtuele schijf als volgt toevoegen aan uw VM:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

De opdracht voor het coderen van de virtuele schijven, opnieuw uitvoeren in dit tijd toe te voegen de `--sequence-version` parameter, en te verhogen de waarde van onze eerste uitvoeren als volgt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van Azure-toets kluis, inclusief cryptografische sleutels en kluizen, verwijderen [beheren toets kluis CLI](../key-vault/key-vault-manage-with-cli.md).
- Zie voor meer informatie over Schijfopruiming versleuteling, zoals het voorbereiden van een versleutelde aangepaste VM uploaden naar Azure [Azure schijfversleuteling](../security/azure-security-disk-encryption.md).