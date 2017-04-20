<properties 
    pageTitle="Installeer Jupyter notitieblok op uw computer en verbindt u deze aan een cluster HDInsight Spark | Microsoft Azure" 
    description="Meer informatie over hoe u Jupyter notitieblok lokaal installeert op uw computer en deze verbinden met een cluster Apache elektrische op Azure HDInsight." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Installeer Jupyter notitieblok op uw computer en verbinding maken met een Apache cluster op HDInsight Linux

In dit artikel leert u hoe u Jupyter notitieblok met de aangepaste PySpark (voor Python) en een (voor Scala) kernels met een bijzonder installeren en verbindt u het notitieblok met een cluster HDInsight. Er kan een aantal redenen Jupyter installeren op uw lokale computer, en kunnen er ook enkele uitdagingen. Zie de sectie [Waarom zou ik Jupyter op mijn computer installeren](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel voor een lijst met redenen en uitdagingen.

Er zijn drie belangrijke stappen voor het installeren van Jupyter en de magie elektrische op uw computer.

* Jupyter notitieblok installeren
* Installeren van de kernels PySpark en een met de magie elektrische
* Een bijzonder voor toegang tot een cluster op HDInsight configureren

Zie voor meer informatie over de aangepaste kernels en de magie elektrische beschikbaar voor Jupyter notitieblokken met HDInsight cluster [Kernels beschikbaar voor Jupyter notitieblokken met Apache elektrische Linux clusters op HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Vereisten voor

De vereisten die hier wordt vermeld, zijn niet voor de installatie van Jupyter. Dit zijn voor het notitieblok Jupyter verbinden met een cluster HDInsight wanneer het notitieblok is geïnstalleerd.

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter notitieblok op uw computer installeren

Voordat u Jupyter notitieblokken kunt installeren, moet u Python installeren. Zowel Python en Jupyter zijn beschikbaar als onderdeel van de [Ananconda verdeling](https://www.continuum.io/downloads). Wanneer u Anaconda installeert, installeren u daadwerkelijk de verdeling van Python. Wanneer Anaconda is geïnstalleerd, kunt u de installatie Jupyter toevoegen door een opdracht uit te voeren. Hier vindt de instructies die u moet volgen.

1. Het [installatieprogramma van Anaconda](https://www.continuum.io/downloads) voor uw platform downloaden en het installatieprogramma uitvoert. Tijdens het uitvoeren van de wizard setup, moet dat u de optie Anaconda toevoegen aan uw variabele pad selecteren.

2. Voer de volgende opdracht Jupyter installeren.

        conda install jupyter

    Zie voor meer informatie over installting Jupyter, [Installatie Jupyter Anaconda gebruiken](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Installeer de kernels en een bijzonder

Voor instructies over het installeren van de magie elektrische, Zie de PySpark en een kernels, de [sparkmagic documentatie](https://github.com/jupyter-incubator/sparkmagic#installation) op GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Een bijzonder voor toegang tot het cluster HDInsight Spark configureren

In deze sectie configureert u de elektrische magie die u eerder hebt geïnstalleerd als u wilt verbinden met een Apache elektrische cluster die moet u al hebt gemaakt in Azure HDInsight.

1. De configuratiegegevens Jupyter is meestal opgeslagen in de basismap van gebruikers. Als u wilt zoeken naar de basismap op elk platform OS, typt u de volgende opdrachten.

    Start de shell Python. Klik op een opdrachtvenster, typt u het volgende:

        python

    Voer de volgende opdracht uit om vast te stellen de basismap op de shell Python.

        import os
        print(os.path.expanduser('~'))

2. Ga naar de basismap en maken van een map genaamd **.sparkmagic** als deze nog niet bestaat.

3. In de map een bestand met de naam **config.json** maken en toevoegen van de volgende JSON-fragment binnen de vorm.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. **{USERNAME}**, **{CLUSTERDNSNAME}**en **{BASE64ENCODEDPASSWORD}** met de juiste waarden vervangen. U kunt een aantal hulpprogramma's in uw favoriete programmeertaal of online een base64-gecodeerde wachtwoord voor uw wachtwoord actualy genereren. Het codefragment van een eenvoudige Python om uit te voeren vanuit uw opdrachtprompt zou zijn:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Jupyter starten. Gebruik de volgende opdracht uit de opdrachtprompt.

        jupyter notebook

6. Controleer of dat u verbinding kunt maken met het gebruik van het notitieblok Jupyter cluster en dat u de magie elektrische beschikbaar met de kernels gebruiken kunt. De volgende stappen uitvoeren.

    1. Een nieuw notitieblok maken. Klik op de rechterhoek op **Nieuw**. U ziet de standaard-kernel **Python2** en de twee nieuwe kernels die u hebt geïnstalleerd, **PySpark** en **een**.

        ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Een nieuw Jupyter-notitieblok maken")

    
        Klik op **PySpark**.


    2. Voer het volgende codefragment.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Als u wel de uitvoer ophaalt kunt, wordt de verbinding met het cluster HDInsight getest.

    >[AZURE.TIP] Als u bijwerken van de configuratie notitieblok verbinding maken met een ander cluster wilt, moet u de config.json bijwerken met de nieuwe set waarden, zoals wordt weergegeven in stap 3 hierboven. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom moet ik Jupyter op mijn computer installeren?

Er zijn een aantal redenen waarom u wilt mogelijk Jupyter installeert op uw computer en verbindt u deze met een cluster elektrische op HDInsight.

* Hoewel Jupyter notitieblokken al beschikbaar op het elektrische cluster in Azure HDInsight zijn, vindt Jupyter installeren op uw computer u de optie voor het maken van uw notitieblokken uw toepassing ten opzichte van een lopende cluster lokaal, testen en vervolgens de notitieblokken te uploaden naar het cluster. Als u wilt uploaden de notitieblokken aan het cluster, kunt u uploadt u ze via het Jupyter-notitieblok dat wordt uitgevoerd of het cluster, of ze opslaan in de map /HdiNotebooks in het opslag-account dat is gekoppeld aan het cluster. Voor meer informatie over hoe notitieblokken worden opgeslagen op het cluster, zien [waar Jupyter notitieblokken worden opgeslagen](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Met de beschikbare notitieblokken lokaal, u kunt verbinding maken met verschillende elektrische clusters op basis van het vereiste voor uw toepassing.
* U kunt GitHub implementeren van een systeem voor bronbeheer gebruikt en hebt versiebeheer voor de notitieblokken. U kunt ook een samenwerkingsomgeving waar meerdere gebruikers met hetzelfde notitieblok werken kunnen hebben.
* U kunt werken met notitieblokken lokaal zonder een cluster omhoog. Hoeft u alleen een cluster om te testen van uw notitieblokken tegen, niet aan uw notitieblokken of een ontwikkelomgeving handmatig te beheren.
* Het is mogelijk beter voor het configureren van uw eigen lokale ontwikkelomgeving dan de configureren voor de installatie Jupyter op het cluster.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder een of meer externe clusters configureren.

>[AZURE.WARNING] Jupyter op uw lokale computer is geïnstalleerd, kunnen meerdere gebruikers kunnen hetzelfde notitieblok op hetzelfde elektrische cluster uitvoeren op hetzelfde moment. In een dergelijke situatie, worden meerdere hier sessies gemaakt. Als u een probleem ondervindt en wilt opsporen die, is dit een complexe taak voor het bijhouden van welke sessie hier welke gebruiker behoort.




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

* [Externe-pakketten gebruiken met Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)
