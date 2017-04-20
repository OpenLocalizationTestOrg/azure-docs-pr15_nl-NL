<properties
    pageTitle="MySQL clusterize met taakverdeling sets | Microsoft Azure"
    description="Een taakverdeling hoge beschikbaarheid Linux MySQL cluster met het implementatiemodel klassieke op Azure gemaakt instellen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Gebruik van taakverdeling sets naar MySQL op Linux clusterize

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Het doel van dit artikel is te verkennen en te illustreren de verschillende methoden die beschikbaar zijn voor het implementeren van maximaal beschikbare Linux gebaseerde services op Microsoft Azure verkennen beschikbaarheid als een handleiding MySQL-Server. Een video illustreren van deze methode is beschikbaar op [kanaal 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

We een overzicht maken van een gedeeld twee knooppunten enkel-outmodel MySQL beschikbaarheid oplossing op basis van DRBD, Corosync en pacemaker heeft. Slechts één knooppunt actief is MySQL tegelijk. Lezen en schrijven van de resource DRBD is ook beperkt tot slechts één knooppunt tegelijk.

Er is niet nodig voor een VIP-oplossing zoals LVS omdat we van Microsoft Azure laden verdeeld Sets gebruiken voor zowel de round robin-functionaliteit en het eindpunt detectie, verwijderen en gecontroleerde herstelprocedure van de VIP. De VIP is een globaal geschikt IPv4-adres dat is toegewezen door Microsoft Azure wanneer we de cloudservice voor het eerst maken.

Er zijn andere mogelijke architecturen voor MySQL met inbegrip van de volgende werkdag Cluster, Percona en Galera, evenals verschillende middleware-oplossingen, waaronder ten minste één beschikbaar als een VM op [VM opslagplaats](http://vmdepot.msopentech.com). Zo lang maken als deze oplossingen op unicast versus multicast of uitzending repliceren kunnen en zijn niet afhankelijk van gedeelde opslag of meerdere netwerkinterfaces, is de scenario's moet eenvoudig te implementeren op Microsoft Azure.

Natuurlijk kunnen deze clustering architecturen aan andere producten zoals PostgreSQL en OpenLDAP op soortgelijke wijze worden verleend. Bijvoorbeeld deze procedure voor taakverdeling met gedeelde succes met meerdere basispagina OpenLDAP is getest en u kunt deze bekijken op onze blog kanaal 9.

## <a name="getting-ready"></a>Op het punt staat

Moet u een Microsoft Azure-account met een geldig abonnement op ten minste twee (2) VMs maken (in dit voorbeeld is X'EN gebruikt), een netwerk en een subnet, een groep affiniteit en een beschikbaarheid instellen, evenals de mogelijkheid om te maken van nieuwe VHD's in dezelfde gebied, als de cloudservice en koppel deze aan de VMs Linux.

### <a name="tested-environment"></a>Geteste omgeving

- Ubuntu 13.10
  - DRBD
  - MySQL-Server
  - Corosync en pacemaker heeft

### <a name="affinity-group"></a>Affiniteit groep

Een groep affiniteit voor de oplossing wordt gemaakt door logboekregistratie in Azure klassieke portal schuiven naar beneden af op instellingen en het maken van een nieuwe affiniteit-groep. Toegewezen bronnen die later zijn gemaakt, worden toegewezen aan deze groep affiniteit.

### <a name="networks"></a>Netwerken

Een nieuw netwerk wordt gemaakt en een subnet binnen het netwerk wordt gemaakt. We hebben een netwerk 10.10.10.0/24 met slechts één /24 subnet binnen gekozen.

### <a name="virtual-machines"></a>Virtuele machines

De eerste Ubuntu 13.10 VM is gemaakt met behulp van een afbeelding Endorsed Ubuntu galerie en genoemd `hadb01`. Een nieuwe cloudservice is gemaakt in het proces, hadb genoemd. We Roep deze deze manier om te laten zien van de gedeelde, verdeeld aard die de service heeft als we meer resources toevoegen. Het maken van `hadb01` is probleemloze en voltooide met behulp van de portal. Een eindpunt voor SSH wordt automatisch gemaakt en onze gemaakte netwerk is geselecteerd. We ook kiezen om te maken van een nieuwe beschikbaarheid instellen voor de VMs.

Nadat de eerste VM is gemaakt (technisch, wanneer de cloudservice is gemaakt) we doorgaan met het maken van de tweede VM `hadb02`. Voor het tweede VM we ook gebruiken Ubuntu 13.10 VM vanuit de galerie met behulp van de portal maar kiezen we gebruik van een bestaande cloudservice, `hadb.cloudapp.net`, in plaats van een nieuwe id maken. De set netwerk- en beschikbaarheid moet voor ons automatisch zijn geselecteerd. Een eindpunt SSH wordt gemaakt, ook.

Nadat u beide VMs hebt gemaakt, duurt we notitie van de poort SSH voor `hadb01` (TCP 22) en `hadb02` (automatisch toegewezen door Azure)

### <a name="attached-storage"></a>Bijgevoegde opslag

We een nieuwe schijf aan beide VMs toegevoegd en maakt nieuwe 5 GB schijven in het proces. De schijven wordt in de container VHD wordt gebruikt voor onze belangrijkste besturingssysteem schijven worden gehost. Zodra schijven zijn gemaakt en die zijn bijgevoegd is niet nodig voor ons Linux opnieuw, zoals de kernel het nieuwe apparaat ziet (meestal `/dev/sdc`, kunt u controleren `dmesg` voor de uitvoer)

Klik op elke VM die we doorgaan met het maken van een nieuwe partition met `cfdisk` (primaire, Linux partition) en de nieuwe partitietabel schrijven. **Maak een bestandssysteem op deze partition geen** .

## <a name="setting-up-the-cluster"></a>Bij het instellen van het cluster

In beide VMs Ubuntu moeten we gebruiken APT Corosync, pacemaker heeft en DRBD installeren. Met `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Installeer MySQL op dit moment niet** . Debian en Ubuntu installatiescripts wordt een MySQL-gegevensmap geïnitialiseerd op `/var/lib/mysql`, maar omdat de map wordt worden vervangen door een bestandssysteem DRBD, moeten we dit later doen.

Nu we moet ook controleren of (met `/sbin/ifconfig`) of beide VMs adressen in het subnet 10.10.10.0/24 en dat ze elkaar op naam kunnen pingen gebruikt. Desgewenst kunt u ook gebruiken `ssh-keygen` en `ssh-copy-id` om ervoor te zorgen dat beide VMs kunnen communiceren via SSH zonder een wachtwoord.

### <a name="setting-up-drbd"></a>Bij het instellen van DRBD

We een DRBD resource die gebruikmaakt van de onderliggende maakt `/dev/sdc1` partition tot een `/dev/drbd1` resources kunnen worden opgemaakt met ext3 en gebruikt in zowel primaire als secundaire knooppunten. Klik hiertoe open `/etc/drbd.d/r0.res` en kopieer de volgende definitie van de resource. Ga als volgt in beide VMs:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Nadat u dit doet, geïnitialiseerd de resource met behulp van `drbdadm` in beide VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

En ten slotte op de primaire (`hadb01`) eigendom (primair) van de resource DRBD afdwingen:

    sudo drbdadm primary --force r0

Als u de inhoud van/procedure/drbd bekijken (`sudo cat /proc/drbd`) op beide VMs, ziet u `Primary/Secondary` op `hadb01` en `Secondary/Primary` op `hadb02`, in overeenstemming met de oplossing op dit punt. De schijf 5 GB wordt gesynchroniseerd via het netwerk 10.10.10.0/24 gratis naar klanten.

Wanneer de schijf wordt gesynchroniseerd, kunt u het bestandssysteem maken op `hadb01`. Voor testdoeleinden we ext2 gebruikt, maar de volgende instructies uit een bestandssysteem ext3 maakt:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>De resource DRBD koppelen

Klik op `hadb01` we nu klaar bent om te koppelen van de resources DRBD. Gebruik debian en producten `/var/lib/mysql` als de MySQL-gegevensmap. Aangezien we MySQL nog niet hebt geïnstalleerd, we de map maken en de resource DRBD koppelen. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>MySQL instellen

Nu u iets wilt installeren MySQL op `hadb01`:

    sudo apt-get install mysql-server

Voor `hadb02`, hebt u twee opties. Mysql-server nu waarmee u /var/lib/mysql maken en deze een arcering met een nieuwe gegevensmap en gaat u verder met de inhoud verwijderen, kunt u installeren. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

De tweede optie is foutherstel naar `hadb02` en installeer vervolgens er mysql-server (installatiescripts zult de bestaande installatie en heeft geen invloed op deze)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Als u niet van plan foutherstel DRBD nu bent, is de eerste optie gemakkelijker Hoewel hoewel minder elegante. Nadat u dit ingesteld, kunt u werken aan uw MySQL-database kunt starten. Klik op `hadb02` (of welke van de servers actief is, op basis van de DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Waarschuwing**: deze laatste instructie effectief verificatie voor de gebruiker hoofdmap in deze tabel is uitgeschakeld. Dit moet worden vervangen door uw productie-grade verlenen instructies en is opgenomen alleen ter illustratie.

U moet ook inschakelen toegang voor MySQL als u wilt maken van query's vanuit buiten de VMs, dat wil het doel van deze handleiding zeggen. Open op beide VMs, `/etc/mysql/my.cnf` en bladert u naar `bind-address`, 127.0.0.1 op 0.0.0.0 wijzigen. Nadat het bestand is opgeslagen, actie-een `sudo service mysql restart` op uw huidige primaire.

### <a name="creating-the-mysql-load-balanced-set"></a>Maken van de MySQL-laden verdeeld instellen

We zullen gaat u terug naar de portal en bladert u naar de `hadb01` VM en vervolgens de eindpunten. We wordt een nieuw eindpunt maken, kiest u MySQL (TCP 3306) in de vervolgkeuzelijst en maatstreepjes in het vak *nieuwe laden maken verdeeld instellen* . We onze eindpunt taakverdeling bellen `lb-mysql`. Gebruikt u de meeste opties alleen, behalve voor de tijd waarop we tot en met 5 (seconden, minimale verlagen)

Nadat het eindpunt is gemaakt we gaat u naar `hadb02`, eindpunten, en maak een nieuwe eindpunt maar kiezen we `lb-mysql`, selecteer MySQL in de vervolgkeuzelijst. U kunt ook de CLI Azure gebruiken voor deze stap.

Op dit moment hebben we alles die we nodig voor een handmatige werking van het cluster.

### <a name="testing-the-load-balanced-set"></a>Het selectievakje laden testen verdeeld instellen

Tests uit een externe computer kunnen worden uitgevoerd, met behulp van een MySQL-client, evenals toepassingen (bijvoorbeeld phpMyAdmin wordt uitgevoerd met een Website Azure) In dit geval we de MySQL opdrachtregel hulpmiddel in een andere Linux gebruikt:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Handmatig verbroken via

U kunt nu failovers simuleren door MySQL afgesloten, overstappen van DRBD primair en MySQL opnieuw te starten.

Klik op hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Klik op hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Zodra u failover handmatig Herhaal uw externe query en deze moeten worden werken perfect.

## <a name="setting-up-corosync"></a>Bij het instellen van Corosync

Corosync is de onderliggende cluster-infrastructuur vereist voor pacemaker heeft om te werken. Voor Heartbeat v1 en v2 gebruikers (en andere methoden zoals Ultramonkey) Corosync is een splitsing van de CRM-functionaliteit, terwijl pacemaker heeft meer lijken op heartbeat in functionaliteit blijft.

De belangrijkste beperking voor Corosync op Azure is dat Corosync voorkeur multicast via broadcast via unicast-communicatie, maar Microsoft Azure netwerken biedt alleen ondersteuning voor unicast.

Gelukkig Corosync heeft een werken unicast-modus en de enige echte beperking is dat, omdat alle knooppunten niet *automagically*onderling communiceren zijn, u het definiëren van de knooppunten in de configuratiebestanden moet, waaronder de IP-adressen. We kunnen de Corosync voorbeeld-bestanden gebruiken voor Unicast en wijzigt adres, knooppunt lijsten en map voor logboekregistratie binden (Ubuntu gebruikt `/var/log/corosync` terwijl het voorbeeld gebruiken bestanden `/var/log/cluster`) en hulpmiddelen voor quorum inschakelen.

**Notitie de `transport: udpu` richtlijn onderstaande en het handmatig gedefinieerde IP-adressen voor de knooppunten**.

Klik op `/etc/corosync/corosync.conf` voor beide knooppunten:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

We dit configuratiebestand in beide VMs kopiëren en Corosync in beide knooppunten starten:

    sudo service start corosync

Kort na het starten van de service het cluster in de huidige ring moet worden ingevoerd en quorum moet bestaan. We deze functionaliteit aan de hand van de logboeken kunt controleren of:

    sudo corosync-quorumtool –l

Een resultaat zoals in de onderstaande afbeelding moet volgen:

![Voorbeeld van corosync-quorumtool - l-uitvoer](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Bij het instellen van pacemaker heeft

Pacemaker heeft gebruikt de cluster bewaken voor resources, definiëren als primaire omlaag gaan en deze resources overschakelen naar secundaire servers. Resources kunnen worden gedefinieerd vanuit een set beschikbare scripts of LSB (initialisatie-achtige) scripts, door de andere beschikbare keuzen.

We horen pacemaker heeft "eigenaar" de resource DRBD, de koppelpunt en de MySQL-service. Als u pacemaker heeft in- of uitschakelen DRBD omzetten kunt, koppelen / umount dit en starten en stoppen MySQL in de juiste volgorde wanneer een ongeldige gebeurt er met de primaire, onze setup is voltooid.

Wanneer u eerst pacemaker heeft geïnstalleerd, is uw configuratie ziet er zo eenvoudig is, bijvoorbeeld:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Controleer of de map door te voeren `sudo crm configure show`. Maak nu een bestand (bijvoorbeeld `/tmp/cluster.conf`) met de volgende bronnen:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

En laadt u deze nu in de configuratie (alleen moet u dit in één knooppunt doen):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Zorg ook dat pacemaker heeft tijdens het opstarten in beide knooppunten begint:

    sudo update-rc.d pacemaker defaults

Na een paar seconden en het gebruik van `sudo crm_mon –L`, Controleer of een van de knooppunten geworden de hoofdlijst voor het cluster, en alle resources is uitgevoerd. U kunt koppelen en ps gebruiken om te controleren of de resources worden uitgevoerd.

In de volgende schermafbeelding ziet `crm_mon` met één knooppunt gestopt (afsluiten met Control-C)

![crm_mon knooppunt gestopt](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

En dit schermafbeelding ziet u beide knooppunten, met één diamodel en één slave:

![crm_mon operationele hoofdsectie/detailsectie](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testen

Wilt u een automatische failover simulatie hebt. Er zijn twee manieren om uit te voeren: zachte en harde. De vloeiende manier van het cluster afsluiten functie wordt gebruikt: ``crm_standby -U `uname -n` -v on``. Met dit in het model, wordt de slave overnemen. Moet u dit opnieuw ingesteld op uit (crm_mon kunt u nagaan één knooppunt is anderszins stand-by)

De harde is manier de primaire VM (hadb01) afgesloten via de portal of wijzigen van de (uitvoeringsniveau) op de VM (dat wil zeggen stoppen, afsluiten) en vervolgens wij helpen Corosync en pacemaker heeft door-signalering van model te gaan naar beneden. We dit kunt testen (handig voor onderhoud windows), maar we kunt ook het scenario afdwingen door te blokkeren alleen de VM.

## <a name="stonith"></a>STONITH

Het moet mogelijk zijn er een VM afsluiten via de CLI Azure in plaats van een STONITH-script waarmee wordt bepaald een fysiek apparaat. U kunt `/usr/lib/stonith/plugins/external/ssh` als een basis- en inschakelen STONITH in de configuratie van het cluster. Azure CLI globaal moet worden geïnstalleerd en het publiceren van de instellingen/profiel moet worden geladen voor de gebruiker van het cluster.

De code van de steekproef voor de resource die beschikbaar zijn op [GitHub](https://github.com/bureado/aztonith). U moet de configuratie van het cluster wijzigen door toe te voegen van de volgende `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Notitie:** het script omhoog/omlaag-controles niet uitvoeren. De oorspronkelijke SSH resource had 15 ping controles maar hersteltijd voor een VM Azure mogelijk meer variabele.

## <a name="limitations"></a>Beperkingen

Er gelden de volgende beperkingen:

- Het script linbit DRBD resource die worden beheerd in DRBD als een resource in pacemaker heeft gebruik `drbdadm down` bij het afsluiten van een knooppunt, zelfs als het knooppunt is alleen stand-by gaat. Dit is niet ideaal sinds de slave wordt niet worden gesynchroniseerd de resource DRBD terwijl het model geschreven wordt. Als het model niet vriendelijk mislukt bevat, kan de slave een oudere bestandssysteem status overnemen. Er zijn twee mogelijke manieren voor het oplossen van dit:
  - Afdwingen een `drbdadm up r0` in alle knooppunten via een lokale (niet clusterized) watchdog, of,
  - Bewerken van de linbit DRBD script ervoor te zorgen dat `down` niet wordt aangeroepen, in `/usr/lib/ocf/resource.d/linbit/drbd`.
- Taakverdeling moet ten minste 5 seconden reageren, zodat toepassingen cluster op de hoogte is en grotere tolerantie voor time-out; andere architecturen kunt, bijvoorbeeld app wachtrijen, query middlewares, enzovoort.
- MySQL afstemmen is geschreven in een gezond tempo en cache zijn leeggemaakt om zo vaak mogelijk te minimaliseren ten geheugen verlies schijf
- Schrijven prestaties zijn afhankelijk in VM verbinding in de virtuele switch, als dit de toegepaste door DRBD is repliceren van het apparaat
