<properties
    pageTitle="Azure Mobile betrokkenheid iOS SDK hebt bereikt integratie | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Hoe kan worden bereikt van betrokkenheid integreren op iOS

U moet de integratie-procedure van [hoe u integreren betrokkenheid op iOS document](mobile-engagement-ios-integrate-engagement.md) voordat u deze handleiding volgen.

Deze documentatie is XCode 8 vereist. Als u echt XCode 7 afhankelijk kunt u de [iOS betrokkenheid SDK v3.2.4](https://aka.ms/r6oouh). Er is van een bekende fout op deze vorige versie tijdens het uitvoeren van op apparaten met iOS 10: systeemmeldingen worden niet mailbericht. U moet de vervallen API implementeren oplossing `application:didReceiveRemoteNotification:` in uw app delegeren als volgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Deze oplossing wordt niet aanbevolen** als dit gedrag kunt wijzigen in een upgrade van de versie komende (zelfs secundaire) iOS omdat deze iOS API is afgeschaft. U moet overschakelen naar XCode 8 zo snel mogelijk.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Uw app stille Push-meldingen wilt ontvangen inschakelen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Integratie van stappen

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>De betrokkenheid hebt bereikt SDK insluiten in uw iOS-project

-   De sdk bereik in uw project Xcode toevoegen. In Xcode, gaat u naar **Project \> toevoegen aan project** en kies de `EngagementReach` map.

### <a name="modify-your-application-delegate"></a>Uw gemachtigde toepassing wijzigen

-   Aan de bovenkant van uw implementatiebestand, importeert u de module betrokkenheid bereiken:

        [...]
        #import "AEReachModule.h"

-   Binnen methode `applicationDidFinishLaunching:` of `application:didFinishLaunchingWithOptions:`, maak een module bereik en doorgegeven aan uw bestaande betrokkenheid initialisatie regel:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   De tekenreeks **'icon.png'** met de naam van de afbeelding die u wilt instellen als het meldingspictogram van de wijzigen.
-   Als u wilt gebruiken, de optie *Update badge waarde* in het bereik campagnes of als u wilt gebruiken systeemeigen push \<SaaS/bereik API/campagne opmaak/Native Push\> campagnes, moet u het bereik module beheren de badge pictogram zelf (deze wordt automatisch schakelt u de toepassing badge en ook opnieuw de waarde die is opgeslagen door betrokkenheid telkens wanneer de toepassing gestart of foregrounded wordt) laten. Hiermee wordt uitgevoerd door het toevoegen van de volgende regel na het bereik module initialisatie:

        [reach setAutoBadgeEnabled:YES];

-   Als u bereiken gegevens push afhandelen wilt, moet u de werkdruk uw toepassing gemachtigde die voldoen aan de `AEReachDataPushDelegate` protocol. Voeg de volgende regel na bereik module initialisatie:

        [reach setDataPushDelegate:self];

-   Vervolgens kunt u de methoden implementeren `onDataPushStringReceived:` en `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in uw gemachtigde toepassing:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Categorie

De categorieparameter is optioneel wanneer u een campagne Push-gegevens maken en kunt dat u gegevens filteren verplaatst. Dit is handig als u wilt verschillende typen push van `Base64` gegevens en u wilt identificeren van het type voordat ze worden geparseerd.

**Uw toepassing is nu klaar voor ontvangen en bereik inhoud weer te geven!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Het ontvangen van aankondigingen en peilingen op elk gewenst moment

Betrokkenheid kunt bereiken meldingen verzenden om uw eindgebruikers op elk gewenst moment via de Apple Push Notification-Service.

Als u wilt deze functionaliteit inschakelt, moet u uw toepassing voor de Apple push-meldingen voorbereiden en uw gemachtigde toepassing wijzigen.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Uw toepassing voor de Apple push-meldingen voorbereiden

Volg de handleiding: [het voorbereiden van uw toepassing voor Apple Push-meldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>De benodigde clientcode hebt toegevoegd

*Uw toepassing moet nu een geregistreerd Apple push-certificaat hebben in de betrokkenheid frontend.*

Als dit nog niet klaar, moet u uw toepassing pushmeldingen wilt ontvangen registreren.

* Importeren de `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Voeg de volgende regel wanneer uw toepassing wel wordt gestart (meestal in `application:didFinishLaunchingWithOptions:`):

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

Vervolgens moet u het apparaat token geretourneerd door Apple-servers betrokkenheid geven. Dit doet u in de methode met de naam `application:didRegisterForRemoteNotificationsWithDeviceToken:` in uw gemachtigde toepassing:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

U moet ten slotte de betrokkenheid SDK informeren wanneer uw toepassing een externe melding krijgt. Bellen om dat te doen, de methode `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in uw gemachtigde toepassing:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] De bovenstaande methode is geïntroduceerd in iOS 7. Als u hebt samengesteld iOS < 7: Zorg ervoor dat willen implementeren methode `application:didReceiveRemoteNotification:` in uw toepassing gemachtigde en gesprek `applicationDidReceiveRemoteNotification` op de EngagementAgent door nul in plaats van de `handler` van de argumenten:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Standaard is de completionHandler besturingselementen betrokkenheid hebt bereikt. Desgewenst kunt u handmatig reageren op de `handler` blokkeren in code, kunt u nul doorgeven voor de `handler` argument en bepaal de voltooiing blokkeren zelf. Zie de `UIBackgroundFetchResult` type voor een lijst van mogelijke waarden.


### <a name="full-example"></a>Volledige voorbeeld

Hier volgt een volledige voorbeeld van de integratie:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Als u uw eigen implementatie UNUserNotificationCenterDelegate hebt

De SDK, heeft ook een eigen implementatie van het UNUserNotificationCenterDelegate-protocol. Wordt gebruikt door de SDK om te controleren van de levenscyclus van betrokkenheid meldingen op apparaten met op iOS 10 of hoger. Als de SDK worden gedetecteerd uw gemachtigde wordt niet gebruikt via een eigen implementatie omdat er slechts één UNUserNotificationCenter gemachtigde per toepassing. Dit betekent dat u hebt de betrokkenheid logica toevoegen aan uw eigen gemachtigde.

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

##<a name="how-to-customize-campaigns"></a>Het aanpassen van campagnes

### <a name="notifications"></a>Meldingen

Er zijn twee soorten meldingen: systeem en app-meldingen.

Systeemmeldingen worden afgehandeld door iOS en kunnen niet worden aangepast.

App-meldingen zijn gemaakt van een weergave die dynamisch wordt toegevoegd aan de huidige toepassingsvenster. Dit is een melding overlay genoemd. Melding overlays zijn zeer geschikt voor een snelle integratie omdat ze u niet hoeft te wijzigen van een weergave in uw toepassing.

#### <a name="layout"></a>Indeling

Als u wilt het uiterlijk van uw app-meldingen wijzigen, kunt u gewoon het bestand wijzigen `AENotificationView.xib` aan uw behoeften, zolang u de markering waarden en het typen van de bestaande subweergaven behouden.

App-meldingen worden standaard weergegeven onder aan het scherm. Als u liever ze aan de bovenkant van het scherm moet worden weergegeven, de meegeleverde bewerken `AENotificationView.xib` en wijzig de `AutoSizing` eigenschap van de belangrijkste weergave, zodat deze boven aan de superview kan worden bewaard.

#### <a name="categories"></a>Categorieën

Als u de meegeleverde indeling wijzigen, wijzigt u het uiterlijk van alle meldingen voor uw. Categorieën kunnen u verschillende gerichte weergaven (mogelijk gedrag) definiëren voor meldingen. Een categorie kan worden opgegeven bij het maken van een bereik campagne. Houd er rekening mee dat categorieën kunnen u ook aanpassen aankondigingen en polls, dat wordt beschreven verderop in dit document.

Als u wilt een categorie-handler voor uw meldingen hebt geregistreerd, moet u een gesprek toevoegen nadat de module bereik is geïnitialiseerd.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`moet een exemplaar van een object dat aan het protocol voldoet `AENotifier`.

U kunt de protocolmethoden implementeren met uzelf of kunt u kiezen om de bestaande klasse reimplement `AEDefaultNotifier` grootste deel van het werk dat al uitvoert.

Als u de weergave van de melding voor een bepaalde categorie definiëren wilt, kunt u bijvoorbeeld in dit voorbeeld volgen:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Dit eenvoudige voorbeeld van categorie wordt ervan uitgegaan dat u hebt een bestand met de naam `MyNotificationView.xib` in het hoofdvenster van de toepassing bundel. Als de methode wordt niet gevonden een bijbehorend `.xib`, de melding wordt niet weergegeven en betrokkenheid de gegevens uit een bericht in de console.

De meegeleverde kroontjespen-bestand moet respecteren van de volgende regels:

-   Slechts moet één weergave bevatten.
-   Subweergaven moeten worden van hetzelfde type als de kleuren die in het bestand meegeleverde kroontjespen genoemd`AENotificationView.xib`
-   Subweergaven moeten hebben dezelfde tags hebben als de kleuren die binnen de meegeleverde kroontjespen-bestand met de naam`AENotificationView.xib`

> [AZURE.TIP] Alleen de meegeleverde kroontjespen-bestand, met de naam kopiëren `AENotificationView.xib`, en start werken vanaf hier. Maar pas op dat u, de weergave in dit bestand kroontjespen is gekoppeld aan de klasse `AENotificationView`. Deze klasse opnieuw de methode gedefinieerd `layoutSubViews` kunt verplaatsen of de grootte van de subweergaven naargelang de context. Wilt u mogelijk vervangen met een `UIView` of u aangepaste weergave class.

Als u nodig grondigere aanpassingen in uw meldingen hebt (als u bijvoorbeeld wilt dat uw weergave laden rechtstreeks vanuit de code), wordt u geadviseerd te eens kijken de meegeleverde bron code en klasse de documentatie van `Protocol ReferencesDefaultNotifier` en `AENotifier`.

Houd er rekening mee dat u de dezelfde kennisgever voor meerdere categorieën gebruiken kunt.

U kunt ook de kennisgever standaard als volgt gedefinieerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Melding afhandeling

Wanneer u met de standaardcategorie, sommige methoden levenscyclus worden genoemd in de `AEReachContent` object naar rapport statistieken en de status van de campagne bijwerken:

-   Wanneer de melding wordt weergegeven in de toepassing, de `displayNotification` methode (dat statistieken) wordt aangeroepen door `AEReachModule` als `handleNotification:` retourneert `YES`.
-   Als de melding wordt geannuleerd, de `exitNotification` methode wordt aangeroepen, statistische gemeld en volgende campagnes kunnen nu worden verwerkt.
-   Als u de melding klikt, `actionNotification` wordt genoemd, statistische gemeld en de bijbehorende actie wordt uitgevoerd.

Als uw implementatie van `AENotifier` omzeilt het standaardgedrag, moet u deze methoden levenscyclus bellen door zelf. De volgende voorbeelden laten zien sommige gevallen waarin het standaardgedrag is overgeslagen:

-   U kunt niet uitbreiden `AEDefaultNotifier`, bijvoorbeeld u categorie afhandeling helemaal geïmplementeerd.
-   U overrode `prepareNotificationView:forContent:`, moet u ten minste toewijzen `onNotificationActioned` of `onNotificationExited` naar een van uw U.I besturingselementen.

> [AZURE.WARNING] Als `handleNotification:` genereert een uitzondering, de inhoud wordt verwijderd en `drop` wordt genoemd, wordt dit gemeld in de statistiek en volgende campagnes kunnen nu worden verwerkt.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Melding als onderdeel van een bestaande weergave opnemen

Overlays kunnen zijn zeer geschikt voor een snelle integratie, maar soms geen handige, of ongewenste bijwerkingen kunnen hebben.

Als u niet tevreden met het systeem overlay in enkele van de weergaven bent, kunt u deze aanpassen voor deze weergaven.

U kunt de indeling van onze melding opnemen in uw bestaande weergaven. Hiervoor moet u er twee implementatie stijlen is:

1.  De weergave van de melding met interface builder toevoegen

    -   Open *Builder Interface*
    -   Plaats een 320 x 60 (of 768 x 60 als u op de iPad) `UIView` waar u de melding moet worden weergegeven
    -   Stel de waarde van het label voor deze weergave aan: **36822491**

2.  De weergave van de melding via programmacode toevoegen. De volgende code toe te voegen wanneer uw weergave is geïnitialiseerd:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`macro's vindt u in `AEDefaultNotifier.h`.

> [AZURE.NOTE] De standaard-kennisgever wordt automatisch gedetecteerd dat de melding-indeling is opgenomen in deze weergave en geen een overlay voor deze worden toegevoegd.

### <a name="announcements-and-polls"></a>Aankondigingen en polls

#### <a name="layouts"></a>Indelingen

U kunt de bestanden wijzigen `AEDefaultAnnouncementView.xib` en `AEDefaultPollView.xib` zo lang maken als u de markering waarden en het typen van de bestaande subweergaven behouden.

#### <a name="categories"></a>Categorieën

##### <a name="alternate-layouts"></a>Alternatieve indelingen

Zoals meldingen, kan de campagne categorie worden gebruikt om te laten alternatieve indelingen voor uw aankondigingen en polls.

Een als categorie wilt maken voor een aankondiging, moet u **AEAnnouncementViewController** uitbreiden en registeren zodra de module bereik is geïnitialiseerd:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Telkens wanneer een gebruiker wordt Klik op een melding voor een aankondiging met de categorie "Mijn\_categorie", de controller geregistreerde weergave (in dat geval `MyCustomAnnouncementViewController`) wordt geïnitialiseerd door te bellen van de methode `initWithAnnouncement:` en de weergave, worden toegevoegd aan het huidige toepassing.

Bij de implementatie van de `AEAnnouncementViewController` class die u moet de eigenschap lezen `announcement` uw subweergaven geïnitialiseerd. Houd rekening met het volgende voorbeeld, waarin twee etiketten, zijn geïnitialiseerd met `title` en `body` eigenschappen van de `AEReachAnnouncement` class:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Als u niet wilt dat uw weergaven laden door uzelf, maar u alleen wilt hergebruiken de standaardindeling voor de weergave van aankondiging, kunt u gewoon de aangepaste weergave controller breidt de meegeleverde klasse `AEDefaultAnnouncementViewController`. Dupliceren in dat geval het bestand kroontjespen `AEDefaultAnnouncementView.xib` en wijzig de naam zodat deze kan worden geladen door de aangepaste weergave controller (voor een controller met de naam `CustomAnnouncementViewController`, moet u uw bestand kroontjespen aanroepen `CustomAnnouncementView.xib`).

Als u wilt vervangen de standaardcategorie van aankondigingen, moet u gewoon de controller aangepaste weergave voor de categorie die is gedefinieerd in registreren `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Polls kunnen worden aangepast dat het op dezelfde manier:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Ditmaal, de meegeleverde `MyCustomPollViewController` moet uitbreiden `AEPollViewController`. Of u kunt ervoor kiezen om uit te breiden van de standaard-controller: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Vergeet niet om te bellen hetzij `action` (`submitAnswers:` voor besturing weergeven voor aangepaste peiling) of `exit` methode voordat de controller weergave is gesloten. Anders statistieken niet verzonden (dat wil zeggen geen analyses van de campagne) en meer is van belang dat volgende campagnes worden hierover niet geïnformeerd totdat het toepassingsproces opnieuw is opgestart.

##### <a name="implementation-example"></a>Voorbeeld van de implementatie

In deze implementatie is de weergave met aangepaste aankondiging geladen uit een externe xib-bestand.

Zoals voor geavanceerde melding aanpassingen, wordt het aanbevolen om te zoeken op de broncode van de standaard-implementatie.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
