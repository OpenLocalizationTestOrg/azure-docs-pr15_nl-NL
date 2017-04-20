<properties
    pageTitle="MySQL-optimaliseren op Linux VMs | Microsoft Azure"
    description="Informatie over het optimaliseren van MySQL uitgevoerd op een Azure virtuele machine (VM) waarop Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>MySQL-prestaties op Azure Linux VMs optimaliseren

Zijn er veel factoren die van invloed zijn op MySQL-prestaties op Azure, zowel in virtuele selectie en software hardwareconfiguratie. In dit artikel ligt de nadruk op optimaliseren prestaties tot en met opslag, systeem en configuraties van de database.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Gebruik van RAID op een Azure virtuele machines
Opslag is de belangrijkste factor die van invloed op de prestaties van de database in de cloud-omgevingen.  Vergeleken met één schijf, kan RAID sneller toegang tot via gelijktijdigheid bieden.  Raadpleeg [Standaard RAID machtigingsniveaus](http://en.wikipedia.org/wiki/Standard_RAID_levels) voor meer details.   

Schijfdoorvoer I/O en i/o-antwoord tijd in Azure kunnen aanzienlijk zijn verbeterd via RAID. Onze tests testomgeving schijf I/O doorvoer kunt verdubbeld en i/o-antwoord tijd kan worden verminderd door halverwege gemiddeld wanneer het aantal RAID-schijven wordt verdubbeld weergeven (van 2 en 4, 4 tot en met 8, enz.). Zie [bijlage A](#AppendixA) voor meer informatie.  

Naast de schijf I/O verbetert MySQL-prestaties wanneer u het RAID-niveau verhogen.  Zie [Bijlage B](#AppendixB) voor meer informatie.  

U kunt ook rekening houden met de grootte van het segment. In het algemeen wanneer u een groter deel hebt, krijgt u lagere belasting, met name voor grote schrijven. Wanneer het segment te groot is, kan deze extra belasting toevoegen en u niet kunt profiteren van de RAID. De huidige standaardgrootte is 512KB, bewezen wordt moeten optimaal voor de meeste algemene productieomgevingen. Zie [Bijlage C](#AppendixC) voor meer informatie.   

Houd er rekening mee dat er limieten op hoeveel schijven u voor andere virtuele machine typen kunt toevoegen. Deze limieten zijn gedetailleerde in [virtuele Machine en Cloud Service maten voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). U moet 4 bijgevoegde gegevensschijven om te volgen in het voorbeeld RAID in dit artikel, hoewel u ervoor kiezen kunt om in te stellen RAID met minder schijven.  

In dit artikel wordt ervan uitgegaan dat u al een Linux virtuele machine hebt gemaakt en hebt MYSQL geïnstalleerd en geconfigureerd. Raadpleeg het installeren van MySQL op Azure voor meer informatie over het aan de slag.  

###<a name="setting-up-raid-on-azure"></a>Bij het instellen van RAID op Azure
De volgende stappen hoe RAID op Azure maken met behulp van de Azure klassieke portal. U kunt ook instellen RAID met Windows PowerShell-scripts.
In dit voorbeeld wordt we RAID 0 configureren met 4 schijven.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Stap 1: Een gegevensschijf toevoegen aan uw VM  

Klik op de pagina virtuele Machines van de Azure klassieke portal op de virtuele machine waarnaar u wilt toevoegen van een gegevensschijf. In dit voorbeeld is de virtuele machine mysqlnode1.  

![][1]

Klik op de pagina voor de virtuele machine op **Dashboard**.  

![][2]


Klik in de taakbalk op **bijvoegen**.

![][3]

En klik op **de lege schijf bijvoegen**.  

![][4]

Voor gegevensschijven, moet de **Host Cache voorkeur** worden ingesteld op **geen**.  

Hiermee wordt een lege schijf in uw VM toegevoegd. Herhaal deze stap drie keer, zodat u beschikt over 4 gegevens schijfstations voor RAID.  

U kunt de toegevoegde stations in de virtuele machine zien door te zoeken bij het bericht kernellogboek. Bijvoorbeeld, om dit op Ubuntu ziet, gebruikt u de volgende opdracht uit:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Stap 2: RAID maken met de extra schijven
In dit artikel voor gedetailleerde RAID instellingsstappen als volgt:  

[Software RAID op Linux configureren](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Als u het bestandssysteem XFS gebruikt, volgt u de onderstaande stappen nadat u RAID hebt gemaakt.

Als u wilt installeren XFS op Debian, Ubuntu of Linux munt, gebruikt u de volgende opdracht uit:  

    apt-get -y install xfsprogs  

Gebruik de volgende opdracht uit om te kunnen installeren XFS op Fedora, CentOS of RHEL:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Stap 3: Een nieuwe opslag-pad instellen
Gebruik de volgende opdracht uit:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Stap 4: De oorspronkelijke gegevens kopiëren naar het nieuwe opslag-pad
Gebruik de volgende opdracht uit:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Stap 5: Machtigingen wijzigen zodat MySQL toegang hebt tot (lezen en schrijven) de gegevensschijf
Gebruik de volgende opdracht uit:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>De schijf i/o-planning algoritme aanpassen
Linux implementeert vier typen I/O algoritmen plannen:  

-   Nooperation algoritme (geen bewerking)
-   Deadline algoritme (Deadline)
-   Volledig fair wachtrij plaatsen algoritme (CFQ)
-   Budget periode algoritme (Anticipatory)  

U kunt verschillende i/o-planners onder verschillende scenario's optimaliseren selecteren. In een omgeving volledig RAM is er een groot verschil tussen de CFQ en Deadline algoritmen voor prestaties. Het is raadzaam om in te stellen van de omgeving MySQL-database aan een Deadline voor stabiliteit. Als er een groot aantal opeenvolgende I/O, mogelijk CFQ schijf o prestaties verkleinen.   

Met Nooperation of Deadline bereiken betere prestaties dan de standaard-planner voor SSD en andere apparatuur.   

Uit de kernel 2,5 is de standaard i/o-planning algoritme Deadline. Begin van de kernel 2.6.18, kwam CFQ de standaard i/o-planning algoritme.  U kunt deze instelling bij het opstarten Kernel opgeven of dynamisch deze instelling te wijzigen wanneer het systeem wordt uitgevoerd.  

Het volgende voorbeeld ziet u hoe u kunt controleren en stel de standaard-planner op de algoritme van Nooperation.  

Voor het gezin Debian verdeling:

###<a name="step-1view-the-current-io-scheduler"></a>Stap 1. weergave de huidige I/O scheduler
Gebruik de volgende opdracht uit:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

U ziet na uitvoer, geeft de huidige planner.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Stap 2. Het huidige apparaat (/ ontwikkelaar/sda) van I/O algoritme van de planning wijzigen
De volgende opdrachten gebruiken:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Niet is het handig om dit instelt voor /dev/sda alleen. Deze moeten worden ingesteld op alle gegevensschijven waar de database zich bevindt.  

U ziet het volgende resultaat, die aangeeft dat grub.cfg zijn opnieuw gemaakt en dat de standaard-planner is bijgewerkt naar Nooperation.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Voor het gezin Redhat verdeling hoeft u alleen de volgende opdracht uit:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Systeem bestand bewerkingen instellingen configureren
Een aanbevolen procedure is voor het uitschakelen van de functie atime logboekregistratie in het bestandssysteem. Atime is de tijd van de laatste bestand-toegang. Wanneer een bestand wordt geopend, wordt in het bestandssysteem de tijdstempel vastgelegd in het logboek. Deze informatie is echter zelden gebruikt. Als u niet nodig hebt, waarmee wordt totale tijd van de schijf toegang beperkt, kunt u deze uitschakelen.  

Als u wilt vastleggen atime uitschakelt, moet u de optie **noatime** toevoegen en wijzigen van het bestand systeem configuratie bestand /etc/ bevinden fstab.  

Bijvoorbeeld, bewerk het vim /etc/fstab bestand, het toevoegen van de noatime zoals hieronder wordt weergegeven.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Vervolgens opnieuw koppelen het bestandssysteem met de volgende opdracht uit:  

    mount -o remount /RAID0

Test de gewijzigde resultaat. Houd er rekening mee dat wanneer u het testbestand wijzigen, de tijd toegang niet bijgewerkt.  

Voordat u voorbeeld:     

![][5]

Na het voorbeeld:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Het maximum aantal systeem grepen voor hoge gelijktijdigheid vergroten
MySQL is hoog gelijktijdigheid database. Het standaardaantal gelijktijdige grepen is 1024 voor Linux, niet altijd voldoende. **Gebruik de volgende stappen om de maximale gelijktijdige om het formaat van het systeem ter ondersteuning van hoge gelijktijdigheid van MySQL de te verbeteren**.

###<a name="step-1-modify-the-limitsconf-file"></a>Stap 1: Het bestand limits.conf wijzigen
Voeg de volgende vier regels in het bestand /etc/security/limits.conf voor het vergroten van de maximaal toegestane gelijktijdige grepen. Houd er rekening mee dat 65536 is het maximum aantal die het systeem kunt ondersteunen.   

    * zachte nofile 65536
    * vaste nofile 65536
    * zachte nproc 65536
    * vaste nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Stap 2: Het systeem voor de nieuwe limieten bijwerken
Voer de volgende opdrachten:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Stap 3: Zorg ervoor dat de limieten worden bijgewerkt bij het opstarten
De volgende opstartopdrachten in het bestand /etc/rc.local plaatsen, zodat het effect tijdens elke keer opstarten duurt.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optimalisatie van MySQL-database
U kunt de strategie voor het afstemmen van prestaties dezelfde MySQL op Azure als configureren op een lokale computer.  

De belangrijkste i/o-optimalisatie regels zijn:   

-   Verhoog de cachegrootte.
-   Verminder i/o-antwoord tijd.  

Als u wilt optimaliseren MySQL-serverinstellingen, kunt u het bestand my.cnf, dat wil het configuratiebestand standaard voor server en clientcomputers zeggen bijwerken.  

De volgende items voor de configuratie zijn de belangrijkste factoren die betrekking hebben op prestaties MySQL:  

-   **innodb_buffer_pool_size**: de buffergroep bevat gegevens en de index. Dit is meestal ingesteld op 70% van fysieke geheugen.
-   **innodb_log_file_size**: dit is de grootte van het opnieuw. U opnieuw Logboeken gebruiken om ervoor te zorgen dat schrijven bewerkingen snelle, betrouwbare en worden hersteld nadat ik heb vastlopen zijn. Dit is ingesteld op 512MB, zodat u voldoende ruimte voor logboekregistratie schrijven bewerkingen.
-   **max_connections**: soms toepassingen niet sluit verbindingen correct. Een hogere waarde geeft de server meer tijd voor de Prullenbak inactief sinds verbindingen. Het maximum aantal verbindingen is 10000, maar het aanbevolen maximum is 5000.
-   **Innodb_file_per_table**: met deze instelling in- of uitschakelen van de mogelijkheid van InnoDB voor de opslag van tabellen in afzonderlijke bestanden. Schakel de optie zorgt ervoor dat dat meerdere van de geavanceerde beheerbewerkingen efficiënt kunnen worden toegepast. Vanuit het oogpunt van prestaties, kan deze de overdracht van de ruimte tabel versnellen en de opgeruimd management prestaties optimaliseren. De aanbevolen instelling voor dit is dus ingeschakeld.</br>
    Uit MySQL-5.6 is de standaardinstelling ingeschakeld. Daarom geen actie moet worden ondernomen. Voor andere-versies die ouder is dan 5.6, standaardinstellingen is uitgeschakeld. Deze aan schakelen is vereist. En moeten deze hebt toegepast voordat gegevens worden geladen, omdat alleen gemaakte tabellen worden beïnvloed.
-   **innodb_flush_log_at_trx_commit**: standaardwaarde is 1, met het bereik dat is ingesteld op 0 ~ 2. De standaardwaarde is de meest geschikte optie voor zelfstandige MySQL-DB. De instelling van 2 schakelt op de meeste gegevensintegriteit en is geschikt voor outmodel in MySQL-cluster. De instelling van 0 kunt verlies van gegevens, welke invloed kan zijn op de betrouwbaarheid, in sommige gevallen met betere prestaties, en is geschikt voor Slave in MySQL-cluster.
-   **Innodb_log_buffer_size**: de logboekbuffer kan transacties uitvoeren zonder dat u moet het leegmaken van het logboek naar schijf voordat de transacties doorvoeren. Als er grote binaire object of tekstveld, wordt de cache zeer snel verbruikt en veelgebruikte schijf I/O wordt geactiveerd. Is het beter de vergroten buffer als Innodb_log_waits staat variabele geen 0.
-   **query_cache_size**: de beste optie is uitgeschakeld vanaf het begin. Query_cache_size ingesteld op 0 (dit is nu de standaardinstelling in MySQL 5.6) en andere methoden gebruiken om query's te versnellen.  

Zie [Bijlage D](#AppendixD) voor het vergelijken van prestaties na de optimalisatie.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Het logboek MySQL traag query voor het analyseren van de bottleneck inschakelen
De MySQL traag query-logboek kunt u de traag query's bepalen voor MySQL. Na het inschakelen van het logboek MySQL-query wordt vertraagd, kunt u MySQL-hulpprogramma's zoals **mysqldumpslow** de bottleneck identificeren.  

Houd er rekening mee dat al dan niet standaard dit niet is ingeschakeld. Inschakelen van de query wordt vertraagd log in beslag sommige CPU-bronnen. Het verdient daarom in te schakelen dit tijdelijk voor het oplossen van prestatieproblemen.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Stap 1: My.cnf bestand wijzigen door het toevoegen van de volgende regels naar het einde   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Stap 2: Start opnieuw mysql-server
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Stap 3: Controleren of de instelling duurt het effect met de opdracht 'weergeven'

![][7]   

![][8]

In dit voorbeeld kunt u zien dat de query wordt vertraagd-functie is ingeschakeld. U kunt de **mysqldumpslow** vervolgens gebruiken om te bepalen van prestatieproblemen en optimaliseren, zoals indexen toe te voegen.





##<a name="appendix"></a>Bijlage

Hierna volgt een voorbeeld test prestatiegegevens geproduceerd op gerichte testomgeving, ze bevatten algemene achtergrond op de prestaties gegevens trend met verschillende prestaties optimaliseren benaderingen, maar de resultaten variëren onder verschillende versies van omgeving of product.

<a name="AppendixA"></a>Bijlage A:  
**Schijfprestaties (IO's / s) met verschillende RAID-niveaus**


![][9]

**Test-opdrachten:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Opmerking: De werklast van deze toets gebruikt 64 threads, probeert te bereiken van de bovengrens van RAID.

<a name="AppendixB"></a>Bijlage B:  
**MySQL prestaties (doorvoer) vergelijking met andere RAID-niveaus**   
(Bestandssysteem XFS)


![][10]  
![][11]

**Test-opdrachten:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL prestaties (OLTP) vergelijking met andere RAID-niveaus**  
![][12]

**Test-opdrachten:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Bijlage C:   
**Vergelijking van de schijf prestaties (IO's / s) voor verschillende segment grootte**  
(Bestandssysteem XFS)


![][13]

**Test-opdrachten:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Opmerking de grootte van het bestand waarmee u deze test is 30 en 1GB respectievelijk, met RAID 0(4 disks) XFS ve systeem.


<a name="AppendixD"></a>Bijlage D:  
**MySQL prestaties (doorvoer) vergelijking voor en na optimalisatie**  
(XFS File System)


![][14]

**Test-opdrachten:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**De configuratie-instelling voor optmization en die standaard is als volgt:**

|Parameters |Standaard    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Geen   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Meer gedetailleerde optimalisatie configuratieparameters, Raadpleeg de officiële mysql-instructies.  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Testomgeving**  

|Hardware   |Meer informatie
|-----------|-------
|CPU    |AMD Opteron (TM) Processor 4171 HE / 4 cores
|Geheugen |14G
|Schijfopruiming   |10G/schijf
|OS |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
