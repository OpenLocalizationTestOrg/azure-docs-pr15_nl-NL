<properties
    pageTitle="Maken en uploaden van de afbeelding van een aangepaste Linux | Microsoft Azure"
    description="Maak en een virtuele harde schijf (VHD) uploaden naar Azure met een aangepaste Linux-afbeelding met het implementatiemodel resourcemanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Uploaden en een VM Linux maken op basis van de afbeelding van de aangepaste Schijfopruiming

In dit artikel leest u hoe u een virtuele harde schijf (VHD) uploaden naar Azure met het implementatiemodel resourcemanager en Linux VMs maken op basis van deze aangepaste afbeelding. Deze functie kunt u installeren en configureren van een distro Linux aan uw behoeften en gebruik vervolgens die VHD snel maken Azure virtuele machines (VMs).

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende informatie in de sectie snel het grondtal de opdrachten voor het uploaden van een VM naar Azure. De rest van het document, [beginnend hier](#requirements)kunnen vindt u meer gedetailleerde informatie en context voor elke stap.

Zorg ervoor dat u hebt [De CLI Azure](../xplat-cli-install.md) aangemeld en het gebruik van resourcemanager modus:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.

Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUs` locatie:

```bash
azure group create myResourceGroup --location "WestUS"
```

Maak een account opslagruimte voor uw virtuele schijven. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Lijst met de toegangstoetsen voor uw account opslag. Noteer `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Maak een container binnen uw opslagruimte rekening met de opslag-toets die u hebt aangeschaft. Het volgende voorbeeld wordt een container met de naam `myimages` met de sleutelwaarde voor de opslag van `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Upload uw VHD ten slotte aan de container die u hebt gemaakt. Geef het lokale pad naar uw VHD onder `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

U kunt nu een VM maken vanuit uw geüploade virtuele schijf [met een sjabloon resourcemanager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). U kunt ook de CLI gebruiken door het opgeven van de URI op de schijf (`--image-urn`). Het volgende voorbeeld wordt een VM met de naam `myVM` gebruik van de virtuele schijf eerder hebt geüpload:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

De bestemming opslag-account heeft hetzelfde resultaat als waar u de virtuele schijf naar hebt geüpload. U moet ook opgeven of answer vraagt om, de aanvullende parameters vereist door de `azure vm create` command zoals virtueel netwerk, openbare IP-adres, gebruikersnaam en SSH toetsen. U vindt meer informatie over de [beschikbare CLI resourcemanager parameters](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vereisten
Als u wilt de volgende stappen uitvoeren, hebt u het volgende nodig:

- **Linux-besturingssysteem is geïnstalleerd in een bestand VHD** - installeren van een [verdeling van Azure ondersteunde Linux](virtual-machines-linux-endorsed-distros.md) (of Zie [informatie](virtual-machines-linux-create-upload-generic.md)voor onderzoeken niet ondersteunde) tot een virtuele schijf in de VHD-indeling. Er zijn meerdere hulpprogramma's waarmee u het maken van een VM en VHD:
    - Installeren en configureren van [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), daarbij VHD gebruiken als uw afbeelding opmaken. Indien nodig kunt u [een afbeelding converteren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) gebruiken `qemu-img convert`.
    - U kunt ook Hyper-V [in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx)gebruiken.

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. Wanneer u een VM hebt gemaakt, geeft u VHD als de notatie. Indien nodig kunt u VHDX schijven converteren naar het gebruik van VHD [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure ondersteunt verder, geen uploaden dynamische VHD's, zodat u deze schijven converteren naar statische VHD's moet voordat u uploadt. Hulpprogramma's zoals [Azure VHD hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) kunt u dynamische schijven tijdens het proces van het uploaden naar Azure converteren.

- VMs gemaakt op basis van uw aangepaste afbeelding moeten zich bevinden in hetzelfde opslag-account als de afbeelding zelf
    - Een opslag-account en de container voor het opslaan van uw aangepaste afbeelding en de gemaakte VMs maken
    - Nadat u alle uw VMs hebt gemaakt, kunt u veilig uw afbeelding verwijderen

Zorg ervoor dat u hebt [De CLI Azure](../xplat-cli-install.md) aangemeld en het gebruik van resourcemanager modus:

```bash
azure config mode arm
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>De afbeelding om te worden geüpload voorbereiden

Azure ondersteunt verschillende Linux onderzoeken (Zie [Onderzoeken goedgekeurd](virtual-machines-linux-endorsed-distros.md)). De volgende artikelen helpt u bij het voorbereiden van de verschillende Linux onderzoeken die worden ondersteund op Azure:

- **[CentOS gebaseerde onderzoeken](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Rode rol Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - onderzoeken niet ondersteunde](virtual-machines-linux-create-upload-generic.md)**

Zie ook de **[Installatieopmerkingen Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** voor meer algemene tips over het voorbereiden van Linux afbeeldingen van Azure.

> [AZURE.NOTE] Het [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) geldt voor VMs waarop Linux alleen wanneer u een van de aangebracht onderzoeken wordt gebruikt met configuratiegegevens van de opgegeven onder versies worden ondersteund in [Linux op Azure-Endorsed onderzoeken](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Een resourcegroep maken
Resourcegroepen samenbrengen logisch alle Azure bronnen ter ondersteuning van uw virtuele machines, zoals de virtuele netwerken en opslag. Lees meer over [Azure resourcegroepen hier](../azure-resource-manager/resource-group-overview.md). Voordat u uw aangepaste schijf afbeelding uploaden en VMs maken, moet u eerst een resourcegroep maken. 

Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Een opslag-account maken
VMs worden opgeslagen als BLOB van pagina's binnen een opslag-account. Lees meer over [Azure-blobopslag hier](../storage/storage-introduction.md#blob-storage). U maken een account opslagruimte voor uw aangepaste schijf afbeelding en VMs. Een VMs die u van uw aangepaste schijf afbeelding maakt moeten worden in hetzelfde opslag-account als die afbeelding.

Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount` in de resourcegroep eerder hebt gemaakt:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Lijst opslag account toetsen
Azure genereert twee 512 bits toegangstoetsen voor elk account opslag. Deze toegangstoetsen worden gebruikt bij het verifiëren van aan de opslag-account, zoals voor het schrijven verrichten. Lees meer over het [beheren van toegang tot opslag hier](../storage/storage-create-storage-account.md#manage-your-storage-account). U kunt weergeven toegangstoetsen met de `azure storage account keys list` opdracht.

Bekijk de toegangstoetsen voor de opslag-account dat u hebt gemaakt:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

De uitvoer is vergelijkbaar met:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Noteer `key1` terwijl u deze wordt gebruikt om te communiceren met uw account opslag in de volgende stappen.

## <a name="create-a-storage-container"></a>Een container opslag maken
In dezelfde manier als u andere mappen wilt logisch indelen uw lokale bestandssysteem maken, maakt u containers in een opslag-account om uw virtuele schijven en de afbeeldingen te organiseren. Een account opslag kan een willekeurig aantal containers bevatten. 

Het volgende voorbeeld wordt een container met de naam `myimages`, geven de toegangstoets verkregen in de vorige stap (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>VHD uploaden
Nu kunt u de afbeelding van uw aangepaste schijf daadwerkelijk uploaden. Als met alle virtuele schijven door VMs, gebruikt u uploadt en de afbeelding van uw aangepaste schijf opslaan als een pagina-blob.

Geef uw access-toets, de container die u hebt gemaakt in de vorige stap en klik op het pad naar de afbeelding aangepaste schijf op uw lokale computer:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>VM maken van aangepaste afbeelding
Wanneer u VMs van de afbeelding van uw aangepaste schijf maken, geeft u de URI naar de afbeelding schijf. Zorg ervoor dat de bestemming opslag account overeenkomsten waarin de afbeelding van uw aangepaste schijf is opgeslagen. U kunt uw VM met behulp van de Azure CLI of resourcemanager JSON-sjabloon maken.


### <a name="create-a-vm-using-the-azure-cli"></a>Een VM gebruik van de Azure CLI maken
U hebt opgegeven de `--image-urn` parameter met de `azure vm create` opdracht te laten verwijzen naar de afbeelding van uw aangepaste Schijfopruiming. Zorg ervoor dat `--storage-account-name` overeenkomt met het opslag-account waarin de afbeelding van uw aangepaste schijf is opgeslagen. U hoeft niet te dezelfde container als de afbeelding aangepaste Schijfopruiming gebruiken om op te slaan uw VMs. Zorg ervoor dat eventuele aanvullende containers op dezelfde manier als de eerdere stappen maken voordat u de afbeeldingen van uw aangepaste schijf uploadt.

Het volgende voorbeeld wordt een VM met de naam `myVM` van de afbeelding van uw aangepaste schijf:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Moet u ook opgeven of answer vraagt om, de aanvullende parameters vereist door de `azure vm create` command zoals virtueel netwerk, openbare IP-adres, gebruikersnaam en SSH toetsen. Meer informatie over de [beschikbare CLI resourcemanager parameters](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Een VM met een JSON-sjabloon maken
Azure resourcemanager sjablonen zijn JavaScript Object notatie (JSON)-bestanden die definiëren van de omgeving die u wilt maken. De sjablonen zijn onderverdeeld andere resource-providers zoals berekeningscluster of netwerk. U kunt bestaande sjablonen gebruiken of schrijft u uw eigen. Meer informatie over het [gebruik van de Resource Manager en sjablonen](../azure-resource-manager/resource-group-overview.md).

Binnen de `Microsoft.Compute/virtualMachines` provider van de sjabloon die u hebt een `storageProfile` knooppunt dat de configuratiegegevens voor uw VM bevat. De twee belangrijkste parameters voor het bewerken zijn de `image` en `vhd` URI's die verwijzen naar de afbeelding van uw aangepaste schijf en uw nieuwe VM virtuele schijf. Hier volgt een voorbeeld van de JSON voor het gebruik van de afbeelding van een aangepaste schijf:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

U kunt [deze bestaande sjabloon te maken van een VM uit een aangepaste afbeelding](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) gebruiken of lees meer over het [maken van uw eigen resourcemanager Azure-sjablonen](../resource-group-authoring-templates.md). 

Nadat u een sjabloon die is geconfigureerd hebt, maakt u uw VMs met de `azure group deployment create` opdracht. Geef de URI van de JSON-sjabloon met de `--template-uri` parameter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Als u een JSON-bestand lokaal opgeslagen op uw computer hebt, kunt u de `--template-file` parameter in plaats daarvan:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en de aangepaste virtuele schijf hebt geüpload, kunt u meer informatie over het [gebruik van de Resource Manager en sjablonen](../azure-resource-manager/resource-group-overview.md). U kunt ook [een gegevensschijf](virtual-machines-linux-add-disk.md) toevoegen aan uw nieuwe VMs. Als u toepassingen op uw VMs die u nodig hebt voor toegang tot hebt, moet u [open poorten en -eindpunten](virtual-machines-linux-nsg-quickstart.md).