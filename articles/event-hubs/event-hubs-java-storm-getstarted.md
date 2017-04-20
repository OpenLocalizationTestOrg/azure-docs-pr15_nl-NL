<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in Java met Apache Storm | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; gebeurtenissen met Java verzenden en ontvangen van deze in een cluster Apache Storm."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer scalable opname-systeem dat opname miljoenen kunt gebeurtenissen per seconde, zodat een toepassing te verwerken en de grote hoeveelheden gegevens analyseren geproduceerd door uw verbonden apparaten en toepassingen. Zodra die worden verzameld in gebeurtenis Hubs, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslag cluster.

Zie [Overzicht van de Hubs gebeurtenissen][]voor meer informatie.

Deze zelfstudie wordt beschreven hoe voor het verzamelen van berichten op een gebeurtenis Hub met gebruikmaking van een consoletoepassing in Java, en deze weer ophalen parallel Apache Storm gebruiken.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Een Java-ontwikkelomgeving is geconfigureerd voor het uitvoeren van [Maven](http://maven.apache.org/). Voor deze zelfstudie nemen we [Eclips](https://www.eclipse.org/)aan.

+ Een actieve Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  De klasse **LogTopology** vanuit Eclips uitvoeren en klik vervolgens wachten om door deze naar het begin van de ontvangers voor alle partities.

2.  Het project **afzender** uitvoeren, drukt u op **Enter** in het consolevenster en raadpleegt u de gebeurtenissen die worden weergegeven in het venster van de ontvanger.

    ![][22]

> [AZURE.NOTE] In deze zelfstudie alleen door Storm in de lokale modus voor ontwikkelingsdoeleinden te gebruiken. Zie de [HDInsight Storm overzicht][] en de officiële [Apache Storm][] documentatie voor meer informatie van Storm implementaties en patronen.

## <a name="next-steps"></a>Volgende stappen

De volgende bronnen zijn beschikbaar voor het ontwikkelen van toepassingen integreren gebeurtenis Hubs en Storm.

- [Sensorgegevens met Storm en HDInsight analyseren] , is een volledige scenario zelfstudie gebeurtenis Hubs, Storm en HBase gebruiken om op te nemen sensorgegevens in een Hadoop-cluster.
- [Ontwikkelen streaming gegevensverwerking toepassingen met SCP.NET en C# op Storm en HDInsight][] is een zelfstudie over het schrijven van Storm pijpleidingen met C#.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[Overzicht van de HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[Sensorgegevens met Storm en HDInsight analyseren]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Ontwikkel streaming gegevensverwerking toepassingen met SCP.NET en C# op Storm en HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 