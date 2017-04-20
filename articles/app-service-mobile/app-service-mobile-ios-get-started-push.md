<properties
    pageTitle="Pushmeldingen toevoegen aan de App met Azure Mobile-Apps voor iOS"
    description="Informatie over het gebruik van Azure Mobile-Apps push-meldingen verzenden naar uw iOS-app."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Pushmeldingen toevoegen aan uw iOS-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie toevoegen u push-meldingen aan het project [iOS snel starten] , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de push notification-extensie-pakket. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

[IOS simulator biedt geen ondersteuning voor push-meldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Moet u een fysiek iOS-apparaat en een [Apple ontwikkelaars programma lidmaatschap](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Register-app voor push-meldingen

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure als u wilt verzenden push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Back-end om te verzenden push-meldingen bijwerken

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Push-meldingen toevoegen aan app

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Test push-meldingen

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Meer

* Sjablonen flexibiliteit om meerdere platforms gereedschap duwt en gelokaliseerde gereedschap duwt te verzenden. [Hoe gebruik iOS Client-bibliotheek voor Azure Mobile-Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ziet u hoe u sjablonen registreren.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS snel aan de slag]: app-service-mobile-ios-get-started.md
