<properties
    pageTitle="Maken en uploaden van een VHD Linux in Azure wordt aangegeven"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informatie voor niet ondersteunde onderzoeken #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Belangrijk**: het Azure platform SLA is van toepassing op virtuele machines met de Linux-besturingssysteem alleen wanneer u een van de [onderzoeken goedgekeurd](virtual-machines-linux-endorsed-distros.md) wordt gebruikt. Alle Linux onderzoeken die beschikbaar zijn in de afbeeldinggalerie met Azure zijn aangebracht onderzoeken met de vereiste configuratie.

- [Linux op Azure - goedgekeurd onderzoeken](virtual-machines-linux-endorsed-distros.md)
- [Ondersteuning voor Linux afbeeldingen in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle onderzoeken waarop Azure moet voldoen aan een aantal vereisten om kans correct uitvoeren op het platform.  In dit artikel is in geen geval volledig zoals elke verdeling verschilt; en is het helemaal mogelijk dat zelfs als u de onderstaande criteria voldoen aan u nog steeds aanzienlijk aanpassen uw systeem Linux om ervoor te zorgen moeten dat deze juist wordt uitgevoerd op het platform.

Het is om die reden dat het is raadzaam dat u beginnen met een van onze [Linux op Azure goedgekeurd onderzoeken](virtual-machines-linux-endorsed-distros.md) indien mogelijk. De volgende artikelen begeleidt u bij het voorbereiden van de verschillende aangebracht Linux onderzoeken die worden ondersteund op Azure:

- **[CentOS gebaseerde onderzoeken](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Rode rol Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

De rest van dit artikel worden de algemene richtlijnen voor het uitvoeren van de verdeling van uw Linux op Azure belichten.


## <a name="general-linux-installation-notes"></a>Algemene Linux Installatieopmerkingen ##

- De indeling VHDX wordt niet ondersteund in Azure wordt aangegeven, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd. Als u werkt met VirtualBox betekent dit dat **vaste grootte** selecteren in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.

- Tijdens de installatie van het Linux-systeem is het *aanbevolen* dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere identieke VM moet voor probleemoplossing. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag worden gebruikt op gegevensschijven.

- Kernel ondersteuning voor het koppelen van UDF-bestandssysteem is vereist. De inrichten configuratie wordt bij de eerste keer op Azure doorgegeven aan de VM Linux via UDF-indeling media die is gekoppeld aan de Gast. De Azure Linux-agent moet kunnen het bestandssysteem UDF om te lezen van de configuratie en inrichten van de VM koppelen.

- Linux versies onder 2.6.37 ondersteunen geen NUMA op Hyper-V met grotere VM. Dit probleem hoofdzakelijk effecten oudere onderzoeken met het boven rode rol 2.6.32 kernel, en is vastgezet op RHEL 6.6 (kernel-2.6.32-504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of RHEL gebaseerde kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten `numa=off` op de opdrachtregel in grub.conf kernel. Zie rood rol [KB 436883](https://access.redhat.com/solutions/436883)voor meer informatie.

- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.


### <a name="installing-linux-without-hyper-v"></a>Installatie van Linux zonder Hyper-V ###

In sommige gevallen Linux installatieprogramma's mogelijk niet bevatten de stuurprogramma's voor Hyper-V op de eerste RAM-schijf (initrd of initramfs) tenzij wordt vastgesteld dat deze wordt uitgevoerd een een Hyper-V-omgeving.  Wanneer u met een ander virtualization-systeem (dat wil zeggen Virtualbox, KVM, enzovoort) voor het voorbereiden van uw afbeelding Linux, moet u mogelijk opnieuw opbouwen de initrd om ervoor te zorgen dat ten minste de `hv_vmbus` en `hv_storvsc` kernelmodules zijn beschikbaar op de eerste ramdisk.  Dit is een bekend probleem ten minste op systemen op basis van de boven rood rol-verdeling.

De om de afbeelding initrd of initramfs PowerPoint opnieuw moet maken, is afhankelijk van de verdeling. Raadpleeg de documentatie van uw verdeling of ondersteuning voor de juiste procedure.  Hier volgt een voorbeeld voor het opnieuw opbouwen van het gebruik van de initrd de `mkinitrd` hulpprogramma:

Eerst een back-up de bestaande initrd afbeelding:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Vervolgens opnieuw opbouwen de initrd met de `hv_vmbus` en `hv_storvsc` kernelmodules:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Het formaat van VHD's wijzigen ###

VHD afbeeldingen op Azure moet een virtuele grootte uitgelijnd op 1MB.  VHD's die zijn gemaakt met behulp van Hyper-V moeten meestal al correct worden uitgelijnd.  Als de VHD wordt niet juist uitgelijnd vervolgens mogelijk ontvangt u een foutbericht van de volgende strekking weergegeven wanneer u probeert te maken van een *afbeelding* vanaf uw VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

U lost dit probleem kunt u het formaat van de VM met behulp van de beheerconsole van Hyper-V of het [Formaat wijzigen-VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-cmdlet.  Als u niet in een Windows-omgeving gebruikt wordt vervolgens het aanbevolen kunt qemu-img gebruiken om te converteren (indien nodig) of de grootte van de VHD.

> [AZURE.NOTE] Er is een bekende fout in qemu-img versies > = 2.2.1 die in een onjuist opgemaakte VHD resulteert. Het probleem is opgelost in QEMU 2,6 af. Het wordt aanbevolen qemu-img 2.2.0 of lager gebruiken of bijwerken op 2.6 of hoger. Verwijzing: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Het formaat van de VHD rechtstreeks met hulpprogramma's zoals `qemu-img` of `vbox-manage` kan dit leiden tot een opgestart VHD.  Zodat wordt het aanbevolen naar eerste converteren de VHD naar een afbeelding ONBEWERKTE schijf.  Als de afbeelding VM al is gemaakt als ONBEWERKTE schijf afbeelding (de standaardinstelling voor sommige Hypervisors zoals KVM) kunt u deze stap overslaan:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. De vereiste grootte van de afbeelding Schijfopruiming om ervoor te zorgen dat de virtuele grootte wordt uitgelijnd op 1MB berekenen.  De volgende we vaker doen-shellscript kunt u doen met dit.  Het script gebruikt "`qemu-img info`' om te bepalen de virtuele grootte van de afbeelding schijf en vervolgens de grootte voor de volgende 1MB berekend:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Het formaat wijzigen de onbewerkte schijf $rounded_size gebruiken zoals in het bovenstaande script:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Nu de ONBEWERKTE schijf terug converteren naar een vaste grootte VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linux Kernel vereisten ##

De Linux Integration Services (LIS)-stuurprogramma's voor Hyper-V en Azure zijn bijgedragen rechtstreeks aan de boven Linux kernel. Veel onderzoeken, zoals een recente versie van Linux kernel (dat wil zeggen 3.x) worden al deze stuurprogramma's beschikbaar of anderszins backported versies van deze stuurprogramma's voorzien van hun kernels.  Deze stuurprogramma's worden voortdurend bijgewerkt in de boven-kernel met nieuwe reparaties en functies, zodat u indien mogelijk het wordt aanbevolen om uit te voeren van een [verdeling goedgekeurd](virtual-machines-linux-endorsed-distros.md) die deze correcties en updates bevat.

Als u een variatie van Red rol Enterprise Linux versies **6.0-6.3**uitvoert, moet u de meest recente LIS-stuurprogramma's voor Hyper-V installeren. De stuurprogramma's vindt u [op deze locatie](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (en afgeleide) de LIS-stuurprogramma's zijn al opgenomen in de kernel en dus geen aanvullende installatie-pakketten nodig zijn om uit te voeren die systemen op Azure.

Als een aangepaste kernel vereist is, wordt het wordt aanbevolen voor het gebruik van een recentere kernelversie (dat wil zeggen **3,8 +**). Voor deze onderzoeken of leveranciers die voor het behoud van hun eigen kernel moeite is vereist om regelmatig backport de LIS-stuurprogramma's van de kernel boven aan uw aangepaste kernel.  Zelfs als u al een relatief recente kernelversie uitvoert, moet het is raadzaam om bij te houden boven fixes in de LIS-stuurprogramma's en backport die naar wens. De locatie van de bronbestanden van de LIS-stuurprogramma is beschikbaar in het bestand [zullen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) in de structuur van Linux kernel bron:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Bij een zeer minimum, gebrek aan de volgende patches hebt is of problemen kunnen voordoen op Azure en dus deze moeten worden opgenomen in de kernel. Deze lijst is in geen geval volledig of voltooid voor alle onderzoeken:

- [ata_piix: schijven naar de Hyper-V-stuurprogramma's al dan niet standaard uitstellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: Account voor Klik in de overdracht-pakketten in het pad opnieuw instellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: gebruik van WRITE_SAME voorkomen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: dezelfde schrijven voor RAID en virtuele host adapter-stuurprogramma's uitschakelen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: null-waarden aanwijzer verwijzing fix](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: bellen buffer fouten kunnen dit leiden tot i/o-blokkeren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: beveiligen tegen dubbele uitvoering van __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>De Azure Linux-Agent ##

De [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) (waagent) is vereist voor het inrichten van correct Linux virtuele machines in Azure wordt aangegeven. U kunt krijgen van de meest recente versie, bestand problemen of halen aanvragen bij de [Linux Agent GitHub cessies‑retrocessies](https://github.com/Azure/WALinuxAgent)indienen.

- De Linux-agent is uitgebracht onder de Apache 2.0-licentie. Veel onderzoeken al bieden RPM of deb pakketten voor de agent en dus in sommige gevallen dit kan worden geïnstalleerd en bijgewerkt met weinig inspanning.

- De Azure Linux-Agent is Python v2.6 + vereist.

- De agent moet ook de module python-pyasn1. De meeste onderzoeken bieden dit als een afzonderlijk pakket dat kan worden geïnstalleerd.

- In sommige gevallen de Azure Linux-Agent mogelijk niet compatibel met NetworkManager. Veel van de RPM/Deb-pakketten verstrekt door onderzoeken NetworkManager configureren als een conflict aan het pakket waagent en dus verwijdert NetworkManager tijdens de installatie van het Linux-agent-pakket.


## <a name="general-linux-system-requirements"></a>Algemeen Linux-systeemvereisten ##

- Wijzig de regel voor het opstarten van kernel in GRUB of GRUB2 wilt opnemen in de volgende parameters. Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen.

    Naast het bovenstaande, wordt het aanbevolen *verwijderen* de volgende parameters als ze bestaat:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt de hoeveelheid verminderen beschikbare geheugen in VM 128MB of meer, die mogelijk problematisch op het kleinere VM notitie.

- Installatie van de Azure Linux-Agent

    De Azure Linux-Agent is vereist voor het inrichten van een afbeelding Linux op Azure.  Veel onderzoeken bieden de agent als een RPM of Deb-pakket (het pakket is meestal genoemd 'WALinuxAgent' of 'walinuxagent').  De agent kan ook handmatig worden geïnstalleerd door de stappen in de [Handleiding voor Linux-Agent](virtual-machines-linux-agent-user-guide.md).

- Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

- Maak geen omwisselen ruimte op de schijf OS

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Een laatste stap, voer de volgende opdrachten aan de virtuele machine deprovision:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Klik op Virtualbox ziet u mogelijk het volgende foutbericht weergegeven als wordt uitgevoerd ' waagent-afdwingen - identiteitsgegevens ': `[Errno 5] Input/output error`. Dit foutbericht wordt weergegeven, is niet kritieke en kan worden genegeerd.

- Vervolgens moet u de virtuele machine afsluiten en de VHD uploaden naar Azure.
