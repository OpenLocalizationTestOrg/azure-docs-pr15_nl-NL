<properties
    pageTitle="Problemen met SSH verbinding oplossen voor een VM | Microsoft Azure"
    description="Klik hier voor meer informatie over het oplossen van problemen, zoals 'SSH verbinding is mislukt' of 'SSH verbinding geweigerd' voor een Azure VM waarop Linux."
    keywords="SSH verbinding geweigerd, ssh fout, azure ssh, SSH verbinding is mislukt"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Problemen met SSH verbindingen met een Azure Linux VM die mislukt, fouten, of is geweigerd
Er zijn verschillende redenen dat u fouten voor Secure Shell (SSH), SSH-verbinding mislukt, of SSH wordt geweigerd wanneer u probeert te verbinden met een Linux virtuele machine (VM). In dit artikel kunt u vinden en corrigeer de problemen. U kunt de Azure-portal Azure CLI of VM Access-extensie voor Linux verbindingsproblemen kunt oplossen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure experts op [het MSDN Azure en stapel overloop forums](http://azure.microsoft.com/support/forums/). U kunt ook een incident Azure ondersteuning indienen. Ga naar de [Azure ondersteunen site](http://azure.microsoft.com/support/options/) en selecteer **ondersteuning**. Lees de [Veelgestelde vragen voor ondersteuning van Microsoft Azure](http://azure.microsoft.com/support/faq/)voor informatie over het gebruik van Azure ondersteunen.


## <a name="quick-troubleshooting-steps"></a>Snelle stappen voor probleemoplossing
Probeer opnieuw verbinding maken met de VM na elke stap bij probleemoplossing.

1. De configuratie SSH opnieuw.
2. De referenties voor de gebruiker opnieuw instellen.
3. Controleer of dat de [Beveiligingsgroep van netwerk](../virtual-network/virtual-networks-nsg.md) -regels SSH verkeer toestaan.
    - Zorg ervoor dat er een beveiligingsgroep voor netwerk-regel bestaat zodanig dat SSH-verkeer (al dan niet standaard, TCP-poorten 22).
    - U kunt poortomleiding niet gebruiken / toewijzing zonder een Azure taakverdeling.
4. De [VM resource servicestatus](../resource-health/resource-health-overview.md)controleren. 
    - Zorg ervoor dat de VM rapporten die in orde.
    - Als er opstarten diagnostische hulpprogramma's die zijn ingeschakeld, controleert u of dat de VM is niet opstartfouten in de logboeken rapportage.
5. Start opnieuw op de VM.
6. Implementeer deze opnieuw de VM.

Doorgaan met lezen voor meer gedetailleerde stappen voor probleemoplossing en beschrijving.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Beschikbare methoden SSH-verbinding oplossen

U kunt referenties of SSH-configuratie met behulp van een van de volgende manieren herstellen:

- [Azure-portal](#using-the-azure-portal) - uitstekende als u wilt snel opnieuw de configuratie SSH of SSH-toets en u hoeft niet de Azure's hebben geïnstalleerd.
- [Azure CLI opdrachten](#using-the-azure-cli) - opnieuw als u al op de opdrachtregel snel de SSH configuratie of referenties.
- [Azure VMAccessForLinux extensie](#using-the-vmaccess-extension) - maken en opnieuw json definitiebestanden om de SSH configuratie of gebruikersreferenties opnieuw te gebruiken.

Probeer opnieuw verbinding te maken voor uw VM na elke stap bij probleemoplossing. Als u nog steeds geen verbinding, kunt u de volgende stap.


## <a name="using-the-azure-portal"></a>Met behulp van de Azure portal
De Azure Portal snel de SSH configuratie of gebruikersreferenties zonder eventuele hulpmiddelen voor de installatie op uw lokale computer opnieuw in te stellen.

Selecteer uw VM in de portal van Azure. Schuif omlaag naar de sectie **ondersteuning + probleemoplossing** en selecteer **wachtwoord opnieuw in** het volgende voorbeeld:

![Beginwaarden SSH configuratie of referenties in de portal van Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>De configuratie SSH opnieuw
Als eerste stap, selecteert u `Reset SSH configuration only` in het menu van de vervolgkeuzelijst **modus** zoals in de voorgaande schermafbeelding, klik vervolgens op de knop **opnieuw instellen** . Wanneer deze actie is voltooid, probeert u opnieuw uw VM toegang.

### <a name="reset-ssh-credentials-for-a-user"></a>SSH referenties voor een gebruiker opnieuw instellen
Als u wilt de referenties van een bestaande gebruiker opnieuw instellen, selecteert u een `Reset SSH public key` of `Reset password` in het menu van de vervolgkeuzelijst **modus** zoals in de voorgaande schermafbeelding. Geef de gebruikersnaam en een SSH-toets of het nieuwe wachtwoord en klik op de knop **opnieuw instellen** .

U kunt ook een gebruiker met bevoegdheden voor sudo op de VM in dit menu maken. Voer een nieuwe gebruikersnaam en het bijbehorende wachtwoord of het SSH-toets en klik vervolgens op de knop **opnieuw instellen** .


## <a name="using-the-azure-cli"></a>Gebruik van de Azure CLI
Als u nog niet gedaan, [de CLI Azure installeren en verbinden met uw Azure-abonnement hebt](../xplat-cli-install.md). Controleer of u met resourcemanager modus als volgt:

```
azure config mode arm
```

Als u hebt gemaakt en die zijn geüpload, wordt een aangepaste afbeelding in de schijf Linux, zorg ervoor dat de [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) versie 2.0.5 of hoger is geïnstalleerd. VMs gemaakt met behulp van de galerie met afbeeldingen, is dit toestel toegang al geïnstalleerd en geconfigureerd voor u.

### <a name="reset-ssh-configuration"></a>Opnieuw SSH configureren
De configuratie SSHD zelf onjuist zijn geconfigureerd of de service is een fout opgetreden. U kunt herstellen SSHD om ervoor te zorgen dat de configuratie SSH zelf geldig is. Opnieuw instellen van SSHD, moet de eerste probleemoplossing die u maakt.

Het volgende voorbeeld wordt SSHD op een VM met de naam `myVM` in de resourcegroep naam `myResourceGroup`. Uw eigen groepsnamen VM- en resourcekalenders als volgt gebruiken:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH referenties voor een gebruiker opnieuw instellen
Als SSHD correct lijkt, kunt u het wachtwoord voor de afzender van een gebruiker opnieuw instellen. Het volgende voorbeeld wordt de referenties voor `myUsername` aan de waarde die is opgegeven in `myPassword`, klik op de VM met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Als SSH belangrijke verificatie gebruikt, kunt u de sleutel SSH voor een bepaalde gebruiker opnieuw instellen. Het volgende voorbeeld bijgewerkt de SSH-sleutel die zijn opgeslagen in `~/.ssh/azure_id_rsa.pub` voor de gebruiker met de naam `myUsername`, klik op de VM met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Gebruik van de extensie VMAccess
De uitbreiding van de toegang tot VM voor Linux leest in een json-bestand dat wordt gedefinieerd acties uit te voeren. Deze acties opnemen opnieuw instellen van SSHD, opnieuw instellen van een sleutel SSH of toevoegen van een gebruiker. U nog steeds de CLI Azure gebruiken om te bellen van de extensie VMAccess, maar u kunt de json-bestanden opnieuw over meerdere VMs desgewenst. Deze methode kunt u een opslagplaats json-bestanden die u kunt vervolgens worden aangeroepen voor bepaalde scenario's maken.

### <a name="reset-sshd"></a>Beginwaarden SSHD
Maken van een bestand met de naam `PrivateConf.json` met de volgende inhoud:

```bash
{  
    "reset_ssh":"True"
}
```

De CLI Azure gebruikt, u vervolgens bellen de `VMAccessForLinux` extensie uw verbinding SSHD door het opgeven van het bestand json opnieuw in te stellen. Het volgende voorbeeld wordt SSHD op de VM met de naam `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH referenties voor een gebruiker opnieuw instellen
Als SSHD correct lijkt, kunt u de referenties voor de afzender van een gebruiker opnieuw instellen. Als u wilt het wachtwoord voor een gebruiker opnieuw instellen, maakt u een bestand met de naam `PrivateConf.json`. Het volgende voorbeeld wordt de referenties voor `myUsername` aan de waarde die is opgegeven in `myPassword`. Voer de volgende regels in uw `PrivateConf.json` bestand, met behulp van uw eigen waarden:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Of als u wilt de SSH-toets voor een gebruiker opnieuw hebt ingesteld, maakt u eerst een bestand met de naam `PrivateConf.json`. Het volgende voorbeeld wordt de referenties voor `myUsername` aan de waarde die is opgegeven in `myPassword`, klik op de VM met de naam `myVM` in `myResourceGroup`. Voer de volgende regels in uw `PrivateConf.json` bestand, met behulp van uw eigen waarden:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nadat u hebt uw json-bestand, met de CLI Azure bellen de `VMAccessForLinux` extensie uw gebruikersreferenties SSH door het opgeven van het bestand json opnieuw in te stellen. Het volgende voorbeeld wordt de referenties voor de benoemde VM `myVM` in `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Opnieuw een VM
Als u hebt de SSH configuratie en gebruiker referenties opnieuw instellen of fout tijdens het doet, kunt u proberen de VM adres onderliggende berekeningscluster problemen opnieuw te starten.

### <a name="azure-portal"></a>Azure-portal
Als wilt starten een VM met behulp van de Azure portal, selecteert u uw VM en klikt u op de ***Start opnieuw op** de knop zoals in het volgende voorbeeld:

![Start een VM opnieuw in de portal van Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Het volgende voorbeeld opnieuw is opgestart de VM met de naam `myVM` in de resourcegroep naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Implementeer deze opnieuw een VM
U kunt een VM naar een ander knooppunt in Azure, die mogelijk Verhelp eventuele onderliggende netwerken problemen implementeren. Voor informatie over het opnieuw te implementeren een VM, Zie [Implementeer deze opnieuw VM naar nieuwe Azure knooppunt](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Nadat deze bewerking is voltooid, kortstondige schijfgegevens verloren die zijn en dynamische IP-adressen die zijn gekoppeld aan de virtuele machine worden bijgewerkt.

### <a name="azure-portal"></a>Azure-portal
Als u wilt een VM met behulp van de Azure portal implementeren, selecteer uw VM en schuif omlaag naar de sectie **ondersteuning + probleemoplossing** . Klik op de knop **Implementeer deze opnieuw** zoals in het volgende voorbeeld:

![Implementeer deze opnieuw een VM in de portal van Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Het volgende voorbeeld de VM met de naam redeploys `myVM` in de resourcegroep naam `myResourceGroup`. Uw eigen waarden als volgt gebruiken:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs die zijn gemaakt met behulp van het model Klassiek-implementatie

Probeer de volgende stappen om op te lossen van de meest voorkomende SSH verbindingsfouten voor VMs die zijn gemaakt met behulp van het implementatiemodel klassieke. Probeer opnieuw verbinding maken met de VM na elke stap.

- Externe toegang via de [portal van Azure](https://portal.azure.com)opnieuw. In de portal Azure, selecteer uw VM en klik op de knop **Opnieuw externe...** .

- Start opnieuw op de VM. In de [portal van Azure](https://portal.azure.com), selecteer uw VM en klik op de knop **Start opnieuw** .

    -OF-

    Selecteer in de [klassieke Azure-portal](https://manage.windowsazure.com), **virtuele machines** > **exemplaren** > **opnieuw starten**.

- Implementeer deze opnieuw de VM naar een nieuw Azure knooppunt. Zie voor informatie over het implementeren van een VM [Implementeer deze opnieuw VM naar nieuwe Azure knooppunt](virtual-machines-windows-redeploy-to-new-node.md).

    Nadat deze bewerking is voltooid, kortstondige schijfgegevens verloren die zijn en dynamische IP-adressen die zijn gekoppeld aan de virtuele machine worden bijgewerkt.

- Volg de instructies in [het opnieuw instellen van een wachtwoord of SSH voor Linux gebaseerde virtuele machines](virtual-machines-linux-classic-reset-access.md) :
    - Opnieuw instellen van het wachtwoord of SSH-sleutel.
    - Maak een gebruikersaccount _sudo_ .
    - De configuratie SSH opnieuw.

- Van de VM resource controleren of er problemen zijn platform.<br>
  Selecteer uw VM en blader naar beneden **Instellingen** > **Servicestatus controleren**.


## <a name="additional-resources"></a>Aanvullende informatie

- Als u nog steeds niet kunt SSH voor uw VM na het uitvoeren van de na stappen, raadpleegt u [meer gedetailleerde stappen voor probleemoplossing](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) moet worden gereviseerd extra stappen om uw probleem te verhelpen.

- Zie voor meer informatie over het oplossen van de toepassing toegang, [problemen oplossen met de toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machines](virtual-machines-linux-troubleshoot-app-connection.md)

- Zie voor meer informatie over het oplossen van virtuele machines die zijn gemaakt met behulp van het implementatiemodel klassieke [opnieuw instellen van een wachtwoord of SSH voor Linux gebaseerde virtuele machines](virtual-machines-linux-classic-reset-access.md).
