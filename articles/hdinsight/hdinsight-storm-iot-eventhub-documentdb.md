<properties
 pageTitle="Proces sensorgegevens voertuig met Apache Storm op HDInsight | Microsoft Azure"
 description="Leer hoe u het proces voertuig sensorgegevens van gebeurtenis Hubs met Apache Storm op HDInsight. Voeg modelgegevens uit DocumentDB en resultaat met storage opslaan."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Voertuig sensorgegevens van Azure gebeurtenis Hubs met Apache Storm op HDInsight verwerken

Leer hoe u het proces voertuig sensorgegevens van Azure gebeurtenis Hubs met Apache Storm op HDInsight. In dit voorbeeld leest sensorgegevens van Azure gebeurtenis Hubs, de gegevens door te verwijzen naar gegevens die zijn opgeslagen in Azure DocumentDB cursussen en ten slotte opslag van de gegevens op te slaan van Azure met de Hadoop-bestand System (HDFS).

![HDInsight en het diagram van de architectuur Internet van dingen (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Overzicht

Sensoren toevoegen aan voertuigen, kunt u postvakken van apparatuur problemen op basis van historische gegevenstrends voorspellen, evenals verbeteren voor toekomstige versies op basis van gebruiksanalyse patroon. Terwijl traditionele MapReduce batchbestand kan worden gebruikt voor deze analyse, moet u mogelijk zijn snel en efficiënt de gegevens van alle voertuigen in laden Hadoop voordat MapReduce verwerking kan zich voordoen. U kunt ook analyse van kritieke fout paden (engine temperatuur, remmen, enzovoort) in realtime uitvoeren.

Azure gebeurtenis Hubs zijn gemaakt voor het verwerken van de enorme hoeveelheid gegevens die zijn gegenereerd door sensoren en Apache Storm op HDInsight laden en de gegevens van proces voordat u ze opslaat in HDFS (ondersteund door Azure Storage) kunnen worden gebruikt voor extra MapReduce verwerking.

##<a name="solution"></a>Oplossing

Telemetriegegevens voor engine temperatuur, temperatuur en voertuigsnelheid is vastgelegd door sensoren, klikt u vervolgens naar gebeurtenis Hubs samen met de auto voertuig identificatie getal (VIN) en een tijdstempel worden verzonden. Hierin de topologie van een Storm een Storm Apache op HDInsight cluster waarop de gegevens leest, verwerkt, en opgeslagen in HDFS.

Tijdens het verwerken, wordt het Chassisnummer modelgegevens ophalen uit Azure DocumentDB gebruikt. Dit is toegevoegd aan de gegevensstroom voordat deze is opgeslagen.

De onderdelen die worden gebruikt in de topologie Storm zijn:

* **EventHubSpout** - leest gegevens van Azure gebeurtenis Hubs

* **TypeConversionBolt** - converteert de JSON-tekenreeks van gebeurtenis Hubs naar een tupel met de afzonderlijke gegevenswaarden voor engine temperatuur, temperatuur, snelheid, Chassisnummer en tijdstempel

* **DataReferencBolt** - zoekt naar bepaalde het voertuigmodel van DocumentDB met het Chassisnummer

* **WasbStoreBolt** - slaat de gegevens naar HDFS (Azure opslag)

Hieronder ziet u een diagram van deze oplossing.

![topologie van storm](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Dit is een eenvoudigere diagram en elk onderdeel in de oplossing kan meerdere exemplaren hebben. Bijvoorbeeld zijn de meerdere exemplaren van elk onderdeel in de topologie verdeeld over de knooppunten in de Storm op HDInsight cluster.

##<a name="implementation"></a>Implementatie

Een voltooid, wordt automatisch oplossing voor dit scenario is beschikbaar als onderdeel van de bibliotheek [HDInsight-Storm-voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples) op GitHub. Als u wilt gebruiken in dit voorbeeld, de stappen in de [IoTExample README uit te voeren. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeeld Storm topologieën, [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md).
