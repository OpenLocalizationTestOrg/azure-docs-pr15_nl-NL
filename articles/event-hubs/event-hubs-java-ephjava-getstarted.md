<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in Java | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; gebeurtenissen met Java verzenden en ontvangen van deze bij het gebruik van de EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer scalable opname-systeem dat opname miljoenen kunt gebeurtenissen per seconde, zodat een toepassing te verwerken en de grote hoeveelheden gegevens analyseren geproduceerd door uw verbonden apparaten en toepassingen. Zodra die worden verzameld in gebeurtenis Hubs, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslag cluster.

Zie [overzicht van de gebeurtenis Hubs][]voor meer informatie.

Deze zelfstudie wordt getoond hoe voor het nemen van berichten op een gebeurtenis Hub met gebruikmaking van een consoletoepassing in Java en deze weer ophalen in parallelle met de bibliotheek Java gebeurtenis Processor Host.

Wilt u deze zelfstudie hebt voltooid, moet u het volgende:

+ Een Java-ontwikkelomgeving. Voor deze zelfstudie wordt ervan uitgegaan [Eclips](https://www.eclipse.org/).

+ Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure gratis proefversie</a>voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  Het project **ontvanger** worden uitgevoerd.

    ![][21]

2.  Voer het project **afzender** .

    ![][22]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werken-toepassing die wordt gemaakt van een gebeurtenis Hub en gegevens verzonden en ontvangen hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.

Zie het [Java Developer Center](/develop/java/)voor meer informatie.

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 