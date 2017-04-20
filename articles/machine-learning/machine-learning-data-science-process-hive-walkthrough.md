<properties
    pageTitle="Het proces van het Team gegevens wetenschappelijke in actie: gebruik Hadoop clusters | Microsoft Azure"
    description="Gebruik het Team gegevens wetenschap proces voor een end-to-end-scenario die gebruikmaakt van een cluster HDInsight Hadoop voor bouwen en implementeren van een model met een openbaar gegevensset."
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Het proces van het Team gegevens wetenschappelijke in actie: HDInsight Hadoop clusters gebruiken

In dit scenario gebruiken we het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md) in een end-to-end-scenario met een [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) om te slaan, te verkennen en aanbevelen engineeren gegevens uit de openbaar [Tevens Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset en omlaag voorbeeld de gegevens. Modellen van de gegevens worden gemaakt met Azure Machine Learning worden afgehandeld binaire en multiclass classificatie en regressie blog taken.

Zie voor stapsgewijze instructies waarin wordt uitgelegd hoe u omgaat met een grotere gegevensset (1 terabyte) voor een soortgelijke scenario HDInsight Hadoop clusters met voor gegevensverwerking, [Team gegevens wetenschap proces - Azure HDInsight Hadoop Clusters gebruiken op een gegevensset 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Het is ook mogelijk om te gebruiken van een notitieblok IPython taken voor de stapsgewijze instructies voor het gebruik van de gegevensset 1 TB gepresenteerd. Gebruikers die u wilt proberen deze methode moeten het onderwerp [Criteo scenario met een ODBC-component-verbinding](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) raadplegen.


## <a name="dataset"></a>Beschrijving van de tevens Taxi reizen gegevensset

De gegevens tevens Taxi reis is ongeveer 20GB gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48GB niet gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke reizen en de tarieven betaald voor elke reis. Elke record reis bevat de ophalen en inleverbibliotheek locatie en tijd, anoniem hack (stuurprogramma van) licentie getal en straten (unieke id van taxi) getal. De gegevens behandelt alle reizen in het jaar 2013 en is opgegeven in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten reis details, zoals het aantal personen, ophalen en dropoff punten reis duur en duur van reis. Hier volgen een paar steekproef records:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. De 'trip_fare' CSV-bestanden bevatten details van het tarief dat is betaald voor elke reis zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en het totale bedrag dat is betaald. Hier volgen een paar steekproef records:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel voor deelname aan reis\_gegevens en reis\_tarief bestaat uit de velden: straten, hack\_certificaat en ophalen van\_datetime.

Als u alle details relevant zijn voor een bepaalde reis, is het voldoende deel te nemen aan met drie sleutels: de "straten", "inbreken\_licentie" en "ophalen\_datetime '.

Sommige meer details van de gegevens worden beschreven wanneer we ze hebt opgeslagen in tabellen component kort.

## <a name="mltasks"></a>Voorbeelden van de voorspelling taken
Als gegevens, het bepalen van het soort voorspellingen die u wilt aanbrengen nadert gebaseerd op de analyse, kunt de taken die u nodig hebt om op te nemen in uw proces verduidelijken.
Hier ziet u drie voorbeelden van tekstvoorspelling problemen die we in dit scenario waarvan formulering is gebaseerd beantwoorden op de *tip\_bedrag*:

1. **Binaire indeling**: voorspellen al dan niet een tip betaald is voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een positief voorbeeld is, wanneer een *tip\_bedrag* van €0 is een negatieve voorbeeld.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass classificatie**: te voorspellen van het bereik van tip bedragen betaald voor de reis. U deelt de *tip\_bedrag* in vijf opslaglocaties of klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressie taak**: het bedrag van de hoofdsom voor een reis tip voorspellen.  


## <a name="setup"></a>Een cluster HDInsight Hadoop voor geavanceerde analyses instellen

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

U kunt een Azure-omgeving voor geavanceerde analyses die gebruikmaakt van een HDInsight cluster in drie stappen instellen:

1. [Een account opslag maken](../storage/storage-create-storage-account.md): dit opslag-account wordt gebruikt voor het opslaan van gegevens in Azure-blobopslag. De gegevens die worden gebruikt in HDInsight clusters ook bevindt zich hier.

2. [Aanpassen Azure HDInsight Hadoop clusters voor het proces van geavanceerde analyses en technologie](machine-learning-data-science-customize-hadoop-cluster.md). Deze stap maakt een Azure HDInsight Hadoop cluster met 64-bits Anaconda Python 2.7 geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen moet denken bij het aanpassen van uw cluster HDInsight.

    * Onthoud dat de opslag-account hebt gemaakt in stap 1 met uw cluster HDInsight bij het maken van deze te koppelen. Dit account opslag wordt gebruikt voor toegang tot gegevens die binnen het cluster worden verwerkt.

    * Nadat het cluster is gemaakt, kunt u de externe toegang inschakelen met het hoofd knooppunt van het cluster. Ga naar het tabblad **configuratie** en klik op **Externe inschakelen**. Deze stap geeft de referenties van de gebruiker die wordt gebruikt voor externe aanmelding.

3. [Een werkruimte Azure Machine Learning maken](machine-learning-create-workspace.md): deze Azure Machine Learning-werkruimte wordt gebruikt om u te maken van machine learning-modellen. Deze taak is gericht na het voltooien van de oorspronkelijke gegevens verkennen en omlaag steekproeven met de opdracht HDInsight.

## <a name="getdata"></a>De gegevens ophalen uit een openbare gegevensbron

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

Als u de gegevensset [Tevens Taxi reizen](http://www.andresmh.com/nyctaxitrips/) vanuit de openbare locatie, kunt u een van de methoden beschreven in de [Gegevens verplaatsen naar en vanuit Azure-blobopslag](machine-learning-data-science-move-azure-blob.md) de gegevens wilt kopiëren naar uw computer.

Hier worden beschreven hoe AzCopy gebruiken om de bestanden met de gegevens te brengen. Als u wilt downloaden en installeren AzCopy de instructies bij [Aan de slag met het hulpprogramma AzCopy-opdrachtregel](../storage/storage-use-azcopy.md).

1. Actie in een venster opdrachtprompt de volgende opdrachten voor AzCopy, *< path_to_data_folder >* vervangen door de gewenste bestemming:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Wanneer de kopie is voltooid, wordt een totaal van 24 ZIP-bestanden zijn in de map data gekozen. Pak de gedownloade bestanden naar dezelfde map op uw lokale computer. Noteer de map waar de niet-gecomprimeerde bestanden maken. Deze map zal worden verwezen als de *< pad\_naar\_unzipped_data\_bestanden\> * is het volgende.


## <a name="upload"></a>De gegevens uploaden naar de standaard-container van Azure HDInsight Hadoop cluster

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

Klik in de volgende opdrachten AzCopy, vervangen door de volgende parameters de werkelijke waarden die u hebt opgegeven bij het maken van het cluster Hadoop en de gegevensbestanden ritsen.

* ***& #60; path_to_data_folder >*** de adreslijst (samen met pad) op de computer die de uitgepakt gegevensbestanden bevatten  
* ***& #60; opslagaccountnaam van Hadoop cluster >*** het opslag-account dat is gekoppeld aan uw cluster HDInsight
* ***& #60; standaardcontainer van Hadoop cluster >*** de standaard-container door uw cluster gebruikt. Houd er rekening mee dat de naam van de standaardcontainer meestal dezelfde naam als het cluster zelf is. Als het cluster heet 'abc123.azurehdinsight.net', is de standaardcontainer bijvoorbeeld abc123.
* ***& #60; opslag accountsleutel >*** de toets voor de opslag-account door uw cluster gebruikt

Voer de volgende twee AzCopy-opdrachten uit een opdrachtprompt of in een Windows PowerShell-venster op de computer.

Deze opdracht uploadt de gegevens van de reis naar ***nyctaxitripraw*** directory in de standaard-container van het cluster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Deze opdracht uploadt de gegevens van het tarief naar ***nyctaxifareraw*** directory in de standaard-container van het cluster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

De gegevens moet nu in Azure-blobopslag en gereed om te worden verbruikt binnen het cluster HDInsight.

## <a name="#download-hql-files"></a>Meld u aan bij het hoofd knooppunt van Hadoop cluster en en voorbereiden voor experimentele gegevensanalyse

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

Voor toegang tot het hoofd knooppunt van de cluster voor experimentele gegevensanalyse en omlaag steekproeven van de gegevens, volgt u de procedure in [Access het hoofd knooppunt van Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

In deze procedure wordt hoofdzakelijk met query's in [component](https://hive.apache.org/), een SQL-achtige querytaal, geschreven voorlopige gegevens explorations uitvoeren. De component query's worden opgeslagen in .hql bestanden. We vervolgens voorbeeldgegevens omlaag deze moet worden gebruikt in Azure Machine Learning voor het samenstellen van modellen.

Het cluster om voor te bereiden experimentele gegevensanalyse, downloaden we de .hql-bestanden met de relevante component scripts vanuit [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) naar een lokale map (C:\temp) op het hoofd knooppunt. Klik hiertoe opent u de **opdrachtprompt** van binnen het hoofd knooppunt van het cluster en de volgende twee opdrachten:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Deze twee opdrachten worden alle .hql-bestanden die nodig zijn in dit scenario naar de lokale map ***C:\temp & #92;*** in het hoofd knooppunt gedownload.

## <a name="#hive-db-tables"></a>Component database en tabellen per maand partities maken

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

We gaan nu component tabellen voor onze tevens taxi gegevensset maken.
Open de ***opdrachtregel Hadoop*** op het bureaublad van het hoofd knooppunt in het hoofd knooppunt van het cluster Hadoop en voert u de map component door in te voeren van de opdracht

    cd %hive_home%\bin

>[AZURE.NOTE] **Alle component opdrachten uitvoeren in deze procedure uit de bovenstaande component opslaglocatie / opdrachtprompt van de map. Hiermee wordt handelingen kunt verrichten van eventuele problemen pad automatisch. We gebruiken de termen "Component directory vragen", "component opslaglocatie / opdrachtprompt van de map ', en" Hadoop-opdrachtregel ' door elkaar in dit scenario.**

Voer de volgende opdracht in Hadoop opdrachtregel van het hoofd knooppunt om in te dienen de query component component database en tabellen maken uit de adreslijst component prompt:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Hier ziet u de inhoud van de ***C:\temp\sample\_component\_maken\_db\_en\_tables.hql*** -bestand geïmporteerd component database ***nyctaxidb*** en tabellen ***reis*** en ***tarief***gemaakt.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Dit onderdeel script Hiermee maakt u twee tabellen:

* de tabel "reis" bevat reis details van elke rijpositie (stuurprogramma details, ophalen tijd, reis afstand en tijden)
* de tabel "tarief" bevat tarief informatie (tarief, tip het bedrag, tolgelden en toeslagen).

Als u een extra hulp nodig met deze procedures of wilt onderzoeken alternatieve bestanden, raadpleegt u de sectie [query's van indienen component rechtstreeks vanaf de opdrachtregel van Hadoop ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Gegevens laden naar component tabellen door wanden

>[AZURE.NOTE] Dit is meestal een taak in een **beheerder** .

De tevens taxi gegevensset heeft een natuurlijke partitioneren per maand, die we gebruiken om in te schakelen verwerking en query's te versnellen. De PowerShell-opdrachten onderstaande (uitgegeven door de component-map met de **opdrachtregel Hadoop**) laden gegevens aan de "reis" en "tarief" component tabellen partities per maand.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

De *voorbeeld\_component\_laden\_gegevens\_door\_partitions.hql* bestand bevat de volgende opdrachten voor het **laden** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Opmerking dat gebruikmaakt van een aantal component-query's die we hier in het proces te verkennen gebruiken op alleen een enkele partition of op een aantal partities gevonden. Maar deze query's over de hele gegevens kunnen worden uitgevoerd.

### <a name="#show-db"></a>Databases weergeven in het cluster HDInsight Hadoop

Als u wilt weergeven van de databases die zijn gemaakt in HDInsight Hadoop cluster in het venster Hadoop-opdrachtregel, voer de volgende opdracht in Hadoop opdrachtregel:

    hive -e "show databases;"

### <a name="#show-tables"></a>De component tabellen in de database nyctaxidb weergeven

Als de tabellen in de database nyctaxidb weergeven, voert u de volgende opdracht in Hadoop opdrachtregel:

    hive -e "show tables in nyctaxidb;"

We kunt bevestigen dat de tabellen zijn partitioneren door de onderstaande opdracht:

    hive -e "show partitions nyctaxidb.trip;"

Hieronder vindt u de verwachte uitvoer:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Daarnaast kunt we ervoor zorgen dat de tabel tarief is partitioneren door de onderstaande opdracht:

    hive -e "show partitions nyctaxidb.fare;"

Hieronder vindt u de verwachte uitvoer:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Gegevens verkennen en functie engineering in component

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

De gegevens verkennen en de functie engineering taken voor de gegevens in de tabellen component geladen kunnen worden uitgevoerd met de component query's. Hier ziet voorbeelden van taken dat we u via in deze sectie begeleiden:

- De top 10-records in beide tabellen weergeven.
- Gegevens onderzoeken met een paar velden in verschillende tijd windows verkennen.
- Onderzoek kwaliteit van de gegevens van de velden lengte en breedte.
- Genereren binaire en multiclass classificatie etiketten op basis van de **tip\_bedrag**.
- Genereer functies door de afstand directe reis computing.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Uitleg: De top 10-records weergeven in de tabel reis

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Als u wilt zien hoe de gegevens eruit, bestudeert u 10 records uit elke tabel. De volgende twee query's afzonderlijk uitvoeren vanaf de prompt component directory in de console Hadoop-opdrachtregel moet worden gecontroleerd van de records.

Aan de top 10-records in de tabel "reis" van de eerste maand:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

De bovenste 10 records in de tabel "tarief" vanaf de eerste maand opvragen:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Het is vaak nuttige informatie voor het opslaan van de records in een bestand voor handige weergave. Een kleine wijziging aan de bovenstaande query doet dit:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Uitleg: Het aantal records in elk van de 12 partities weergeven

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Belangrijke als volgt het aantal reizen varieert tijdens het kalenderjaar. Groeperen per maand kunt ons om te zien hoe deze verdeling van reizen eruitziet.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Hierdoor ontstaat de uitvoer geeft:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Hier de eerste kolom is de maand en het tweede is het aantal bezoeken voor die maand.

We kunt ook het totale aantal records tellen in onze gegevensverzameling reis door de volgende opdracht bij de opdrachtprompt van de map component.

    hive -e "select count(*) from nyctaxidb.trip;"

Dit levert:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Opdrachten die overeenkomen met die wordt weergegeven voor de gegevensgroep reis gebruikt, kunnen we component query's vanuit de component directory prompt voor de gegevensgroep tarief voor het valideren van het aantal records verlenen.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Hierdoor ontstaat de uitvoer geeft:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Houd er rekening mee dat de exacte hetzelfde aantal bezoeken per maand voor beide gegevenssets wordt geretourneerd. Hier vindt u de eerste validatie dat de gegevens correct is geladen.

Het totale aantal records in de gegevensset tarief tellen kan worden uitgevoerd met de opdracht onder vanaf de component directory prompt:

    hive -e "select count(*) from nyctaxidb.fare;"

Dit levert:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Het totale aantal records in beide tabellen is ook hetzelfde. Hier vindt u een tweede validatie dat de gegevens correct is geladen.

### <a name="exploration-trip-distribution-by-medallion"></a>Uitleg: Reis verdeling door straten

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

In dit voorbeeld wordt de straten (taxi getallen) aangeduid met meer dan 100 reizen binnen een bepaalde periode. De voordelen van de query uit de access gepartitioneerde tabel omdat deze zijn afhankelijk per partition variabele **maand**. De queryresultaten worden geschreven naar een lokaal bestand queryoutput.tsv in `C:\temp` op het hoofd knooppunt.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Hier ziet u de inhoud van *voorbeeld\_component\_reis\_tellen\_door\_medallion.hql* bestand voor controle.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

De straten in de gegevensset tevens taxi kunt u een unieke cab identificeren. We kunnen vaststellen om wat welke cabines "bezet" bent door de functies die meer dan een bepaald aantal reizen aangebracht in een bepaalde periode vragen te stellen. Het volgende voorbeeld wordt cabines die meer dan honderd reizen aangebracht in de eerste drie maanden en Hiermee slaat de query is het resultaat naar een lokaal bestand, C:\temp\queryoutput.tsv.

Hier ziet u de inhoud van *voorbeeld\_component\_reis\_tellen\_door\_medallion.hql* bestand voor controle.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Probleem van de component directory-prompt de opdracht hieronder:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Uitleg: Reis verdeling door straten en hack_license

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Wanneer een gegevensset verkennen, wilt we vaak onderzoeken welke gevolgen het aantal co-exemplaren van groepen met waarden. In dit gedeelte vindt u een voorbeeld van hoe u deze stap herhalen voor cabines en -stuurprogramma's.

De *voorbeeld\_component\_reis\_tellen\_door\_straten\_license.hql* bestand de gegevensset tarief op "straten" en "hack_license" zijn gegroepeerd en geeft als resultaat aantallen elke combinatie. Hieronder vindt u de inhoud ervan.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Deze query retourneert cab en bepaalde stuurprogramma combinaties gesorteerd op aflopende aantal reizen.

Vanaf de prompt component directory uitvoeren:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

De queryresultaten worden geschreven naar een lokaal bestand C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Uitleg: Beoordeling van kwaliteit van de gegevens door te schakelen voor Ongeldige lengtegraad/breedtegraad records

>[AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Een algemene doel van experimentele gegevensanalyse is te selecteren ongeldige of onjuiste records. Het voorbeeld in deze sectie wordt bepaald of het lengtegegevens of breedtegraad een waarde uiterst buiten het gebied tevens bevatten. Aangezien er waarschijnlijk dat deze records een foutieve lengtegegevens-breedtegraad-waarden bevatten, willen we Verwijder deze uit alle gegevens die moet worden gebruikt voor modellering.

Hier ziet u de inhoud van *voorbeeld\_component\_kwaliteit\_assessment.hql* bestand voor controle.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Vanaf de prompt component directory uitvoeren:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Het argument *-S* is opgenomen in deze opdracht onderdrukt de afdruk van het scherm status van de Map/verkleinen component-taken. Dit is handig omdat dit zorgt ervoor dat het scherm afdrukken van de queryuitvoer component beter leesbaar.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Uitleg: Binaire class onderzoeken reis tips

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Voor de binaire indeling probleem uit die in de sectie [voorbeelden van de voorspelling taken](machine-learning-data-science-process-hive-walkthrough.md#mltasks) beschreven, is het handig om te weten of een tip al dan niet is opgegeven. Deze verdeling van tips luidt binaire als volgt:

* Tip gegeven (Class 1, tip\_verschuldigde > $0)  
* geen tip (klasse 0, tip\_bedrag = $0).

De *voorbeeld\_component\_Gekantelde\_frequencies.hql* bestand hieronder doet dit.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Vanaf de prompt component directory uitvoeren:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Uitleg: Class onderzoeken in de multiclass instelling

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Voor het multiclass classificatie-probleem uit die in de sectie [voorbeelden van de voorspelling taken](machine-learning-data-science-process-hive-walkthrough.md#mltasks) beschreven deze gegevensset ook gepaard met een natuurlijke classificatie waar willen we het bedrag van de tips gegeven voorspellen. We kunt opslaglocaties definiëren tip bereiken in de query. Als u de class onderzoeken voor de verschillende bereiken tip, gebruiken we de *voorbeeld\_component\_tip\_bereik\_frequencies.hql* bestand. Hieronder vindt u de inhoud ervan.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Voer de volgende opdracht uit Hadoop-opdrachtregel console:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Uitleg: Directe afstand tussen twee lengtegegevens breedtegraad locaties berekenen

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Een maateenheid voor de rechtstreekse afstand ondervindt, kunnen we om vast te stellen het verschil tussen deze en de werkelijke reis-afstand. We motiveren deze functie door te wijzen dat een personen mogelijk waarschijnlijk minder tip als ze duidelijk dat het stuurprogramma heeft met opzet hebt genomen ze via een veel langer weg.

Als u wilt zien van de vergelijking tussen werkelijke reis afstand en de [Haversine afstand](http://en.wikipedia.org/wiki/Haversine_formula) tussen twee lengtegegevens breedtegraad punten (de afstand "fraai cirkel"), we gebruiken de beschikbaar trigonometrische functies binnen component, dus:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

In de bovenstaande query R is de straal van de aarde in mijl en pi wordt geconverteerd naar radialen. Houd er rekening mee dat de lengtegraad breedtegraad wordt verwezen zijn 'gefilterd"om waarden die ver van het gebied tevens zijn te verwijderen.

In dit geval schrijven we onze resultaten naar een map met de naam 'queryoutputdir'. De reeks opdrachten die hieronder wordt weergegeven, maakt eerst deze uitvoermap, en voert de opdracht component.

Vanaf de prompt component directory uitvoeren:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


De queryresultaten naar 9 Azure BLOB's zijn geschreven ***queryoutputdir/000000\_0*** naar ***queryoutputdir/000008\_0*** onder de standaard-container van het cluster Hadoop.

Als u wilt zien van de grootte van de afzonderlijke BLOB's, voer we de volgende opdracht uit de component directory prompt:

    hdfs dfs -ls wasb:///queryoutputdir

Als u wilt zien van de inhoud van een bepaald bestand, zeg 000000\_0, gebruiken we de Hadoop `copyToLocal` opdracht, dus.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`kan heel lang voor grote bestanden, en niet geschikt is voor gebruik met hen.  

Een belangrijk voordeel van deze gegevens bevinden zich in een Azure blob is dat mogelijk besproken voor de gegevens in Azure Machine Learning met de [Gegevens importeren] [ import-data] module.


## <a name="#downsample"></a>Omlaag steekproef gegevens en opbouwen modellen in Azure Machine Learning

> [AZURE.NOTE] Dit is meestal een taak **Gegevens wetenschappelijk** .

Na de experimentele gegevens-analyse-fase gaan we nu voorbeeld de gegevens voor het samenstellen van gegevensmodellen in Azure Machine Learning-omlaag. In dit gedeelte we het gebruik van een query component omlaag voorbeeld de gegevens worden weergegeven, dat vervolgens toegankelijk is vanuit de [Gegevens importeren] [ import-data] module Azure Machine weten.

### <a name="down-sampling-the-data"></a>Omlaag steekproef van de gegevens

Er zijn twee stappen in deze procedure. Eerst we de tabellen **nyctaxidb.trip** en **nyctaxidb.fare** samenvoegen op drie sleutels die aanwezig in alle records zijn: "straten", "inbreken\_licentie ', en" ophalen\_datetime '. We hoofdidee een binaire indeling label **Gekantelde** en een label meerdere class classificatie **tip\_class**.

Kunnen de pijl-omlaag gebruiken dat u gegevens rechtstreeks vanuit de [Gegevens importeren] [ import-data] module Azure Machine Learning, is het nodig zijn voor de opslag van de resultaten van de bovenstaande query toevoegen aan een interne component-tabel. Klik in het volgende, we een interne component-tabel maken en de inhoud ervan met de gekoppelde en omlaag steekproef gegevens vullen.

De query is van toepassing standaardfuncties component rechtstreeks om te genereren van het uur van de dag, week van het jaar, weekday (1 staat voor maandag en 7 staat voor zondag) van de ' ophalen\_datetime ' veld en de directe afstand tussen de locaties ophalen en dropoff. Gebruikers kunnen [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) raadplegen voor een volledige lijst van deze functies.

De query vervolgens voorbeelden omlaag de gegevens zodat de queryresultaten in het Azure Machine Learning Studio past. Alleen ongeveer 1% van de oorspronkelijke gegevensset is in de Studio geïmporteerd.

Hieronder vindt u de inhoud van *voorbeeld\_component\_voorbereiden\_voor\_aml\_full.hql* worden gegevens voorbereid voor model samenstellen in Azure Machine Learning-bestand.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Deze query uitvoeren vanaf de component directory prompt:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Nu zijn er een interne tabel "nyctaxidb.nyctaxi_downsampled_dataset" die kunnen worden geopend met de [Gegevens importeren] [ import-data] module van Azure Machine Learning. We kunnen deze dataset bovendien gebruiken voor het samenstellen van Machine Learning-modellen.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Gebruik van de module gegevens importeren in Azure Machine Learning voor toegang tot de omlaag steekproef gegevens

Als de vereisten voor het verlenen van component query's in de [Gegevens importeren] [ import-data] module van Azure Machine Learning we nodig hebben toegang tot een Azure Machine Learning-werkruimte en toegang tot de referenties van het cluster en de bijbehorende opslag-account.

Sommige meer informatie over de [Gegevens importeren] [ import-data] module en de parameters om in te voeren:

**HCatalog server-URI**: als de naam van de cluster abc123 is, is dit gewoon: https://abc123.azurehdinsight.net

**Hadoop gebruikersnaam** : de naam van de gebruiker gekozen voor het cluster (**niet** de naam van de RAS-gebruiker)

**Hadoop ser accountwachtwoord** : het wachtwoord gekozen voor het cluster (**niet** het wachtwoord voor externe toegang)

**Locatie van uitvoergegevens** : Hiermee worden Azure wordt gekozen.

**Azure-opslagaccountnaam** : naam van het standaardaccount voor de opslag die is gekoppeld aan het cluster.

**De containernaam van de Azure** : dit is de standaardnaam van de container voor het cluster en is meestal hetzelfde als de naam van het cluster. Voor een cluster 'abc123' genoemd, is dit alleen abc123.

> [AZURE.IMPORTANT] **Een tabel wilt query met de [Gegevens importeren] [ import-data] module in Azure Machine Learning een interne tabel moet zijn.** Een tip om te bepalen of een tabel T in een database D.db een interne tabel is is als volgt.

Geef de opdracht uit de adreslijst component prompt:

    hdfs dfs -ls wasb:///D.db/T

Als de tabel een interne tabel is en deze is ingevuld, wordt de inhoud moeten hier weergegeven. Een andere manier om te bepalen of een tabel een interne tabel is is via de Verkenner Azure-opslag. Gebruik deze om te navigeren naar de standaardnaam van de container van het cluster en vervolgens filteren op de naam van de tabel. Als de tabel en de inhoud ervan worden weergegeven, wordt dit bevestigd dat dit een interne tabel is.

Hier ziet u een momentopname van de query component en de [Gegevens importeren] [ import-data] module:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Opmerking dat de resulterende component query van Azure Machine Learning sinds onze omlaag steekproef gegevens zich in de standaardcontainer bevinden, heel eenvoudig is en alleen is een "Selecteer * uit nyctaxidb.nyctaxi\_verkleind\_gegevens".

De gegevensset kan nu worden gebruikt als uitgangspunt voor het samenstellen van Machine Learning-modellen.

### <a name="mlmodel"></a>Modellen in Azure Machine Learning maken

We kunnen nu gaat u verder met het model bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens is gereed voor ons in adressering van de voorspelling problemen hierboven gebruiken:

**1. binaire indeling**: te voorspellen al dan niet een tip is betaald voor een reis.

**Ingesteld die wordt gebruikt:** Twee-klasse logistische regressie

een. Voor dit probleem, is ons doel (of een klasse) label 'Gekantelde". Onze oorspronkelijke gedownsampled gegevensset heeft een paar kolommen die doel lekkage voor dit experiment classificatie zijn. Met name: tip\_klasse, tip\_bedrag en totaal\_verschuldigde onthulling informatie over de labels die niet beschikbaar is binnen het testen van de tijd. We deze kolommen verwijderen uit de aandacht met behulp van de [Kolommen selecteren in de gegevensset] [ select-columns] module.

De onderstaande momentopname ziet u ons experiment te voorspellen al dan niet een tip voor een bepaald reis is betaald.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Voor dit experiment zijn ons doel label onderzoeken ongeveer 1:1.

De onderstaande momentopname toont de verdeling van tip class labels voor het probleem binaire indeling.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Hierdoor wordt een AUC van 0.987 aanvragen zoals wordt weergegeven in de onderstaande afbeelding.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass classificatie**: het bereik van tip bedragen betaald voor de reis, met de eerder gedefinieerde klassen voorspellen.

**Ingesteld die wordt gebruikt:** Multiclass logistische regressie

een. Voor dit probleem ons doel (of een klasse) label is ' tip\_class ' die een van de vijf waarden (0,1,2,3,4) kunt uitvoeren. Net als in het geval is binaire indeling hebben we enkele kolommen die doel lekkage voor dit experiment zijn. Met name: Gekantelde, tip\_totale bedrag\_verschuldigde onthulling informatie over de labels die niet beschikbaar is binnen het testen van de tijd. We deze kolommen met behulp van de [Kolommen selecteren in de gegevensset] verwijderen[ select-columns] module.

De onderstaande momentopname ziet u ons experiment te voorspellen in welke voor een tip is waarschijnlijk vallen (klasse 0: tip = $0, klasse 1: tip > $0 en tip < = $5, klasse 2: tip > $5 en tip < = $10, klasse 3: tip > $10 en tip < = $20, klasse 4: tip > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Nu u leert hoe onze werkelijke test class verdeling eruitziet. Zien we dat klasse 0 en 1 Class gangbare zijn, de andere klassen zijn niet vaak voorkomen.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Voor dit experiment gebruiken we een matrix verwarring onze accuratesse tekstvoorspelling nagaan. Dit is hieronder weergegeven.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Houd er rekening mee dat onze accuratesse class op de bekende klassen helemaal goed is, het model wordt niet goed 'leren' op de sommige klassen.


**3. regressie taak**: aan de hoeveelheid tip betaald voor een reis voorspellen.

**Ingesteld die wordt gebruikt:** Gestimuleerd beslissingsstructuur

een. Voor dit probleem ons doel (of een klasse) label is ' tip\_bedrag ". Ons doel lekkage in dit geval zijn: Gekantelde, tip\_class, totale\_verschuldigde; alle deze variabelen geven informatie over het tip bedrag dat meestal niet beschikbaar voor het testen van de tijd. We deze kolommen met behulp van de [Kolommen selecteren in de gegevensset] verwijderen[ select-columns] module.

De belows momentopname ziet u ons experiment te voorspellen van het bedrag van de opgegeven tip.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Voor regressie problemen meten we de accuratesse van onze tekstvoorspelling door te kijken de square error in de voorspellingen, de correlatiecoëfficiënt en dergelijke. U leert deze hieronder.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Door onze coëfficiënten model wordt: ongeveer 71% van de variantie voor de overdracht zien we dat 0.709 over de correlatiecoëfficiënt is, uitgelegd.

> [AZURE.IMPORTANT] Meer informatie over Azure Machine Learning en hoe u toegang tot en deze kunt gebruiken, Raadpleeg [Wat Machine Learning is?](machine-learning-what-is-machine-learning.md). Een resource bijzonder nuttig zijn voor het afspelen van met een reeks Machine Learning-experimenten op Azure Machine Learning is de [Galerie met Cortana Intelligence](https://gallery.cortanaintelligence.com/). De galerie een gamma van experimenten behandelt en biedt een uitgebreide inleiding in het bereik van de mogelijkheden van Azure Machine Learning.

## <a name="license-information"></a>Licentie-informatie

In dit voorbeeld Stapsgewijze instructies en de bijbehorende begeleidende scripts worden gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt, in de adreslijst van de steekproef-code op GitHub voor meer informatie.

## <a name="references"></a>Verwijzingen

• [Andrés Monroy tevens Taxi reizen downloadpagina](http://www.andresmh.com/nyctaxitrips/)  
• [Van tevens Taxi reis gegevens door Chris Whong fOILing](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Tevens Taxi en Limousine provisie onderzoek en statistieken worden aangepast](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
