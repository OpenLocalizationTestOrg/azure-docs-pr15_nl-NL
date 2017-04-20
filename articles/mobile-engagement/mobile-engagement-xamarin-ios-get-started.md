<properties
    pageTitle="Aan de slag met Azure mobiele betrokkenheid voor Xamarin.iOS"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor Xamarin.iOS Apps."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Aan de slag met Azure mobiele betrokkenheid voor Xamarin.iOS-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u meer informatie over het appgebruik van uw en push-meldingen verzenden naar gesegmenteerde gebruikers in een toepassing voor Xamarin.iOS met Azure Mobile betrokkenheid.
In deze zelfstudie maakt u een lege Xamarin.iOS-app die worden verzameld basisgegevens en Apple Push Notification-systeem (APNS) met push-meldingen ontvangt.

Deze zelfstudie is het volgende nodig:

+ [Xamarin Studio](http://xamarin.com/studio). U kunt ook Visual Studio gebruikt met Xamarin, maar deze zelfstudie wordt Xamarin Studio. Zie [installatie en installeer voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx)voor installatie-instructies. 
+ [Mobiele betrokkenheid Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobile betrokkenheid instellen voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie"dat wil de minimale instellen vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden.

We gaan een eenvoudige app maken met Xamarin om te laten zien van de integratie:

###<a name="create-a-new-xamarinios-project"></a>Een nieuw Xamarin.iOS project maken

1. Start Xamarin Studio. Ga naar **bestand** -> **nieuwe** -> **oplossing** 

    ![][1]

2. Selecteer **Één weergave-App**, Controleer of dat de geselecteerde taal is **C#** en klik op **volgende**.

    ![][2]

3. Vul de **App-naam** en de **Organisatie-id** en klik vervolgens op **volgende**. 

    ![][3]

    > [AZURE.IMPORTANT] Zorg ervoor dat het publicerende profiel dat u uiteindelijk gebruiken om te implementeren van uw iOS-app een App-ID die overeenkomt met exact met de bundel-id die u hier hebt is gebruikt. 

4. Werk de **Naam van het Project**, de **Naam van de oplossing** en de **locatie** als vereist en klik op **maken**.

    ![][4]
 
Xamarin Studio maakt de demo-app waarin Mobile betrokkenheid integreert. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met mobiele betrokkenheid backend

1. Klik met de rechtermuisknop op de map **pakketten** in de vensters oplossing en selecteer **Pakketten toevoegen**

    ![][5]

2. Zoeken naar de **Microsoft Azure Mobile betrokkenheid Xamarin SDK** en deze toevoegen aan uw oplossing.  

    ![][6]
   
3. Open **AppDelegate.cs** en voeg de volgende instructie gebruiken:

        using Microsoft.Azure.Engagement.Xamarin;

4. In de methode **FinishedLaunching** toevoegen de volgende handelingen uit om de verbinding met mobiele betrokkenheid backend geïnitialiseerd. Zorg ervoor dat uw **ConnectionString**toevoegen. Deze code wordt ook gebruikt voor een pop **NotificationIcon** die is toegevoegd door de mobiele betrokkenheid-SDK die u wilt vervangen. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Realtime-controle inschakelen

Om te beginnen met het verzenden van gegevens en ervoor zorgen dat de gebruikers actief zijn, moet u ten minste één scherm naar de backend Mobile betrokkenheid sturen.

1. Open **ViewController.cs** en voeg de volgende instructie gebruiken:

        using Microsoft.Azure.Engagement.Xamarin;

2. Vervang de klas waaruit `ViewController` overneemt van `UIViewController` naar `EngagementViewController`. 

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u interactief werken met uw gebruikers en met push-meldingen en in-app in de context van campagnes messaging hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties uw app instellen voor het ontvangen van deze.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. Open de **AppDelegate.cs** en voeg de volgende instructie gebruiken:

        using System; 

2. Nu binnen de `FinishedLaunching` methode, de volgende handelingen uit om te registreren voor push-berichten na toevoegen`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Ten slotte - bijwerken of toevoegen van de volgende methoden:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Controleer of de **Bundel id** overeenkomt met met de **App-ID** die u in uw profiel inrichten in het midden van de ontwikkelaar Apple hebt in het bestand **Info.plist** in de oplossing. 

    ![][7]

5. In hetzelfde **Info.plist** , controleert u of u de **Achtergrond modi inschakelen** en **Externe meldingen**hebt ingeschakeld. 

    ![][8]

6. De app op het apparaat dat u hebt gekoppeld aan dit publicerende profiel uitvoeren. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
