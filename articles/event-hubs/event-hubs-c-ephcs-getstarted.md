<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in C en C# | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; gebeurtenissen in C verzenden en ontvangen van hem in C# met behulp van de EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer scalable opname-systeem dat opname miljoenen kunt gebeurtenissen per seconde, zodat een toepassing te verwerken en de grote hoeveelheden gegevens analyseren geproduceerd door uw verbonden apparaten en toepassingen. Zodra die worden verzameld in gebeurtenis Hubs, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslag cluster.

Zie [Overzicht van de Hubs gebeurtenissen][]voor meer informatie.

In deze zelfstudie leert u hoe u voor het nemen van berichten op een gebeurtenis Hub met een consoletoepassing in C en deze weer ophalen in parallelle met de bibliotheek C# [Gebeurtenis Processor Host][] .

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Een ontwikkelomgeving C. Voor deze zelfstudie wordt ervan uitgegaan de stapel gcc op een [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) met Ubuntu 14.04. Instructies voor andere omgevingen krijgt in externe koppelingen.

+ Microsoft Visual Studio Express voor Windows

+ Een actieve Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  Het project uit van de **ontvanger** in Visual Studio uitvoeren en klik vervolgens wachten om door deze naar het begin van de ontvangers voor alle partities.

    ![][21]

2.  De toepassing van de **afzender** wordt uitgevoerd en Zie de gebeurtenissen die worden weergegeven in het venster van de ontvanger.

    ![][24]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werken-toepassing die wordt gemaakt van een gebeurtenis Hub en gegevens verzonden en ontvangen hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.
- [Overzicht van de Hubs gebeurtenissen][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Gebeurtenis Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
