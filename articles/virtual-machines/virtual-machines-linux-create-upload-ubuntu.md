<properties
    pageTitle="Maken en uploaden van een Ubuntu Linux VHD in Azure wordt aangegeven"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een Ubuntu Linux-besturingssysteem."
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
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Een VM Ubuntu voorbereiden voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Officiële Ubuntu cloud-afbeeldingen
Ubuntu publiceert nu officiële Azure VHD's downloaden vanaf [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Als u uw eigen gespecialiseerde Ubuntu-afbeelding maken voor Azure moet, liever dan gebruik van de volgende handmatige procedure is het raadzaam om te beginnen met deze bekende VHD's werken en naar wens aanpassen. De meest recente versies van de afbeelding altijd vindt u op de volgende locaties:

 - Ubuntu 12.04/nauwkeurig: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/betrouwbare: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Vereisten voor

In dit artikel wordt ervan uitgegaan dat u al een Ubuntu Linux-besturingssysteem aan een virtuele harde schijf hebt geïnstalleerd. Er bestaan meerdere hulpprogramma's om VHD-bestanden, bijvoorbeeld een oplossing virtualization zoals Hyper-V te maken. Zie [de functie Hyper-V en configureren van een virtuele Machine installeren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.

**Installatieopmerkingen Ubuntu**

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.
- De indeling VHDX wordt niet ondersteund in Azure wordt aangegeven, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd.
- Het wordt aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken tijdens de installatie van het systeem Linux. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.
- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken.  Meer informatie hierover vindt u in de onderstaande stappen.
- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.


## <a name="manual-steps"></a>Handmatige stappen

> [AZURE.NOTE] Overweeg het gebruik van de afbeeldingen van [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) in plaats daarvan voordat u uw eigen aangepaste Ubuntu afbeelding maakt voor Azure.


1. Selecteer in het middelste deelvenster van Hyper-V Manager, de virtuele machine.

2. Klik op **verbinding maken** om het venster voor de virtuele machine te openen.

3.  Vervang de huidige opslagplaatsen in de afbeelding van Ubuntu Azure repo's gebruiken. De stappen verschillen afhankelijk van de versie Ubuntu.

    Voordat u met het bewerken van /etc/apt/sources.list, wordt het aanbevolen een reservekopie te maken:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. De afbeeldingen Ubuntu Azure volgt nu de kernel *hardware activering* (HWE). Het besturingssysteem bijwerken naar de meest recente kernel door de volgende opdrachten uit te voeren:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. De regel kernel opstarten voor wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Hiervoor openen "/ enzovoort/standaard/grub' in een teksteditor, vindt u de variabele met de naam `GRUB_CMDLINE_LINUX_DEFAULT` (of voeg deze toe als nodig) en als u wilt opnemen van de volgende parameters te bewerken:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Opslaan en sluit dit bestand en vervolgens uitvoeren '`sudo update-grub`'. Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin de technische ondersteuning van de Azure met foutopsporing problemen kan helpen.

6.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

7.  Installeer de Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Houd er rekening mee dat installatie van de `walinuxagent` pakket worden verwijderd de `NetworkManager` en `NetworkManager-gnome` pakketten, als ze zijn geïnstalleerd.

8.  Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Ubuntu Linux virtuele harde schijf te maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Verwijzingen ##

Ubuntu hardware activering (HWE) kernel:

- [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
