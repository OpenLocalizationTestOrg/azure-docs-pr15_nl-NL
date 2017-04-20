<properties
    pageTitle="Azure Mobile betrokkenheid iOS SDK overzicht | Microsoft Azure"
    description="Meest recente updates en procedures voor iOS SDK voor Azure Mobile betrokkenheid"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK voor Azure Mobile betrokkenheid

Begin hier om alle details over het integreren van Azure Mobile betrokkenheid in een App voor iOS. Als u wilt zelf proberen eerst, controleert u of doen u onze [15 minuten zelfstudie](mobile-engagement-ios-get-started.md).

Klik op om de [SDK inhoud](mobile-engagement-ios-sdk-content.md) weer te geven

##<a name="integration-procedures"></a>Integratie van procedures
1. Begin hier: [het integreren van Mobile betrokkenheid in uw iOS-app](mobile-engagement-ios-integrate-engagement.md)

2. Voor meldingen: [het integreren van bereiken (meldingen) in uw iOS-app](mobile-engagement-ios-integrate-engagement-reach.md)

3. Een melding geven voor de implementatie plannen: [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw iOS-app](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Releaseopmerkingen

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Vaste melding niet mailbericht op apparaten met iOS 10.
-   Afschaffen XCode 7.

Zie de [volledige releaseopmerkingen](mobile-engagement-ios-release-notes.md) voor eerdere versie

##<a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van betrokkenheid geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

U moet mogelijk verschillende procedures volgen als u verschillende versies van de SDK Zie de volledige [Procedures Upgrade](mobile-engagement-ios-upgrade-procedure.md)hebt gemist.

Voor elke nieuwe versie van de SDK moet u eerst vervangen (verwijderen en opnieuw te importeren in xcode) de mappen EngagementSDK en EngagementReach.

###<a name="from-300-to-400"></a>Uit 3.0.0 naar 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 is verplicht vanaf versie 4.0.0 van de SDK.

> [AZURE.NOTE] Als u echt XCode 7 afhankelijk kunt u de [iOS betrokkenheid SDK v3.2.4](https://aka.ms/r6oouh). Er is van een bekende fout op de module bereik van deze vorige versie tijdens het uitvoeren van op apparaten met iOS 10: systeemmeldingen worden niet mailbericht. U moet de vervallen API implementeren oplossing `application:didReceiveRemoteNotification:` in uw app delegeren als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een upgrade van de versie komende (zelfs secundaire) iOS omdat deze iOS API is afgeschaft. U moet overschakelen naar XCode 8 zo snel mogelijk.

#### <a name="usernotifications-framework"></a>UserNotifications framework
U moet toevoegen de `UserNotifications` framework in uw fasen maken.

in de Projectverkenner, opent u het deelvenster van uw project en selecteer het juiste doel. Klik, open het tabblad **'Fasen maken'** en toevoegen in het menu **"koppeling binaire met bibliotheken"** framework `UserNotifications.framework` -de koppeling als instellen`Optional`

#### <a name="application-push-capability"></a>De mogelijkheid push toepassing
XCode 8 mogelijk uw app opnieuw push mogelijkheid: dubbele Raadpleeg deze de `capability` tabblad van het geselecteerde doel.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>De nieuwe iOS 10 melding registratiecode toevoegen
Het oudere codefragment registreren van de app naar meldingen werkt nog steeds, maar vervallen API's wordt gebruikt terwijl u op iOS 10 uitvoert. 

Importeren de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

In uw toepassing gemachtigde `application:didFinishLaunchingWithOptions` methode vervangen:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

door:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u al uw eigen implementatie UNUserNotificationCenterDelegate

De SDK heeft een eigen implementatie van het UNUserNotificationCenterDelegate-protocol. Wordt gebruikt door de SDK om te controleren van de levenscyclus van betrokkenheid meldingen op apparaten met op iOS 10 of hoger. Als de SDK worden gedetecteerd uw gemachtigde wordt niet gebruikt via een eigen implementatie omdat er slechts één UNUserNotificationCenter gemachtigde per toepassing. Dit betekent dat u hebt de betrokkenheid logica toevoegen aan uw eigen gemachtigde.

Er zijn twee manieren hiervoor.

Oproepen door uw gemachtigde doorsturen naar de SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Of door het overnemen van de `AEUserNotificationHandler` class

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] U kunt bepalen of een melding afkomstig van betrokkenheid of niet door het doorgeven van is de `userInfo` woordenlijst die u wilt de Agent `isEngagementPushPayload:` klasse methode.