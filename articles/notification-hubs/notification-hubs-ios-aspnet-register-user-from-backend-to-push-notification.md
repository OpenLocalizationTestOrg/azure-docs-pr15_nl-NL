<properties
    pageTitle="De huidige gebruiker voor push-meldingen hebt geregistreerd met behulp van Web API | Microsoft Azure"
    description="Meer informatie over het aanvragen van registratie van push-meldingen in een iOS-app met Azure melding Hubs wanneer registeration wordt uitgevoerd door ASP.NET Web API."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>De huidige gebruiker voor push-meldingen hebt geregistreerd met behulp van ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u de registratie van push-meldingen met Azure melding Hubs aanvragen bij registratie wordt uitgevoerd door ASP.NET Web API. In dit onderwerp breidt de zelfstudie [Waarschuwen gebruikers met melding Hubs]. U moet zijn al voltooid de vereiste stappen in deze zelfstudie te maken van de geverifieerde telefoonprovider. Zie voor meer informatie over het waarschuwen gebruikers scenario's [Waarschuwen gebruikers met melding Hubs].

##<a name="update-your-app"></a>Uw app bijwerken  

1. In de objectbibliotheek van uw MainStoryboard_iPhone.storyboard, toevoegen de volgende onderdelen:

    + **Label**: "Push aan gebruiker met Hubs melding"
    + **Label**: "Installatie-id"
    + **Label**: "Gebruiker"
    + **Tekstveld**: "Gebruiker"
    + **Label**: "Wachtwoord"
    + **Tekstveld**: "Wachtwoord"
    + **Knop**: "Login"

    Nu uw storyboard ziet er als volgt uit:

    ![][0]

2. Klik in de editor assistent wandcontactdozen voor alle switched besturingselementen maken en gebeld, verbinding maken met de tekstvelden met de weergave Controller uit (gemachtigde) en een **actie** voor de knop **aanmelden** maken.

    ![][1]

    Uw bestand BreakingNewsViewController.h bevat nu de volgende code:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Maak een klasse met de naam **DeviceInfo**en kopieer de volgende code in de sectie interface van het bestand DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopieer de volgende code in de sectie implementatie van het bestand DeviceInfo.m:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. In PushToUserAppDelegate.h, de volgende eigenschap singleton items toevoegen:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. In de methode **didFinishLaunchingWithOptions** in PushToUserAppDelegate.m, de volgende code hebt toegevoegd:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    De eerste regel wordt de singleton **DeviceInfo** geïnitialiseerd. De tweede regel begint de registratie voor push-meldingen, die al aanwezig is dat u de zelfstudie [Aan de slag met melding Hubs] al hebben uitgevoerd.

9. De methode **didRegisterForRemoteNotificationsWithDeviceToken** implementeren in uw AppDelegate in PushToUserAppDelegate.m, en voeg de volgende code:

        self.deviceInfo.deviceToken = deviceToken;

    Hiermee stelt u het apparaat token voor het verzoek.

    > [AZURE.NOTE] In dit stadium moet niet er een andere code in deze methode. Als u al een gesprek met de methode **registerNativeWithDeviceToken** die is toegevoegd als u de zelfstudie [Aan de slag met melding Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) hebt voltooid, moet u Opmerking-out of verwijderen van dat gesprek.

10. In het bestand PushToUserAppDelegate.m, voegt u de volgende handler methode:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Deze methode wordt een melding weergegeven in de gebruikersinterface, terwijl uw app meldingen ontvangt terwijl deze wordt uitgevoerd.

9. Open het bestand PushToUserViewController.m en het toetsenbord te retourneren in de volgende implementatie:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. In de methode **viewDidLoad** in het bestand PushToUserViewController.m geïnitialiseerd u als volgt het label de installatie-id:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. De volgende eigenschappen interface in PushToUserViewController.m toevoegen:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Vervolgens voegt u de volgende implementatie:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopieer de volgende code naar de **login** handler-methode gemaakt door XCode.

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Deze methode wordt een installatie-ID en de kanaal voor push-meldingen en verzendt, samen met het apparaattype, met de geverifieerde Web API-methode die een registratie in de melding Hubs maakt. Deze Web API is gedefinieerd in [Waarschuwen gebruikers met melding Hubs].

Nu dat de client-app is bijgewerkt, Ga terug naar de [Waarschuwen gebruikers met melding Hubs] en de mobiele service om meldingen te verzenden met behulp van de melding Hubs bijwerken.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Gebruikers met melding Hubs hoogte]: /manage/services/notification-hubs/notify-users-aspnet

[Aan de slag met melding Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
