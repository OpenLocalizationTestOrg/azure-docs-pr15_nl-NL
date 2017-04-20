<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor iOS in Swift | Microsoft Azure"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor iOS Apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Aan de slag met Azure Mobile betrokkenheid voor iOS-Apps in Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u het gebruiken van Azure Mobile betrokkenheid om meer informatie over het appgebruik van uw en push-meldingen verzenden naar gesegmenteerde gebruikers aan een iOS-toepassing.
In deze zelfstudie maakt u een lege iOS-app die worden verzameld basisgegevens en Apple Push Notification-systeem (APNS) met push-meldingen ontvangt.

Deze zelfstudie is het volgende nodig:

+ XCode 8, die u via uw MAC App Store installeren kunt
+ de [mobiele betrokkenheid iOS SDK]
+ Push-meldingen certificaat (.p12) die u kunt krijgen op uw Apple-ontwikkelaar Center

> [AZURE.NOTE] In deze zelfstudie wordt Swift versie 3.0. 

Voltooien van deze zelfstudie is vereist voor alle andere mobiele betrokkenheid zelfstudies voor iOS-apps.

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobile betrokkenheid instellen voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie", dat wil de minimale set vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. De volledige integratie documentatie vindt u in de [Mobile-betrokkenheid iOS SDK-integratie](mobile-engagement-ios-sdk-overview.md)

We gaan een eenvoudige app maken met XCode om te laten zien van de integratie:

###<a name="create-a-new-ios-project"></a>Maak een nieuw iOS-project

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met mobiele betrokkenheid backend

1. De [mobiele betrokkenheid iOS SDK] downloaden
2. Pak de. tar.gz-bestand naar een map op uw computer
3. Klik met de rechtermuisknop op het project en selecteer "U bestanden toevoegen aan..."

    ![][1]

4. Navigeer naar de map waarin u de SDK en selecteer uitgepakt de `EngagementSDK` map en vervolgens drukt u op OK.

    ![][2]

5. Open de `Build Phases` tabblad en in de `Link Binary With Libraries` menu toevoegen de kaders, zoals hieronder wordt weergegeven:

    ![][3]

8. Maken van een koptekst Bridging om te kunnen gebruiken van de SDK doelstelling C API's met de opdracht Bestand > Nieuw > Bestand > iOS > bron > Koptekst-bestand.

    ![][4]

9. Bewerk het ondersteunende koptekst-bestand om te laten zien Mobile betrokkenheid doelstelling-C-code aan uw snelle code, voegt u de volgende invoer:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Controleer of de doel-C bruggen koptekst opbouwen instellen onder Swift compileerprogramma - Code genereren heeft een pad naar deze koptekst onder instellingen voor het maken. Hier volgt een voorbeeld van het pad: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (afhankelijk van het pad)**

    ![][6]

11. Ga terug naar de Azure-portal in de pagina *Verbindingsgegevens* van uw app en kopieer de verbindingsreeks

    ![][5]

12. Nu plakken de verbindingsreeks in het `didFinishLaunchingWithOptions` delegeren

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Realtime-controle inschakelen

Om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actieve, moet u ten minste één scherm (activiteit) op de backend Mobile betrokkenheid verzenden.

1. Open het bestand **ViewController.swift** en de klasse base van **ViewController** moeten **EngagementViewController**vervangen:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u communiceren en contact met uw gebruikers met Push-meldingen en Messaging In-app in de context van campagnes. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties wordt uw app voor het ontvangen van deze instelling.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app stille Push-meldingen wilt ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>De bibliotheek bereik toevoegen aan uw project

1. Klik met de rechtermuisknop op uw project
2. Selecteer`Add file to ...`
3. Navigeer naar de map waarin u de SDK hebt uitgepakt
4. Selecteer de `EngagementReach` map
5. Klik op toevoegen
6. Bewerk het ondersteunende koptekst-bestand om te laten zien Mobile betrokkenheid doelstelling-C bereikt kop en de volgende invoer toevoegen:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. Binnen de `didFinishLaunchingWithOptions` - maken van een bereik-module en doorgegeven aan uw bestaande betrokkenheid initialisatie regel:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Uw app APNS Push-meldingen wilt ontvangen inschakelen
1. Voeg de volgende regel aan de `didFinishLaunchingWithOptions` methode:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Toevoegen de `didRegisterForRemoteNotificationsWithDeviceToken` methode als volgt:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Toevoegen de `didReceiveRemoteNotification:fetchCompletionHandler:` methode als volgt:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile betrokkenheid iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
