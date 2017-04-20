<properties
    pageTitle="Gegevens wetenschappelijke Scala en uitgerust met op Azure | Microsoft Azure"
    description="Het gebruik van Scala voor gecontroleerde machine learning-taken met de elektrische scalable MLlib en een ML pakketten op een cluster Azure HDInsight elektrische."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Gegevens wetenschappelijke Scala en elektrische op Azure gebruiken

In dit artikel leest u hoe u Scala voor gecontroleerde machine learning-taken met de elektrische scalable MLlib en een ML pakketten op een cluster Azure HDInsight elektrische gebruikt. Deze begeleidt u bij de taken die de [gegevens wetenschap proces](http://aka.ms/datascienceprocess)vormen: gegevens opname en uitleg, visualisatie functie engineering, modellering en model verbruik. De modellen in het artikel zijn logistieke en lineaire regressie willekeurig forests en kleurovergang verhoogd bomen (GBTs), behalve twee algemene gecontroleerde machine learning-taken:

- Regressie probleem: voorspelling van het bedrag van tip ($) voor een reis taxi
- Binaire indeling: voorspelling van tip of geen tip (1/0) voor een reis taxi

Het proces modellering vereist training en evaluatie van een gegevensverzameling test en relevante nauwkeurigheid aan de doelstellingen. In dit artikel leert u hoe u het opslaan van deze modellen in Azure-blobopslag en hoe u score en hun blog prestaties evalueren. In dit artikel worden ook de meer geavanceerde onderwerpen van modellen met behulp van cross-gegevensvalidatie en hyper-parameter sweeping optimaliseren. De gegevens die worden gebruikt, is een voorbeeld van een 2013 tevens taxi reis en tarief gegevensverzameling beschikbaar op GitHub.

[Scala](http://www.scala-lang.org/), een taal op basis van de Java virtual machine, geïntegreerd concepten van de object-georiënteerd en functionele taal. Dit is een scalable taal dat wordt vooral geschikt voor gedistribueerde verwerking in de cloud en op een van de Azure clusters wordt uitgevoerd.

[Elektrische](http://spark.apache.org/) is een open source parallel-verwerking framework die ondersteuning biedt voor de verwerking in het geheugen om zo de prestaties van grote gegevens analytics-toepassingen. De elektrische verwerking-engine is geoptimaliseerd voor snelheid, gebruiksgemak, en geavanceerde analytische. Van een in het geheugen verdeelde berekening mogelijkheden kunnen u een goede keuze voor iteratieve algoritmen in machine learning en graph berekeningen. Het pakket [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) biedt een uniform reeks op hoog niveau API's die zijn gebouwd gegevens frames waarmee u kunt maken en afstemmen praktische machine learning-pijpleidingen. [MLlib](http://spark.apache.org/mllib/) is van een scalable machine learning-bibliotheek, komt modelleringsmogelijkheden deze gedistribueerde omgeving.

[HDInsight elektrische](../hdinsight/hdinsight-apache-spark-overview.md) is Azure gehoste aangeboden open source elektrische. Ook ondersteuning voor Jupyter Scala notitieblokken op het cluster elektrische bevat en een SQL-interactieve query's om te transformeren, filteren en gegevens die zijn opgeslagen in Azure-blobopslag visualiseren kan worden uitgevoerd. De Scala codefragmenten in dit artikel bieden en ziet u de relevante waarnemingspunten om de gegevens anders worden uitgevoerd in Jupyter notitieblokken die zijn geïnstalleerd op een clusters. De stappen modellering in deze onderwerpen hebben code die u hoe ziet u trainen, evalueren, opslaan en gebruiken van elk type model.

De instellingsstappen en code in dit artikel zijn bedoeld voor Azure HDInsight 3.4 elektrische 1,6. De code in dit artikel en klik in het [Scala Jupyter notitieblok](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) zijn echter algemene en moet werken aan een cluster elektrische. Het is mogelijk dat de cluster-instelling en beheer stappen iets anders dan wat wordt weergegeven in dit artikel als u niet HDInsight Spark gebruikt.

> [AZURE.NOTE] Zie voor een onderwerp dat u hoe ziet u Python in plaats van Scala gebruiken om taken voor een end-to-end gegevens wetenschap proces te voltooien, [Gegevens wetenschappelijk met een op Azure HDInsight](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Vereisten voor

-   U moet een Azure-abonnement hebben. Als u niet één, [een gratis evaluatieversie van Azure ophalen hebt nog](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   U moet een cluster Azure HDInsight 3.4 elektrische 1,6 naar de volgende procedures uitvoeren. Als u wilt een cluster maakt, raadpleegt u de instructies in [aan de slag: Apache elektrische maken op Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Stel de clustertype en versie in het menu **Cluster Type selecteren** .

![De configuratie van het type voor HDInsight-cluster](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Zie voor een beschrijving van de tevens taxi reis gegevens en instructies over het uitvoeren van code uit een notitieblok Jupyter op het cluster elektrische, de betreffende secties in een [Overzicht van gegevens wetenschappelijk met een op Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Scala code uit een notitieblok Jupyter op het cluster elektrische uitvoeren

U kunt een notitieblok Jupyter van de Azure-portal op te starten. Zoek het cluster elektrische op uw dashboard en klikt u vervolgens op als u wilt de beheerpagina voor uw cluster invoeren. Klik vervolgens op **Cluster Dashboards**en klik vervolgens op **Jupyter notitieblok** om het notitieblok dat is gekoppeld aan het cluster elektrische te openen.

![Cluster dashboard en Jupyter notitieblokken](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

U kunt Jupyter notitieblokken op https:// ook openen&lt;clusternaam&gt;.azurehdinsight.net/jupyter. *Clusternaam* vervangen door de naam van uw cluster. Moet u het wachtwoord voor uw beheerdersaccount voor toegang tot de notitieblokken Jupyter.

![Ga naar Jupyter notitieblokken met behulp van de naam van het cluster](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Selecteer **Scala** om een map met een paar voorbeelden van voorverpakte notitieblokken met de API PySpark weer te geven. De uitleg Modeling en score Scala.ipynb notitieblok met de voorbeelden van de code voor deze reeks elektrische onderwerpen is beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


U kunt het notitieblok rechtstreeks vanuit GitHub op de server Jupyter notitieblok op uw cluster elektrische uploaden. Klik op de knop **uploaden** op uw startpagina Jupyter. Plak de URL van de GitHub (onbewerkte-inhoud) van het notitieblok Scala in de Verkenner en klik vervolgens op **openen**. Het notitieblok Scala is beschikbaar op de volgende URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Installatie: Een vooraf ingestelde en component contexten, een magics en een bibliotheken

### <a name="preset-spark-and-hive-contexts"></a>Vooraf ingestelde elektrische en component contexten

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


De elektrische kernels die worden geleverd met Jupyter notitieblokken hebben vooraf ingestelde contexten. U hoeft niet expliciet instellen de elektrische of component contexten voordat u begint met het werken met de toepassing u ontwikkelt. De vooraf ingestelde contexten zijn:

- `sc`voor SparkContext
- `sqlContext`voor HiveContext


### <a name="spark-magics"></a>Elektrische magics

De kernel elektrische vindt u enkele vooraf gedefinieerde "magics," welke zijn speciale opdrachten die u met bellen kunt `%%`. Twee van deze opdrachten worden in de volgende voorbeelden van de code gebruikt.

- `%%local`Hiermee wordt opgegeven dat de code in de volgende regels lokaal wordt uitgevoerd. De code moet geldige Scala code.
- `%%sql -o <variable name>`voert een query component tegen `sqlContext`. Als de `-o` parameter wordt overschreden, wordt het resultaat van de query is doorgevoerd in de `%%local` Scala context als een elektrische gegevensframe.

Voor meer informatie over de kernels voor Jupyter notitieblokken en hun vooraf gedefinieerde "magics" dat u belt met `%%` (bijvoorbeeld `%%local`), raadpleegt u [Kernels beschikbaar voor Jupyter notitieblokken met HDInsight elektrische Linux clusters op HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Bibliotheken importeren

De elektrische, MLlib en andere bibliotheken die u nodig hebt met behulp van de volgende code importeren.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Opname van gegevens

De eerste stap in het proces gegevens wetenschappelijk is aan het nemen van de gegevens die u wilt analyseren. U kunt de gegevens uit externe bronnen of systemen betreffende doen in uw gegevens verkennen en modellering-omgeving. In dit artikel is de gegevens die u nemen een gekoppelde 0,1 steekproef van de taxi reis en tarief-bestand (opgeslagen als een bestand .tsv). De gegevens verkennen en modellering omgeving is een. In deze sectie bevat de code om te voltooien van de volgende reeks taken:

1. Stel directory paden voor de opslag van gegevens en model.
2. Lees meer in de invoer gegevensset (opgeslagen als een bestand .tsv).
3. Definiëren van een schema voor de gegevens en de gegevens wissen.
4. Een kader wachtrij is opgeschoond gegevens maken en deze in het geheugen in cache.
5. De gegevens als een tijdelijke tabel in SQLContext registreren.
6. Query in de tabel en de resultaten in een gegevensframe te importeren.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Directory paden voor opslaglocaties instellen in Azure-blobopslag

Elektrische kunt lezen en schrijven met Azure-blobopslag. U kunt een gebruiken voor het verwerken van uw bestaande gegevens en op te slaan de resultaten weer in blobopslag.

Om op te slaan modellen of bestanden in-blobopslag, moet u het pad correct opgeven. Verwijzen naar de standaard-container die zijn bijgevoegd bij het cluster uitgerust met behulp van een pad dat met begint `wasb:///`. Verwijzen naar andere locaties met behulp van `wasb://`.

De locatie van de invoergegevens om te worden gelezen en het pad naar blobopslag die is gekoppeld aan het cluster een opslaglocatie van het model Hiermee geeft u het volgende voorbeeld.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Gegevens importeren, een RDD maken en een gegevensframe op basis van het schema definiëren

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 8 seconden.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Query uitvoeren op de tabel en resultaten in een gegevensframe importeren

Vervolgens query in de tabel voor tarief, personen en gegevens van de tip; uitfilteren beschadigd en grote gegevens; en verschillende rijen afdrukken.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Uitvoer:**

fare_amount|passenger_count|tip_amount|Gekantelde
-----------|---------------|----------|------
       13,5|            1.0|       2,9|   1.0
       16,0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Gegevens verkennen en visualisatie

Nadat u de gegevens naar een overbrengen, is de volgende stap in het proces gegevens wetenschappelijk te krijgen een beter begrip van de gegevens door te verkennen en visualisatie. In deze sectie, kunt u de gegevens taxi controleren met behulp van de SQL-query's. Importeer de resultaten in een gegevensframe wilt uitzetten van de doelsite variabelen en potentiële functies voor visueel onderzoek met behulp van de functie automatisch-visualisatie van Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Lokaal en SQL bijzonder gebruiken om te gegevens uitzetten

Standaard is de uitvoer van een codefragment die u uit een notitieblok Jupyter uitvoert beschikbaar is binnen de context van de sessie dat behouden is gebleven op de knooppunten werknemer. Als u wilt een reis opslaan naar de werknemer knooppunten voor elke berekenen en als u alle gegevens die u nodig hebt voor uw berekening lokaal op het Jupyter serverknooppunt (dit is het hoofd knooppunt) beschikbaar is, kunt u de `%%local` bijzonder het codefragment uitvoeren op de server Jupyter.

- **SQL bijzonder** (`%%sql`). De kernel HDInsight Spark ondersteunt eenvoudig inline HiveQL query's op SQLContext. De (`-o VARIABLE_NAME`) van de argumenten zich blijft voordoen de uitvoer van de SQL-query als een frame Pandas gegevens op de server Jupyter. Dit betekent waarna steeds beschikbaar is in de lokale modus.
- `%%local`**bijzonder**. De `%%local` bijzonder wordt uitgevoerd de code lokaal op de server Jupyter, dat wil het hoofd knooppunt van de cluster HDInsight zeggen. Meestal gebruikt u `%%local` magische in combinatie met de `%%sql` magische met de `-o` parameter. De `-o` parameter zou de uitvoer van de SQL-query lokaal opgeslagen en kiest u vervolgens `%%local` bijzonder zou de volgende set codefragment lokaal uitvoeren ten opzichte van de uitvoer van de SQL-query's die lokaal behouden is gebleven activeren.

### <a name="query-the-data-by-using-sql"></a>De gegevens met behulp van de SQL-query
Deze query haalt de reizen taxi door tarief bedrag, personen tellen en tip bedrag.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

In de volgende code, de `%%local` bijzonder Hiermee maakt u een lokale gegevensframe, klikt u sqlResults. U kunt sqlResults gebruiken om te tekenen met behulp van matplotlib.

> [AZURE.TIP] Lokale bijzonder wordt meerdere keren gebruikt in dit artikel. Voorbeeld om te maken van een gegevensframe dat past in het lokale geheugen als uw gegevensset groot is, is.

### <a name="plot-the-data"></a>De gegevens uitzetten

U kunt uitzetten met behulp van Python code, wanneer het gegevensframe is ingevoegd in de lokale context als een frame Pandas-gegevens.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 De kernel elektrische gevisualiseerd automatisch de uitvoer van (HiveQL) SQL-query's nadat u de code hebt uitgevoerd. U kunt kiezen uit diverse soorten visualisaties:
 
- Tabel
- Cirkel
- Lijn
- Gebied
- Balk

Hier ziet u de code wilt uitzetten van de gegevens:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Uitvoer:**

![Tip bedrag histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tip bedrag door personen tellen](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip bedrag met tarief bedrag](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Maken van de functies en functies transformeren, klikt u vervolgens gegevens en voorbereiden om invoer in modeling functies

Voor modellering structuur gebaseerde functies uit een ML en MLlib moet u doel- en -functies met behulp van verschillende technieken, zoals binning, indexering, één direct codering en vectorization voorbereiden. Hier volgen de procedures volgen in deze sectie:

1. Maak een nieuwe functie door **binning** uur in verkeer tijd gerangschikte verzamelingen.
2. **Indexering en één direct codering** toepassen voor functies van bestaan.
3. **Steekproef en gesplitste de gegevensverzameling** in breuken van training en test.
4. **Geef training variabele en functies**, en maakt u geïndexeerde of één direct codering training en testen invoer gelabelde punt robuuste distributed gegevenssets (RDDs) of gegevensframes.
5. Automatisch **categoriseren en de functies en doelen vectorize** wilt gebruiken als invoer voor machine learning-modellen.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Een nieuwe functie maken door binning uur tot verkeer tijd gerangschikte verzamelingen

Deze code ziet u hoe u een nieuwe functie door binning uur in verkeer tijd gerangschikte verzamelingen maakt en hoe u de resulterende gegevensframe in het geheugen in cache. Waar RDDs en gegevens frames zo vaak worden gebruikt, caching leidt tot verbeterde execution tijden. Dienovereenkomstig gewijzigd, moet u RDDs en gegevensframes cache in verschillende stadia in de volgende procedures.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexering en één direct codering van bestaan functies

De modellering en voorspellen functies van MLlib vereisen functies met bestaan invoergegevens moeten worden geïndexeerd of codering vóór gebruiken. In dit gedeelte ziet u hoe u indexeren of bestaan functies om invoer in de functies modellering coderen.

U wilt indexeren of coderen van uw modellen op verschillende manieren, afhankelijk van het model. Logistieke en lineaire regressie modellen vereisen bijvoorbeeld één direct codering. Bijvoorbeeld, kan een functie met drie categorieën worden uitgevouwen in drie kolommen van de functie. Elke kolom bevat 0 of 1 afhankelijk van de categorie van een opmerking. De functie [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) biedt MLlib voor het coderen van één direct. Een kolom met het label indexen worden koppelt deze encoder aan een kolom met binaire vectoren met maximaal één één-waarde. Met deze codering, kunnen de algoritmen die in numerieke waarden en dit type functies, zoals logistische regressie verwachten worden toegepast op bestaan functies.

Hier kunt u alleen vier variabelen als u wilt weergeven van voorbeelden die tekenreeksen transformeren. U kunt ook andere variabelen, zoals weekdag, voorgesteld door numerieke waarden, als bestaan variabelen index.

Voor het indexeren, gebruikt u `StringIndexer()`, en gebruiken voor het coderen van één direct, `OneHotEncoder()` functies in MLlib. Hier ziet u de code wilt indexeren en coderen bestaan functies:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 4 seconden.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Steekproef en gesplitste de gegevensverzameling in breuken van training en testen

Deze code maakt een steekproeven van de gegevens (25%, in dit voorbeeld). Hoewel steekproeven niet vereist voor dit voorbeeld vanwege de grootte van een gegevensverzameling is, het artikel leest u hoe u kunt voorbeeld, zodat u hoe weet u deze gebruiken voor uw eigen problemen als dat nodig is. Wanneer voorbeelden groot zijn, kan dit aanzienlijk tijd besparen terwijl u modellen trainen. Splitsen vervolgens in het voorbeeld in een training delen (75%, in dit voorbeeld) en een testen (25%, in dit voorbeeld) als u wilt gebruiken in de indeling en regressie-modellen.

Een willekeurig getal (tussen 0 en 1) toevoegen aan elke rij (in een kolom "ASELECT") die kan worden gebruikt om te selecteren cross-validatie vouwen tijdens training.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 2 seconden ingedrukt.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Training variabelen en functies, en maak geïndexeerde of één direct codering van training en testen van de invoer van gegevens of punt RDDs frames het label

In deze sectie bevat code die u hoe ziet u de index bestaan tekstgegevens als een gegevenstype gelabelde punt, en deze coderen zodat u deze gebruiken kunt om te trainen en MLlib logistische regressie en andere classificatie-modellen testen. Gelabelde point-objecten zijn RDDs die zijn opgemaakt op een manier die nodig is als invoergegevens door de meeste van machine learning-algoritmen in MLlib. Een [punt het label](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, op compacte of verspreid die is gekoppeld aan een label/reactie.

In deze code geeft u de doeltoepassing (afhankelijke) variabele en de functies gebruiken om te trainen modellen. Maakt u geïndexeerde of één direct codering van training en testen van de invoer van gegevens of punt RDDs frames het label.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 4 seconden.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatisch categoriseren en de functies en doelen wilt gebruiken als invoer voor machine learning-modellen vectorize

Gebruik een ML om de doeltoepassing en functies die u kunt gebruiken in de boomstructuur gebaseerde modellering functies te categoriseren. De code is voltooid twee taken:

-   Hiermee maakt u een binaire doel voor classificatie door het toewijzen van een waarde van 0 of 1 aan elk gegevenspunt tussen 0 en 1 met behulp van de drempelwaarde van 0,5.
- Automatisch gecategoriseerd functies. Als het aantal unieke numerieke waarden voor elke functie kleiner is dan 32 is, is deze functie gecategoriseerd.

Hier ziet u de code voor deze twee taken.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binaire indeling model: voorspellen of een tip moet worden betaald

In deze sectie, moet u drie soorten modellen binaire indeling te voorspellen al dan niet een tip moet worden betaald maken:

- Een **logistische regressiemodel** met behulp van de ML elektrische `LogisticRegression()` functie
- Een **willekeurig bos classificatie model** met behulp van de ML elektrische `RandomForestClassifier()` functie
- Een **kleurovergang prestatieverbetering structuur classificatie model** met behulp van de MLlib `GradientBoostedTrees()` functie

### <a name="create-a-logistic-regression-model"></a>Een model logistische regressie maken

Maak vervolgens een model logistische regressie met behulp van de ML elektrische `LogisticRegression()` functie. U maken het model-code in een reeks stappen samenstellen:

1. Gegevens van de **trein het model** met één parameter instellen.
2. **Evalueren het model** op een toets gegevensverzameling met aan de doelstellingen.
3. **Sla het model** in blobopslag voor toekomstige verbruik.
4. **Het model score** tegen testgegevens.
5. **De resultaten uitzetten** met ontvanger kenmerk (ROC) bochten besturingsomgeving.

Hier ziet u de code voor deze procedures:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Laden, score en de resultaten opslaan.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC op testgegevens = 0.9827381497557599


Gebruik Python op lokale Pandas gegevensframes wilt uitzetten van de kromme ROC.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Uitvoer:**

![Tip of geen tip ROC curve benaderen](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Een willekeurig bos classificatie model maken

Maak vervolgens een willekeurig bos classificatie model met behulp van de ML elektrische `RandomForestClassifier()` , functie en vervolgens het model op testgegevens evalueren.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC op testgegevens = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Maken van een GBT classificatie-model

Maak vervolgens een GBT classificatie model met behulp van MLlib `GradientBoostedTrees()` , functie en vervolgens het model op testgegevens evalueren.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Uitvoer:**

Gebied onder ROC curve: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressiemodel: voorspellen tip bedrag

In deze sectie, moet u twee soorten modellen regressie te voorspellen van het schuifblok wordt verplaatst tip maken:

- Een **lineaire regressiemodel geregulariseerd** met behulp van de ML elektrische `LinearRegression()` functie. U bespaart het model en het model op testgegevens evalueren.
- Een **boomstructuur regressie kleurovergang verhogen** met behulp van de ML elektrische `GBTRegressor()` functie.


### <a name="create-a-regularized-linear-regression-model"></a>Maken van een lineaire regressie geregulariseerd-model

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 13 seconden.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Uitvoer:**

R-sqr op testgegevens = 0.5960320470835743


Vervolgens de testresultaten query als een gegevensframe en AutoVizWidget en matplotlib gebruiken om te visualiseren deze.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

De code maakt u een frame lokale gegevens van de queryuitvoer en de gegevenspunten. De `%%local` bijzonder Hiermee maakt u een lokale gegevensframe, klikt u `sqlResults`, die u wilt uitzetten met matplotlib kunt gebruiken.

>[AZURE.NOTE] In dit bijzonder elektrische wordt meerdere keren gebruikt in dit artikel. Als de hoeveelheid gegevens groot is, moet u voorbeeld om te maken van een gegevensframe dat past in het lokale geheugen.

Maak waarnemingspunten met behulp van Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Uitvoer:**

![Tip bedrag: Werkelijke versus voorspelde](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Maken van een GBT regressie-model

Een GBT regressiemodel maken met behulp van de ML elektrische `GBTRegressor()` , functie en vervolgens het model op testgegevens evalueren.

[Kleurovergang verhoogd bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles beslissing bomen. GBTs trainen beslissingsbomen iteratief te minimaliseren ten een functie verlies. U kunt GBTs gebruiken voor regressie en classificatie. Ze bestaan functies kunnen worden verwerkt, is niet vereist schaalbaarheid van de functie en nonlinearities en functie interacties kunnen vastleggen. Ook kunt u deze in een multiclass-classificatie-instelling.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Test-R-sqr is: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Geavanceerde modellering hulpprogramma's voor optimalisatie

In deze sectie gebruikt u machine learning-hulpprogramma's die ontwikkelaars vaak voor optimalisatie van het model gebruikt. U kunt specifieke taken machine learning-modellen drie verschillende manieren optimaliseren met behulp van de parameter sweeping en cross-verificatie:

-   De gegevens in de trein en validatie sets splitsen, optimaliseren van het model met behulp van hyper-parameter sweeping op een trainingsset en evalueren op een set validatie (lineaire regressie)
-   Het model optimaliseren met behulp van cross-validatie en hyper-parameter verstrekkende met behulp van een ML CrossValidator-functie (binaire classificatie)
-   Het model optimaliseren met behulp van aangepaste cross-validatie en parameter-sweeping code voor het gebruik van een machine learning-functie en parameter instellen (lineaire regressie)


**Cross-validatie** is een techniek die bepaalt hoe u ook een model op een bekende reeks gegevens training als u wilt de functies van gegevenssets waarop deze is niet getraind voorspellen wordt generalize. De algemene idee achter deze methode is dat een model is training in een gegevensverzameling met bekende gegevens en vervolgens de nauwkeurigheid van de voorspellingen een onafhankelijke gegevensverzameling is getest. Een algemene implementatie is moet worden verdeeld van een gegevensverzameling over *k*-vouwen en klik vervolgens trainen het model op een wijze round robin op alle maar een van de vouwen.

**Optimalisatie van Hyper-parameter** is het probleem van het kiezen van een reeks hyper-parameters voor een algoritme learning meestal met het doel van het optimaliseren van een maateenheid voor de algoritme van de prestaties van een onafhankelijke gegevensverzameling. Als u een hyper-parameter is een waarde die u buiten de procedure model-training opgeven moet. Hypothesen over hyper-parameterwaarden invloed kunnen zijn op de flexibiliteit en de nauwkeurigheid van het model. Besluit bomen hebben bijvoorbeeld hyper-parameters, zoals de gewenste diepteas en het aantal bladen in de structuur. Een term misclassification boete voor een ondersteuning vector machine (SVM), moet u instellen.

Een algemene manier om uit te voeren optimalisatie van hyper-parameter is een raster zoeken, ook wel een **parameter opschonen**gebruiken. In een zoekopdracht raster, wordt een uitgebreide gezocht tot en met de waarden van een opgegeven subset van de ruimte hyper-parameter voor een algoritme onderwijs. Cross-validatie kunt een meting prestaties sorteren van de beste resultaten geproduceerd door de algoritme van de zoekopdracht raster opgeven. Als u meerdere validatie hyper-parameter sweeping gebruikt, kunt u limiet problemen, zoals een model naar trainingsgegevens overfitting. Op deze manier het model behoudt de capaciteit toe te passen op de algemene reeks gegevens waaruit de trainingsgegevens die is gehaald.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Een lineaire regressiemodel met hyper-parameter sweeping optimaliseren

Vervolgens splitsen gegevens in de trein en validatie sets gebruiken hyper-parameter verstrekkende op een trainingsset te optimaliseren van het model en klik op een set validatie (lineaire regressie) als resultaat.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Test-R-sqr is: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Het model binaire indeling met behulp van cross-validatie en hyper-parameter sweeping optimaliseren

In dit gedeelte ziet u hoe u een model binaire indeling met behulp van cross-validatie en hyper-parameter sweeping optimaliseren. Deze maakt gebruik van de ML elektrische `CrossValidator` functie.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd in beslag van de cel: 33 seconden.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Het model lineaire regressie met behulp van aangepaste cross-validatie en parameter-sweeping code optimaliseren

Vervolgens het model met behulp van aangepaste code optimaliseren en de beste Modelparameters met behulp van het criterium van hoogste nauwkeurigheid identificeren. Maak vervolgens het uiteindelijke model, het model op testgegevens evalueren en sla het model in blobopslag. Ten slotte het model te laden, testgegevens score en nauwkeurigheid evalueren.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Uitvoer:**

Tijd in beslag van de cel: 61 seconden.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Een ingebouwd machine learning-modellen automatisch met Scala gebruiken

Zie voor een overzicht van onderwerpen waarmee u de taken waaruit het proces gegevens wetenschappelijke in Azure wordt aangegeven, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

[Team gegevens wetenschap proces walkthroughs](data-science-process-walkthroughs.md) worden andere end-to-end-scenario's die de stappen in het proces van het Team gegevens wetenschappelijke voor specifieke scenario's demonstreren. De walkthroughs illustreren ook hoe u het combineren van cloud en on-premises hulpprogramma's en services in een werkstroom of verkooppijplijn een intelligente toepassing maken.

[Score een ingebouwd machine learning-modellen](machine-learning-data-science-spark-model-consumption.md) ziet u hoe u met Scala code automatisch laden en nieuwe gegevensgroepen score met machine learning-modellen een ingebouwd en opgeslagen in Azure-blobopslag. U kunt de instructies te volgen en gewoon de code Python vervangen door Scala-code in dit artikel voor geautomatiseerde verbruik.
