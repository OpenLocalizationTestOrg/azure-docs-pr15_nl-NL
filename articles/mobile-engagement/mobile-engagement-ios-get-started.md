<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor iOS in doelstelling C | Microsoft Azure"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en push-meldingen voor iOS-apps."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Aan de slag met Azure Mobile betrokkenheid voor iOS-apps in doelstelling C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u het gebruik van Azure Mobile betrokkenheid om te begrijpen uw appgebruik en push-meldingen verzenden naar gesegmenteerde gebruikers aan een iOS-toepassing.
In deze zelfstudie maakt u een lege iOS-app die worden verzameld basisgegevens en Apple Push Notification-systeem (APNS) met push-meldingen ontvangt.

Deze zelfstudie is het volgende nodig:

+ XCode 8, die u via uw MAC App Store installeren kunt
+ de [mobiele betrokkenheid iOS SDK]

Voltooien van deze zelfstudie is vereist voor alle andere mobiele betrokkenheid zelfstudies voor iOS-apps.

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobile betrokkenheid instellen voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie", dat wil de minimale set vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. De volledige integratie documentatie vindt u in de [Mobile-betrokkenheid iOS SDK-integratie](mobile-engagement-ios-sdk-overview.md)

We gaan een eenvoudige app maken met XCode om te laten zien van de integratie.

###<a name="create-a-new-ios-project"></a>Maak een nieuw iOS-project

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

1. Download de [betrokkenheid van mobiele iOS SDK].
2. Pak de. tar.gz-bestand naar een map op uw computer.
3. Met de rechtermuisknop op het project en selecteer vervolgens **de bestanden toevoegen aan**.

    ![][1]

4. Navigeer naar de map waarin u de SDK, selecteer uitgepakt de `EngagementSDK` map en druk vervolgens op **OK**.

    ![][2]

5. Open het tabblad **Fasen maken** en toevoegen in het menu **Binaire koppeling met bibliotheken** de kaders zoals hieronder wordt weergegeven:

    ![][3]

6. Ga terug naar de Azure-portal in de pagina **Verbindingsgegevens** van uw app en kopieer de verbindingsreeks.

    ![][4]

7. Voeg de volgende regel met code in het bestand **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Nu plakken de verbindingsreeks in het `didFinishLaunchingWithOptions` delegeren.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`is een optionele instructie waarmee SDK Logboeken kunt identificeren van problemen. 

##<a id="monitor"></a>Realtime controle inschakelen

Om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actieve, moet u ten minste één scherm (activiteit) op de backend Mobile betrokkenheid verzenden.

1. Open het bestand **ViewController.h** en **EngagementViewController.h**importeren:

    `# import "EngagementViewController.h"`

2. Nu vervangen door de bovenliggende klasse van de interface **ViewController** door `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u interactief werken met uw gebruikers en met push-meldingen en in-app in de context van campagnes messaging hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties uw app instellen voor het ontvangen van deze.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app stille Push-meldingen wilt ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>De bibliotheek bereik toevoegen aan uw project

1. Met de rechtermuisknop op uw project.
2. Selecteer **bestand toevoegen aan**.
3. Navigeer naar de map waarin u de SDK hebt uitgepakt.
4. Selecteer de `EngagementReach` map.
5. Klik op **toevoegen**.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

1. Terug in **AppDeletegate.m** -bestand, importeert u de module betrokkenheid hebt bereikt.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Binnen de `application:didFinishLaunchingWithOptions` methode, maak een module bereik en doorgegeven aan uw bestaande betrokkenheid initialisatie regel:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Uw app APNS Push-meldingen wilt ontvangen inschakelen

1. Voeg de volgende regel aan de `application:didFinishLaunchingWithOptions` methode:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Toevoegen de `application:didRegisterForRemoteNotificationsWithDeviceToken` methode als volgt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Toevoegen de `didFailToRegisterForRemoteNotificationsWithError` methode als volgt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Toevoegen de `didReceiveRemoteNotification:fetchCompletionHandler` methode als volgt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile betrokkenheid iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

