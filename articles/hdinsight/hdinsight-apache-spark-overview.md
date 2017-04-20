<properties 
    pageTitle="Een overzicht van Apache elektrische in HDInsight | Microsoft Azure" 
    description="Een inleiding tot Apache elektrische in HDInsight en scenario's waarin een op HDInsight in uw toepassingen gebruiken." 
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
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Overzicht: Apache elektrische op HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache elektrische</a> is een open source parallel processing die ondersteuning biedt voor de verwerking in het geheugen om zo de prestaties van groot-gegevens analytische toepassingen. Elektrische verwerking-engine is geoptimaliseerd voor snelheid, gebruiksgemak, en geavanceerde analytische. Een van de berekening in het geheugen mogelijkheden kunnen u een goede keuze voor iteratieve algoritmen in machine learning en graph berekeningen. Elektrische is ook compatibel met Azure-blobopslag (WASB) zodat uw bestaande gegevens die zijn opgeslagen in Azure wordt aangegeven via een eenvoudig kan worden verwerkt.

Wanneer u een een-cluster in HDInsight maakt, kunt u Azure berekeningscluster resources maken met een geïnstalleerd en geconfigureerd. Alleen duurt ongeveer tien minuten voor het maken van een een-cluster in HDInsight. De gegevens die moeten worden verwerkt wordt opgeslagen in Azure-blobopslag. Zie [Gebruik Azure Blob Storage met HDInsight][hdinsight-storage].

![Apache elektrische op Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache elektrische op Azure HDInsight")


**Wilt u aan de slag met Apache elektrische op Azure HDInsight?** Zie [Snelstartgids: Maak een cluster elektrische op HDInsight Linux en uitvoeren van voorbeeldtoepassingen met Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Zie [bekende problemen van Apache elektrische in Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md)voor een lijst met bekende problemen en beperkingen met de huidige versie.


## <a name="why-use-spark-on-azure-hdinsight"></a>Waarom een op Azure HDInsight gebruiken? 

Azure HDInsight biedt een volledig beheerde elektrische-service. Voordelen van het gebruik van een op HDInsight zijn:

| Functie                             | Beschrijving       |
|-------------------------------------|-------------------|
| Gebruiksgemak clusters maken            | U kunt een nieuw elektrische cluster op HDInsight maken in de beheerportal Azure, Azure PowerShell of de HDInsight .NET SDK minuten. Zie [aan de slag met een cluster in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Gebruiksgemak                     | Elektrische in HDInsight clusters bevat Jupyter notitieblokken vooraf is geconfigureerd. U kunt deze voor interactieve gegevensverwerking en visualisatie. De URL's voor de https://CLUSTERNAME.azurehdinsight.net/jupyter is. __CLUSTERNAAM__ vervangen door de naam van uw cluster elektrische HDInsight.|
| REST API 's                       | Elektrische in HDInsight [hier](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)bevat, en een REST-API op basis van een Project server op afstand verzenden en controleren van actieve taken. |
| Ondersteuning voor Azure Lake gegevensopslag | Elektrische op HDInsight kan worden geconfigureerd voor gebruik van Azure Lake gegevensopslag als een extra opslagruimte. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Azure Lake gegevensopslag](../data-lake-store/data-lake-store-overview.md).
| Integratie met Azure services | Elektrische op HDInsight wordt geleverd met een verbindingslijn met Azure gebeurtenis Hubs. Klanten kunnen samenstellen streaming toepassingen met de gebeurtenis Hubs, naast [Kafka](http://kafka.apache.org/), die nog beschikbaar als onderdeel van een. |
| Ondersteuning voor R-Server  | U kunt een R-Server op HDInsight Spark cluster instellen verdeelde R berekeningen uitvoeren met de snelheden toegezegd met een cluster elektrische. Zie [aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)voor meer informatie.   |
| Integratie met IntelliJ IDEE  | U kunt de HDInsight-Plugin voor IntelliJ maken en toepassingen aan een cluster HDInsight Spark indienen. Zie [Gebruik HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ IDEE elektrische toepassingen voor HDInsight elektrische Linux cluster maken](hdinsight-apache-spark-intellij-tool-plugin.md)voor meer informatie. |
| Gelijktijdige query 's              | Elektrische in HDInsight ondersteunt gelijktijdige query's. Hiermee worden meerdere query's vanuit één gebruiker of meerdere query's vanuit verschillende gebruikers en toepassingen delen dezelfde cluster resources. |
| Caching op SSD                 | U kunt cachegegevens in het geheugen of in SSD die zijn bijgevoegd bij knooppunten. In cache opslaan in het geheugen beschikt u over de beste queryprestaties maar mogelijk dure; in cache opslaan in SSD biedt een handige optie voor het verbeteren van de prestaties van query's zonder dat u een cluster met een grootte die is vereist voor de hele gegevensset in het geheugen passen maken.|
| Integratie met hulpmiddelen voor BI       | Elektrische voor HDInsight biedt verbindingslijnen voor BI-functies zoals [Power BI](http://www.powerbi.com/) en [Tableau](http://www.tableau.com/products/desktop) van gegevens analysegegevens.|
| Vooraf geladen Anaconda-bibliotheken        | Elektrische clusters op HDInsight komen Anaconda bibliotheken vooraf geïnstalleerd. [Anaconda](http://docs.continuum.io/anaconda/) biedt dicht bij 200 bibliotheken voor machine learning, gegevensanalyse, visualisatie, enzovoort.|
| Schaalbaarheid                     | Maar u het aantal knooppunten in het cluster tijdens het maken opgeven kunt, kunt u groter of kleiner van het cluster zodat deze overeenkomen met werkbelasting. Alle HDInsight clusters kunnen u het aantal knooppunten in het cluster wijzigen. Elektrische clusters kunnen ook worden verwijderd zonder verlies van gegevens, omdat alle gegevens is opgeslagen in Azure-blobopslag. |
| 24 x 7-ondersteuning                    | Elektrische op HDInsight wordt geleverd met ondersteuning voor enterprise-level 24 x 7 en een SLA 99,9% omhoog-tijd.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Wat zijn de dozen gebruiken voor een op HDInsight?

Apache elektrische in HDInsight kunt de volgende belangrijke scenario's.

### <a name="interactive-data-analysis-and-bi"></a>Interactieve gegevensanalyse en BI

[Een zelfstudie bekijken](hdinsight-apache-spark-use-bi-tools.md)

Apache elektrische in HDInsight worden gegevens opgeslagen in Azure BLOB's. Zakelijke experts en belangrijke besluitvorming kunnen analyseren en -rapporten samenstelt via die gegevens en het gebruik van Microsoft Power BI voor interactieve rapporten van de geanalyseerde gegevens maken. Analisten kunnen starten vanuit ongestructureerde/semi gestructureerde gegevens in de opslagruimte van de Azure, definiëren van een schema voor de gegevens met notitieblokken en stelt u vervolgens samen met Microsoft Power BI-gegevensmodellen. Elektrische in HDInsight ondersteunt ook een aantal hulpprogramma's van derden BI zoals Tableau, Qlikview en SAP Lumira zodat u het ideale platform voor gegevens analisten, zakelijke experts en belangrijke besluitvorming.

### <a name="iterative-machine-learning"></a>Iteratieve Machine Learning

[Bekijk een zelfstudie: voorspellen temperaturen uisng Aircoschema gegevens samenstellen](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Bekijk een zelfstudie: eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache elektrische wordt geleverd met [MLlib](http://spark.apache.org/mllib/), een machine learning-bibliotheek elektrische gebouwd. Daarnaast bevat een op HDInsight ook Anaconda, een Python-verdeling met een verscheidenheid aan pakketten voor machine learning. Combineer dit met een ingebouwde ondersteuning voor Jupyter notitieblokken en er een boven-van-het-regel-omgeving voor het maken van machine learning-toepassingen.  

### <a name="streaming-and-real-time-data-analysis"></a>Streaming en realtime gegevensanalyse

[Een zelfstudie bekijken](hdinsight-apache-spark-eventhub-streaming.md)

Realtime gegevensanalyse wordt gebruikt voor scenario's die variëren van de tijd aan gegevens inzicht wordt afgetrokken van het verwerken van gegevens, zoals deze terechtkomt, tot het bouwen van een oplossing streaming waar. Elektrische in HDInsight biedt een uitgebreide ondersteuning voor het ontwikkelen van oplossingen voorstelt realtime analytics. Terwijl een al aan het nemen van gegevens uit veel bronnen zoals Kafka, Flume, Twitter, ZeroMQ of TCP sockets connectors bevat, komt elektrische in HDInsight eersteklas ondersteuning voor de gegevens van Azure gebeurtenis Hubs ingesting. Gebeurtenis Hubs zijn de meestgebruikte wachtrij plaatsen op Azure. Hebt een kant-van-het-box-ondersteuning voor gebeurtenis Hubs dus maken in HDInsight een ideaal platform voor het samenstellen van realtime analytics verkooppijplijn.

##<a name="next-steps"></a>Welke onderdelen zijn opgenomen als onderdeel van een cluster elektrische?

Elektrische in HDInsight bevat de volgende onderdelen die beschikbaar op de clusters al dan niet standaard zijn.

- [Een Core](https://spark.apache.org/docs/1.5.1/). Bevat een Core, een SQL, een streaming API's, GraphX en MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Hier](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter notitieblok](https://jupyter.org)

Elektrische in HDInsight biedt ook een [ODBC-stuurprogramma](http://go.microsoft.com/fwlink/?LinkId=616229) voor connectiviteit met een clusters in HDInsight van BI-functies zoals Microsoft Power BI en Tableau.

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

Begin met het maken van een cluster elektrische op HDInsight Linux. Zie [Snelstartgids: Maak een cluster elektrische op HDInsight Linux en uitvoeren van voorbeeldtoepassingen met Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Volgende stappen

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyses elektrische in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight building temperatuur met Aircoschema gegevens analyseren](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
