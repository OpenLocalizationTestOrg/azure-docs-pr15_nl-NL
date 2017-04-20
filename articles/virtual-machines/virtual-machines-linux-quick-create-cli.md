<properties
   pageTitle="Een VM Linux op Azure maken met behulp van de CLI | Microsoft Azure"
   description="Maak een VM Linux op Azure met behulp van de CLI."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Een VM Linux op Azure maken met behulp van de CLI

In dit artikel leest u hoe u snel een Linux virtuele machine (VM) op Azure implementeren met behulp van de `azure vm quick-create` command in de opdrachtregel Azure (CLI). De `quick-create` opdracht implementeert een VM binnen een eenvoudige, beveiligde infrastructuur die u kunt om te prototype gebruiken of een concept snel testen. In het artikel moet:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).

- de [Azure CLI](../xplat-cli-install.md) aangemeld `azure login`.

- de modus Azure CLI _, moeten zich in_ Azure resourcemanager `azure config mode arm`.

U kunt ook snel een VM Linux implementeren met behulp van de [Azure-portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Snelle opdrachten

Het volgende voorbeeld ziet hoe u een VM CoreOS implementeren en uw Secure Shell (SSH)-sleutel (uw argumenten wijkt mogelijk af) wilt toevoegen:

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

De volgende scenario heeft een UbuntuLTS VM wordt geïmplementeerd, stap voor stap, met de beschrijving van wat elke stap doet.

## <a name="vm-quick-create-aliases"></a>VM snel-aliassen maken

Een snelle manier om te kiezen van een verdeling is het gebruik van de Azure CLI aliassen toegewezen aan de meest voorkomende OS onderzoeken. De volgende tabel bevat de aliassen (vanaf Azure CLI versie 0,10). Alle implementaties met `quick-create` standaard VMs die worden ondersteund door solid-state drive (SSD) opslag toegang tot de sneller inrichten en high-performance schijf biedt. (Deze aliassen geeft een zeer klein deel van de beschikbare onderzoeken op Azure. Meer afbeeldingen zoeken in de Azure Marketplace op [Zoeken naar een afbeelding in PowerShell](virtual-machines-linux-cli-ps-findimage.md), [op het web](https://azure.microsoft.com/marketplace/virtual-machines/)of [uw eigen aangepaste afbeelding uploaden](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Publisher | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiele      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | Rode rol    | RHEL         | 7.2         | meest recente  |
| UbuntuLTS | Canonieke | Ubuntu Server | 14.04.4-LTS | meest recente  |

De volgende secties gebruiken de `UbuntuLTS` alias voor de optie **ImageURN** (`-Q`) om te implementeren van een Ubuntu 14.04.4 LTS-Server.

De vorige `quick-create` voorbeeld alleen sectienummers de `-M` vlag om aan te geven van de openbare sleutel SSH uploaden tijdens het uitschakelen van SSH wachtwoorden, zodat u wordt gevraagd om de volgende argumenten:

- de naam van de groep resource (een willekeurige tekenreeks is meestal fijn voor uw eerste Azure resourcegroep)
- VM naam
- locatie (`westus` of `westeurope` zijn goede standaardwaarden)
- Linux (om te laten weten welke OS gewenste Azure)
- gebruikersnaam

Het volgende voorbeeld worden alle waarden, zodat er geen verdere waarin u wordt gevraagd vereist is. Zo lang er een `~/.ssh/id_rsa.pub` als een ssh-rsa openbare sleutel-bestandsindeling, dat werkt zoals is:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

De uitvoer ziet er als het volgende blok in de uitvoer:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Meld u aan bij de nieuwe VM

Meld u aan bij uw VM met behulp van het openbare IP-adres in de uitvoer. U kunt ook de volledig gekwalificeerde domeinnaam (FQDN) dat wordt vermeld:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Het aanmeldingsproces ziet er ongeveer als het volgende blok in de uitvoer:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Volgende stappen

De `azure vm quick-create` opdracht is de manier om snel een VM implementeren, zodat u kunt bij een shell we vaker doen aanmelden en starten (Engelstalig). Echter `vm quick-create` kunt u de uitgebreide besturingselement en het kunnen maken van een complexere-omgeving.  Als u wilt een Linux VM dat aangepast voor de infrastructuur van uw implementeert, kunt u een van deze artikelen volgen:

- [Een sjabloon Azure Resource Manager gebruiken om te maken van een specifieke implementatie](virtual-machines-linux-cli-deploy-templates.md)
- [Uw eigen aangepaste omgeving maken voor een Linux VM rechtstreeks Azure CLI opdrachten gebruiken](virtual-machines-linux-create-cli-complete.md)
- [Maak een SSH beveiligd Linux VM op Azure sjablonen gebruiken](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

U kunt ook [gebruiken de `docker-machine` Azure stuurprogramma met diverse opdrachten om snel te maken een VM Linux als host docker](virtual-machines-linux-docker-machine.md).
