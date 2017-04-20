<properties 
    pageTitle="Zeppelin notitieblokken gebruikt met een cluster op HDInsight Linux | Microsoft Azure" 
    description="Stapsgewijze instructies voor het gebruik van Zeppelin notitieblokken met een clusters op HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Zeppelin notitieblokken gebruikt met Apache elektrische cluster op HDInsight Linux

HDInsight Spark clusters opnemen Zeppelin notitieblokken die u kunt een taken uitvoeren. In dit artikel leert u hoe u het notitieblok Zeppelin gebruikt op een cluster HDInsight.


**Vereisten:**

* Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Een cluster Apache elektrische. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Een notebook Zeppelin starten

1. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Zeppelin notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Zeppelin voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Een nieuw notitieblok maken. Klik op **notitieblok**vanuit het deelvenster kop en klik vervolgens op **Nieuwe notitie maken**.

    ![Een nieuw Zeppelin-notitieblok maken] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Een nieuw Zeppelin-notitieblok maken")

    Voer een naam voor het notitieblok en klik vervolgens op **Notitie maken**.

3. Zorg er ook voor dat de kop van het notitieblok ziet u de status van een verbonden. Dit wordt aangeduid met een groene stip in de rechterbovenhoek.

    ![Zeppelin notitieblok status] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin notitieblok status")

4. Voorbeeldgegevens in een tijdelijke tabel laden. Wanneer u een een-cluster in HDInsight maakt, wordt het voorbeeld van gegevensbestand, **hvac.csv**, gekopieerd naar het bijbehorende opslag account onder **\HdiSamples\SensorSampleData\hvac**.

    In de lege alinea die standaard in het nieuwe notitieblok is gemaakt, plak de volgende fragment.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Druk op **SHIFT + ENTER** of klik op de knop **afspelen** van de alinea om uit te voeren van het fragment. De status in de rechterbenedenhoek van de alinea moet werken vanaf READY, nog moeten worden uitgevoerd op gereed. De uitvoer weergegeven onderaan in dezelfde alinea. De schermafbeelding ziet er als volgt uit:

    ![Een tijdelijke tabel maken van onbewerkte gegevens] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Een tijdelijke tabel maken van onbewerkte gegevens")

    U kunt ook een titel aan elke alinea geven. Vanaf de rechterkant hoek, klikt u op het pictogram **Instellingen** en klik vervolgens op **Titel weergeven**.

5. U kunt nu een SQL-instructies in de tabel **Aircoschema** uitvoeren. Plak de volgende query in een nieuwe alinea. De query haalt de building-ID en het verschil tussen de doel- en werkelijke temperaturen voor elke maken op een bepaalde datum. Druk op **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    De **% sql** -instructie aan het begin Hiermee wordt aan het notitieblok dat u wilt de interpreter hier Scala gebruiken.

    De volgende schermafbeelding ziet u de uitvoer.

    ![Een een SQL-instructie met het notitieblok uitvoert] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Een een SQL-instructie met het notitieblok uitvoert")

     Klik op de weergaveopties (gemarkeerd rechthoek) om te schakelen tussen verschillende weergaven voor hetzelfde resultaat op. Klik op **Instellingen** om te kiezen welke consitutes de sleutel en de waarden in de uitvoer. De schermopname hierboven gebruikt **buildingID** als de sleutel en het gemiddelde van **temp_diff** als de waarde.

    
6. U kunt ook een SQL-instructies met de variabelen in de query uitvoeren. Het codefragment van de volgende ziet hoe u een variabele, **Temp**, in de query met de mogelijke waarden die u wilt opvragen met definiëren. Wanneer u eerst de query uitvoert, wordt automatisch een vervolgkeuzelijst gevuld met de waarden die u hebt opgegeven voor de variabele.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Het codefragment van deze plakken in een nieuwe alinea en druk op **SHIFT + ENTER**. De volgende schermafbeelding ziet u de uitvoer.

    ![Een een SQL-instructie met het notitieblok uitvoert] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Een een SQL-instructie met het notitieblok uitvoert")

    U kunt voor de volgende query's, selecteer een nieuwe waarde in de vervolgkeuzelijst en voer de query nogmaals uit. Klik op **Instellingen** om te kiezen welke consitutes de sleutel en de waarden in de uitvoer. De bovenstaande schermopname gebruikt **buildingID** als de toets, het gemiddelde van **temp_diff** als de waarde en **targettemp** als de groep.

7. Start de interpreter hier om af te sluiten van de toepassing opnieuw. Hiervoor interpreter instellingen openen door te klikken op de aangemelde in gebruikersnaam in te voeren in de rechterbovenhoek en klik vervolgens op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Uitvoer component")

2. Schuif naar hier interpreter instellingen en klik op **opnieuw starten**.

    ![Start opnieuw de intepreter hier] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Start opnieuw de intepreter Zeppelin")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Hoe gebruik ik externe-pakketten met het notitieblok?

U kunt het notitieblok Zeppelin configureren in een Apache cluster op HDInsight (Linux) als u wilt gebruiken, externe, community bijgedragen-pakketten die niet opgenomen out-van-het-box in het cluster zijn. De [Maven opslagplaats](http://search.maven.org/) voor de volledige lijst met pakketten die beschikbaar zijn, kunt u zoeken. U kunt ook een lijst met beschikbare pakketten krijgen van andere bronnen. Bijvoorbeeld is een volledige lijst van de community bijgedragen-pakketten beschikbaar binnen [Een pakketten](http://spark-packages.org/).

In dit artikel ziet u hoe u de [een-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -pakket met het notitieblok Jupyter.

1. Open interpreter-instellingen. In de rechterbovenhoek op de aangemelde in gebruikersnaam in te voeren en klik vervolgens op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Uitvoer component")

2. Schuif naar hier interpreter instellingen en klik vervolgens op **bewerken**.

    ![Interpreter-instellingen wijzigen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Interpreter-instellingen wijzigen")

3. Voeg een nieuwe sleutel, **livy.spark.jars.packages** genoemd en stelt u de waarde in de indeling `group:id:version`. Als u gebruiken het [elektrische-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -pakket wilt, moet u zo is, de waarde van de toets om instellen `com.databricks:spark-csv_2.10:1.4.0`.

    ![Interpreter-instellingen wijzigen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Interpreter-instellingen wijzigen")

    Klik op **Opslaan** en start de interpreter hier.

4. **Tip**: als u wilt begrijpen hoe aankomt op de waarde van de sleutel hierboven, hier ziet u hoe.

    een. Zoek het pakket in de bibliotheek Maven. Voor deze zelfstudie we [elektrische-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)gebruikt.
    
    b. Verzamel uit de bibliotheek, de waarden voor **groeps-id**, **ArtifactId**en **versie**.

    ![Gebruik externe-pakketten met Jupyter notebook] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Gebruik externe-pakketten met Jupyter notebook")

    c. De drie waarden, gescheiden door een dubbele punt (**:**) samenvoegen.

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Waar zijn de Zeppelin-notitieblokken opgeslagen?

De Zeppelin-notitieblokken worden opgeslagen in de headnodes cluster. Zodat u, als u het cluster verwijdert, worden de notitieblokken ook verwijderd. Als u behouden van uw notitieblokken voor later gebruik op andere clusters wilt, moet u deze exporteren als u klaar bent met het uitvoeren van de taken. Als u wilt exporteren van een notitieblok, klikt u op het pictogram **exporteren** zoals wordt weergegeven in de onderstaande afbeelding.

![Notitieblok downloaden] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Downloaden van het notitieblok")

Hiermee wordt het notitieblok opgeslagen als een JSON-bestand op uw downloadlocatie.

## <a name="livy-session-management"></a>Hier sessiebeheer

Wanneer u de eerste alinea van de code in uw notitieblok Zeppelin uitvoert, wordt een nieuwe hier-sessie gemaakt in uw cluster HDInsight Spark. Deze sessie worden verdeeld over alle Zeppelin notitieblokken die u maakt. Als voor een reden de hier sessie is beëindigd (cluster opnieuw opstarten, enzovoort), is het niet mogelijk om uit te voeren taken van het notitieblok Zeppelin.

In dat geval, moet u de volgende stappen uitvoeren voordat u kunt uitvoeren van taken uit een notitieblok Zeppelin. 

1. Start opnieuw op de hier interpreter van het notitieblok Zeppelin. Hiervoor interpreter instellingen openen door te klikken op de aangemelde in gebruikersnaam in te voeren in de rechterbovenhoek en klik vervolgens op **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Uitvoer component")

2. Schuif naar hier interpreter instellingen en klik op **opnieuw starten**.

    ![Start opnieuw de intepreter hier] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Start opnieuw de intepreter Zeppelin")

3. Een cel code uitvoeren vanaf een bestaand Zeppelin-notitieblok. Hiermee maakt u een nieuwe hier-sessie in het cluster HDInsight.

## <a name="seealso"></a>Zie ook


* [Overzicht: Apache elektrische op Azure HDInsight](hdinsight-apache-spark-overview.md)

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

* [Kernels beschikbaar voor Jupyter notitieblok in een cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Externe-pakketten gebruiken met Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter installeren op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

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







