<properties
    pageTitle="Aan de slag met gebeurtenis Hubs in C# | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs met C# en gebruiken van de EventProcessorHost."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met Hubs gebeurtenis

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een service waarmee grote hoeveelheden gegevens (telemetrielogboek) uit verbonden apparaten en toepassingen worden verwerkt. Nadat u gegevens in de gebeurtenis Hubs verzamelen, kunt u de gegevens met een cluster opslagruimte opslaan of transformeren met een realtime analytics-provider. Deze gebeurtenis grootschalige verzamelen en verwerken mogelijkheid is een belangrijk onderdeel van moderne toepassingsarchitecturen, inclusief de Internet van zaken aan bod (IoT).

Deze zelfstudie wordt getoond hoe u met de portal van Azure klassieke de Hub van een gebeurtenis maken. Deze ook ziet u hoe u voor het verzamelen van berichten op een gebeurtenis Hub met een consoletoepassing die is geschreven in C# en hoe u deze weer ophalen in parallelle met de bibliotheek C# [Gebeurtenis Processor Host][] .

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een actieve Azure-account. Als u deze niet hebt, kunt u een gratis account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/free/)voor meer informatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Open in Visual Studio, de **ontvanger** project dat u eerder hebt gemaakt.

2. Met de rechtermuisknop op de oplossing **ontvanger** en vervolgens op **toevoegen**en klik vervolgens op **Bestaand Project**.
 
3. Zoek het bestaande Sender.csproj-bestand en klik erop dubbelklikken om het toe te voegen aan de oplossing.
 
4. Klik nogmaals met de rechtermuisknop op de **ontvanger** -oplossing en klik vervolgens op **Eigenschappen**. De pagina van de eigenschap **ontvanger** wordt weergegeven.

5. Klik op **Project opstarten**en klik op de knop **meerdere opstarten projecten** . Stel het vak **actie** voor de **ontvanger** en de **afzender** projecten te **starten**.

    ![][19]

6. Klik op **Projectafhankelijkheden**. Klik in het vak **projecten** op **afzender**. Controleer in het vak **Depends.exe op** of **dat ontvanger** is ingeschakeld.

    ![][20]

7. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen** .

1.  Druk op F5 u uitvoeren van het project uit van de **ontvanger** in Visual Studio en wacht totdat deze vervolgens de ontvangers voor alle partities wordt gestart.

    ![][21]

2.  De **afzender** project wordt automatisch uitgevoerd. Druk op **Enter** in het consolevenster en Zie de gebeurtenissen die worden weergegeven in het venster van de ontvanger.

    ![][22]

Druk op **Ctrl + C** in het venster van de **afzender** aan het einde van de afzender-toepassing en druk op **Enter** in het venster van de ontvanger van die toepassing afsluiten.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werken-toepassing die wordt gemaakt van een gebeurtenis Hub en gegevens verzonden en ontvangen hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.
- [Overzicht van de Hubs gebeurtenissen][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Gebeurtenis Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
