<properties 
    pageTitle="LVM configureren op een virtuele machine waarop Linux | Microsoft Azure" 
    description="Informatie over het configureren van LVM op Linux in Azure wordt aangegeven." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM configureren op een VM Linux in Azure wordt aangegeven

In dit document wordt uitgelegd hoe het configureren van logische Volume Manager (LVM) in uw Azure virtuele machines. Het is mogelijk gecombineerd LVM configureren op een schijf die is gekoppeld aan de virtuele machine, al dan niet standaard hebben de meeste cloud afbeeldingen geen LVM geconfigureerd op de schijf OS. Dit is om te voorkomen dat problemen met dubbele volume groepen als de schijf OS wordt ooit gekoppeld aan een andere VM van de dezelfde verdeling te typen, dat wil zeggen tijdens een herstelscenario. Het verdient daarom alleen voor LVM op de gegevensschijven gebruiken.


## <a name="linear-vs-striped-logical-volumes"></a>Lineaire versus gestreepte logische volumes

LVM kan worden gebruikt om een aantal fysieke schijven combineren in één opslagvolume. Standaard LVM wordt meestal lineaire logische volumes maken, wat betekent dat de fysieke opslag is samengevoegd. In dit geval wordt alleen-lezen/schrijven bewerkingen meestal alleen worden verzonden naar één schijf. We kunnen daarentegen ook gestreepte logische volumes waar lezen en schrijven zijn verdeeld over meerdere schijven die zijn opgenomen in de groep volume (dat wil zeggen vergelijkbaar met RAID 0) maken. Prestatieoverwegingen er waarschijnlijk wilt u stripe logische begint zodat lezen en schrijven gebruikmaken van alle gekoppelde gegevensschijven.

In dit document wordt beschreven hoe verschillende gegevensschijven combineren in een groep één volume en vervolgens een gestreepte logisch volume te maken. De onderstaande stappen worden enigszins generalized als u wilt werken met de meeste onderzoeken. In de meeste gevallen zijn de hulpprogramma's en werkstromen voor het beheren van LVM op Azure niet fundamenteel anders dan andere omgevingen. Zoals u gewend bent, ook Raadpleeg de leverancier van uw Linux voor documentatie en aanbevolen procedures voor het gebruik van LVM met uw bepaalde verdeling.


## <a name="attaching-data-disks"></a>Gegevensschijven koppelen
Een wilt meestal beginnen met twee of meer lege gegevensschijven bij gebruik van LVM. Op basis van uw behoeften IO, kunt u kiezen om te koppelen schijven die zijn opgeslagen in onze standaard opslagcapaciteit, met maximaal 500 IO/ps per schijf of de opslag van onze Premium met maximaal 5000 IO/ps per schijf. In dit artikel gaat niet uitgebreide informatie over het inrichten en gegevensschijven als bijlage toevoegen aan een virtuele Linux machine. Raadpleeg de Microsoft Azure-artikel [een schijf hebt toegevoegd](virtual-machines-linux-add-disk.md) voor uitgebreide instructies over het toevoegen van een gegevensschijf lege aan een virtuele Linux machine op Azure.

## <a name="install-the-lvm-utilities"></a>De LVM hulpprogramma's installeren

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS en Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 en openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Klik op SLES11 moet u ook /etc/sysconfig/lvm bewerken en stel `LVM_ACTIVATED_ON_DISCOVERED` ' inschakelen ':

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>LVM configureren
In deze handleiding wordt ervan uitgegaan u drie gegevensschijven, waarin wordt verwezen naar als hebt bijgevoegd `/dev/sdc`, `/dev/sdd` en `/dev/sde`. Houd er rekening mee dat deze mogelijk niet altijd hetzelfde padnamen in uw VM. U kunt uitvoeren '`sudo fdisk -l`' of een vergelijkbare opdracht voor een overzicht van de beschikbare schijven.

1. De fysieke hoeveelheden voorbereiden:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Maak een volume-groep. In dit voorbeeld belt we het volume groep "gegevens-vg01':

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Maak de logische volume (s). De opdracht onder we maakt u één logische volume "gegevens-lv01" om de hele volume-groep's beslaan, maar houd er rekening mee dat het is ook mogelijk gecombineerd meerdere logische volumes maken in de groep volume genoemd.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Het volume van het logische opmaken

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Met SLES11 gebruik "-t ext3" in plaats van ext4. SLES11 ondersteunt alleen-lezen toegang tot ext4 filesystems.


## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem toevoegen aan /etc/fstab

**Let op:** Onjuist bewerken van het bestand /etc/fstab kan resulteren in een opgestart systeem. Als u niet zeker, raadpleeg dan van de verdeling documentatie voor informatie over hoe u dit bestand correct te bewerken. Het is ook aanbevolen dat een back-up van het bestand /etc/fstab wordt gemaakt voordat u kunt bewerken.

1. Maak de gewenste koppeling komma voor uw nieuwe bestandssysteem, bijvoorbeeld:

        # sudo mkdir /data


2. Zoek het logische Volumepad

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Open /etc/fstab in een teksteditor en een fragment voor de nieuwe bestandssysteem worden gemaakt, bijvoorbeeld toevoegen:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Vervolgens opslaan en sluiten /etc/fstab.


4. Test of het fragment/enzovoort/fstab juist is:

        # sudo mount -a

    Controleer de syntaxis van de in het bestand/enz/fstab als deze opdracht in een foutbericht wordt weergegeven resulteert.

    Volgende start de `mount` opdracht om te zorgen dat het bestandssysteem is gekoppeld:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Optioneel) Failsafe opstarten parameters in /etc/fstab

    Veel onderzoeken bevat de `nobootwait` of `nofail` parameters die kunnen worden toegevoegd aan het bestand/enz/fstab koppelen. Deze parameters toestaan voor fouten bij het koppelen van een bepaald bestandssysteem en laat het systeem Linux gaat u verder met het starten zelfs als deze is niet correct koppelen het bestandssysteem RAID. Zie de documentatie van uw verdeling voor meer informatie over deze parameters.

    Voorbeeld (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
