<properties
    pageTitle="Andere manieren om te maken van een Linux VM | Microsoft Azure"
    description="Informatie over de verschillende manieren om te maken van een Linux virtuele machine op Azure, inclusief koppelingen naar hulpprogramma's en zelfstudies voor elke methode."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Andere manieren om te maken van een Linux virtuele machine in Azure wordt aangegeven

U hebt de flexibiliteit in een Linux virtuele machine maken (VM) met hulpprogramma's en werkstromen weet hoe u Azure wordt aangegeven. In dit artikel bevat een overzicht van deze verschillen en voorbeelden voor het maken van uw VMs Linux.


## <a name="azure-cli"></a>Azure CLI 

De CLI Azure is beschikbaar op platforms via een pakket met npm, geleverde distro-pakketten of Docker container. U kunt meer informatie over [het installeren en configureren van de Azure CLI](../xplat-cli-install.md). De volgende zelfstudies vindt u voorbeelden over het gebruik van de Azure CLI. Lees elk artikel voor meer informatie over de CLI snel aan de slag opdrachten weergegeven:

- [Een VM Linux maken op basis van de Azure CLI voor ontwikkelaar en testen](virtual-machines-linux-quick-create-cli.md)
    - Het volgende voorbeeld wordt een CoreOS VM met een openbare sleutel met de naam `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Maken van een beveiligde Linux VM met een sjabloon van Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Het volgende voorbeeld wordt een met een sjabloon die is opgeslagen op GitHub VM:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Een volledige Linux-omgeving met de CLI Azure maken](virtual-machines-linux-create-cli-complete.md)
    - Bevat een taakverdeling en meerdere VMs maken in een set beschikbaarheid.

- [Een schijf toevoegen aan een VM Linux](virtual-machines-linux-add-disk.md)
    - Het volgende voorbeeld wordt een schijf 5Gb voor een bestaande VM met de naam `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure-portal

De [portal van Azure](https://portal.azure.com) kunt u snel een VM maken omdat er niets te installeren op uw systeem. De Azure-portal gebruiken om het maken van de VM:

- [Een Linux VM met behulp van de Azure portal maken](virtual-machines-linux-quick-create-portal.md) 
- [Een schijf met behulp van de Azure portal hebt toegevoegd](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Het besturingssysteem en opties voor afbeelding
Als u een VM maakt, kiest u een afbeelding op basis van het besturingssysteem dat u wilt uitvoeren. Azure en haar partners bieden veel afbeeldingen, die zijn onder meer toepassingen en hulpprogramma's vooraf geïnstalleerd. Of een van uw eigen afbeeldingen (Zie [de volgende sectie](#use-your-own-image)) uploaden.

### <a name="azure-images"></a>Azure afbeeldingen
Gebruik de `azure vm image` CLI opdrachten om te zien wat door publisher, distro release en builds beschikbaar is.

Een lijst met beschikbare uitgevers als volgt:

```bash
azure vm image list-publishers --location WestUS
```

Een lijst met beschikbare producten (aanbiedingen) voor een bepaalde uitgever als volgt:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Een lijst met beschikbare SKU's (distro releases) van een bepaald aanbieding als volgt:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Lijst van alle beschikbare installatiekopieën voor een bepaald release volgt:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Zie voor meer voorbeelden op Bladeren en het gebruik van de beschikbare afbeeldingen [navigeren en selecteer Azure virtuele machines afbeeldingen met de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

De `azure vm quick-create` en `azure vm create` opdrachten aliassen die u kunt snel toegang tot de meest voorkomende distros en de meest recente versies hebben. Met behulp van aliassen is vaak sneller dan de uitgever, aanbieding, SKU en versie opgeven telkens wanneer die u een VM maken:

| Alias     | Publisher | Aanbieding        | SKU         | Versie |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | meest recente  |
| CoreOS    | CoreOS    | CoreOS       | Stabiele      | meest recente  |
| Debian    | credativ  | Debian       | 8           | meest recente  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | meest recente  |
| RHEL      | RedHat    | RHEL         | 7.2         | meest recente  |
| SLES      | SLES      | SLES         | 12-SP1      | meest recente  |
| UbuntuLTS | Canonieke | UbuntuServer | 14.04.4-LTS | meest recente  |

### <a name="use-your-own-image"></a>Uw eigen afbeelding gebruiken

Als u specifieke aanpassingen vereist, kunt u een afbeelding op basis van een bestaande Azure VM *vastleggen* die VM. U kunt ook een on-premises implementatie gemaakt afbeelding uploaden. Zie voor meer informatie over ondersteunde distros en hoe u uw eigen afbeeldingen gebruikt, de volgende artikelen:

- [Azure goedgekeurd onderzoeken](virtual-machines-linux-endorsed-distros.md)

- [Informatie voor niet ondersteunde onderzoeken](virtual-machines-linux-create-upload-generic.md)

- [Hoe u een Linux virtuele machine als een sjabloon resourcemanager vastleggen](virtual-machines-linux-capture-image.md).
    - Snel aan begin voorbeeld opdrachten om vast te leggen van een bestaande VM:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Volgende stappen

- Een VM Linux maken van de [portal](virtual-machines-linux-quick-create-portal.md), waarbij de [CLI](virtual-machines-linux-quick-create-cli.md)of een [Azure resourcemanager sjabloon](virtual-machines-linux-cli-deploy-templates.md)gebruiken.

- Na het maken van een Linux VM, [een gegevensschijf toevoegen](virtual-machines-linux-add-disk.md).

- Snelle stappen voor het [opnieuw instellen van een wachtwoord of SSH toetsen en gebruikers beheren](virtual-machines-linux-using-vmaccess-extension.md)
