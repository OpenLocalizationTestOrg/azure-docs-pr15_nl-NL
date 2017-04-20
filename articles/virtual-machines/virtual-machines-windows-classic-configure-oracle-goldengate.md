<properties
    pageTitle="Oracle GoldenGate configureren in VMs | Microsoft Azure"
    description="Een zelfstudie voor het instellen en implementeren van Oracle GoldenGate op Azure Windows Server VMs voor hoge beschikbaarheid herstel stapsgewijs."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />


#<a name="configuring-oracle-goldengate-for-azure"></a>Oracle GoldenGate configureren voor Azure


Deze zelfstudie wordt getoond hoe Oracle GoldenGate instellen voor Azure virtuele Machines-omgeving voor beschikbaarheid en herstel. De zelfstudie bevat informatie over [bidirectionele replicatie](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) voor RAC Oracle-databases en moet beide sites actief zijn.

Oracle GoldenGate ondersteunt verdeling van gegevens en gegevensintegratie van. Dit kunt u voor het instellen van een verdeling van gegevens en de synchronisatie van gegevensoplossing tot en met de configuratie van de Oracle-Oracle-replicatie en een oplossing flexibele beschikbaarheid bevat. Oracle GoldenGate vult de beveiliging van Oracle-gegevens met de mogelijkheden herhaling gegevens voor het hele enterprise distributie en nul-downtime upgrades en migraties kunt inschakelen. Zie [Oracle GoldenGate gebruiken met Oracle gegevens beveiliging](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm)voor gedetailleerde informatie.

Oracle GoldenGate bevat de volgende hoofdonderdelen: extraheren, gegevens pomp, Replicat, sporen of te extraheren controlepunten, Manager en verzamelen-bestanden. Als u wilt dat bidirectionele replicatie tussen twee sites, moet u maken en alle onderdelen op beide sites te starten. Zie voor gedetailleerde informatie over Oracle GoldenGate architectuur, [Oracle GoldenGate handleiding](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Deze zelfstudie wordt ervan uitgegaan dat u al ervaring hebt met theoretisch en praktisch op beschikbaarheid Oracle-Database en herstel concepten, evenals [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Zie de [Oracle-website](http://www.oracle.com/technetwork/database/features/availability/index.html)voor meer informatie.

Bovendien de zelfstudie wordt ervan uitgegaan dat u de volgende vereisten al hebt geïmplementeerd:

- U hebt de sectie beschikbaarheid en aandachtspunten voor noodgevallen herstel in het onderwerp van de [afbeeldingen van VM Oracle - diverse overwegingen](virtual-machines-windows-classic-oracle-considerations.md) al bekeken. Houd er rekening mee dat Azure zelfstandige Oracle-Database exemplaren, maar niet Oracle Real Application Clusters (Oracle RAC) momenteel ondersteunt.

- U kunt de software Oracle GoldenGate hebt gedownload van de website [Oracle-Downloads](http://www.oracle.com/us/downloads/index.html) . Integratie van gegevens, hebt u het Product Pack Oracle fusie Middleware – geselecteerd. U hebt vervolgens Oracle GoldenGate op Oracle v11.2.1 Media Pack geselecteerd voor Microsoft Windows x64 (64-bits) voor een Oracle-database 11 g. Download Oracle GoldenGate V11.2.1.0.3 voor Oracle vervolgens 11g 64-bits Windows 2008 (64-bits).

- U kunt twee virtuele Machines (VMs) hebt gemaakt in Azure wordt aangegeven met Oracle Enterprise Edition op Windows Server. Zorg ervoor dat de virtuele Machines in de [dezelfde cloudservice](virtual-machines-linux-load-balance.md) en in hetzelfde [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) om ervoor te zorgen dat ze toegang tot elkaar via het permanente privé IP-adres.

- U kunt de namen VM hebt ingesteld als "MachineGG1" voor Site A en "MachineGG2" voor Site B bij de portal van Azure klassieke.

- U hebt test databases "TestGG1" gemaakt op de Site A en 'TestGG2' op de Site b te drukken.

- U zich aanmeldt bij uw Windows-server als een lid van de groep Administrators of als lid van de groep **ORA_DBA** .

In deze zelfstudie zal u het volgende doen:

1. Setup-database op de Site A en B-Site  

    1. Eerste import van gegevens uitvoeren

2. Site A en B Site voorbereiden voor databasereplicatie

3. Alle benodigde objecten ter ondersteuning van DDL-replicatie maken

4. GoldenGate Manager op de Site A en B Site configureren

5. Extraheren groep maken en Data Pump processen op de Site A en B-Site

    1. Uitpakken en Data Pump processen op een Site maken

    2. Een GoldenGate controlepunt-tabel maken op de Site B

    3. REPLICAT op B-Site toevoegen

    4. Uitpakken en Data Pump processen maken op de Site B

    5. Een GoldenGate controlepunt-tabel op een Site maken

    6. REPLICAT op A-Site toevoegen

    7. Trandata toe te voegen op de Site A en B-Site

    8. Uitpakken en Data Pump processen starten op Site A

    9. Uitpakken en Data Pump processen starten op Site-B

    10. REPLICAT starten op Site A

    11. REPLICAT starten op Site-B

6. Controleer of het proces bidirectionele herhaling

>[AZURE.IMPORTANT] Deze zelfstudie is instellen en de volgende softwareconfiguratie getest:
>
>|                        | **Een Database**              | **B-sitedatabase**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle-Release**     | Oracle11g Release 2 – (11.2.0.1) | Oracle11g Release 2 – (11.2.0.1) |
>| **De computernaam van de**       | MachineGG1                       | MachineGG2                       |
>| **Besturingssysteem**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle beveiligings-id**         | TESTGG1                          | TESTGG2                          |
>| **Replicatie Schema** | SCOTT                            | SCOTT                            |

Voor latere versies van Oracle-Database en Oracle GoldenGate, is het mogelijk dat er een enkele aanvullende wijzigingen die u wilt implementeren. Zie de documentatie bij de [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) en [Oracle-Database](http://www.oracle.com/us/corporate/features/database-12c/index.html) op Oracle-website voor de meest recente versie specifieke informatie. Bijvoorbeeld, voor een brondatabase release 11.2.0.4 en later, wordt de opname van DDL wordt uitgevoerd door de server logmining asynchroon en vereist geen speciale triggers, tabellen of andere databaseobjecten zijn geïnstalleerd. Oracle GoldenGate upgrades kunnen worden uitgevoerd zonder de gebruikerstoepassingen stoppen. Het gebruik van een DDL-trigger en ondersteunende objecten is vereist als uitpakken in de modus integrated met een Oracle 11g bron-database die ouder is dan versie 11.2.0.4. Zie [installeren en configureren van Oracle GoldenGate voor Oracle-Database](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf)voor gedetailleerde instructies.

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. setup-database op de Site A en B-Site
In deze sectie wordt uitgelegd hoe u de vereisten van de database uitvoeren op zowel Site A en Site b te drukken. U moet de stappen in deze sectie uitvoeren op beide sites: Site A en Site b te drukken.

Eerste, extern bureaublad Site A en B Site via de portal van Azure klassieke. Open een opdrachtprompt van Windows en een basismap voor Oracle GoldenGate setup-bestanden maken:

    mkdir C:\OracleGG

Vervolgens pak en de Oracle GoldenGate-software installeren in deze map. Na deze stap kunt u de GoldenGate Software opdracht Interpreter (GGSCI) starten door de volgende opdracht uit te voeren:

    C:\OracleGG\.\ggsci

U kunt [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) verschillende opdrachten die configureren kunt uitvoeren, beheer en monitor Oracle GoldenGate gebruiken.

Voer de volgende opdracht uit om te maken van alle onderliggende mappen uit het installatiepakket te gaan:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Voer de volgende opdracht om af te sluiten van de opdrachtprompt GGSCI:

    GGSCI (Hostname) 1> EXIT

Vervolgens moet u een databasegebruiker, die worden gebruikt door de processen Oracle GoldenGate Manager, uitpakken en herhaling maken. Houd er rekening mee dat u kunt maken van individuele gebruikers voor elk proces of slechts één algemene gebruiker configureren. In deze zelfstudie maken we een gebruiker, die als ggate wordt genoemd. We verlenen vervolgens de benodigde bevoegdheden van deze gebruiker. Houd er rekening mee dat u de volgende bewerkingen uitvoeren op Site A en Site b te drukken moet.

Open omhoog SQL\*Plus opdrachtvenster op Site A en B van de Site met beheerdersbevoegdheden voor database met **SYSDBA**, zoals:

    Enter username: / as sysdba

En uitvoeren:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Zoek vervolgens naar de Initialisatie\<DatabaseSID\>. ORA bestand in de map % ORACLE\_Start %\\webdatabase map op de Site A en B van de Site en de volgende databaseparameters toevoegen aan INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Zie voor een volledige lijst met alle opdrachten voor Oracle GoldenGate GGSCI, [Reference for Oracle GoldenGate voor Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Eerste import van gegevens uitvoeren

U kunt de eerste import van gegevens uitvoeren in de database aan de hand van verschillende methoden. Bijvoorbeeld, kunt u de [Oracle GoldenGate directe laden](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) of gewone hulpprogramma's exporteren en importeren exporteren van gegevens in een tabel van Site A naar Site b te drukken.

Als u wilt laten zien hoe u Oracle GoldenGate herhaling, ziet u deze zelfstudie maken van een tabel op zowel Site A en B-site met behulp van de volgende opdrachten.

Open eerst omhoog SQL\*Plus opdracht venster en voer de volgende opdracht maken van een voorraadtabel op Site A en B Site databases:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Een beperking vervolgens toevoegen aan de nieuwe tabel aan de Site A en B Site databases:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Vervolgens alle bevoegdheden die op de nieuwe voorraadtabel aan de gebruiker ggate op Site A en b. Site verlenen

    grant all on scott.inventory to ggate;

Vervolgens maken en inschakelen van een databasetrigger, INVENTORY_CDR_TRG, klik op de nieuwe tabel om ervoor te zorgen dat alle transacties aan de nieuwe tabel moeten worden vastgelegd als de gebruiker niet ggate is. Deze bewerking uitvoeren op de Site A en Site b te drukken.

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. voorbereiden op een Site A en B Site databasereplicatie
In deze sectie wordt uitgelegd hoe Site A en B Site voorbereiden voor databasereplicatie. U moet de stappen in deze sectie uitvoeren op beide sites: Site A en Site b te drukken.

Eerste, extern bureaublad Site A en B Site via de portal van Azure klassieke. Overschakelen van de database naar archivelog-modus met behulp van de SQL * Plus opdrachtvenster:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Vervolgens als volgt minimale [aanvullende logboekregistratie](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) inschakelen:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Bereid vervolgens de database moet ondersteuning voor replicatie DDL (data definition language):

    SQL> alter system set recyclebin=off scope=spfile;

Typt u vervolgens, afsluiten en opnieuw starten van de database:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. alle benodigde objecten ter ondersteuning van DDL-replicatie maken
In deze sectie bevat de scripts die u gebruiken om alle benodigde objecten moet ter ondersteuning van DDL-replicatie te maken. Voor het uitvoeren van de scripts die zijn opgegeven in deze sectie op zowel Site A en Site b te drukken.

Open een opdrachtprompt van Windows en navigeer naar de map Oracle GoldenGate, zoals C:\\OracleGG. Start SQL\*Plus opdrachtprompt met beheerdersbevoegdheden van de database, zoals **SYSDBA** op Site A en B. Site gebruiken

Voer de volgende scripts:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle GoldenGate hulpmiddel is een niveau tabel-aanmelding voor ondersteuning van DDL (data definition language) vereist. Daarom heb ik, aanvullende logboekregistratie op het tabelniveau van de met de opdracht toevoegen TRANDATA inschakelen. Open omhoog Oracle GoldenGate interpreter opdrachtvenster, meld u aan bij de database, en voer vervolgens de opdracht TRANDATA toevoegen:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. GoldenGate Manager op de Site A en B Site configureren
De Manager van Oracle GoldenGate voert een aantal functies, zoals het starten van de andere GoldenGate processen, beheer van logboekbestanden audittrail en rapportage.

U moet het proces Oracle GoldenGate Manager op zowel Site A en B. Site configureren Hiervoor kunt u de volgende stappen uitvoeren op de Site A en Site b te drukken.

Geopende vensters opdracht venster en het opdrachtverwerkingsprogramma Oracle GoldenGate starten:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


De sessie GGSCI registreert in een database, zodat u de opdrachten die betrekking hebben op de database kunt uitvoeren:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

De status en vertraging (indien van toepassing) voor alle Manager, uitpakken en Replicat processen op een systeem:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

De status en vertraging (indien van toepassing) voor alle Manager, uitpakken en Replicat processen op een systeem:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

De sessie GGSCI registreert in een database, zodat u de opdrachten die betrekking hebben op de database kunt uitvoeren:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

De manager te starten:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. maken uitpakken groeperen en Data Pump processen op de Site A en B-Site

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Uitpakken en Data Pump processen op een Site maken

U moet de uitpakken en Data Pump processen maken op de Site A en Site B. extern bureaublad tot Site A en B Site via de portal van Azure klassieke. Open omhoog GGSCI interpreter opdrachtvenster. Voer de volgende opdrachten op de Site A:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Open de parameter-bestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie: GGSCI (MachineGG1) 18 > bewerken van parameters ext1 uitpakken ext1 gebruikers-id ggate, wachtwoord ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate tabel scott.inventory, GETBEFORECOLS (op UPDATE KEYINCLUDING (prod_category, qty_in_stock, last_dml), op verwijderen KEYINCLUDING (prod_category, qty_in_stock, last_dml));

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Een GoldenGate controlepunt-tabel maken op de Site B

Vervolgens moet u een tabel controlepunt toevoegen op de Site b te drukken. Hiervoor moet u opent u een GoldenGate interpreter opdrachtvenster en uitvoeren:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

En vervolgens de tabel met controlepunten toevoegen aan de database, waar ggate de eigenaar is:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

De naam van het selectievakje punt tabel toevoegen aan het globale variabelen-bestand op de doelserver, dat wil Site B in deze stap zeggen. Bewerk het bestand globale variabelen op Site-B:

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Vervolgens de parameter CHECKPOINTTABLE toevoegen aan het bestaande globale variabelen-bestand:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Een laatste stap, opslaan en sluit het bestand van de parameter globale variabelen.


###<a name="add-replicat-on-site-b"></a>REPLICAT op B-Site toevoegen
Deze sectie wordt beschreven hoe u een proces REPLICAT "REP2" toe te voegen op de Site b te drukken.

Gebruik toevoegen REPLICAT-opdracht om een Replicat-groep maken op de Site B:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Uitpakken en Data Pump processen maken op de Site B

Deze sectie wordt beschreven hoe u een nieuwe uitpakken 'EXT2' en een nieuwe gegevens pomp proces "DPUMP2" maakt op Site-B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Een GoldenGate controlepunt-tabel op een Site maken

Opent u Oracle GoldenGate interpreter opdrachtvenster en maak een tabel controlepunt:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

U moet ook de naam van het selectievakje punt tabel toevoegen aan het globale variabelen-bestand op de Site A.

Oracle GoldenGate opdracht interpreter-venster openen en bewerken van het bestand globale variabelen op Site-A:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Opslaan en sluit het bestand van de parameter globale variabelen.

###<a name="add-replicat-on-site-a"></a>REPLICAT op A-Site toevoegen

Deze sectie wordt beschreven hoe u een proces REPLICAT "REP1" toe te voegen op de Site A.

De volgende opdracht wordt een Replicat groep rep1 gemaakt met de naam van een spoor, en de bijbehorende checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Open het parameterbestand met de opdracht parameters bewerken en vervolgens toevoegen de volgende informatie:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Trandata toe te voegen op de Site A en B-Site

Aanvullende logboekregistratie op het tabelniveau van de met de opdracht toevoegen TRANDATA inschakelt. Open Oracle GoldenGate interpreter opdrachtvenster, meld u aan bij de database, en voer de opdracht TRANDATA toevoegen.

Extern bureaublad naar MachineGG1, opent u de opdrachtregel Oracle GoldenGate en uitvoeren:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Extern bureaublad naar MachineGG2, opent u de opdrachtregel Oracle GoldenGate en uitvoeren:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Weergave-informatie over de status van de aanvullende tabel niveau voor logboekregistratie:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Trandata toe te voegen op de Site A en B-Site

Aanvullende logboekregistratie op het tabelniveau van de met de opdracht toevoegen TRANDATA inschakelt. Open Oracle GoldenGate interpreter opdrachtvenster, meld u aan bij de database, en voer de opdracht TRANDATA toevoegen.

Extern bureaublad naar MachineGG1, opent u de opdrachtregel Oracle GoldenGate en uitvoeren:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Extern bureaublad naar MachineGG2, opent u de opdrachtregel Oracle GoldenGate en uitvoeren:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Uitpakken en Data Pump processen starten op Site A

Het uitpakken proces ext1 starten op Site-A:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

De gegevens pomp proces dpump1 starten op Site-A:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Informatie over het uitpakken groep ext1 weergeven: GGSCI (MachineGG1) 32 > info extraheren 2013-11-25 ext1 EXTRAHEREN EXT1 laatste gestart 08:03 Status uitgevoerd controlepunt vertragings 00:00:00 (bijgewerkte 00:00:02 geleden) Log gelezen controlepunt Oracle opnieuw uitvoeren logboeken 2013-11-25-08:03:18 het Seqno 6, RBA 3230720 SCN 0.1074371 (1074371)

De status en vertraging (indien van toepassing) voor alle Manager, uitpakken en Replicat processen op een systeem:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Uitpakken en Data Pump processen starten op Site-B

Het uitpakken proces ext2 starten op Site-B:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

De gegevens pomp proces dpump2 starten op Site-B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

De status en vertraging (indien van toepassing) voor alle Manager, uitpakken en Replicat processen op een systeem:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>REPLICAT starten op Site A

Deze sectie wordt beschreven hoe u het proces REPLICAT "REP1" starten op Site A.

Het Replicat starten op Site-A:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

De status van een groep Replicat weergeven:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>REPLICAT starten op Site-B

Deze sectie wordt beschreven hoe u het proces REPLICAT "REP2" starten op Site b te drukken.

Het Replicat starten op Site-B:

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

De status van een groep Replicat weergeven:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Controleer of het proces bidirectionele herhaling

Als u wilt controleren of de Oracle GoldenGate-configuratie, kunt u een rij invoegen in de database op de Site A. extern bureaublad naar de Site A. openen van SQL * Plus opdrachtvenster en -klaar: SQL > Selecteer naam uit v$-database.

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Controleer als die rij op de Site B. worden gerepliceerd Klik hiertoe extern bureaublad naar de Site B. openen SQL Plus venster omhoog en uitvoeren:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Het invoegen van een nieuwe record op de Site B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Extern bureaublad tot Site A en controleer of de replicatie heeft plaatsgevonden:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Aanvullende informatie
[Oracle VM afbeeldingen voor Azure](virtual-machines-linux-classic-oracle-images.md)
