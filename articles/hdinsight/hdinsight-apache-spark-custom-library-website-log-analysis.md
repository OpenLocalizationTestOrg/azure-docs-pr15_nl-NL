<properties 
    pageTitle="Aangepaste bibliotheken met een cluster HDInsight Spark gebruiken om te analyseren logboeken aan de website | Microsoft Azure" 
    description="De aangepaste bibliotheken gebruiken met een cluster HDInsight Spark te analyseren logboeken aan de website" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Logboeken aan de website met een aangepaste bibliotheek met Apache elektrische cluster op HDInsight Linux analyseren

Dit notitieblok laat zien hoe logboekgegevens te analyseren met een aangepaste bibliotheek met een op HDInsight. De aangepaste bibliotheek die we gebruiken is een Python bibliotheek **iislogparser.py**genoemd.

> [AZURE.TIP] Deze zelfstudie is ook beschikbaar als een Jupyter notitieblok op een cluster elektrische (Linux) die u in HDInsight maakt. De ervaring notitieblok kunt u de fragmenten Python van het notitieblok zelf uitvoeren. Als u de zelfstudie uit in een notitieblok, een cluster elektrische maken, een notebook Jupyter starten (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voert u het notitieblok **analyseren logboeken met uitgerust met een aangepaste library.ipynb** onder de map **PySpark** .

**Vereisten:**

U hebt het volgende:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Onbewerkte gegevens opslaan als een RDD

In deze sectie gebruiken we het [Jupyter](https://jupyter.org) notitieblok dat is gekoppeld aan een cluster Apache elektrische in HDInsight taken die verwerken van uw onbewerkte voorbeeldgegevens en opslaan als een tabel component uitvoeren. De voorbeeldgegevens is een CSV-bestand (hvac.csv) beschikbaar op alle clusters al dan niet standaard.

Als uw gegevens worden opgeslagen als een tabel component, in het volgende gedeelte maakt we verbinding met de component-tabel met BI-functies zoals Power BI en Tableau.

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Een nieuw Jupyter-notitieblok maken")

3. Een nieuw notitieblok is gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok aan het begin en voer een beschrijvende naam.

    ![Geef een naam voor het notitieblok] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Geef een naam voor het notitieblok")

4. Omdat u een notitieblok met de kernel PySpark hebt gemaakt, hoeft u niet alle contexten expliciet maken. Elektrische en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel van de code uitvoert. U kunt starten door te importeren van de typen die vereist voor dit scenario zijn. Plak de volgende fragment in een lege cel en druk op **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Maak een RDD gebruik van de steekproef logboekgegevens al beschikbaar op het cluster. U kunt de gegevens in de standaard-opslag-account dat is gekoppeld aan het cluster bij **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**openen.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Ophalen van een steekproef-logboek instellen om te bevestigen dat de vorige stap is voltooid.

        logs.take(5)

    Hier ziet u een uitvoer ongeveer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Logboekgegevens te analyseren met een aangepaste Python-bibliotheek

7. De eerste paar regels Neem de kopgegevens in de bovenstaande uitvoer, en elke resterende regel overeenkomt met het schema wordt beschreven in deze kop. Parseren van dergelijke logboeken kan ingewikkeld zijn. Zo is, gebruiken we een aangepaste Python bibliotheek (**iislogparser.py**) dat parseren van dergelijke logboeken veel eenvoudiger maakt. Deze bibliotheek is opgenomen in uw cluster elektrische op HDInsight bij **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**standaard.

    Deze bibliotheek is echter niet in de `PYTHONPATH` zodat we deze niet gebruiken met behulp van een importinstructie zoals `import iislogparser`. Als u wilt deze bibliotheek gebruikt, moeten we deze naar alle werknemer knooppunten distribueren. Voer de volgende fragment.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`biedt een functie `parse_log_line` die retourneert `None` als een regel een veldnamenrij is en geeft als een exemplaar van resultaat de `LogLine` klasse als wordt een logboek lijn aangetroffen. Gebruik de `LogLine` class worden alleen de lijnen van het logboek geëxtraheerd uit de RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Ophalen enkele opgehaalde log regels om te bevestigen dat de stap is voltooid.

        logLines.take(2)

    De uitvoer moet er ongeveer als volgt te werk:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. De `LogLine` op zijn beurt heeft enkele nuttige methoden, zoals class, `is_error()`, die resulteert of de invoer in een logboek met een foutcode heeft. Gebruik deze opdracht te berekenen van het aantal fouten in de opgehaalde log lijnen en meld u vervolgens alle fouten naar een ander bestand.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Hier ziet u een uitvoer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. U kunt ook **Matplotlib** gebruiken om een visualisatie van de gegevens samen te stellen. Als u isoleren de oorzaak van aanvragen die lange tijd worden uitgevoerd wilt, wilt u mogelijk de bestanden die duren voordat de meest gemiddeld dienen hebt gevonden.
Het codefragment van de onderstaande haalt de bovenste 25 bronnen die u hebt gemaakt, de meeste tijd moet fungeren van een nieuw vergaderverzoek.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Hier ziet u een uitvoer als volgt uit:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. U kunt ook deze informatie in de vorm van tekeningen presenteren. Als eerste stap te maken van een tekening, laat ons eerst maakt u een tijdelijke tabel **AverageTime**. De logboeken aan de tabel gegroepeerd op tijd om te zien of er een pieken ongebruikelijke latentie beschikbaar op een willekeurig moment zijn.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Vervolgens kunt u de volgende SQL-query voor alle records in de tabel **AverageTime** uitvoeren.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    De `%%sql` bijzonder gevolgd door `-o averagetime` zorgt ervoor dat de uitvoer van de query lokaal op de server Jupyter (meestal de headnode van het cluster behouden blijft). De uitvoer blijft behouden als een dataframe [Pandas](http://pandas.pydata.org/) met de opgegeven naam **averagetime**.

    Hier ziet u een uitvoer als volgt uit:

    ![SQL-queryuitvoer] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "SQL-queryuitvoer")

    Voor meer informatie over de `%%sql` bijzonder, evenals andere magics beschikbaar voor communicatie met de kernel PySpark, raadpleegt u [Kernels beschikbaar op Jupyter notitieblokken met een HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. U kunt nu Matplotlib, een bibliotheek gebruikt om te maken van de weergave van gegevens, een tekeningen maken. Omdat het tekenen van de dataframe lokaal permanente **averagetime** moet worden, het codefragment moet beginnen met de `%%local` bijzonder. Dit zorgt ervoor dat de code lokaal wordt uitgevoerd op de server Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Hier ziet u een uitvoer als volgt uit:

    ![Matplotlib uitvoer] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib uitvoer")

16. Nadat u klaar bent met het uitvoeren van de toepassing, moet u het notitieblok dat u wilt de bronnen vrijgeven afsluiten. Klik hiertoe vanuit het menu **bestand** op het notitieblok, klikt u op **sluiten en stoppen**. Deze uitgeschakeld en het notitieblok sluiten.
    

## <a name="seealso"></a>Zie ook


* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyse met een in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight building temperatuur met Aircoschema gegevens analyseren](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Een Streaming: Gebruik een in HDInsight voor het samenstellen van realtime streaming-toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

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
