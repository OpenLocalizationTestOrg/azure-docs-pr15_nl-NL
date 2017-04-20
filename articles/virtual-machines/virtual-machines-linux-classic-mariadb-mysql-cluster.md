<properties
    pageTitle="Een cluster MariaDB (MySQL) op Azure uitgevoerd"
    description="Maken van een MariaDB + Galera MySQL cluster op Azure virtuele Machines"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL) cluster - Azure zelfstudie

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB Enterprise cluster is nu beschikbaar in de Azure Marketplace.  De nieuwe aanbod implementeert automatisch een MariaDB Galera cluster in de cloud. U moet de nieuwe aanbod van https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ gebruiken 

We creëren een meerdere basispagina [Galera](http://galeracluster.com/products/) cluster van [MariaDBs](https://mariadb.org/en/about/), robuuste, scalable en betrouwbare onverwachte vervanging voor MySQL, om te werken in een omgeving met hoge beschikbaarheid op Azure virtuele Machines.

## <a name="architecture-overview"></a>Overzicht van de architectuur

In dit onderwerp worden de volgende stappen uitgevoerd:

1. Een 3-knooppunt cluster maken
2. De gegevensschijven scheiden van de schijf OS
3. De schijven gegevens maken in RAID 0 gestreepte/instelling om uit te breiden IO's / s
4. Gebruik van de Azure taakverdeling taakverdeling voor de 3 knooppunten
5. Als u wilt minimaliseren terugkerende werk, de afbeelding van een VM met MariaDB + Galera maken en hiermee het andere cluster VMs maken.

![Architectuur](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  In dit onderwerp worden de hulpmiddelen [Azure CLI](../xplat-cli-install.md) gebruikt, Controleer dus of te downloaden en deze verbinden met uw Azure abonnement volgens de instructies. Als u een verwijzing naar de opdrachten in de CLI Azure nodig hebt, raadpleegt u deze koppeling voor de [Azure CLI opdrachtverwijzing](../virtual-machines-command-line-tools.md). U wordt ook moet [maken van een sleutel SSH voor verificatie] en noteer de **bestandslocatie .pem**.


## <a name="creating-the-template"></a>De sjabloon maken

### <a name="infrastructure"></a>Infrastructuur

1. Een groep affiniteit waarin de bronnen samen maken

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Een virtueel netwerk maken

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Maak een Account opslag als host voor alle onze schijven. Opmerking dat niet mag worden plaatst u meer dan 40 intensief gebruikt schijven op hetzelfde opslag-Account om te voorkomen kunt u door de 20.000 IO's / s account opslaglimiet. In dit geval we ver uit vanaf dit nummer zodat alles wordt opgeslagen op hetzelfde account voor eenvoudig

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Zoek de naam van de afbeelding CentOS 7 virtuele machines

        azure vm image list | findstr CentOS
Hiermee wordt ongeveer zo uitvoer `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Gebruik de naam in de volgende stap.

4. De **/path/to/key.pem** vervangen door het pad waar u de gegenereerde .pem SSH toets opgeslagen VM-sjabloon maken

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4-x 500GB gegevensschijven als bijlage toevoegen aan de VM voor gebruik in de configuratie RAID

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH in de sjabloon VM die u hebt gemaakt op **mariadbhatemplate.cloudapp.net:22** en verbinding maken met uw persoonlijke sleutel.

### <a name="software"></a>Software

1. Hoofdmap verkrijgen

        sudo su

2. Ondersteuning voor RAID installeren:

     - Mdadm installeren

                yum install mdadm

     - De configuratie RAID 0/streep maken met een bestandssysteem EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - De gekoppelde punt map maken

                mkdir /mnt/data

     - Ophalen van de UUID van het zojuist gemaakte RAID-apparaat

                blkid | grep /dev/md0

     - /Etc/fstab bewerken

                vi /etc/fstab

     - Het apparaat dat er automatisch mouting bij opstarten de UUID vervangen door de waarde opgehaald van de opdracht **blkid** voordat inschakelen toevoegen

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - De nieuwe partition koppelen

                mount /mnt/data

3. MariaDB installeren:

     - Het bestand MariaDB.repo maken:

                vi /etc/yum.repos.d/MariaDB.repo

     - Vullen met de hieronder inhoud

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Verwijder bestaande postfix en mariadb-libs om conflicten te voorkomen

            yum remove postfix mariadb-libs-*

     - MariaDB installeren met Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. De MySQL-gegevensmap verplaatsen naar het schijfstation RAID

     - Kopieer de huidige MySQL-map naar de nieuwe locatie en verwijder de oude adreslijst

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Machtigingen instellen voor nieuwe map dienovereenkomstig gewijzigd

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Een symlink die de oude map wijst naar de nieuwe locatie op de partition RAID maken

            ln -s /mnt/data/mysql /var/lib/mysql

5. Omdat [dat SELinux verstoort de clusterbewerkingen](http://galeracluster.com/documentation-webpages/configuration.html#selinux), moet worden uitschakelen voor de huidige sessie (totdat een compatibele versie wordt weergegeven). Bewerken `/etc/selinux/config` deze voor volgende opnieuw opstarten uitschakelen:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Valideer de MySQL wordt uitgevoerd

    - MySQL starten

            service mysql start

    - Secure de MySQL-installatie, het wachtwoord van de hoofdsite instellen, anonieme gebruikers, externe hoofdmap login uitschakelen en het verwijderen van de testdatabase verwijderen

            mysql_secure_installation

    - Een gebruiker maken van de database voor clusterbewerkingen en u kunt ook uw toepassingen

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - MySQL stoppen

            service mysql stop

7. Tijdelijke aanduiding voor de configuratie maken

    - De MySQL-configuratie als u wilt maken van een tijdelijke aanduiding voor de clusterinstellingen bewerken. Niet vervangen de **`<Vairables>`** of verwijder de opmerkingen nu bij. Die treedt nadat we een VM met deze sjabloon maken.

            vi /etc/my.cnf.d/server.cnf

    - De sectie **[galera]** bewerken en maak deze

    - De sectie **[mariadb]** bewerken

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Vereiste poorten op de firewall (via FirewallD op CentOS 7) openen

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IJST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Opnieuw laden de firewall:`firewall-cmd --reload`

9.  Het systeem voor de prestaties optimaliseren. Raadpleeg dit artikel over [Prestaties afstemmen strategie](virtual-machines-linux-classic-optimize-mysql.md) voor meer informatie

    - Het configuratiebestand MySQL opnieuw bewerken

            vi /etc/my.cnf.d/server.cnf

    - De sectie **[mariadb]** bewerken en toevoegen de onder

    > [AZURE.NOTE] Het wordt aanbevolen dat **innodb\_buffer\_pool_size** 70% van uw VM geheugen. Dit is ingesteld op hier 2,45 GB voor het Medium Azure VM met 3,5 GB RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. MySQL stoppen, MySQL-service uitvoeren op opstarten om te voorkomen dat u de cluster bij het toevoegen van een nieuw knooppunt uitschakelen en deprovision van de computer.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. De VM via de portal vastleggen. (Momenteel [probleem #1268 in de CLI Azure] hulpmiddelen voor beschrijving van het feit dat afbeeldingen die zijn vastgelegd met het hulpprogramma Azure CLI's niet de gekoppelde gegevensschijven vastlegt.)

    - De computer via de portal afsluiten
    - Klik op vastleggen en geef de naam van de afbeelding als **mariadb-galera-afbeelding** en geef een beschrijving en controleren "Ik heb waagent uitvoer".
    ![De virtuele Machine vastleggen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![de virtuele Machine vastleggen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Het cluster maken

Maak 3 VMs afmelden bij de sjabloon die u zojuist hebt gemaakt en vervolgens configureren en het cluster starten.

1. Maken van de eerste CentOS 7 VM van de **mariadb-galera-afbeelding van de** afbeelding die u hebt gemaakt, mits de virtueel netwerk naam **mariadbvnet** en de subnet **mariadb**, grootte, **Gemiddeld**, doorgeven in de Cloud-Service van de computer een naam moeten **mariadbha** (of de naam die u wilt worden geraadpleegd via mariadbha.cloudapp.net), de naam van deze instelling machine **mariadb1** en de gebruikersnaam moeten **azureuser**en SSH toegang bieden en doorgeven van de SSH certificaat .pem bestands- en vervangen van **/path/to/key.pem** door het pad waar u de gegenereerde .pem SSH toets opgeslagen.

    > [AZURE.NOTE] De onderstaande opdrachten zijn onderverdeeld over meerdere regels voor helderheid, maar u moet elk als één regel invoeren.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. 2 meer virtuele Machines maken door _verbinding_ te ze naar de huidige gemaakte **mariadbha** Cloudservice, de **naam van de VM** , evenals de **SSH poort** wijzigen in een unieke poort niet conflicteert met andere VMs in de dezelfde Cloud-Service.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
en voor MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. U moet het interne IP-adres van elk van de 3 VMs krijgen voor de volgende stap:

    ![IP-adres ophalen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH in de 3 VMs en en het configuratiebestand op elk bewerken

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** en **`wsrep_cluster_address`** doordat de **#** aan het begin en validatie ze daadwerkelijk wens zijn.
    Verder vervangen **`<ServerIP>`** in **`wsrep_node_address`** en **`<NodeName>`** in **`wsrep_node_name`** van de VM IP-adres van een naam respectievelijk en verwijder de opmerkingen bij deze regels ook.

5. Het cluster op MariaDB1 starten en laat deze uitgevoerd bij het opstarten

        sudo service mysql bootstrap
        chkconfig mysql on

6. MySQL op MariaDB2 en MariaDB3 starten en laat deze uitgevoerd bij het opstarten

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Het cluster van taakverdeling
Wanneer u de gegroepeerde VMs hebt gemaakt, kunt u deze in een beschikbaarheid instellen genaamd **clusteravset** om te zorgen dat zij op verschillende foutenstructuuranalyse worden gebracht en domeinen en dat Azure nooit onderhoud op alle computers in één keer doet bijwerken toegevoegd. Deze configuratie voldoet aan de vereisten ondersteund door deze Azure Service niveau overeenkomst (SLA).

Nu kunt u de verdeling van de Azure belasting saldo vanaf aanvragen tussen onze 3 knooppunten.

Voer de onderstaande opdrachten op uw computer met de CLI Azure.
De structuur van de opdracht parameters luidt als volgt:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Ten slotte, omdat de CLI wordt het interval van taakverdeling test ingesteld op 15 seconden (die mogelijk iets te lang zijn), deze wijzigen in de portal onder **eindpunten** naar een of meer van de VMs

![Eindpunt bewerken](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

Klik vervolgens op Reconfigure The Load-Balanced instellen en volgende gaan

![Opnieuw te configureren laden gebalanceerde instellen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

vervolgens het Interval voor het zoeken naar 5 seconden wijzigen en opslaan

![Test-Interval wijzigen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>De cluster te valideren

Het werk is gedaan. Het cluster wordt nu toegankelijk is bij `mariadbha.cloudapp.net:3306` waarin wordt druk op de taakverdeling en aanvragen tussen de 3 VMs soepel en efficiënt doorsturen.

Gebruik uw favoriete MySQL-client verbinding te maken of alleen verbinding maken vanaf een van de VMs om te controleren of dat dit cluster werkt.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Vervolgens een nieuwe database maken en deze te vullen met sommige gegevens

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Resulteert in de onderstaande tabel

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

In dit artikel u een 3 hebt gemaakt knooppunt MariaDB + Galera grote beschikbaarheid cluster op Azure virtuele Machines CentOS 7 gebruikt. De VMs zijn verdeeld met de verdeling laden van de Azure.

U wilt gaat u naar [een andere manier om cluster MySQL op Linux](virtual-machines-linux-classic-mysql-cluster.md) en manieren [optimaliseren](virtual-machines-linux-classic-optimize-mysql.md)en test u MySQL-prestaties van Azure Linux VMs.

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[maken van een sleutel SSH voor verificatie]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[probleem #1268 in de CLI Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268
