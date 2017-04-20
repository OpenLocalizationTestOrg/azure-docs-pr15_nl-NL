<properties
    pageTitle="Gegevens verkennen en modellering met een geavanceerde | Microsoft Azure"
    description="Gebruik HDInsight Spark moet u doen verkennen en trainen van binaire indeling en regressie modellen cross-validatie en hyperparameter optimalisatie gebruiken."
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

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Geavanceerde gegevens verkennen en modellering met een 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In deze procedure wordt HDInsight Spark moet u doen verkennen en trainen binaire indeling en regressie-modellen met cross-validatie en hyperparameter optimalisatie van een steekproef van de tevens taxi reis en ritbedrag 2013 gegevensset. Deze begeleidt u bij de stappen van de [Gegevens wetenschap proces](http://aka.ms/datascienceprocess), end-to-end, met een HDInsight Spark cluster voor verwerking en Azure BLOB's om op te slaan en de modellen voor de gegevens. Het proces wordt verkend en gevisualiseerd gegevens uit een Azure opslag Blob toegevoegd en klikt u vervolgens bereidt de gegevens om blog modellen te maken. Python is gebruikt om de oplossing en weer te geven van de relevante waarnemingspunten. Deze modellen zijn opbouwen met de toolkit elektrische MLlib binaire indeling en regressie modellering taken uitvoeren. 

- De **binaire classificatie** -taak is om te voorspellen al dan niet een tip voor de reis is betaald. 
- De taak **regressie** is het bedrag van de tip op basis van andere functies tip voorspellen. 

De stappen modellering bevatten ook code die laat zien hoe u trainen, evalueren en opslaan van elk type model. Het onderwerp behandelt enkele van de dezelfde grond als [gegevens verkennen en modellering met een](machine-learning-data-science-spark-data-exploration-modeling.md) onderwerp. Maar deze is 'geavanceerdere"in dat wordt gebruikt cross-validatie in combinatie met hyperparameter verstrekkende om te trainen optimaal nauwkeurige classificatie en regressie-modellen. 

**Cross-validatie (AVK)** is een techniek die hoe u ook een model training op een bekende reeks gegevens bepaalt aan de functies van gegevenssets waarop deze is niet getraind voorspellen generaliseert verdwijnt. De algemene idee achter deze methode is dat een model is training op een gegevensset van bekende gegevens op en klikt u vervolgens de nauwkeurigheid van de voorspellingen een onafhankelijke dataset is getest. Er is een algemene implementatie hier gebruikt een gegevensset verdeeld over K vouwen en klik vervolgens trainen van het model op een wijze round robin op alle maar een van de vouwen. 

**Hyperparameter optimalisatie** is het probleem van het kiezen van een reeks hyperparameters voor een algoritme learning meestal met het doel van het optimaliseren van een maateenheid voor de algoritme van de prestaties van een onafhankelijke gegevensverzameling. **Hyperparameters** zijn waarden die buiten de model training procedure moeten worden opgegeven. Hypothesen over deze waarden kunnen van invloed zijn op de flexibiliteit en de nauwkeurigheid van de modellen. Besluit bomen hebt hyperparameters, bijvoorbeeld, zoals de gewenste diepteas en het aantal bladen in de structuur. Ondersteuning voor Vector Machines (SVMs) is vereist voor het instellen van een misclassification boete term. 

Een gebruikelijke manier om uit te voeren hyperparameter optimalisatie hier gebruikt, is een zoekopdracht raster of een **parameter opschonen**. Dit is een uitgebreide zoeken via de waarden uitvoering van een opgegeven subset van de ruimte hyperparameter voor een algoritme onderwijs. Cross validatie kunt een meting prestaties sorteren van de beste resultaten geproduceerd door de algoritme van de zoekopdracht raster opgeven. AVK gebruikt met hyperparameter vérstrekkende helpt limiet problemen, zoals een model naar trainingsgegevens overfitting zodat het model behoudt de capaciteit toe te passen op de algemene reeks gegevens waaruit de trainingsgegevens die is gehaald.

De modellen die we gebruiken zijn logistieke en lineaire regressie willekeurig forests en kleurovergang gestimuleerd bomen:

- [Lineaire regressie met SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is een lineaire regressie-model dat een stochastische kleurovergang afkomst (SGD)-methode gebruikt en voor optimalisatie en de functie voorspellen van de bedragen tip aanpassen dat is betaald. 
- [Logistische regressie met LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) of "logit" regressie, is een regressiemodel dat kan worden gebruikt als de afhankelijke variabele bestaan moet classificatie van gegevens is. LBFGS is een quasi-Newton optimalisatie algoritme die de algoritme van de Broyden – Fletcher – Goldfarb – Shanno (BFGS) met een beperkte tijdsduur van de computergeheugen benadert en die veel wordt gebruikt voor machine learning.
- [Willekeurig forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) zijn ensembles beslissing bomen.  Ze combineren veel beslissingsbomen om te verkleinen van de kans op pesterijen te. Willekeurige forests worden gebruikt voor regressie en classificatie en bestaan functies kunnen worden verwerkt en kunnen worden verlengd naar de multiclass classificatie-instelling. Deze is niet vereist schaalbaarheid van de functie en kunnen niet-mogelijkheid tot vastleggen en interacties aanbevelen. Willekeurige forests zijn een van de meest succesvolle machine learning-modellen voor de indeling en regressie.
- [Kleurovergang verhoogd bomen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) zijn ensembles beslissing bomen. GBTs trainen beslissingsbomen iteratief te minimaliseren ten een functie verlies. GBTs worden gebruikt voor regressie en classificatie bestaan functies kunnen worden verwerkt, is niet vereist schaalbaarheid van de functie en kunnen niet-mogelijkheid tot vastleggen en interacties aanbevelen. Ze kunnen ook worden gebruikt in een multiclass-classificatie-instelling.

Voorbeelden met AVK en Hyperparameter Modeling opschonen worden weergegeven voor het probleem binaire indeling. Eenvoudiger voorbeelden worden (zonder de parameter duplicaten) weergegeven in het onderwerp voor regressie taken. Maar in de bijlage voor gegevensvalidatie elastische net gebruiken voor lineaire regressie en CV met de parameter opschonen met voor willekeurig bos regressie ook worden weergegeven. Het **elastische net** is een geregulariseerd regressiemethode voor passend maken van lineaire regressie modellen waarin de doelstellingen van de L1 en L2 lineair zijn gecombineerd als sancties van de [functie lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) en [gleuf](https://en.wikipedia.org/wiki/Tikhonov_regularization) methoden.   



>[AZURE.NOTE] Hoewel de toolkit elektrische MLlib is ontworpen voor gebruik op grote gegevenssets, wordt een relatief kleine steekproef (~ 30 Mb met 170K rijen, ongeveer 0,1% van de oorspronkelijke tevens gegevensset) hier uitkomt gebruikt. De oefening hier opgegeven efficiënt (in ongeveer 10 minuten) op een HDInsight cluster met 2 werknemer knooppunten wordt uitgevoerd. Dezelfde code, met kleine wijzigingen kan worden gebruikt groter-gegevenssets, met de juiste wijzigingen voor caching van gegevens in het geheugen en het wijzigen van het formaat van een cluster verwerken.


## <a name="prerequisites"></a>Vereisten voor

Moet u een Azure-account en een HDInsight elektrische u moet een HDInsight 3.4 elektrische 1,6 cluster om te voltooien van deze procedure. Bekijk het [Overzicht van gegevens wetenschappelijk met een op Azure HDInsight](machine-learning-data-science-spark-overview.md) voor instructies over het voldoen aan deze vereisten is voldaan. Dit onderwerp bevat ook een beschrijving van de tevens 2013 Taxi gegevens hier gebruikt en instructies voor hoe u de code uit een notitieblok Jupyter op het cluster elektrische uitvoeren. Het **pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** notitieblok met de voorbeelden van de code in dit onderwerp is beschikbaar in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Installatie: opslaglocaties, bibliotheken en de context van de vooraf ingestelde elektrische

Elektrische kan lezen en schrijven naar Azure opslag Blob (ook wel bekend als WASB). Zodat uw bestaande gegevens opgeslagen kunnen er worden verwerkt met een en de resultaten weer in WASB worden opgeslagen.

Als u wilt opslaan modellen of bestanden in WASB, moet het pad correct worden opgegeven. De standaard-container die zijn bijgevoegd bij het cluster elektrische kan worden verwezen met een pad die begint met: "wasb: / / / '. Andere locaties wordt verwezen door ' wasb: / / '.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory paden voor opslaglocaties instellen in WASB

Het volgende voorbeeld wordt de locatie van de gegevens die moeten worden gelezen en het pad voor het model opslag-map waartoe de uitvoer model wordt opgeslagen:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**UITVOER**

DateTime.DateTime (2016, 4, 18, 17, 36, 27 en 832799)


### <a name="import-libraries"></a>Bibliotheken importeren

Importeer nodig bibliotheken met de volgende code:

    # LOAD PYSPARK LIBRARIES
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

De PySpark kernels die worden geleverd met Jupyter notitieblokken hebben een vooraf ingestelde context. Dus u hoeft niet voor het instellen van de elektrische of component contexten expliciet voordat u begint met het werken met de toepassing u ontwikkelt. Volgende contexten zijn beschikbaar voor u al dan niet standaard. Volgende contexten zijn:

- SC - voor een 
- sqlContext - voor component

De kernel PySpark vindt u enkele vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met bellen kunt %%. Er zijn twee die opdrachten die in deze codevoorbeelden worden gebruikt.

- **%% lokale** Geeft aan dat de code in de volgende regels lokaal wordt uitgevoerd. Code moet geldige Python code.
- **%%sql -o <variable name>** Hiermee voert u een query component ten opzichte van de sqlContext. Als de parameter -o wordt doorgegeven, het resultaat van de query is doorgevoerd in de %% lokale Python context als een DataFrame Pandas.
 

Voor meer informatie over de kernels voor Jupyter notitieblokken en de vooraf gedefinieerde "magics" die ze bieden, raadpleegt u [Kernels beschikbaar voor Jupyter notitieblokken met HDInsight elektrische Linux clusters op HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="data-ingestion-from-public-blob"></a>De opname van de gegevens uit openbare blob: 

De eerste stap in het proces van de wetenschappelijke gegevens is de gegevens die moeten worden geanalyseerd uit bronnen waar deze zich in uw gegevens verkennen en modelleringsomgeving bevindt nemen. Deze omgeving is een in dit scenario. In deze sectie bevat de code als u wilt een reeks taken te voltooien:

- de steekproef gegevens kunnen worden vormgegeven nemen
- lezen in de invoer gegevensset (opgeslagen als een bestand .tsv)
- Maak en de gegevens wissen
- maken en objecten (RDDs of gegevens-frames) in het geheugen in cache
- registreren als een temp-tabel in de SQL-context.

Hier ziet u de code voor de opname van de gegevens.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
        
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 276.62 seconden


## <a name="data-exploration--visualization"></a>Gegevens verkennen en visualisatie 

Nadat de gegevens een binnenkomen heeft, is de volgende stap in het proces van de wetenschappelijke gegevens beter begrip van de gegevens door te verkennen en visualisatie krijgen. In dit gedeelte we de taxi gegevens met SQL-query's controleren en uitzetten van de doelsite variabelen en potentiële functies voor visueel onderzoek. Name uitzetten we de frequentie van personen telt in taxi reizen, de frequentie van tip bedragen en hoe tips is afhankelijk van het betalingsbedrag en type.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Een histogram van personen tellen frequentie in het voorbeeld van taxi reizen uitzetten

Deze code en latere fragmenten gebruiken SQL bijzonder om query's in de steekproef en lokale bijzonder wilt uitzetten van de gegevens.

- **SQL bijzonder (`%%sql`)** De kernel HDInsight PySpark ondersteunt eenvoudig inline HiveQL query's op de sqlContext. De (-o naam_variabele) van de argumenten zich blijft voordoen de uitvoer van de SQL-query als een DataFrame Pandas op de server Jupyter. Dit betekent dat deze is beschikbaar in de lokale modus.
- De ** `%%local` bijzonder** code lokaal uitvoeren op de server Jupyter, dat wil de headnode van het cluster HDInsight zeggen wordt gebruikt. Meestal gebruikt u `%%local` magische in combinatie met de `%%sql` magische met -o-parameter. De parameter -o zou de uitvoer van de SQL-query lokaal opgeslagen en kiest u vervolgens %% lokale bijzonder zou de volgende set codefragment lokaal uitvoeren ten opzichte van de uitvoer van de SQL-query's die lokaal behouden is gebleven activeren

De uitvoer wordt automatisch weergegeven nadat u de code hebt uitgevoerd.

Deze query haalt de reizen door personen tellen. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Deze code maakt u een lokale gegevens-kader van de queryuitvoer en de gegevenspunten. De `%%local` bijzonder Hiermee maakt u een lokale gegevens-frame, `sqlResults`, die kan worden gebruikt met matplotlib getekend. 

>[AZURE.NOTE] In dit bijzonder PySpark wordt meerdere keren in dit scenario gebruikt. Als de hoeveelheid gegevens groot is, moet u voorbeeld om te maken van een gegevens-frame dat past in het lokale geheugen.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Hier ziet u de code wilt uitzetten het reizen door personen telt

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**UITVOER**

![Frequentie van reizen door personen tellen](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

U kunt selecteren tussen verschillende typen visualisaties (tabel, cirkel-, lijn, gebied of balk) met behulp van het **Type** menuknoppen in het notitieblok. Hier ziet u de balk tekenen.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Een histogram van tip bedragen en hoe tip bedrag is afhankelijk van de bedragen voor het aantal en tarief van personen uitzetten.

Een SQL-query naar voorbeeldgegevens gebruiken...
    
    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25
    

Deze cel code wordt de SQL-query gebruikt de gegevens van drie waarnemingspunten maken.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**UITVOER:** 

![Tip bedrag verdeling](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tip bedrag door personen tellen](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip bedrag door tarief bedrag](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Technische functie, transformatie en gegevens voorbereiden voor modellering aanbevelen

In deze sectie worden beschreven en vindt u de code voor procedures voor het voorbereiden van gegevens voor gebruik in ML modellering. Deze ziet u hoe u de volgende taken uitvoeren:

- Een nieuwe functie maken door binning uur tot verkeer tijd gerangschikte verzamelingen
- Index en klik op direct coderen bestaan functies
- Gelabelde point-objecten om invoer in ML functies maken
- Een onderliggend steekproeven van de gegevens maken en splits deze in de training en testen van sets
- Schaalbaarheid van de functie
- Cacheobjecten in het geheugen


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Een nieuwe functie maken door binning uur tot verkeer tijd gerangschikte verzamelingen

Deze code ziet u hoe u een nieuwe functie maakt met binning uur in verkeer tijd gerangschikte verzamelingen en klikt u vervolgens hoe u de resulterende gegevensframe in het geheugen in cache. Indien robuuste Distributed gegevenssets (RDDs) en gegevens-frames net zo vaak gebruikt worden, leidt caching tot verbeterde execution tijden. Dienovereenkomstig gewijzigd, wordt in de cache RDDs en gegevens-frames in verschillende stadia in de stapsgewijze instructies.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**UITVOER**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Index en één direct coderen bestaan functies

In dit gedeelte ziet hoe u indexeren of bestaan functies om invoer in de functies modellering coderen. De modellering en voorspellen functies van MLlib vereisen functies met bestaan invoergegevens moeten worden geïndexeerd of codering vóór gebruiken. 

Afhankelijk van het model, die u wilt indexeren of ze op verschillende manieren coderen. Bijvoorbeeld modellen Logistic en lineaire regressie vereisen één direct codering, waar, bijvoorbeeld een functie met drie categorieën kan worden uitgevouwen in drie kolommen van de functie, met elke met 0 of 1 afhankelijk van de categorie van een opmerking. MLlib biedt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -functie voor één direct codering. Een kolom met het label indexen worden koppelt deze encoder aan een kolom met binaire vectoren, met maximaal één één-waarde. Deze codering kunt algoritmen die in numerieke waarden en dit type functies, zoals logistische regressie, moeten worden toegepast om bestaan functies verwachten.

Hier ziet u de code wilt indexeren en coderen bestaan functies:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 3.14 seconden


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Gelabelde point-objecten om invoer in ML functies maken

In deze sectie bevat code waarin wordt uitgelegd hoe bestaan tekstgegevens als een gegevenstype gelabelde punt indexeren en deze coderen zodat deze kan worden gebruikt om te trainen en MLlib logistische regressie en andere classificatie-modellen testen. Gelabelde point-objecten zijn robuuste Distributed gegevenssets (RDD) opgemaakt in een manier die nodig is als invoergegevens door de meeste ML algoritmen in MLlib. Een [punt het label](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, op compacte of verspreid die is gekoppeld aan een label/reactie.

Hier ziet u de code wilt indexeren en coderen tekstfuncties voor binaire indeling.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Hier ziet u de code voor het coderen en indexeren bestaan tekstfuncties voor een lineaire regressie-analyse.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Een onderliggend steekproeven van de gegevens maken en splits deze in de training en testen van sets

Deze code maakt een steekproeven van de gegevens (25% is hier gebruikt). Hoewel het is niet vereist voor dit voorbeeld vanwege de grootte van de gegevensset, wordt gedemonstreerd hoe u kunt voorbeeld hier zodat u hoe weet u deze gebruiken voor uw eigen probleem als dat nodig is. Wanneer voorbeelden groot zijn, kunt dit sneller aanzienlijk training-modellen. Volgende splitsen we de steekproef in een training delen (hier 75%) en een testen (hier 25%) in de indeling en regressie modellering gebruiken.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd uitvoering boven cel: 0.31 seconden


### <a name="feature-scaling"></a>Schaalbaarheid van de functie

Functie schaalbaarheid, ook bekend als gegevens normaliseren, verzekert dat functies met sterk uiteen betaald waarden zijn niet opgegeven overtollige wegen in de doelstelling functie. De code voor schaalbaarheid van de functie gebruikt de [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) naar de schaal van de functies die u kunt de afwijking van de eenheid. Dit is opgegeven door MLlib voor gebruik in lineaire regressie met stochastische kleurovergang afkomst (SGD), een populaire algoritme voor een groot aantal andere machine learning-modellen zoals geregulariseerd behalen of ondersteuning vector machines (SVM) training.   

>[AZURE.TIP] We hebben de algoritme van de LinearRegressionWithSGD gevoelige naar aanbevelen schaalbaarheid worden gevonden.   

Hier ziet u de code wilt schaal variabelen voor gebruik met de regularized algoritme van de lineaire SGD.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd uitvoering boven cel: 11.67 seconden


### <a name="cache-objects-in-memory"></a>Cacheobjecten in het geheugen

De duur van training en testen van ML algoritmen kan worden verminderd door caching van het frame invoergegevens objecten die wordt gebruikt voor classificatie, regressie en schaal van functies.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER** 

Tijd uitvoering boven cel: 0.13 seconden


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Voorspellen al dan niet een tip is betaald met binaire classificatie-modellen

In dit gedeelte ziet u hoe gebruiken drie modellen voor de taak binaire classificatie van voorspellingen te doen al dan niet een tip voor een reis taxi is betaald. De modellen voor gepresenteerd zijn:

- Logistische regressie 
- Willekeurige bos
- Kleurovergang prestatieverbetering bomen

Elk model codesectie samenstellen opgedeeld in stappen: 

1. **Model training** gegevens met een parameter instellen
2. **Model evaluatie** van een gegevensverzameling test met aan de doelstellingen
3. **Model op te slaan** in blob voor toekomstige verbruik

U leert hoe cross-validatie (AVK) doen met parameter verstrekkende op twee manieren:

1. Met **algemene** aangepaste code die kan worden toegepast op elke algoritme in MLlib en aan eventuele parameter Hiermee stelt u in een algoritme. 
1. Gebruik de **pySpark CrossValidator verkooppijplijn functie**. Houd er rekening mee dat handige, op basis van onze ervaring, CrossValidator enkele beperkingen voor een 1.5.0 heeft: 

    - Verkooppijplijn modellen mogen niet opgeslagen/behouden gebleven voor toekomstige verbruik.
    - Kan niet worden gebruikt voor elke parameter in een model.
    - Kan niet worden gebruikt voor elke algoritme MLlib.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Algemeen cross gegevensvalidatie en hyperparameter sweeping gebruikt met de algoritme van de logistische regressie voor binaire indeling

De code in deze sectie ziet hoe u trainen, evalueren en een model logistische regressie opslaan met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die worden voorspeld al dan niet een tip voor een reis in de gegevensset tevens taxi reis en tarief dat is betaald. Het model is ervaren met kruisverwijzingen gegevensvalidatie (AVK) en hyperparameter sweeping geïmplementeerd met aangepaste code die kan worden toegepast op een van de algoritmen leren in MLlib.   

>[AZURE.NOTE] De uitvoering van deze aangepaste AVK-code kan enkele minuten duren.

**Het logistische regressie-model met AVK en hyperparameter sweeping trainen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Coëfficiënten: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Snijpunt:-0.0111216486893

Tijd uitvoering boven cel: 14.43 seconden


**De binaire indeling model met standaard aan de doelstellingen evalueren**

De code in deze sectie ziet hoe u een model logistische regressie ten opzichte van de test-gegevensverzameling, met inbegrip van de tekening van de kromme ROC evalueren.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Gebied onder prijs = 0.985336538462

Gebied onder ROC = 0.983383274312

Samenvatting stat

Precisie = 0.984174341679

Intrekken = 0.984174341679

F1 Score = 0.984174341679

Tijd uitvoering boven cel: 2.67 seconden


**De ROC curve tekenen.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, *tmp_results*, in de vorige cel. *tmp_results* kan worden gebruikt voor doe query's en uitvoer resultaten in de sqlResults gegevens-frame getekend. Hier ziet u de code.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier ziet u de code wilt voorspellingen en de ROC-curve tekenen.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**UITVOER**

![Logistische regressie ROC curve voor algemene methode](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Persistent model in een blob voor toekomstige verbruik**

De code in deze sectie ziet u hoe u het model logistische regressie voor verbruik opslaat.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**UITVOER**

Tijd uitvoering boven cel: 34.57 seconden


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Gebruik van MLlib CrossValidator verkooppijplijn functie met logistische regressie (elastische regressie)-model

De code in deze sectie ziet hoe u trainen, evalueren en een model logistische regressie opslaan met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die worden voorspeld al dan niet een tip voor een reis in de gegevensset tevens taxi reis en tarief dat is betaald. Het model is ervaren met kruisverwijzingen validatie (AVK) en hyperparameter sweeping geïmplementeerd met de functie van de verkooppijplijn MLlib CrossValidator voor AVK met parameter opschonen.   


>[AZURE.NOTE] De uitvoering van deze MLlib AVK-code kan enkele minuten duren.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER**

Tijd uitvoering boven cel: 107.98 seconden


**De ROC curve tekenen.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, *tmp_results*, in de vorige cel. *tmp_results* kan worden gebruikt voor doe query's en uitvoer resultaten in de sqlResults gegevens-frame getekend. Hier ziet u de code.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Hier ziet u de code wilt uitzetten van de kromme ROC.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**UITVOER**

![Logistische regressie ROC curve van MLlib CrossValidator gebruiken](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Willekeurige bos classificatie

De code in deze sectie ziet hoe u trainen, evalueren en opslaan van een willekeurig bos regressie die voorspellen = al dan niet een tip voor een reis in de gegevensset tevens taxi reis en tarief dat is betaald.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Gebied onder ROC = 0.985336538462

Tijd uitvoering boven cel: 26.72 seconden


### <a name="gradient-boosting-trees-classification"></a>Kleurovergang prestatieverbetering bomen classificatie

De code in deze sectie ziet u hoe u trainen, evalueren, en sla een overgangskleur prestatieverbetering bomen model dat voorspellen = al dan niet een tip voor een reis in de tevens taxi reis is betaald en ritbedrag gegevensset.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Gebied onder ROC = 0.985336538462

Tijd uitvoering boven cel: 28.13 seconden


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tip bedrag voorspellen met regressie-modellen (niet via AVK)

In dit gedeelte ziet u hoe drie modellen gebruiken voor de taak regressie van het bedrag van de tip voorspellen betaald voor een taxi reis op basis van andere functies tip. De modellen voor gepresenteerd zijn:

- Lineaire regressie geregulariseerd
- Willekeurige bos
- Kleurovergang prestatieverbetering bomen

Deze modellen zijn beschreven in de inleiding. Elk model codesectie samenstellen opgedeeld in stappen: 

1. **Model training** gegevens met een parameter instellen
2. **Model evaluatie** van een gegevensverzameling test met aan de doelstellingen
3. **Model op te slaan** in blob voor toekomstige verbruik   


>AZURE Opmerking: Cross-validatie wordt niet gebruikt met de drie regressie-modellen in deze sectie, aangezien dit is weergegeven in detail voor de logistische regressie-modellen. Voorbeeld van het gebruik van AVK met elastische Net voor lineaire regressie is opgegeven in de bijlage van dit onderwerp.


>AZURE Opmerking: In onze ervaring, kunnen er problemen met de onderlinge integratie van LinearRegressionWithSGD-modellen en parameters moeten worden gewijzigd/geoptimaliseerd zorgvuldig voor het verkrijgen van een geldige model. Schaalbaarheid van variabelen aanzienlijk helpt bij het convergentie. Elastische netto regressie, weergegeven in de bijlage bij dit onderwerp, kan ook worden gebruikt in plaats van LinearRegressionWithSGD.


### <a name="linear-regression-with-sgd"></a>Lineaire regressie met SGD

De code in deze sectie wordt beschreven hoe scaled functies gebruiken om te trainen van een lineaire regressie die stochastische kleurovergang afkomst (SGD) voor optimalisatie wordt gebruikt, en hoe u score, geëvalueerd en sla het model in Azure Blob Storage (WASB).

>[AZURE.TIP] In onze ervaring, kunnen er problemen met de onderlinge integratie van LinearRegressionWithSGD-modellen en parameters moeten worden gewijzigd/geoptimaliseerd zorgvuldig voor het verkrijgen van een geldige model. Schaalbaarheid van variabelen aanzienlijk helpt bij het convergentie.


    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Coëfficiënten: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Snijpunt: 0.854507624459

RMSE = 1.23485131376

R-sqr = 0.597963951127

Tijd uitvoering boven cel: 38.62 seconden


### <a name="random-forest-regression"></a>Willekeurig bos regressie

De code in deze sectie ziet hoe u trainen, evalueren en een willekeurig bos-model dat voorspellen = tip bedrag voor de tevens taxi reis-gegevens opslaan.   

>[AZURE.NOTE] Cross-validatie met parameter verstrekkende met aangepaste code wordt weergegeven in de bijlage.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

RMSE = 0.931981967875

R-sqr = 0.733445485802

Tijd uitvoering boven cel: 25.98 seconden


### <a name="gradient-boosting-trees-regression"></a>Kleurovergang prestatieverbetering bomen regressie

De code in deze sectie ziet hoe u trainen, evalueren en een overgangskleur model voor het maken van prestatieverbetering bomen dat voorspellen = tip bedrag voor de tevens taxi reis-gegevens opslaan.

**Trainen en evalueren**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE = 0.928172197114

R-sqr = 0.732680354389

Tijd uitvoering boven cel: 20.9 seconden


**Uitzetten**
    
*tmp_results* is geregistreerd als een tabel component in de vorige cel. Resultaten van de tabel worden uitgevoerd in de *sqlResults* -gegevens-frame getekend. Hier ziet u de code

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier ziet u de code wilt uitzetten van de gegevens met de server Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Werkelijke-vs-voorspeld-tip-bedragen](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Bijlage: Aanvullende regressiegrootheden taken cross gegevensvalidatie gebruiken met parameter krijgen

Deze bijlage bevat code met hoe u met behulp van elastische net voor lineaire regressie AVK en hoe u AVK met parameter opschonen met aangepaste code voor willekeurig bos regressie.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Cross validatie met elastische netto voor lineaire regressie

De code in deze sectie ziet u hoe u met behulp van elastische net voor lineaire regressie validatie overschrijden en hoe het model met testgegevens wordt berekend.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 161.21 seconden

**Met R-SQR metrisch beoordelen**

*tmp_results* is geregistreerd als een tabel component in de vorige cel. Resultaten van de tabel worden uitgevoerd in de *sqlResults* -gegevens-frame getekend. Hier ziet u de code

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Hier ziet u de code voor het berekenen van R-sqr.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**UITVOER**

R-sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Cross validatie met parameter opschonen met aangepaste code voor willekeurig bos regressie

De code in deze sectie ziet u hoe u overschrijden validatie met parameter opschonen met aangepaste code voor willekeurig bos regressie en hoe het model met testgegevens wordt berekend.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE = 0.906972198262

R-sqr = 0.740751197012

Tijd uitvoering boven cel: 69.17 seconden


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Objecten uit het geheugen en af te drukken model locaties opschonen

Gebruik `unpersist()` naar objecten die zijn opgeslagen in de cache in het geheugen verwijderen.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**UITVOER**

PythonRDD [122] bij RDD bij PythonRDD.scala: 43


**Afdruk pad naar modelbestanden moet worden gebruikt in het notitieblok verbruik.** Als u wilt gebruiken en een onafhankelijke gegevensverzameling score, die u wilt kopiëren en plakken van deze bestandsnamen in het notitieblok"verbruik".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**UITVOER**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Hoe nu verder?

Nu u modellen regressie en classificatie met de MlLib elektrische hebt gemaakt, bent u gereed om te leren score en deze modellen evalueren.

**Model verbruik:** Als u wilt weten hoe u score en evalueren van de indeling en regressie modellen die zijn gemaakt in dit onderwerp, raadpleegt u [Score evalueert in een ingebouwd machine learning-modellen](machine-learning-data-science-spark-model-consumption.md).
