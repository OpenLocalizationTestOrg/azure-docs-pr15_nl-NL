<properties
pageTitle="Een Oracle Linux virtuele Machine voorbereiden voor Azure | Microsoft Azure"
description="Stap voor stap de configuratie van een Oracle-VM Linux uitgevoerd in Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Een VM Oracle Linux voorbereiden voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Een VM Oracle Linux 6.4 + voorbereiden voor Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Een VM Oracle Linux 7.0 + voorbereiden voor Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u al een Oracle-Linux-besturingssysteem aan een virtuele harde schijf hebt geïnstalleerd. Er bestaan meerdere hulpprogramma's om VHD-bestanden, bijvoorbeeld een oplossing virtualization zoals Hyper-V te maken. Zie voor instructies [Hyper-V installeren en maak een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

**Oracle Linux Installatieopmerkingen**

- Van Oracle rood rol compatibele kernel en de UEK3 (Unbreakable Enterprise Kernel) beide op Hyper-V en Azure worden ondersteund. Voor de beste resultaten moet bijwerken naar de meest recente kernel bij het voorbereiden van uw Oracle Linux VHD.

- Van Oracle UEK2 wordt niet ondersteund op Hyper-V en Azure als deze niet bij de vereiste stuurprogramma's inbegrepen is.

- De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. U kunt de schijf converteren naar VHD opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd.

- Wanneer u het systeem Linux installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) te gebruiken. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. LVM of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in Linux versies onder 2.6.37. Dit probleem invloed hoofdzakelijk onderzoeken het boven met rode rol 2.6.32 kernel. Handmatige installatie van de Azure Linux-agent (waagent) wordt automatisch NUMA uitgeschakeld in de configuratie WORMGATEN voor de kernel Linux. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken. Meer informatie hierover vindt u in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.

- Zorg ervoor dat de `Addons` opslagplaats is ingeschakeld. Kies het bestand wilt bewerken `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Specifieke configuratiestappen uit in het besturingssysteem voor de virtuele machine om uit te voeren in Azure wordt aangegeven, moet u uitvoeren.

1. Selecteer in het middelste deelvenster van Hyper-V Manager, de virtuele machine.

2. Klik op **verbinding maken** om het venster voor de virtuele machine te openen.

3. NetworkManager verwijderen door de volgende opdracht uit te voeren:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Als het pakket niet al is geïnstalleerd, wordt deze opdracht, mislukt met een foutbericht wordt weergegeven. Dit is normaal.

4. Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Maken van een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Udev regels om te vermijden statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u bent een virtuele machine in Azure of Hyper-V: klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # chkconfig network on

8.  Installeer python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

9.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe openen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA vanwege een fout in de Oracle rood rol compatibele kernel uitgeschakeld.

    Naast het bovenstaande, raden we u *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit kan zijn op de kleinere VM problematisch.

10.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde.

11.  Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installeren WALinuxAgent

    Houd er rekening mee dat het NetworkManager installatie van het pakket WALinuxAgent worden verwijderd en NetworkManager-gnome-pakketten als ze volgens de beschrijving niet al zijn verwijderd in stap 2.

12.  Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat op Azure is geïnstalleerd. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en leeggemaakt mogelijk wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## NOTITIE: Stel dit in op wat u nodig hebt om te worden.

13.  Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-afdwingen - identiteitsgegevens
        # <a name="export-histsize0"></a>exporteren HISTSIZE = 0
        # <a name="logout"></a>Meld u af

14.  Klik op **actie -\> afgesloten** in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Wijzigingen in Oracle Linux 7**

Een VM Oracle Linux 7 voorbereiden voor Azure is vergelijkbaar met het proces voor Oracle Linux 6. Er zijn echter enkele belangrijke verschillen overigens:

-   De rode rol compatibele kernel zowel van Oracle UEK3 worden ondersteund in Azure wordt aangegeven. U wordt aangeraden de kernel UEK3.

-   Het pakket NetworkManager is niet langer conflicten oplevert met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en het is raadzaam deze niet is verwijderd.

-   GRUB2 wordt nu gebruikt als de standaard-bootloader, zodat de procedure voor het bewerken van parameters kernel is gewijzigd (Zie hieronder).

-   XFS is nu de standaard-bestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.

**Volgende configuratiestappen uit**

1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Maken van een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Udev regels om te vermijden statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels worden problemen veroorzaken wanneer u bent een virtuele machine in Microsoft Azure of Hyper-V klonen.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7.  Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8.  De volgende opdracht uit te schakelt u de huidige yum-metagegevens en installeer alle updates:

        # sudo yum clean all
        # sudo yum -y update

9.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Hiervoor openen "/ enzovoort/standaard/grub' in een teksteditor en bewerken de WORMGATEN\_OPDRACHTREGEL\_LINUX-parameter, bijvoorbeeld:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Naast het bovenstaande, raden we u *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit kan zijn op de kleinere VM problematisch.

10.  Zodra u klaar bent met bewerken '/ enzovoort/standaard/grub', voer de volgende opdracht uit om de configuratie wormgaten opnieuw te maken:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2-mkconfig - o /boot/grub2/grub.cfg

11.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde.

12.  Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum installeren WALinuxAgent

13.  Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat op Azure is geïnstalleerd. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en leeggemaakt mogelijk wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## NOTITIE: Stel dit in op wat u nodig hebt om te worden.

14.  Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-afdwingen - identiteitsgegevens
        # <a name="export-histsize0"></a>exporteren HISTSIZE = 0
        # <a name="logout"></a>Meld u af

15.  Klik op **actie -\> afgesloten** in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.
