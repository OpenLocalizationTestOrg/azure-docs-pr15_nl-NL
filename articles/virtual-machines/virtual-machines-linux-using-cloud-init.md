<properties
    pageTitle="Gebruik van cloud initialisatie voor het aanpassen van een Linux VM tijdens het maken van | Microsoft Azure"
    description="Gebruik cloud initialisatie voor het aanpassen van een Linux VM tijdens het maken van."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Gebruik van cloud initialisatie voor het aanpassen van een Linux VM tijdens het maken van

In dit artikel leest hoe u een script cloud initialisatie instellen de hostname, geïnstalleerd-updatepakketten en gebruikersaccounts beheren.  De cloud initialisatie scripts worden tijdens het maken van VM van Azure CLI genoemd.  In het artikel moet:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).

- de [Azure CLI](../xplat-cli-install.md) aangemeld `azure login`.

- de modus Azure CLI _, moeten zich in_ Azure resourcemanager `azure config mode arm`.

## <a name="quick-commands"></a>Snelle opdrachten

Een cloud-init.txt script maken waarmee Hiermee stelt u de hostnaam, werkt u alle pakketten en voegt u een gebruiker sudo toe aan Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Maak een resourcegroep om te starten VMs in.

```bash
azure group create myResourceGroup westus
```

Maak een Linux VM cloud initialisatie met configureert u deze tijdens het opstarten.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

### <a name="introduction"></a>Inleiding

Als u een nieuwe Linux VM start, kunt u een standaard Linux VM met niets wel aangepaste of gereed is voor uw behoeften. [Cloud initialisatie](https://cloudinit.readthedocs.org) is een standaard manier om een script of configuratie-instellingen in die Linux VM invoeren, zoals deze voor de eerste keer wordt opgestart.

Op Azure, er zijn een drie verschillende manieren om wijzigingen te brengen naar een Linux VM zoals deze wordt geïmplementeerd of opgestart.

- Scripts met cloud initialisatie invoeren.
- Gebruik van de Azure [VMAccess extensie](virtual-machines-linux-using-vmaccess-extension.md)scripts invoeren.
- Azure met een sjabloon cloud initialisatie.
- Azure met een sjabloon [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Om toe te voegen scripts op elk gewenst moment na het opstarten:

- SSH opdrachten rechtstreeks uitvoeren
- Invoeren van scripts met de Azure [VMAccess extensie](virtual-machines-linux-using-vmaccess-extension.md), imperatively of in een sjabloon van Azure
- Configuratie beheerprogramma's zoals Ansible, zout, Chef en Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Beschikbaarheid van de cloud initialisatie op Azure VM snel-afbeelding aliassen maken:

| Alias     | Publisher | Aanbieding        | SKU         | Versie | cloud initialisatie |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | meest recente  | geen         |
| CoreOS    | CoreOS    | CoreOS       | Stabiele      | meest recente  | Ja        |
| Debian    | credativ  | Debian       | 8           | meest recente  | geen         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  | geen         |
| RHEL      | RedHat    | RHEL         | 7.2         | meest recente  | geen         |
| UbuntuLTS | Canonieke | UbuntuServer | 14.04.4-LTS | meest recente  | Ja        |

Microsoft werkt samen met onze partners om cloud initialisatie opgenomen en het werken met de afbeeldingen die u naar Azure kunt.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Een script cloud initialisatie toevoegen aan de VM maken met de CLI Azure

Als u wilt een script cloud initialisatie starten bij het maken van een VM in Azure opgeven het cloud initialisatie-bestand met de CLI Azure `--custom-data` schakelen.

Maak een resourcegroep om te starten VMs in.

```bash
azure group create myResourceGroup westus
```

Maak een Linux VM cloud initialisatie met configureert u deze tijdens het opstarten.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Maken van een wolk initialisatie-script om in te stellen van de hostnaam van een Linux-VM

Een van de eenvoudigste en belangrijkste instellingen voor een VM Linux zou de hostnaam. We kunt eenvoudig deze instellen met de cloud initialisatie met dit script.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Voorbeeldscript voor cloud initialisatie met de naam `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Tijdens het opstarten van VM, dit script cloud initialisatie Hiermee stelt u de hostnaam op `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Aanmelden en controleer of de hostnaam van de nieuwe VM.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Maken van een script cloud initialisatie als u wilt bijwerken Linux

Voor beveiliging wilt u uw Ubuntu VM bijwerken op de eerste keer opstarten.  Via cloud initialisatie we dat kunt doen met het script volgen, afhankelijk van de Linux-verdeling die u gebruikt.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Voorbeeldscript cloud initialisatie `cloud_config_apt_upgrade.txt` voor het Debian gezin

```bash
#cloud-config
apt_upgrade: true
```

Nadat Linux heeft opgestart, alle geïnstalleerde pakketten worden bijgewerkt `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Aanmelden en controleer of alle pakketten worden bijgewerkt.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Maken van een script cloud initialisatie een gebruiker wil toevoegen aan Linux

Een van de eerste taken op een nieuwe Linux VM is een gebruiker toevoegen voor uzelf of om te vermijden `root`. SSH toetsen zijn geschikt aanbevolen procedures voor beveiliging en bruikbaarheid en ze zijn toegevoegd aan de `~/.ssh/authorized_keys` bestand met dit script cloud initialisatie.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Voorbeeldscript cloud initialisatie `cloud_config_add_users.txt` voor Debian gezin

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nadat Linux heeft opgestart, worden de weergegeven gebruikers gemaakt en toegevoegd aan de groep sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Aanmelden en controleer of de nieuwe gebruiker.

```bash
ssh myVM
cat /etc/group
```

Uitvoer

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Volgende stappen

Cloud initialisatie wordt één gebruikelijke manier voor het wijzigen van uw VM Linux bij het opstarten. Azure, heeft ook VM extensies, waarmee u kunt wijzigen van uw LinuxVM bij het opstarten of terwijl deze wordt uitgevoerd. U kunt bijvoorbeeld de VMAccessExtension Azure SSH of Gebruikersgegevens opnieuw instellen, terwijl de VM wordt uitgevoerd. Met de cloud initialisatie moet u opnieuw het wachtwoord opnieuw in te stellen.

[Over VM extensies en functies](virtual-machines-linux-extensions-features.md)

[Beheren van gebruikers, SSH en controleren of schijven op Azure Linux VMs met de extensie VMAccess herstellen](virtual-machines-linux-using-vmaccess-extension.md)
