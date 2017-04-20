<properties
    pageTitle="Scalable gegevens wetenschappelijke in Azure gegevens Lake: een end-to-end-scenario | Microsoft Azure"
    description="Het gebruik van Azure gegevens Lake moet gegevens verkennen en binaire indeling taken op een gegevensset."  
    services="machine-learning"
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
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Scalable gegevens wetenschappelijke in Azure gegevens Lake: een end-to-end-scenario

In dit scenario wordt uitgelegd hoe u Lake van Azure-gegevens die u moet u doen verkennen en taken van de binaire classificatie van een steekproef van de tevens taxi reis en ritbedrag gegevensset om te voorspellen al dan niet een tip worden betaald door een tarief. Deze begeleidt u bij de stappen van het [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess), end-to-end, uit overname van de gegevens naar het modelleren van training en vervolgens naar de implementatie van een webservice waarmee het model worden gepubliceerd.


### <a name="azure-data-lake-analytics"></a>Azure gegevens Lake Analytics

De [Microsoft Azure-gegevens Lake](https://azure.microsoft.com/solutions/data-lake/) heeft alle mogelijkheden vereist om ervoor zorgen dat eenvoudig gegevens wetenschappers voor de opslag van gegevens van elke grootte, de vorm en de snelheid en om te leiden gegevensverwerking, geavanceerde analyses en machine learning-modellering met hoge schaalbaarheid op een efficiënte manier.   U kunt op basis van het per taak, betalen alleen wanneer de gegevens daadwerkelijk wordt verwerkt. Azure gegevens Lake Analytics I-SQL bevat, en een taal waarbij de declaratieve aard van SQL met de duidelijke kracht van C# te leveren scalable distributed query mogelijkheid. Dit kunt u proces ongestructureerde gegevens volgens schema op gelezen, aangepaste logica en de gebruiker gedefinieerde functies (UDF) invoegen en uitbreiden om in te schakelen prima korrelig controle over het uitvoeren van bij het op schaal bevat. Meer informatie over het ontwerpfilosofie achter I-SQL, raadpleegt u [Visual Studio blogbericht](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Gegevens Lake Analytics is ook een belangrijk onderdeel van Cortana Analytics-Suite en het werkt met Azure SQL Data Warehouse, Power BI en gegevens Factory. Hiermee kunt u een volledige cloud big data en de platform van geavanceerde analyses.

In dit scenario wordt gestart door te beschrijven van de vereisten en bronnen die nodig zijn voor de taken voltooien met gegevens Lake Analytics die vormen het proces van de wetenschappelijke gegevens en hoe deze worden geïnstalleerd. En vervolgens deze worden de gegevensverwerking stappen beschreven die met U SQL en die eindigt met laat zien hoe u Python en component gebruiken met Azure Machine Learning Studio voor bouwen en implementeren van de blog modellen. 


### <a name="u-sql-and-visual-studio"></a>I-SQL en Visual Studio

In dit scenario raadt Visual Studio I-SQL-scripts te verwerken van de gegevensset bewerken. De I-SQL-scripts zijn die hier worden beschreven en die beschikbaar zijn in een afzonderlijk bestand. Het proces bestaat uit ingesting, verkennen en steekproef van de gegevens. Ook ziet u hoe u een taak I-SQL-script uitvoeren vanuit de Azure-portal. Component tabellen worden gemaakt voor de gegevens in een gekoppeld HDInsight cluster om het bouwen en de implementatie van een binaire classificatie-model in Azure Machine Learning Studio te vergemakkelijken.  


### <a name="python"></a>Python

In dit scenario bevat ook een sectie die laat hoe zien voor bouwen en implementeren van een blog model Python gebruikt met Azure Machine Learning Studio.  Wij bieden een Jupyter-notitieblok met de scripts Python voor deze stappen in dit proces. Het notitieblok bevat code voor enkele extra functie technische stappen en modellen bouwen zoals multiclass classificatie en regressie modeling naast het binaire classificatie-model hier beschreven. De taak regressie is het bedrag van de tip op basis van andere functies tip voorspellen. 


### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Studio wordt gebruikt om te bouwen en implementeren van de blog modellen. Dit gebeurt met twee methoden: eerst met Python scripts en klik vervolgens met de component tabellen op een cluster HDInsight (Hadoop).


### <a name="scripts"></a>Scripts

Alleen de belangrijkste stappen worden beschreven in dit scenario. U kunt de volledige **I-SQL-script** en **Jupyter notitieblok** downloaden van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Vereisten voor

Voordat u deze onderwerpen, hebt u het volgende:

- Een Azure-abonnement. Als u nog een, raadpleegt u [de gratis proefversie Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- (Aanbevolen) Visual Studio 2013 of 2015. Als u nog een van deze versies is geïnstalleerd, kunt u een gratis Community-editie van [hier](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)downloaden. Klik op de knop **downloaden Community 2015** onder de sectie Visual Studio. 

>[AZURE.NOTE] In plaats van Visual Studio, kunt u ook de Azure-Portal gebruiken om in te dienen Azure gegevens Lake query's. Wij bieden instructies over het toelaten zowel met Visual Studio en klik op de portal in de sectie **procesgegevens met U-SQL**. 

- Registratie voor Gegevensvoorbeeld Lake van Azure

>[AZURE.NOTE] Moet u goedkeuring Azure gegevens Lake Store (ADLS) en Azure gegevens Lake Analytics (ADLA) gebruiken als deze services, in de proefversie van worden verkrijgen. U wordt gevraagd om u te registreren als u uw eerste ADLS of ADLA maakt. Als u wilt sigh omhoog, klikt u op **om een voorbeeld te registreren**, lees de gebruiksrechtovereenkomst en klik op **OK**. Hier is bijvoorbeeld de aanmelding ADLS pagina:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Gegevens wetenschappelijke omgeving voor Lake van Azure-gegevens voorbereiden
Als u wilt de gegevens wetenschappelijke omgeving voor deze procedure voorbereiden, maken de volgende bronnen:

- Azure Lake gegevensopslag (ADLS) 
- Azure gegevens Lake Analytics (ADLA)
- Azure Blob storage-account
- Azure Machine Learning Studio-account
- Azure Data Lake Tools voor Visual Studio (aanbevolen)

In deze sectie bevat instructies voor het maken van elk van deze resources. Als u wilt gebruiken component tabellen met Azure Machine Learning, in plaats van Python, om te maken van een gegevensmodel, moet ook u voor het inrichten van een cluster HDInsight (Hadoop). Deze alternatieve procedure in in de desbetreffende sectie hieronder beschreven.
<br/>
>AZURE. Opmerking de **Azure Lake gegevensopslag** kunt maken op afzonderlijk of bij het maken van de **Azure gegevens Lake Analytics** als de standaard-opslag. Instructies voor het maken van elk van deze afzonderlijk onderstaande bronnen wordt verwezen, maar de gegevens Lake opslag-account nodig niet afzonderlijk worden gemaakt.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Een Azure-gegevensopslag Lake maken

Een ADLS maken van de [Azure-Portal](http://portal.azure.com). Zie voor meer informatie [een HDInsight cluster met Lake gegevensopslag maken met behulp van Azure-Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u voor het instellen van de identiteit van de AAD Cluster in het blad **gegevensbron** van het **Optionele configuratie** blad daar beschreven. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Maak een Azure gegevens Lake Analytics-account
Maak een account ADLA van de [Azure-Portal](http://portal.azure.com). Zie voor meer informatie [Zelfstudie: aan de slag met Azure gegevens Lake analyses met behulp van Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Maak een account van Azure Blob storage
Een Azure Blob storage-account maken van de [Azure-Portal](http://portal.azure.com). Zie het maken van een sectie voor het account van opslag in [over Azure opslag accounts](../storage/storage-create-storage-account.md)voor meer informatie.
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Een Azure Machine Learning Studio-mailaccount instellen
Meld u aan omhoog/naar Azure Machine Learning Studio van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -pagina. Klik op de knop **nu aan de slag** en kies vervolgens een "Gratis werkruimte" of "Standaard werkruimte". Na dit is mogelijk experimenten maken in Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Installeren van hulpmiddelen voor Lake Azure-gegevens (aanbevolen)
Hulpmiddelen voor Lake Azure-gegevens voor uw versie van Visual Studio van [Azure Data Lake Tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)installeren.

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Nadat de installatie voltooid is, opent u omhoog Visual Studio. Hier ziet u de gegevens Lake tabblad van het menu aan de bovenkant. Uw Azure resources moeten worden weergegeven in het linkerdeelvenster wanneer u zich bij uw Azure-account aanmeldt.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>De gegevensset tevens Taxi reizen
De gegevensset die we hier gebruikt, is een openbaar gegevensset--de [tevens Taxi reizen gegevensset](http://www.andresmh.com/nyctaxitrips/). De gegevens tevens Taxi reis bestaat uit ongeveer 20GB gecomprimeerde CSV-bestanden (~ 48GB niet gecomprimeerd), vastleggen van meer dan 173 miljoen afzonderlijke reizen en de tarieven betaald voor elke reis. Elke record reis bevat de locaties ophalen en inleverbibliotheek en tijden, anoniem hack (stuurprogramma van) licentie getal en het getal straten (taxi van unieke id). De gegevens behandelt alle reizen in het jaar 2013 en is opgegeven in de volgende twee gegevenssets voor elke maand:

 - Trip_data CSV bevat reis informatie, zoals het aantal personen, ophalen en dropoff punten reis duur en duur van reis. Hier volgen een paar steekproef records:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - Het 'trip_fare' CSV bevatten informatie over het tarief dat is betaald voor elke reis zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en het totale bedrag dat is betaald. Hier volgen een paar steekproef records:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel voor deelname aan reis\_gegevens en reis\_tarief bestaat uit de volgende drie velden: straten, hack\_licentie en ophalen van\_datetime. De onbewerkte CSV-bestanden zijn toegankelijk vanaf een openbare opslag van Azure blob. Het I-SQL-script voor deze join is in de sectie [Join reis en tarief tabellen](#join) .

## <a name="process-data-with-u-sql"></a>Gegevens met U-SQL verwerken

De gegevensverwerking taken geïllustreerde in deze sectie zijn ingesting, kwaliteitscontrole, verkennen en steekproef van de gegevens. We ook hoe tabellen reis en tarief samenvoegen. De laatste sectie ziet uitvoeren een script I-SQL-taak van de Azure-portal. Hier vindt u koppelingen naar elke subsectie:

- [Opname van gegevens: lezen in de gegevens uit openbare blob](#ingest)
- [Gegevens kwaliteit controles](#quality)
- [Gegevens verkennen](#explore)
- [Tabellen reis en tarief samenvoegen](#join)
- [Gegevens steekproeven](#sample)
- [I-SQL-taken uitvoeren](#run)

De I-SQL-scripts zijn die hier worden beschreven en die beschikbaar zijn in een afzonderlijk bestand. U kunt de volledige **I-SQL-scripts** downloaden van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Als u wilt uitvoeren I-SQL, Open Visual Studio, klikt u op **Bestand--> Nieuw--> Project**, **I-SQL-Project**, naam en bewaart u deze naar een map kiezen.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Het is mogelijk gebruik van de Portal Azure I-SQL in plaats van Visual Studio uitvoeren. U kunt navigeren naar de resource Azure gegevens Lake Analytics op de portal en verzenden van query's rechtstreeks als geïllustreerd in de volgende afbeelding.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Opname van gegevens: Lezen in de gegevens uit openbare blob

De locatie van de gegevens in de Azure blob wordt verwezen als **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** en kunnen worden geëxtraheerd **Extractors.Csv()**gebruiken. Vervangt door uw eigen de containernaam van de en opslagaccountnaam in de volgende scripts voor container_name@blob_storage_account_name in het adres wasb. Aangezien de bestandsnamen in dezelfde indeling, kunnen we gebruiken **reis\_data_ {\*\}.csv** te lezen in alle 12 reis-bestanden. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Aangezien er koppen in de eerste rij, moeten we de kopteksten verwijderen en wijzigen van kolomtypen in gewenste eenheden. We kunnen beide opslaan de verwerkte gegevens naar Azure Lake gegevensopslag **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ gebruiken of naar Azure-blobopslag-gebruikt account **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

We kan op dezelfde manier lezen in de gegevensgroepen tarief. Klik met de rechtermuisknop op Azure Lake gegevensopslag, kunt u kiezen om uw gegevens in de **Portal van Azure--> Data Explorer** of **File Explorer** binnen Visual Studio te bekijken. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Gegevens kwaliteit controles

Nadat de tabellen reis en tarief zijn gelezen, kunnen gegevens kwaliteit controles worden uitgevoerd in de volgende manier. De resulterende CSV-bestanden kunnen worden uitgevoerd naar Azure-blobopslag of Azure Lake gegevensopslag. 

Het aantal medallions en unieke aantal medallions zoeken:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Deze medallions die meer dan 100 reizen had vinden:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Deze ongeldige records met pickup_longitude zoeken:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Ontbrekende waarden voor sommige variabelen vinden:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Gegevens verkennen

We kunt enkele verkennen als u een beter begrip van de gegevens doen.

De verdeling van Gekantelde en niet-Gekantelde reizen vinden:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

De verdeling van tip bedrag met deadline waarden zoeken: 0,5,10 en 20 dollars.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Eenvoudige statistieken van reis afstand vinden:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

De percentielen van reis afstand vinden:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Tabellen reis en tarief samenvoegen

Tabellen reis en tarief kunnen worden verbonden door straten, hack_license en pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Voor elk niveau van personen tellen, het aantal records, gemiddelde tip bedrag, afwijking van tip bedrag, percentage van Gekantelde reizen te berekenen.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Gegevens steekproeven

We selecteert willekeurig u eerst 0,1% van de gegevens uit de gekoppelde tabel:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

We Voer vervolgens de toepassing stratificatie steekproeven door binaire variabele tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>I-SQL-taken uitvoeren

Wanneer u klaar bent met het bewerken van de I-SQL-scripts, kunt u deze naar de server met uw account Azure gegevens Lake Analytics verzenden. Op **Gegevens Lake** **Baan Submit**selecteren uw **Analytics-Account**, kiest u **parallellisme**en klikt u op de knop **verzenden** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Wanneer de taak is is voldaan, wordt de status van uw taak worden weergegeven in Visual Studio voor controle. Nadat de taak is voltooid, kunt u zelfs opnieuw afspelen van het proces van taak kan worden uitgevoerd en zoek de knelpunt stappen uit om de efficiëntie van uw project te verbeteren. U kunt ook gaan naar Azure-Portal om te controleren van de status van uw taken U-SQL.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


U kunt nu de uitvoerbestanden in Azure-blobopslag of Azure-Portal controleren. We gebruiken de toepassing stratificatie voorbeeldgegevens voor onze modellering in de volgende stap.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Bouwen en implementeren van gegevensmodellen in Azure Machine Learning

Gedemonstreerd twee opties die beschikbaar zijn voor u om gegevens te halen in Azure Machine Learning maken en 

- In de eerste optie die u kunt gebruiken van de steekproef gegevens die naar een Azure Blob (in de **gegevens steekproeven** stap hierboven) is geschreven en Python voor bouwen en implementeren van modellen van Azure Machine Learning gebruikt. 
- In de tweede optie query u de gegevens in Azure gegevens Lake rechtstreeks met een query component. Deze optie is vereist dat u een nieuw HDInsight cluster maken of gebruiken van een bestaand HDInsight cluster waar de tabellen component wijst u de gegevens za Taxi in Azure Lake gegevensopslag.  Beide deze onderstaande opties besproken. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Optie 1: Gebruik Python voor bouwen en implementeren machine learning-modellen

Als u wilt bouwen en implementeren van machine learning-modellen met Python, door een notitieblok Jupyter te maken in uw lokale computer of in Azure Machine Learning Studio. Het Jupyter notitieblok weergegeven op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) bevat de volledige code als u wilt verkennen, visualiseren gegevens, functie voor technische, modellering en implementatie. In dit artikel ziet alleen de modellering en de implementatie. 

### <a name="import-python-libraries"></a>Python bibliotheken importeren

Script-bestand, de volgende Python pakketten nodig zijn om te kunnen uitvoeren van de steekproef Jupyter notitieblok of de Python. Als u de service AzureML notitieblok gebruikt, zijn deze pakketten vooraf geïnstalleerd.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Lezen in de gegevens uit blob

- Verbindingsreeks   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Lezen in als tekst

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Kolomnamen toevoegen en scheidt u kolommen

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Aantal kolommen wijzigen in numerieke

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Machine learning-modellen maken

Hier maken we een model binaire indeling om te voorspellen of een reis is Gekantelde al dan niet. Klik in het notitieblok Jupyter u andere twee modellen kunt vinden: multiclass classificatie en regressie-modellen.

- Eerst moeten we maken pop variabelen die kunnen worden gebruikt in scikit-modellen meer

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Gegevensframe voor de modellering maken

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Training en testen van 60-40 splitsen

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistische regressie training instellen

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Testen gegevensverzameling score

        Y_test_pred = logit_fit.predict(X_test)

- Evaluatie van de doelstellingen berekenen

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Web Service API maken en deze gebruiken in Python

Wij willen het machine learning-model nadat deze is opgebouwd mogelijk te maken. We gebruiken de binaire logistieke model hier als voorbeeld. Zorg ervoor dat de scikit-Ervaar versie in uw lokale computer 0.15.1. U hoeft niet te bang dit als u Azure ML studio service.

- Uw referenties werkruimte van Azure ML studio instellingen zoeken. Klik op **Instellingen**in Azure Machine Learning Studio --> **naam** --> **Autorisatie Tokens**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Webservice maken

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Web Servicereferenties ophalen

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Web service API belt. U moet wachten 5-10 seconden na de vorige stap.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Optie 2: Maak en implementeer modellen rechtstreeks in Azure Machine Learning

Azure Machine Learning Studio gegevens rechtstreeks vanuit de gegevensopslag Lake Azure kunt lezen en vervolgens worden gebruikt om te maken en implementeren van modellen. Deze methode component tabel gebruikt die naar de Azure Lake gegevensopslag verwijst. Hiervoor is vereist dat deze is ingericht op een aparte Azure HDInsight cluster worden gebruikt, klik op de tabel component wordt gemaakt. De volgende secties wordt aangegeven hoe u dit wilt doen. 

### <a name="create-an-hdinsight-linux-cluster"></a>Een HDInsight Linux Cluster maken

Maak een HDInsight Cluster (Linux) van de [Azure-Portal](http://portal.azure.com). Zie de sectie **een HDInsight cluster met toegang tot de gegevensopslag Lake Azure maken** in [een HDInsight cluster met Lake gegevensopslag maken met behulp van Azure-Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)voor meer informatie.

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Component tabel maken in HDInsight

Nu maken we component-tabellen moeten worden gebruikt in Azure Machine Learning Studio in het gebruik van de gegevens die zijn opgeslagen in de gegevensopslag Lake Azure in de vorige stap HDInsight-cluster. Ga naar het HDInsight cluster net hebt gemaakt. Klik op **Instellingen** --> **Eigenschappen** --> **Cluster AAD identiteit** --> **ADLS Access**, Controleer of uw Azure Lake gegevensopslag-account is toegevoegd in de lijst met lezen, schrijven en uitvoeren van rechten. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Klik op **Dashboard** naast de knop **Instellingen** en een venster wordt weergegeven. Klik op **Beeld component** in de rechterbovenhoek rechterhoek van de pagina en u ziet het **Query-Editor**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Plak de volgende component scripts om een tabel te maken. De locatie van de gegevensbron is in de verwijzing Azure Lake gegevensopslag op deze manier: **adl://data_lake_store_name.azuredatalakestore.net:443/mapnaam/pad**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Wanneer de query is voltooid, ziet u de resultaten als volgt:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Bouwen en implementeren van gegevensmodellen in Azure Machine Learning Studio

We gaan nu bouwen en implementeren van een model dat voorspellen = al dan niet een tip met Azure Machine Learning is betaald. De toepassing stratificatie voorbeeldgegevens is klaar voor gebruik in deze binaire indeling (tip al dan niet) probleem. De blog modellen met multiclass classificatie (tip_class) en regressie (tip_amount) kunnen ook worden gemaakt en geïmplementeerd met Azure Machine Learning Studio, maar hier alleen u leert hoe u omgaat met de hoofdletters/kleine letters met het model binaire indeling.

1. De gegevens ophalen van Azure ML gebruik van de module **Gegevens importeren** in de sectie **invoer van gegevens en uitvoer** . Zie de pagina met [gegevens importeren module](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) naslaginformatie voor meer informatie.
2. Selecteer **Component Query** als **gegevensbron** in het deelvenster **Eigenschappen** .
3. Plak de volgende component script in de editor **component databasequery 's**

        select * from nyc_stratified_sample;

4. Voer de URI van HDInsight cluster (dit kan worden gevonden in de Portal van Azure), de Hadoop-referenties, de locatie van de uitvoergegevens en naam/sleutel/container van Azure opslagaccountnaam.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Een voorbeeld van een binaire indeling experiment het lezen van gegevens uit de tabel component wordt weergegeven in de onderstaande afbeelding.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Nadat het experiment is gemaakt, klikt u op **Webservice instellen** --> **Blog webservice**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Voer de automatisch gemaakte experiment scoren nadat deze is voltooid, klikt u op **Webservice implementeren**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Het dashboard van de service web kort weergegeven:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Overzicht

U kunt een wetenschappelijke-omgeving van gegevens voor het samenstellen van scalable end-to-end-oplossingen in Azure gegevens Lake hebt gemaakt door de procedure is voltooid. Deze omgeving is wordt gebruikt voor het analyseren van een grote openbare gegevensset, laat dit door de canonieke stappen voor het proces van het wetenschappelijke gegevens, uit gegevens overname tot en met model training en vervolgens naar de implementatie van het model als een webservice. I-SQL is gebruikt om te verwerken, te verkennen en de voorbeeldgegevens. Python en component zijn met Azure Machine Learning Studio voor bouwen en implementeren van blog modellen gebruikt.

## <a name="whats-next"></a>Hoe nu verder?

Leerpad voor het [Team gegevens wetenschap proces (TDSP)](http://aka.ms/datascienceprocess) vindt u koppelingen naar informatie over elke stap in het proces van geavanceerde analyses. Er zijn een reeks walkthroughs gespecificeerde op de pagina [Team gegevens wetenschap proces walkthroughs](data-science-process-walkthroughs.md) over het gebruik van resources en services in verschillende blog analytics-scenario's:

- [Het proces van het Team gegevens wetenschappelijke in actie: SQL Data Warehouse gebruiken](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Het proces van het Team gegevens wetenschappelijke in actie: HDInsight Hadoop clusters gebruiken](machine-learning-data-science-process-hive-walkthrough.md)
- [Het Team gegevens wetenschap proces: SQL Server gebruiken](machine-learning-data-science-process-sql-walkthrough.md)
- [Overzicht van de gegevens wetenschap proces met een op Azure HDInsight](machine-learning-data-science-spark-overview.md)

