<properties
    pageTitle="Op tijd gebaseerde Hadoop Oozie coördinator gebruiken in HDInsight | Microsoft Azure"
    description="Gebruik op tijd gebaseerde Hadoop Oozie coördinator in HDInsight, een groot gegevensservice. Informatie over het definiëren Oozie werkstromen en coördinatoren en taken."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Op tijd gebaseerde Oozie coördinator gebruiken met Hadoop in HDInsight definieert werkstromen en taken te coördineren

In dit artikel leert u hoe u werkstromen en coördinatoren definiëren en hoe u de coördinator taken, op basis van tijd activeren. Het is handig om te gaan door [Gebruik Oozie met HDInsight] [ hdinsight-use-oozie] voordat u dit artikel lezen. Naast het Oozie, kunt u ook taken met behulp van Azure gegevens Factory plannen. Meer informatie over Azure gegevens Factory, raadpleegt u [Gebruik varken en component met gegevens Factory](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] In dit artikel is vereist een cluster HDInsight op basis van Windows. Zie voor meer informatie over het gebruik van Oozie, met inbegrip van taken op basis van tijd op een cluster Linux gebaseerde [Gebruik Oozie met Hadoop definiëren en uitvoeren van een werkstroom op Linux gebaseerde HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Wat is Oozie

Apache Oozie is een werkstroom/afhankelijk-systeem die worden beheerd in Hadoop-taken. Het is geïntegreerd met de stapel Hadoop, en Hadoop taken voor Apache MapReduce, Apache varken Apache component en Apache Sqoop wordt ondersteund. Dit kan ook worden gebruikt om taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn te plannen.

De volgende afbeelding ziet u de werkstroom die u wilt implementeren:

![Werkstroomdiagram][img-workflow-diagram]

De werkstroom bevat twee acties:

1. Een actie component een HiveQL-script om het tellen van elk type log niveau in een logboekbestand log4j wordt uitgevoerd. Elke log log4j bestaat uit een reeks velden met een veld [LOGBOEKNIVEAU] als u wilt weergeven van het type en de ernst, bijvoorbeeld:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    De component script-uitvoer is vergelijkbaar met:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Zie voor meer informatie over component, [Gebruik component met HDInsight][hdinsight-use-hive].

2.  Een actie Sqoop Hiermee exporteert u de uitvoer van de actie HiveQL aan een tabel in een Azure SQL-database. Zie voor meer informatie over Sqoop, [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Zie voor ondersteunde Oozie versies op HDInsight clusters [Wat is er nieuw in de cluster versies geleverd door HDInsight?] [hdinsight-versions].


##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Een HDInsight cluster**. Zie voor informatie over het maken van een cluster HDInsight [HDInsight maken clusters][hdinsight-provision], of [aan de slag met HDInsight][hdinsight-get-started]. U moet de volgende gegevens via de zelfstudie:

    <table border = "1">
    <tr><th>Cluster eigenschap</th><th>Windows PowerShell-variabelennaam</th><th>Waarde</th><th>Beschrijving</th></tr>
    <tr><td>De naam van de cluster HDInsight</td><td>$clusterName</td><td></td><td>Het HDInsight cluster waarop u deze zelfstudie wordt uitgevoerd.</td></tr>
    <tr><td>HDInsight cluster gebruikersnaam</td><td>$clusterUsername</td><td></td><td>De naam van de gebruiker in de HDInsight cluster. </td></tr>
    <tr><td>HDInsight cluster gebruikerswachtwoord </td><td>$clusterPassword</td><td></td><td>Het wachtwoord van de gebruiker in de HDInsight cluster.</td></tr>
    <tr><td>Azure-opslagaccountnaam</td><td>$storageAccountName</td><td></td><td>Een opslag van Azure-account beschikbaar voor het cluster HDInsight. Gebruik het standaardaccount voor opslag die u hebt opgegeven tijdens het inrichten cluster voor deze zelfstudie.</td></tr>
    <tr><td>Naam van de container Azure Blob</td><td>$containerName</td><td></td><td>Gebruik de Azure Blob storage container die wordt gebruikt voor het bestandssysteem standaard HDInsight cluster in dit voorbeeld. Standaard heeft dezelfde naam als het cluster HDInsight.</td></tr>
    </table>

- **Een Azure SQL-database**. U kunt een firewallregel voor de SQL-databaseserver om toegang te krijgen vanuit uw werkstation moet configureren. Voor instructies over het maken van een Azure SQL database en configureren van de firewall, raadpleegt u [aan de slag met Azure SQL-database][sqldatabase-get-started]. In dit artikel bevat een Windows PowerShell-script voor het maken van de Azure SQL database-tabel die u nodig voor deze zelfstudie hebt.

    <table border = "1">
    <tr><th>SQL database-eigenschap</th><th>Windows PowerShell-variabelennaam</th><th>Waarde</th><th>Beschrijving</th></tr>
    <tr><td>De naam van een SQL-database-server</td><td>$sqlDatabaseServer</td><td></td><td>De SQL-databaseserver waarnaar Sqoop gegevens worden geëxporteerd. </td></tr>
    <tr><td>Aanmeldingsnaam voor SQL-database</td><td>$sqlDatabaseLogin</td><td></td><td>Aanmeldingsnaam van de SQL-Database.</td></tr>
    <tr><td>Wachtwoord voor aanmelden bij SQL-database</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Wachtwoord voor aanmelden bij SQL-Database.</td></tr>
    <tr><td>De naam van de SQL-database</td><td>$sqlDatabaseName</td><td></td><td>De SQL Azure-database waarnaar Sqoop gegevens worden geëxporteerd. </td></tr>
    </table>

    > [AZURE.NOTE] Een Azure SQL-database staat standaard verbindingen van Azure-Services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u deze in de Portal Azure inschakelen. Zie [maken en SQL-Database configureren]voor instructies over het maken van een SQL-Database en firewallregels configureren,[sqldatabase-get-started].


> [AZURE.NOTE] Fill-in de waarden in de tabellen. Dit is nuttig zijn voor deze zelfstudie doorlopen.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie werkstroom en de gerelateerde HiveQL script definiëren

Oozie werkstromen definities zijn in hPDL (een XML-proces definition language) geschreven. De standaardbestandsnaam van de werkstroom is *workflow.xml*.  U wordt de werkstroom voor het bestand lokaal opslaan en vervolgens op het cluster HDInsight toepassen met behulp van Azure PowerShell verderop in deze zelfstudie.

De actie component in de werkstroom roept een HiveQL script-bestand. Dit scriptbestand bevat drie HiveQL instructies:

1. **Het neerzetten TABLE, instructie** Hiermee verwijdert u de tabel van de component log4j indien aanwezig.
2. **De maken TABLE, instructie** Hiermee maakt u een externe tabel component van de log4j, die naar de locatie van het logboekbestand log4j verwijst;
3.  **De locatie van het logboekbestand log4j**. Het scheidingsteken voor velden is ",". Het scheidingsteken voor de regel is "\n". Een externe tabel component wordt gebruikt om te voorkomen dat het gegevensbestand die wordt verwijderd uit de oorspronkelijke locatie, voor het geval u wilt uitvoeren van de werkstroom Oozie meerdere keren.
3. **Het invoegen OVERSCHRIJVEN instructie** telt de exemplaren van elk type log niveau uit de tabel log4j-component en wordt opgeslagen in de uitvoer een opslaglocatie van Azure Blob.

**Opmerking**: Er is een bekend probleem van de component-pad. U wordt dit probleem ondervindt bij het verzenden van een taak Oozie. De instructies om het probleem op te lossen kunt u vinden op de TechNet-Wiki: [HDInsight component-fout: kan de naam van][technetwiki-hive-error].

**De HiveQL script-bestand moet worden gebeld door de werkstroom definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Er zijn drie variabelen in het script gebruikt:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Het bestand in de werkstroom definition Language (workflow.xml in deze zelfstudie) worden deze waarden doorgeven aan dit script HiveQL tijdens runtime.

2. Sla het bestand als **C:\Tutorials\UseOozie\useooziewf.hql** met behulp van ANSI (ASCII)-codering. (Kladblok gebruik als uw teksteditor deze optie niet zelf). Dit scriptbestand wordt geïmplementeerd in het cluster HDInsight verderop in deze zelfstudie.



**Een werkstroom definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Er zijn twee acties die zijn gedefinieerd in de werkstroom. De actie starten naar is *RunHiveScript*. Als de actie wordt uitgevoerd *OK*, is de volgende actie *RunSqoopExport*.

    De RunHiveScript heeft meerdere variabelen bevatten. U kunt de waarden worden doorgeven wanneer u de taak Oozie vanuit uw werkstation verstuurt met behulp van Azure PowerShell.

    <table border = "1">
    <tr><th>Werkstroomvariabelen</th><th>Beschrijving</th></tr>
    <tr><td>${jobTracker}</td><td>Geef de URL van het vastleggen van de taak Hadoop. Gebruik <strong>jobtrackerhost:9010</strong> op HDInsight cluster versie 3.0 en 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Geef de URL van het knooppunt Hadoop-naam. Gebruik van de standaard bestand systeem wasbs: / / adres, bijvoorbeeld <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Wachtrijnaam}</td><td>Hiermee geeft u de naam van de wachtrij die de taak om te worden verzonden. Gebruik de <strong>standaardwaarde</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Component actie variabele</th><th>Beschrijving</th></tr>
    <tr><td>${hiveDataFolder}</td><td>De bronmap voor de opdracht maken Componententabel.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>De uitvoermap voor de instructie invoegen OVERSCHRIJVEN.</td></tr>
    <tr><td>${hiveTableName}</td><td>De naam van de tabel component dat verwijst naar de log4j gegevensbestanden.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop actie variabele</th><th>Beschrijving</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL-Database-verbindingsreeks.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>De SQL Azure-database te tabel waar de gegevens worden geëxporteerd.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>De uitvoermap voor de instructie component invoegen OVERSCHRIJVEN. Dit is dezelfde map voor de export Sqoop (exporteren-dir).</td></tr>
    </table>

    Zie voor meer informatie over Oozie werkstroom en het gebruik van de werkstroomacties, [Apache Oozie 4.0 documentatie] [ apache-oozie-400] (voor HDInsight cluster versie 3.0) of [Apache Oozie 3.3.2 documentatie] [ apache-oozie-332] (voor HDInsight cluster versie 2.1).

2. Sla het bestand als **C:\Tutorials\UseOozie\workflow.xml** met behulp van ANSI (ASCII)-codering. (Kladblok gebruik als uw teksteditor deze optie niet zelf).

**Coördinator definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Er zijn vijf variabelen die worden gebruikt in het definitiebestand:

  	| Variabele          | Beschrijving |
  	| ------------------|------------ |
  	| ${coordFrequency} | Taak onderbreken tijd. De frequentie is altijd uitgedrukt in minuten. |
  	| ${coordStart}     | Begintijd van de taak. |
  	| ${coordEnd}       | De eindtijd van de taak. |
  	| ${coordTimezone}  | Oozie verwerkt coördinator taken in een vaste tijdzone met geen zomertijd (meestal weergegeven met behulp van UTC). Deze tijdzone wordt verwezen als de "Oozie verwerking tijdzoneverschil." |
  	| ${wfPath}         | Het pad naar de workflow.xml.  Als de naam van de werkstroom-bestand is niet de standaardbestandsnaam (workflow.xml), moet u deze opgeven. |

2. Sla het bestand als **C:\Tutorials\UseOozie\coordinator.xml** met behulp van de ANSI (ASCII)-codering. (Kladblok gebruik als uw teksteditor deze optie niet zelf).

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Implementeer het project Oozie en de zelfstudie voorbereiden

U wordt een Azure PowerShell script uitvoeren om als volgt te werk:

- Kopieer het script HiveQL (useoozie.hql) naar Azure-blobopslag, wasbs:///tutorials/useoozie/useoozie.hql.
- Workflow.xml naar wasbs:///tutorials/useoozie/workflow.xml kopiëren.
- Coordinator.xml naar wasbs:///tutorials/useoozie/coordinator.xml kopiëren.
- Het gegevensbestand kopiëren (/ example/data/sample.log) naar wasbs:///tutorials/useoozie/data/sample.log.
- Maak een Azure SQL database-tabel voor het opslaan van gegevens voor Sqoop exporteren. De naam van de tabel is *log4jLogCount*.

**Meer informatie over HDInsight opslag**

Azure-blobopslag HDInsight gebruikt voor gegevensopslag. wasbs: / / is Microsoft implementatie van het Hadoop distributed file system (HDFS) in Azure-blobopslag. Zie voor meer informatie [Gebruik Azure-blobopslag met HDInsight][hdinsight-storage].

Wanneer u een cluster HDInsight inrichten, is een Azure Blob storage-account en een specifieke container uit dat account aangewezen als het standaard bestandssysteem worden gemaakt, zoals in HDFS. Naast deze opslag-account, kunt u extra opslagruimte accounts toevoegen vanuit hetzelfde Azure-abonnement of andere Azure abonnementen tijdens het inrichten. Zie voor instructies over het toevoegen van extra opslagruimte-accounts [inrichten HDInsight clusters][hdinsight-provision]. Om te vereenvoudigen het Azure PowerShell-script in deze zelfstudie gebruikt, worden alle bestanden opgeslagen in de container standaard bestand system */tutorials/useoozie*zich bevindt. Standaard bevat deze container dezelfde naam als de naam van het cluster HDInsight.
De syntaxis van de luidt als volgt:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Alleen de *wasb: / /* syntaxis wordt ondersteund in HDInsight cluster versie 3.0. De oudere *luchtverzadigingswaarde: / /* syntaxis wordt ondersteund in HDInsight 2.1 en 1,6 clusters, maar deze niet wordt ondersteund in HDInsight 3.0 clusters.

> [AZURE.NOTE] De wasb: / / pad is virtual. Zie voor meer informatie [Gebruik Azure-blobopslag met HDInsight][hdinsight-storage].

Een bestand dat is opgeslagen in de container standaard bestand system zijn toegankelijk vanaf HDInsight via een van de volgende URI's (ik gebruik workflow.xml als voorbeeld):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Als u het bestand openen rechtstreeks vanuit het opslag-account wilt, wordt de naam van de blob voor het bestand is:

    tutorials/useoozie/workflow.xml

**Meer informatie over component interne en externe-tabellen**

Er zijn enkele dingen die u moet weten over component interne en externe tabellen:

- De opdracht tabel maken Hiermee maakt u een interne tabel, ook bekend als een beheerde. Het gegevensbestand moet zich bevinden in de standaardcontainer.
- De opdracht tabel maken het gegevensbestand verplaatst naar de /hive/warehouse/<TableName> map in de standaardcontainer.
- De opdracht externe tabel maken Hiermee maakt u een externe tabel. Het gegevensbestand kan zich bevinden buiten de standaardcontainer.
- De opdracht externe tabel maken het gegevensbestand niet wordt verplaatst.
- De opdracht externe tabel maken eventuele submappen onder de map die is opgegeven in de component locatie niet is toegestaan. Dit is de reden waarom de zelfstudie een kopie van het bestand sample.log wordt.

Zie voor meer informatie [HDInsight: component interne en externe tabellen inleiding][cindygross-hive-tables].

**De zelfstudie voorbereiden**

1. Open de Windows PowerShell-wissen ( **PowerShell_ISE**Typ in het scherm Start van Windows 8 en klik vervolgens op **Windows filter wissen**. Zie [Windows PowerShell starten in Windows 8 en Windows]voor meer informatie[powershell-start]).
2. Klik in het deelvenster onder Voer de volgende opdracht verbinding maken met uw Azure-abonnement:

        Add-AzureAccount

    U wordt gevraagd om uw Azure-account. Deze methode van het toevoegen van een abonnement verbinding treedt er een time-out en na 12 uur, moet u de cmdlet opnieuw uitvoeren.

    > [AZURE.NOTE] Als u meerdere Azure abonnementen hebt en de standaardabonnement is niet degene die u wilt gebruiken, gebruikt u de cmdlet <strong>Selecteer AzureSubscription</strong> om te selecteren van een abonnement.

3. Kopieer het script in het deelvenster script en stelt u de eerste zes variabelen:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Zie de sectie [voorwaarden](#prerequisites) in deze zelfstudie voor meer beschrijvingen van de variabelen.

3. De volgende manieren te werk toevoegen aan het script in het deelvenster script:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klik op **Script uitvoeren** of druk op **F5** om het script uitvoeren. De uitvoer is vergelijkbaar met:

    ![Voorbereiding van zelfstudie uitvoer][img-preparation-output]

##<a name="run-the-oozie-project"></a>Het project Oozie uitvoeren

Azure PowerShell momenteel kunt niet zelf een cmdlets voor het definiëren van Oozie taken. U kunt de cmdlet **Roep RestMethod** roepen Oozie-webservices. De Oozie web services-API is een HTTP REST JSON-API. Zie voor meer informatie over de webservices Oozie API [Apache Oozie 4.0 documentatie] [ apache-oozie-400] (voor HDInsight cluster versie 3.0) of [Apache Oozie 3.3.2 documentatie] [ apache-oozie-332] (voor HDInsight cluster versie 2.1).

**Een taak Oozie indienen**

1. Open de Windows PowerShell-wissen (Typ **PowerShell_ISE**in 8 startscherm van Windows, en klik vervolgens op **Windows filter wissen**. Zie [Windows PowerShell starten in Windows 8 en Windows]voor meer informatie[powershell-start]).

3. Kopieer het script in het deelvenster script en stel vervolgens de eerst 14 variabelen (echter **$storageUri**overslaan).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Zie de sectie [voorwaarden](#prerequisites) in deze zelfstudie voor meer beschrijvingen van de variabelen.

    $coordstart en $coordend zijn in de werkstroom begin- en eindtijd. Als u erachter komt de UTC/GMT tijd, wilt zoeken "utc-tijd" op bing.com. De $coordFrequency is hoe vaak in minuten die u wilt uitvoeren van de werkstroom.

3. Toevoegen aan de volgende handelingen uit om het script. In dit gedeelte worden de nettolading Oozie gedefinieerd:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Het belangrijkste verschil vergeleken met de werkstroom nettolading opdrachtbestand is de variabele **oozie.coord.application.path**. Wanneer u een werkstroomtaak indient, gebruikt u **oozie.wf.application.path** in plaats daarvan.

4. Toevoegen aan de volgende handelingen uit om het script. In dit onderdeel controleert de servicestatus van Oozie web:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Toevoegen aan de volgende handelingen uit om het script. In dit gedeelte maakt u een taak Oozie:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Wanneer u een werkstroomtaak indient, moet u een andere webservice belt u bij het starten van de taak nadat de taak is gemaakt. In dit geval wordt de taak coördinator door tijd geactiveerd. De taak wordt automatisch gestart.

6. Toevoegen aan de volgende handelingen uit om het script. In dit onderdeel controleert de taakstatus Oozie:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Optioneel) Toevoegen aan de volgende handelingen uit om het script.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. De volgende manieren te werk toevoegen aan het script:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    De # positief of negatief verwijderen als u wilt uitvoeren van de aanvullende functies.

7. Als uw cluster HDinsight versie 2.1 is, vervangt u 'https://$clusterName.azurehdinsight.net:443/oozie/v2/' met 'https://$clusterName.azurehdinsight.net:443/oozie/v1/'. HDInsight cluster versie 2.1 wordt niet ondersteunt versie 2 van de webservices.

7. Klik op **Script uitvoeren** of druk op **F5** om het script uitvoeren. De uitvoer is vergelijkbaar met:

    ![Zelfstudie werkstroom uitvoer uitvoeren][img-runworkflow-output]

8. Verbinding maken met uw SQL-Database om de geëxporteerde gegevens weer te geven.

**Het foutenlogboek van de taak controleren**

Om op te lossen een werkstroom, kan het logboekbestand Oozie worden gevonden op C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log uit de headnode cluster. Zie voor informatie over RDP, [HDInsight beheren clusters met behulp van de Azure portal][hdinsight-admin-portal].

**De zelfstudie opnieuw moet worden uitgevoerd**

Als u wilt herhalen de werkstroom, moet u de volgende taken uitvoeren:

- Verwijder het bestand component script uitvoer.
- De gegevens in de tabel log4jLogsCount verwijderen.

Hier volgt een voorbeeld Windows PowerShell-script die u kunt gebruiken:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd het definiëren van een werkstroom Oozie en een coördinator Oozie en hoe u een Oozie coördinator taak uitvoeren via Azure PowerShell. Meer informatie raadpleegt u de volgende artikelen:

- [Aan de slag met HDInsight][hdinsight-get-started]
- [Azure-blobopslag gebruiken met HDInsight][hdinsight-storage]
- [HDInsight beheren met behulp van Azure PowerShell][hdinsight-admin-powershell]
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
- [Component gebruiken met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [Ontwikkel Java MapReduce-programma's voor HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
