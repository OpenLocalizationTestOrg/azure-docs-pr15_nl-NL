<properties
    pageTitle="Maken en uploaden van een SUSE Linux VHD in Azure wordt aangegeven"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een SUSE Linux-besturingssysteem."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Een virtuele SLES of openSUSE machine voorbereiden voor Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten voor ##

In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd een SUSE of Linux-besturingssysteem openSUSE aan een virtuele harde schijf. Er bestaan meerdere hulpprogramma's om VHD-bestanden, bijvoorbeeld een oplossing virtualization zoals Hyper-V te maken. Zie [de functie Hyper-V en configureren van een virtuele Machine installeren](http://technet.microsoft.com/library/hh846766.aspx)voor instructies.

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE Installatieopmerkingen

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De indeling VHDX wordt niet ondersteund in Azure wordt aangegeven, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de cmdlet converteren vhd.

- Het wordt aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) gebruiken tijdens de installatie van het systeem Linux. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. [LVM](virtual-machines-linux-configure-lvm.md) of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.

- Configureer een partition omwisselen niet op de schijf OS. De Linux-agent kan worden geconfigureerd als een bestand uitwisselen op de tijdelijke resource-schijf wilt maken.  Meer informatie hierover vindt u in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.


## <a name="use-suse-studio"></a>Gebruik SUSE Studio
[SUSE Studio](http://www.susestudio.com) kunt eenvoudig maken en beheren van uw afbeeldingen SLES en openSUSE voor Azure en Hyper-V. Dit is de aanbevolen methode voor het aanpassen van uw eigen afbeeldingen SLES en openSUSE.

Als alternatief voor het samenstellen van uw eigen VHD publiceert SUSE ook BYOS (uw eigen abonnement brengen) afbeeldingen voor SLES bij [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 voorbereiden ##

1. Selecteer in het middelste deelvenster van Hyper-V Manager, de virtuele machine.

2. Klik op **verbinding maken** om het venster voor de virtuele machine te openen.

3. Registreren uw systeem SUSE Linux Enterprise te laten pakketten updates downloaden en installeren.

4. Het systeem bijwerken met de meest recente patches:

        # sudo zypper update

5. De Azure Linux-Agent installeren vanaf de opslagplaats SLES:

        # sudo zypper install WALinuxAgent

6. Chkconfig inchecken als waagent is ingesteld op "op" en zo niet, inschakelen voor automatisch starten:
               
        # sudo chkconfig waagent on

7. Schakel dit selectievakje in als waagent service wordt uitgevoerd en zo niet, start u het: 

        # sudo service waagent start
                
8. De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Moet u deze openen doen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen.

9. Bevestig dat /boot/grub/menu.lst en /etc/fstab beide verwijzen naar de schijf de UUID (door-uuid) gebruikt in plaats van de schijf-ID (door-id). 

    Schijf UUID ophalen
    
        # ls /dev/disk/by-uuid/

    Als /dev/disk/by-id / is /boot/grub/menu.lst zowel/enzovoort/fstab hebt gebruikt, worden bijgewerkt met de juiste door uuid-waarde

    Voordat u wijzigen
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Na wijzigen
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Wijzig udev regels om te voorkomen dat statische regels voor de Ethernet-interfaces. Deze regels kunnen problemen veroorzaken wanneer een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Het wordt aanbevolen om het bestand te bewerken '/ enzovoort/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` -parameter voor de volgende handelingen uit:

        DHCLIENT_SET_HOSTNAME="no"

12. In '/ enzovoort/sudoers', uitschakelen of verwijderen van de volgende regels als ze bestaat:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

14. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.


----------

## <a name="prepare-opensuse-131"></a>OpenSUSE 13,1 + voorbereiden ##

1. Selecteer in het middelste deelvenster van Hyper-V Manager, de virtuele machine.

2. Klik op **verbinding maken** om het venster voor de virtuele machine te openen.

3. Klik op de shell, voert u de opdracht '`zypper lr`'. Als deze opdracht geeft uitvoer vergelijkbaar met de volgende handelingen uit en vervolgens de opslagplaatsen zijn geconfigureerd, zoals verwacht--geen wijzigingen (Houd er rekening mee dat versienummers kunnen verschillen):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Als de opdracht het resultaat 'Geen opslagplaatsen gedefinieerd...' gebruikt u de volgende opdrachten om toe te voegen deze repo's:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    U kunt vervolgens controleren of de opslagplaatsen zijn toegevoegd met de opdracht '`zypper lr`' opnieuw. Als een van de opslagplaatsen relevante update niet is ingeschakeld, kunt u deze met de volgende opdracht inschakelen:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. De kernel bijwerken naar de nieuwste versie:

        # sudo zypper up kernel-default

    Of het systeem met de meest recente patches bijwerken:

        # sudo zypper update

5.  Installeer de Azure Linux-Agent.

        # sudo zypper install WALinuxAgent

6.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe openen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Bovendien de volgende parameters verwijderen uit de kernel opstarten lijn, indien aanwezig:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Het wordt aanbevolen om het bestand te bewerken '/ enzovoort/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` -parameter voor de volgende handelingen uit:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Belangrijke:** In '/ enzovoort/sudoers', uitschakelen of verwijderen van de volgende regels als ze bestaat:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten.  Dit is meestal de standaardwaarde.

10. Maak geen omwisselen ruimte op de schijf OS.

    De Azure Linux-Agent kunt omwisselen ruimte met behulp van de lokale resource die is gekoppeld aan de VM na het inrichten van Azure automatisch configureren. Houd er rekening mee dat de lokale resource-schijf een *tijdelijke* schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Na de installatie van de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Controleer of dat de Azure Linux-Agent wordt uitgevoerd bij het opstarten:

        # sudo systemctl enable waagent.service

13. Klik op **actie-Afsluiten > omlaag** -in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de SUSE Linux virtuele harde schijf te maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
