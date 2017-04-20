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


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Machine Learning-toepassingen om uit te voeren op een van de Apache clusters op HDInsight Linux maken

Informatie over het maken van een machine learning-toepassing met behulp van een cluster Apache elektrische in HDInsight. In dit artikel leest hoe u het notitieblok Jupyter beschikbaar met het cluster bouwen en testen van de toepassing. De toepassing gebruikt HVAC.csv met de voorbeeldgegevens die beschikbaar zijn op alle clusters al dan niet standaard is.

**Vereisten:**

U hebt het volgende:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>De gegevens weergeven

Voordat we beginnen met het samenstellen van de toepassing, laat ons de structuur van de gegevens en het soort analyse die wij doen op de gegevens te begrijpen. 

In dit artikel gebruiken we het **HVAC.csv** gegevens voorbeeldbestand die beschikbaar is in de opslag van Azure-account dat u dat is gekoppeld aan het cluster HDInsight. Binnen de opslag-account is het bestand op **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Download en het CSV-bestand als u een overzicht van de gegevens wilt openen.  

![Momentopname van de gegevens Aircoschema] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Momentopname van de gegevens Aircoschema")

De gegevens ziet u de temperatuur doel en de werkelijke temperatuur bouwen met Aircoschema systemen is geïnstalleerd. Stel dat u de kolom **systeem** staat de systeem-ID en de kolom **SystemAge** voor het aantal jaren dat het Aircoschema-systeem bij het gebouw is.

We gebruiken deze gegevens om te voorspellen of een gebouw wordt hotter of colder gebaseerd op de doel-temperatuur, gegeven een systeem-ID en systeem leeftijd.

##<a name="app"></a>Een machine learning-toepassing met een MLlib schrijven

In deze toepassing gebruiken we een pijplijn elektrische ML om uit te voeren de classificatie van een document. In de pijplijn, we het splitsen van het document in woorden, de woorden converteren naar een vector numerieke functie en ten slotte maken een tekstvoorspelling model met de functie vectoren en labels. De volgende stappen als u wilt maken van de toepassing uitvoeren.

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Een nieuw Jupyter-notitieblok maken")

3. Een nieuw notitieblok is gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok aan het begin en voer een beschrijvende naam.

    ![Geef een naam voor het notitieblok] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Geef een naam voor het notitieblok")

3. Omdat u een notitieblok met de kernel PySpark hebt gemaakt, hoeft u niet alle contexten expliciet maken. Elektrische en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel van de code uitvoert. U kunt starten door te importeren van de typen die vereist voor dit scenario zijn. Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. U moet nu de gegevens (hvac.csv) te laden, parseren op en deze gebruiken om te trainen van het model. Hiervoor kunt u een functie die wordt gecontroleerd of de werkelijke temperatuur van het gebouw groter dan de doel-temperatuur is definiëren. Als de werkelijke temperatuur groter is, is het gebouw er populair, aangeduid door de waarde **1.0**. Als de werkelijke temperatuur lagere, is het gebouw koudwatersystemen, aangeduid met de waarde **0,0**. 

    Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. De elektrische machine learning pijplijn die uit drie fasen bestaat configureren: tokenizer, hashingTF en lr. Zie <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">een machine learning-pijplijn</a>voor meer informatie over wat een pijplijn is en hoe deze werkt.

    Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Aanpassen aan de pijplijn naar het trainingsdocument. Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Controleer of u het trainingsdocument controlepunt voortgang van uw Zorg dat de toepassing. Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.

        training.show()

    Dit geeft de uitvoer ongeveer als volgt uit:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Ga terug en controleer of de uitvoer ten opzichte van het onbewerkte CSV-bestand. De eerste rij van het CSV-bestand bevat bijvoorbeeld deze gegevens:

    ![Momentopname van de gegevens Aircoschema] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Momentopname van de gegevens Aircoschema")

    Zoals u ziet hoe de werkelijke temperatuur kleiner is dan het doel temperatuur suggesties voor dat het gebouw koudwatersystemen is. In de uitvoer training, dus is de waarde voor **label** in de eerste rij **0,0**, wat betekent dat het gebouw is niet er populair.

8.  Een gegevensgroep om uit te voeren het ervaren model tegen voorbereiden. Hiervoor zou geven we op een systeem-ID en systeem leeftijd (in de uitvoer training aangeduid als **SystemInfo** ) en het model zou voorspellen of het bouwen met die systeem-ID en systeem leeftijd zou hotter (aangeduid met 1.0) of koeler (aangeduid met 0,0).

    Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Tot slot voorspellingen op de testgegevens. Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Hier ziet u een uitvoer ongeveer als volgt uit:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    In de eerste rij in de voorspelling, kunt u zien dat voor een Aircoschema systeem met 20-ID en systeem leeftijd van 25 jaar, de building zal warm (**tekstvoorspelling = 1,0**). De eerste waarde voor DenseVector (0.49999) overeenkomt met de voorspelling 0,0 en de tweede waarde (0.5001) overeenkomt met de voorspelling 1.0. In de uitvoer, hoewel de tweede waarde alleen iets hoger is is, wordt het model aangegeven **tekstvoorspelling = 1,0**.

11. Nadat u klaar bent met het uitvoeren van de toepassing, moet u het notitieblok dat u wilt de bronnen vrijgeven afsluiten. Klik hiertoe vanuit het menu **bestand** op het notitieblok, klikt u op **sluiten en stoppen**. Deze uitgeschakeld en het notitieblok sluiten.
           

##<a name="anaconda"></a>Gebruik Anaconda scikit-bibliotheek meer voor Machine Learning

Apache elektrische clusters op HDInsight opnemen Anaconda bibliotheken. Hierbij zijn ook de **scikit-informatie over** bibliotheek voor machine learning. De bibliotheek bevat ook verschillende gegevenssets waarmee u kunt een voorbeeldtoepassingen rechtstreeks vanuit een Jupyter-notitieblok maken. Voor voorbeelden over het gebruik van de scikit-bibliotheek meer, raadpleegt u [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Zie ook

* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyses elektrische in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

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

* [Jupyter installeert op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
