<properties
    pageTitle="Maken en uploaden van een Linux CentOS gebaseerde VHD in Azure wordt aangegeven"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een CentOS gebaseerde Linux-besturingssysteem."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Een virtuele CentOS gebaseerde machine voorbereiden voor Azure

- [Een CentOS 6.x virtuele machine voorbereiden voor Azure](#centos-6x)
- [Een virtuele CentOS 7.0 + machine voorbereiden voor Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten voor ##

In dit artikel wordt ervan uitgegaan dat u al een CentOS hebt geïnstalleerd (of soortgelijke afgeleide) Linux-besturingssysteem aan een virtuele harde schijf. Er bestaan meerdere hulpprogramma's om VHD-bestanden, bijvoorbeeld een oplossing virtualization zoals Hyper-V te maken. Zie [de functie Hyper-V en configureren van een virtuele Machine installeren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.


**Installatieopmerkingen centOS**

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De indeling VHDX wordt niet ondersteund in Azure wordt aangegeven, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd.

- Het wordt aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken tijdens de installatie van het systeem Linux. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing.  [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.

- NUMA wordt niet ondersteund voor grotere VM vanwege een fout in Linux versies onder 2.6.37. Dit probleem invloed hoofdzakelijk onderzoeken met het boven rode rol 2.6.32 kernel. Handmatige installatie van de Azure Linux-agent (waagent) wordt automatisch NUMA uitgeschakeld in de configuratie WORMGATEN voor de kernel Linux. Meer informatie hierover vindt u in de onderstaande stappen.

- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

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

        # sudo chkconfig network on


8. **CentOS 6.3 alleen**: Installeer de stuurprogramma's voor Linux Integration Services (hierin).

    **Belangrijk: De stap is alleen geldig voor het CentOS 6.3 en lager.**  De Linux Integration Services zijn in CentOS 6.4 + *al beschikbaar in de standaard kernel*.

    - Volg de installatie-instructies op de [downloadpagina LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) en installeer de RPM naar de afbeelding.  


9. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

10. Als u de komt overeen met OpenLogic die worden gehost binnen de Azure datacenters gebruiken wilt, moet u vervolgens het bestand /etc/yum.repos.d/CentOS-Base.repo vervangen door de volgende opslagplaatsen.  Dit wordt ook de **[openlogic]** opslagplaats waarin pakketten voor de Azure Linux-agent bevat toevoegen:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Notitie:** De rest van deze handleiding wordt wordt ervan uitgegaan dat u gebruikt ten minste de cessies‑retrocessies [openlogic], die worden gebruikt voor het installeren van de onderstaande Azure Linux-agent.


11. Voeg de volgende regel toe /etc/yum.conf:

        http_caching=packages

    En voeg de volgende regel **op alleen CentOS 6.3** :

        exclude=kernel*

12. De module yum "fastestmirror" uitschakelen door het bestand te bewerken ' / etc/yum/pluginconf.d/fastestmirror.conf ', en klik onder `[main]`, typt u het volgende:

        set enabled=0

13. Voer de volgende opdracht om te wissen van de huidige yum metagegevens:

        # yum clean all

14. **Klik op CentOS 6.3 alleen**, de kernel met de volgende opdracht bijwerken:

        # sudo yum --disableexcludes=all install kernel

15. De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe openen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA uitgeschakeld vanwege een fout in de kernelversie gebruikt door CentOS 6.

    Naast het bovenstaande, wordt het aanbevolen *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt de hoeveelheid verminderen beschikbare geheugen in VM 128MB of meer, die mogelijk problematisch op het kleinere VM notitie.


16. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

17. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent

    Houd er rekening mee dat het NetworkManager installatie van het pakket WALinuxAgent worden verwijderd en NetworkManager-gnome-pakketten als ze volgens de beschrijving niet al zijn verwijderd in stap 2.

18. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Wijzigingen in CentOS 7 (en soortgelijke producten)**

Een virtuele CentOS 7 machine voorbereiden voor Azure is vergelijkbaar met CentOS 6, maar er zijn enkele belangrijke verschillen overigens:

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

6.  Wijzig udev regels om te voorkomen dat statische regels voor de Ethernet-interfaces. Deze regels kunnen problemen veroorzaken wanneer een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Controleer of dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

7. Installeer het pakket python-pyasn1 door de volgende opdracht uit te voeren:

        # sudo yum install python-pyasn1

8. Als u de komt overeen met OpenLogic die worden gehost binnen de Azure datacenters gebruiken wilt, moet u vervolgens het bestand /etc/yum.repos.d/CentOS-Base.repo vervangen door de volgende opslagplaatsen.  Dit wordt ook de **[openlogic]** opslagplaats waarin pakketten voor de Azure Linux-agent bevat toevoegen:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Notitie:** De rest van deze handleiding wordt wordt ervan uitgegaan dat u gebruikt ten minste de cessies‑retrocessies [openlogic], die worden gebruikt voor het installeren van de onderstaande Azure Linux-agent.

9.  De volgende opdracht uit te schakelt u de huidige yum-metagegevens en installeer alle updates:

        # sudo yum clean all
        # sudo yum -y update

10. De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Hiervoor openen "/ enzovoort/standaard/grub' in een teksteditor en bewerken de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Dit zorgt ook ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Ook worden uitgeschakeld de nieuwe CentOS 7-naamgevingsconventies voor NIC's. Naast het bovenstaande, wordt het aanbevolen *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De `crashkernel` optie is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt de hoeveelheid verminderen beschikbare geheugen in VM 128MB of meer, die mogelijk problematisch op het kleinere VM notitie.

11. Zodra u klaar bent met bewerken '/ enzovoort/standaard/wormgaten"per hierboven, voer de volgende opdracht om de configuratie wormgaten opnieuw te maken:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

13. **Alleen als het bouwen van de afbeelding uit VMWare, VirtualBox of KVM:** Hyper-V modules in initramfs toevoegen:

    Bewerken `/etc/dracut.conf`, inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Bouw die tabellen opnieuw de initramfs:

        # dracut –f -v

14. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de CentOS Linux virtuele harde schijf te maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
