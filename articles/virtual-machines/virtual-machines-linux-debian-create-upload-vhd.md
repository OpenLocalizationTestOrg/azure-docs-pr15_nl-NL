<properties
    pageTitle="Debian Linux VHD voorbereiden | Microsoft Azure"
    description="Informatie over het maken van Debian 7 en 8 VHD-bestanden voor implementatie in Azure wordt aangegeven."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Een Debian VHD voorbereiden voor Azure

## <a name="prerequisites"></a>Vereisten voor
In deze sectie wordt ervan uitgegaan dat u al een Debian Linux-besturingssysteem hebt geïnstalleerd vanaf een ISO-bestand dat is gedownload van de [website van Debian](https://www.debian.org/distrib/) naar een virtuele harde schijf. Er bestaan meerdere hulpprogramma's om bestanden van VHD; te maken Hyper-V is slechts één voorbeeld. Zie [installeren van de functie Hyper-V en een virtuele Machine configureren](https://technet.microsoft.com/library/hh846766.aspx)voor instructies voor het gebruik van Hyper-V.


## <a name="installation-notes"></a>Installatieopmerkingen

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.
- De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet **converteren vhd** .
- Het wordt aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken tijdens de installatie van het systeem Linux. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.
- Configureer een partition omwisselen niet op de schijf OS. De Azure Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken. Meer informatie hierover vindt u in de onderstaande stappen.
- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Azure-beheren via Debian VHD's maken

Er bestaan hulpprogramma's beschikbaar zijn voor het genereren van Debian VHD's voor Azure, zoals de [azure-beheren](https://github.com/credativ/azure-manage) scripts [credativ](http://www.credativ.com/). Dit is de aanbevolen wijze versus het maken van een afbeelding maken. Bijvoorbeeld maken een Debian 8 VHD Voer de volgende opdrachten om te downloaden azure-beheren (en afhankelijkheden) en de azure_build_image-script uitvoeren:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Handmatig een Debian VHD voorbereiden

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3. Opmerking de regel voor **deb cd-rom** in `/etc/apt/source.list` als u de VM ten opzichte van een ISO-bestand hebt ingesteld.

4. Bewerken de `/etc/default/grub` archiveren en wijzig de parameter **GRUB_CMDLINE_LINUX** als volgt als u wilt opnemen extra kernel parameters voor Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. De grub opnieuw te maken en uitvoeren:

        # sudo update-grub

6. Debian van Azure opslagplaatsen toevoegen aan /etc/apt/sources.list voor Debian 7 of 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Installeer de Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Voor Debian 7: dit is vereist om uit te voeren van de kernel 3,16 gebaseerde uit de bibliotheek wheezy backports. Maak eerst een bestand met de naam van /etc/apt/preferences.d/linux.pref met de volgende onderwerpen:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Voer 'sudo enz-get-installatie linux-afbeelding-amd64' voor het installeren van de nieuwe kernel.

8. De virtuele machine deprovision en deze voorbereiden voor het inrichten van op Azure en uitvoeren:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie** -afsluiten > ingedrukt in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen

U bent nu klaar voor gebruik van de Debian virtuele harde schijf te maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
