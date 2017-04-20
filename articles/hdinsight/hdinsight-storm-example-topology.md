<properties
 pageTitle="Voorbeeld Apache Storm topologieën op HDInsight | Microsoft Azure"
 description="Een lijst met voorbeeld Storm topologieën gemaakt en getest met Apache Storm op HDInsight inclusief eenvoudige C# en Java topologieën en werken met Hubs gebeurtenis."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Voorbeeld Storm toplogies en de onderdelen voor Apache Storm op HDInsight

Hier volgt een lijst met voorbeelden gemaakt en beheerd door Microsoft voor gebruik met Apache Storm op HDInsight. In deze voorbeelden duidelijk tal van onderwerpen maken van eenvoudige C# en Java topologieën tot het werken met Azure services zoals gebeurtenis Hubs, DocumentDB, Power BI, SQL-Database, HBase op HDInsight en Azure Storage. Enkele voorbeelden wordt ook het werken met niet-Azure of zelfs niet-Microsoft technologieën, zoals SignalR en Socket.IO

| Beschrijving                                                                                             | Laat zien                                         | Taal/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Vanaf Apache Storm naar Azure Lake gegevensopslag schrijft](hdinsight-storm-write-data-lake-store.md) | Schrijven naar Azure Lake gegevensopslag | Java |
| [Bron van gebeurtenis Hub Spout en bout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Bron voor de gebeurtenis Hub Spout en bout | Java |
| [Ontwikkel Java gebaseerde topologieën voor Apache Storm op HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Ontwikkel C# topologieën voor Apache Storm op HDInsight gebruik van Visual Studio][16fce2d1]                     | HDInsight Tools voor Visual Studio                    | C#, Java                   |
| [Meerdere gegevensstromen maken in een topologie C# Storm][ec5a4064]                                         | Meerdere streams                                     | C#                         |
| [Twitter trending onderwerpen met Storm op HDInsight bepalen][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Procesgebeurtenissen van Azure gebeurtenis Hubs met Storm op HDInsight (C#)][844d1d81]                                | Gebeurtenis Hubs                                           | C# en Java                |
| [Procesgebeurtenissen van Azure gebeurtenis Hubs met Storm op HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Gebeurtenis Hubs | Java |
| [Power Bi gebruiken voor het visualiseren van gegevens uit een topologie Storm][94d15238]                              | Power BI                                             | C#                         |
| [Sensorgegevens met Storm en HBase in HDInsight analyseren][ab894747]                                     | Gebeurtenis Hubs, HBase, Socket.IO, webdashboard          | C#, Java, JavaScript, HTML |
| [Voertuig sensorgegevens van gebeurtenis Hubs met Storm op HDInsight verwerken][246ee964]                        | Gebeurtenis Hubs, DocumentDb, opslag Azure Blob (WASB)    | C#, Java                   |
| [Extraheren, transformeren en laden (ETL) van Azure gebeurtenis Hubs naar HBase, Storm op HDInsight gebruiken][b4b68194] | Gebeurtenis Hubs, HBase                                    | C#                         |
| [Sjabloon C# Storm topologie project voor het werken met Azure services van Storm op HDInsight][ce0c02a2]  | Gebeurtenis Hubs, DocumentDb, SQL-Database, HBase, SignalR | C#, Java                   |
| [Schaalbaarheid benchmarks voor het lezen van Azure gebeurtenis Hubs Storm op HDInsight gebruiken][d6c540e3]           | Bericht doorvoer, gebeurtenis Hubs, SQL-Database         | C#, Java                   |
| [Gebeurtenissen met Storm en HBase op HDInsight relateren](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Python gebruiken met Storm op HDInsight](hdinsight-storm-develop-python-topology.md) | Python onderdelen met Java en Clojure op basis van Storm topologieën | Python |

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Apache Storm op HDInsight][2b8c3488]

* [Meer informatie over het gebruiken en beheren van Storm topologieën met Storm op HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Informatie over het maken van een Storm op HDInsight cluster met behulp van het Dashboard Storm en het implementeren van voorbeeld topologieën."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Informatie over het implementeren en te beheren met behulp van het Dashboard Storm op het web en Storm UI of de HDInsight-hulpmiddelen voor Visual Studio topologieën."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Informatie over het maken van C# Storm topologieën met behulp van de hulpmiddelen HDInsight voor Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Informatie over het maken van Storm topologieën in Java, met behulp van Maven, door te maken van de topologie van een eenvoudige wordcount."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Laat zien hoe u gegevens naar Power BI uit een C#-topologie schrijven en maken van een grafiek en het dashboard van de gegevens."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Ziet u een eenvoudige Storm topologie waarmee een wordcount, geïmplementeerd in C#. Dit toont ook aan het maken van meerdere gegevensstromen binnen een C#-topologie."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Leer hoe u gegevens lezen en schrijven van Azure gebeurtenis Hubs met Storm op HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Informatie over het gebruik van Apache Storm op HDInsight voor het verwerken van sensorgegevens van Azure gebeurtenis Hubs visualiseren met D3.js, en sla deze (optioneel), naar HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Leer hoe u Trident gebruiken om te maken van de topologie van een Storm die bepaalt trending onderwerpen (gebaseerd op hashtags,) op Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Documenten van Azure DocumentDB voor gegevens verwijst naar informatie over het gebruik van de topologie van een Storm om berichten te lezen van Azure gebeurtenis Hubs, lezen en opslaan van gegevens met Azure Storage."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Verschillende topologieën om te laten zien doorvoer bij het lezen van Azure gebeurtenis Hubs en opslaan met SQL-Database met Apache Storm op HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informatie over het lezen van gegevens van Azure gebeurtenis Hubs, statistische en transformeren van de gegevens en vervolgens opslaan naar HBase op HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dit project bevat sjablonen voor spouts, bouten en topologieën om te communiceren met verschillende Azure services zoals gebeurtenis Hubs, DocumentDB en SQL-Database."
 
