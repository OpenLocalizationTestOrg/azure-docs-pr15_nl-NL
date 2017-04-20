<properties
    pageTitle="Maken en uploaden van een rood rol Enterprise Linux VHD voor gebruik in Azure wordt aangegeven"
    description="Leer hoe maken en uploaden van een Azure virtuele harde schijf (VHD) met een rood rol Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Een virtuele rood rol gebaseerde machine voorbereiden voor Azure

In dit artikel leert u hoe u een rood rol Enterprise Linux (RHEL) virtuele machine voorbereiden voor gebruik in Azure wordt aangegeven. Versies van RHEL die worden beschreven in dit artikel zijn 6,7, 7.1 en 7.2. Hypervisors voor voorbereiding van die worden beschreven in dit artikel zijn Hyper-V, Kernel gebaseerde VM (KVM) en VMware. Zie voor meer informatie over geschiktheid vereisten voor deelname aan van de rol van de rood Cloudtoegang-programma's [van de rol van de rood Cloudtoegang website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) - en [RHEL uitgevoerd op Azure](https://access.redhat.com/articles/1989673).

[Een virtuele RHEL 6,7 machine van Hyper-V Manager voorbereiden](#rhel67hyperv)

[Een virtuele RHEL 7.1/7.2 machine van Hyper-V Manager voorbereiden](#rhel7xhyperv)

[Een virtuele RHEL 6,7 machine uit KVM voorbereiden](#rhel67kvm)

[Een virtuele RHEL 7.1/7.2 machine uit KVM voorbereiden](#rhel7xkvm)

[Een virtuele RHEL 6,7 machine uit VMware voorbereiden](#rhel67vmware)

[Een virtuele RHEL 7.1/7.2 machine uit VMware voorbereiden](#rhel7xvmware)

[Een RHEL 7.1/7.2 virtuele machine uit een bestand kickstart voorbereiden](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Een virtuele rood rol gebaseerde machine van Hyper-V Manager voorbereiden
### <a name="prerequisites"></a>Vereisten voor
In deze sectie wordt ervan uitgegaan dat u al hebt geïnstalleerd de afbeelding van een RHEL (vanuit een ISO-bestand dat u hebt aangeschaft bij de website van de rol van de rood) aan een virtuele harde schijf (VHD). Zie [installeren van de functie Hyper-V en een virtuele Machine configureren](http://technet.microsoft.com/library/hh846766.aspx)voor meer informatie over het gebruik van Hyper-V-beheer voor het installeren van een afbeelding van het besturingssysteem.

**Installatieopmerkingen RHEL**

- Raadpleeg ook [Algemene Linux Installatieopmerkingen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) voor meer tips over het voorbereiden van Linux van Azure.

- De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. U kunt de schijf converteren naar VHD opmaken met behulp van Hyper-V Manager of de **converteren vhd** PowerShell-cmdlet.

- VHD's moeten worden gemaakt zoals 'vaste'--dynamische VHD's worden niet ondersteund.

- Wanneer u het systeem Linux installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) te gebruiken. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. LVM of [RAID](virtual-machines-linux-configure-raid.md) mag op gegevensschijven worden gebruikt als de voorkeur.

- Configureer een partition omwisselen niet op de schijf OS. U kunt de Linux-agent als u wilt maken van een bestand uitwisselen op de tijdelijke resource-schijf configureren. Meer informatie over dit is beschikbaar in de onderstaande stappen.

- Alle de VHD's moet puntgrootten die een veelvoud van 1 MB hebben.

- Wanneer u met **qemu-img** schijf afbeeldingen converteren naar VHD-indeling, houd er rekening mee dat er een bekende fout is opgetreden in qemu-img versies 2.2.1 of hoger. Deze fout levert een onjuist opgemaakte VHD. Het probleem is bedoeld om te worden opgelost in een nieuwe versie van qemu-img. Nu is raadzaam dat u qemu-img versie 2.2.0 gebruikt of eerder.

### <a id="rhel67hyperv"> </a>RHEL 6,7 virtuele machines van Hyper-V Manager voorbereiden###


1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

3.  NetworkManager verwijderen door de volgende opdracht uit te voeren:

        # sudo rpm -e --nodeps NetworkManager

    Houd er rekening mee dat als het pakket niet al is geïnstalleerd, deze opdracht met een foutbericht wordt weergegeven mislukt. Dit is normaal.

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

6.  Udev regels om te vermijden statische regels voor de Ethernet-interface genereren verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

8.  Registreer uw abonnement rood rol om in te schakelen van de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe open `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA uitgeschakeld vanwege een fout in de kernelversie die wordt gebruikt door RHEL 6.

    Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.

    De optie crashkernel kan worden geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde. /Etc/ssh/sshd_config als u wilt opnemen van de volgende regel wijzigen:

        ClientAliveInterval 180

12. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Houd er rekening mee dat het NetworkManager installatie van het pakket WALinuxAgent worden verwijderd en NetworkManager-gnome-pakketten als ze volgens de beschrijving niet al zijn verwijderd in stap 2.

13. Maak geen omwisselen ruimte op de schijf OS.
De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Het abonnement (indien nodig) unregister door de volgende opdracht uit te voeren:

        # sudo subscription-manager unregister

15. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klik op **actie > afgesloten** in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.
 

### <a id="rhel7xhyperv"> </a>RHEL 7.1/7.2 virtuele machines van Hyper-V Manager voorbereiden###

1.  Selecteer de virtuele machine in Hyper-V-beheer.

2.  Klik op **verbinding maken met** een consolevenster voor de virtuele machine te openen.

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

5.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

6.  Registreer uw abonnement rood rol om in te schakelen van de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe open `/etc/default/grub` in een teksteditor en de parameter **GRUB_CMDLINE_LINUX** bewerken. Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.
    De optie crashkernel kan worden geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

8.  Nadat u klaar bent met bewerken `/etc/default/grub`, voer de volgende opdracht uit om de configuratie wormgaten opnieuw te maken:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde. Wijzigen `/etc/ssh/sshd_config` om op te nemen van de volgende regel:

        ClientAliveInterval 180

10. Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Maak geen omwisselen ruimte op de schijf OS. De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap) wijzigen van de volgende parameters in `/etc/waagent.conf` correct:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Als u het abonnement unregister wilt, voert u de volgende opdracht uit:

        # sudo subscription-manager unregister

14. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klik op **actie > afgesloten** in Hyper-V-beheer. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Een virtuele rood rol gebaseerde machine uit KVM voorbereiden

### <a id="rhel67kvm"> </a>RHEL 6,7 virtuele machines uit KVM voorbereiden###


1.  Download de afbeelding KVM van RHEL 6,7 vanuit de website van de rol van de rood.

2.  Een wachtwoord hoofdmap instellen.

    Een versleutelde wachtwoord genereren en de uitvoer van de opdracht kopiëren:

        # openssl passwd -1 changeme

    Een wachtwoord hoofdmap met guestfish instellen:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Wijzigen van het tweede veld van de gebruiker van de hoofdsite van "!" naar het versleutelde wachtwoord.

3.  Een virtuele machine in KVM maken van de afbeelding qcow2, het schijftype ingesteld op **qcow2**en het virtuele netwerk interface Apparaatmodel instelt op **virtio**. Vervolgens de virtuele machine start en meld u aan als de hoofdmap.

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

6.  De udev regels om te voorkomen dat u genereert statische regels voor de Ethernet-interface verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # chkconfig network on

8.  Registreer uw abonnement rood rol om in te schakelen van de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe open `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA uitgeschakeld vanwege een fout in de kernelversie die wordt gebruikt door RHEL 6.

    Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.
    De optie crashkernel is mogelijk geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

10. Hyper-V modules in initramfs toevoegen:  

    Bewerken `/etc/dracut.conf` en inhoud hieraan toevoegen: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Bouw die tabellen opnieuw initramfs: # dracut – f - v

11. Cloud initialisatie verwijderen:

        # yum remove cloud-init

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten:

        # chkconfig sshd on

    Wijzig /etc/ssh/sshd_config als u wilt opnemen van de volgende regels:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Opnieuw starten sshd:

        # service sshd restart

13. Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap), de volgende parameters in **/etc/waagent.conf** correct wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Het abonnement (indien nodig) unregister door de volgende opdracht uit te voeren:

        # subscription-manager unregister

17. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. De VM in KVM afgesloten.

19. De qcow2-afbeelding converteren naar VHD-indeling.
    Eerst de afbeelding converteren naar onbewerkte opmaken:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd op 1 MB. Afronden, de grootte om uit te lijnen met 1 MB: # MB = $((1024*1024)) # grootte = $(qemu-img info -f onbewerkte--uitvoer json "rhel-6.7.raw" | \ gawk ' overeenkomen met ($0, / "virtuele-grootte": ([0-9] +), /, val) {afdrukken val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    De onbewerkte schijf converteren naar een vaste grootte en VHD:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>RHEL 7.1/7.2 virtuele machines uit KVM voorbereiden###


1.  Download de afbeelding KVM van RHEL 7.1 (of 7.2) via de website rood rol. We gebruiken RHEL 7.1 als in het voorbeeld hier.

2.  Een wachtwoord hoofdmap instellen.

    Een versleutelde wachtwoord genereren en de uitvoer van de opdracht kopiëren:

        # openssl passwd -1 changeme

    Een wachtwoord hoofdmap met guestfish instellen.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Wijzigen van het tweede veld van de gebruiker van de hoofdsite van "!" naar het versleutelde wachtwoord.

3.  Een virtuele machine in KVM maken van de afbeelding qcow2, het schijftype ingesteld op **qcow2**en het virtuele netwerk interface Apparaatmodel instelt op **virtio**. Vervolgens de virtuele machine start en meld u aan als de hoofdmap.

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

6.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # chkconfig network on

7.  Registreer uw abonnement rood rol zodat de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe open `/etc/default/grub` in een teksteditor en de parameter **GRUB_CMDLINE_LINUX** bewerken. Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.
    De optie crashkernel kan worden geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

9.  Nadat u klaar bent met bewerken `/etc/default/grub`, voer de volgende opdracht uit om de configuratie wormgaten opnieuw te maken:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Hyper-V modules in initramfs toevoegen:

    Bewerken `/etc/dracut.conf` en inhoud hieraan toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Opnieuw opbouwen initramfs:

        # dracut –f -v

11. Cloud initialisatie verwijderen:

        # yum remove cloud-init

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten:

        # systemctl enable sshd

    Wijzig /etc/ssh/sshd_config als u wilt opnemen van de volgende regels:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Opnieuw starten sshd:

        systemctl restart sshd

13. Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # yum install WALinuxAgent

    De service waagent inschakelen:

        # systemctl enable waagent.service

15. Maak geen omwisselen ruimte op de schijf OS. De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap) wijzigen van de volgende parameters in `/etc/waagent.conf` correct:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Het abonnement (indien nodig) unregister door de volgende opdracht uit te voeren:

        # subscription-manager unregister

17. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. De virtuele machine in KVM afgesloten.

19. De qcow2-afbeelding converteren naar VHD-indeling.

    Eerst de afbeelding converteren naar onbewerkte opmaken:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd op 1 MB. Afronden, de tekengrootte met 1 MB uitlijnen:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    De onbewerkte schijf converteren naar een vaste grootte en VHD:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Een virtuele rood rol gebaseerde machine uit VMware voorbereiden
### <a name="prerequisites"></a>Vereisten voor
In deze sectie wordt ervan uitgegaan dat u al een RHEL virtuele machine in VMware hebt geïnstalleerd. Zie voor meer informatie over het installeren van een besturingssysteem in VMware [Installatiehandleiding voor VMware Gast-besturingssysteem](http://partnerweb.vmware.com/GOSIG/home.html).

- Wanneer u het besturingssysteem Linux installeert, wordt u aangeraden standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties) te gebruiken. Dit vermijdt LVM naamconflicten met gekloonde VMs, met name als een OS schijf ooit worden gekoppeld aan een andere VM moet voor probleemoplossing. LVM of RAID kan worden gebruikt op gegevensschijven indien gewenst.

- Configureer een partition omwisselen niet op de schijf OS. U kunt de Linux-agent als u wilt maken van een bestand uitwisselen op de tijdelijke resource-schijf configureren. U vindt meer informatie over dit in de onderstaande stappen.

- Wanneer u de virtuele harde schijf hebt gemaakt, selecteert u **virtuele schijf van de Store als één bestand**.



### <a id="rhel67vmware"> </a>RHEL 6,7 virtuele machines van VMware voorbereiden###

1.  NetworkManager verwijderen door de volgende opdracht uit te voeren:

         # sudo rpm -e --nodeps NetworkManager

    Houd er rekening mee dat als het pakket niet al is geïnstalleerd, deze opdracht met een foutbericht wordt weergegeven mislukt. Dit is normaal.

2.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Maken van een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  De udev regels om te voorkomen dat u genereert statische regels voor de Ethernet-interface verplaatsen (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Microsoft Azure of Hyper-V: klonen

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

6.  Registreer uw abonnement rood rol om in te schakelen van de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe openen "/ boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de standaard-kernel de volgende parameters bevat:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Hiermee wordt NUMA uitgeschakeld vanwege een fout in de kernelversie die wordt gebruikt door RHEL 6.
    Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.
    De optie crashkernel kan worden geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

9.  Hyper-V modules in initramfs toevoegen:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde. Wijzigen `/etc/ssh/sshd_config` om op te nemen van de volgende regel:

        ClientAliveInterval 180

11. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Maak geen omwisselen ruimte op de schijf OS:

    De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap) wijzigen van de volgende parameters in `/etc/waagent.conf` correct:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Het abonnement (indien nodig) unregister door de volgende opdracht uit te voeren:

        # sudo subscription-manager unregister

14. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. De VM afsluiten en de VMDK-bestand converteren naar een VHD-bestand.

    Eerst de afbeelding converteren naar onbewerkte opmaken:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd op 1 MB. Afronden, de tekengrootte met 1 MB uitlijnen:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    De onbewerkte schijf converteren naar een vaste grootte en VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>RHEL 7.1/7.2 virtuele machines van VMware voorbereiden###

1.  Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Maken van een bestand met de naam **ifcfg-eth0** in de /etc/sysconfig/network-scripts / directory met de volgende tekst:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Zorg ervoor dat de netwerkservice bij het opstarten wordt gestart door de volgende opdracht uit te voeren:

        # sudo chkconfig network on

4.  Registreer uw abonnement rood rol om in te schakelen van de installatie van pakketten uit de bibliotheek RHEL door de volgende opdracht uit te voeren:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  De regel kernel opstarten in uw configuratie wormgaten om op te nemen extra kernel parameters voor Azure wijzigen. Klik hiertoe open `/etc/default/grub` in een teksteditor en de parameter **GRUB_CMDLINE_LINUX** bewerken. Bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Dit wordt ook Zorg ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, waarin Azure kunt helpen ondersteuning met foutopsporing problemen. Naast de bovenstaande actie, raden we u aan dat u de volgende parameters verwijderen:

        rhgb quiet crashkernel=auto

    Grafische en stille opstarten komen niet in een cloud-omgeving die we de locatie waar de logboeken naar de seriële poort wordt verzonden.
    De optie crashkernel kan worden geconfigureerd desgewenst links, maar dat deze parameter wordt minder beschikbare geheugen in VM door 128 MB of meer notitie. Dit is mogelijk problematisch op kleinere VM.

6.  Nadat u klaar bent met bewerken `/etc/default/grub`, voer de volgende opdracht uit om de configuratie wormgaten opnieuw te maken:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Hyper-V modules in initramfs toevoegen:

    Bewerken `/etc/dracut.conf`, inhoud toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Opnieuw opbouwen initramfs:

        # dracut –f -v

8.  Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te beginnen bij het opstarten. Dit is meestal de standaardwaarde. Wijzigen `/etc/ssh/sshd_config` om op te nemen van de volgende regel:

        ClientAliveInterval 180

9.  Het pakket WALinuxAgent `WALinuxAgent-<version>` heeft zijn verplaatst naar de bibliotheek rood rol-extra's. De bibliotheek extra's inschakelen door de volgende opdracht uit te voeren:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installeer de Azure Linux-Agent door de volgende opdracht uit te voeren:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Maak geen omwisselen ruimte op de schijf OS. De Azure Linux-Agent kunt omwisselen ruimte automatisch configureren met behulp van de lokale resource-schijf die is gekoppeld aan de VM nadat de VM op Azure is ingericht. Houd er rekening mee dat de lokale resource-schijf een tijdelijke schijf is en mogelijk worden leeggemaakt wanneer de VM wordt opgeheven. Nadat u de Azure Linux-Agent (Zie de vorige stap) wijzigen van de volgende parameters in `/etc/waagent.conf` correct:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Als u het abonnement unregister wilt, voert u de volgende opdracht uit:

        # sudo subscription-manager unregister

13. Voer de volgende opdrachten kunnen de virtuele machine deprovision en voorbereiden voor het inrichten van op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Sluit de VM af en het VMDK-bestand converteren naar VHD-indeling.

    Eerst de afbeelding converteren naar onbewerkte opmaken:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Zorg ervoor dat de grootte van de onbewerkte afbeelding wordt uitgelijnd op 1 MB. Afronden, de tekengrootte met 1 MB uitlijnen:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    De onbewerkte schijf converteren naar een vaste grootte en VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Een virtuele rood rol gebaseerde machine van een ISO met behulp van een bestand kickstart automatisch voorbereiden


### <a id="rhel7xkickstart"> </a>RHEL 7.1/7.2 virtuele machines uit een bestand kickstart voorbereiden###


1.  Een bestand kickstart maken met de onderstaande inhoud, en sla het bestand. Zie voor meer informatie over kickstart installatie, de [Installatiehandleiding voor Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Bewaar het bestand kickstart op een locatie die toegankelijk is vanuit het systeem installatie.

3.  Maak een nieuwe VM in Hyper-V beheer. Klik op de pagina **verbinding maken met virtuele harde schijf** Selecteer **bijvoegen later een virtuele harde schijf**en voert u de Wizard van de nieuwe virtuele Machine.

4.  Open de VM-instellingen:

    een.  Een nieuwe virtuele harde schijf toevoegen aan de VM. Zorg ervoor dat **VHD-indeling** en **Vaste grootte**.

    b.  De installatie ISO toevoegen aan het DVD-station.

    c.  Stel het BIOS op opstarten vanaf CD.

5.  Start de VM. Wanneer de installatiehandleiding wordt weergegeven, drukt u op **tabblad** voor het configureren van de opties voor opstarten.

6.  ENTER `inst.ks=<the location of the kickstart file>` aan het einde van de opties voor opstarten en druk op **Enter**.

7.  Wacht totdat de installatie om te voltooien. Als deze voltooid, wordt de VM automatisch afsluiten. Uw VHD Linux is nu klaar om te worden geüpload naar Azure.

## <a name="known-issues"></a>Bekende problemen
Er zijn bekende problemen wanneer u RHEL 7.1 in Hyper-V en Azure gebruikt.

### <a name="disk-io-freeze"></a>Schijf i/o-blokkeren

Dit probleem kan zich voordoen tijdens veelgebruikte opslag schijf i/o-activiteiten met RHEL 7.1 in Hyper-V en Azure.   

Reproduceren rente:

Dit probleem wordt onderbroken. Echter het zich daadwerkelijk voordoet vaak meer tijdens vaker schijf i/o-bewerkingen in Hyper-V en Azure.   


[AZURE.NOTE] Al is deze bekend probleem verholpen in rood rol. Als u wilt de bijbehorende oplossingen hebt geïnstalleerd, voert u de volgende opdracht uit:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Het stuurprogramma Hyper-V kan niet worden opgenomen in de eerste RAM-schijf bij gebruik van een niet-Hyper-V hypervisor

In sommige gevallen kunnen Linux installatieprogramma's niet bevatten de stuurprogramma's voor Hyper-V in de eerste RAM-schijf (initrd of initramfs) tenzij wordt vastgesteld dat deze wordt uitgevoerd in een omgeving Hyper-V.

Wanneer u een ander virtualization-systeem (dat wil zeggen Virtualbox, Xen, enzovoort) voor het voorbereiden van uw afbeelding Linux gebruikt, moet u mogelijk opnieuw opbouwen initrd om ervoor te zorgen dat ten minste de kernelmodules hv_vmbus en hv_storvsc zijn beschikbaar op de eerste RAM-schijf. Dit is een bekend probleem ten minste op systemen op basis van de boven rood rol-verdeling.

U lost dit probleem, moet u het toevoegen van Hyper-V modules in initramfs en opnieuw opbouwen:

Bewerken `/etc/dracut.conf` en inhoud hieraan toevoegen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Opnieuw opbouwen initramfs:

        # dracut –f -v

Zie de informatie over het [opnieuw initramfs maken](https://access.redhat.com/solutions/1958)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Red rol Enterprise Linux virtuele harde schijf te maken van nieuwe virtuele machines in Azure wordt aangegeven. Als dit de eerste keer dat u het bestand VHD naar Azure uploadt is, raadpleegt u stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Voor meer informatie over de hypervisors die zijn gecertificeerd voor Red rol Enterprise Linux uitvoeren, raadpleegt u [de website van de rol van rood](https://access.redhat.com/certified-hypervisors).
