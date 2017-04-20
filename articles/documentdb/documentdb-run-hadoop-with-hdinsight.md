<properties
    pageTitle="Een Hadoop-taak met DocumentDB en HDInsight uitvoeren | Microsoft Azure"
    description="Leer hoe u een eenvoudige component varken en MapReduce taak uitvoeren met DocumentDB en Azure HDInsight."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Een Hadoop-taak met DocumentDB en HDInsight uitvoeren

Deze zelfstudie leert u hoe u het uitvoeren van [Apache component][apache-hive], [Apache varken][apache-pig], en [Apache Hadoop] [ apache-hadoop] MapReduce taken op Azure HDInsight met DocumentDB van Hadoop verbindingslijn. De DocumentDB Hadoop verbindingslijn kunt DocumentDB fungeert als een bron- en sink voor component, varken en MapReduce taken. Deze zelfstudie, DocumentDB gebruikt als de gegevensbron en de waarde van doel voor Hadoop-taken.

Na het voltooien van deze zelfstudie, kunt u wel de volgende vragen beantwoorden:

- Hoe laad ik gegevens van DocumentDB behulp van een taak component, varken of MapReduce?
- Hoe kan ik gegevens opslaan in DocumentDB behulp van een taak component, varken of MapReduce?

Het is raadzaam om aan de slag door de volgende video, waarbij we uitvoeren via een component taak met DocumentDB en HDInsight te bekijken.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Vervolgens terug naar dit artikel, waar u ontvangt de volledige informatie over hoe u analytics taken op uw gegevens DocumentDB uitvoeren kunt.

> [AZURE.TIP] Deze zelfstudie wordt ervan uitgegaan dat u vorige ervaring hebt met Apache Hadoop, component en/of varken. Als u nog niet eerder Apache Hadoop, component en varken, raden u bezoekt de [Apache Hadoop documentatie][apache-hadoop-doc]. Deze zelfstudie wordt ervan uitgegaan dat u ervaring met DocumentDB en DocumentDB-account hebt. Als u nog niet eerder met DocumentDB of er geen een DocumentDB-account, check onze [Aan de slag] [ getting-started] pagina.

Geen tijd in beslag de zelfstudie hebt en alleen wilt krijgen van de volledige PowerShell-voorbeeldscripts voor component, varken en MapReduce? Geen probleem, deze [hier][documentdb-hdinsight-samples]. Het downloaden bevat ook de hql, varken en java-bestanden voor deze voorbeelden.

## <a name="NewestVersion"></a>Nieuwste versie

<table border='1'>
    <tr><th>Hadoop-Connector versie</th>
        <td>1.2.0</td></tr>
    <tr><th>Script-Uri</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Gewijzigd op</th>
        <td>04/26/2016</td></tr>
    <tr><th>Ondersteunde HDInsight versies</th>
        <td>3.1, 3,2</td></tr>
    <tr><th>Wijzigingenlogboek</th>
        <td>Bijgewerkte DocumentDB Java SDK naar 1.6.0</br>
            Ondersteuning voor gepartitioneerde verzamelingen als een bron- en sink</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Vereisten voor
Voordat u de instructies in deze zelfstudie te volgen, moet u zorgen dat u het volgende hebt:

- Een account DocumentDB, een database en een lijst met documenten in. Zie voor meer informatie [Aan de slag met DocumentDB][getting-started]. Voorbeeldgegevens importeren in uw account DocumentDB met de [DocumentDB importeren hulpmiddel][documentdb-import-data].
- Doorvoer. Gelezen en geschreven uit HDInsight richting van uw toegewezen aanvraag eenheden voor uw siteverzamelingen worden geteld. Zie voor meer informatie, [Provisioned doorvoer, verzoek eenheden, database of][documentdb-manage-throughput].
- Capaciteit voor een aanvullende opgeslagen procedure binnen elke uitvoer van siteverzameling. De opgeslagen procedures worden gebruikt voor het overbrengen van resulterende documenten. Zie voor meer informatie, [siteverzamelingen en ingerichte doorvoer][documentdb-manage-document-storage].
- Capaciteit voor de resulterende documenten uit de component, varken of MapReduce taken. Zie voor meer informatie [DocumentDB beheren capaciteit en prestaties][documentdb-manage-collections].
- [*Optionele*] Capaciteit voor een aanvullende siteverzameling. Zie voor meer informatie, [Provisioned document opslagcapaciteit en de index belasting][documentdb-manage-document-storage].

> [AZURE.WARNING] Om te voorkomen dat het maken van een nieuwe verzameling tijdens een van de taken, kunt u de resultaten naar stdout afdrukken, de uitvoer aan de container WASB opslaan of een bestaande siteverzameling opgeven. Nieuwe documenten binnen de siteverzameling worden gemaakt voor het opgeven van een bestaande siteverzameling, en al bestaande documenten alleen worden beïnvloed als er een conflict *-id's*. **De verbindingslijn bestaande documenten met ID-conflicten automatisch overschreven**. U kunt deze functie uitschakelen door de optie upsert ONWAAR. Als upsert ONWAAR is en een conflict optreedt, mislukt de taak Hadoop; een foutmelding id conflict.


## <a name="ProvisionHDInsight"></a>Stap 1: Maak een nieuw HDInsight cluster
Deze zelfstudie wordt scriptactie uit de Portal Azure gebruikt om aan te passen uw cluster HDInsight. In deze zelfstudie gebruiken we de Portal Azure uw cluster HDInsight maken. Voor instructies over het gebruik van de PowerShell-cmdlets of de HDInsight .NET SDK, raadpleegt u de [HDInsight aanpassen clusters met de actie Script] [ hdinsight-custom-provision] artikel.

1. Meld u aan bij de [Portal van Azure][azure-portal].

2. Klik op **+ Nieuw** boven aan het linker navigatiegedeelte op Zoeken naar **HDInsight** op de bovenste balk op het nieuwe blad.

3. **HDInsight** gepubliceerd door **Microsoft** wordt weergegeven boven aan de resultaten. Klik erop en klik vervolgens op **maken**.

4. Klik op het nieuwe HDInsight Cluster blade maken, uw **Clusternaam** hebt ingevoerd en selecteer het **abonnement** dat u wilt inrichten van deze resource onder.

    <table border='1'>
        <tr><td>De naam van cluster</td><td>De naam van het cluster.<br/>
   DNS-naam moet beginnen en eindigen met een alfanumerieke tekens en streepjes kan bevatten.<br/>
   Het veld moet een tekenreeks tussen 3 en 63 tekens.</td></tr>
        <tr><td>De naam van abonnement</td>
            <td>Als u meer dan één Azure-abonnement hebt, selecteert u het abonnement waarop uw cluster HDInsight. </td></tr>
    </table>

5. Klik op **Cluster Type selecteren** en stel de eigenschappen van de volgende op de opgegeven waarden.

    <table border='1'>
        <tr><td>Clustertype</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Cluster laag</td><td><strong>Standaard</strong></td></tr>
        <tr><td>Besturingssysteem</td><td><strong>Windows</strong></td></tr>
        <tr><td>Versie</td><td>meest recente versie</td></tr>
    </table>

    Klik nu op **selecteren**.

    ![Geef Hadoop HDInsight eerste cluster details][image-customprovision-page1]

6. Klik op **referenties** voor het instellen van uw aanmelding en RAS-referenties. Kies uw **Cluster Login gebruikersnaam** en **wachtwoord voor aanmelden bij Cluster**.

    Als u externe in uw cluster wilt, selecteert u *Ja* onderaan in het blad en geef een gebruikersnaam en wachtwoord.

7. Klik op de **Gegevensbron** voor het instellen van uw hoofdlocatie bent voor toegang tot gegevens. Kies de gewenste **Methode te selecteren** en geef een al bestaande opslag-account of een nieuw account te maken.

8. Klik op het hetzelfde blad, Geef een **Container standaard** en een **locatie**. En klik op **selecteren**.

    > [AZURE.NOTE] Selecteer een locatie dicht bij uw regio DocumentDB account voor betere prestaties

8. Klik op **prijzen** om het getal en type knooppunten te selecteren. U kunt de standaardconfiguratie behouden en het aantal knooppunten werknemer later schalen.

9. Klik op **optionele configuratie** **scriptacties** in het blad optionele configuratie.

    Voer de volgende informatie om aan te passen uw cluster HDInsight in scriptacties.

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>Script URI</td>
            <td>Geef de URI aan het script die als u wilt aanpassen van het cluster wordt geactiveerd.</br></br>
   Geef op: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Hoofd</td>
            <td>Klik op het selectievakje in als u wilt de PowerShell-script naar het knooppunt hoofd uitvoeren.</br></br>
            <strong>Schakel dit selectievakje in</strong>.</td></tr>
        <tr><td>Werknemer</td>
            <td>Klik op het selectievakje in als u wilt uitvoeren van de PowerShell-script naar het knooppunt werknemer.</br></br>
            <strong>Schakel dit selectievakje in</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Klik op het selectievakje in als u wilt de PowerShell-script op de Zookeeper uitvoeren.</br></br>
            <strong>Niet nodig</strong>.
            </td></tr>
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script.</br></br>
            <strong>Geen Parameters nodig</strong>.</td></tr>
    </table>

10. Maak een nieuwe **Resourcegroep** of gebruik een bestaande resourcegroep onder uw Azure-abonnement.

11. Controleer nu **vastmaken aan dashboard** om te volgen de implementatie en klik op **maken**.

## <a name="InstallCmdlets"></a>Stap 2: Installeren en configureren van Azure PowerShell

1. Azure PowerShell installeren. Instructies vindt u [hier][powershell-install-configure].

    > [AZURE.NOTE] U kunt ook alleen betrekking heeft op component query's van HDInsight online component Editor gebruiken. Hiervoor moet u zich aanmelden bij de [Portal van Azure][azure-portal], klikt u op **HDInsight** in het linkerdeelvenster om weer te geven van een lijst met clusters HDInsight. Klik op het cluster die u component query's uitvoeren wilt op en klik vervolgens op **Query-Console**.

2. Open de Azure PowerShell geïntegreerde uitvoeren van scripts-omgeving:
    - Klik op een computer met Windows 8 of Windows Server 2012 of hoger, kunt u de ingebouwde zoekfunctie gebruiken. Vanuit het startscherm, typ **Filter wissen** en klikt u op **Enter**.
    - Gebruik het menu Start op een computer met een versie eerder dan Windows 8 of Windows Server 2012. Vanuit het menu Start, typ **opdrachtprompt** in het zoekvak en klik in de lijst met resultaten, klikt u op **opdrachtprompt**. In de opdrachtprompt typt u **powershell_ise** en klikt u op **Enter**.

3. Uw Azure-Account toevoegen.
    1. Klik in het deelvenster Console Typ **Toevoegen-AzureAccount** en klikt u op **Enter**.
    2. Typ in het e-mailadres dat is gekoppeld aan uw Azure abonnement en klik op **Doorgaan**.
    3. Typ in het wachtwoord voor uw Azure-abonnement.
    4. Klik op **aanmelden**.

4. Het volgende diagram worden de belangrijkste delen van uw Azure-omgeving voor het uitvoeren van scripts van PowerShell.

    ![Diagram voor Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Stap 3: Een component-taak met DocumentDB en HDInsight uitvoeren

> [AZURE.IMPORTANT] Alle variabelen die worden aangeduid met haken moeten worden ingevuld met de configuratieinstellingen.

1. Stel de volgende variabelen in de PowerShell-Script-veld.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Laten we beginnen het bouwen van de queryreeks. We een component-query waarmee alle documenten systeem gegenereerde tijdstempels (_ts) wordt schrijven en vervolgens winkels de resultaten weer in een nieuwe DocumentDB collectie unieke id's (_rid) uit een verzameling DocumentDB telt alle documenten door de minuut.</p>

    <p>Eerst een tabel component uit onze DocumentDB-verzameling maken. Het volgende codefragment toevoegen aan de PowerShell-Script deelvenster <strong>nadat</strong> het codefragment tussen #1. Zorg ervoor dat u de spaties.wissen optioneel DocumentDB.query parameter t onze documenten die u kunt alleen _ts en _rid.</p>

    > [AZURE.NOTE]**Naming DocumentDB.inputCollections is niet een fout.** Ja, we staan voor het toevoegen van meerdere siteverzamelingen als invoer: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Vervolgens een tabel component voor de collectie uitvoer maken. De eigenschappen van de uitvoer is de maand, dag, uur, minuut en het totale aantal exemplaren.

    > [AZURE.NOTE]**Nog nogmaals naming DocumentDB.outputCollections is niet een fout.** Ja, kunnen we meerdere verzamelingen toevoegen als een uitvoer: </br>
'*DocumentDB.outputCollections*'='*\<naam van de verzameling DocumentDB uitvoer 1\>*,*\<naam van de siteverzameling DocumentDB uitvoer 2\>*' </br> De namen van de siteverzameling worden gescheiden zonder spaties, met alleen een komma. </br></br>
Documenten zijn verdeeld round robin over meerdere siteverzamelingen. Een reeks documenten worden opgeslagen in een siteverzameling en klik vervolgens een tweede reeks documenten worden opgeslagen in de volgende siteverzameling, enzovoort.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Tot slot laten we de documenten per maand, dag, uren en minuten geregistreerd en de resultaten weer in de uitvoer invoegen component tabel.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Het codefragment van de volgende script als u wilt maken van de definitie van een component uit de vorige query toevoegen.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    U kunt ook de - bestand schakeloptie om op te geven van een HiveQL-scriptbestand op HDFS.

6. Het volgende fragment als u wilt de begintijd opslaan en verzenden van de taak component toevoegen.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Voeg de volgende moet wachten om door de component taak is voltooid.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. De volgende handelingen uit als u wilt afdrukken van de standaard uitvoer en de begin- en eindtijden toevoegen.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Voer** uw nieuwe script! **Klik op** de knop groene uitvoeren.

10. Controleer de resultaten. Meld u aan bij de [Portal van Azure][azure-portal].
    1. Klik in het deelvenster links op <strong>Bladeren</strong> . </br>
    2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het deelvenster bladeren. </br>
    3. Zoek en klik op <strong>DocumentDB-Accounts</strong>. </br>
    4. Zoek vervolgens uw <strong>DocumentDB-Account</strong>, en vervolgens <strong>DocumentDB Database</strong> en de <strong>DocumentDB siteverzameling</strong> die is gekoppeld aan de uitvoer-verzameling in uw query component opgegeven.</br>
    5. Klik tot slot op <strong>Document Explorer</strong> onder <strong>Speciale tools voor ontwikkelaars</strong>.</br></p>

    Hier ziet u de resultaten van uw query component.

    ![Component queryresultaten][image-hive-query-results]

## <a name="RunPig"></a>Stap 4: Een varken-taak met DocumentDB en HDInsight uitvoeren

> [AZURE.IMPORTANT] Alle variabelen die worden aangeduid met haken moeten worden ingevuld met de configuratieinstellingen.

1. Stel de volgende variabelen in de PowerShell-Script-veld.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Laten we beginnen het bouwen van de queryreeks. We een varken-query waarmee alle documenten systeem gegenereerde tijdstempels (_ts) wordt schrijven en vervolgens winkels de resultaten weer in een nieuwe DocumentDB collectie unieke id's (_rid) uit een verzameling DocumentDB telt alle documenten door de minuut.</p>
    <p>Laden eerst documenten uit DocumentDB in HDInsight. Het volgende codefragment toevoegen aan de PowerShell-Script deelvenster <strong>nadat</strong> het codefragment tussen #1. Zorg ervoor dat een query DocumentDB toevoegen aan de optioneel DocumentDB queryparameter knippen van onze documenten die u kunt alleen _ts en _rid.</p>

    > [AZURE.NOTE]Ja, we staan voor het toevoegen van meerdere siteverzamelingen als invoer: </br>
'*\<Naam van de verzameling DocumentDB invoer 1\>*,*\<naam van de siteverzameling DocumentDB invoer 2\>*'</br> De namen van de siteverzameling worden gescheiden zonder spaties, met alleen een komma. </b>

    Documenten zijn verdeeld round robin over meerdere siteverzamelingen. Een reeks documenten worden opgeslagen in een siteverzameling en klik vervolgens een tweede reeks documenten worden opgeslagen in de volgende siteverzameling, enzovoort.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Vervolgens geregistreerd laten we de documenten door de maand, dag, uur, minuut en het totale aantal exemplaren.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Laten we opslaan ten slotte de resultaten in onze nieuwe uitvoer-verzameling.

    > [AZURE.NOTE]Ja, kunnen we meerdere verzamelingen toevoegen als een uitvoer: </br>
'\<Naam van de verzameling DocumentDB uitvoer 1\>,\<naam van de siteverzameling DocumentDB uitvoer 2\>'</br> De namen van de siteverzameling worden gescheiden zonder spaties, met alleen een komma.</br>
Documenten zijn verdeeld round robin over de meerdere siteverzamelingen. Een reeks documenten worden opgeslagen in een siteverzameling en klik vervolgens een tweede reeks documenten worden opgeslagen in de volgende siteverzameling, enzovoort.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Het codefragment van de volgende script als u wilt maken van de definitie van een varken uit de vorige query toevoegen.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    U kunt ook de - bestand schakeloptie om op te geven van een scriptbestand varken op HDFS.

6. Het volgende fragment als u wilt de begintijd opslaan en verzenden van de taak varken toevoegen.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Voeg de volgende moet wachten om door de varken taak is voltooid.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. De volgende handelingen uit als u wilt afdrukken van de standaard uitvoer en de begin- en eindtijden toevoegen.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Voer** uw nieuwe script! **Klik op** de knop groene uitvoeren.

10. Controleer de resultaten. Meld u aan bij de [Portal van Azure][azure-portal].
    1. Klik in het deelvenster links op <strong>Bladeren</strong> . </br>
    2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het deelvenster bladeren. </br>
    3. Zoek en klik op <strong>DocumentDB-Accounts</strong>. </br>
    4. Zoek vervolgens uw <strong>DocumentDB-Account</strong>, en vervolgens <strong>DocumentDB Database</strong> en de <strong>DocumentDB siteverzameling</strong> die is gekoppeld aan de collectie uitvoer is opgegeven in uw query varken.</br>
    5. Klik tot slot op <strong>Document Explorer</strong> onder <strong>Speciale tools voor ontwikkelaars</strong>.</br></p>

    Hier ziet u de resultaten van uw query varken.

    ![Varken queryresultaten][image-pig-query-results]

## <a name="RunMapReduce"></a>Stap 5: Een MapReduce-taak met DocumentDB en HDInsight uitvoeren

1. Stel de volgende variabelen in de PowerShell-Script-veld.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. We wordt een MapReduce-taak die het aantal terugkerende vergaderingen voor elke documenteigenschap uit uw verzameling DocumentDB telt uitgevoerd. In dit script fragment **nadat** het codefragment van de bovenstaande toevoegen.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties-v01.jar wordt geleverd met de aangepaste installatie van de DocumentDB Hadoop verbindingslijn.

3. De volgende opdracht uit om in te dienen de taak MapReduce toevoegen.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Naast de definitie van de MapReduce, moet u ook de naam van het HDInsight-cluster waar u wilt uitvoeren van de taak MapReduce en de referenties opgeven. De begin-AzureHDInsightJob is een asynchrone uitvoering te starten. Als u wilt controleren of de voltooiing van de taak, gebruikt u de cmdlet *Wachten-AzureHDInsightJob* .

4. De volgende opdracht uit om te controleren van eventuele fouten bij de taak MapReduce actieve toevoegen.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Voer** uw nieuwe script! **Klik op** de knop groene uitvoeren.

6. Controleer de resultaten. Meld u aan bij de [Portal van Azure][azure-portal].
    1. Klik in het deelvenster links op <strong>Bladeren</strong> .
    2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het deelvenster bladeren.
    3. Zoek en klik op <strong>DocumentDB-Accounts</strong>.
    4. Zoek vervolgens uw <strong>DocumentDB-Account</strong>, en vervolgens <strong>DocumentDB Database</strong> en de <strong>DocumentDB siteverzameling</strong> die is gekoppeld aan de uitvoer-verzameling in uw taak MapReduce opgegeven.
    5. Klik tot slot op <strong>Document Explorer</strong> onder <strong>Speciale tools voor ontwikkelaars</strong>.

    Hier ziet u de resultaten van uw werk MapReduce.

    ![De queryresultaten MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Volgende stappen

Gefeliciteerd! U zojuist hebt uitgevoerd uw eerste component varken en MapReduce taken met Azure DocumentDB en HDInsight.

We hebben onze Hadoop-Connector die afkomstig zijn openen. Als u geïnteresseerd bent, kunt u bijdragen op [GitHub][documentdb-github].

Meer informatie raadpleegt u de volgende artikelen:

- [Een Java-toepassing met Documentdb ontwikkelen][documentdb-java-application]
- [Ontwikkel Java MapReduce-programma's voor Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Aan de slag met Hadoop met onderdeel in HDInsight te analyseren mobiele telefoon gebruiken][hdinsight-get-started]
- [MapReduce gebruiken met HDInsight][hdinsight-use-mapreduce]
- [Component gebruiken met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [HDInsight clusters met de actie Script aanpassen][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
