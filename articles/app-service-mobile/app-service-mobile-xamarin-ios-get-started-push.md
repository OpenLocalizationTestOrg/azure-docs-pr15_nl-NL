<properties
    pageTitle="Push-meldingen toevoegen aan uw Xamarin.iOS-app met App-Azure-Service"
    description="Informatie over het gebruik van Azure App Service push-meldingen verzenden naar uw Xamarin.iOS-app"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Push-meldingen toevoegen aan uw Xamarin.iOS-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie toevoegen u push-meldingen aan het project [Xamarin.iOS snel starten](app-service-mobile-xamarin-ios-get-started.md) , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket push melding extensie. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten voor

* Voltooi de zelfstudie [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) .

* Een fysieke iOS-apparaat. Push-meldingen worden niet ondersteund door de iOS-simulator.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor push-meldingen op van Apple developer portal registreren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>De Mobile-App als u wilt verzenden push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject als u wilt verzenden push-meldingen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Uw project Xamarin.iOS configureren

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Push-meldingen toevoegen aan uw app

1. In **QSTodoService**, moet u de volgende eigenschap toevoegen zodat **AppDelegate** kunnen de mobiele client downloaden:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Voeg de volgende `using` instructie naar het begin van het bestand **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Overschrijven in **AppDelegate**, de gebeurtenis **FinishedLaunching** :

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Overschrijven in hetzelfde bestand, de gebeurtenis **RegisteredForRemoteNotifications** . In deze code registreert u voor een eenvoudige sjabloon melding dat wordt verzonden voor alle ondersteunde platforms door de server.

    Zie voor meer informatie over sjablonen met melding Hubs, [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Vervolgens overschrijven de gebeurtenis **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Uw app wordt nu bijgewerkt ter ondersteuning van push-meldingen.

## <a name="test"></a>Test push-meldingen in uw app

1. Druk op de knop **uitvoeren** voor het bouwen van het project en start de app in een iOS-apparaat en klik op **OK** om pushmeldingen te accepteren.

    > [AZURE.NOTE] U moet expliciet push-meldingen uit uw app accepteren. Deze aanvraag vindt alleen plaats voor de eerste keer is dat de app wordt uitgevoerd.

2. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.

3. Controleer of dat een melding wordt ontvangen en klik op **OK** om te sluiten van de melding.

4. Herhaal stap 2 en direct sluiten van de app en controleer of een melding wordt weergegeven.

Deze zelfstudie voltooid is.

<!-- Images. -->

<!-- URLs. -->



