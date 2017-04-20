<properties
    pageTitle="Azure melding Hubs Secure Push"
    description="Leer hoe u de beveiligde push-meldingen verzenden naar een iOS-app van Azure. Voorbeelden van de code in de doel-C en C# geschreven."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure melding Hubs Secure Push

> [AZURE.SELECTOR]
- [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-apparaat](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Overzicht

Ondersteuning voor push-meldingen in Microsoft Azure krijgt u toegang tot een eenvoudig te gebruiken, meerdere platforms, schaal-out push-infrastructuur, waardoor enorm eenvoudiger de uitvoering van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms.

Vanwege wettelijke of beveiligingsbeperkingen, soms een toepassing mogelijk moeten worden opgenomen iets in de melding die niet kan worden overgebracht via de standaard push notification-infrastructuur. Deze zelfstudie wordt beschreven hoe de dezelfde ervaring bereiken door te sturen vertrouwelijke gegevens door een beveiligde, geverifieerde verbinding tussen het clientapparaat worden gebruikt en de app-end.

Op hoog niveau is de stroom als volgt uit:

1. De app back-enddatabase:
    - Winkels secure nettolading in back-enddatabase.
    - Hiermee worden de ID van deze melding bij het apparaat (er worden geen gegevens veilig is verzonden).
2. De app op het apparaat, wanneer u de melding ontvangt:
    - Het apparaat contact op met de back-enddatabase aanvragen van de beveiligde nettolading.
    - De app kunt u de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), wordt ervan uitgegaan dat het apparaat een verificatietoken in de lokale opslag, opslaat nadat de gebruiker zich aanmeldt. Dit zorgt ervoor een volledig naadloze oplossing, terwijl het apparaat van de melding secure payload met deze token kunt ophalen. Als uw toepassing slaat geen verificatietokens op het apparaat, of als deze tokens kunnen worden verlopen, moet een algemene melding dat de gebruiker wordt gevraagd naar de app Start de app apparaat bij de melding worden weergegeven. De app wordt geverifieerd door de gebruiker vervolgens en ziet u de melding nettolading.

Deze zelfstudie Secure Push ziet u hoe u een melding push veilig verzendt. Dit is de zelfstudie die is gebaseerd op de zelfstudie [Gebruikers waarschuwen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , zodat u moet eerst de stappen in deze zelfstudie uitvoeren.

> [AZURE.NOTE] Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en configureren van uw melding-hub, zoals beschreven in [Aan de slag met melding Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Het iOS-project wijzigen

Nu dat u uw app-back-enddatabase in als u wilt verzenden, alleen de *id* van een melding hebt gewijzigd, moet u uw iOS-app u deze melding voor uw back-enddatabase om op te halen de beveiligd bericht wordt weergegeven voor terugbellen wijzigen.

Als u wilt realiseren, moeten we schrijven van de logica voor het ophalen van de beveiligde inhoud uit de app-back-enddatabase.

1. Controleer of de app kassa's voor stille meldingen zodat de id van de melding worden verwerkt verzonden vanuit de backend in **AppDelegate.m**. De optie **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions toevoegen:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. In uw **AppDelegate.m** voegt u een sectie implementatie bovenaan met de volgende declaratie toe:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Klik in de sectie implementatie de volgende code toevoegen, wordt vervangen door de tijdelijke aanduiding `{back-end endpoint}` met het eindpunt voor uw back-enddatabase eerder verkregen:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Nu hebben we de binnenkomende melding verwerken met behulp van de bovenstaande methode en het ophalen van de inhoud wordt weergegeven. We hebben eerst uw iOS-app op de achtergrond wordt uitgevoerd wanneer een push-meldingen ontvangen inschakelen. In **XCode**, selecteert u uw app-project in het linker deelvenster, klik op het doel van de belangrijkste app in de sectie **doelen** uit het centrale deelvenster.

5. Vervolgens klikt u op het tabblad **mogelijkheden** boven aan uw centraal deelvenster en schakel het selectievakje **Externe meldingen** .

    ![][IOS1]


6. In **AppDelegate.m** toevoegen de volgende methode voor het verwerken van push-meldingen:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Houd er rekening mee bij voorkeur voor het geval van ontbrekende verificatie koptekst eigenschap of afkeuring door de back-enddatabase. De specifieke afhandeling van deze zaken afhankelijk voornamelijk van de gebruikerservaring voor de doeltoepassing. Eén optie is om een melding met een algemene prompt voor de gebruiker om te verifiëren om op te halen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>Voer de toepassing

Ga als volgt te werk om de toepassing uitvoert:

1. In XCode, moet u de app uitvoeren op een fysieke iOS-apparaat (push-meldingen niet in de simulator werkt).

2. Voer een gebruikersnaam en wachtwoord in de iOS-app UI. Deze kunnen een willekeurige tekenreeks, maar ze moeten hetzelfde resultaat.

3. In de iOS-app UI, klikt u op **aanmelden**. Klik vervolgens op **push verzenden**. Hier ziet u de beveiligde melding wordt weergegeven in uw beheercentrum melding.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
