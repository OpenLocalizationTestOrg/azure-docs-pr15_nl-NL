<properties
    pageTitle="Melding Hubs recente nieuws zelfstudie - iOS"
    description="Informatie over het gebruik van Azure Service Bus melding Hubs recente nieuws meldingen naar apparaten met iOS stuurt."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Laatste nieuws verzenden via melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u het gebruiken van Azure melding Hubs uitzenden recente nieuws meldingen aan een iOS-app. Als alles compleet is, wordt u mogelijk te registreren voor verbreken nieuwscategorieën waarin u geïnteresseerd bent en u ontvangt alleen push-meldingen voor deze categorieën. Dit scenario is een algemene patroon voor veel apps waar meldingen moeten worden verzonden naar groepen gebruikers die eerder rente in deze, bijvoorbeeld RSS-lezer, apps voor muziek ventilatoren, enzovoort zijn gedeclareerd.

Uitzenden scenario's zijn ingeschakeld door een of meer _tags_ op te nemen bij het maken van een registratie in de hub melding. Wanneer meldingen worden verzonden naar een tag, worden alle apparaten die zijn geregistreerd voor de tag de melding ontvangt. Omdat tags gewoon tekenreeksen zijn, hebben ze geen vooraf worden ingericht. Voor meer informatie over de labels, raadpleegt u [de melding Hubs Routering en Tag expressies](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Vereisten voor

In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met melding Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebben uitgevoerd [aan de slag met melding Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande storyboard waarmee de gebruiker om te selecteren van categorieën om u te registreren. De categorieën die door een gebruiker is geselecteerd zijn op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in de hub melding met de geselecteerde categorieën gemaakt als labels.

1. In de objectbibliotheek van uw MainStoryboard_iPhone.storyboard toevoegen de volgende onderdelen:
    + Een label met tekst 'Nieuws verbreken',
    + Etiketten, met de tekst van de categorie "Wereld", "Politiek", "Bedrijven", "Technologie", "Wetenschap", "Sports",
    + Zes schakelopties, één per categorie, stelt u elke switch **staat** moeten **uitschakelen** al dan niet standaard.
    + Een knop gelabelde "Abonneren"

    Uw storyboard ziet er als volgt uit:

    ![][3]

2. Klik in de editor assistent wandcontactdozen voor alle schakelaars maken en het aanroepen van "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Een actie voor uw knop met de naam "abonneren" maken. Uw ViewController.h moet de volgende elementen bevatten:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Maak een nieuwe **Cacao Raak Class** genoemd `Notifications`. Kopieer de volgende code in de sectie interface van het bestand Notifications.h:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. De volgende importeren-instructie toevoegen aan Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopieer de volgende code in de sectie implementatie van het bestand Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Deze klasse gebruikt lokale opslag opslaan en ophalen van de categorieën van nieuws dat dit apparaat ontvangt. Het bevat ook een methode om u te registreren voor deze categorieën met de registratie van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) .

7. In het bestand AppDelegate.h, een verklaring omtrent het importeren toevoegen voor Notifications.h en een eigenschap voor een exemplaar van de klas meldingen toevoegen:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. In de methode **didFinishLaunchingWithOptions** in AppDelegate.m, de code als u wilt het exemplaar meldingen aan het begin van de methode geïnitialiseerd hebt toegevoegd.  
 
    `HUBNAME`en `HUBLISTENACCESS` (gedefinieerd in hubinfo.h) moet al het `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen vervangen door de naam van de hub van de melding en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Omdat referenties die worden geleverd met een client-app niet in het algemeen veilige zijn, moet u alleen de sleutel voor beluisteren van access met uw client-app distribueren. Access wordt ingeschakeld uw app om u te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd beluisteren en meldingen kunnen niet worden verzonden. De volledige toegang-toets wordt gebruikt in een beveiligde backend-service voor het verzenden van meldingen en bestaande registraties wijzigen.


9. In de methode **didRegisterForRemoteNotificationsWithDeviceToken** in AppDelegate.m, kunt u de code in de methode vervangen door de volgende code voor het doorgeven van het apparaat token aan de klas meldingen. De klas meldingen wordt uitgevoerd de registratie voor meldingen met de categorieën. Als categorieselecties voor de gebruiker wordt gewijzigd, verwijst naar de `subscribeWithCategories` methode in antwoord op de knop **abonneren** deze bijwerken.

    > [AZURE.NOTE] Omdat het apparaat token toegewezen door de Apple Push-meldingen Service (APNS) kunt op elk gewenst moment kans, dient u te registreren voor meldingen regelmatig om te voorkomen dat melding fouten. In dit voorbeeld wordt geregistreerd voor melding elke keer dat de app wordt gestart. Apps die vaak worden uitgevoerd voor kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie wilt behouden bandbreedte wanneer minder dan een dag is verstreken sinds de vorige registratie.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Houd er rekening mee dat op dit punt moet er geen andere code in de methode **didRegisterForRemoteNotificationsWithDeviceToken** .

10. De volgende methoden moeten al aanwezig zijn in AppDelegate.m uit het voltooien van de [aan de slag met melding Hubs] [ get-started] zelfstudie.  Als dit niet het geval is, wordt deze toevoegen.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Deze methode verwerkt meldingen ontvangen wanneer de app wordt uitgevoerd door een eenvoudige **UIAlert**weer te geven.

11. Het toevoegen van een importinstructie voor AppDelegate.h in ViewController.m, en kopieer de volgende code naar de methode XCode gegenereerde **abonneren** . Deze code wordt de registratie van de melding voor het gebruik van het nieuwe categorielabels voor die de gebruiker heeft gekozen in de gebruikersinterface bijwerken.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Deze methode Hiermee maakt u een **NSMutableArray** van categorieën en gebruikt de klas **meldingen** voor de opslag van de lijst in de lokale opslag en de bijbehorende labels registreert bij uw hub melding. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

12. Voeg de volgende code in de methode **viewDidLoad** om in te stellen van de gebruikersinterface op basis van de categorieën die eerder opgeslagen in ViewController.m.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



De app kan een set categorieën nu opgeslagen in de lokale opslag van apparaat wordt gebruikt om te registreren met de hub melding wanneer de app wordt gestart.  De gebruiker kan de selectie van categorieën gedurende runtime wijzigen en klik op de methode **abonneren** als u wilt bijwerken van de registratie voor het apparaat. Vervolgens wordt u de app als u wilt verzenden de recente nieuws meldingen rechtstreeks in de app zelf bijwerken.


##<a name="optional-sending-tagged-notifications"></a>(optioneel) Gelabelde meldingen verzenden

Als u geen toegang tot Visual Studio hebt, kunt u gaat u verder met het volgende gedeelte en meldingen verzenden vanuit de app zelf. U kunt de juiste sjabloon melding ook verzenden in de [Klassieke Azure-Portal] met behulp van het tabblad foutopsporing voor uw hub melding. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(optioneel) Meldingen van het apparaat verzenden

Normaal zou meldingen worden verzonden door een back-end-service, maar u kunt recente nieuws meldingen verzenden rechtstreeks vanuit de app. Hiervoor we worden bijgewerkt via de `SendNotificationRESTAPI` methode die we gedefinieerd in de [aan de slag met melding Hubs] [ get-started] zelfstudie.


1. In ViewController.m update de `SendNotificationRESTAPI` methode als volgt zodat deze accepteert een parameter voor de tag categorie en wordt de juiste [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) melding gestuurd.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. Werk in ViewController.m de actie **Verzenden melding** zoals wordt weergegeven in de code die volgt op. Zodat deze wordt elk naamkaartje afzonderlijk met berichten verzenden en naar meerdere platforms verzenden.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Uw project opnieuw en zorg ervoor dat er geen opbouwfouten.


##<a name="run-the-app-and-generate-notifications"></a>De app uitvoeren en meldingen genereren

1. Druk op de knop uitvoeren voor het bouwen van het project en start de app. Selecteer de opties van enkele recente nieuws abonneren op en druk vervolgens op de knop **abonneren** . Hier ziet u een dialoogvenster waarin wordt aangegeven dat de meldingen bent geabonneerd op.

    ![][1]

    Als u **abonneren**kiest, wordt de app zet om de geselecteerde categorieën in labels en vraagt een nieuwe apparaatregistratie voor de geselecteerde codes van de hub melding.

2. Typ een bericht om te worden verzonden als laatste nieuws en druk op de knop **Bericht verzenden** . U kunt ook de .NET-console-app om meldingen te genereren uitvoeren.

    ![][2]


3. Elk apparaat geabonneerd op belangrijk nieuws, ontvangt de recente nieuws meldingen die u zojuist hebt gestuurd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u het laatste nieuws uitzenden per categorie. Houd rekening met het voltooien van een van de volgende zelfstudies die andere geavanceerde melding Hubs-scenario's markeren:

+ **[Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]**

    Leer hoe u de app recente nieuws om in te schakelen verzendende gelokaliseerde meldingen uitvouwen.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klassieke Portal]: https://manage.windowsazure.com
