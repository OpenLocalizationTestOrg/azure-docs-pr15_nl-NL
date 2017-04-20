<properties
    pageTitle="Optimaliseren van uw VM Linux op Azure | Microsoft Azure"
    description="Informatie over enkele tips voor optimalisatie om ervoor te zorgen dat u hebt uw Linux VM voor optimale prestaties op Azure ingesteld"
    keywords="Linux virtuele machine, VM linux, ubuntu virtuele machines" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Uw VM Linux op Azure optimaliseren

Maken van een Linux virtuele machine (VM) is heel eenvoudig vanaf de opdrachtregel of vanaf de portal. Deze zelfstudie ziet u hoe u zorgen dat u dit hebt ingesteld voor het optimaliseren van de prestaties op de Microsoft Azure-platform. In dit onderwerp wordt een VM Ubuntu-Server gebruikt, maar u kunt ook Linux VM met [uw eigen afbeeldingen als sjablonen](virtual-machines-linux-create-upload-generic.md)maken.  

## <a name="prerequisites"></a>Vereisten voor

In dit onderwerp wordt ervan uitgegaan dat u al een werkende Azure-abonnement ([gratis proefabonnement aanmelden](https://azure.microsoft.com/pricing/free-trial/)), [de CLI Azure geïnstalleerd](../xplat-cli-install.md) hebt en die u hebt al een VM ingericht in uw Azure-abonnement. U moet eerst willekeurig met Azure - verifiëren aan uw abonnement. Klik hiertoe met Azure CLI alleen typen `azure login` om de interactieve proces te starten. 

## <a name="azure-os-disk"></a>Azure OS Schijfopruiming

Nadat u een Linux Vm in Azure maakt, heeft twee schijven die zijn gekoppeld. /dev/SDA is de schijf OS, /dev/sdb is uw tijdelijke schijf.  Gebruik de belangrijkste OS schijf (/ ontwikkelaar/sda) niet voor iets anders dan het besturingssysteem, zoals deze wordt geoptimaliseerd voor snelle VM opstarten en geen goede prestaties voor uw werkbelasting biedt. U wilt een of meer schijven als bijlage toevoegen aan uw VM om permanente en geoptimaliseerd opslagruimte voor uw gegevens. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Schijven toe te voegen voor de grootte en prestaties van doelen 

Op basis van de grootte VM, kunt u maximaal 16 extra schijven op een A-reeks, 32 schijven op een D-reeks koppelen en 64 schijven op een G-reeks machine - elk maximaal 1 TB grootte. U toevoegen zo nodig per uw ruimte en IO's / s vereisten extra schijven. Elke schijf heeft een prestatiedoel van 500 IO's / s voor standaardopslag plus maximaal 5000 IO's / s per schijf voor Premium opslag.  Raadpleeg voor meer informatie over schijven voor Premium, [Premium opslag: High-Performance-opslag voor Azure VMs](../storage/storage-premium-storage.md)

Als u wilt de hoogste IO's / s op Premium schijven waar de cache-instellingen zijn ingesteld op "Alleen-lezen" of "Geen" bereiken, moet u "afsluitingen" uitschakelen tijdens het koppelen van de-bestandssysteem in Linux. U hoeft niet afsluitingen omdat het schrijven naar Premium back schijven duurzame voor de cache-instellingen.

- Als u **reiserFS**gebruikt, schakelt u afsluitingen met de optie koppeling "blok geen =" (gebruiken voor het inschakelen van afsluitingen, "blok uitlijnen =")
- Als u **ext3/ext4**, schakelt u met de optie koppeling afsluitingen "blok = 0 ' (voor het inschakelen van afsluitingen, gebruikt u ' blok = 1")
- Als u **XFS**gebruikt, schakelt u afsluitingen met de optie koppeling "nobarrier" (voor het inschakelen van afsluitingen, gebruikt u de optie "blok")

## <a name="storage-account-considerations"></a>Aandachtspunten voor de opslag-Account

Wanneer u uw VM Linux in Azure wordt aangegeven maakt, moet u ervoor zorgen dat u schijven koppelen van opslag-accounts op hetzelfde gebied, als uw VM om te verzekeren dicht en netwerklatentie minimaliseren wonen.  Elke standaard opslag-account heeft een maximum van 20 k IO's / s en de capaciteit van een 500 TB grootte.  Dit werkt naar ongeveer 40 intensief gebruikte schijven inclusief zowel de schijf OS en alle gegevensschijven die u maakt. Onbeperkt maximale IO's / s voor Premium opslag-accounts, maar er is een bestandslimiet 32 TB. 

Wanneer omgaan met hoge IO's / s werkbelasting en u er standaard opslagruimte voor uw schijven hebt gekozen, moet u mogelijk de schijven splitsen meerdere opslag accounts om ervoor te zorgen dat u hebt niet de limiet 20.000 IO's / s bereikt voor standaardopslag-accounts. Uw VM kan een combinatie van schijven uit bevatten over de opslag van verschillende accounts en accounttypen opslagruimte aan uw optimale configuratie bereiken. 

## <a name="your-vm-temporary-drive"></a>Uw tijdelijke VM harde schijf

Al dan niet standaard bij het maken van een VM, biedt Azure u een schijf OS (/ ontwikkelaar/sda) en een tijdelijke schijf (/ ontwikkelaar/sdb).  Alle aanvullende schijven die u hebt toegevoegd weergegeven als /dev/sdc, /dev/sdd, /dev/sde enzovoort. Alle gegevens op de tijdelijke schijf (/ ontwikkelaar/sdb) niet blijvend is, en kunnen verloren gaan als specifieke gebeurtenissen zoals VM formaat wijzigen, opnieuw installeren, of onderhoud forceert u een herstart van uw VM.  De grootte en type van uw tijdelijke schijf is gerelateerd aan de VM-grootte die u hebt gekozen bij de implementatie. Als een van de premium-grootte VMs (DS, G en DS_V2 reeksenas) wordt de tijdelijke station ondersteund door een lokale SSD voor extra prestaties van maximaal 48 k IO's / s. 

## <a name="linux-swap-file"></a>Linux omwisselen-bestand

Als uw VM Azure van een afbeelding Ubuntu of CoreOS is, kunt u een cloud-config versturen naar cloud initialisatie CustomData gebruiken. Als u [een aangepaste Linux-afbeelding hebt geüpload](virtual-machines-linux-upload-vhd.md) die cloud initialisatie gebruikt, u ook configureren omwisselen partities met cloud initialisatie.

Voor Ubuntu Cloud afbeeldingen, moet u cloud initialisatie gebruiken voor het configureren van de partition uitwisselen. Zie de volgende pagina op de wiki Ubuntu voor meer informatie: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Voor afbeeldingen zonder cloud initialisatie ondersteuning hebben VM afbeeldingen die zijn geïmplementeerd van Azure Marketplace een VM Linux-Agent geïntegreerd met het besturingssysteem. Deze agent kan de VM om te communiceren met verschillende Azure services. Als dat u een standaard afbeelding van Azure Marketplace hebt geïmplementeerd, moet u als volgt uw instellingen Linux omwisselen bestand configureren:

Zoeken en twee vermeldingen in het bestand **/etc/waagent.conf** wijzigen. Ze bepalen de aanwezigheid van een bestand speciale uitwisselen en grootte van het bestand uitwisselen. De parameters die u op zoek bent om te wijzigen zijn `ResourceDisk.EnableSwap=N` en`ResourceDisk.SwapSizeMB=0` 

U moet deze als volgt uit:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size in MB aan uw behoeften} 

Nadat u de wijziging hebt aangebracht, moet u naar de waagent start of opnieuw op van uw VM Linux zodat de wijzigingen.  U weet dat de wijzigingen die zijn geïmplementeerd en een omwisselen-bestand is gemaakt wanneer u de `free` opdracht om weer te geven van de beschikbare ruimte. In het onderstaande voorbeeld heeft een 512MB omwisselen bestand gemaakt als gevolg van het wijzigen van het bestand waagent.conf.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/o-planning algoritme voor Premium-opslag

Met de 2.6.18 Linux kernel, de standaard I/O algoritme van de planning is gewijzigd van Deadline naar CFQ (volledig fair wachtrij plaatsen algoritme). Er is verwaarloosbaar verschil in prestatieverschillen tussen CFQ en Deadline voor RAM i/o-patronen.  Bezig met terugschakelen naar de algoritme van de Nooperation of Deadline bereiken betere o prestaties voor SSD gebaseerde schijven waar het patroon van de i/o-schijf hoofdzakelijk opeenvolgende is.

### <a name="view-the-current-io-scheduler"></a>De huidige i/o-planner weergeven

Gebruik de volgende opdracht uit:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

U ziet na uitvoer, geeft de huidige planner.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Het huidige apparaat (/ ontwikkelaar/sda) van I/O algoritme van de planning wijzigen

De volgende opdrachten gebruiken:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Niet is het handig om dit instelt voor /dev/sda alleen. Deze moeten worden ingesteld op alle gegevensschijven waar opeenvolgende I/O het patroon i/o-zomaar.  

U ziet het volgende resultaat, die aangeeft dat grub.cfg zijn opnieuw gemaakt en dat de standaard-planner is bijgewerkt naar Nooperation.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Voor het gezin Redhat verdeling hoeft u alleen de volgende opdracht uit:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Software RAID gebruiken om te bereiken hoger ik / Ops

Als uw werkbelasting meer IOP's vereist dan mogelijk is één schijf, moet u een software RAID configuratie van meerdere schijven gebruiken. Omdat Azure al schijf tolerantie op de lokale stof laag uitvoert, kunt u het hoogste niveau van de prestaties van een RAID-0 gesegmenteerd te verdelen configuratie bereiken.  U moet inrichten en nieuwe schijven maken in de Azure-omgeving en koppel deze aan uw VM Linux vóór partitioneren, opmaak en de stations koppelen.  Meer informatie over het configureren van een software RAID-instellingen op uw VM Linux in azure vindt u in het document **[Software RAID configureren op Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Volgende stappen

Onthouden, zoals met alle optimalisatie discussies, moet u tests uitvoeren voor en na elke wijziging de impact van de wijziging meten.  Optimalisatie is een proces stap voor stap die verschillende resultaten op verschillende computers in uw omgeving.  Wat werkt voor één configuratie werkt mogelijk niet voor anderen.

Enkele nuttige koppelingen naar aanvullende bronnen: 

- [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md)
- [De gebruikershandleiding Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
- [MySQL-prestaties op Azure Linux VMs optimaliseren](virtual-machines-linux-classic-optimize-mysql.md)
- [Software RAID op Linux configureren](virtual-machines-linux-configure-raid.md)
