<properties
    pageTitle="Azure Mobile betrokkenheid iOS SDK upgraden Procedure | Microsoft Azure"
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

#<a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van betrokkenheid geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

Voor elke nieuwe versie van de SDK moet u eerst vervangen (verwijderen en opnieuw te importeren in xcode) de mappen EngagementSDK en EngagementReach.

##<a name="from-300-to-400"></a>Uit 3.0.0 naar 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 is verplicht vanaf versie 4.0.0 van de SDK.

> [AZURE.NOTE] Als u echt XCode 7 afhankelijk kunt u de [iOS betrokkenheid SDK v3.2.4](https://aka.ms/r6oouh). Er is van een bekende fout op de module bereik van deze vorige versie tijdens het uitvoeren van op apparaten met iOS 10: systeemmeldingen worden niet mailbericht. U moet de vervallen API implementeren oplossing `application:didReceiveRemoteNotification:` in uw app delegeren als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een upgrade van de versie komende (zelfs secundaire) iOS omdat deze iOS API is afgeschaft. U moet overschakelen naar XCode 8 zo snel mogelijk.

### <a name="usernotifications-framework"></a>UserNotifications framework
U moet toevoegen de `UserNotifications` framework in uw fasen maken.

in de Projectverkenner, opent u het deelvenster van uw project en selecteer het juiste doel. Klik, open het tabblad **'Fasen maken'** en toevoegen in het menu **"koppeling binaire met bibliotheken"** framework `UserNotifications.framework` -de koppeling als instellen`Optional`

### <a name="application-push-capability"></a>De mogelijkheid push toepassing
XCode 8 mogelijk uw app opnieuw push mogelijkheid: dubbele Raadpleeg deze de `capability` tabblad van het geselecteerde doel.

### <a name="add-the-new-ios-10-notification-registration-code"></a>De nieuwe iOS 10 melding registratiecode toevoegen
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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u al uw eigen implementatie UNUserNotificationCenterDelegate

De SDK, heeft ook een eigen implementatie van het UNUserNotificationCenterDelegate-protocol. Wordt gebruikt door de SDK om te controleren van de levenscyclus van betrokkenheid meldingen op apparaten met op iOS 10 of hoger. Als de SDK worden gedetecteerd uw gemachtigde wordt niet gebruikt via een eigen implementatie omdat er slechts één UNUserNotificationCenter gemachtigde per toepassing. Dit betekent dat u hebt de betrokkenheid logica toevoegen aan uw eigen gemachtigde.

Er zijn twee manieren hiervoor.

Oproepen naar de SDK door uw gemachtigde doorschakelen:

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

##<a name="from-200-to-300"></a>Uit 2.0.0 naar 3.0.0
Ondersteuning voor iOS tekst 4.X. Starten vanuit deze versie van het implementatiedoel van uw toepassing moet ten minste iOS 6.

Als u bereik in uw toepassing gebruikt, moet u toevoegen `remote-notification` waarde aan de `UIBackgroundModes` matrix in uw bestand Info.plist om te kunnen externe meldingen ontvangen.

De methode `application:didReceiveRemoteNotification:` moet worden vervangen door `application:didReceiveRemoteNotification:fetchCompletionHandler:` in uw toepassing gemachtigde.

"AEPushDelegate.h" is afgeschaft interface en u moet alle verwijzingen verwijderen. Dit geldt ook voor verwijderen `[[EngagementAgent shared] setPushDelegate:self]` en de gemachtigde methoden uit uw gemachtigde toepassing:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Uit 1.16.0 naar 2.0.0
Het volgende beschreven hoe u het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app mogelijk gemaakt door Azure Mobile betrokkenheid.
Als u vanuit een eerdere versie migreert, raadpleegt u de website Capptain om te migreren naar 1.16 eerst de volgende procedure toepassen.

>[AZURE.IMPORTANT] Capptain en Mobile betrokkenheid zijn niet dezelfde services en het migreren van de client-app in de onderstaande procedure alleen worden gemarkeerd. Migreren van de SDK in de app migreert geen gegevens van de Capptain-servers met de servers Mobile betrokkenheid

### <a name="agent"></a>Agent

De methode `registerApp:` is vervangen door de nieuwe methode `init:`. Uw gemachtigde toepassing dienovereenkomstig gewijzigd moeten worden bijgewerkt en verbindingsreeks gebruiken:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd bijhouden is verwijderd uit SDK u hoeft te verwijderen van alle exemplaren van `AETrackModule` class

### <a name="class-name-changes"></a>Klasse naamwijzigingen

Als onderdeel van de rebranding zijn er enkele klassebestand/namen die moeten worden gewijzigd.

De namen van alle klassen voorafgegaan door 'CP' worden gewijzigd met "AE" voorvoegsel.

Voorbeeld:

-   `CPModule.h`naam is gewijzigd in `AEModule.h`.

De namen van alle klassen voorafgegaan door 'Capptain' worden gewijzigd met "Betrokkenheid" voorvoegsel.

Voorbeelden:

-   De klasse `CapptainAgent` is gewijzigd in `EngagementAgent`.
-   De klasse `CapptainTableViewController` is gewijzigd in `EngagementTableViewController`.
-   De klasse `CapptainUtils` is gewijzigd in `EngagementUtils`.
-   De klasse `CapptainViewController` is gewijzigd in `EngagementViewController`.
