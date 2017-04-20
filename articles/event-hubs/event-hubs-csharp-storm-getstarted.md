<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in C# met Apache Storm | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; verzenden van gebeurtenissen in C# en ontvangt deze in een cluster Apache Storm."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer scalable opname-systeem dat opname miljoenen kunt gebeurtenissen per seconde, zodat een toepassing te verwerken en de grote hoeveelheden gegevens analyseren geproduceerd door uw verbonden apparaten en toepassingen. Zodra die worden verzameld in gebeurtenis Hubs, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslag cluster.

Voor meer informatie raadpleegt u de [gebeurtenis Hubs overzicht].

In deze zelfstudie leert u hoe berichten op een gebeurtenis Hub met een consoletoepassing in C# nemen en deze weer ophalen parallel Apache Storm gebruiken.

Als u wilt deze zelfstudie moet u de volgende handelingen uit:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een Java-ontwikkelomgeving is geconfigureerd voor het uitvoeren van [Maven](http://maven.apache.org/). Voor deze zelfstudie wordt ervan uitgegaan [Eclips](https://www.eclipse.org/)

+ Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure gratis proefversie</a>voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  De klasse **LogTopology** onderdelen uitvoeren vanaf Eclips en wacht totdat deze naar het begin van de ontvangers voor alle partities.

2.  Het project **afzender** uitvoeren, drukt u op **Enter** in het consolevenster en raadpleegt u de gebeurtenissen die worden weergegeven in het venster van de ontvanger.

    ![][22]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werken-toepassing die wordt gemaakt van een gebeurtenis Hub en gegevens verzonden en ontvangen hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 