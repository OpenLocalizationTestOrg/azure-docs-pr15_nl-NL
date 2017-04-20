<properties
    pageTitle="Overzicht van gegevens wetenschappelijk met een op Azure HDInsight | Microsoft Azure"
    description="De toolkit elektrische MLlib Hiermee aanzienlijke machine learning modeling mogelijkheden aan de gedistribueerde HDInsight-omgeving."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Overzicht van gegevens wetenschappelijk met een op Azure HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Deze reeks onderwerpen ziet u hoe u algemene gegevens wetenschappelijke taken zoals gegevens opname, functie engineering, modellering en model evaluatie uitvoeren met HDInsight Spark. De gegevens die worden gebruikt, is een voorbeeld van de 2013 tevens taxi reis en tarief gegevensset. De modellen voor ingebouwd zijn logistieke en lineaire regressie, willekeurige forests en kleurovergang gestimuleerd bomen. De onderwerpen weergeven ook opslaan van deze modellen in Azure-blobopslag (WASB) en hoe u score en hun blog prestaties evalueren. Meer geavanceerde onderwerpen besproken hoe modellen kunnen worden ervaren met cross-validatie en hyper-parameter sweeping. In dit overzichtsonderwerp worden ook het instellen van het elektrische cluster die u moet de stappen in de drie scenario's beschreven. 

[Elektrische](http://spark.apache.org/) is een open source parallel processing die ondersteuning biedt voor de verwerking in het geheugen om zo de prestaties van groot-gegevens analytische toepassingen. Elektrische verwerking-engine is geoptimaliseerd voor snelheid, gebruiksgemak, en geavanceerde analytische. Van een in het geheugen verdeelde berekening mogelijkheden kunnen u een goede keuze voor iteratieve algoritmen in machine learning en graph berekeningen. [MLlib](http://spark.apache.org/mllib/) is van een scalable machine learning-bibliotheek die modelleringsmogelijkheden naar deze gedistribueerde omgeving brengt. 

[HDInsight elektrische](../hdinsight/hdinsight-apache-spark-overview.md) is een open source Azure gehoste aangeboden. Ook is er ondersteuning voor **Jupyter PySpark notitieblokken** in het elektrische cluster dat een SQL-interactieve query's voor transformeren, filteren en visualiseren-gegevens die zijn opgeslagen in Azure BLOB's (WASB) kan worden uitgevoerd. PySpark is de API Python voor een. De codefragmenten bieden en ziet u de relevante waarnemingspunten de gegevens die hier worden uitgevoerd in Jupyter notitieblokken die zijn geïnstalleerd op een clusters kunt visualiseren. De stappen modellering in deze onderwerpen bevatten code die ziet u hoe u trainen, evalueren, opslaan en gebruiken van elk type model. 

De instellingsstappen en code die beschikbaar zijn in dit scenario is voor HDInsight 3.4 elektrische 1,6. De code hier en in de notitieblokken is echter algemene en moet werken aan een cluster elektrische. Als u niet HDInsight Spark gebruikt, is het mogelijk dat de cluster-instelling en beheer stappen iets anders wat hier wordt weergegeven.

## <a name="prerequisites"></a>Vereisten voor

1. u moet een Azure-abonnement hebben. Als u nog een, raadpleegt u [de gratis proefversie Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. u moet een cluster HDInsight 3.4 elektrische 1,6 om te voltooien van deze procedure. Als u wilt maken, raadpleegt u de instructies in [aan de slag: een Apache maken op Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). De clustertype en versie is opgegeven in het menu **Cluster Type selecteren** . 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Zie de [gegevens wetenschappelijke Scala met een op Azure gebruiken](machine-learning-data-science-process-scala-walkthrough.md)voor een onderwerp dat wordt uitgelegd hoe u Scala in plaats van Python om taken voor een end-to-end gegevens wetenschap proces te voltooien.

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>De gegevens tevens 2013 Taxi

De gegevens tevens Taxi reis is ongeveer 20 GB gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48 GB niet gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke reizen en de tarieven betaald voor elke reis. Elke record reis bevat de ophalen en afgiftepunt en tijd, anoniem hack (stuurprogramma van) licentie getal, en straten (unieke id van taxi) getal. De gegevens behandelt alle reizen in het jaar 2013 en is opgegeven in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten reis details, zoals het aantal personen, verdergaan en dropoff gegevenspunten, krachtvoertuigen duur en duur van reis. Hier volgen een paar steekproef records:

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


We hebben een steekproef 0,1% van deze bestanden die u hebt gemaakt en deelnemen aan de reis\_gegevens en reis\_ritbedrag CVS bestanden in een één gegevensset wilt gebruiken als de invoer gegevensset voor deze procedure. De unieke sleutel voor deelname aan reis\_gegevens en reis\_tarief bestaat uit de velden: straten, hack\_certificaat en ophalen van\_datetime. Elke record van de gegevensset bevat de volgende kenmerken dat staat voor een reis tevens Taxi:


|Veld| Korte beschrijving
|------|---------------------------------
| straten |Anoniem taxi straten (unieke taxi id)
| hack_license |    Anoniem Hackney regeleinden licentie getal
| vendor_id |   Taxi leverancier-id
| rate_code | TEVENS taxi rentabiliteit tarief
| store_and_fwd_flag | Store- en doorsturen vlag
| pickup_datetime | Verdergaan datum / tijd
| dropoff_datetime | Dropoff datum / tijd
| pickup_hour | Verdergaan uur
| pickup_week | Verdergaan week van het jaar
| weekdag | Weekday (het bereik 1 tot en met 7)
| passenger_count | Aantal personen in een reis taxi
| trip_time_in_secs | Reis tijd in seconden
| trip_distance | Afgelegde in mijl reis-afstand
| pickup_longitude | Verdergaan lengtegegevens
| pickup_latitude | Breedtegraad verdergaan
| dropoff_longitude | Dropoff lengtegegevens
| dropoff_latitude | Dropoff breedtegraad
| direct_distance | Afstand tussen Kies rechtstreekse omhoog en dropoff locaties
| payment_type | Betalingstype (cas, creditcard enz.)
| fare_amount | Tarief bedrag in
| extra kosten | Extra kosten
| mta_tax | MTA belasting
| tip_amount | Tip bedrag
| tolls_amount | Tolgelden bedrag
| total_amount | Totaalbedrag
| Gekantelde | Gekantelde (0-1 voor Nee of Ja)
| tip_class | Tip class (0: $0, 1: $0 tot en met 5, 2: $6-10, 3: $11-20, 4: > $20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Code uit een notitieblok Jupyter op het cluster elektrische uitvoeren 

U kunt het notitieblok Jupyter van de Azure-portal op te starten. Zoek uw cluster elektrische op uw dashboard en klikt u erop om in te voeren beheerpagina voor uw cluster. Als u wilt dat is gekoppeld aan het cluster een notitieblok wordt geopend, klikt u op **Cluster Dashboards** -> **Jupyter notitieblok** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

U kunt ook naar ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** voor toegang tot de notitieblokken Jupyter bladeren. Het gedeelte CLUSTERNAAM van deze URL vervangen door de naam van uw eigen cluster. Moet u het wachtwoord voor uw beheerdersaccount voor toegang tot de notitieblokken.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Selecteer PySpark om een map met een paar voorbeelden van vooraf worden verpakt notitieblokken met de API PySpark weer te geven. De notitieblokken met de voorbeelden van de code voor deze suite van een onderwerp vindt u op [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


U kunt de notitieblokken rechtstreeks vanuit Github op de server Jupyter-notitieblok op uw cluster elektrische uploaden. Klik op de startpagina van uw Jupyter, op de knop **uploaden** op de juiste deel van het scherm. Een Verkenner wordt geopend. Hier kunt u de URL van de Github (onbewerkte-inhoud) van het notitieblok plakken en klik op **openen**. De notitieblokken PySpark zijn beschikbaar op de volgende URL's:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Ziet u de bestandsnaam op uw lijst met Jupyter bestanden met de knop **uploaden** opnieuw. Klik op deze knop **uploaden** . Nu kunt u het notitieblok hebt geïmporteerd. Herhaal deze stappen als u wilt uploaden van de andere notitieblokken in deze procedure.

> [AZURE.TIP] U kunt met de rechtermuisknop op de koppelingen in uw browser en kies **Koppeling kopiëren** naar de URL github onbewerkte inhoud. U kunt deze URL in het uploaden van Jupyter explorer bestandsdialoogvenster plakken.

U kunt nu het volgende doen:

- Zie de code door te klikken op het notitieblok.
- Elke cel uitvoeren door op **SHIFT + ENTER**te drukken.
- Het hele notitieblok uitvoeren door te klikken op de **cel** -> **uitvoeren**.
- Gebruik de automatische visualisatie van query's.

> [AZURE.TIP] De kernel PySpark gevisualiseerd automatisch de uitvoer van (HiveQL) SQL-query's. Krijgt u de optie kiezen uit verschillende typen visualisaties (tabel, cirkel-, lijn, gebied of balk) met behulp van het **Type** menuknoppen in het notitieblok:

![Logistische regressie ROC curve voor algemene methode](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Hoe nu verder?

Nu u worden ingesteld met een cluster HDInsight Spark en de notitieblokken Jupyter hebt geüpload, bent u gereed om te werken via de onderwerpen die met de drie PySpark-notitieblokken overeenkomen. Ze hoe u uw gegevens verkennen en klik vervolgens op hoe u maken en gebruiken van modellen worden weergegeven. Het geavanceerde gegevens verkennen en modellering notitieblok ziet hoe u meerdere-validatie, hyper-parameter verstrekkende, opnemen en modelleren van evaluatie. 

**Verkennen en modellering met een:** De gegevensset verkennen en maken, score en de machine learning-modellen door te werken via het onderwerp [maken binaire indeling en regressie-modellen voor gegevens met de toolkit elektrische MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) evalueren.

**Model verbruik:** Als u wilt weten hoe u de indeling en regressie modellen die zijn gemaakt in dit onderwerp score, raadpleegt u [Score evalueert in een ingebouwd machine learning-modellen](machine-learning-data-science-spark-model-consumption.md).

**Cross-validatie en hyperparameter verstrekkende**: Zie [Geavanceerde verkennen en modellering met een](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen training cross-validatie en hyper-parameter sweeping gebruiken

