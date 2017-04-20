<properties
    pageTitle="Maken en uploaden van een Oracle Linux VHD | Microsoft Azure"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een Oracle-Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Een VM Oracle Linux voorbereiden voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten voor ##

In dit artikel wordt ervan uitgegaan dat u al een Oracle-Linux-besturingssysteem aan een virtuele harde schijf hebt geïnstalleerd. Er bestaan meerdere hulpprogramma's om VHD-bestanden, bijvoorbeeld een oplossing virtualization zoals Hyper-V te maken. Zie [de functie Hyper-V en configureren van een virtuele Machine installeren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.


### <a name="oracle-linux-installation-notes"></a>Oracle Linux Installatieopmerkingen

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- Van Oracle rood rol compatibele kernel en hun UEK3 (Unbreakable Enterprise Kernel) beide op Hyper-V en Azure worden ondersteund. Voor de beste resultaten Zorg ervoor dat u bijwerkt naar de meest recente kernel bij het voorbereiden van uw Oracle Linux VHD.

- Van Oracle UEK2 wordt niet ondersteund op Hyper-V en Azure als deze niet bij de vereiste stuurprogramma's inbegrepen is.

- De indeling VHDX wordt niet ondersteund in Azure wordt aangegeven, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd.

- Het wordt aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken tijdens de installatie van het systeem Linux. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in Linux versies onder 2.6.37. Dit probleem invloed hoofdzakelijk onderzoeken met het boven rode rol 2.6.32 kernel. Handmatige installatie van de Azure Linux-agent (waagent) wordt automatisch NUMA uitgeschakeld in de configuratie WORMGATEN voor de kernel Linux. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.

- Zorg ervoor dat de `Addons` opslagplaats is ingeschakeld. Bewerk het bestand `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzig de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

Specifieke configuratiestappen uit in het besturingssysteem voor de virtuele machine om uit te voeren in Azure wordt aangegeven, moet u uitvoeren.

1. Selecteer in het middelste deelvenster van Hyper-V Manager, de virtuele machine.

2. Klik op **verbinding maken** om het venster voor de virtuele machine te openen.

3. NetworkManager verwijderen door de volgende opdracht uit te voeren:

        # sudo rpm -e --nodeps NetworkManager

    **Notitie:** Als het pakket niet al is geïnstalleerd, wordt deze opdracht, mislukt met een foutbericht wordt weergegeven. Dit is normaal.

4.  Maken van een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Maken van een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Wijzig udev regels om te voorkomen dat statische regels voor de Ethernet-interfaces. Deze regels kunnen problemen veroorzaken wanneer een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Controleer of dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # chkconfig network on

8. Installeer python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

9.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Moet u deze openen doen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA vanwege een fout in de Oracle rood rol compatibele kernel uitgeschakeld.

    Naast het bovenstaande, wordt het aanbevolen *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt de hoeveelheid verminderen beschikbare geheugen in VM 128MB of meer, die mogelijk problematisch op het kleinere VM notitie.


10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

11. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren. De meest recente versie is 2.0.15.

        # sudo yum install WALinuxAgent

    Houd er rekening mee dat het NetworkManager installatie van het pakket WALinuxAgent worden verwijderd en NetworkManager-gnome-pakketten als ze volgens de beschrijving niet al zijn verwijderd in stap 2.

12. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Wijzigingen in Oracle Linux 7**

Een VM Oracle Linux 7 voorbereiden voor Azure is vergelijkbaar met Oracle Linux 6, maar er zijn enkele belangrijke verschillen overigens:

 - De rode rol compatibele kernel zowel van Oracle UEK3 worden ondersteund in Azure wordt aangegeven.  De kernel UEK3 wordt aanbevolen.
 - Het pakket NetworkManager is niet langer conflicten oplevert met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en het is raadzaam deze niet is verwijderd.
 - GRUB2 wordt nu gebruikt als de standaard-bootloader, zodat de procedure voor het bewerken van parameters kernel is gewijzigd (Zie hieronder).
 - XFS is nu de standaard-bestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.


**Volgende configuratiestappen uit**

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  Maken van een bestand met de naam **netwerk** in de `/etc/sysconfig/` directory met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Maken van een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Wijzig udev regels om te voorkomen dat statische regels voor de Ethernet-interfaces. Deze regels kunnen problemen veroorzaken wanneer een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Controleer of dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8.  De volgende opdracht uit te schakelt u de huidige yum-metagegevens en installeer alle updates:

        # sudo yum clean all
        # sudo yum -y update

9.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Hiervoor openen "/ enzovoort/standaard/grub' in een teksteditor en bewerken de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Ook worden uitgeschakeld de nieuwe indicatie 7-naamgevingsconventies voor NIC's. Naast het bovenstaande, wordt het aanbevolen *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt de hoeveelheid verminderen beschikbare geheugen in VM 128MB of meer, die mogelijk problematisch op het kleinere VM notitie.


10. Zodra u klaar bent met bewerken '/ enzovoort/standaard/wormgaten"per hierboven, voer de volgende opdracht om de configuratie wormgaten opnieuw te maken:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

12. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van uw Oracle Linux VHD maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
