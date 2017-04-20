<properties
    pageTitle="Azure melding Hubs RTF Push"
    description="Leer hoe u uitgebreide push-meldingen verzenden naar een iOS-app van Azure. Voorbeelden van de code in de doel-C en C# geschreven."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure melding Hubs RTF Push


##<a name="overview"></a>Overzicht

Om te sluiten gebruikers met direct uitgebreide inhoud, eventueel een toepassing push voorbij tekst zonder opmaak. Deze meldingen reclame maken voor gebruikersinteracties en inhoud presenteren zoals URL's en geluiden, afbeeldingen/coupons enzovoort. Deze zelfstudie is gebaseerd op het onderwerp van de [Gebruikers een melding](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) en ziet u hoe u het verzenden van push-meldingen met nettoladingen (bijvoorbeeld afbeelding).


Deze zelfstudie is compatibel met iOS 7 en 8.

  ![][IOS1]

Op hoog niveau:

1. De app-end:
    - De uitgebreide nettolading opgeslagen (in dit geval afbeelding) in de back-end database/lokaal opslagruimte
    - ID van deze uitgebreide melding verzendt naar het apparaat
2. De App op het apparaat:
    - De backend aanvragen van de uitgebreide nettolading met de ID die deze ontvangt van contactpersonen
    - Gestuurd gebruikers op het apparaat wanneer ophalen van gegevens voltooid is, en ziet u de nettolading onmiddellijk wanneer gebruikers tikken voor meer informatie


## <a name="webapi-project"></a>WebAPI Project

1. Open het project **AppBackend** die u hebt gemaakt in deze zelfstudie [Gebruikers een melding](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) in Visual Studio.
2. Een afbeelding die u wilt gebruikers met een melding en zet dit in een map **img** in het telefoonboek van uw project aanvragen.
3. Klik op **Alle bestanden weergeven** in de Verkenner oplossing en met de rechtermuisknop op de map wilt **Opnemen In Project**.
4. Wijzigen met de afbeelding is geselecteerd en de actie maken in venster Eigenschappen aan **Ingesloten Resource**.

    ![][IOS2]

5. Voeg het volgende in **Notifications.cs**, met instructie:

        using System.Reflection;

6. De hele **meldingen** klas bijwerken met de volgende code. Zorg ervoor dat u de tijdelijke aanduidingen vervangen door uw melding hub referenties en de bestandsnaam van de afbeelding.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (optioneel) Raadpleeg [het insluiten van en toegang tot bronnen met behulp van Visual C#](http://support.microsoft.com/kb/319292) voor meer informatie over het toevoegen en projectbronnen krijgen.

7. Definiëren in **NotificationsController.cs**, **NotificationsController** met de volgende fragmenten. Hiermee wordt een id eerste stille uitgebreide melding verzonden naar apparaat en kunt aan de clientzijde voor het ophalen van afbeelding:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Nu implementeert we deze app aan een Website Azure opnieuw zodat u toegankelijk zijn vanuit alle apparaten. Met de rechtermuisknop op het project **AppBackend** en selecteer **publiceren**.

9. Selecteer Azure Website als het doel-publiceren. Meld u aan met uw Azure-account en selecteer een bestaande of nieuwe Website en noteert u de eigenschap **doel-URL** in het tabblad **verbinding** . Wordt verwezen naar deze URL als de *backend-eindpunt* verderop in deze zelfstudie. Klik op **publiceren**.

## <a name="modify-the-ios-project"></a>Het iOS-project wijzigen

Nu dat u uw app backend als u wilt verzenden, alleen de *id* van een melding hebt gewijzigd, kunt u uw iOS-app om te verwerken die id en het uitgebreide bericht op te halen uit uw backend wordt gewijzigd.

1. Open uw iOS-project en externe meldingen inschakelen door te gaan naar de doelsite belangrijkste app in de sectie **doelen** .

2. Klik op **mogelijkheden**, **Achtergrond modi**inschakelen en controleren het selectievakje **Externe meldingen** .

    ![][IOS3]

3. Ga naar **Main.storyboard**en zorg ervoor dat u een weergave Controller (zoals als Home weergave Controller in deze zelfstudie) van de [Gebruiker inlichten](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) zelfstudie hebt.

4. Een **Navigatie Controller** toevoegen aan uw storyboard en control ingedrukt houden en op Home weergave Controller zodat u de **weergave van de hoofdsite** van navigatie. Controleer of dat de **Eerste weergave Controller Is** in kenmerken controle voor alleen de navigatie-Controller is geselecteerd.

5. Een **Weergave Controller** om regisseer toe te voegen van een **Afbeeldingsweergave**toevoegen. Dit is de pagina gebruikers zien wanneer ze voor meer informatie door te klikken op de notifiication kiezen. Uw storyboard ziet er als volgt uit:

    ![][IOS4]

6. Klik op de **Home weergave Controller** in storyboard en controleert u of er **homeViewController** als de **Aangepaste klasse** en de **Storyboard-ID** onder de controle identiteit.

7. Doe hetzelfde voor afbeelding van de weergave Controller als **imageViewController**.

8. Maak een nieuwe weergave Controller-klasse getiteld **imageViewController** u omgaat met de gebruikersinterface die u zojuist hebt gemaakt.

9. In **imageViewController.h**, moet u de volgende toevoegen aan van de besturing interface declaraties. Zorg ervoor dat besturingselement en sleep vanuit de weergave van de afbeelding storyboard naar deze eigenschappen om te koppelen van de twee:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Voeg de volgende aan het einde van **viewDidload**in **imageViewController.m**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. In **AppDelegate.m**, importeert u de afbeelding controller die u hebt gemaakt:

        #import "imageViewController.h"

12. Een sectie interface met het volgende declaratie toevoegen:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, Controleer of in uw app registreert voor stille meldingen in **toepassing: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Subsitute in de volgende uitvoering voor de **toepassing: didRegisterForRemoteNotificationsWithDeviceToken** uitvoeren van het storyboard UI gewijzigd in aanmerking:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. De volgende methoden vervolgens toevoegen aan **AppDelegate.m** voor het ophalen van de afbeelding uit uw eindpunt en een lokale bericht verzenden wanneer opgehaald zijn. Zorg ervoor dat de tijdelijke aanduiding vervangen `{backend endpoint}` met uw back-end-eindpunt:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Het lokale bericht hierboven afhandelen door te openen de controller van de afbeelding van de-weergave in **AppDelegate.m** met de volgende methoden:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Voer de toepassing

1. In XCode, moet u de app uitvoeren op een fysieke iOS-apparaat (push-meldingen niet in de simulator werkt).

2. In de iOS-app UI een gebruikersnaam en wachtwoord van dezelfde waarde voor verificatie en klik op **Log In**.

3. Klik op **verzenden push** en ziet u een melding in-app. Als u op **meer**klikt, wordt u naar de afbeelding die u wilt opnemen in uw app backend worden gebracht.

4. U kunt ook klikken op **push verzenden** en druk onmiddellijk op de knop Start van uw apparaat. In een paar seconden, ontvangt u een push-bericht. Als u erop tikt of klikt u op meer, wordt u in uw app en de inhoud van de uitgebreide afbeelding gebracht.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
