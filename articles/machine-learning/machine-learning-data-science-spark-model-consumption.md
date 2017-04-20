<properties
    pageTitle="Een ingebouwd machine learning-modellen score | Microsoft Azure"
    description="Hoe u score learning modellen die zijn opgeslagen in Azure Blob Storage (WASB)."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Een ingebouwd machine learning-modellen score 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In dit onderwerp wordt beschreven hoe u het laden van machine learning (ML) modellen die zijn ingebouwd met een MLlib en opgeslagen in Azure Blob Storage (WASB) en hoe u ze met gegevenssets die ook zijn opgeslagen in WASB score. Deze ziet u hoe u de invoergegevens vooraf worden verwerkt, transformeren functies met de functies indexing en codering in de toolkit MLlib, en het maken van een object gelabelde punt gegevens die kan worden gebruikt als invoer voor het scoren met de ML-modellen. De modellen die worden gebruikt voor het scoren opnemen lineaire regressie, logistische regressie willekeurig bos-modellen en kleurovergang verhogen structuur-modellen.


## <a name="prerequisites"></a>Vereisten voor

1. Moet u een Azure-account en een HDInsight elektrische u moet een HDInsight 3.4 elektrische 1,6 cluster om te voltooien van deze procedure. Bekijk het [Overzicht van gegevens wetenschappelijk met een op Azure HDInsight](machine-learning-data-science-spark-overview.md) voor instructies over het voldoen aan deze vereisten is voldaan. Dit onderwerp bevat ook een beschrijving van de tevens 2013 Taxi gegevens hier gebruikt en instructies voor hoe u de code uit een notitieblok Jupyter op het cluster elektrische uitvoeren. Het **pySpark-machine-learning-data-science-spark-model-consumption.ipynb** notitieblok met de voorbeelden van de code in dit onderwerp is beschikbaar in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. U moet ook de machine learning-modellen te beoordelen hier door te werken door de [gegevens verkennen en modellering met een](machine-learning-data-science-spark-data-exploration-modeling.md) onderwerp maken.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Installatie: opslaglocaties, bibliotheken en de context van de vooraf ingestelde elektrische

Elektrische kan lezen en schrijven naar een Azure opslag Blob (WASB). Zodat uw bestaande gegevens opgeslagen kunnen er worden verwerkt met een en de resultaten weer in WASB worden opgeslagen.

Als u wilt opslaan modellen of bestanden in WASB, moet het pad correct worden opgegeven. De standaard-container die zijn bijgevoegd bij het cluster elektrische kan worden verwezen met een pad die begint met: *"wasb / / '*. Het volgende voorbeeld wordt de locatie van de gegevens die moeten worden gelezen en het pad voor het model opslag-map waartoe de uitvoer model wordt opgeslagen. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory paden voor opslaglocaties instellen in WASB

Modellen worden opgeslagen in: "wasb: / / / gebruikers/remoteuser/NYCTaxi/modellen". Als dit pad is niet juist ingesteld, worden niet modellen voor het scoren geladen.

De scored resultaten zijn opgeslagen: "wasb: / / / gebruikers/remoteuser/NYCTaxi/ScoredResults". Als het pad naar de map onjuist is, worden resultaten worden niet opgeslagen in die map.   


>[AZURE.NOTE] Het pad bestandslocaties kunnen worden gekopieerd en geplakt in de tijdelijke aanduidingen in deze code uit de resultaten van de laatste cel van het notitieblok dat **machine-learning-data-science-spark-data-exploration-modeling.ipynb** .   


Hier ziet u de code voor het instellen van directory paden: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**UITVOER:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Bibliotheken importeren

Een context instellen en importeren nodig bibliotheken met de volgende code

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Vooraf ingestelde een context en PySpark magics

De PySpark kernels die worden geleverd met Jupyter notitieblokken hebben een vooraf ingestelde context. Dus u hoeft niet voor het instellen van de elektrische of component contexten expliciet voordat u begint met het werken met de toepassing u ontwikkelt. Deze zijn beschikbaar voor u al dan niet standaard. Volgende contexten zijn:

- SC - voor een 
- sqlContext - voor component

De kernel PySpark vindt u enkele vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met bellen kunt %%. Er zijn twee die opdrachten die in deze codevoorbeelden worden gebruikt.

- **%% lokale** Opgegeven dat de code in de volgende regels lokaal wordt uitgevoerd. Code moet geldige Python code.
- **%% sql -o<variable name>** 
- Hiermee voert u een query component ten opzichte van de sqlContext. Als de parameter -o wordt doorgegeven, het resultaat van de query is doorgevoerd in de %% lokale Python context als een dataframe Pandas.
 

Voor meer informatie over de kernels voor Jupyter notitieblokken en de vooraf gedefinieerde "magics" die ze bieden, raadpleegt u [Kernels beschikbaar voor Jupyter notitieblokken met HDInsight elektrische Linux clusters op HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Gegevens nemen en een gegevensframe wachtrij is opgeschoond maken

In deze sectie bevat de code voor een reeks taken die zijn vereist voor het nemen van de gegevens die moeten worden van een score voorzien. Lezen in een gekoppelde 0,1 voorbeeld van de taxi reis en tarief bestand (opgeslagen als een bestand .tsv), opmaak de gegevens en maakt vervolgens een gegevensframe wissen.Control.

De taxi reis en tarief-bestanden zijn die zijn gekoppeld op basis van de procedure in de: [het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md) onderwerp.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 46.37 seconden


## <a name="prepare-data-for-scoring-in-spark"></a>Gegevens voorbereiden voor het scoren in een 

In dit gedeelte ziet hoe u de index, coderen en schaal bestaan functies voor gebruik in MLlib die onder toezicht learning algoritmen voor de indeling en regressie voor te bereiden.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Functie transformatie: index en coderen bestaan functies om invoer in modellen voor het scoren 

In dit gedeelte ziet u hoe u het indexeren bestaan gegevens met behulp van een `StringIndexer` en functies met coderen `OneHotEncoder` in de modellen voor ingevoerd.

De [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) Hiermee wordt een kolom tekenreeks met etiketten op een kolom met het label indexen worden gecodeerd. De indexen worden geordend door label frequentie. 

De [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) wordt een kolom met het label indexen worden toegewezen aan een kolom met binaire vectoren, met maximaal één één-waarde. Deze codering kunt algoritmen die in continue waarden functies, zoals logistische regressie verwachten, moeten worden toegepast om bestaan functies.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 5,37 seconden


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>RDD objecten met een functie matrices om invoer in modellen maken

In deze sectie bevat code waarin wordt uitgelegd hoe bestaan tekstgegevens als een object RDD indexeren en deze een direct coderen zodat deze kan worden gebruikt om te trainen en MLlib logistische regressie en structuur gebaseerde modellen testen. De geïndexeerde gegevens wordt opgeslagen in de objecten [Robuuste Distributed gegevensset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Hierna ziet u de eenvoudige onttrekking in een. Een object RDD vertegenwoordigt een onveranderlijke, gepartitioneerde verzameling elementen die kunnen worden beheerd op parallel met een.

Het ook code bevat die ziet u hoe u de schaal van gegevens met de `StandardScalar` MLlib gekregen voor gebruik in lineaire regressie met stochastische kleurovergang afkomst (SGD), een populaire algoritme voor de training een breed scala van machine learning-modellen. De [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) wordt gebruikt om de schaal van de functies die u kunt de afwijking van de eenheid. Functie schaalbaarheid, ook bekend als gegevens normaliseren, verzekert dat functies met breed betaald waarden zijn niet opgegeven overtollige wegen in de doelstelling functie. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 11.72 seconden


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Met het logistieke regressie-Model score en uitvoer naar blob opslaan

De code in deze sectie leert hoe u een logistieke regressie-Model dat is opgeslagen in Azure-blobopslag laden en deze gebruiken om te voorspellen al dan niet een tip op reis taxi is betaald, deze score met standaard classificatie aan de doelstellingen, sla en de resultaten met blob storage uitzetten. De scored resultaten worden opgeslagen in RDD objecten. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd uitvoering boven cel: 19.22 seconden


## <a name="score-a-linear-regression-model"></a>Een lineaire regressie-Model score

We [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) gebruikt om te trainen van een lineaire regressiemodel stochastische kleurovergang afkomst (SGD) voor optimalisatie gebruiken om te voorspellen van de hoeveelheid tip dat is betaald. 

De code in deze sectie ziet hoe u een lineaire regressie-Model van Azure-blobopslag laden en slaat u de resultaten terug naar de blob score scaled variabelen gebruiken.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER:**

Tijd uitvoering boven cel: 16.63 seconden


## <a name="score-classification-and-regression-random-forest-models"></a>Score classificatie en regressie willekeurig bos-modellen

De code in deze sectie wordt uitgelegd hoe de opgeslagen classificatie laden en regressie willekeurig bos modellen die zijn opgeslagen in Azure-blobopslag, score hun prestaties met standaard classificatie en regressie metingen en slaat u de resultaten terug naar blobopslag.

[Willekeurig forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) zijn ensembles beslissing bomen.  Ze combineren veel beslissingsbomen om te verkleinen van de kans op pesterijen te. Willekeurige forests kunnen afhandelen, bestaan functies, uitbreiden naar de instelling multiclass classificatie, is niet vereist schaalbaarheid van de functie en kunnen niet-mogelijkheid tot vastleggen en interacties aanbevelen. Willekeurige forests zijn een van de meest succesvolle machine learning-modellen voor de indeling en regressie.

[Spark.mllib](http://spark.apache.org/mllib/) ondersteunt willekeurig forests voor binaire en multiclass classificatie en voor regressie, met continue en bestaan-functies. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd uitvoering boven cel: 31.07 seconden


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Classificatie regressie kleurovergang verhogen structuur modellen en score

De code in deze sectie ziet hoe u classificatie regressie kleurovergang verhogen structuur modellen en laden van Azure-blobopslag, score hun prestaties met standaard classificatie en regressie metingen en slaat u de resultaten terug naar blobopslag. 

**Spark.mllib** ondersteunt GBTs voor binaire indeling en voor regressie, met continue en bestaan-functies. 

[Kleurovergang prestatieverbetering bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles beslissing bomen. GBTs trainen beslissingsbomen iteratief te minimaliseren ten een functie verlies. GBTs bestaan functies kan worden verwerkt, is niet vereist schaalbaarheid van de functie en kunnen niet-mogelijkheid tot vastleggen en interacties aanbevelen. Ze kunnen ook worden gebruikt in een multiclass-classificatie-instelling.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**UITVOER:**

Tijd uitvoering boven cel: 14.6 seconden


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Objecten uit het geheugen opschonen en afdrukken van een score voorzien bestandslocaties

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**UITVOER:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Elektrische modellen via een webservice-interface gebruiken

Elektrische biedt een om om in te dienen extern batchtaken of interactieve query's via een REST-interface met een onderdeel genaamd hier. Hier is op uw cluster HDInsight Spark standaard ingeschakeld. Zie voor meer informatie over hier: [elektrische indienen taken op afstand met hier](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

U kunt hier een taak die batch scores extern stuurt een bestand dat is opgeslagen in een Azure blob en schrijft vervolgens de resultaten naar een andere blob. Dit doet uploaden u het script Python uit  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) naar de label van het cluster elektrische. U kunt een hulpmiddel zoals **Microsoft Azure opslag Explorer** of **AzCopy** het script kopiëren naar de blob cluster. In ons geval geüpload we het script naar ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] De toegangstoetsen die u nodig hebt vindt u op de portal voor de opslag-account dat is gekoppeld aan het cluster elektrische. 


Zodra u hebt geüpload naar deze locatie, wordt dit script wordt uitgevoerd binnen het cluster een in een gedistribueerde context. Het model geladen en voorspellingen worden uitgevoerd op invoer bestanden op basis van het model.  

U kunt dit script extern aanroepen door een eenvoudige HTTPS/REST-aanvraag op hier.  Hier ziet u een curl-opdracht om de HTTP-aanvraag om aan te roepen het script Python extern te maken. CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAAM vervangen door de juiste waarden voor uw cluster elektrische.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

U kunt een taal op het externe systeem roepen elektrische tot en met hier doordat een eenvoudige HTTPS-gesprek met basisverificatie.   


>[AZURE.NOTE] Het is handig om te gebruiken van de bibliotheek Python aanvragen wanneer u deze HTTP-oproep plaatst, maar deze momenteel niet is geïnstalleerd al dan niet standaard in Azure-functies. Zodat u oudere HTTP-bibliotheken gebruikt.   


Hier ziet u de code Python voor de HTTP-oproep:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


U kunt ook deze Python-code toevoegen aan [Azure-functies](https://azure.microsoft.com/documentation/services/functions/) voor het starten van een elektrische taak indiening dat scores een blob op basis van verschillende gebeurtenissen zoals een timer, maken of bijwerken van een blob. 

Als u liever een code gratis Clientervaring, moet u de [Azure logica Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) gebruiken om aan te roepen de elektrische batch scoren door een HTTP-actie die aan de **Logica Apps Designer** en het instellen van de parameters. 

- Via Azure-portal, kunt u een nieuwe logica-App maken door te selecteren **+ Nieuw** -> **Web + Mobile** -> **Logica-App**. 
- Als u wilt de **Logica Apps Designer**weer, voer de naam van de logica-App en de App-Service plannen.
- Selecteer een actie HTTP en voer de parameters weergegeven in de volgende afbeelding:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Hoe nu verder? 

**Cross-validatie en hyperparameter verstrekkende**: Zie [Geavanceerde verkennen en modellering met een](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen ervaren met cross-validatie en hyper-parameter sweeping.
