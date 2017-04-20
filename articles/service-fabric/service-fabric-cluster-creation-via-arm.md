
<properties
   pageTitle="Maken van een beveiligde Service stof cluster met Azure Resource Manager | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe voor het instellen van een beveiligde Service stof cluster in Azure wordt aangegeven met Azure resourcemanager, Azure-toets kluis en Azure Active Directory (AAD) voor clientverificatie."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Maken van een Service stof cluster in Azure wordt aangegeven met Azure Resource Manager

> [AZURE.SELECTOR]
- [Azure resourcemanager](service-fabric-cluster-creation-via-arm.md)
- [Azure-portal](service-fabric-cluster-creation-via-portal.md)

Dit is een stapsgewijze handleiding waarin u bij de stappen begeleidt voor het instellen van een beveiligde Azure-Service stof cluster in Azure wordt aangegeven met Azure Resource Manager. Deze handleiding helpt u bij de volgende stappen uit:

 - Toets kluis instellen voor het beheren van sleutels voor beveiliging cluster en toepassing.
 - Maak een beveiligde cluster in Azure wordt aangegeven met Azure Resource Manager.
 - Gebruikers met Azure Active Directory (AAD) om uw cluster beheren verifiëren.

Een secure cluster is een cluster dat voorkomt onbevoegde toegang tot beheertaken uit te voeren, waaronder implementeert, upgraden en toepassingen, services en de bijbehorende gegevens verwijderen. Een onbeveiligde cluster is een cluster dat iedereen kan verbinding met op elk gewenst moment maken en management bewerkingen uitvoeren. Is het mogelijk te maken van een onbeveiligde cluster, maar het is **raadzaam een beveiligde cluster maken**. Een onbeveiligde cluster **later kan niet worden beveiligd** - een nieuw cluster moet worden gemaakt.

De concepten zijn hetzelfde voor het maken van secure kolomgroepen, ongeacht of de clusters Linux clusters of Windows-clusters zijn. Zie voor meer informatie en helper scripts voor het maken van secure Linux clusters, [beveiligde clusters maken op Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure
Deze handleiding gebruikmaakt van [Azure PowerShell][azure-powershell]. Wanneer een nieuwe PowerShell-sessie starten, meld u aan bij uw Azure-account en selecteert u uw abonnement voordat het uitvoeren van Azure opdrachten.

Meld u aan bij uw azure-account:

```powershell
Login-AzureRmAccount
```

Selecteer uw abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Toets kluis instellen

In dit gedeelte begeleidt tijdens het maken van een sleutel kluis voor stof Service-toepassingen en voor een Service stof cluster in Azure wordt aangegeven. Raadpleeg de [handleiding kluis sleutel aan de slag]voor een volledige hulplijn op de toets kluis,[key-vault-get-started].

Service stof gebruikt x.509-certificaten naar een cluster secure en geef de beveiligingsfuncties van toepassing. Azure-toets kluis wordt gebruikt voor het beheren van certificaten voor Service stof clusters in Azure wordt aangegeven. Wanneer een cluster is geïmplementeerd in Azure wordt aangegeven, wordt de provider Azure resource die verantwoordelijk is voor het maken van Service stof clusters worden certificaten opgehaald uit kluis-toets en installeert deze op het cluster VMs.

In het volgende diagram ziet u de relatie tussen toets kluis, een cluster Service stof en de provider van Azure resource gebruikmaakt van certificaten die zijn opgeslagen in de sleutel kluis bij het maken van een cluster:

![Certificaat installatie][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Een resourcegroep maken

De eerste stap is het opzetten van een resourcegroep specifiek voor sleutel kluis. Toets kluis plaatsen in een eigen resourcegroep wordt aanbevolen. Hiermee kunt u verwijderen van de resourcegroepen berekeningscluster en opslag, inclusief de resourcegroep waarop uw cluster Service stof zonder uw toetsen en geheimen kwijt te raken. De resourcegroep waarop uw sleutel kluis moet zich in hetzelfde gebied, als het cluster die wordt gebruikt.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Belangrijke kluis maken 

Maak een kluis sleutel in de resourcegroep nieuw. De toets kluis **moet zijn ingeschakeld voor implementatie** toe te staan dat de provider van de resource Service stof certificaten ophalen van deze en installeer op knooppunten:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Als u een bestaande sleutel kluis hebt, kunt u deze inschakelen voor implementatie Azure CLI gebruiken:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Certificaten toevoegen aan de sleutel kluis

Certificaten worden voor verificatie en versleuteling voor het beveiligen van verschillende aspecten van een cluster en bijbehorende toepassingen in Service stof gebruikt. Zie voor meer informatie over hoe certificaten worden gebruikt in de Service stof, [scenario's de beveiliging van de Service stof cluster][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster en server certificaat (vereist) 

Dit certificaat is vereist voor het beveiligen van een cluster en onbevoegde toegang tot deze voorkomen. Biedt cluster beveiliging in een paar manieren:
 
 - **Cluster verificatie:** Verifieert naar knooppunten communicatie voor cluster Federatie. Alleen knooppunten die hun identiteit aan dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
 - **Serververificatie:** Verifieert de eindpunten van de cluster management aan een management-client, zodat de client management weet dat deze aan de reële cluster aan het woord is. Dit certificaat ook biedt SSL voor de API voor het beheer van de HTTPS en Service stof Explorer via HTTPS.

Als u wilt deze doeleinden, het certificaat moet voldoen aan de volgende vereisten:

 - Het certificaat moet een persoonlijke sleutel bevatten.
 - Het certificaat moet worden gemaakt voor belangrijke exchange, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
 - Naam van de onderwerp van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service stof cluster. Deze matchng is verplicht voor SSL bieden voor de eindpunten van HTTPS management en Service stof Explorer van het cluster. U kunt geen een SSL-certificaat bij een certificeringsinstantie (CA) aanvragen voor het `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, de naam van de onderwerp van het certificaat moet overeenkomen met de aangepaste domeinnaam die wordt gebruikt voor uw cluster worden weergegeven.

### <a name="application-certificates-optional"></a>Certificaten van toepassing (optioneel)

Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om veiligheidsredenen van toepassing. Voordat u uw cluster maakt, kunt u overwegen de toepassing beveiliging-scenario's waarvoor een certificaat zijn geïnstalleerd op de knooppunten, zoals:

 - Coderen en decoderen van toepassing configuratiewaarden
 - Codering van gegevens op knooppunten tijdens herhaling 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Opmaak van certificaten voor gebruik van de provider Azure bronnen

Privé key-bestanden (.pfx) toe te voegen en rechtstreeks via de sleutel kluis gebruikt. De provider Azure resource vereist echter toetsen worden opgeslagen in een speciale JSON-indeling waarin de .pfx als een basis-64 codering tekenreeks en het wachtwoord voor persoonlijke sleutel. Als u wilt deze vereisten is voldaan, worden sleutels in een tekenreeks JSON geplaatst en vervolgens opgeslagen als een *geheimen* in sleutel kluis.

Om dit proces vereenvoudigen, een PowerShell-module is [beschikbaar op GitHub][service-fabric-rp-helpers]. Volg deze stappen om het gebruik van de module:

  1. Download de volledige inhoud van de cessies‑retrocessies in een lokale map. 
  2. Importeer de module in het venster PowerShell:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
De `Invoke-AddCertToKeyVault` opdracht in deze PowerShell-module automatisch de persoonlijke sleutel van een certificaat in een tekenreeks JSON-indelingen en uploadt dit naar de toets kluis. Gebruik deze het certificaat cluster en eventuele aanvullende toepassing certificaten toevoegen aan de sleutel kluis. Herhaal deze stap voor elke extra certificaten die u wilt installeren in uw cluster.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

De voorgaande tekenreeksen zijn de sleutel kluis vereisten voor het configureren van een sjabloon Service stof cluster resourcemanager die wordt geïnstalleerd samen certificaten voor verificatie, management eindpunt beveiliging en verificatie en eventuele aanvullende toepassing beveiligingsfuncties die x.509-certificaten gebruiken. Nu hebt u nu de volgende instellingen in Azure wordt aangegeven:

 - Toets kluis resourcegroep
   - Belangrijke kluis
     - Certificaat voor serververificatie cluster
     - Certificaten van toepassing

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor de clientverificatie

AAD kan organisaties die (tenants genoemd) voor het beheren van de gebruikerstoegang tot de toepassingen die zijn onderverdeeld in toepassingen met een aanmelding web gebaseerde UI en toepassingen met een systeemeigen Clientervaring. In dit document, we is ervan uitgegaan dat u al een tenant hebt gemaakt. Zo niet, begint u met het lezen [hoe u een Azure Active Directory-tenant][active-directory-howto-tenant].

Een cluster stof Service biedt diverse vermelding verwijst naar de management-functionaliteit, waaronder het web gebaseerde [Service stof Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio][service-fabric-manage-application-in-visual-studio]. Hierdoor maakt u twee AAD-toepassingen toegang tot het cluster te beheren, één webtoepassing en één van de bijbehorende toepassing.

Om te vereenvoudigen enkele van de stappen voor het configureren van AAD met een Service stof cluster, hebt wordt een set van Windows PowerShell-scripts gemaakt.

>[AZURE.NOTE] U moet uitvoeren deze stappen *voordat* u maakt de cluster zo in gevallen waarin de scripts clusternamen en eindpunten verwacht, moet deze de geplande waarden, niet met randen die u al hebt gemaakt.

1. [Downloaden van de scripts] [ sf-aad-ps-script-download] naar uw computer.

2. Met de rechtermuisknop op het zip-bestand, kies **Eigenschappen**, en schakel het selectievakje **deblokkeren** en toepassen.

3. Pak het zipbestand.

4. Uitvoeren `SetupApplications.ps1`, biedt de TenantId, clusternaam en WebApplicationReplyUrl parameters. Bijvoorbeeld:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    U kunt uw **TenantId** vinden door het uitvoeren van de PowerShell-opdracht '' Get-AzureSubscription'' '. Hierdoor wordt de **TenantId** voor elk abonnement weergegeven.

    De **clusternaam** wordt gebruikt om de aanduiding voor de AAD-toepassingen die door het script is gemaakt. Deze hoeft niet overeenkomen met de naam van de werkelijke cluster exact in zoals deze alleen is bedoeld om het gemakkelijker maken om berichten te AAD onderdelen toewijzen aan de Service stof cluster dat ze worden gebruikt met.

    De **WebApplicationReplyUrl** is de standaardeindpunt dat AAD keert terug naar uw gebruikers na het voltooien van het proces aanmelden. U moet dit naar het eindpunt van de Service stof Explorer instellen voor uw cluster, die standaard is:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    U zich aanmeldt bij een account met voor de tenant AAD beheerdersmachtigingen wordt u gevraagd. Zodra u doet, is het script gaat door naar het web en systeemeigen toepassingen voor uw cluster Service stof maken. Als u naar toepassingen van de tenant in de [portal van Azure klassieke kijkt][azure-classic-portal], moet u twee nieuwe items ziet:

    - *Clusternaam*\_Cluster
    - *Clusternaam*\_Client

    De script afdrukken dat de Json vereist door de sjabloon Azure resourcemanager wanneer u het cluster in het volgende gedeelte maakt houden zodat de PowerShell venster openen.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Een Service stof cluster resourcemanager-sjabloon maken

In dit gedeelte worden de uitvoer van de voorgaande PowerShell-opdrachten in een Service stof cluster resourcemanager sjabloon gebruikt.

Resourcemanager steekproef-sjablonen zijn beschikbaar in de [sjabloongalerie via Azure werkbalk Snel starten op GitHub][azure-quickstart-templates]. Deze sjablonen kunnen worden gebruikt als uitgangspunt voor uw cluster-sjabloon. 

### <a name="create-the-resource-manager-template"></a>De resourcemanager-sjabloon maken

Deze handleiding gebruikt de [secure cluster 5 knooppunten] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] voorbeeldsjabloon en sjabloonparameters. Download `azuredeploy.json` en `azuredeploy.parameters.json` naar uw computer en opent u beide bestanden in uw favoriete teksteditor.

### <a name="add-certificates"></a>Certificaten toevoegen

Certificaten worden toegevoegd aan een cluster resourcemanager-sjabloon door te verwijzen naar de kluis toets met het certificaat toetsen. Het wordt aanbevolen dat deze toets kluis-waarden in een resourcemanager parameters sjabloonbestand om de resourcemanager sjabloon bestand herbruikbare en vrij te geven met waarden die specifiek zijn voor een implementatie zijn geplaatst.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Alle certificaten toevoegen aan de osProfile VMSS

Elk certificaat dat moet worden geïnstalleerd in het cluster moet worden geconfigureerd in de sectie osProfile van de resource VMSS (Microsoft.Compute/virtualMachineScaleSets). Dit Hiermee geeft u de resource-provider het certificaat op de VMs te installeren. Dit geldt ook voor het certificaat cluster, evenals een toepassing beveiligingscertificaten die u wilt gebruiken voor uw toepassingen:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Service stof cluster-certificaat configureren

De cluster-certificaat voor serververificatie moet ook worden geconfigureerd in de Service stof cluster resource (Microsoft.ServiceFabric/clusters) en in de Service stof-extensie voor VMSS in de bron VMSS. Hiermee kan de Service stof resource provider configureren voor gebruik voor cluster verificatie en serververificatie voor management eindpunten.

##### <a name="vmss-resource"></a>VMSS resource:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service stof resource:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>AAD config invoegen

De AAD-configuratie eerder hebt gemaakt, kan worden ingevoegd rechtstreeks aan op uw sjabloon resourcemanager, maar het wordt aanbevolen de waarden in parameters eerst naar een parameterbestand u de sjabloon resourcemanager herbruikbare en gratis van waarden specifieke aan een distributie uitpakken.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < een ' configureren-arm"></a>parameters van de sjabloon resourcemanager configureren

Ten slotte de uitvoerwaarden uit de sleutel kluis en AAD PowerShell-opdrachten gebruiken om te vullen de parameterbestand:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Nu hebt u nu het volgende:

 - Toets kluis resourcegroep
    - Belangrijke kluis
    - Certificaat voor serververificatie cluster
    - Gegevens uitwisselen certificaat
 - Azure Active Directory-tenant 
    - AAD-toepassing voor de web-beheer en Service stof Explorer
    - AAD-toepassing voor systeemeigen clientbeheer
    - Gebruikers met rollen zijn toegewezen 
 - Service stof cluster resourcemanager sjabloon
    - Certificaten geconfigureerd met behulp van de sleutel kluis
    - Azure Active Directory geconfigureerd 

In het volgende diagram ziet u waar toets kluis en AAD configuratie in uw sjabloon resourcemanager past.

![Resourcemanager afhankelijkheid kaart][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Het cluster maken

U bent nu klaar om te maken van het cluster [ARM implementatie]via[resource-group-template-deploy].

#### <a name="test-it"></a>Testen

Gebruik de volgende PowerShell-opdracht om te testen van uw resourcemanager-sjabloon met een parameterbestand:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Het dashboard implementeren

Als de test van de sjabloon resourcemanager worden doorgegeven, gebruikt u de volgende PowerShell-opdracht om te implementeren van uw resourcemanager-sjabloon met een parameterbestand:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen

Nadat u de toepassingen voor uw cluster hebt gemaakt, moet u uw gebruikers toewijzen aan de rollen die worden ondersteund door de Service stof: alleen-lezen en beheerder. U kunt dit doen met behulp van de [Azure klassieke portal][azure-classic-portal].

1. Ga naar uw tenant en kies toepassingen.
2. Kies de webtoepassing, waarvoor een naam zoals `myTestCluster_Cluster`.
3. Klik op het tabblad gebruikers.
4. Kies een gebruiker toewijzen en klik op de knop **toewijzen** aan de onderkant van het scherm.

    ![Gebruikers toewijzen aan rollen-knop][assign-users-to-roles-button]

5. Selecteer de rol toewijzen aan de gebruiker.

    ![Gebruikers toewijzen aan rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Zie voor meer informatie over rollen in Service stof [Rolgebaseerd toegangsbeheer voor Service stof clients](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Secure clusters op Linux maken

Om het proces vereenvoudigen, een script helper is voldaan [hier](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Voor het gebruik van dit Help-script, wordt uitgegaan dat u al Azure CLI geïnstalleerd hebt en in het pad is. Zorg ervoor dat het script machtigingen uitvoeren door te voeren heeft `chmod +x cert_helper.py` na het downloaden. De eerste stap is aan te melden bij uw Azure-account met de CLI met de `azure login` opdracht. Na het aanmelden bij uw Azure-account, gebruikt u de helper met uw ondertekend certificeringsinstantie, zoals de volgende opdracht wordt getoond:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Deze opdracht geeft als resultaat de volgende drie tekenreeksen de uitvoer: 

1. Een SourceVaultID, dat wil zeggen de ID voor de nieuwe KeyVault ResourceGroup het voor u gemaakt. 

2. Een CertificateUrl voor toegang tot het certificaat.

3. Een CertificateThumbprint, die wordt gebruikt voor verificatie.


Het volgende voorbeeld ziet u hoe u de opdracht gebruikt:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Uitvoeren van de voorgaande opdracht wordt met de drie tekenreeksen als volgt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Naam van de onderwerp van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service stof cluster. Dit is vereist om aan te bieden van SSL voor de eindpunten van HTTPS management en Service stof Explorer van het cluster. U kunt geen een SSL-certificaat bij een certificeringsinstantie (CA) aanvragen voor het `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt de onderwerpnaam van het certificaat moet overeenkomen met de aangepaste domeinnaam die wordt gebruikt voor uw cluster worden weergegeven.

Hierna ziet u de items die u nodig hebt voor het maken van een beveiligde service stof cluster (zonder AAD), zoals beschreven op [parameters van de sjabloon resourcemanager configureren](#configure-arm). U kunt verbinding maken met het beveiligde cluster via de instructies op de [clienttoegang tot een cluster verifiëren](service-fabric-connect-to-secure-cluster.md). Linux preview clusters ondersteunen AAD-verificatie niet. U kunt rollen beheerder en -client zoals is beschreven in de sectie [rollen aan gebruikers toewijzen](#assign-roles). Wanneer u beheerder en client rollen voor een Linux preview cluster opgeeft, moet u certificaat-vingerafdrukken bieden voor verificatie (in plaats van onderwerpnaam, aangezien er geen ketting validatie of intrekking wordt uitgevoerd in deze release preview).


Als u een zelfondertekend certificaat gebruiken wilt voor het testen, kunt u hetzelfde script gebruiken om te genereren van een zelfondertekend certificaat en uploadt dit naar KeyVault, doordat de vlag `ss` in plaats van het certificaatpad en de naam van het certificaat leveren. Zie bijvoorbeeld de volgende opdracht voor het maken en uploaden van een zelfondertekend certificaat:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Deze opdracht geeft als resultaat de dezelfde drie tekenreeksen, SourceVault, CertificateUrl en CertificateThumbprint, die wordt gebruikt voor het maken van een beveiligde Linux cluster, samen met de locatie waar het zelfondertekend certificaat is geplaatst. Moet u het zelfondertekend certificaat verbinding maken met de cluster.  U kunt verbinding maken met het beveiligde cluster via de instructies op de [clienttoegang tot een cluster verifiëren](service-fabric-connect-to-secure-cluster.md). Naam van de onderwerp van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service stof cluster. Dit is vereist om aan te bieden van SSL voor de eindpunten van HTTPS management en Service stof Explorer van het cluster. U kunt geen een SSL-certificaat bij een certificeringsinstantie (CA) aanvragen voor het `.cloudapp.azure.com` domein. U kunt een aangepaste domeinnaam voor uw cluster moet aanschaffen. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt de onderwerpnaam van het certificaat moet overeenkomen met de aangepaste domeinnaam die wordt gebruikt voor uw cluster worden weergegeven.

De parameters die door het script helper kunnen worden ingevuld in de portal, zoals beschreven in de sectie [een cluster in de portal van Azure maken](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Volgende stappen

U hebt nu een beveiligde cluster met Azure Active Directory die management verificatie. Volgende, [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en informatie over het [beheren van toepassing geheimen](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Problemen met het instellen van Azure Active Directory voor clientverificatie

Als u optreden tijdens het instellen van Azure Active Directory voor verificatie problemen, raadpleegt u de volgende suggestings voor mogelijke oplossingen.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Service stof Explorer aanwijzingen voor het selecteren van certificaat

#### <a name="problem"></a>Probleem

Hierna aanmelding op de aanmeldingspagina AAD in Service stof Explorer, is de browser keert terug naar de startpagina, maar wordt gevraagd een dialoogvenster voor het selecteren van een certificaat.

![Dialoogvenster voor SFX select certificaat][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reden

De gebruiker is niet een bepaalde rol in AAD cluster-toepassing. Dus mislukt AAD-verificatie op stof Service cluster. Service stof Explorer valt terug op verificatie via clientcertificaat.

#### <a name="solution"></a>Oplossing

Volg de instructies voor het instellen van AAD en rollen toewijzen. Bovendien "Gebruiker toewijzing vereist aan ACCESS-APP" wordt aanbevolen om te worden ingeschakeld als `SetupApplications.ps1` bevat.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Verbinding maken met PowerShell mislukt met fout: de opgegeven referenties zijn ongeldig

#### <a name="problem"></a>Probleem

Wanneer PowerShell gebruiken om u te verbinden met cluster met behulp van "AzureActiveDirectory" beveiligingsmodus, na de aanmelding op de aanmeldingspagina AAD,-verbinding mislukt met fout: de opgegeven referenties zijn ongeldig weergegeven.

#### <a name="solution"></a>Oplossing

Hetzelfde als hierboven.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Service stof Explorer ondertekenen in mislukt: AADSTS50011

#### <a name="problem"></a>Probleem

Na de aanmelding op AAD aanmeldingspagina in de Service stof Explorer, retourneert pagina sign in mislukt - AADSTS50011: het antwoordadres &lt;url&gt; niet overeenkomen met de antwoordadressen die is geconfigureerd voor de toepassing: &lt;guid&gt;. 

![Antwoordadres SFX die niet overeenkomen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reden

De toepassing cluster(web) dat staat voor Service stof Explorer pogingen om te verifiëren tegen AAD als onderdeel van de aanvraag krijgen de afzender Omleidings-URL. Maar deze niet wordt vermeld in de lijst AAD toepassing 'Antwoord URL'.

#### <a name="solution"></a>Oplossing

Url van de Service stof Explorer toevoegen aan de 'Antwoord URL' op het tabblad configureren van cluster(web) toepassing of een van de items in de lijst vervangen. Sla.

![Antwoord van de webtoepassing][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Kan ik de dezelfde AAD-tenant voor meerdere clusters opnieuw gebruiken?

#### <a name="answer"></a>Answer

Ja. Maar denk eraan dat de URL van Service stof Explorer toevoegt aan uw cluster(web)-toepassing die anders Service stof Explorer werkt niet.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Waarom moet ik nog steeds servercertificaat terwijl AAD ingeschakeld?

#### <a name="answer"></a>Answer

FabricClient en FabricGateway uitvoeren onderlinge verificatie. Voor het geval AAD-authenticatie biedt AAD-integratie de identiteit van de client op server en servercertificaat wordt gebruikt om te controleren of de serveridentiteit. Voor meer informatie over de werking van certificaat op stof Service, raadpleegt u [x.509-certificaten en Service stof][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png