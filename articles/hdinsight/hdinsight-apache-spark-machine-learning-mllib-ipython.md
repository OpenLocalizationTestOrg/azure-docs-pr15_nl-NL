<properties 
    pageTitle="Apache elektrische gebruiken om te maken van machine learning-toepassingen op HDInsight | Microsoft Azure" 
    description="Stapsgewijze instructies voor het gebruik van notitieblokken met Apache elektrische machine learning-toepassingen maken" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Machine learning: Bekijk analyses van eten inspectie gegevens met behulp van MLlib met Apache elektrische cluster op HDInsight Linux

> [AZURE.TIP] Deze zelfstudie is ook beschikbaar als een Jupyter notitieblok op een cluster elektrische (Linux) die u in HDInsight maakt. De ervaring notitieblok kunt u de fragmenten Python van het notitieblok zelf uitvoeren. Als u de zelfstudie uit in een notitieblok, een cluster elektrische maken, een notebook Jupyter starten (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voert u het notitieblok **een Machine Learning - Bekijk analyses van eten inspectie gegevens MLLib.ipynb** onder de map **Python** .


Dit artikel wordt beschreven hoe u een eenvoudige blog analyse uitvoeren op een geopende gegevensset met **MLLib**, van een ingebouwde machine learning-bibliotheken. MLLib is een core elektrische-bibliotheek waarin een aantal hulpprogramma's die handig voor machine learning taken biedt zijn, zoals het hulpprogramma's die geschikt zijn voor:

* Classificatie

* Regressie

* Cluster

* Onderwerp modellering

* Enkelvoud waarde uitgevouwen (SVD) en principal onderdeel analyse (VO)

* Hypothese testen en steekproef statistieken te berekenen

In dit artikel bevat een eenvoudige manier *classificatie* tot en met logistische regressie.

## <a name="what-are-classification-and-logistic-regression"></a>Wat zijn classificatie en logistische regressie?

*Classificatie*, een veelvoorkomende machine learning-taak, is het proces voor het sorteren van invoergegevens in categorieën. Dit is de taak van een classificatie-algoritme om te bepalen hoe u kunt toewijzen 'etiketten"voor de invoer van gegevens die u opgeeft. Kan bijvoorbeeld dat van een machine learning-algoritme die worden geaccepteerd voorraad informatie als invoer en wordt de voorraad verdeeld in twee categorieën: bestanden waarvoor u moet verkoopt en bestanden waarvoor u moet bewaren.

Logistische regressie is het algoritme dat u voor classificatie gebruikt. Van een logistische regressie API is handig voor *binaire indeling*of classificatie van invoergegevens in een van twee groepen. Zie voor meer informatie over logistieke behalen, [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

In het overzicht levert het proces van het logistische regressie een *logistieke functie* die kan worden gebruikt om de kans dat een invoer vector in één groep of de andere behoort.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Wat we wilt uitvoeren in dit artikel?

U kunt een enkele blog analyses uitvoeren op eten inspectie gegevens (**Food_Inspections1.csv**) die via de [portal van stad van Chicago gegevens](https://data.cityofchicago.org/)hebt aangeschaft. Deze dataset bevat informatie over eten controles die zijn uitgevoerd in Arnhem, met inbegrip van informatie over elke eten inrichting die is gecontroleerd, de schending die zijn gevonden (indien aanwezig) en de resultaten van de controle. Het CSV-bestand is al beschikbaar in de opslagruimte-account dat is gekoppeld aan het cluster op **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

In de onderstaande stappen, kunt u een model om te zien wat duren gehaald of mislukken van een inspectie eten ontwikkelen. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Beginnen met het samenstellen van een machine learning-toepassing met behulp van een MLlib

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Een nieuw Jupyter-notitieblok maken")

3. Een nieuw notitieblok is gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok aan het begin en voer een beschrijvende naam.

    ![Geef een naam voor het notitieblok] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Geef een naam voor het notitieblok")

3. Omdat u een notitieblok met de kernel PySpark hebt gemaakt, hoeft u niet alle contexten expliciet maken. Elektrische en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel van de code uitvoert. U kunt beginnen met het samenstellen van uw machine learning-toepassing door deze te importeren van de typen die is vereist voor dit scenario. Als u wilt doen, plaats de cursor in de cel en druk op **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Een invoer dataframe maken

We de beschikking over `sqlContext` transformaties voor gestructureerde gegevens uitvoeren. De eerste taak is de voorbeeldgegevens ((**Food_Inspections1.csv**)) in een een SQL- *dataframe*laden. 

1. Omdat de onbewerkte gegevens zich in een CSV-indeling, moeten we de context elektrische gebruiken om op te halen van elke regel van het bestand in het geheugen als ongestructureerde tekst; Vervolgens kunt u de Python CSV-bibliotheek afzonderlijk parseren van elke regel. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. We nu hebben het CSV-bestand als een RDD. Laat ons één rij uit de RDD voor meer informatie over het schema van de gegevens ophalen.


        inspections.take(1)


    Hier ziet u een uitvoer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Ontstaat een idee van het schema van bestand; de bovenstaande uitvoer het bestand bevat de naam van elke instelling, het type inrichting, het adres, de gegevens van de controles en de locatie, onder andere. Laten we eens enkele kolommen die zijn handig voor onze blog analyse en de resultaten te groeperen als een dataframe, die we vervolgens gebruiken om te maken van een tijdelijke tabel selecteren


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Nu zijn er een *dataframe*, `df` waarop we onze analyse kunt uitvoeren. We hebben ook een tijdelijke tabel **CountResults**bellen. Belangrijke in de dataframe 4 kolommen hebt opgenomen: **id**, **naam**, **resultaten**en **schending**. 
    
    We gaan een kleine steekproef van de gegevens:

        df.show(5)

    Hier ziet u een uitvoer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Meer informatie over de gegevens

1. Laten we beginnen aan een idee van wat onze gegevensset bevat. Wat zijn bijvoorbeeld de verschillende waarden in de kolom **resultaten** ?


        df.select('results').distinct().show()

    
    Hier ziet u een uitvoer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Een snelle visualisatie kunt ons reden dan ook over de verdeling van deze resultaten. We al beschikt over de gegevens in een tijdelijke tabel **CountResults**. U kunt de volgende SQL-query uitvoeren op de tabel om een beter begrip van hoe de resultaten zijn verdeeld.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    De `%%sql` bijzonder gevolgd door `-o countResultsdf` zorgt ervoor dat de uitvoer van de query lokaal op de server Jupyter (meestal de headnode van het cluster behouden blijft). De uitvoer blijft behouden als een dataframe [Pandas](http://pandas.pydata.org/) met de opgegeven naam **countResultsdf**.
    
    Hier ziet u een uitvoer als volgt uit:
    
    ![SQL-queryuitvoer] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL-queryuitvoer")

    Voor meer informatie over de `%%sql` bijzonder, evenals andere magics beschikbaar voor communicatie met de kernel PySpark, raadpleegt u [Kernels beschikbaar op Jupyter notitieblokken met een HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. U kunt ook Matplotlib, een bibliotheek gebruikt om te maken van de weergave van gegevens, gebruiken om te maken van een tekening. Omdat het tekenen van de dataframe lokaal permanente **countResultsdf** moet worden, het codefragment moet beginnen met de `%%local` bijzonder. Dit zorgt ervoor dat de code lokaal wordt uitgevoerd op de server Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Hier ziet u een uitvoer als volgt uit:

    ![Resultaat uitvoer](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. U kunt zien dat er 5 afzonderlijke resultaten op te geven dat een inspectie kunt:
    
    * Bedrijven die niet vinden 
    * Mislukt
    * Doorgeven
    * PSS met voorwaarden voldoen, en
    * Afmelden bij bedrijven 

    Laat ons ontwikkel een model dat het resultaat van een inspectie eten kan raden de schending gegeven. Aangezien logistische regressie een indelingsmethode binaire is, is het verstandig onze om gegevens te groeperen in twee categorieën: **mislukt** en **doorgeven**. Een "doorgeven met voorwaarden" is nog een keer, zodat wanneer we het model training, zullen we de twee resultaten equivalente overwegen. Gegevens met de andere resultaten ("Bedrijven niet gevestigd", "afwezigheidsberichten bedrijven") zijn niet handig zodat deze wordt verwijderd uit de trainingsset van onze. Dit moet aangezien deze twee categorieën van een zeer klein percentage van de resultaten toch maken.

5. Laat ons verdergaan en converteer onze bestaande dataframe (`df`) in een nieuwe dataframe waar elke inspectie wordt weergegeven als een paar label-schending. In ons geval een label van `0.0` duidt op een fout, een label van `1.0` vertegenwoordigt een gunstige uitkomst en een label van `-1.0` sommige resultaten naast deze twee vertegenwoordigt. We wordt deze andere resultaten bij het berekenen van het gegevensframe met nieuwe uitfilteren.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Laten we ophalen met één rij de gelabelde gegevens om te zien hoe dit eruitziet.


        labeledData.take(1)


    Hier ziet u een uitvoer als volgt uit:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Een model logistische regressie maken op basis van de invoer dataframe

Onze definitief taak is het gelabelde om gegevens te converteren naar een bestandsindeling die kan worden geanalyseerd met logistische regressie. De invoer op de algoritme van een logistische regressie moet een reeks *label-functie vector paren*, waarbij de "functie vector" is een vector van getallen dat de invoer punt in een andere manier vertegenwoordigt. Zo is, moeten we een manier om te converteren van de kolom "schending" die gedeeltelijk gestructureerd en bevat een groot aantal opmerkingen in vrije tekst, naar een matrix van reële getallen die een computer eenvoudig kan begrijpen. 

Een standaard machine learning-methode voor het verwerken van natuurlijke taal is een "index" voor elk woord distinct toewijzen, en klikt u vervolgens een vector doorgeven aan de machine learning-algoritme zodat van elke index waarde de relatieve frequentie van dat woord in de tekenreeks bevat. 

MLLib biedt een eenvoudige manier om deze bewerking uitvoeren. Eerst we wordt 'basisvormen"elke tekenreeks schending voor de afzonderlijke woorden in elke tekenreeks en vervolgens gebruiken we een `HashingTF` en elke set van tokens omzetten in een functie vector die vervolgens kan worden doorgegeven aan de algoritme van de logistische regressie om een model te maken. We wordt al deze stappen uitvoeren in volgorde met een "pijplijn".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Het model op een afzonderlijke test gegevensset evalueren

We de beschikking over het model die we eerder hebt gemaakt om te *voorspellen* wat de resultaten van nieuwe controles zal zijn, op basis van de schending die werden waargenomen. We ervaren dit model op de gegevensset **Food_Inspections1.csv**. Laat ons gebruikt u een tweede gegevensset **Food_Inspections2.csv**, om te *evalueren* sterkte van dit model op nieuwe gegevens. Deze tweede gegevensset (**Food_Inspections2.csv**) moet al in de standaard-opslag container die is gekoppeld aan het cluster.

1. Het codefragment van de onderstaande Hiermee maakt u een nieuwe dataframe, **predictionsDf** die de voorspelling gegenereerd door het model bevat. Het codefragment maakt ook een tijdelijke tabel **die voorspellingen** op basis van de dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Hier ziet u een uitvoer als volgt uit:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Bekijk een van de voorspellingen. Het codefragment van deze uitvoeren:

        predictionsDf.take(1)

    Hier ziet u de voorspelling voor het eerste item in de gegevensset test.

3. De `model.transform()` methode worden de dezelfde transformatie toepassen op nieuwe gegevens met hetzelfde schema en een schatting van de manier waarop de gegevens classificeren aankomen. We kunt enkele eenvoudige statistieken om een idee van hoe nauwkeurig onze voorspellingen zijn doen:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    De uitvoer ziet er als volgt uit:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Logistische regressie gebruikt met een ontstaat een nauwkeurige model van de relatie tussen schending beschrijvingen in het Engels en een bepaald bedrijfsproces zou doorgeven of afbreken van een inspectie eten. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Een visuele weergave van de voorspelling maken

We kunt een definitief visualisatie om ons te helpen nu reden dan ook over de resultaten van deze test maken. 

1. Begin met het ophalen van de verschillende voorspellingen en de resultaten van de **voorspellingen** tijdelijke tabel eerder hebt gemaakt. De volgende query's scheidt u de uitvoer als *true_positive*, *false_positive* *true_negative*en *false_negative*. In de onderstaande query's we uitschakelen visualisatie met behulp van `-q` en de uitvoer ook opslaan (met behulp van `-o`) als dataframes die vervolgens kunnen worden gebruikt met de `%%local` bijzonder. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Gebruik ten slotte het volgende fragment te genereren van de tekening met **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Hier ziet u de volgende uitvoer.
    
    ![Voorspellen-uitvoer](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    In deze grafiek verwijst "" positief naar de inspectie mislukte eten, terwijl een negatieve resultaat naar een doorgegeven inspectie verwijst.

## <a name="shut-down-the-notebook"></a>Het notitieblok afsluiten

Nadat u klaar bent met het uitvoeren van de toepassing, moet u het notitieblok dat u wilt de bronnen vrijgeven afsluiten. Klik hiertoe vanuit het menu **bestand** op het notitieblok, klikt u op **sluiten en stoppen**. Deze uitgeschakeld en het notitieblok sluiten.


## <a name="seealso"></a>Zie ook


* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyses elektrische in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight building temperatuur met Aircoschema gegevens analyseren](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Een Streaming: Gebruik een in HDInsight voor het samenstellen van realtime streaming-toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met behulp van een in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Taken op afstand uitvoeren op een elektrische cluster met hier](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en uitbreidingen

* [HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ IDEE maken en indienen elektrische Scala toepassingen gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ verloop foutopsporing elektrische toepassingen op afstand uitvoeren](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruikt met een cluster elektrische op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels beschikbaar voor Jupyter notitieblok in een cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter installeren op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)
