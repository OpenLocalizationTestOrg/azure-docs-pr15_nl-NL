<properties
    pageTitle="iOS Pushmeldingen met melding Hubs voor Xamarin apps | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen verzenden naar een Xamarin iOS-toepassing."
    services="notification-hubs"
    keywords="IOS push-meldingen, push-berichten push-meldingen, push-bericht"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS Pushmeldingen met melding Hubs voor Xamarin-apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht
> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started)voor meer informatie.

Deze zelfstudie wordt getoond hoe u met Azure melding Hubs push-meldingen verzenden naar een iOS-toepassing.
U kunt een lege Xamarin.iOS-app die push-meldingen ontvangt via de [Apple Push Notification-Service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)wilt maken. Wanneer u klaar bent, kunt u zult gebruikmaken van uw hub melding uitzenden van push-meldingen naar alle apparaten die uw app uitvoeren. De code die klaar is beschikbaar in de [app NotificationHubs] [ GitHub] voorbeeld.

Deze zelfstudie ziet u het bericht eenvoudige push scenario uitzenden met melding Hubs.

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ [Xcode 6.0][Install Xcode]
+ Een iOS 7.0 (of hoger) compatibele apparaat
+ iOS ontwikkelaars programma lidmaatschap
+ [Xamarin Studio]

   > [AZURE.NOTE] Vanwege configuratievereisten voor iOS push-meldingen, moet u implementeren en testen van de steekproef-toepassing op een fysieke iOS-apparaat (iPhone of iPad) in plaats van in de simulator.

Voltooien van deze zelfstudie is vereist voor alle andere meldingen Hubs zelfstudies voor Xamarin iOS-apps.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>De melding hub configureren

In dit gedeelte begeleidt u bij het maken van een nieuwe melding hub en verificatie configureren met APNS met behulp van de **.p12** push-certificaat dat u hebt gemaakt. Als u gebruiken van een melding hub die u al hebt gemaakt wilt, kunt u verder met stap 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Omdat de verbinding APNS configureren in de Portal Azure, opent u de instellingen van de melding Hub en ande <b>Notification Services</b>op en klik vervolgens op het item <b>Apple (APNS)</b> in de lijst. Als ingevuld, klikt u op <b>Certificaat uploaden</b> en selecteer het <b>.p12</b> -certificaat dat u eerder hebt geëxporteerd en het wachtwoord voor het certificaat.</p>
<p>Zorg ervoor dat <b>Sandbox</b> -modus sinds u push-berichten in een ontwikkelomgeving verzenden. De <b>productieomgeving</b> invoert alleen gebruiken als u wilt verzenden push-meldingen aan gebruikers die al uw app in de winkel gekocht.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Uw hub melding is nu geconfigureerd voor het werken met APNS en u de verbindingstekenreeksen naar uw app hebt geregistreerd en het verzenden van push-meldingen hebt.


##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de melding hub

#### <a name="create-a-new-project"></a>Een nieuw project maken

1. Maak een nieuw project van iOS in Xamarin Studio en selecteert u de **API Unified** > **één weergave** sjabloon.

    ![Xamarin Studio - Select toepassingstype][31]

2. Een verwijzing naar het onderdeel Azure Messaging toevoegen. Klik in de weergave oplossing met de rechtermuisknop op de map **Components** voor uw project en kies **Meer onderdelen ophalen**. Zoek het onderdeel **Azure Messaging** en het onderdeel toevoegen aan uw project.

3. Voeg het volgende in **AppDelegate.cs**, met instructie:

        using WindowsAzure.Messaging;

4. Een exemplaar van **SBNotificationHub**declareren:

        private SBNotificationHub Hub { get; set; }

5. Maak een klasse **Constants.cs** met de volgende variabelen:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Bijwerken in **AppDelegate.cs**, **FinishedLaunching()** zodat deze overeenkomen met het volgende:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. De methode **RegisteredForRemoteNotifications()** in **AppDelegate.cs**overschrijven:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. De methode **ReceivedRemoteNotification()** in **AppDelegate.cs**overschrijven:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. De volgende **ProcessNotification()** methode op **AppDelegate.cs**maken:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] U kunt kiezen om te negeren **FailedToRegisterForRemoteNotifications()** om af te handelen situaties zoals helemaal geen netwerkverbinding. Dit is vooral belangrijk waar de gebruiker kan de toepassing start in de offlinemodus (bijvoorbeeld vliegtuig) en er moet gebeuren push messaging scenario's voor uw app.


10. De app uitvoeren op uw apparaat.


## <a name="sending-push-notifications"></a>Push-meldingen verzenden


U kunt testen push-meldingen ontvangen in uw app door te sturen van meldingen in de [Portal van Azure] via de mogelijkheid **Testen verzenden** in de werkset **Probleemoplossing** , rechts op de pagina van de hub melding, zoals wordt weergegeven in het volgende scherm.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Push-meldingen worden meestal verzonden via een back-enddatabase zoals Mobile Services of met behulp van een compatibele bibliotheek. U kunt ook de REST API rechtstreeks naar de push-berichten verzenden als een bibliotheek niet beschikbaar in uw scenario is. 

In deze zelfstudie we Houd het eenvoudig en alleen demonstreren testen van de client-app door meldingen met behulp van de .NET SDK voor melding hubs in een consoletoepassing in plaats van een back-end-service te verzenden. Het is raadzaam de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) als de volgende stap voor het verzenden van meldingen van een ASP.NET-end. De volgende methoden kunnen echter worden gebruikt voor het verzenden van meldingen:

* **REST API Interface**: U kunt push-bericht op elk backend-platform met de [REST API interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)ondersteunt.

* **Microsoft Azure melding Hubs .NET SDK**: In het Nuget pakket Manager voor Visual Studio [- Installatiepakket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)uitvoeren.

* **Node.js** : [het gebruik van de melding Hubs uit Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile-Apps**: Zie voor een voorbeeld van hoe u meldingen van een end Azure App Service Mobile-Apps die geïntegreerd met melding Hubs verzendt, [push-meldingen toevoegen aan uw mobiele app](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: Zie voor een voorbeeld van hoe u push-meldingen verzendt met behulp van de REST API's, "Het gebruik van de melding Hubs uit Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Optioneel) Push-meldingen verzenden vanuit een .NET-Console-App

In dit gedeelte worden we push-meldingen verzonden via een eenvoudige .NET-console-app. Voor de toepassing van dit voorbeeld wordt wordt we overschakelen naar een Windows-ontwikkelomgeving waarop Visual Studio al is geïnstalleerd.

1. Visual Studio, een nieuwe Visual C# consoletoepassing te maken:

    ![Visual Studio - Maak een nieuwe consoletoepassing][213]

2. Klik op **Hulpmiddelen voor**Visual Studio, op **NuGet Package Manager**en klik vervolgens op **Package Manager-Console**.

    De beheerconsole pakket moet worden gekoppeld aan de onderkant van uw werkruimte Visual Studio weergegeven.

3. Klik in het venster Package Manager Console instellen met de **project standaard** naar uw nieuwe console-toepassingsproject en klik vervolgens in het consolevenster Voer de volgende opdracht uit:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de Azure melding Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Open de `Program.cs` bestands- en voeg de volgende `using` instructie, zorgen dat die we Azure klassen en functies binnen uw belangrijkste klas kunt gebruiken:

        using Microsoft.Azure.NotificationHubs;

3. In uw `Program` klasse, voegt u de volgende methode (Vergeet niet voor het vervangen van de **verbindingsreeks** en de **hubnaam**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Voeg de volgende regels in uw `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Druk op F5 om uit te voeren van de app. In enkele seconden ziet u een push-bericht worden weergegeven op uw apparaat. Of u Wi-Fi of een mobiele dataverbinding gebruikt, zorg dat er een actieve internetverbinding op het apparaat.

U vindt de mogelijke nettoladingen in de Apple [lokaal en Push-meldingen Programming Guide].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Optioneel) Meldingen verzenden vanuit een mobiele Service

In dit gedeelte sturen we push-meldingen met een mobiele service via een script knooppunt.

Een bericht verzenden met behulp van een mobiele service, Ga als volgt [aan de slag met mobiele Services], en kiest u vervolgens:

1. Meld u aan bij de [Klassieke Azure-Portal]en selecteer uw telefoonprovider.

2. Selecteer het tabblad **Scheduler** op de voorgrond.

    ![Azure klassieke Portal - Scheduler][215]

3. Een nieuwe geplande taak maakt, Voeg een naam en selecteer **op aanvraag**.

    ![Azure klassieke Portal - nieuwe taak maken][216]

4. Wanneer de taak is gemaakt, klikt u op de taaknaam. Klik vervolgens op het tabblad **Script** op de bovenste balk.

5. Voeg in het volgende script binnen uw functie scheduler. Zorg ervoor dat de tijdelijke aanduidingen vervangen door de naam van de hub van de melding en de verbindingsreeks, voor *DefaultFullSharedAccessSignature* die u eerder hebt gekregen. Klik op **Opslaan**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Klik **Eenmaal uitvoeren** op de onderste balk. U ontvangt een melding op uw apparaat.

##<a name="next-steps"></a>Volgende stappen

In dit voorbeeld eenvoudige uitgezonden u push-meldingen voor al uw iOS-apparaten. Om u te richten op specifieke gebruikers, raadpleegt u de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers]. Als u wilt uw gebruikers door rente groepen segmenten, vindt u [Gebruik melding Hubs naar het laatste nieuws verzenden]. Meer informatie over het gebruik van de melding Hubs in de [Melding Hubs richtlijnen] en in de [Melding Hubs stapsgewijze procedures voor iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Aan de slag met mobiele Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure klassieke Portal]: https://manage.windowsazure.com/
[Melding Hubs richtlijnen]: http://msdn.microsoft.com/library/jj927170.aspx
[Melding Hubs stapsgewijze procedures voor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Gebruik van de melding Hubs om push-meldingen aan gebruikers]: /manage/services/notification-hubs/notify-users-aspnet
[Laatste nieuws verzenden via melding Hubs]: /manage/services/notification-hubs/breaking-news-dotnet

[Lokale en Push-meldingen hoeft te programmeren handleiding]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure-Portal]: https://portal.azure.com
