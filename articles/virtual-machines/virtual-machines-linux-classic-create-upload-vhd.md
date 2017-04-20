<properties
    pageTitle="Maken en uploaden van een VHD Linux | Microsoft Azure"
    description="Maken en uploaden van een Azure virtuele harde schijf (VHD) met het klassieke implementatiemodel waarop het besturingssysteem Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Maken en het uploaden van een virtuele harde schijf waarop het besturingssysteem Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [een aangepaste schijf-afbeelding met Azure Resource Manager uploaden](virtual-machines-linux-upload-vhd.md).

In dit artikel leest u hoe maken en uploaden van een virtuele harde schijf (VHD), zodat u deze als uw eigen afbeelding gebruiken kunt virtuele machines maken in Azure wordt aangegeven. Informatie over het voorbereiden van het besturingssysteem, zodat u deze gebruiken kunt om te maken van meerdere virtuele machines op basis van die afbeelding. 

>  [AZURE.NOTE] Als u even hebt, kunt u ons helpen de documentatie Azure Linux VM verbeteren door deze [Snelle enquête](https://aka.ms/linuxdocsurvey) van uw ervaringen. Elk antwoord kan wij help u uw werk te doen krijgt.

## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u de volgende items hebt:

- **Linux-besturingssysteem is geïnstalleerd in een bestand VHD** - u een [Azure ondersteunde Linux verdeling](virtual-machines-linux-endorsed-distros.md) hebt geïnstalleerd (of Zie [informatie](virtual-machines-linux-create-upload-generic.md)voor onderzoeken niet ondersteunde) tot een virtuele schijf in de VHD-indeling. Er zijn meerdere hulpprogramma's waarmee u het maken van een VM en VHD:
    - Installeren en configureren van [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), daarbij VHD gebruiken als uw afbeelding opmaken. Indien nodig kunt u [een afbeelding converteren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) gebruiken `qemu-img convert`.
    - U kunt ook Hyper-V [in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [Windows Server 2012-2012 R2](https://technet.microsoft.com/library/hh846766.aspx)gebruiken.

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. Wanneer u een VM hebt gemaakt, geeft u VHD als de notatie. Indien nodig kunt u VHDX schijven converteren naar het gebruik van VHD [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure ondersteunt verder, geen uploaden dynamische VHD's, zodat u deze schijven converteren naar statische VHD's moet voordat u uploadt. Hulpprogramma's zoals [Azure VHD hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) kunt u dynamische schijven tijdens het proces van het uploaden naar Azure converteren.

- **Azure opdrachtregel-Interface** - Installeer de meest recente [Azure opdrachtregel-Interface](../virtual-machines-command-line-tools.md) voor het uploaden van de VHD.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Stap 1: De afbeelding om te worden geüpload voorbereiden

Azure ondersteunt verschillende Linux onderzoeken (Zie [Onderzoeken goedgekeurd](virtual-machines-linux-endorsed-distros.md)). De volgende artikelen helpt u bij het voorbereiden van de verschillende Linux onderzoeken die worden ondersteund op Azure. Nadat u de stappen in de volgende handleidingen hebt voltooid, afkomstig zijn terug hier nadat u een VHD-bestand dat u wilt uploaden naar Azure hebt:

- **[CentOS gebaseerde onderzoeken](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Rode rol Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Andere - onderzoeken niet ondersteunde](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Het Azure platform SLA is van toepassing op virtuele machines met de Linux-besturingssysteem alleen als een van de aangebracht onderzoeken met configuratiegegevens van de opgegeven onder versies worden ondersteund in [Linux op Azure-Endorsed onderzoeken](virtual-machines-linux-endorsed-distros.md)wordt gebruikt. Alle Linux onderzoeken in de afbeeldinggalerie met Azure zijn aangebracht onderzoeken met de vereiste configuratie.

Zie ook de **[Installatieopmerkingen Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** voor meer algemene tips over het voorbereiden van Linux afbeeldingen van Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Stap 2: De verbinding met Azure voorbereiden

Zorg ervoor dat u de CLI Azure gebruikt in het implementatiemodel klassieke (`azure config mode asm`), meld u aan bij uw account:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Stap 3: De afbeelding uploaden naar Azure

Moet u een account opslagruimte aan uw bestand VHD om te uploaden. U kunt op een bestaande opslag-account of [een nieuw account te maken](../storage/storage-create-storage-account.md)kiezen.

Gebruik de CLI Azure voor het uploaden van de afbeelding met behulp van de volgende opdracht uit:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

In het vorige voorbeeld:

- **BlobStorageURL** is de URL voor de opslag-account dat u wilt gaan gebruiken
- **YourImagesFolder** is de container binnen blobopslag waar u uw afbeeldingen op te slaan
- **VHDName** is het label dat wordt weergegeven in de portal om aan te geven van de virtuele harde schijf.
- **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand op uw computer.

Hieronder vindt u een volledig voorbeeld:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Stap 4: Een VM maken van de afbeelding
U maakt een VM met `azure vm create` op dezelfde manier als een gewone VM. Geef de naam die u voor uw afbeelding in de vorige stap hebt opgegeven. In het volgende voorbeeld gebruiken we de naam **UbuntuLTS** afbeelding in de vorige stap:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Als u wilt uw eigen VMs maken, vindt u uw eigen gebruikersnaam + wachtwoord, locatie, de naam van de DNS- en de naam van de afbeelding.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CLI overzicht van het model Azure klassieke-implementatie](../virtual-machines-command-line-tools.md)voor meer informatie.

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
