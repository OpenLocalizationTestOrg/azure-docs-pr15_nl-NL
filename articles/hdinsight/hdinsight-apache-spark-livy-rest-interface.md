<properties
    pageTitle="Taken van een extern met hier | Microsoft Azure"
    description="Leer hoe u hier gebruiken met HDInsight clusters om in te dienen elektrische taken op afstand."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Taken van een extern met een cluster Apache elektrische op HDInsight Linux met hier indienen

Apache elektrische cluster op Azure HDInsight bevat hier, een REST interface voor het verzenden van taken op afstand naar een cluster elektrische. Zie [hier](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)voor gedetailleerde informatie.

Hier kunt u wilt uitvoeren interactieve elektrische houders of batchtaken kunnen worden uitgevoerd op een indienen. In dit artikel moment spreekt over het gebruik van hier om in te dienen batchtaken. De syntaxis van de onderstaande gebruikt krul REST gesprekken naar het eindpunt hier.

**Vereisten:**

U hebt het volgende:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Een batchtaak het cluster verzenden

Voordat u een batchtaak stuurt, kunt u het oppervlak van toepassing op de clusteropslag die is gekoppeld aan het cluster moet uploaden. U kunt [**AzCopy**](../storage/storage-use-azcopy.md), een opdrachtregelhulpprogramma kunt doen. Zijn er een groot aantal andere clients die u kunt gebruiken voor het uploaden van gegevens. U vindt meer informatie over deze bij het [uploaden van gegevens voor Hadoop-projecten in HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Voorbeelden**:

* Als het oppervlak-bestand zich op de clusteropslag (WASB bevindt)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Als u wilt de bestandsnaam oppervlak en de klassenaam doorgeven als onderdeel van een invoer-bestand (in dit voorbeeld input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Informatie over batches uitgevoerd op de cluster

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Voorbeelden**:

* Als u ophalen van alle batches uitgevoerd op de cluster wilt:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Als u wilt ophalen van een specifieke batch met een bepaald batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Een batchtaak verwijderen

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Voorbeeld**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Hier en hoge beschikbaarheid

Hier vindt u hoge beschikbaarheid voor een taken op de cluster. Hier volgen een paar voorbeelden van.

* Als de service hier uitvalt nadat u een taak op afstand voor een cluster elektrische hebt verzonden, blijft de taak op de achtergrond uitvoeren. Hier is een back-up, wordt de status van de taak en rapporten dat alleen terug hersteld.

* Jupyter notitieblokken voor HDInsight zijn mogelijk gemaakt door hier in de backend. Als een taak een wordt uitgevoerd door een notitieblok en de hier-service opnieuw wordt gestart, blijft het notitieblok dat de code cellen worden uitgevoerd. 

## <a name="show-me-an-example"></a>Een voorbeeld weergeven

In dit gedeelte kijken we naar voorbeelden over het gebruik van hier een aanvraag elektrische ingediend, de voortgang van de toepassing en verwijder vervolgens de taak. De toepassing die we in dit voorbeeld gebruiken is het een ontwikkeld in het artikel [maken een zelfstandige Scala-toepassing en om uit te voeren op een van de HDInsight cluster](hdinsight-apache-spark-create-standalone-application.md). De onderstaande stappen wordt ervan uitgegaan dat volgt het volgende:

* U hebt al hebt gekopieerd over het oppervlak toepassing in het opslag-account dat is gekoppeld aan het cluster.
* U hebt geïnstalleerd op de computer waarop u deze stappen probeert krul.

De volgende stappen uitvoeren.

1. Laat ons eerst verifiëren dat hier wordt uitgevoerd op het cluster. We kunt dit doen door een lijst van de uitvoering van batches. Als dit de eerste keer dat u een taak met hier uitvoert, moet dit nul retourneren.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Moet krijgt u een uitvoer ongeveer als volgt uit:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Zoals u ziet hoe de laatste regel van de uitvoer **Totaal: 0**, die duidt op geen actieve batches staat.

2. Laat ons nu een batchtaak stuurt. Het codefragment van de onderstaande maakt gebruik van een invoer-bestand (input.txt) om de naam van het oppervlak te geven en de klassenaam als parameters. Dit is de aanbevolen werkwijze als u deze stappen vanuit een Windows-computer uitvoert.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    De parameters in het bestand **input.txt** zijn als volgt gedefinieerd:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Hier ziet u een uitvoer ongeveer als volgt uit:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Zoals u ziet hoe de laatste regel van de uitvoer zegt **staat: beginnend**. Ook staat, **id: 0**. Dit is de batch-ID.

3. U kunt nu ophalen de de status van deze specifieke batch met de batch-ID.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Hier ziet u een uitvoer ongeveer als volgt uit:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Nu ziet u de uitvoer **staat: success**, die duidt op dat de taak is voltooid.

4. Als u wilt, kunt u nu de batch verwijderen.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Hier ziet u een uitvoer ongeveer als volgt uit:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    De laatste regel van de uitvoer ziet u dat de batch is verwijderd. Als u een taak verwijdert terwijl deze wordt uitgevoerd, wordt dit in feite de taak beëindigen. Als u een taak die is voltooid, succes of anders verwijdert celbewerkingsmodus verwijdert u hiermee de taakgegevens volledig.

## <a name="seealso"></a>Zie ook


* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyse met een in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight building temperatuur met Aircoschema gegevens analyseren](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Een Streaming: Gebruik een in HDInsight voor het samenstellen van realtime streaming-toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met behulp van een in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Maken en uitvoeren van toepassingen

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)

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
