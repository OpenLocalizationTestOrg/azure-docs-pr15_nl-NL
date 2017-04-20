 <properties
    pageTitle="Inleiding tot Hadoop - wat is Hadoop op HDInsight? | Microsoft Azure"
    description="Bekijk de inleiding voor Hadoop, verdeelde groot gegevensverwerking en analyse en de onderdelen van het Hadoop-systeem in de cloud op HDInsight."
    keywords="groot gegevensanalyse, inleiding tot hadoop, wat is hadoop, hadoop technologie stapel, hadoop-systeem"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Een inleiding tot het Hadoop-systeem op Azure HDInsight

 Dit artikel bevat een inleiding tot Hadoop op Azure HDInsight, de-systeem en groot gegevens. Meer informatie over de onderdelen Hadoop, algemene terminologie en scenario's voor gegevensanalyse groot.

## <a name="what-is-hadoop-on-hdinsight"></a>Wat is Hadoop op HDInsight?

Hadoop verwijst naar een selectie aan open source-software die is een kader voor verdeelde behandeld, opgeslagen en analyse van grote gegevenssets op clusters van computers. Azure HDInsight de Hadoop-onderdelen van de verdeling **Hortonworks gegevens Platform (HDP)** beschikbaar in de cloud, en wordt geïmplementeerd en beheerde clusters bepalingen met hoge betrouwbaarheid en beschikbaarheid.  

Apache Hadoop is de oorspronkelijke open source project om dit te groot gegevensverwerking. Volgende is de ontwikkeling van verwante software en hulpprogramma's beschouwd als onderdeel van de stapel Hadoop technologie, inclusief Apache component, Apache HBase Apache elektrische en dergelijke. Zie [overzicht van het Hadoop-systeem in HDInsight](#overview) voor meer informatie.

## <a name="what-is-big-data"></a>Wat zijn groot gegevens?

Grote gegevens worden eventuele grote hoofdtekst van digitale informatie uit de tekst in een Twitter feed, aan de gegevens sensor uit industriële apparatuur naar informatie over de klant bladeren en aankopen op een website. Grote gegevens worden historische (wat betekent opgeslagen gegevens) of realtime (wat betekent streamen rechtstreeks vanuit de bron). Grote gegevens worden verzameld in ooit escaleren hoeveelheden, bij steeds sneller luchtsnelheden, en in een gegevensniveaus uitvouwen diverse indelingen.

Voor grote gegevens op te geven sneller intelligence of inzicht, moet u relevante gegevens verzamelen en stel de juiste vragen. U moet er ook voor zorgen dat de gegevens zijn toegankelijk, opgeschoond, geanalyseerd en klik vervolgens op een handige manier wordt gepresenteerd. Dit is waar groot gegevensanalyse op Hadoop in HDInsight kan helpen.

## <a name="overview"></a>Overzicht van het Hadoop-systeem in HDInsight

HDInsight is de cloud-verdeling op Microsoft Azure van de snel groeiende Apache Hadoop-technologie stapel voor groot gegevensanalyse. Het bevat implementaties van Apache elektrische, HBase Storm, varken, component, Sqoop, Oozie, Ambari en dergelijke. HDInsight kan ook worden geïntegreerd met business intelligence (BI) hulpprogramma's zoals Power BI, Excel, SQL Server Analysis Services en SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase elektrische, Storm en aangepaste clusters

HDInsight biedt clusterconfiguraties voor Apache Hadoop, een, HBase of Storm. Of, kunt u [clusters met scriptacties aanpassen](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: [HDFS](#hdfs)en een eenvoudige [MapReduce](#mapreduce) programming model te verwerken en te analyseren gegevens parallel biedt betrouwbare gegevensopslag.

* **<a target="_blank" href="http://spark.apache.org/">Apache elektrische</a>**: een parallelle verwerking-kader die ondersteuning biedt voor de verwerking in het geheugen om zo de prestaties van groot-gegevensanalyse-toepassingen, dus werkt voor SQL, gegevens, streaming en machine learning. Zie [Overzicht: Wat is een Apache in HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: een database NoSQL is gebouwd op Hadoop met RAM en sterke consistentie voor grote hoeveelheden ongestructureerde en gedeeltelijk gestructureerde gegevens - potentieel miljard van rijen tijden miljoenen kolommen. Zie [overzicht van HBase op HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: een systeem gedistribueerde, realtime berekening voor het verwerken van grote streams gegevens snel. Storm wordt aangeboden als een beheerde cluster in HDInsight. Zie [realtime sensorgegevens analyseren met Storm en Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Aanpassing voorbeeldscripts

Scriptacties zijn scripts die uitvoeren tijdens het inrichten van cluster en kunnen worden gebruikt om extra onderdelen op het cluster installeren. Voor Linux gebaseerde kolomgroepen zijn we vaker doen scripts.

Het volgende voorbeeldscripts worden verstrekt door het team HDInsight:

* [Tint](hdinsight-hadoop-hue-linux.md): een set webtoepassingen gebruikt om te communiceren met een cluster. Linux clusters.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Graph processing naar model relaties tussen dingen of personen.

* [R](hdinsight-hadoop-r-scripts-linux.md): een open source taal- en -omgeving voor statistische computing in machine learning gebruikt.

* [Solr](hdinsight-hadoop-solr-install-linux.md): een platform voor het zoeken van enterprise-schaal waarmee de zoekfunctie van gegevens.

Zie voor informatie over het ontwikkelen van uw eigen Script-acties, [scriptactie ontwikkeling met HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Wat zijn de Hadoop-onderdelen en hulpprogramma's?

De volgende onderdelen en hulpprogramma's worden opgenomen op HDInsight clusters.

* **[Ambari](#ambari)**: Cluster inrichting, management bewaken en hulpprogramma's.

* **[Avro](#avro)** (Microsoft .NET-bibliotheek voor Avro): serialisatie van gegevens voor de Microsoft .NET-omgeving.

* **[Component & HCatalog](#hive)**: taal SQL (Structured Query)-zoals query's uitvoeren en een tabel en opslag management laag.

* **[Mahout](#mahout)**: voor scalable machine learning toepassingen.

* **[MapReduce](#mapreduce)**: verouderd framework voor Hadoop distributed verwerking-en resourcebeheer. Zie [garens](#yarn), het kader van de volgende generatie resource.

* **[Oozie](#oozie)**: werkstroombeheer.

* **[Phoenix](#phoenix)**: relationele databaselaag via HBase.

* **[Varken](#pig)**: eenvoudiger uitvoeren van scripts voor MapReduce transformaties.

* **[Sqoop](#sqoop)**: gegevens importeren en exporteren.

* **[Tez](#tez)**: veel gegevens-processen worden efficiënt uitgevoerd bij het op schaal kunt.

* **[Garens](#yarn)**: deel uit van de Hadoop core bibliotheek en de volgende generatie van het kader van de software MapReduce.

* **[ZooKeeper](#zookeeper)**: afhankelijk van processen in gedistribueerde systemen.

> [AZURE.NOTE] Zie voor informatie over de specifieke onderdelen en versie-informatie [Hadoop-onderdelen, versiebeheer, en -services in HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari is voor het inrichten, beheren en Apache Hadoop clusters cmdlets voor controle. Het bevat een intuïtieve verzameling hulpprogramma's voor operator en een reeks robuuste API's die de complexiteit van Hadoop, de werking van clusters vereenvoudigen verbergen. Linux gebaseerde HDInsight clusters verlenen de Ambari web gebruikersinterface en de Ambari REST API, terwijl Windows gebaseerde clusters bieden een subset van de REST API. Ambari weergaven op HDInsight clusters kunt invoegtoepassing UI mogelijkheden.

Zie [beheren HDInsight clusters met Ambari](hdinsight-hadoop-manage-ambari.md) (alleen Linux), [Monitor Hadoop clusters in HDInsight de Ambari-API gebruiken](hdinsight-monitor-use-ambari-api.md)en <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API verwijzing</a>.

### <a name="avro"></a>Avro (Microsoft .NET-bibliotheek voor Avro)

De Microsoft .NET-bibliotheek voor Avro implementeert de Apache Avro compacte binaire gegevens interchange-indeling voor serialisatie voor de Microsoft .NET-omgeving. <a target="_blank" href="http://www.json.org/">De notatie voor JavaScript-Object (JSON)</a> wordt gebruikt om een schema taal-agnostic die taal interoperability schadequoten te definiëren, zin gegevens in één taal serienummer kunnen worden gelezen in een andere. Gedetailleerde informatie over de notatie vindt u in de < een doel = _ 'lege' href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specificatie</a>.
De indeling van Avro-bestanden ondersteunt het verdeelde MapReduce programming model. Bestanden, zijn 'splittable', wat betekent dat u kunt een willekeurige plaats in een bestand te zoeken en lezen starten vanuit een bepaald tekstblok. Zie meer informatie over, [geserialiseerd gegevens met de Microsoft .NET-bibliotheek voor Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hadoop Distributed bestand System (HDFS) is een gedistribueerd bestandssysteem die met MapReduce en garens, het belangrijk onderdeel van het Hadoop-systeem is. HDFS is het standaard bestandssysteem voor Hadoop clusters op HDInsight.

### <a name="hive"></a>Component & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache component</a> is data warehouse software gebouwd op Hadoop waarmee u kunt de query en grote gegevenssets in verdeelde opslag beheren met behulp van een SQL-achtige taal HiveQL genoemd. Component, zoals varken, is een abstractie boven aan de MapReduce. Wanneer wordt uitgevoerd, zet component om query's in een reeks MapReduce taken. Component conceptueel dichter bij een relationele database management systeem dan varken is en daarom geschikt is voor gebruik met meer gestructureerde gegevens. Varken is voor ongestructureerde gegevens, de betere keuze. Zie [component met Hadoop in HDInsight gebruiken](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> is een tabel en opslag management laag voor Hadoop waarin gebruikers met een relationele weergave van gegevens. In HCatalog, kunt u lezen en schrijven van bestanden in een willekeurige waarvoor een SerDe component (serialisatiefunctie-deserializer) kunnen worden geschreven.

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> is een scalable machine learning-algoritmen die worden uitgevoerd op Hadoop-bibliotheek. Beginselen van statistieken gebruikt, Leer machine learning-toepassingen systemen voor meer informatie over van gegevens en gebruik eerdere resultaten om te bepalen toekomstige gedrag. Zie [genereren film aanbevelingen Mahout op Hadoop gebruiken](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
Het kader oudere software voor Hadoop is MapReduce voor het schrijven van toepassingen naar de grote gegevenssets proces parallel batch. Een taak MapReduce wordt gesplitst grote gegevenssets en de gegevens zijn gerangschikt in sleutel-waardeparen voor verwerking.

[Garens](#yarn) is het kader Hadoop allernieuwste resource manager en -toepassing en MapReduce 2.0 genoemd. MapReduce taken worden uitgevoerd op garens.

Zie voor meer informatie over MapReduce, <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> in de Hadoop-Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> is een werkstroom afhankelijk-systeem die worden beheerd in Hadoop-taken. Er is geïntegreerd met de stapel Hadoop en Hadoop taken voor MapReduce, varken, component en Sqoop ondersteunt. Dit kan ook worden gebruikt om taken die specifiek zijn voor een systeem, zoals Java-programma's of shell-scripts te plannen. Zie [Gebruik Oozie met Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
Een relationele databaselaag is <a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> verstreken HBase. Phoenix bevat een JDBC-stuurprogramma waarmee gebruikers opvragen en SQL-tabellen rechtstreeks beheren. Phoenix equivalent query's en andere instructies in systeemeigen NoSQL API-oproepen - in plaats van het gebruik van MapReduce - zodat sneller toepassingen boven aan de NoSQL winkels. Zie [Gebruik Apache Phoenix en eekhoorn met HBase clusters](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Varken
<a  target="_blank" href="http://pig.apache.org/">Apache varken</a> is een hoog niveau platform waarmee u complexe MapReduce transformaties op zeer grote gegevenssets uitvoeren met behulp van een eenvoudige scripttaal varken Latijns genoemd. Varken equivalent de varken Latijns-scripts, zodat deze wordt uitgevoerd binnen Hadoop. U kunt door de gebruiker gedefinieerde functies (UDF) om uit te breiden varken Latijns maken. Zie [Gebruik varken met Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> is hulpprogramma dat overdrachten bulksgewijs gegevens tussen Hadoop en relationele databases, zoals een SQL of andere winkels gestructureerde gegevens zo efficiënt mogelijk. Zie [Gebruik Sqoop met Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> is een toepassing framework gebouwd op Hadoop garens die wordt uitgevoerd complex, acyclische grafieken van de algemene gegevensverwerking. Dit is een meer flexibele en krachtige opvolger het MapReduce framework waarmee processen veel gegevens, zoals component, efficiënter uitvoeren bij het op schaal. Zie ['Apache Tez gebruiken voor betere prestaties', in gebruik component en HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>GARENS
Apache garens is de volgende generatie van MapReduce (MapReduce 2.0 of MRv2) en scenario's voor gegevensverwerking voorbij MapReduce batch verwerken met grotere schaalbaarheid en realtime verwerking ondersteunt. GARENS biedt resourcebeheer en een kader gedistribueerde toepassing. MapReduce taken worden uitgevoerd op garens.

Zie voor meer informatie over garens, <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (garens)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> coördinaten processen in grote gedistribueerde systemen met behulp van een gedeelde hiërarchische naamruimte van gegevens registreert (znodes). Znodes bevatten kleine hoeveelheden metagegevens die nodig zijn om te coördineren processen: status, locatie, configuratie, enzovoort.

## <a name="programming-languages-on-hdinsight"></a>De talen hoeft te programmeren op HDInsight

HDInsight clusters--Hadoop, HBase, Storm en een clusters--een aantal programming talen ondersteund, maar sommige al dan niet standaard worden niet geïnstalleerd. Voor bibliotheken, modules of -pakketten niet standaard geïnstalleerd, gebruikt u de scriptactie voor een om het onderdeel te installeren. Zie [Script actie development met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Standaard taal support programmeren

Standaard clusters HDInsight ondersteuning:

* Java

* Python

Extra talen kunnen worden geïnstalleerd scriptacties gebruiken: [Script actie ontwikkeling met HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java VM (JVM) talen

Veel talen dan Java kunnen worden uitgevoerd met een Java virtuele machine JVM (); echter kan uitgevoerd enkele van deze talen vragen om extra onderdelen op het cluster is geïnstalleerd.

Deze JVM gebaseerde talen worden ondersteund in HDInsight clusters:

* Clojure

* Jython (Python voor Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifieke talen

HDInsight clusters ondersteunen de volgende talen die specifiek voor het Hadoop-systeem zijn:

* Varken Latijns voor varken taken

* HiveQL voor component functies en SparkSQL


## <a name="advantage"></a>Voordelen van Hadoop in de cloud

Als onderdeel van de Azure cloud-systeem biedt Hadoop in HDInsight een aantal voordelen, onderling:

* Automatisch inrichten van Hadoop clusters. HDInsight clusters zijn veel eenvoudiger te maken dan het handmatig configureren Hadoop clusters. Zie voor meer informatie [inrichten Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* De staat-van-het-art Hadoop-onderdelen. Zie voor meer informatie, [Hadoop-onderdelen, versiebeheer, en -services in HDInsight][component-versioning].

* Beschikbaarheid en betrouwbaarheid van clusters. Zie [beschikbaarheid en betrouwbaarheid van Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) voor meer informatie.

* Efficiënt en min kosten gegevensopslag met Azure-blobopslag, een optie voor Hadoop-compatibele. Zie [Azure-blobopslag gebruiken met Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) voor meer informatie.

* Integratie met andere Azure services, met inbegrip van [WebApps](https://azure.microsoft.com/documentation/services/app-service/web/) en [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/).

* Aanvullende VM maten en typen voor het uitvoeren van HDInsight clusters. Zie [Hadoop-onderdelen, versiebeheer, en -services in HDInsight] [ component-versioning] voor meer informatie.

* Cluster schaalbaarheid. Cluster schaalbaarheid, kunt u het aantal knooppunten van een lopende HDInsight cluster wijzigen zonder dat u moet verwijderen of opnieuw maken.

* Virtuele netwerkondersteuning. HDInsight clusters kunnen worden gebruikt met Azure Virtual Network ter ondersteuning van moeten worden geïsoleerd van cloud resources of hybride-scenario's die zijn gekoppeld cloud resources met die in uw datacenter.

* Lage vermelding kosten. Start een [gratis proefversie](https://azure.microsoft.com/free/)of Raadpleeg [prijsgegevens HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Meer informatie over de voordelen op Hadoop in HDInsight, raadpleegt u de [pagina Azure onderdelen voor HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard en HDInsight Premium

HDInsight biedt groot gegevens cloud aanbiedingen in twee categorieën, Standard en Premium. HDInsight Standard biedt een enterprise-schaal cluster die kunnen worden gebruikt om uit te voeren hun werkbelasting groot gegevens. HDInsight Premium is gebaseerd op die en biedt geavanceerde analytische en beveiligingsmogelijkheden voor een cluster HDInsight. Voor meer informatie raadpleegt u [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Bronnen voor meer informatie over grote-gegevensanalyse, Hadoop en HDInsight

Deze inleiding Hadoop in de cloud en groot gegevensanalyse met de onderstaande resources combineren.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop-documentatie voor HDInsight

* [HDInsight documentatie](https://azure.microsoft.com/documentation/services/hdinsight/): de documentatiepagina voor Hadoop op Azure HDInsight met koppelingen naar artikelen, video's en meer informatie.

* [Aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): een beknopte begin-zelfstudie voor HDInsight Hadoop clusters inrichting en steekproef component query's uitvoeren.

* [Aan de slag met een in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): een beknopte begin zelfstudie voor het maken van een cluster elektrische en interactieve een SQL-query's uitvoeren.

* [Gebruik R Server op HDInsight](hdinsight-hadoop-r-server-get-started.md): aan de slag met R-Server in HDInsight Premium.

* [Inrichten HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md): meer informatie over het inrichten van een cluster HDInsight Hadoop via de portal van Azure, Azure CLI of Azure PowerShell.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: meer informatie over de bibliotheek met Apache Hadoop-software, een kader waarmee de gedistribueerde verwerking van grote gegevenssets over clusters van computers.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: meer informatie over de architectuur en het ontwerp van de Hadoop Distributed File System, het primaire opslag-systeem gebruikt door Hadoop-toepassingen.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce zelfstudie</a>: meer informatie over het programmeren framework voor het schrijven van Hadoop-toepassingen die snel grote hoeveelheden gegevens parallel in grote clusters van berekeningscluster knooppunten verwerken.


### <a name="microsoft-business-intelligence"></a>Bedrijfsinformatie van Microsoft

Vertrouwde business intelligence (BI) extra - zoals Excel, PowerPivot, SQL Server Analysis Services en SQL Server Reporting Services - ophalen, analyseren en te rapporteren van gegevens die zijn geïntegreerd met HDInsight via de invoegtoepassing Power Query of het Microsoft Component ODBC-stuurprogramma.

Deze BI-functies kunt in de groot-gegevensanalyse:

* [Excel verbinden met een Hadoop met Power Query](hdinsight-connect-excel-power-query.md): Leer hoe u Excel verbinden met de opslag van Azure-account waarin de gegevens die zijn gekoppeld aan uw cluster HDInsight met behulp van Microsoft Power Query voor Excel. Windows-workstation vereist. Als u werkt met Windows - of Linux-gebaseerde cluster.

* [Excel verbinden met een Hadoop met het Microsoft Component ODBC-stuurprogramma](hdinsight-connect-excel-hive-odbc-driver.md): meer informatie over het importeren van gegevens uit HDInsight met het Microsoft Component ODBC-stuurprogramma. Windows-workstation vereist. Als u werkt met Windows - of Linux-gebaseerde cluster.

* [Microsoft Cloud-Platform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): meer informatie over Power BI voor Office 365, downloaden van de evaluatieversie van SQL Server en SharePoint Server 2013 en SQL Server BI instellen.

* [SQL Server analyseservices](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
