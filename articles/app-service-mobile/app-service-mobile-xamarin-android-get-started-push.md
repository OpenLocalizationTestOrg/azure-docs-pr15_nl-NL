<properties
    pageTitle="Push-meldingen toevoegen aan uw Xamarin.Android-app | Azure App-Service"
    description="Informatie over het gebruik van Azure App-Service en Azure melding Hubs push-meldingen verzenden naar uw Xamarin.Android-app"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Push-meldingen toevoegen aan uw Xamarin.Android-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht


In deze zelfstudie toevoegen u push-meldingen aan het project [Xamarin.Android snel starten](app-service-mobile-windows-store-dotnet-get-started.md) , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket push melding extensie. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.


##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ Een actieve Google-account. U kunt zich aanmelden voor een Google-account aan [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud Messaging clientcomponent](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Een melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Firebase inschakelen Cloud Messaging

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Azure push-query's verzonden configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Bijwerken van het serverproject als u wilt verzenden push-meldingen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Het project client voor push-meldingen configureren

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Push-meldingen code toevoegen aan uw app

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Test push-meldingen in uw app

U kunt de app testen met behulp van een virtueel apparaat in de emulator. Er zijn extra configuratiestappen wanneer u zich in een emulator vereist.

1. Zorg ervoor dat u naar implementeren of foutopsporing op een virtueel apparaat waarop Google APIs instellen als het doel, zoals hieronder wordt weergegeven in de manager Android virtuele apparaat (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**, volg de aanwijzingen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. De takenlijst van-app als voordat u uitvoeren en voeg een nieuw item van de taak. Schakel ditmaal wordt een meldingspictogram voor een weergegeven in het systeemvak. U kunt de melding voor vellen om weer te geven van de volledige tekst van de melding openen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
