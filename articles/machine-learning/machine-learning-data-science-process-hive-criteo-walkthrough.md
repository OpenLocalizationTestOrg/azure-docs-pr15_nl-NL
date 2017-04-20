<properties
    pageTitle="Het proces van het Team gegevens wetenschappelijke in actie: gebruik HDInsight Hadoop clusters op de 1 TB Criteo gegevensset | Microsoft Azure"
    description="Met behulp van het Team gegevens wetenschap proces voor een end-to-end-scenario die gebruikmaakt van een cluster HDInsight Hadoop voor bouwen en implementeren van een model met een grote (1 TB) openbaar gegevensset"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Het proces van het Team gegevens wetenschappelijke in actie - Azure HDInsight Hadoop Clusters gebruiken op een gegevensset 1 TB

In dit scenario wordt gedemonstreerd met behulp van het Team gegevens wetenschappelijke processen in een scenario voor end-to-end met een [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) opslaan, verkennen, functie engineeren, en voorbeeldgegevens uit een van de openbaar [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) gegevenssets omlaag. We Azure Machine Learning om aan te vullen een model binaire indeling deze gegevens gebruiken. We ook hoe u een van deze modellen publiceren als een webservice worden weergegeven.

Het is ook een notitieblok IPython gebruiken voor het uitvoeren van de taken in dit scenario wordt gepresenteerd. Gebruikers die u wilt proberen deze methode moeten het onderwerp [Criteo scenario met een ODBC-component-verbinding](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) raadplegen.


## <a name="dataset"></a>Beschrijving van de gegevensset Criteo

De Criteo gegevens is een klik tekstvoorspelling gegevensset dat ongeveer 370GB gzip gecomprimeerd TSV-bestanden (~1.3TB niet gecomprimeerd), bestaande uit meer dan 4,3 miljard records. Dit is afkomstig van 24 dagen van Klik op gegevens die beschikbaar zijn aangebracht door [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Voor het gemak van gegevens wetenschappers, hebben we de gegevens die beschikbaar zijn voor ons om te experimenteren met bestanden.

Elke record in deze gegevensset bevat 40 kolommen:

- de eerste kolom is de labelkolom van een waarin wordt aangegeven of een gebruiker een **toevoegen** (waarde 1) of niet op een (waarde 0)
- volgende 13 kolommen moeten zijn numerieke, en
- laatste 26 zijn bestaan kolommen

De kolommen zijn anoniem en een reeks opgesomde namen gebruiken: "Kol1" (voor de labelkolom) naar ' Col40 "(voor de laatste bestaan kolom).            

Hier is een uittreksel van de eerste 20 kolommen van twee metingen (rijen) uit deze dataset:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Ontbrekende waarden zijn in de kolommen numerieke en bestaan er in deze gegevensset. Een eenvoudige methode voor het verwerken van de ontbrekende waarden worden beschreven. Extra details van de gegevens worden verkend wanneer we ze naar component tabellen hebt opgeslagen.

**Definitie:** *Doorklik rente (CTR):* Dit is het percentage van klikken in de gegevens. In deze gegevensset Criteo is de CTR ongeveer 3.3% of 0.033.

## <a name="mltasks"></a>Voorbeelden van de voorspelling taken
Twee steekproeven tekstvoorspelling problemen worden besproken in deze procedure:

1. **Binaire indeling**: voorspellen = of een gebruiker hebt geklikt toevoegen:
    - Klasse 0: Geen Klik
    - Klasse 1: klik op

2. **Regressie**: voorspellen = de kans op een ad-Klik op uit gebruikersfuncties.


## <a name="setup"></a>Instellen van een HDInsight Hadoop cluster voor gegevens wetenschappelijk

**Notitie:** Dit is meestal een taak in een **beheerder** .

Stel uw omgeving wetenschappelijke van Azure-gegevens voor het ontwikkelen van oplossingen voorstelt Bekijk analyses met HDInsight clusters in drie stappen:

1. [Een account opslag maken](../storage/storage-create-storage-account.md): dit opslag-account wordt gebruikt voor de opslag van gegevens in Azure-blobopslag. De gegevens die worden gebruikt in HDInsight clusters wordt hier opgeslagen.

2. [Azure HDInsight Hadoop Clusters aanpassen voor wetenschappelijk van gegevens](machine-learning-data-science-customize-hadoop-cluster.md): deze stap maakt u een Azure HDInsight Hadoop-cluster met 64-bits Anaconda Python 2.7 geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen (beschreven in dit onderwerp) om uit te voeren bij het aanpassen van het cluster HDInsight.

    * Het opslag-account dat is gemaakt in stap 1 met uw cluster HDInsight wanneer deze is gemaakt, moet u koppelen. Dit account opslag wordt gebruikt voor toegang tot gegevens die binnen het cluster kan worden verwerkt.

    * Nadat deze is gemaakt, moet u naar het hoofd knooppunt van het cluster externe toegang inschakelen. Onthoud dat de RAS-referenties die u hier opgeeft (anders dan die zijn opgegeven voor het cluster bij het is gemaakt): u moet de volgende procedures uitvoeren.

3. [Een werkruimte Azure ML maken](machine-learning-create-workspace.md): deze Azure Machine Learning-werkruimte wordt gebruikt voor het samenstellen van machine learning-modellen na de eerste gegevens verkennen en omlaag steekproeven op het cluster HDInsight.

## <a name="getdata"></a>Ophalen en gebruiken van gegevens uit een openbare gegevensbron

De gegevensset [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) kan worden geopend door te klikken op de koppeling, de gebruiksvoorwaarden te accepteren en een naam toevoegen. Hier ziet u een momentopname van hoe dit eruitziet:

![Ga akkoord met de voorwaarden Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klik op **Doorgaan om te downloaden** voor meer informatie over de gegevensset en de beschikbaarheid.

De gegevens zich bevinden in een openbare [Azure blob storage](../storage/storage-dotnet-how-to-use-blobs.md) -locatie: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. De "wasb" verwijst naar de locatie van de Azure-blobopslag. 

1. De gegevens in deze openbare blobopslag bestaat uit drie submappen van uitgepakt gegevens.

    1. De submap *Onbewerkte/telling/* bevat de eerste 21 dagen van de gegevens - vanaf dag\_00 naar dag\_20
    2. De submap *onbewerkte/trein/* bestaat uit één dag van de gegevens, dag\_21
    3. De submap *onbewerkte/testen/* bestaat uit twee dagen van gegevens, dag\_22 en dag\_23

2. Voor mensen die u wilt beginnen met de gegevens onbewerkte gzip, deze zijn ook beschikbaar in de hoofdmap *onbewerkte /* als day_NN.gz, waar NN van 00 en 23 hoort.

Een alternatief methode u wilt openen, te verkennen en model deze gegevens die niet voor een lokale downloads vereist is verderop in dit scenario wordt uitgelegd wanneer we component tabellen maakt.

## <a name="login"></a>Meld u aan bij de headnode cluster

Voor het aanmelden bij de headnode van het cluster, gebruikt u de [Azure-portal](https://ms.portal.azure.com) om te zoeken van het cluster. Klik op het pictogram HDInsight plaats aan de linkerkant en dubbelklik op de naam van uw cluster. Ga naar het tabblad **configuratie** , dubbelklik op het pictogram verbinding maken vanaf de onderkant van de pagina en voer uw referenties RAS wanneer hierom wordt gevraagd. Hiermee gaat u naar de headnode van het cluster.

Hier volgt een typisch eerste aangemeld bij de headnode cluster ziet er als:

![Meld u aan bij het cluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Aan de linkerkant zien we de "Hadoop opdrachtregel ', dat wil onze werkpaard voor het verkennen zeggen. We zien ook twee handige URL's - "Hadoop garens Status" en "Hadoop naam knooppunt". De URL van de status garens taakvoortgang van de wordt weergegeven en de naam knooppunt URL krijgt informatie over de configuratie van het cluster.

Nu we zijn ingesteld en gereed voor het eerste deel van de procedure begint: gegevens verkennen met component en voorbereiden gegevens Azure Machine Learning.

## <a name="hive-db-tables"></a>Component database en tabellen maken

Component tabellen voor onze gegevensset Criteo maken, opent u de ***opdrachtregel Hadoop*** op het bureaublad van het hoofd knooppunt en voert u de map component door in te voeren van de opdracht

    cd %hive_home%\bin

>[AZURE.NOTE] Alle component opdrachten uitvoeren in deze procedure uit de opslaglocatie component / opdrachtprompt van de map. Dit zorgt voor eventuele problemen pad automatisch. We gebruiken de termen "Component directory vragen", "component opslaglocatie / opdrachtprompt van de map ', en ' Hadoop-opdrachtregel ' door elkaar.

>[AZURE.NOTE]  Een component als query wilt uitvoeren, kan een altijd de volgende opdrachten gebruiken:

        cd %hive_home%\bin
        hive

Nadat de component REPL wordt weergegeven met een "component >"Meld u aan, gewoon knippen en plakken de query als u wilt uitvoeren.

De volgende code maakt u een database "criteo" en genereert vervolgens 4 tabellen:


* een *tabel voor het genereren van telt* gebouwd op dagen dag\_00 naar dag\_20,
* een *tabel gebruiken als de gegevensset trein* die is gebaseerd op de dag\_21, en
* twee *tabellen voor gebruiken als de test gegevenssets* gebouwd op dag\_22 en dag\_23 respectievelijk.

We splitsen onze gegevensset test in twee verschillende tabellen omdat een van de dagen een vakantie is en we bepalen wilt als het model verschillen tussen een feestdagen en niet vanuit het tarief weer dat Doorklik kunt detecteren.

Het script [voorbeeld & #95; component & #95; maken & #95; criteo & #95; database & #95; en & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) uitkomt Hier wordt weergegeven:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

We Houd er rekening mee dat alle deze tabellen externe zijn als we wijst u gewoon naar locaties Azure-blobopslag (wasb).

**Er zijn twee manieren een willekeurig component query die we nu vermelden uit te voeren.**

1. **Gebruik van de component REPL opdrachtregel**: de eerste is voor het verlenen van een opdracht "component" en kopiëren en plakken van een query op de component REPL opdrachtregel. Ga hiervoor doen:

        cd %hive_home%\bin
        hive

    Nu op de opdrachtregel REPL uitvoert knippen en plakken van de query deze.

2. **Query's naar een bestand op te slaan en de opdracht uitvoeren**: het tweede is een .hql-bestand opslaan in de query's ([voorbeeld & #95; component & #95; maken & #95; criteo & #95; database & #95; en & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) en geef vervolgens de volgende opdracht de query uit te voeren:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Bevestig de database en tabel maken

Vervolgens we het maken van de database met de volgende opdracht uit de opslaglocatie component controleren / opdrachtprompt van de map:

        hive -e "show databases;"

Het resultaat is:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Hiermee wordt het maken van de nieuwe database, "criteo" bevestigd.

Als u wilt zien welke tabellen die u hebt gemaakt, geeft we gewoon de opdracht uit de opslaglocatie component / opdrachtprompt van de map:

        hive -e "show tables in criteo;"

We raadpleegt u het volgende resultaat:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Gegevens verkennen in component

We kunnen nu moet u enkele eenvoudige verkennen in component doen. We beginnen door het aantal voorbeelden in de trein te tellen en gegevenstabellen testen.

### <a name="number-of-train-examples"></a>Aantal trein voorbeelden

De inhoud van de [steekproef & #95; component & #95; #95 & tellen; trein & #95; tabel & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) worden hier weergegeven:

        SELECT COUNT(*) FROM criteo.criteo_train;

Dit levert:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

U kunt ook een mogelijk ook Geef de volgende opdracht uit de opslaglocatie component / opdrachtprompt van de map:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Aantal test voorbeelden in de twee test gegevenssets

We nu tellen het nummer van de voorbeelden in de twee test gegevenssets. De inhoud van [voorbeeld & #95; component & #95; #95 & tellen; criteo & #95; test & #95; dag & #95; 22 & #95; tabel & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) bent hier:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Dit levert:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Zoals u gewend bent, wordt ook het script mogelijk aanroepen vanuit de opslaglocatie component / Active directory prompt voor de opdracht:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Tot slot we onderzoeken welke gevolgen het aantal test voorbeelden in de test gegevensset op basis van de dag\_23.

De opdracht hiervoor is vergelijkbaar met net zoals wordt weergegeven (Zie [voorbeeld & #95; component & #95; #95 & tellen; criteo & #95; test & #95; dag & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Het resultaat is:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Label verdeling in de trein gegevensset

De verdeling label in de trein gegevensset is van belang. Om dit te zien, wordt de inhoud van de [steekproef & #95; component & #95; criteo & #95; label & #95; verdeling & #95; trein & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql)weergeven:

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Dit levert de verdeling label:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Houd er rekening mee dat het percentage van de positieve etiketten ongeveer 3.3% (consistent zijn met de oorspronkelijke gegevensset) is.

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram onderzoeken van enkele numerieke variabelen in de trein gegevensset

We de beschikking over de component native "histogram\_numerieke" functie om vast te stellen wat de verdeling van de numerieke variabelen eruitziet. Hier vindt u de inhoud van de [steekproef & #95; component & #95; criteo & #95; histogram & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Dit levert het volgende:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

De DWARSKRACHTEN weergave - uitlichten combinatie in component biedt om de uitvoer van een SQL-achtige in plaats van de gebruikelijke lijst te produceren. Let in deze tabel, de eerste kolom overeenkomt met het beheercentrum opslaglocatie en de tweede naar de opslaglocatie frequentie.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Niet-geheel exacte percentielen van enkele numerieke variabelen in de trein gegevensset

Wordt ook de berekening van de niet-geheel exacte percentielen belangrijke met numerieke variabelen. Component bevindt zich systeemeigen "percentiel\_ongeveer" doet u dit voor ons. De inhoud van [voorbeeld & #95; component & #95; criteo & #95; benadering & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) zijn:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Dit levert:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

We dat de verdeling van percentielen is verwant aan het histogram verdeling van een numerieke variabele meestal te schakelen.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Het aantal unieke waarden vinden voor sommige bestaan kolommen in de trein gegevensset

Het verkennen doorgaat vinden we nu, voor sommige bestaan kolommen, het aantal unieke waarden die genomen worden. Klik hiertoe we de inhoud van weergeven [voorbeeld & #95; component & #95; criteo & #95; unieke & #95; waarden & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Dit levert:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

We Houd er rekening mee dat Col15 19M unieke waarden heeft! Het gebruik van naïef technieken zoals 'één direct codering' is om te coderen van dergelijke hoog dimensionaal bestaan variabelen onbruikbare. Met name, er wordt uitgelegd en een krachtige, robuuste techniek aangeroepen [Learning met telt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) dit probleem efficiënt aanpak illustreren.

We beëindigen deze subsectie door te zoeken bij het aantal unieke waarden voor enkele andere bestaan kolommen ook. De inhoud van [voorbeeld & #95; component & #95; criteo & #95; unieke & #95; waarden & #95; veelvoud & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) zijn:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Dit levert:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Opnieuw zien we dat de alle andere kolommen, behalve voor Col20, aantal unieke waarden hebben.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>CO exemplaar telt paren van bestaan variabelen in de trein gegevensset

De collega exemplaar aantallen paren van bestaan variabelen is ook van belang. Dit kan worden bepaald met behulp van de code in [voorbeeld & #95; component & #95; criteo & #95; gepaarde & #95; bestaan & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

We omgekeerde het telt sorteren op hun exemplaar en kijkt u naar de bovenkant 15 in dit geval. Hierdoor ontstaat:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Voorbeeld van de gegevenssets voor Azure Machine Learning omlaag

Met de gegevenssets verkend en gedemonstreerd hoe we mogelijk kan dit type uitleg voor alle variabelen (inclusief combinaties), we nu omlaag voorbeeld de gegevensgroepen zodat we modellen in Azure Machine Learning kunt maken. Let erop dat we richten op om het probleem is: een set voorbeeld kenmerken (functie waarden uit de Kol2 - Col40) gegeven, we voorspellen of Kol1 een 0 (geen klik) of een 1 (Klik hierop is).

Als u wilt inzoomen voorbeeld van onze trein en test gegevenssets 1% van de oorspronkelijke grootte, we gebruiken de component systeemeigen ASELECT() functie. Het volgende script [voorbeeld & #95; component & #95; criteo & #95; verkleinen & #95; trein & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) doet dit voor de gegevensset trein:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Het script [voorbeeld & #95; component & #95; criteo & #95; verkleinen & #95; test & #95; dag & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) kost deze voor testgegevens, dag\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Ten slotte het script [voorbeeld & #95; component & #95; criteo & #95; verkleinen & #95; test & #95; dag & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) kost deze voor testgegevens, dag\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Hier ziet zijn de gereed voor gebruik van onze omlaag steekproef trein en test gegevenssets voor het samenstellen van gegevensmodellen in Azure Machine Learning.

Er is een definitief belangrijk onderdeel voordat we de tabel tellen doorgaan naar Azure Machine Learning, dat wil bezwaren zeggen. In de volgende onderliggend sectie, wordt besproken dit sommige uitgebreid beschreven.

##<a name="count"></a>Een kort discussie op de tabel tellen

Zoals u hebt gezien, hebben meerdere bestaan variabelen bevatten een zeer hoog dimensionaliteit. In ons scenario presenteren we een krachtige techniek [Learning met telt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) als u wilt deze variabelen in een efficiënte, robuuste wijze coderen genoemd. Meer informatie over deze techniek is in de koppeling.

**Notitie:** In dit scenario richten we ons op het aantal tabellen gebruiken om te produceren, compacte weergaven van hoog-dimensionaal bestaan functies. Dit is niet de enige manier om te coderen bestaan functies; voor meer informatie over andere technieken, geïnteresseerd gebruikers kunnen raadplegen [één-direct-versleuteling](http://en.wikipedia.org/wiki/One-hot) en de [functie hashing](http://en.wikipedia.org/wiki/Feature_hashing).

Maak aantal tabellen op het aantal gegevens door gebruiken we de gegevens in de map onbewerkte/tellen. Klik in de sectie modellering we gebruikers laten zien hoe u deze tabellen tellen voor bestaan functies maken, kunt maken of u kunt ook een tabel te gebruiken vooraf gedefinieerde tellen voor hun explorations. In welke volgt, wanneer we verwijzen naar "vooraf gedefinieerde aantal tabellen," we betekent dat het aantal tabellen die wij bieden gebruiken. Gedetailleerde instructies voor toegang tot deze tabellen worden gegeven in het volgende gedeelte.

## <a name="aml"></a>Een model met Azure Machine Learning maken

Het bouwen van proces in Azure Machine Learning model: volgende stappen

1. [De gegevens uit tabellen component ophalen Azure Machine Learning](#step1)
2. [Het experiment maken: de gegevens wissen, kies een ingesteld en featurize met aantal tabellen](#step2)
3. [Het model trainen](#step3)
4. [Het model score op testgegevens](#step4)
5. [Het model evalueren](#step5)
6. [Het model publiceren als een webservice te worden gebruikt](#step6)

We kunnen nu klaar om modellen in Azure Machine Learning studio te maken. Onze omlaag steekproef gegevens worden opgeslagen als component tabellen in het cluster. We gebruiken de module Azure Machine Learning- **Gegevens importeren** om deze gegevens te lezen. De referenties voor toegang tot het account opslag van deze cluster vindt u in het volgende.

### <a name="step1"></a>Stap 1: Gegevens ophalen uit component tabellen in Azure Machine Learning gebruik van de module gegevens importeren en selecteert u deze voor een machine learning-experiment

Begin met het selecteren van een **+ Nieuw** -> **EXPERIMENT** -> **Lege Experiment**. Vervolgens via **het zoekvak op linksboven,** zoekt u 'Importeren-gegevens'. Slepen en neerzetten van de module **Gegevens importeren** naar het canvas experiment (het middelste deel van het scherm) als u wilt de module gebruiken voor toegang tot gegevens.

Dit is wat de **Gegevens importeren** eruitziet tijdens het ophalen van gegevens uit de tabel component:

![Gegevens importeren wordt gegevens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Voor de module **Gegevens importeren** zijn de waarden van de parameters die beschikbaar zijn in de afbeelding alleen voorbeelden van het sorteren van waarden die u wilt geven. Hier ziet u enkele algemene richtlijnen voor het invullen van de parameter ingesteld voor de module **Gegevens importeren** .

1. Kies 'Component query' voor **Gegevensbron**
2. In het vak **component databasequery's** , een eenvoudige SELECT * FROM < uw\_database\_name.your\_tabel\_naam >-genoeg is.
3. **Hcatalog server-URI**: als uw cluster 'abc' is, is dit gewoon: https://abc.azurehdinsight.net
4. **Hadoop gebruikersnaam**: de gebruikersnaam in te voeren op het moment van het bedrijf stellen het cluster gekozen. (Niet de RAS-gebruikersnaam!)
5. **Wachtwoord voor een gebruikersaccount Hadoop**: het wachtwoord voor de gebruikersnaam in te voeren op het moment van het bedrijf stellen het cluster gekozen. (Niet het wachtwoord voor externe toegang!)
6. **Locatie van uitvoergegevens**: kies "Azure"
7. **Azure-opslagaccountnaam**: het opslag-account dat is gekoppeld aan het cluster
8. **Azure opslag accountsleutel**: de sleutel van de opslag-account dat is gekoppeld aan het cluster.
9. **De containernaam van de Azure**: als de naam van de cluster is 'abc' en klikt u gewoon 'abc', maar gewoonlijk dit is.


Nadat de **Gegevens importeren** is voltooid (ziet u de groene maatstreepjes op de Module) gegevens ophalen, slaat u deze gegevens op als een gegevensset (met een naam van uw keuze). Hoe dit eruitziet:

![Gegevens importeren opslaan gegevens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Met de rechtermuisknop op de uitvoerpoort van de module **Gegevens importeren** . Dit klikt, worden een optie voor **Opslaan als gegevensset** en een optie voor het **visualiseren** . De optie **visualiseren** , als u klikt, wordt weergegeven 100 rijen van de gegevens, samen met een rechterdeelvenster dat geschikt is voor bepaalde samenvattingsgegevens. Als gegevens wilt opslaan, selecteert u **Opslaan als gegevensset** en volg de instructies.

Als u wilt selecteren de opgeslagen gegevensset voor gebruik in een experiment machine learning, zoek de gegevenssets met **het zoekvak dat wordt weergegeven in de volgende afbeelding** . Gewoon Typ de naam u de gegevensset gedeeltelijk naar toegang toe en sleep de gegevensset in het deelvenster aan de belangrijkste gegeven. Neer te zetten naar de belangrijkste deelvenster, wordt deze geselecteerd voor gebruik in machine learning-modellen.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Deze stap herhalen voor zowel de trein en de test gegevenssets. Denk eraan gebruikt u de naam van de database en tabelnamen die u hebt opgegeven voor dit doel. De waarden die worden gebruikt in de afbeelding worden uitsluitend voor afbeelding purposes.* *

### <a name="step2"></a>Stap 2: Maak een eenvoudige experiment in Azure Machine Learning te voorspellen muisklikken / geen klikken

Ons experiment Azure ML ziet er zo uit:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

We nu eens kijken hoe de belangrijkste onderdelen van dit experiment. Als een herinnering moeten we onze opgeslagen trein sleept en gegevenssets kunnen onze tekenpapier experiment eerst te testen.

#### <a name="clean-missing-data"></a>Ontbrekende gegevens opschonen

De module **Wissen.Control ontbrekende gegevens** biedt wat de naam wordt gesuggereerd: het ontbrekende gegevens op een manier die gebruiker opgegeven worden kan opschonen. Zoek in deze module, zien we dit:

![Ontbrekende gegevens wissen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

We hebben hier gekozen alle ontbrekende waarden vervangen door een 0. Er zijn andere opties als u ook, die kunnen worden bekeken door bekijken via de vervolgkeuzelijsten in de module.

#### <a name="feature-engineering-on-the-data"></a>Technische functie functie op de gegevens

Er zijn miljoenen unieke waarden voor sommige bestaan functies van grote gegevenssets. Met behulp van naïef methoden zoals voor het weergeven van dergelijke hoog dimensionaal bestaan functies één direct codering is volledig onbruikbare. In dit scenario gedemonstreerd hoe u het aantal functies met ingebouwde Azure Machine Learning-modules te genereren compacte weergaven van deze hoog dimensionaal bestaan variabelen gebruiken. Het eindresultaat is een kleinere model, training's te versnellen en prestatiestatistieken ophalen die helemaal vergelijkbaar met het gebruik van andere technieken.

##### <a name="building-counting-transforms"></a>Building transformaties tellen

Als u wilt tellen functies samenstelt, gebruiken we de module **Bouwen tellen transformeren** , die beschikbaar is in Azure Machine Learning. De module ziet er zo uit:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Belangrijke opmerking** : we Voer In het vak **aantal kolommen** de kolommen die we telt willen op uitvoeren. Dit zijn meestal (zoals genoemd) hoog dimensionaal bestaan kolommen. Aan het begin, gezegd dat de gegevensset Criteo 26 bestaan kolommen heeft: van Col15 naar Col40. Hier wordt op al deze tellen en geef de indexen (van 15 tot en met 40 gescheiden door komma's, zoals wordt weergegeven).

Gebruik van de module in de modus MapReduce (geschikt voor grote gegevenssets), we nodig hebben toegang tot een cluster HDInsight Hadoop (het account waarmee voor een uitleg van de functie opnieuw kan worden gebruikt voor dit doel ook) en de referenties. De vorige cijfers illustreren welke de ingevuld waarden eruitzien (de waarden die zijn opgegeven voor de afbeelding met die relevant zijn voor uw eigen gebruiksvoorbeeld vervangen).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

In de bovenstaande afbeelding u leert hoe de locatie van de invoer blob invoeren. Deze locatie heeft de gegevens gereserveerd voor het aantal tabellen op maken.


Nadat deze module is voltooid, kunnen we de transformatie voor later opslaan met de rechtermuisknop op de module selecteren de optie **Opslaan als transformeren** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

In onze experiment architectuur hierboven, correspondeert de gegevensset "ytransform2" exact opgeslagen tellen transformatie. Voor de rest van dit experiment Stel we dat de lezer kon u een module **Bouwen tellen transformeren** op sommige gegevens telt genereren en deze telt vervolgens kunt genereren aantal functies in de trein en test u gegevenssets.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Kiezen welke aantal functies als u wilt opnemen als onderdeel van de trein en test gegevenssets

Nadat we een telling transformeren bij de hand hebt, kan de gebruiker kunt kiezen welke functies wilt opnemen in de trein en testen van gegevenssets gebruik van de module **Parameters voor aantal wijzigen** . We kunnen alleen weergeven in deze module hier voor volledigheid, maar in belang van eenvoudig niet daadwerkelijk gebruik dit in ons experiment.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

In dit geval zoals u kunt zien, hebt we gekozen voor het gebruik van alleen de log-kans en de achtergrond uit kolom genegeerd. We kan ook parameters, zoals de drempelwaarde voor permanent opslaglocatie, hoeveel pseudo voorgaande voorbeelden om toe te voegen voor demping en of u geen geluid Laplacian of niet ingesteld. Alles dit zijn geavanceerde functies en het is om de standaardwaarden zijn een goed beginpunt voor gebruikers die nog niet eerder met dit type functie generatie.

##### <a name="data-transformation-before-generating-the-count-features"></a>Gegevenstransformatie voordat u de functies aantal genereert

Nu we een belangrijk punt over transformeren onze trein te belichten en gegevens voordat u daadwerkelijk genereert aantal functies testen. Houd er rekening mee dat er twee **R-Script uitvoeren** modules gebruikt voordat we het aantal transformeren op onze gegevens toepassen.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Hier ziet u het eerste R-script:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


In dit script R Wijzig we onze kolomnamen in namen 'Kol1' naar 'Col40'. Dit is omdat de transformatie tellen namen van deze indeling verwacht.

In het tweede R script we saldo vanaf de verdeling tussen positieve en negatieve klassen (respectievelijk klassen 1 en 0) door de negatieve klas downsampling. Het script R Hier ziet u hoe u dit wilt doen:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

In dit eenvoudige R-script, gebruiken we "pos\_neg\_verhouding" voor het instellen van de hoeveelheid saldo tussen het positieve en negatieve klassen. Dit is belangrijk te doen omdat de prestatievoordelen verbeteren class onevenwicht meestal heeft voor classificatie problemen waar de klasse-verdeling is vervormd (intrekken dat in ons geval we 3,3% positieve class en negatieve class 96,7 hebben %).

##### <a name="applying-the-count-transformation-on-our-data"></a>Het aantal-transformatie toepassen van onze gegevens

Ten slotte kunt we de module **Transformatie toepassen** gebruiken om het aantal transformaties toepassen in onze trein en gegevenssets testen. Deze module wordt de transformatie opgeslagen tellen als één invoer en de trein of test gegevenssets als de andere invoer en geeft als resultaat gegevens met functies tellen. Hier wordt aangegeven:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Een fragment welke functies tellen eruit

Het is leerzaam te zien hoe de functies aantal eruit in onze hoofdletters/kleine letters. Hier geven we een uittreksel van dit:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

In dit fragment ziet dat voor de kolommen die we op geteld, we de telt ophalen en meld u kans naast alle relevante backoffs.

We gaan nu een Azure Machine Learning-model met deze getransformeerd gegevenssets maken. Klik in de volgende sectie leert hoe u dit kunt doen.

#### <a name="azure-machine-learning-model-building"></a>Azure Machine Learning-model bouwen

##### <a name="choice-of-learner"></a>Keuze ingesteld

Eerst moeten we kiest u een ingesteld. We gaan gebruiken een beslissingsstructuur twee class verhoogd als onze ingesteld. Hier volgen de standaardopties voor deze ingesteld:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

We gaan kiest u de standaardwaarden voor onze experiment. We u ziet dat de standaardwaarden meestal zinvolle zijn en een goede manier om snel basislijnen op de prestaties. U kunt op de prestaties verbeteren door parameters verstrekkende als u wilt wanneer u een basislijn hebt.

#### <a name="train-the-model"></a>Het model trainen

Voor training roepen we gewoon een module **Trein Model** . De twee invoer voor dit zijn de twee-klasse verhoogd beslissingsstructuur ingesteld en onze gegevensset trein. Dit is hier weergegeven:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Het model score

Als we een ervaren model hebben, zijn wij gereed is voor het verkrijgen van op de toets gegevensset en de prestaties wordt berekend. We doen dit door het gebruik van de **Score Model** module weergegeven in de volgende afbeelding, samen met een module **Evalueren Model** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Stap 5: Het model evalueren

Ten slotte, willen we model prestaties analyseren. Meestal is een goede eenheid op twee class (binair) classificatie problemen, het AUC. Als u wilt visualiseren dit, aansluiten we de module **Score Model** een module **Evalueren Model** hiervoor. **Visualiseren** klikken op de module **Evalueren Model** levert een afbeelding, zoals de volgende:

![Module BDT model evalueren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

In de binaire (of twee class) classificatie problemen, een goede maateenheid voor de nauwkeurigheid tekstvoorspelling is het gebied onder Curve (AUC). In het volgende, weergeven we onze resultaten die met dit model op onze test gegevensset. Als u dit, met de rechtermuisknop op de uitvoerpoort van de module **Evalueren Model** en klik vervolgens **visualiseren**.

![Evalueren Model module visualiseren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Stap 6: Het model publiceren als een webservice
De mogelijkheid om een Azure Machine Learning-model publiceren als webservices met ten minste vrij eenvoudig is een zeer handige functie voor het maken van deze schaal beschikbaar moet zijn. Als dat is ingevuld, kan iedereen bellen naar de webservice met invoergegevens dat zij nodig hebben voorspellingen voor en de webservice maakt gebruik van het model om terug te keren die voorspellingen.

Klik hiertoe we eerst onze ervaren model opslaan als een object ervaren Model. Hiermee wordt uitgevoerd door met de rechtermuisknop op de module **Trein Model** en het gebruik van de optie **Opslaan als ervaren Model** .

Vervolgens moet maken van de invoer en poorten voor onze webservice uitvoer:

* een invoer poort gaat gegevens in hetzelfde formulier als de gegevens die we nodig voorspellingen voor
* een uitvoerpoort geeft als resultaat de Labels van een score voorzien en de bijbehorende kansen.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Selecteer een paar rijen met gegevens voor de invoer poort

Is het handig zijn alleen 10 rijen moet fungeren als de poortgegevens van de invoer selecteren met een **SQL-transformatie toepassen** -module. Selecteer alleen deze rijen met gegevens voor onze invoer poort met de SQL-query die u hier kunt zien:

![Poortgegevens van de invoer](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webservice
We kunnen nu een kleine experiment die kan worden gebruikt voor het publiceren van onze webservice uitvoeren.

#### <a name="generate-input-data-for-webservice"></a>Genereren van invoergegevens voor webservice

Als een stap zeroth aangezien groot is, is de tabel tellen we een paar regels testgegevens duren en genereren uitvoergegevens met functies tellen. Dit kan dienen als de invoergegevens-indeling voor onze webservice. Dit is hier weergegeven:

![De invoergegevens BDT maken](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Voor de invoergegevens-indeling gebruiken we nu de uitvoer van de module **Tellen Featurizer** . Wanneer dit experiment is voltooid, de uitvoer opslaan vanuit de module **Tellen Featurizer** als een gegevensset. Deze Dataset wordt gebruikt voor de invoergegevens in de webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Experiment scoren voor publicerende webservice

We geven eerst hoe dit eruitziet. De structuur van de essentiële is een **Score Model** -module die accepteert onze ervaren model-object en een paar regels invoergegevens die we in de vorige stappen gebruik van de module **Tellen Featurizer** gegenereerd. We gebruiken "Kolommen in gegevensset Selecteer" aan project de etiketten Scored en de waarschijnlijkheid Score.

![Selecteer de kolommen in de gegevensset](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Zoals u ziet hoe de module **Kolommen selecteren in de gegevensset** kan worden gebruikt voor het 'uitgefilterd' gegevens uit een gegevensset. We hier de inhoud kunnen weergeven:

![Filteren met de kolommen selecteren in de gegevensset module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Als u de blauwe invoer- en uitvoerpoorten, klikt u op **voorbereiden webservice** rechtsonder. Dit experiment ook kunt wij de webservice publiceren: klik op het pictogram **WEBSERVICE publiceren** rechtsonder wat hier wordt getoond:

![Webservice publiceren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Als de webservice is gepubliceerd, krijgen we omgeleid naar een pagina die er dus:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Twee koppelingen voor webservices zien we aan de linkerkant:

* De **Aanvraag en respons** Service (of records) is bedoeld voor één voorspellingen en we gebruiken in deze workshop is.
* De **BATCH EXECUTION** Service (BES) wordt gebruikt voor batch voorspellingen en is vereist dat de ingevoerde gegevens gebruikt om te maken van voorspellingen bevinden zich in Azure-blobopslag.

Klikken op de koppeling **Die aanvraag en respons** ons leidt naar een pagina die ontstaat blik code in C#, python en R. vooraf Deze code kan gemakkelijk worden gebruikt voor het plaatsen van oproepen naar de webservice. Houd er rekening mee dat de API-sleutel op deze pagina moet worden gebruikt voor verificatie.

Is het handig kunt u deze code python via kopiëren naar een nieuwe cel in het notitieblok IPython.

Hier leert een segment van python code met de juiste API-sleutel.

![Python code](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Houd er rekening mee dat we de standaard-API-sleutel door van onze webservices API-sleutel vervangen. Te klikken op **uitvoeren** op deze cel in een notitieblok IPython levert het volgende antwoord:

![IPython antwoord](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

U ziet dat voor de twee test voorbeelden die wordt gesteld over (in de JSON-kader van het script python), we weer in het formulier antwoorden "Scored etiketten, Scored kansen". Houd er rekening mee dat in dit geval we hebben gekozen de standaardwaarden die vooraf blik biedt (0 voor alle numerieke kolommen en de tekenreeks 'waarde' voor alle bestaan kolommen).

Hiermee wordt onze end-to-end-scenario laat zien hoe u omgaat met grootschalige gegevensset met Azure Machine Learning afgesloten. We de slag met een terabyte van gegevens, gebouwd als een model tekstvoorspelling en deze als een webservice in de cloud geïmplementeerd.
