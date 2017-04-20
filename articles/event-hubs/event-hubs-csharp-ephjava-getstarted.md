<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in C# | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; gebeurtenissen in C# verzenden en ontvangen ze in Java met behulp van de EventProcessorHost."
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
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een service waarmee grote hoeveelheden gegevens (telemetrielogboek) uit verbonden apparaten en toepassingen worden verwerkt. Nadat u gegevens in de gebeurtenis Hubs verzamelen, kunt u de gegevens met een cluster opslagruimte opslaan of transformeren met een realtime analytics-provider. Deze gebeurtenis grootschalige verzamelen en verwerken mogelijkheid is een belangrijk onderdeel van moderne toepassingsarchitecturen, inclusief de Internet van zaken aan bod (IoT).

Deze zelfstudie wordt getoond hoe u met de portal van Azure klassieke de Hub van een gebeurtenis maken. Deze ook ziet u hoe u voor het verzamelen van berichten op een gebeurtenis Hub met een consoletoepassing die is geschreven in C# en hoe u deze weer ophalen in parallelle met de bibliotheek Java gebeurtenis Processor Host.

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een actieve Azure-account. <br/>Als u deze niet hebt, kunt u een gratis account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank")voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

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
- [Overzicht van de Hubs gebeurtenissen][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
