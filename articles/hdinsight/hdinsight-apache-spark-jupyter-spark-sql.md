<properties
    pageTitle="Maak een cluster elektrische op HDInsight Linux en gebruiken van een SQL-code uit Jupyter voor een interactieve analyse | Microsoft Azure"
    description="Stapsgewijze instructies voor het snel maken een elektrische Apache cluster in HDInsight en vervolgens met een SQL vanaf Jupyter notitieblokken interactieve query's uitvoeren."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Aan de slag: Maak een Apache cluster op HDInsight Linux interactieve query's en uitvoeren met een SQL

Informatie over het maken van een cluster Apache elektrische in HDInsight en gebruikt u [Jupyter](https://jupyter.org) notitieblok interactieve een SQL-query's uitvoeren op het cluster elektrische.

   ![Aan de slag met Apache elektrische in HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Aan de slag met Apache elektrische in zelfstudie HDInsight. Stappen geïllustreerd: Maak een account opslag; maken van een cluster. Een SQL-instructies uitvoeren")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Vereisten voor

- **Een Azure-abonnement**. Voordat u deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **A Secure Shell (SSH)-client**: Linux, Unix en OS X-systemen opgegeven een SSH client via het `ssh` opdracht. Voor Windows-systemen raden [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Secure Shell (SSH) toetsen (optioneel)**: U kunt het SSH-account gebruikt om verbinding met het cluster met een wachtwoord of een openbare sleutel te beveiligen. Met een wachtwoord, krijgt u snel de slag en moet u deze optie als u wilt snel een cluster maken en uitvoeren van bepaalde testtaken. Met behulp van een sleutel is veiliger, maar er aanvullende instellingen moeten. Mogelijk wilt u deze methode te gebruiken bij het maken van een cluster productie. In dit artikel gebruiken we de methode wachtwoord. Raadpleeg de volgende artikelen voor instructies over het maken en gebruiken van SSH toetsen met HDInsight:

    -  Vanuit een Linux-computer - [Gebruik SSH met Linux gebaseerde HDInsight (Hadoop) uit Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Vanaf een computer met Windows - [Gebruik SSH met Linux gebaseerde HDInsight (Hadoop) vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] In dit artikel wordt een manager-sjabloon van Azure resource gebruikt een een-cluster met [Azure opslag BLOB's als de clusteropslag](hdinsight-hadoop-use-blob-storage.md)maken. U kunt ook een een-cluster dat [Azure Lake gegevensopslag](../data-lake-store/data-lake-store-overview.md) als een extra opslagruimte, naast Azure opslag BLOB's als de standaard-opslag gebruikt maken. Zie [een HDInsight cluster met Lake gegevensopslag maken](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)voor instructies.

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Een cluster maken

In dit gedeelte maakt u een HDInsight versie 3.4 cluster (een versie 1.6.1) met een sjabloon voor het beheren van Azure resource. Zie voor informatie over HDInsight versies en hun serviceovereenkomsten [HDInsight onderdeel versiebeheer](hdinsight-component-versioning.md). Zie voor andere methoden voor het maken van cluster [clusters HDInsight maken](hdinsight-hadoop-provision-linux-clusters.md).

1. Klik op de volgende afbeelding om te openen van de sjabloon in de Portal Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De sjabloon bevindt zich in een openbare blob container, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Voer de volgende gegevens van het blad Parameters:

    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster aanmeldingsnaam en wachtwoord**: de standaard-aanmeldingsnaam is beheerder.
    - **SSH-gebruikersnaam en wachtwoord**.
    
    Noteer deze waarden.  U moet ze later in deze zelfstudie.

    > [AZURE.NOTE] SSH wordt extern toegang tot het HDInsight cluster met een opdrachtregel gebruikt. De gebruikersnaam en wachtwoord dat u hier gebruikt, wordt gebruikt wanneer u verbinding maakt met het cluster via SSH. Ook zijn de naam van de gebruiker SSH uniek, zoals een gebruikersaccount op de HDInsight knooppunten gemaakt. De volgende zijn enkele van de accountnamen gereserveerd voor gebruik door services op het cluster en kunnen niet worden gebruikt als de naam van de gebruiker SSH:
    >
    > hoofdmap, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, garens, mapred, hbase, component, oozie, falcon, sqoop, beheerder, tez, hcat, hdinsight-zookeeper.

    > Zie een van de volgende artikelen voor meer informatie over het gebruik van SSH met HDInsight:

    > * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Klik op **OK** als u wilt opslaan van de parameters.

4. van het blad **aangepaste implementatie** op **resourcegroep** vervolgkeuzelijst in en klik vervolgens op **Nieuw** om een nieuwe resourcegroep te maken. De resourcegroep is een container waarin het cluster, het afhankelijke opslag-account en andere gekoppelde resource worden gegroepeerd.

5. op **juridische voorwaarden**en klik vervolgens op **maken**.

6. Klik op **maken**. Hier ziet u een nieuwe tegel getiteld Submitting implementatie voor implementatie van de sjabloon. Het duurt over ongeveer 20 minuten om het cluster en SQL-database te maken.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Een SQL-query's met een notitieblok Jupyter uitvoeren

In deze sectie gebruikt u Jupyter notitieblok naar een SQL-query's uitvoeren op het cluster een. HDInsight Spark clusters bieden twee kernels die u bij het notitieblok Jupyter kunt gebruiken. Dit zijn:

* **PySpark** (voor toepassingen geschreven in Python)
* **Elektrische** (voor toepassingen geschreven in Scala)

In dit artikel gebruikt u de kernel PySpark. In het artikel [Kernels beschikbaar op Jupyter notitieblokken met een HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) kunt u lezen in details over de voordelen van het gebruik van de kernel PySpark. Enkele van de belangrijkste voordelen van het gebruik van de kernel PySpark zijn echter:

* U hoeft niet het contexten instellen voor een en -component. Deze worden automatisch voor u ingesteld.
* U kunt cel magics, zoals `%%sql`, rechtstreeks uw SQL- of component query's worden uitgevoerd, zonder een voorgaande codefragmenten.
* De uitvoer voor de query's voor SQL- of component wordt automatisch weergegeven.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Jupyter notitieblok met PySpark kernel maken 

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Een nieuw notitieblok maken. Klik op **Nieuw**en klik vervolgens op **PySpark**.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Een nieuw Jupyter-notitieblok maken")

3. Een nieuw notitieblok is gemaakt en geopend met de naam Untitled.pynb. Klik op de naam van het notitieblok aan het begin en voer een beschrijvende naam.

    ![Geef een naam voor het notitieblok] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Geef een naam voor het notitieblok")

4. Omdat u een notitieblok met de kernel PySpark hebt gemaakt, hoeft u niet alle contexten expliciet maken. Elektrische en component contexten wordt automatisch voor u gemaakt wanneer u de eerste cel van de code uitvoert. U kunt starten door te importeren van de typen die is vereist voor dit scenario. Klik hiervoor het volgende codefragment plakken in een cel en druk op **SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Telkens wanneer u een taak in Jupyter uitvoert, wordt een status **(bezet)** samen met de titel van het notitieblok in uw web browser venstertitel weergegeven. U ziet ook een effen cirkel naast de tekst **PySpark** in de rechterbovenhoek. Nadat de taak is voltooid, wordt deze gewijzigd in een lege cirkel.

     ![Status van een taak Jupyter-notitieblok] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status van een taak Jupyter-notitieblok")

4. Voorbeeldgegevens in een tijdelijke tabel laden. Wanneer u een een-cluster in HDInsight maakt, wordt het voorbeeld van gegevensbestand, **hvac.csv**, gekopieerd naar de bijbehorende opslag account **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Plak in het volgende voorbeeld in een lege cel en druk op **SHIFT + ENTER**. In dit codevoorbeeld wordt de gegevens naar een tijdelijke tabel **Aircoschema**genoemd.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Aangezien u een kernel PySpark gebruikt, kunt u nu rechtstreeks uitvoeren een SQL-query op de tijdelijke tabel **Aircoschema** die u zojuist hebt gemaakt met behulp van de `%%sql` bijzonder. Voor meer informatie over de `%%sql` bijzonder, evenals andere magics beschikbaar voor communicatie met de kernel PySpark, raadpleegt u [Kernels beschikbaar op Jupyter notitieblokken met een HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Nadat de taak is voltooid, wordt het volgende in tabelvorm uitvoer al dan niet standaard weergegeven.

    ![Tabel-uitvoer van de queryresultaten] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Tabel-uitvoer van de queryresultaten")

    U kunt ook de resultaten bekijken in als u ook andere visualisaties. Een grafiek gebied voor de dezelfde uitvoer er bijvoorbeeld, zoals de volgende.

    ![Gebied grafiek van de queryresultaten] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Gebied grafiek van de queryresultaten")


6. Nadat u klaar bent met het uitvoeren van de toepassing, moet u het notitieblok dat u wilt de bronnen vrijgeven afsluiten. Klik hiertoe vanuit het menu **bestand** op het notitieblok, klikt u op **sluiten en stoppen**. Deze uitgeschakeld en het notitieblok sluiten.

##<a name="delete-the-cluster"></a>Het cluster verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Zie ook


* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario 's

* [Elektrische met BI: interactieve gegevensanalyses elektrische in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight building temperatuur met Aircoschema gegevens analyseren](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Elektrische met Machine Learning: gebruik een in HDInsight eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Een Streaming: Gebruik een in HDInsight voor het samenstellen van realtime streaming-toepassingen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website logboekanalyse met behulp van een in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Toepassing inzicht telemetrielogboek gegevensanalyse met een in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
