<properties
    pageTitle="Melding Hubs gelokaliseerd verbreken nieuws zelfstudie voor iOS"
    description="Informatie over het gebruik van Azure Service Bus melding Hubs gelokaliseerde recente nieuws meldingen (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Melding Hubs gelokaliseerde laatste nieuws versturen naar iOS-apparaten gebruiken

> [AZURE.SELECTOR]
- [Windows Store-C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u het gebruik van de functie [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) van Azure melding Hubs uitzenden recente nieuws meldingen die door de taal- en apparaat zijn aangepast. In deze zelfstudie begint u met de iOS-app in [Gebruik melding Hubs naar het laatste nieuws verzenden]hebt gemaakt. Als alles compleet is, dat is mogelijk registreren voor categorieën waarin u geïnteresseerd bent, Geef een taal waarin u de meldingen wilt ontvangen en ontvangt alleen push-meldingen voor de geselecteerde categorieën in die taal.


Er zijn twee onderdelen dit scenario:

- iOS-app kan client apparaten om op te geven van een taal en zich kunnen abonneren op verschillende recente nieuwscategorieën;

- de back-enddatabase verzendt de meldingen, met het **label** - en **sjabloon** funcites van Azure melding Hubs.



##<a name="prerequisites"></a>Vereisten voor

U moet al hebben uitgevoerd de zelfstudie [Gebruik melding Hubs naar het laatste nieuws verzenden] en hebt u de code die beschikbaar zijn, omdat deze zelfstudie gebaseerd rechtstreeks op die code.

Visual Studio 2012 of later is optioneel.



##<a name="template-concepts"></a>Sjabloon concepten

In [Gebruik melding Hubs naar het laatste nieuws verzenden] ingebouwd u een app die **labels** gebruikt om u te abonneren op meldingen voor verschillende nieuwscategorieën.
Veel apps, meerdere markten doelgerichte echter en lokalisatie vereisen. Dit betekent dat de inhoud van de meldingen zelf hoeft te worden gelokaliseerd en verzonden naar de juiste reeks apparaten.
In dit onderwerp wordt wordt uitgelegd hoe de functie te gebruiken **sjabloon** van Hubs melding voor het leveren van eenvoudig gelokaliseerde recente nieuws meldingen.

Opmerking: een manier om gelokaliseerde meldingen te verzenden is het opzetten van meerdere versies van elk naamkaartje. Bijvoorbeeld ter ondersteuning van Engels, Frans en Mandarijn, zou moeten we drie verschillende labels voor wereldnieuws: "world_en", "world_fr," en "world_ch". Zou moeten we vervolgens een gelokaliseerde versie van de wereldnieuws verzenden naar elk van deze tags. In dit onderwerp we sjablonen gebruiken om te voorkomen dat de verspreiding van tags en de vereiste meerdere berichten te verzenden.

Sjablonen zijn op hoog niveau, een manier om te bepalen hoe een bepaald apparaat een melding moet ontvangen. De sjabloon geeft de exacte nettolading-indeling door te verwijzen naar de eigenschappen die deel van het bericht is verzonden door uw app back-enddatabase uitmaken. In ons geval stuurt we een landinstelling-agnostic-bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Klik we ervoor dat zorgt dat apparaten met een sjabloon die naar de juiste eigenschap verwijst registreren. Een iOS-app die wil registreren voor Franse nieuws registreren bijvoorbeeld het volgende:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Sjablonen zijn krachtige functies die kunt u meer informatie over in onze artikel [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>De app-gebruikersinterface

We wordt nu de verbreken nieuws-app die u hebt gemaakt in het onderwerp [Gebruik melding Hubs naar het laatste nieuws verzenden] om te verzenden gelokaliseerde laatste nieuws met sjablonen wijzigen.


In uw MainStoryboard_iPhone.storyboard, Voeg een gesegmenteerde besturingselement met de drie talen die we biedt ondersteuning: Engels, Frans en Mandarijn.

![][13]

Zorg ervoor mag een IBOutlet toevoegen aan uw ViewController.h, zoals hieronder wordt weergegeven:

![][14]

##<a name="building-the-ios-app"></a>De iOS-app maken


1. Voeg de methode *retrieveLocale* in uw Notification.h en wijzigen van de store en methoden abonneren, zoals hieronder wordt weergegeven:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Wijzig de methode *storeCategoriesAndSubscribe* door de parameter landinstelling toe te voegen en deze opslaan in de standaardinstellingen voor de gebruiker in uw Notification.m:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Wijzig de methode *abonneren* als u wilt de landinstelling:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Houd er rekening mee hoe nu gebruiken we de methode *registerTemplateWithDeviceToken*, in plaats van *registerNativeWithDeviceToken*. We hebben de json-sjabloon en ook een naam voor de sjabloon opgeven (zoals onze app verschillende sjablonen registreren wilt mogelijk) wanneer we voor een sjabloon registreren. Zorg ervoor dat uw categorieën registreren als labels, zoals we horen om te controleren of de notifciations voor deze nieuws ontvangen.

    Een methode voor het ophalen van de landinstelling van de standaardinstellingen voor de gebruiker toevoegen:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Nu we onze klasse meldingen hebt gewijzigd, we nog om ervoor te zorgen dat onze ViewController gebruikmaakt van de nieuwe UISegmentControl. Voeg de volgende regel in de methode *viewDidLoad* om ervoor te zorgen om weer te geven van de landinstelling die momenteel is geselecteerd:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Vervolgens in uw methode *zich hebt geabonneerd* , wijzigt u de oproep door naar de *storeCategoriesAndSubscribe* als volgt uit:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. U moet ten slotte bijwerken van de methode *didRegisterForRemoteNotificationsWithDeviceToken* in uw AppDelegate.m, zodat u uw registratie correct vernieuwen kunt wanneer de app wordt gestart. Wijzig de oproep door naar de methode *abonneren* van meldingen met de volgende items:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen verzenden vanuit .NET-console-app.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(optioneel) Gelokaliseerde Sjabloonmeldingen van het apparaat verzenden

Als u geen toegang tot Visual Studio hebt, of gewoon wilt testen verzenden van de Sjabloonmeldingen gelokaliseerde rechtstreeks vanuit de app op het apparaat.  U kunt eenvoudig toevoegen van de sjabloonparameters gelokaliseerde voor de `SendNotificationRESTAPI` methode die u in de eerdere zelfstudie hebt gedefinieerd.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van sjablonen:

- [Gebruikers met melding Hubs hoogte: ASP.NET]
- [Gebruikers met melding Hubs hoogte: Mobile-Services]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Laatste nieuws verzenden via melding Hubs]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Gebruikers met melding Hubs hoogte: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Gebruikers met melding Hubs hoogte: Mobile-Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
