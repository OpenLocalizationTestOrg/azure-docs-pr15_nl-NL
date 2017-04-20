<properties
    pageTitle="Beveiliging van Oracle-gegevens configureren in VMs | Microsoft Azure"
    description="Een zelfstudie voor het instellen en implementeren van Oracle gegevens beveiliging op Azure virtuele machines voor hoge beschikbaarheid herstel stapsgewijs."
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

#<a name="configuring-oracle-data-guard-for-azure"></a>Beveiliging van Oracle-gegevens voor Azure configureren


Deze zelfstudie wordt getoond hoe instellen en implementeren van Oracle gegevens beveiliging in Azure virtuele Machines omgeving voor beschikbaarheid en herstel. De zelfstudie is gericht op één richting replicatie voor RAC Oracle-databases.

Oracle gegevens beveiliging ondersteunt gegevensbescherming en herstel voor Oracle-Database. Het is een eenvoudig, krachtige, onverwachte oplossing voor herstel, gegevensbescherming en beschikbaarheid voor de hele Oracle-database.

Deze zelfstudie wordt ervan uitgegaan dat u al ervaring hebt met theoretisch en praktisch op beschikbaarheid Oracle-Database en herstel concepten. Zie de [Oracle-website](http://www.oracle.com/technetwork/database/features/availability/index.html) en ook de [Oracle-Gegevensconcepten beveiliging en beheer van de handleiding](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm)voor informatie.

Bovendien de zelfstudie wordt ervan uitgegaan dat u de volgende vereisten al hebt geïmplementeerd:

- U hebt de sectie beschikbaarheid en aandachtspunten voor noodgevallen herstel in het onderwerp van de [afbeeldingen van VM Oracle - diverse overwegingen](virtual-machines-windows-classic-oracle-considerations.md) al bekeken. Azure ondersteunt momenteel zelfstandige Oracle-Database exemplaren, maar niet Oracle Real Application Clusters (Oracle RAC).


- U kunt twee virtuele Machines (VMs) hebt gemaakt in Azure wordt aangegeven met hetzelfde platform opgegeven Oracle Enterprise Edition afbeelding. Zorg ervoor dat de virtuele Machines zijn in de [dezelfde cloudservice](virtual-machines-windows-load-balance.md) en in hetzelfde virtuele netwerk om ervoor te zorgen dat ze toegang tot elkaar via het permanente privé IP-adres. Bovendien wordt het aanbevolen de VMs om in te plaatsen de dezelfde [beschikbaarheid instellen](virtual-machines-windows-manage-availability.md) mogen Azure voor deze plaatsen in afzonderlijke foutenstructuuranalyse domeinen en het upgraden van domeinen. Oracle gegevens beveiliging is alleen beschikbaar met Oracle-Database Enterprise Edition. Elke computer moet ten minste 2 GB geheugen en 5 GB schijfruimte hebben. Zie voor de meest recente informatie over het platform opgegeven VM formaten, [VM grootten voor Azure](virtual-machines-windows-sizes.md). Als u het volume van de schijfruimte voor uw VMs nodig hebt, kunt u extra schijven koppelen. Zie [het toevoegen van een schijf gegevens aan een virtuele Machine](virtual-machines-windows-classic-attach-disk.md)voor informatie.



- U hebt de namen VM als "Machine1" instellen voor de primaire VM en "Machine2" voor stand-by VM bij de portal van Azure klassieke.

- U hebt ingesteld dat de omgevingsvariabele **ORACLE_HOME** zodat deze verwijzen naar dezelfde oracle installatie pad naar de hoofdmap in de primaire en stand-by virtuele Machines, zoals `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- U zich aanmeldt bij uw Windows-server als een lid van de groep **Administrators** of als lid van de groep **ORA_DBA** .

In deze zelfstudie zal u het volgende doen:

De fysieke stand-by database-omgeving implementeren

1. Een primaire-database maken

2. De primaire database voorbereiden voor stand-by database maken

    1. U wordt gedwongen logboekregistratie inschakelen

    2. Een wachtwoordbestand maken

    3. Een logboek stand-by opnieuw configureren

    4. Archivering inschakelen

    5. Primaire database initialisatieparameters instellen

Een fysieke stand-by database maken

1. Een bestand van de parameter initialisatie voor stand-by database voorbereiden

2. De luisteraar ervan af en tnsnames ter ondersteuning van de database op primaire en stand-by computers configureren

    1. Listener.ora op beide servers vermeldingen voor beide databases hebben voor configureren

    2. Als u wilt posten voor primaire en stand-by databases in wachtstand, tnsnames.ora op de primaire en stand-by virtuele Machines te configureren. 

    3. Start de luisteraar ervan af en controleer van tnsping op beide virtuele Machines naar beide services.

3. De stand-by exemplaar nomount status opstart

4. Gebruik RMAN kopie van de database en een stand-by database maken

5. De fysieke stand-by database in de herstelmodus voor beheerde starten

6. Controleer of de fysieke stand-by database

> [AZURE.IMPORTANT] Deze zelfstudie is instellen en de volgende hardware en software-configuratie getest:
>
>|                      | **Primaire Database**                      | **Stand-by Database**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle-Release**   | Oracle11g Enterprise-versie (11.2.0.4.0) | Oracle11g Enterprise-versie (11.2.0.4.0) |
>| **De computernaam van de**     | Machine1                                  | Machine2                                  |
>| **Besturingssysteem** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle beveiligings-id**       | TEST                                      | TEST\_STBY                                |
>| **Geheugen**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Schijfruimte**       | Min 5 GB                                  | Min 5 GB                                  |

Voor latere versies van Oracle-Database en beveiliging voor Oracle-gegevens, is het mogelijk dat er een enkele aanvullende wijzigingen die u wilt implementeren. Zie de documentatie bij de [Beveiliging van gegevens](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [Oracle-Database](http://www.oracle.com/us/corporate/features/database-12c/index.html) op Oracle-website voor de meest recente versie-specifieke informatie.

##<a name="implement-the-physical-standby-database-environment"></a>De fysieke stand-by database-omgeving implementeren
### <a name="1-create-a-primary-database"></a>1. een primaire-database maken

- Een primaire database maken 'Testen' in de primaire virtuele Machine. Zie voor informatie maken en configureren van een Oracle-Database.
- Als u wilt zien van de naam van uw database, verbinding maken met uw database als de gebruiker SYS met SYSDBA rol in de SQL * Plus opdrachtprompt en uitvoeren met de volgende instructie:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Query vervolgens de namen van de databasebestanden vanuit de weergave van de systeem dba_data_files:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. de hoofddatabase voorbereiden voor stand-by database maken

Het wordt aanbevolen dat u het ervoor zorgen dat de hoofddatabase juist is geconfigureerd voor het maken van een stand-by database. Hier volgt een lijst met de stappen die u nodig hebt om uit te voeren:

1. U wordt gedwongen logboekregistratie inschakelen
2. Een wachtwoordbestand maken
3. Een logboek stand-by opnieuw configureren
4. Archivering inschakelen
5. Primaire database initialisatieparameters instellen

#### <a name="enable-forced-logging"></a>U wordt gedwongen logboekregistratie inschakelen

Implementatie van een Database stand-by, moeten we u wordt gedwongen logboekregistratie inschakelen in de primaire-database. Deze optie zorgt ervoor dat zelfs als een 'nologging' bewerking is voltooid, voorrang dwingen logboekregistratie en alle bewerkingen bent aangemeld bij de logboeken/opnieuw in. Daarom zorg we dat alles in de primaire database is aangemeld en replicatie op de stand-by alle bewerkingen in de primaire database omvat. Als u logboekregistratie dwingen, voert u de instructie alter database:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Een wachtwoordbestand maken

Om te kunnen verzenden en gearchiveerde logboeken van de primaire server toepassen op de stand-by-server, moet het wachtwoord sys identieke op primaire en stand-by servers. Daarom heb ik u maakt een wachtwoordbestand in de primaire database en kopieer deze naar de stand-by-server.

>[AZURE.IMPORTANT] Wanneer u een Oracle-Database 12c gebruikt, is er een nieuwe gebruiker, **SYSDG**, waarin u gebruiken kunt voor het beheer van de beveiliging van Oracle-gegevens. Zie [wijzigingen in Oracle-Database 12 c Release](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404)voor meer informatie.

Bovendien Zorg ervoor dat de ORACLE\_start-omgeving is al in Machine1 gedefinieerd. Zo niet, kunt u deze als een omgevingsvariabele via het dialoogvenster omgevingsvariabelen definiëren. Voor toegang tot dit dialoogvenster, start u **het hulpprogramma** door te dubbelklikken op het pictogram systeem in het **Configuratiescherm**. vervolgens klikt u op het tabblad **Geavanceerd** en kies **Omgevingsvariabelen**. Als u wilt de omgevingsvariabelen instellen, klikt u op de knop **Nieuw** onder **Variabelen**. Nadat u de omgevingsvariabelen ingesteld, sluit u de bestaande opdrachtprompt van Windows en open een nieuwe record.

Voer de volgende instructie om te schakelen naar de Oracle\_start adreslijst, zoals C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\database.

    cd %ORACLE_HOME%\database

Maak vervolgens een wachtwoordbestand met het hulpprogramma voor wachtwoord bestand maken, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). In dezelfde Windows opdrachtprompt in Machine1, voert u de volgende opdracht uit door de wachtwoordwaarde als het wachtwoord van **SYS**:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Deze opdracht Hiermee maakt u een wachtwoordbestand, met de naam als PWDTEST.ora, in de ORACLE\_HOME\\de database met adreslijstservices. U moet dit bestand kopiëren naar % ORACLE\_Start %\\directory in Machine2 handmatig database.

#### <a name="configure-a-standby-redo-log"></a>Een logboek stand-by opnieuw configureren

Vervolgens moet u een stand-by opnieuw logboek zodanig configureren dat de primaire het opnieuw correct ontvangen kan wanneer deze verandert in een stand-by. Vooraf hier maakt, kunt ook de stand-by opnieuw logboeken op de stand-by automatisch wordt gemaakt. Het is belangrijk is voor het configureren van de stand-by opnieuw Logboeken (SRL) met dezelfde afmetingen als de online logboeken opnieuw uitvoeren. De grootte van de huidige logboekbestanden van stand-by opnieuw moet precies overeenkomen met de grootte van de huidige primaire database online opnieuw logboekbestanden.

Voer de volgende instructie in de SQL\*PLUS opdrachtprompt in Machine1. Het logboekbestand van v$ is de weergave van een systeem met informatie over de logboekbestanden/opnieuw in.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Houd er rekening mee dat 52428800 50 MB is.

Vervolgens in de SQL\*Plus venster uitvoeren met de volgende instructies om een nieuwe groep van stand-by opnieuw log bestand toevoegen aan een stand-by database en geef een nummer waarmee de groep met de groep-component. Gebruik getallen groep kunt aanbrengen stand-by opnieuw log bestandsgroepen eenvoudiger te beheren:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Voer de weergave van de volgende systeem naar de lijst met informatie over opnieuw logboekbestanden. Deze bewerking wordt ook gecontroleerd of de stand-by opnieuw log bestandsgroepen zijn gemaakt:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Archivering inschakelen

Schakel vervolgens archivering door de volgende instructies als u wilt de hoofddatabase in ARCHIVELOG modus hebt opgeslagen en automatische archivering inschakelen uit te voeren. Door de database koppelen en klikt u vervolgens de opdracht archivelog wordt uitgevoerd, kunt u archief log-modus inschakelen.

Meld u eerst aan als sysdba. Voer in de opdrachtprompt van Windows:

    sqlplus /nolog

    connect / as sysdba

Klik op afsluiten de database in de SQL\*Plus de opdrachtprompt:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

De opdracht voor het koppelen van opstarten om te koppelen van de database vervolgens uitgevoerd. Dit zorgt ervoor dat Oracle worden gekoppeld aan het exemplaar van de opgegeven database.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Voer vervolgens uit:

    SQL> alter database archivelog;
    Database altered.

Voert u de Alter database, instructie met de component openen de database te maken voor normaal gebruik beschikbaar:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Primaire database initialisatieparameters instellen

Als u wilt configureren de beveiliging van de gegevens, die u wilt maken en configureren van de stand-by parameters op een gewone pfile (initialisatie parameter tekstbestand) eerste. Wanneer de pfile klaar is, moet u deze converteren naar een parameter serverbestand (SPFILE).

U kunt de gegevens beveiliging-omgeving met de parameters in de Initialisatie bepalen. ORA-bestand. Wanneer deze zelfstudie te volgen, moet u de primaire-Initialisatie van de database bijwerken. ORA zodat deze beide rollen bevatten kan: primaire of stand-by.

    SQL> create pfile from spfile;
    File created.

Vervolgens moet u de pfile om toe te voegen van de stand-by parameters bewerken. Open de INITTEST hiervoor. ORA bestand in de locatie van % ORACLE\_Start %\\database. Vervolgens toevoegen de volgende beweringen naar het bestand INITTEST.ora. De naamgevingsconventie op voor uw Initialisatie. ORA-bestand is Initialisatie\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Het vorige instructie blok bevat drie belangrijke setup items:
-   **LOG_ARCHIVE_CONFIG...:** U definiëren de database unieke id's met deze instructie.
-   **LOG_ARCHIVE_DEST_1...:** U definiëren de locatie van de lokale archief-map met deze instructie. Het is raadzaam om u een nieuwe map voor het archiveren behoeften van uw database maken en de archieflocatie van de lokale deze instructie expliciet gebruiken in plaats van met behulp van Oracle standaard map % ORACLE_HOME%\database\archive opgeven.
-   **LOG_ARCHIVE_DEST_2... ... LGWR ASYNCHROON:** u een asynchroon log schrijver proces (LGWR) voor het verzamelen van gegevens opnieuw en deze verzenden naar stand-by bestemmingen definiëren. Hier geeft de DB_UNIQUE_NAME een unieke naam voor de database op de stand-by doelserver.

Zodra de nieuwe parameterbestand klaar is, moet u de spfile maken op basis van deze.

Eerste, afgesloten de database:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Voer opstarten nomount opdracht vervolgens als volgt uit:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Maak nu een spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Vervolgens wordt afgesloten de database:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

De opstartopdracht vervolgens gebruiken om een exemplaar te starten:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Een fysieke stand-by database maken
In deze sectie bevat informatie over de stappen die u in Machine2 uitvoeren moet voor het voorbereiden van de fysieke stand-by database.

Eerst moet u extern bureaublad naar Machine2 via de portal van Azure klassieke.

Maak vervolgens op de stand-by Server (Machine2), alle benodigde mappen voor de stand-by database, zoals C:\\\<YourLocalFolder\>\\TEST. Tijdens deze zelfstudie te volgen, zorg dat de mapstructuur overeenkomt met de mapstructuur op Machine1 dat alle benodigde bestanden, zoals controlfile, datafiles redologfiles, udump, bdump en cdump-bestanden. Daarnaast definiëren de ORACLE\_voor THUISGEBRUIK en ORACLE\_GRONDTAL omgevingsvariabelen in Machine2. Zo niet, kunt u deze als een omgevingsvariabele via het dialoogvenster omgevingsvariabelen definiëren. Voor toegang tot dit dialoogvenster, start u **het hulpprogramma** door te dubbelklikken op het pictogram systeem in het **Configuratiescherm**. vervolgens klikt u op het tabblad **Geavanceerd** en kies **Omgevingsvariabelen**. Als u wilt de omgevingsvariabelen instellen, klikt u op de knop **Nieuw** onder de **Variabelen**. Nadat u de omgevingsvariabelen ingesteld, moet u de bestaande opdrachtprompt van Windows sluiten en open een nieuw om de wijzigingen te bekijken.

Vervolgens als volgt te werk:

1. Een bestand van de parameter initialisatie voor stand-by database voorbereiden

2. De luisteraar ervan af en tnsnames ter ondersteuning van de database op primaire en stand-by computers configureren

    1. Listener.ora op beide servers vermeldingen voor beide databases hebben voor configureren

    2. Tnsnames.ora configureren op de primaire en stand-by virtuele Machines vermeldingen voor primaire en stand-by databases hebben voor

    3. Start de luisteraar ervan af en controleer van tnsping op beide virtuele Machines naar beide services.

3. De stand-by exemplaar nomount status opstart

4. Gebruik RMAN kopie van de database en een stand-by database maken

5. De fysieke stand-by database in de herstelmodus voor beheerde starten

6. Controleer of de fysieke stand-by database

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. een bestand van de parameter initialisatie voor stand-by database voorbereiden

In deze sectie wordt beschreven hoe voor het voorbereiden van een bestand initialisatie parameter voor de stand-by database. Hiervoor moet u eerst de INITTEST kopiëren. ORA bestand wordt gewijzigd van Machine 1 in Machine2 handmatig. U moet mogelijk om de INITTEST weer te geven. ORA bestand in de map % ORACLE\_Start %\\databasemap in beide computers. Wijzig het bestand INITTEST.ora in Machine2 in te stellen voor de stand-by rol zoals hieronder aangegeven:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Het vorige instructie blok bevat twee belangrijke setup items:

-   **\*. LOG_ARCHIVE_DEST_1:** moet u de map c:\OracleDatabase\TEST_STBY\archives handmatig in Machine2 maken.
-   **\*. LOG_ARCHIVE_DEST_2:** dit is een optionele stap. Stelt u dit zoals mogelijk nodig als de primaire machine onderhoud en de stand-by machine verandert in een primaire-database.

Vervolgens moet u om de stand-by exemplaar te starten. Klik op de stand-by database-server, voer de volgende opdracht opdrachtprompt van Windows een Oracle-exemplaar maken met het maken van een Windows-service:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

De opdracht **Oradim** Hiermee maakt u een Oracle-exemplaar, maar deze niet gestart. U vindt deze in de C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\BIN-map.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>De luisteraar ervan af en tnsnames ter ondersteuning van de database op primaire en stand-by computers configureren
Voordat u een stand-by database maakt, moet u om ervoor te zorgen dat de primaire en stand-by databases in uw configuratie met elkaar kunnen praten. Hiervoor moet u zowel de luisteraar ervan af en de TNSNames configureren handmatig of met behulp van de configuratie netwerkhulpprogramma NETCA. Dit is een verplicht taak wanneer u het hulpprogramma herstel Manager (RMAN) gebruiken.

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Listener.ora op beide servers vermeldingen voor beide databases hebben voor configureren

Extern bureaublad Machine1 te bewerken dat het bestand listener.ora als hieronder. Wanneer u het bestand listener.ora bewerkt, Zorg altijd dat de begin- en haakje sluiten in dezelfde kolom uitlijnen. U kunt het bestand listener.ora vinden in de volgende map: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\netwerk\\beheerder\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Volgende, remote desktop naar Machine2 en bewerken die de listener.ora bestand als volgt: # listener.ora netwerk configuratiebestand: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Tnsnames.ora configureren op de primaire en stand-by virtuele Machines vermeldingen voor primaire en stand-by databases hebben voor

Extern bureaublad Machine1 te bewerken dat het bestand tnsnames.ora als hieronder. U kunt het bestand tnsnames.ora vinden in de volgende map: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\netwerk\\beheerder\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Extern bureaublad te Machine2 en bewerken de tnsnames.ora als volgt:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Start de luisteraar ervan af en controleer van tnsping op beide virtuele Machines naar beide services.

Opent u een nieuwe opdrachtprompt van Windows in zowel primaire als stand-by virtuele Machines en uitvoeren van de volgende instructies:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>De stand-by exemplaar nomount status opstart
Voor het instellen van de omgeving ter ondersteuning van de stand-by database op de stand-by virtuele Machine (MACHINE2).

Eerst het wachtwoordbestand van de primaire computer (Machine1) naar de stand-by machine (Machine2) handmatig kopiëren. Dit is nodig als het wachtwoord **sys** identiek op beide computers zijn moet.

Klik, open de opdrachtprompt van Windows in Machine2 en configuratie van de omgevingsvariabelen zodat deze verwijzen naar de stand-by database als volgt:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Vervolgens de stand-by-database in nomount staat starten en vervolgens een spfile te genereren.

De database starten:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Gebruik RMAN kopie van de database en een stand-by database maken
U kunt het hulpprogramma herstel Manager (RMAN) maken van een back-up van de primaire database om de fysieke stand-by database te maken.

Extern bureaublad naar de stand-by VM (MACHINE2) en -klaar het hulpprogramma RMAN door op te geven van een volledige verbinding tekenreeks voor zowel het doel (primaire database, Machine1) en HULPAPPARAAT (stand-by database, Machine2) exemplaren.

>[AZURE.IMPORTANT] Gebruik de verificatie van het besturingssysteem niet als er nog geen database in de stand-by servercomputer.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>De fysieke stand-by database in de herstelmodus voor beheerde starten
Deze zelfstudie laat zien hoe een fysieke stand-by database maken. Zie de Oracle-documentatie voor informatie over het maken van een logische stand-by database.

Open omhoog SQL\*Plus opdrachtprompt en de beveiliging van de gegevens op de stand-by VM of de server (MACHINE2) als volgt inschakelen:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Als u de stand-by database opent in de modus **koppelen** het logboek archiveren verzending zich blijft voordoen en het beheerde herstelproces blijft log toepassen op de stand-by database. Dit zorgt ervoor dat de stand-by database bijgewerkt met de hoofddatabase blijft. Houd er rekening mee dat de stand-by database kan niet toegankelijk voor rapportage tijdens deze periode.

Wanneer u de stand-by database opent in de modus **Alleen-lezen** , het archief verzending melden zich blijft voordoen. Maar het beheerde herstelproces stopt. Hierdoor wordt de stand-by database te worden steeds verouderd totdat het beheerde herstelproces wordt hervat. U kunt de stand-by database openen voor rapportage tijdens deze periode maar gegevens mogelijk niet de meest recente wijzigingen doorgevoerd.

In het algemeen, is het raadzaam dat u de stand-by database in de modus **koppelen behouden** aan de gegevens in de stand-by database up-to-date houden als er een fout van de hoofddatabase. U kunt echter de stand-by database behouden in de modus **Alleen-lezen** voor rapportage afhankelijk van de vereisten van uw toepassing. De volgende stappen demonstreren het inschakelen van de beveiliging van de gegevens in de alleen-lezen-modus met SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Controleer of de fysieke stand-by database
In deze sectie wordt beschreven hoe om te controleren of de configuratie beschikbaarheid als beheerder.

Open omhoog SQL\*Plus opdrachtpromptvenster en selectievakje gearchiveerd opnieuw aanmelden op de stand-by virtuele Machine (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Open omhoog SQL * Plus opdrachtprompt-venster en de schakeloptie logboekbestanden op de primaire computer (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Controleer gearchiveerde opnieuw aanmelden op de stand-by virtuele Machine (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Controleren op eventuele ruimte op de stand-by virtuele Machine (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Een andere verificatiemethode kan worden foutherstel met de stand-by database en test vervolgens als is het mogelijk te failback in de primaire-database. Als u wilt activeren de stand-by database als een primaire database, gebruikt u de volgende beweringen:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Als u niet flashback op de oorspronkelijke primaire database hebt ingeschakeld, worden het wordt aanbevolen dat u de oorspronkelijke primaire database weghalen en opnieuw als een stand-by database maken.

Het is raadzaam om in te schakelen flashback-database op de primaire en de stand-by databases. Als een failover gebeurt, kunt u de primaire database geknipperd terug naar de tijd voordat u de overname en snel geconverteerd naar een stand-by database.

##<a name="additional-resources"></a>Aanvullende informatie
[Oracle VM afbeeldingen voor Azure](virtual-machines-windows-classic-oracle-images.md)
