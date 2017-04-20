<properties
    pageTitle="Maken en gegevens laadt in component tabellen vanuit blobopslag | Microsoft Azure"
    description="Component tabellen maken en gegevens in blob naar tabellen Component laden"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Maken en gegevens laadt in component tabellen vanuit Azure-blobopslag

In dit onderwerp vindt u algemene component-query's die component tabellen maken en gegevens van Azure-blobopslag laden. Enkele richtlijnen is ook beschikbaar op de component tabellen partitioneren en over het gebruik van de geoptimaliseerd rij kolommen (ORC) opmaak om queryprestaties te verbeteren.

In dit **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u het nemen van gegevens in de doel-omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, raadpleegt u [over Azure opslag-accounts](../storage/storage-create-storage-account.md). 
* Deze is ingericht een aangepaste Hadoop-cluster met de service HDInsight.  Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop clusters voor geavanceerde analyses](machine-learning-data-science-customize-hadoop-cluster.md).
* Enabled externe toegang tot het cluster, aangemeld en de opdrachtregel Hadoop-console geopend. Als u instructies nodig hebt, raadpleegt u [toegang tot het hoofd knooppunt van Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure-blobopslag
Als u een Azure virtuele machines gemaakt volgens de instructies in het [instellen van een Azure virtuele machines voor geavanceerde analyses](machine-learning-data-science-setup-virtual-machine.md), dit scriptbestand moet worden gedownload naar de *C:\\gebruikers\\\<gebruikersnaam in te voeren\>\\documenten\\gegevens wetenschappelijke Scripts* map op de virtuele machine. Deze component query's wordt alleen vereist dat u hebt opgegeven in uw eigen gegevensschema en configuratie van de Azure blob storage in de betreffende velden gereed voor indiening.

We ervan uit dat de gegevens voor component tabellen in een **niet-gecomprimeerde** tabelvorm en dat de gegevens zijn geüpload naar de standaard (of een extra) container van het opslag-account door het cluster Hadoop gebruikt.

Als u oefenen met het op de **Tevens Taxi reis gegevens wilt**, moet u:

- **download** de 24 [tevens Taxi reis](http://www.andresmh.com/nyctaxitrips) -gegevensbestanden (12 reis-bestanden en 12 tarief-bestanden)
- **pak** alle bestanden in de CSV-bestanden, en kiest u vervolgens
- **Upload** deze naar de standaard (of het juiste container) van de Azure opslag-account dat is gemaakt door de procedure in een overzicht in het onderwerp [aanpassen Azure HDInsight Hadoop clusters voor proces van geavanceerde analyses en technologie](machine-learning-data-science-customize-hadoop-cluster.md) . Het proces voor het CSV-bestanden uploaden naar de standaard-container voor de opslag-account kan worden gevonden op deze [pagina](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Het indienen van component query 's

Component query's kunnen worden verzonden met behulp van:

1. [Component query's via de opdrachtregel voor Hadoop in headnode van Hadoop cluster indienen](#headnode)
2. [Component query's met de Editor component indienen](#hive-editor)
3. [Component query's met Azure PowerShell-opdrachten indienen](#ps)

Component query's zijn SQL-achtige. Als u bekend met SQL bent, vindt u mogelijk de [component voor SQL gebruikers cheats blad](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) nuttig zijn.

Wanneer u een query component verzendt, kunt u ook de bestemming van de uitvoer van component query's bepalen, ongeacht of deze op het scherm of naar een lokaal bestand op het hoofd knooppunt of naar een Azure blob.


###<a name="headnode"></a>1. component query's via de opdrachtregel voor Hadoop in headnode van Hadoop cluster indienen

Als de query component complexe is, leidt verzendt, wordt deze rechtstreeks in het hoofd knooppunt van de Hadoop cluster meestal tot sneller schakelen rond dan u deze verzendt met een Editor voor de component of Azure PowerShell-scripts.

Meld u aan bij het hoofd knooppunt van de Hadoop-cluster, opent u de opdrachtregel Hadoop op het bureaublad van het hoofd knooppunt en voert u de opdracht `cd %hive_home%\bin`.

U hebt gebruikt om in te dienen component query's in de Hadoop-opdrachtregel op drie manieren:

* rechtstreeks
* .hql bestanden gebruiken
* met de component opdracht-console

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Component query's rechtstreeks in Hadoop opdrachtregel verzenden.

U kunt de opdracht zoals uitvoeren `hive -e "<your hive query>;` om in te dienen eenvoudige component query's rechtstreeks in Hadoop opdrachtregel. Hier volgt een voorbeeld, waarin de rood vak geeft een overzicht van de opdracht waarmee de component-query en het groene vak geeft een overzicht van de uitvoer van de query component.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Component query's in .hql bestanden verzenden

Als de query component ingewikkelder is meerdere lijnen, voor het bewerken van query's in de opdrachtregel of component opdrachtenconsole niet praktisch haalbaar is. Een alternatief is een teksteditor in het hoofd knooppunt van het cluster Hadoop via de component query's opslaan in een bestand .hql in een lokale map van het hoofd knooppunt. En vervolgens de component query in het bestand .hql kan worden verzonden met behulp van de `-f` argument als volgt:

    hive -f "<path to the .hql file>"

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Onderdrukken voortgang status scherm afdrukken van component query 's**

Standaard nadat component query is ingediend in Hadoop opdrachtregel wordt de voortgang van de Map/verkleinen taak afgedrukt op scherm. Als u wilt afdrukken van het scherm van de voortgang van de taak kaart/verkleinen onderdrukken, kunt u een argument `-S` ("S" in hoofdletters) in de opdracht regel als volgt:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Component query's in component opdrachtenconsole verzenden.

U kunt ook eerst de opdracht-console component invoeren door de opdracht uit te voeren `hive` in Hadoop opdrachtregel, en vervolgens dient component query's in component opdrachtenconsole. Hier volgt een voorbeeld. In dit voorbeeld Markeer de twee rode vakken de opdrachten voor het invoeren van de component opdracht-console en de component query ingediend respectievelijk in component opdrachtenconsole. Het groene vak wordt de uitvoer van de query component gemarkeerd.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

De voorgaande Column uitvoer rechtstreeks de queryresultaten component op het scherm. U kunt ook de uitvoer schrijven naar een lokaal bestand op het hoofd knooppunt of een Azure blob. Vervolgens kunt u andere hulpprogramma's voor verder analyseren, de uitvoer van component query's.

**Uitvoer component queryresultaten naar een lokaal bestand.**

Als u wilt de queryresultaten component naar een lokale map op het hoofd knooppunt uitvoert, moet u aan de component query in de Hadoop-opdrachtregel als volgt verzenden:

    hive -e "<hive query>" > <local path in the head node>

In het volgende voorbeeld wordt de uitvoer van component query in een bestand is geschreven `hivequeryoutput.txt` in directory `C:\apps\temp`.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Uitvoer component queryresultaten naar een Azure blob**

U kunt ook de queryresultaten component naar een Azure blob, in de standaardcontainer van het cluster Hadoop uitvoeren. De query component hiervoor is als volgt:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Klik in het volgende voorbeeld wordt de uitvoer van component query is geschreven naar een map blob `queryoutputdir` in de standaardcontainer van het cluster Hadoop. Hier, hoeft u alleen op te geven van de naam van de map, zonder de naam van de blob. Een fout gegenereerd als u zowel directory als blob namen, zoals `wasb:///queryoutputdir/queryoutput.txt`.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Als u de standaard-container van het Hadoop-cluster met Azure Opslagverkenner opent, kunt u de uitvoer van de component query zoals wordt weergegeven in de volgende afbeelding kunt zien. U kunt het filter (gemarkeerd met een rood vak) toepassen om alleen de blob met de opgegeven letters in namen ophalen.

![Werkruimte maken](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. component query's met de Editor component indienen

U kunt ook de Query-Console (Component Editor) door een URL van het formulier *https://&#60; De naam van de Hadoop-cluster >.azurehdinsight.net/Home/HiveEditor* in een webbrowser. U moet zijn aangemeld in de zien deze console en dus moet u uw Hadoop cluster referenties hier.

###<a name="ps"></a>3. indienen component query's met Azure PowerShell-opdrachten

U kunt ook PowerShell gebruiken om in te dienen component query's. Zie voor instructies voor het [indienen component taken via PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Component database en tabellen maken

De component query's zijn gedeeld in de [bibliotheek Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) en van daaruit kunnen worden gedownload.

Hier ziet u de component query die een tabel component maakt.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Hier volgen de beschrijvingen van de velden die u moet invoegtoepassing en andere configuraties:

- **& #60; databasenaam >**: de naam van de database die u wilt maken. Als u alleen gebruiken de standaard-database wilt, kan de query *maken van database...* worden weggelaten.
- **& #60; tabelnaam >**: de naam van de tabel die u wilt maken binnen de opgegeven database. Als u gebruiken de standaard-database wilt, de tabel rechtstreeks kan worden verwezen door *& #60; tabelnaam >* zonder & #60; databasenaam >.
- **& #60; veldscheidingsteken >**: het scheidingsteken waarmee de velden in het gegevensbestand dat moet worden geüpload naar de tabel component.
- **& #60; lijn scheidingsteken >**: het scheidingsteken waarmee de regels in het gegevensbestand.
- **& #60; opslaglocatie >**: de opslaglocatie van Azure om de gegevens van component tabellen te slaan. Als u geen opgeeft *locatie & #60; opslaglocatie >*, de database en de tabellen zijn opgeslagen in *component/warehouse/* directory in de standaard-container van het cluster component al dan niet standaard. Als u opgeven van de opslaglocatie wilt, heeft de opslaglocatie van binnen de standaardcontainer voor de database en tabellen. Deze locatie moet worden verwezen als locatie ten opzichte van de standaardcontainer van het cluster in de notatie van *' wasb: / / / & #60; map 1 > /'* of *' wasb: / / / & #60; map 1 > / & #60; map 2 > /'*, enz. Nadat de query wordt uitgevoerd, wordt de relatieve mappen worden gemaakt in de standaardcontainer.
- **TBLPROPERTIES("SKIP.header.line.Count"="1")**: als het gegevensbestand een kopregel heeft, moet u deze eigenschap toevoegen **aan het einde** van de query *-tabel maken* . Anders is de kop-regel als een record aan de tabel geladen. Als het gegevensbestand geen een kopregel, kan deze configuratie in de query worden weggelaten.

## <a name="load-data"></a>Gegevens aan component tabellen laden
Hier ziet u de component-query die de gegevens in een tabel component laadt.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; pad naar blob-gegevens >**: als het bestand blob om te worden geüpload naar de component-tabel in de standaard-container van het cluster HDInsight Hadoop, de *& #60; pad naar blob-gegevens >* moeten worden in de indeling *' wasb: / / / & #60; directory in deze container > / & #60; blob bestandsnaam >'*. Het bestand blob kan ook worden in een extra container van het cluster HDInsight Hadoop. In dit geval *& #60; pad naar blob-gegevens >* moeten worden in de indeling *' wasb: / / & #60; container name>@&#60;storage accountnaam >.blob.core.windows.net/ & #60; blob bestandsnaam >'*.

    >[AZURE.NOTE] De blobgegevens die moeten worden geüpload naar Component tabel heeft in de standaard- of extra container van het account opslag voor het cluster Hadoop. De query *Gegevens laden* mislukt anders klagen dat deze geen toegang de gegevens tot.


## <a name="partition-orc"></a>Geavanceerde onderwerpen: gegevens in de tabel en store de component in ORC indeling partitioneren

Als de gegevens groot is, is het partitioneren van de tabel handig voor query's die hoeft slechts eenmaal te scannen van een paar partities van de tabel. Het is bijvoorbeeld redelijk partitioneren de logboekgegevens van een website op datum.

Naast het partitioneren component tabellen, is het ook nuttig om de opslag van de component-gegevens in de indeling geoptimaliseerd rij kolommen (ORC). Zie <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Gebruik ORC bestanden verbeteren de prestaties wanneer component is lezen, schrijven, en gegevensverwerking</a>voor meer informatie over het opmaken van ORC.

### <a name="partitioned-table"></a>Gepartitioneerde tabel
Hier ziet u de component-query die u maakt een gepartitioneerde tabel en laadt gegevens erin.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Wanneer de query's uitvoeren gepartitioneerde tabellen, is het raadzaam de voorwaarde partition toevoegen aan het **begin** van de `where` component als volgt verbetert de effectiviteit van aanzienlijk zoeken.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Component gegevens bevatten in ORC-indeling

U laden niet rechtstreeks gegevens uit blobopslag naar component tabellen die zijn opgeslagen in de notatie ORC. Hier volgen de stappen die u moet uitvoeren om te laden gegevens van Azure BLOB's aan component tabellen die zijn opgeslagen in ORC-indeling.

De gegevens van **Die zijn opgeslagen als TEXTFILE** en laden in een externe tabel maken van blobopslag aan de tabel.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Een interne tabel maken met hetzelfde schema als de externe tabel in stap 1, met het scheidingsteken voor dezelfde velden, en opslag van de component-gegevens in de indeling ORC.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Selecteer gegevens uit de externe tabel in stap 1 en in de tabel ORC invoegen

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Als de tabel TEXTFILE *& #60; databasenaam >. & #60; externe textfile tabelnaam >* partities, klik in stap 3 heeft de `SELECT * FROM <database name>.<external textfile table name>` opdracht Hiermee selecteert u de variabele partition Als een veld in de resulterende gegevensset. Ingevoegd in de *& #60; databasenaam >. & #60; ORC tabelnaam >* mislukt sinds *& #60; databasenaam >. & #60; ORC tabelnaam >* heeft geen de variabele partition als veld in het tabelschema. In dit geval moet u specifiek Selecteer de velden die moet worden ingevoegd naar *& #60; databasenaam >. & #60; ORC tabelnaam >* als volgt:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Deze veilig neerzetten is de *& #60; externe textfile tabelnaam >* wanneer met behulp van de volgende query nadat alle gegevens is ingevoegd in *& #60; databasenaam >. & #60; ORC tabelnaam >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Na het uitvoeren van deze procedure, moet u een tabel met gegevens in de indeling ORC klaar voor gebruik hebben.  
