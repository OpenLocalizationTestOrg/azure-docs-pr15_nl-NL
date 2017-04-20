<properties 
    pageTitle="Externe-pakketten gebruiken met Jupyter notitieblokken in Apache elektrische clusters op HDInsight | Azure"
    description="Stapsgewijze instructies voor het configureren van Jupyter notitieblokken beschikbaar met HDInsight Spark clusters externe elektrische-pakketten gebruiken." 
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


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Externe-pakketten gebruiken met Jupyter notitieblokken in Apache elektrische clusters op HDInsight Linux

>[AZURE.NOTE] In dit artikel is van toepassing op elektrische 1.5.2 op HDInsight 3.3 en een 1.6.2 op HDInsight 3.4. 

Meer informatie over het configureren van een notitieblok Jupyter in Apache elektrische cluster op HDInsight (Linux) gebruik van externe, community bijgedragen-pakketten die niet zijn opgenomen out-van-het-box in het cluster. 

De [Maven opslagplaats](http://search.maven.org/) voor de volledige lijst met pakketten die beschikbaar zijn, kunt u zoeken. U kunt ook een lijst met beschikbare pakketten krijgen van andere bronnen. Bijvoorbeeld is een volledige lijst van de community bijgedragen-pakketten beschikbaar binnen [Een pakketten](http://spark-packages.org/).

In dit artikel leert u hoe u de [een-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -pakket met het notitieblok Jupyter.

##<a name="prerequisites"></a>Vereisten voor

U hebt het volgende:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Externe-pakketten gebruiken met Jupyter notitieblokken 

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Klik op **Snelkoppelingen**uit het blad een cluster, en klik vervolgens op het blad **Cluster Dashboard** op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **een**.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Een nieuw Jupyter-notitieblok maken")

3. Een nieuw notitieblok is gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok aan het begin en voer een beschrijvende naam.

    ![Geef een naam voor het notitieblok] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Geef een naam voor het notitieblok")

4. Gebruikt u de `%%configure` bijzonder voor het configureren van het notitieblok om te gebruiken van een externe-pakket. Zorg ervoor dat u belt in de notitieblokken die externe-pakketten gebruiken, de `%%configure` magische in de eerste cel van de code. Dit zorgt ervoor dat de kernel is geconfigureerd voor het gebruik van het pakket voordat de sessie wordt gestart.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Als u de kernel configureren in de eerste cel vergeet, kunt u de `%%configure` met de `-f` parameter, maar die wordt de sessie opnieuw starten en alle voortgang, verloren gaan.

5. In het bovenstaande, fragment `packages` een lijst met maven coördinaten in Maven centrale opslagplaats verwacht. In dit fragment, `com.databricks:spark-csv_2.10:1.4.0` is de coördinaten maven voor **een csv** -pakket. Hier ziet u hoe u de coördinaten voor een pakket maken.

    een. Zoek het pakket in de bibliotheek Maven. Voor deze zelfstudie gebruiken we [een-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Verzamel uit de bibliotheek, de waarden voor **groeps-id**, **ArtifactId**en **versie**.

    ![Gebruik externe-pakketten met Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Gebruik externe-pakketten met Jupyter notebook")

    c. De drie waarden, gescheiden door een dubbele punt (**:**) samenvoegen.

        com.databricks:spark-csv_2.10:1.4.0

6. Voer de code-cel met de `%%configure` bijzonder. Hiermee wordt de onderliggende hier-sessie voor het gebruik van het pakket dat u hebt opgegeven configureren. U kunt nu het pakket gebruiken in de volgende cellen in het notitieblok, zoals hieronder wordt weergegeven.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. U kunt de fragmenten, klikt u vervolgens wilt uitvoeren weergegeven onder, voor het weergeven van de gegevens uit de dataframe die u in de vorige stap hebt gemaakt.

        df.show()

        df.select("Time").count()


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

* [Taken op afstand uitvoeren op een elektrische cluster met hier](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Hulpprogramma's en uitbreidingen

* [HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ IDEE maken en indienen elektrische Scala toepassingen gebruiken](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Gebruik HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ verloop foutopsporing elektrische toepassingen op afstand uitvoeren](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Zeppelin notitieblokken gebruikt met een cluster elektrische op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels beschikbaar voor Jupyter notitieblok in een cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter installeert op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)
