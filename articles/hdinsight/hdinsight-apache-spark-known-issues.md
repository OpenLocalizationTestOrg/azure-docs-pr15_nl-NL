<properties 
    pageTitle="Bekende problemen van Apache elektrische in HDInsight | Microsoft Azure" 
    description="Bekende problemen van Apache elektrische in HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Bekende problemen voor een Apache cluster op HDInsight Linux

In dit document houdt van alle bekende problemen voor de openbare preview HDInsight Spark.  

##<a name="livy-leaks-interactive-session"></a>Hier lekt interactieve sessie
 
Wanneer u hier opnieuw is opgestart met een interactieve sessie (van Ambari of vanwege headnode 0 virtuele computer opnieuw is opgestart) nog in leven, is een sessie voor een interactieve taak wordt meer. Reden, nieuwe taken in de stand geaccepteerde kunnen hangen en kunnen niet worden gestart.

**Risicobeperking:**

Gebruik de volgende procedure om de tijdelijke oplossing het probleem:

1. SSH in headnode. 
2. Voer de volgende opdracht om te vinden van de toepassings-id's van de interactieve taken gestart via hier. 

        yarn application –list

    De standaard-taak namen opgegeven hier worden als de taken zijn gestart met een interactieve sessie hier zonder expliciete namen, voor het hier sessie die is gestart door Jupyter notitieblok, de taaknaam van de wordt gestart met remotesparkmagics_ *. 

3. Voer de volgende opdracht om deze taken af te sluiten. 

        yarn application –kill <Application ID>

Nieuwe taken wordt gestart. 

##<a name="spark-history-server-not-started"></a>Een geschiedenis Server niet gestart 

Een geschiedenis Server wordt niet automatisch gestart nadat een cluster is gemaakt.  

**Risicobeperking:** 

De geschiedenis-server handmatig starten vanuit Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Machtiging probleem in een logboekdirectory 

Wanneer hdiuser een project met spark-submit indient, er is een fout java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (toestemming geweigerd) en het Stuurprogrammalogboek niet is geschreven. 

**Risicobeperking:**
 
1. Hdiuser toevoegen aan de Hadoop-groep. 
2. Geef 777 machtigingen op /var/log/spark na het maken van het cluster. 
3. De locatie van het elektrische-logboek met Ambari naar een map met 777 machtigingen bijwerken.  
4. Elektrische-submit uitvoeren als sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Met betrekking tot Jupyter notitieblokken

Hier volgen enkele bekende problemen die betrekking hebben op Jupyter notitieblokken.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Notitieblokken met niet-ASCII-tekens in bestandsnamen

Jupyter-notitieblokken die kunnen worden gebruikt in een HDInsight clusters mogen geen niet-ASCII-tekens in bestandsnamen. Als u probeert te uploaden van een bestand via de Jupyter UI een niet-ASCII-bestandsnaam heeft, deze stilte, mislukt (dat wil zeggen Jupyter kunt u het bestand uploaden, maar deze won't ofwel een zichtbare fout genereren). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fout tijdens het laden van notitieblokken van grotere

Ziet u mogelijk een fout **`Error loading notebook`** wanneer u de notitieblokken die groter zijn laden.  

**Risicobeperking:**

Als u deze fout wordt weergegeven, betekent niet dat uw gegevens is beschadigd of verloren.  Uw notitieblokken worden nog steeds op de schijf in `/var/lib/jupyter`, en u kunt SSH in het cluster deze te openen. U kunt uw notitieblokken uit uw cluster kopiëren naar uw lokale computer (via SCP of WinSCP) als een back-up om te voorkomen dat het verlies van alle belangrijke gegevens in het notitieblok. U kunt vervolgens SSH tunnel in uw headnode bij poort 8001 voor toegang tot Jupyter zonder tussenkomst van de gateway.  Hierin kunt u de uitvoer van uw notitieblok wissen en opnieuw opslaan om de grootte van het notitieblok te beperken.

Als u wilt voorkomen dat deze fout in de toekomst, moet u enkele aanbevolen procedures volgen:

* Het is belangrijk dat de grootte van het notitieblok kleine. Een uitvoer van uw taken elektrische die wordt verzonden naar een Jupyter blijft behouden in het notitieblok.  Dit is een goede gewoonte met Jupyter in het algemeen aan kunt u voorkomen dat `.collect()` aan grote RDD of dataframes; in plaats daarvan kunt uitvoeren als u bekijken van de inhoud van een RDD wilt, `.take()` of `.sample()` zodat uw uitvoer niet te groot.
* Ook als u een notitieblok opslaat, uitvoer Schakel het selectievakje alle cellen om de grootte te beperken.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Notitieblok opstarten duurt langer dan verwacht 

Eerste code-instructie in Jupyter notitieblok met een bijzonder kan meer dan een paar minuten duren.  

**Toelichting op:**
 
Dit gebeurt omdat wanneer de eerste cel van de code wordt uitgevoerd. Op de achtergrond dit Start sessie configuratie en een, SQL en component contexten worden ingesteld. Nadat deze contexten zijn ingesteld, de eerste instructie wordt uitgevoerd en dit de indruk dat geeft de instructie hebt gemaakt, een veel tijd in beslag.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter notitieblok time-out bij het maken van de sessie

Wanneer een cluster afmelden bij resources, wordt de elektrische en Pyspark kernels in het notitieblok Jupyter time-out voor het maken van de sessie. 

**Beperkingen:** 

1. Enkele bronnen in uw cluster elektrische door vrijgemaakt:

    - Andere notitieblokken elektrische stoppen door te gaan naar het menu sluiten en stoppen of afsluiten te klikken op in de Verkenner notitieblok.
    - Andere toepassingen elektrische van GAREN stoppen.

2. Start opnieuw op het notitieblok dat u probeerde te starten. Voldoende beschikbaar moet zijn voor u nu een sessie maken.

##<a name="see-also"></a>Zie ook

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

* [Jupyter installeert op uw computer en verbinding maken met een cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren

* [Bronnen voor de Apache elektrische cluster in Azure HDInsight beheren](hdinsight-apache-spark-resource-manager.md)

* [Bijhouden en foutopsporing taken op een cluster Apache elektrische in HDInsight](hdinsight-apache-spark-job-debugging.md)
