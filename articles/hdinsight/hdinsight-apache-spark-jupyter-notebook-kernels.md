<properties 
    pageTitle="Beschikbaar voor communicatie met Jupyter notitieblokken op HDInsight Spark kernels clusters op Linux | Microsoft Azure" 
    description="Meer informatie over de beschikbaar met een cluster op HDInsight Linux extra Jupyter notitieblok kernels." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Beschikbaar voor Jupyter notitieblokken met Apache elektrische clusters op HDInsight Linux kernels

Apache elektrische cluster op HDInsight (Linux) bevat Jupyter notitieblokken die u gebruiken kunt om te testen van uw toepassingen. Een kernel is een programma dat wordt uitgevoerd en uw code interpreteert. HDInsight Spark clusters bieden twee kernels die u bij het notitieblok Jupyter kunt gebruiken. Dit zijn:

1. **PySpark** (voor toepassingen geschreven in Python)
2. **Elektrische** (voor toepassingen geschreven in Scala)

In dit artikel leert u over het gebruik van deze kernels en wat zijn de voordelen van ze te gebruiken.

**Vereisten:**

U hebt het volgende:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Een cluster Apache elektrische op HDInsight Linux. Zie voor instructies voor het [maken Apache elektrische clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Hoe gebruik ik de kernels? 

1. In de [Portal van Azure](https://portal.azure.com/)uit de startboard, klikt u op de tegel voor uw cluster elektrische (als u deze aan de startboard vastgemaakt). U kunt ook naar uw cluster onder **Door alles bladeren**gaan > **HDInsight Clusters**.   

2. Van het blad een cluster, klik op **Dashboard Cluster**en klik vervolgens op **Jupyter notitieblok**. Als u wordt gevraagd, voert u de beheerdersreferenties voor het cluster.

    > [AZURE.NOTE] U mogelijk ook het notitieblok Jupyter voor uw cluster bereiken door te openen van de volgende URL in uw browser. __CLUSTERNAAM__ vervangen door de naam van uw cluster:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Maak een nieuw notitieblok met de nieuwe kernels. Klik op **Nieuw**en klik vervolgens op **Pyspark** of **een**. U moet de elektrische kernel voor Scala-toepassingen en PySpark kernel voor Python toepassingen gebruiken.

    ![Een nieuw Jupyter-notitieblok maken] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Een nieuw Jupyter-notitieblok maken") 

3. Dit moet een nieuw notitieblok openen met de kernel die u hebt geselecteerd.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Waarom moet ik de PySpark of een kernels gebruiken?

Hier volgen een paar voordelen van het gebruik van de nieuwe kernels.

1. **Vooraf ingestelde contexten**. Met de **PySpark** of **een** kernels die worden geleverd met Jupyter notitieblokken, hoeft u niet expliciet elektrische of component contexten instellen voordat u kunt werken met de toepassing die u ontwikkelt; Deze zijn beschikbaar voor u al dan niet standaard. Volgende contexten zijn:

    * **sc** - voor een context
    * **sqlContext** - voor component context


    U moet dus niet instructies als volgt te werk om in te stellen van de contexten uitvoeren:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    U kunt de vooraf ingestelde contexten in plaats daarvan rechtstreeks in uw toepassing gebruiken.
    
2. **Cel magics**. De kernel PySpark vindt u enkele vooraf 'magics', die zijn speciale opdrachten die u met bellen kunt `%%` (bijvoorbeeld `%%MAGIC` <args>). De opdracht magische moet worden het eerste woord in een cel code en toestaan voor meerdere regels van inhoud. Het woord magische moet het eerste woord in de cel. Toevoegen van vóór de magie, even opmerkingen, treedt een fout.   Zie voor meer informatie over magics, [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    De onderstaande tabel worden de verschillende magics beschikbaar via de kernels.

  	| Bijzonder     | Voorbeeld                         | Beschrijving  |
  	|-----------|---------------------------------|--------------|
  	| Help      | `%%help`                            | Genereert een tabel met alle beschikbare magics met voorbeeld en beschrijving |
  	| Info      | `%%info`                          | Uitvoer van sessie-informatie voor het huidige hier eindpunt |
  	| configureren | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Hiermee configureert u de parameters voor het maken van een sessie. De vlag (-f) is verplicht als een sessie al is gemaakt en de sessie wordt verwijderd en opnieuw ingesteld. Bekijk [hier van bericht /sessions hoofdtekst aanvragen](https://github.com/cloudera/livy#request-body) voor een lijst met geldige parameters. Parameters worden doorgegeven als een tekenreeks JSON en moeten op de volgende regel na de magie, zoals wordt weergegeven in de voorbeeldkolom. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Hiermee voert u een query component ten opzichte van de sqlContext. Als de `-o` parameter wordt overschreden, wordt het resultaat van de query is doorgevoerd in de %% lokale Python context als een dataframe [Pandas](http://pandas.pydata.org/) .   |
  	| lokale     |     `%%local`<br>`a=1`              | De code in de volgende regels worden lokaal worden uitgevoerd. Code moet geldige Python code. |
  	| Logboeken      | `%%logs`                        | Uitvoer van de logboeken voor de huidige sessie van hier.  |
  	| verwijderen    | `%%delete -f -s <session number>` | Hiermee verwijdert u een bepaalde sessie van het huidige hier-eindpunt. Houd er rekening mee dat u de sessie die is gestart voor de kernel zelf niet verwijderen. |
  	| opruimen   | `%%cleanup -f`                    | Hiermee verwijdert u alle sessies voor het huidige hier eindpunt, met inbegrip van dit notitieblok sessie. De werking van de vlag -f is verplicht.  |

    >[AZURE.NOTE] Naast het magics door de kernel PySpark hebt toegevoegd, kunt u ook de [ingebouwde IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), met inbegrip van `%%sh`. U kunt de `%%sh` bijzonder uitvoeren van scripts en blok met code op de headnode cluster. 

3. **Automatisch visualisatie**. De kernel **Pyspark** gevisualiseerd automatisch de uitvoer van component en SQL-query's. Hebt u de optie te kiezen tussen verschillende typen visualisaties met inbegrip van de tabel-, cirkel-, lijn-, gebied-, staaf.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameters die worden ondersteund met de %% sql bijzonder

De %% sql bijzonder ondersteunt verschillende parameters die u gebruiken kunt om te bepalen van het soort uitvoer dat wordt weergegeven wanneer u query's uitvoeren. De volgende tabel bevat de uitvoer.

| Parameter     | Voorbeeld                         | Beschrijving  |
|-----------|---------------------------------|--------------|
| + o      | `-o <VARIABLE NAME>`                          | Deze parameter gebruiken om het resultaat van de query in de %% lokale Python context, als een dataframe [Pandas](http://pandas.pydata.org/) . De naam van de variabele dataframe is de naam van de variabele die u opgeeft. |
| -q      | `-q`                          | Gebruik deze visualisaties voor de cel uitschakelen. Als u niet wilt de inhoud van een cel met een automatisch te visualiseren en wilt dit als een dataframe vastleggen en gebruik `-q -o <VARIABLE>`. Als u uitschakelen visualisaties wilt zonder het vastleggen van de resultaten (bijvoorbeeld voor het uitvoeren van een SQL-query met kant-effecten, zoals een `CREATE TABLE` instructie), alleen gebruiken `-q` zonder op te geven een `-o` argument. |
| -m       |  `-m <METHOD>`    | Waar **methode** **nemen** of **voorbeeld** is (standaard is **uitvoeren**). Als de methode **duren is**, hervat de kernel elementen vanaf de bovenkant van de resultatenset voor gegevens die worden aangegeven door MAXROWS (Zie verderop in deze tabel). Als de methode **steekproef**, het kernel elementen van een gegevensverzameling volgens willekeurig voorbeeld `-r` parameter, hieronder wordt beschreven in deze tabel.   |
| -r     |     `-r <FRACTION>`            | Hier ziet u **breuk** een drijvendekommagetal tussen 0,0 en 1,0. Als de methode steekproef voor de SQL-query `sample`, en vervolgens de kernel willekeurig voorbeelden voor het opgegeven deel van de elementen van de resultatenset voor u; bijvoorbeeld als u een SQL-query met de argumenten uitvoeren `-m sample -r 0.01`, en vervolgens 1% van de Resultatenrijen worden willekeurig partijen. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** is een geheel getal. De kernel wordt het aantal rijen in dat uitvoer moet **MAXROWS**beperken. Als **MAXROWS** een negatief getal zoals **-1 is**, klikt u vervolgens het aantal rijen in de resultatenset worden niet beperkt. |

**Voorbeeld:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

De bovenstaande instructie gebeurt het volgende:

* Hiermee selecteert u alle records vanaf **hivesampletable**.
* Omdat we - q gebruiken, worden uitgeschakeld auto-visualisatie.
* Omdat we gebruiken `-m sample -r 0.1 -n 500` er willekeurig voorbeelden van 10% van de rijen in de hivesampletable en de grootte van de resultatenset 500 rijen beperkt.
* Ten slotte, omdat we gebruikt `-o query2` ook worden de uitvoer opgeslagen in een dataframe **query2**genoemd.
    

## <a name="considerations-while-using-the-new-kernels"></a>Overwegingen bij het gebruik van de nieuwe kernels

Ongeacht kernel die u gebruikt (PySpark of een), blijft de notitieblokken die zijn uitgevoerd gebruikt uw cluster resources.  Met deze kernels, omdat de contexten vooraf ingestelde zijn, gewoon afsluiten de notitieblokken worden niet beëindigen de context en dus de de resources cluster blijft in gebruik. Een goede gewoonte met de PySpark en een kernels zou u met de optie **sluiten en stoppen** vanuit de menu **bestand** van het notitieblok. Hiermee de context beëindigd en vervolgens afgesloten het notitieblok.    


## <a name="show-me-some-examples"></a>Enkele voorbeelden weergeven

Wanneer u een notitieblok Jupyter opent, ziet u twee mappen die beschikbaar zijn op het hoogste niveau.

* De map **PySpark** heeft steekproef notitieblokken met de nieuwe **Python** kernel.
* De map **Scala** heeft steekproef notitieblokken met de nieuwe **elektrische** kernel.

U kunt de **00 - [lezen mij eerst] een bijzonder Kernel functies** -notitieblok openen vanuit de **PySpark** of **een** map voor meer informatie over de verschillende magics beschikbaar. U kunt ook de andere beschikbare steekproef-notitieblokken onder de twee mappen gebruiken om te leren hoe u verschillende scenario's Jupyter notitieblokken gebruikt met HDInsight Spark clusters bereiken.

## <a name="where-are-the-notebooks-stored"></a>Waar kan ik de notitieblokken opgeslagen?

Jupyter notitieblokken worden opgeslagen in het opslag-account dat is gekoppeld aan het cluster onder de map **/HdiNotebooks** .  Notitieblokken, tekstbestanden en mappen die u vanuit Jupyter maakt zijn toegankelijk vanaf WASB.  Als u Jupyter gebruikt om een map **mijnmap** en een notitieblok **myfolder/mynotebook.ipynb**te maken, kunt u bijvoorbeeld op van dat notitieblok openen `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Omgekeerd geldt ook, dat wil zeggen als u rechtstreeks bij uw account opslag voor om een notitieblok te uploaden `/HdiNotebooks/mynotebook1.ipynb`, het notitieblok ook van Jupyter zichtbaar zijn.  Notitieblokken blijft in het account opslag, zelfs nadat het cluster wordt verwijderd.

De manier waarop notitieblokken worden opgeslagen in de opslagruimte-account is compatibel met HDFS. Dat, als u SSH in het cluster dat kunt u opdrachten voor het beheer als volgt uit:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Als er problemen toegang krijgen tot het account opslag voor het cluster zijn, de notitieblokken ook zijn opgeslagen op de headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Ondersteunde browser
Jupyter notitieblokken uitgevoerd op HDInsight Spark clusters worden alleen ondersteund op Google Chrome.

## <a name="feedback"></a>Feedback

De nieuwe kernels nog verder te fase zijn en zal vervallen na verloop van tijd. Dit kan ook betekenen dat API's kan worden gewijzigd wanneer deze kernels vervallen. Waarderen we feedback die u hebt bij het gebruik van deze nieuwe kernels zou doen. Dit is bijzonder nuttig zijn in de definitieve versie van deze kernels structureren. U kunt feedback uw opmerkingen/onder de sectie **opmerkingen** onderaan in dit artikel.


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

* [Zeppelin notitieblokken gebruikt met een cluster elektrische op HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Externe-pakketten gebruiken met Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter installeren op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)
