<properties
    pageTitle="Een schijf hebt toegevoegd aan een VM Linux | Microsoft Azure"
    description="Lees hoe u een gegevensschijf koppelt aan een Azure virtuele machines waarop Linux en geïnitialiseerd zodat de werkmap gereed voor gebruik is."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Hoe u een gegevensschijf koppelt aan een virtuele Linux Machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie hoe u [een data-schijf met het implementatiemodel resourcemanager hebt toegevoegd](virtual-machines-linux-add-disk.md).

U kunt zowel lege schijven en schijven met gegevens naar uw VMs Azure koppelen. Beide typen schijven zijn VHD-bestanden die zich in een Azure opslag-account bevinden. Als met het toevoegen van een schijf aan een Linux-computer, moet nadat u de schijf koppelen u geïnitialiseerd en opmaken zodat de werkmap gereed voor gebruik is. In dit artikel details koppelen lege schijven zowel schijven al met de gegevens naar uw VMs, evenals vervolgens geïnitialiseerd en opmaken van een nieuwe schijf.

> [AZURE.NOTE]Dit is een goede gewoonte om een of meer afzonderlijke schijven gebruiken voor de opslag van de computer van een virtuele gegevens. Wanneer u een Azure virtuele machines maakt, heeft een schijf besturingssysteem en een tijdelijke schijf. **Gebruik de tijdelijke schijf niet voor de opslag permanente gegevens.** Als de naam aangeeft, biedt deze alleen tijdelijke opslag. Deze optie biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.
> De tijdelijke schijf is gewoonlijk beheerd door de Azure Linux-Agent en automatisch worden gekoppeld aan **/mnt/resource** (of **mnt** op Ubuntu afbeeldingen). Aan de andere kant, een dvd gegevens kan de naam door de kernel Linux ongeveer zo `/dev/sdc`, en u moet partitioneren, opmaken en deze resource koppelen. Zie de [Gebruikershandleiding van Azure Linux Agent] [ Agent] voor meer informatie.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Een nieuwe gegevensschijf in Linux geïnitialiseerd

1. SSH voor uw VM. Voor meer informatie raadpleegt u [aanmelden met een virtuele machine waarop Linux][Logon].

2. Vervolgens moet u het apparaat-id voor de gegevensschijf initialisatie vinden. Er zijn twee manieren doen:

    een) Grep voor SCSI-apparaten in de logboeken, zoals in de volgende opdracht uit:

            $sudo grep SCSI /var/log/messages

    Voor recente Ubuntu onderzoeken, moet u wellicht gebruiken `sudo grep SCSI /var/log/syslog` omdat bij `/var/log/messages` al dan niet standaard mogelijk zijn uitgeschakeld.

    U vindt de id van de laatste gegevensschijf die is toegevoegd in de berichten die worden weergegeven.

    ![De schijf berichten ontvangt](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    OF-BEWERKING

    b) Gebruik de `lsscsi` opdracht om vast te stellen de apparaat-id. `lsscsi`kan worden geïnstalleerd door een `yum install lsscsi` (rood rol gebaseerd onderzoeken) of `apt-get install lsscsi` (Debian gebaseerd onderzoeken). U vindt de schijf die u met de _lun_ of **logische eenheid getal zoekt**. Bijvoorbeeld het _lun_ voor de schijven u bijgevoegd gemakkelijk zichtbaar uit `azure vm disk list <virtual-machine>` als:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Vergelijken van deze gegevens met de uitvoer van `lsscsi` voor hetzelfde voorbeeld VM:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Het laatste nummer in de tupel in elke rij is het _lun_. Zie `man lsscsi` voor meer informatie.

3. Typ bij de prompt de volgende opdracht uit om uw apparaat te maken:

        $sudo fdisk /dev/sdc


4. Wanneer u wordt gevraagd, typt u **n** als u wilt een nieuwe partition maken.


    ![Apparaat maken](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Wanneer u wordt gevraagd, typt u **p** zodat de partition de primaire partition. Typ **1** zodat dit de eerste partition en type Voer accepteer de standaardwaarde voor de cilinder. Klik op sommige systemen, kan deze de standaardwaarden van de eerste en laatste sector, in plaats van de cilinder weergeven. U kunt deze standaardinstellingen te accepteren.


    ![Partition maken](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Typ **p** om de details van de schijf die wordt opgedeeld weer te geven.


    ![Lijstgegevens Schijfopruiming](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Typ **w** om te schrijven van de instellingen voor de schijf.


    ![De wijzigingen schijf schrijven](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. U kunt nu de-bestandssysteem op de nieuwe partition maken. Het getal partition toevoegen aan de apparaat-ID (in het volgende voorbeeld `/dev/sdc1`). Het volgende voorbeeld wordt een partition ext4 op /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Bestandssysteem maken](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] Alleen-lezentoegang ondersteuning SuSE Linux Enterprise 11 systemen alleen voor ext4-bestandssysteem. Voor deze systemen, wordt het aanbevolen om het nieuwe bestandssysteem als ext3 in plaats van ext4.


9. Maak een map wilt koppelen op het nieuwe bestandssysteem, als volgt:

        # sudo mkdir /datadrive


10. Ten slotte kunt u de schijf, als volgt koppelen:

        # sudo mount /dev/sdc1 /datadrive

    De gegevensschijf is nu klaar om te gebruiken als **/datadrive**.
    
    ![Maak de map en de schijf koppelen](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Het nieuwe station toevoegen aan /etc/fstab:

    Om ervoor te zorgen dat het station automatisch opnieuw is gekoppeld na het opnieuw opstarten moet deze worden toegevoegd aan het bestand/enz/fstab. Bovendien is het ten zeerste aanbevolen dat de UUID (universeel unieke id) wordt gebruikt in /etc/fstab om te verwijzen naar de schijf in plaats van alleen de naam van het apparaat (dat wil zeggen /dev/sdc1). Gebruik van de UUID voorkomt de onjuiste schijf wordt gekoppeld aan een bepaalde locatie als het besturingssysteem wordt een fout gedetecteerd tijdens opstarten en eventuele overige gegevensschijven vervolgens deze apparaat-id's wordt toegewezen. Als u wilt de UUID van het nieuwe station hebt gevonden, kunt u het hulpprogramma **blkid** :

        # sudo -i blkid

    De uitvoer ziet er ongeveer als volgt uit:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Onjuist bewerken van het bestand **/etc/fstab** kan resulteren in een opgestart systeem. Als u niet zeker, raadpleegt u de documentatie van de verdeling voor meer informatie over het bewerken van correct dit bestand. Het is ook aanbevolen dat een back-up van het bestand /etc/fstab wordt gemaakt voordat u kunt bewerken.

    Open vervolgens het bestand **/etc/fstab** in een teksteditor:

        # sudo vi /etc/fstab

    In dit voorbeeld gebruiken we de UUID-waarde voor de nieuwe **/dev/sdc1** apparaat dat is gemaakt in de vorige stappen en het koppelpunt **/datadrive**. De volgende regel toevoegen aan het einde van het bestand **/etc/fstab** :

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Of, op op basis van SuSE Linux-systemen wellicht moet u iets anders opmaak gebruiken:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] De `nofail` optie zorgt ervoor dat de VM begint, zelfs als het bestandssysteem beschadigd is of de schijf niet bij het opstarten bestaat. Zonder deze optie wordt tegenkomen u gedrag zoals is beschreven in [Niet kan SSH voor Linux VM vanwege FSTAB fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    U kunt nu testen of het bestandssysteem correct is gekoppeld door ontkoppelen en opnieuw het opstellen van het bestandssysteem, dat wil zeggen volgens het voorbeeld koppelen punt `/datadrive` gemaakt in de eerdere stappen:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Als de `mount` opdracht, treedt een fout, schakelt u het/enzovoort/fstab-bestand voor de juiste syntaxis. Als u meer gegevensstations of partities worden gemaakt, deze invoeren/enzovoort/fstab afzonderlijk ook.

    Controleer het station schrijfbare met behulp van deze opdracht:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Vervolgens een gegevensschijf verwijderen zonder het bewerken van fstab kan leiden tot de VM mislukt om op te starten. Als dit een algemene exemplaar is, de meeste onderzoeken hetzij bieden de `nofail` en/of `nobootwait` fstab opties waarmee een systeem opstart, zelfs als de schijf niet bij opstarten tijd. De documentatie van uw verdeling voor meer informatie over deze parameters.

### <a name="trimunmap-support-for-linux-in-azure"></a>KNIPPEN/UNMAP ondersteuning voor Linux in Azure wordt aangegeven
Sommige kernels Linux ondersteund knippen/UNMAP om niet-gebruikte blokken op de schijf ongedaan te maken. Deze bewerkingen zijn vooral handig in standard opslagruimte op de hoogte brengt Azure die verwijderde pagina's zijn niet langer geldig en kan worden genegeerd. Verwijderen van pagina's kunt kosten opslaan als u grote bestanden maakt en verwijder deze.

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
U kunt meer informatie over het gebruik van uw VM Linux in de volgende artikelen:

- [Aanmelden met een virtuele machine waarop Linux][Logon]

- [Hoe u een schijf vanaf een virtuele Linux machine loskoppelen](virtual-machines-linux-classic-detach-disk.md)

- [Gebruik van de Azure CLI met het model Klassiek-implementatie](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
