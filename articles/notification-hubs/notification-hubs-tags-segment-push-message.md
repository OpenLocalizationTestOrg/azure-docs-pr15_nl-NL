<properties
    pageTitle="Omleiden en Tag expressies"
    description="In dit onderwerp wordt uitgelegd Routering en tag expressies voor Azure melding hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>De Routering en tag expressies

##<a name="overview"></a>Overzicht

Tag expressies kunnen u specifieke sets doel van apparaten of specifieker registraties, bij het verzenden van een push-bericht tot en met melding Hubs.


## <a name="targeting-specific-registrations"></a>Specifieke registraties doelgroepen

De enige manier naar doelsite specifieke melding registraties is het koppelen van labels met hen, klikt u vervolgens deze tags afstemmen. Zoals is beschreven in de [Registratie Management](notification-hubs-push-notification-registration-management.md), push-meldingen ontvangen heeft een app tot de greep van een apparaat aan een hub melding hebt geregistreerd. Nadat een registratie is gemaakt op een melding-hub, kan de toepassing-end push-meldingen verzenden naar deze.
De toepassing-end kunt u de registraties naar doelsite met een specifieke melding kiezen in de volgende manieren:

1. **Uitzenden**: alle registraties in de hub melding ontvangt de melding.
2. **Tag**: alle rapporten met de opgegeven tag ontvangen het bericht.
3. **Code-expressie**: alle registraties waarvan set labels overeenkomen met de opgegeven expressie ontvangen het bericht.

## <a name="tags"></a>Labels

Een tag kan een willekeurige tekenreeks, maximaal 120 tekens, die bevat alfanumerieke en de volgende niet-alfanumerieke tekens zijn: '_', ‘@’, '#', '. ',':', '-'. Het volgende voorbeeld ziet u een toepassing waaruit u kunt mailpop meldingen over specifieke muziekgroepen ontvangen. In dit scenario is een eenvoudige manier om te routeren meldingen op label registraties met labels die staan voor de verschillende banden, zoals in de volgende afbeelding.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

In deze afbeelding, het bericht gelabelde **Beatles** bereikt alleen de tablet die met het label **Beatles**geregistreerd.

Zie voor meer informatie over het maken van registraties voor labels [Registratie Management](notification-hubs-push-notification-registration-management.md).

U kunt meldingen verzenden aan tags de manieren verzenden meldingen van de `Microsoft.Azure.NotificationHubs.NotificationHubClient` klasse in de [Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. U kunt ook Node.js of de Push-meldingen REST API gebruiken.  Hier volgt een voorbeeld met de SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Labels niet hoeft te worden vooraf ingerichte en gebruikt om te verwijzen naar meerdere app / regiospecifieke concepten. Gebruikers van deze toepassing voorbeeld kunnen bijvoorbeeld becommentariëren banden en wilt toasts, niet alleen voor de opmerkingen op hun favoriete banden, maar ook voor alle opmerkingen ontvangen van hun vrienden, ongeacht de band waarop ze een opmerking wilt maken. De volgende afbeelding ziet een voorbeeld van dit scenario:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

In deze afbeelding Lisa is geïnteresseerd updates voor de Beatles en Stefan updates voor de Wailers geïnteresseerd is. Stefan is ook geïnteresseerd in Charlie van opmerkingen en Charlie staat in geïnteresseerd in de Wailers. Wanneer er een melding voor Charlie van opmerking bij de Beatles verzonden wordt, ontvangt Lisa zowel Stefan deze.

Terwijl u meerdere bezwaren in labels (bijvoorbeeld "band_Beatles" of "follows_Charlie") coderen kunt, zijn labels eenvoudige tekenreeksen en niet de eigenschappen met waarden. Een registratie alleen op de aanwezigheid of afwezigheid van een bepaald label bij elkaar worden gezocht.

Zie voor een volledige stapsgewijze zelfstudie over het gebruik van labels voor het verzenden naar rente groepen, [Nieuws verbreken](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Labels aan Doelgebruikers gebruiken

Een andere manier om markeringen gebruiken is alle apparaten van een bepaalde gebruiker identificeren. Registraties kunnen zijn gemarkeerd met een code die een gebruikers-id, zoals in de volgende afbeelding bevat:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

In deze afbeelding bereikt het bericht gelabeld uid:Alice alle registraties gelabelde uid:Alice; daarom alle Lisa van apparaten.


##<a name="tag-expressions"></a>Tag expressies

Zijn er omstandigheden waarin een melding heeft een set van registraties die wordt aangegeven, niet door één tag, maar dan met een Booleaanse expressie op labels afstemmen.

Houd rekening met een sport-toepassing die een herinnering voor iedereen in Boston over een spel tussen de rood Sox en Cardinals stuurt. Als de app client tags belangstelling teams en locatie registreert, moet de melding naar iedereen in Boston die geïnteresseerd in het rood Sox of de Cardinals is worden afgestemd. Deze voorwaarde kan worden berekend met de volgende Booleaanse expressie:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Tag expressies kunnen bevat alle Booleaans operatoren, zoals AND (& &), of (|), en niet (!). Ze kunnen ook haakjes bevatten. Tag expressies zijn beperkt tot 20 tags als ze alleen ORs; bevatten anders zijn ze beperkt tot 6 tags.

Hier volgt een voorbeeld voor het verzenden van meldingen met tag expressies met de SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
