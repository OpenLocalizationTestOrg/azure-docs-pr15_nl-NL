<properties
    pageTitle="Een schijf toevoegen aan Linux VM | Microsoft Azure"
    description="Leer hoe u een permanente schijf toevoegen aan uw VM Linux"
    keywords="VM Linux, resource-schijf toevoegen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Een schijf toevoegen aan een VM Linux

In dit artikel leest hoe u koppelt een permanente schijf aan uw VM zodat u kunt uw gegevens - zelfs als uw VM is door de service vanwege onderhoud of het formaat wijzigen. Als u wilt een schijf toevoegt, moet u [De CLI Azure](../xplat-cli-install.md) geconfigureerd in de modus resourcemanager (`azure config mode arm`).  

## <a name="quick-commands"></a>Snelle opdrachten

In de volgende voorbeelden van de opdracht, vervangt u de waarden tussen &lt; en &gt; met de waarden uit uw eigen omgeving.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Een schijf hebt toegevoegd

Een nieuwe schijf koppelen gaat snel. Type `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` maken en een nieuwe GB-schijf voor uw VM toegevoegd. Als u een opslag-account niet expliciet identificeren, bevindt een schijf die u maakt zich in dezelfde opslag account waarin de schijf OS zich bevindt.  Deze ziet er ongeveer als volgt te werk:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Uitvoer

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Verbinding maken met de Linux VM de nieuwe schijf koppelen

> [AZURE.NOTE] In dit onderwerp maakt verbinding met een VM met gebruikersnamen en wachtwoorden opnieuw instellen. Lees [hoe u gebruik SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md)om te communiceren met uw VM gebruiken voor paren van openbare en persoonlijke sleutels. Kunt u de connectiviteit **SSH** van VMs die zijn gemaakt met de `azure vm quick-create` command met behulp van de `azure vm reset-access` opdracht **SSH** access volledig opnieuw, toevoegen of verwijderen van gebruikers of openbare sleutel bestanden beveiligde toegang toevoegen.

U moet SSH in uw VM Azure naar partition, opmaken en uw nieuwe schijf koppelen zodat uw VM Linux deze kunt gebruiken. Als u niet bekend bent met verbinding maken met **ssh**, de opdracht heeft de vorm `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, en ziet er als volgt uit:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Uitvoer

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Nu dat u met uw VM verbonden bent, kunt u bent een schijf hebt toegevoegd.  Zoek eerst de schijf met `dmesg | grep SCSI` (de methode die u kunt uw nieuwe schijf ontdekken kan verschillen). In dit geval er ongeveer als volgt:

```bash
dmesg | grep SCSI
```

Uitvoer

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

en wat in dit onderwerp, de `sdc` schijf is de fase die we horen. Nu de schijf met partitioneren `sudo fdisk /dev/sdc` --ervan uitgaande dat in de zaak de schijf is `sdc`, kunt u een primaire schijf op partition 1 en accepteer de andere standaardinstellingen.

```bash
sudo fdisk /dev/sdc
```

Uitvoer

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

De partition maken door te typen `p` bij de opdrachtprompt:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

En een bestandssysteem naar de partition schrijven met de opdracht **mkfs** , waarbij het type bestandssysteem en naam van het apparaat opgeeft. In dit onderwerp, gebruiken we `ext4` en `/dev/sdc1` dan hierboven:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Uitvoer

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Nu we een map als u wilt koppelen het bestandssysteem via maken `mkdir`:

```bash
sudo mkdir /datadrive
```

En u koppelen het gebruik van de map `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

De gegevensschijf is nu klaar voor gebruik als `/datadrive`.

```bash
ls
```

Uitvoer

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Om ervoor te zorgen dat het station automatisch opnieuw is gekoppeld na het opnieuw opstarten moet deze worden toegevoegd aan het bestand/enz/fstab. Bovendien is het raadzaam dat de UUID (universeel unieke id) te verwijzen naar de schijf in plaats van alleen de naam van het apparaat wordt gebruikt in/enzovoort/fstab (zoals `/dev/sdc1`). Als het besturingssysteem wordt een fout gedetecteerd tijdens het opstarten, voorkomt gebruikt de UUID de onjuiste schijf wordt gekoppeld aan een bepaalde locatie. Resterende hoeveelheid gegevensschijven wordt vervolgens toegewezen die dezelfde apparaat-id's. Gebruik het hulpprogramma **blkid** de UUID van het nieuwe station vindt:

```bash
sudo -i blkid
```

De uitvoer ziet er ongeveer als volgt uit:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Onjuist bewerken van het bestand **/etc/fstab** kan resulteren in een opgestart systeem. Als u niet zeker, raadpleegt u de documentatie van de verdeling voor meer informatie over het bewerken van correct dit bestand. Het is ook aanbevolen dat een back-up van het bestand /etc/fstab wordt gemaakt voordat u kunt bewerken.

Open vervolgens het bestand **/etc/fstab** in een teksteditor:

```bash
sudo vi /etc/fstab
```

In dit voorbeeld gebruiken we de UUID-waarde voor de nieuwe **/dev/sdc1** apparaat dat is gemaakt in de vorige stappen en het koppelpunt **/datadrive**. De volgende regel toevoegen aan het einde van het bestand **/etc/fstab** :

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Later een gegevensschijf verwijderen zonder het bewerken van fstab kan leiden tot de VM mislukt om op te starten. De meeste onderzoeken bieden hetzij de `nofail` en/of `nobootwait` fstab opties. Deze opties kunt een systeem opstart, zelfs als de schijf niet bij het opstarten. De documentatie van uw verdeling voor meer informatie over deze parameters.

>[AZURE.NOTE] De optie **nofail** zorgt ervoor dat de VM begint, zelfs als het bestandssysteem beschadigd is of de schijf niet bij het opstarten bestaat. Deze optie niet, kunnen optreden gedrag zoals is beschreven in [Niet kan SSH voor Linux VM vanwege FSTAB fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>KNIPPEN/UNMAP ondersteuning voor Linux in Azure wordt aangegeven
Sommige kernels Linux ondersteund knippen/UNMAP om niet-gebruikte blokken op de schijf ongedaan te maken. Dit is vooral handig in standard opslagruimte op de hoogte brengt Azure die verwijderde pagina's zijn niet langer geldig en kan worden genegeerd. Dit kunt kosten opslaan als u grote bestanden maakt en verwijder deze.

Er zijn twee manieren om in te schakelen knippen ondersteuning in uw VM Linux. Zoals u gewend bent, raadpleegt u uw verdeling voor de aanbevolen werkwijze:

- Gebruik de `discard` koppelen optie in `/etc/fstab`, bijvoorbeeld:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of voeg deze toe aan uw crontab regelmatig wordt uitgevoerd:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Problemen oplossen
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen

- Onthoud dat de nieuwe schijf is niet beschikbaar voor de VM als deze opnieuw is opgestart tenzij u deze informatie aan uw bestand [fstab](http://en.wikipedia.org/wiki/Fstab) schrijven.
- Controleer dat uw VM Linux correct is geconfigureerd, zodat de aanbevelingen [optimaliseren de prestaties van uw Linux-computer](virtual-machines-linux-optimization.md) .
- Uw opslagcapaciteit uitvouwen met het toevoegen van extra schijven en [RAID configureren](virtual-machines-linux-configure-raid.md) voor extra prestaties.
