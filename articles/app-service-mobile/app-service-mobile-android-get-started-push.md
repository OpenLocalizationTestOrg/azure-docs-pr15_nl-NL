<properties
    pageTitle="Push-meldingen aan Android-App met Azure mobiele Apps toevoegen"
    description="Informatie over het gebruik van Azure Mobile-Apps te push-meldingen verzenden naar uw Android-app."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Push-meldingen toevoegen aan uw Android-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie toevoegen u push-meldingen aan het project [Android snel aan de slag] , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de push notification-extensie-pakket. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie.

## <a name="prerequisites"></a>Vereisten voor

U hebt het volgende nodig:

* Een IDE afhankelijk van uw project backend:

    * [Android Studio](https://developer.android.com/sdk/index.html) als deze app een backend Node.js heeft.

    * Heeft een backend .net [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) of hoger als deze app.

* Android 2.3 of hoger, Google opslagplaats revisie 27 of hoger en Google Play Services 9.0.2 of hoger voor Firebase Cloud Messaging.

* Voltooi het [Android snel aan de slag].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Een project die ondersteuning biedt voor Firebase Cloud Messaging maken

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Een melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure als u wilt verzenden push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Push-meldingen voor het serverproject inschakelen

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Push-meldingen toevoegen aan uw app

In deze sectie, kunt u uw client Android-app voor het verwerken van push-meldingen bijwerken.

### <a name="verify-android-sdk-version"></a>Versie van Android SDK controleren

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

De volgende stap is voor het installeren van Google Play services. Google Cloud Messaging heeft enkele API niveau minimumvereisten voor ontwikkelen en testen, die de eigenschap **minSdkVersion** in het Manifest moet voldoen aan.

Als u met een oudere apparaat testen wilt, raadpleegt u vervolgens [Instellen van Google Play Services SDK] om te bepalen hoe laag u kunt Stel deze waarde in en stel deze correct.

### <a name="add-google-play-services-to-the-project"></a>Google-Services afspelen toevoegen aan het project

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Code toevoegen

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>De app ten opzichte van de gepubliceerde mobiele service testen

U kunt de app testen door rechtstreeks koppelen een Android-telefoon met een USB-kabel, of door een virtueel apparaat in de emulator.

## <a name="more"></a>Meer

<!-- URLs -->
[Android snel aan de slag]: app-service-mobile-android-get-started.md

[Google Play Services SDK instellen]:https://developers.google.com/android/guides/setup
