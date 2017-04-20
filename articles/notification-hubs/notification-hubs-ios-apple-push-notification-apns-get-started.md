<properties
    pageTitle="Push-meldingen verzenden naar iOS met Azure melding Hubs | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen verzenden naar een iOS-toepassing."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="push-meldingen, push-meldingen, ios push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Push-meldingen voor iOS met Azure melding Hubs verzenden

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started)voor meer informatie.

Deze zelfstudie wordt getoond hoe u met Azure melding Hubs push-meldingen verzenden naar een iOS-toepassing. U kunt een lege iOS-app die push-meldingen ontvangt via de [Apple Push Notification-service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)wilt maken. 

Wanneer u klaar bent, kunt u zult gebruikmaken van uw hub melding uitzenden van push-meldingen naar alle apparaten die uw app uitvoeren.

## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kunt vinden [op GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ [Mobile-Services iOS SDK versie 1.2.4]
+ Meest recente versie van [Xcode]
+ Een iOS 8 (of een latere versie)-apparaat
+ Lidmaatschap van [Apple ontwikkelaars programma](https://developer.apple.com/programs/) .

   > [AZURE.NOTE] Vanwege de configuratievereisten voor push-meldingen, moet u implementeren en testen van push-meldingen op een fysieke iOS-apparaat (iPhone of iPad) in plaats van de iOS Simulator.

Voltooien van deze zelfstudie is vereist voor alle andere meldingen Hubs zelfstudies voor iOS-apps.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>De melding Hub voor voor iOS push-meldingen configureren

In dit gedeelte begeleidt u bij het maken van een nieuwe melding hub en verificatie configureren met APNS met behulp van de **.p12** push-certificaat dat u hebt gemaakt. Als u gebruiken van een melding hub die u al hebt gemaakt wilt, kunt u verder met stap 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Klik op de knop <b>Notification Services</b> in het blad <b>Instellingen</b> en selecteer <b>Apple (APNS)</b>. Klik op <b>Certificaat uploaden</b> en selecteer het <b>.p12</b> -bestand dat u eerder hebt geëxporteerd. Zorg ervoor dat u ook het juiste wachtwoord opgeven.</p>
<p>Zorg ervoor dat <b>Sandbox</b> -modus omdat dit is bedoeld voor de ontwikkeling. De <b>productie</b> alleen gebruiken als u wilt verzenden push-meldingen aan gebruikers die uw app in de winkel gekocht.</p>
</li>
</ol>
&emsp;&emsp;![APNS configureren in Azure-Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![APNS-certificaat configureren in Azure-Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Uw hub melding is nu geconfigureerd voor het werken met APNS en u de verbindingstekenreeksen naar uw app hebt geregistreerd en het verzenden van push-meldingen hebt.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Uw iOS-app verbinden met melding Hubs

1. Maak een nieuw project van iOS in Xcode, en selecteert u de sjabloon **Één weergave-toepassing** .

    ![Xcode - toepassing één weergeven][8]

2. Wanneer u de opties voor het nieuwe project, zorg er dan voor dat dezelfde **Productnaam** en **Organisatie-id** die u gebruikt wanneer u de bundel-ID in de Apple Developer portal eerder hebt ingesteld.

    ![Xcode - project-opties][11]

3. Onder **doelen**, klikt u op de naam van uw project, klik op het tabblad **Instellingen voor het maken** en **Code ondertekening identiteit**uitvouwen en stel vervolgens onder **Foutopsporing**, uw identiteit ondertekening van programmacode. **Niveaus** van **eenvoudig** naar **alle**in-of uitschakelen en **Inrichting profiel** ingesteld op het inrichten profiel dat u eerder hebt gemaakt.

    Als u het nieuwe inrichten profiel die u hebt gemaakt in Xcode niet ziet, kunt u Vernieuw de profielen voor uw identiteit ondertekend. **Xcode** Klik op de menubalk op **Voorkeuren**, klik op het tabblad **Account** , klikt u op de knop **Details weergeven** , klikt u op uw ondertekend identiteit, en klik vervolgens op de knop Vernieuwen in de rechterbenedenhoek.

    ![Xcode - inrichten profiel][9]

4. Download de [Mobile-Services iOS SDK versie 1.2.4] en pak het bestand. Xcode, met de rechtermuisknop op het project en klik op de optie **Bestanden toevoegen aan** de map **WindowsAzureMessaging.framework** toevoegen aan uw project Xcode. Selecteer **items desgewenst kopiëren**en klik vervolgens op **toevoegen**.

    >[AZURE.NOTE] De melding hubs SDK niet ondersteunt momenteel bitcode op Xcode 7.  U moet **Bitcode inschakelen** ingesteld op **Nee** in de **Opties voor het maken** voor uw project.

    ![Pak Azure SDK][10]

5. Een nieuw bestand in de koptekst toevoegen aan uw project met de naam `HubInfo.h`. Dit bestand wordt de constanten voor uw hub melding ingevoerd.  Voeg de volgende definities en de tijdelijke aanduidingen voor letterlijke tekenreeks vervangen door de *hubnaam* en de *DefaultListenSharedAccessSignature* die u eerder hebt genoteerd.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Open uw `AppDelegate.h` bestand toevoegen de volgende richtlijnen voor importeren:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. In uw `AppDelegate.m file`, voeg de volgende code in de `didFinishLaunchingWithOptions` methode op basis van uw versie van iOS. Deze code registreert de greep van uw apparaat met APNs:

    Voor iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Voor iOS-versies vóór 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. In hetzelfde bestand toevoegen de volgende methoden. Deze code maakt verbinding met de melding hub met de verbindingsgegevens die u hebt opgegeven in HubInfo.h. Vervolgens wordt het token apparaat op de melding-hub zodat de hub melding meldingen kunt verzenden:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. In hetzelfde bestand, voegt u de volgende methode om weer te geven van een **UIAlert** als de melding wordt ontvangen wanneer de app geopend is:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Maken en uitvoeren van de app op uw apparaat om te bevestigen dat er geen fouten.

## <a name="send-test-push-notifications"></a>Test push-meldingen verzenden


U kunt testen meldingen ontvangen in uw app door te sturen van push-meldingen in de [Portal van Azure] via het gedeelte **Probleemoplossing** in het blad hub (Gebruik de optie *Testen verzenden* ).

![Azure-Portal - Test verzenden][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Optioneel) Push-meldingen verzenden vanuit de app

>[AZURE.IMPORTANT] In dit voorbeeld uit het verzenden van meldingen van de client-app is opgegeven voor alleen leermateriaal. Aangezien dit houdt in dat de `DefaultFullSharedAccessSignature` als u wilt presenteren op de client-app, de melding hub naar het risico dat een gebruiker kan toegang tot gegevens door onbevoegden meldingen verzenden naar uw klanten worden getoond.

Als u verzenden push-meldingen van een app wilt, wordt in deze sectie een voorbeeld van hoe u dit wilt doen met de REST-interface bevat.

1. Open in Xcode, `Main.storyboard` en de volgende onderdelen van de gebruikersinterface van de objectbibliotheek zodat de gebruiker push-meldingen verzenden in de app toevoegen:

    - Een label zonder label-tekst. Deze worden gebruikt om fouten te melden bij het verzenden van meldingen. De eigenschap **lijnen** moet worden ingesteld op **0** zodat deze automatisch wordt grootte beperkt tot de rechter- en linkermarge en de bovenkant van de weergave.
    - Een tekstveld met **tijdelijke aanduiding voor** tekst is ingesteld op **Meldingsbericht invoeren**. Het veld net onder het label beperken zoals hieronder wordt weergegeven. De Controller weergave instellen als de gemachtigde winkel.
    - Een knop **Verzenden melding** getiteld beperkt net onder het tekstvak en klik in het horizontale middelpunt.

    De weergave ziet er als volgt uit:

    ![Xcode designer][32]


2. Voor het veld label en tekst [toevoegen wandcontactdozen](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) verbonden uw weergave, en bij te werken uw `interface` definitie ter ondersteuning van `UITextFieldDelegate` en `NSXMLParserDelegate`. Voeg de drie eigenschapdeclaraties helpt bij de ondersteuning voor het bellen van de REST API en parseren van het antwoord hieronder wordt weergegeven.

    Uw bestand ViewController.h ziet er als volgt uit:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Open `HubInfo.h` en de volgende constanten die worden gebruikt voor het verzenden van meldingen op uw hub toevoegen. De tijdelijke aanduiding voor tekenreeks vervangen door de werkelijke *DefaultFullSharedAccessSignature* -verbindingsreeks.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Voeg de volgende `#import` instructies naar uw `ViewController.h` bestand.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. In `ViewController.m` de volgende code toevoegen aan de interface-implementatie. Deze code wordt de verbindingsreeks *DefaultFullSharedAccessSignature* parseren. Zoals in de [REST API verwijzing](http://msdn.microsoft.com/library/azure/dn495627.aspx)is vermeld, wordt deze geparseerde gegevens worden gebruikt om een token SA's voor de kop van de aanvraag **autorisatie** te genereren.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. In `ViewController.m`, werken de `viewDidLoad` methode de verbindingsreeks parseren wanneer de weergave wordt geladen. Ook toevoegen de nuttige methoden, hieronder, naar de interface-implementatie.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. In `ViewController.m`, de volgende code toevoegen aan de interface-implementatie de SA's autorisatie om token te genereren die u in de koptekst **autorisatie** zoals in de [REST API verwijzing](http://msdn.microsoft.com/library/azure/dn495627.aspx)is vermeld.

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + Sleep vanuit de **Melding verzenden** naar de knop naar `ViewController.m` een benoemde **SendNotificationMessage** voor de gebeurtenis **Raak omlaag** actie toevoegen. Methode bijwerken met de volgende code aan wie de melding dat de REST API gebruiken.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. In `ViewController.m`, voegt u de volgende gemachtigdenmethode ter ondersteuning van het toetsenbord voor het tekstveld sluiten. CTRL + Sleep vanuit het tekstvak naar de weergave Controller-pictogram in de interface-designer om de controller weergave instellen als de gemachtigde winkel.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. In `ViewController.m`, toevoegen de volgende methoden gemachtigde om te ondersteunen parseren van het antwoord met behulp van `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Het project bouwen en controleer of er geen fouten.


> [AZURE.NOTE] Als u een fout opbouwen in Xcode7 over de ondersteuning van bitcode, moet u de **Instellingen voor het maken**van de > **Inschakelen Bitcode (ENABLE_BITCODE)** op **Nee** in Xcode. De melding Hubs SDK ondersteunt momenteel niet bitcode. 

U kunt alle de mogelijke melding nettoladingen vinden in de Apple [lokaal en Push-meldingen Programming Guide].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Als uw app push-meldingen kan ontvangen controleren

Als u wilt testen push-meldingen op iOS, moet u de app implementeren naar een fysieke iOS-apparaat. U kunt geen Apple push-meldingen verzenden met behulp van de iOS Simulator.

1. De app uitvoeren en controleer of registratie is geslaagd, en druk vervolgens op **OK**.

    ![iOS-App Push-meldingen registratie testen][33]

2. U kunt een test push-bericht verzenden vanuit de [Azure-Portal], zoals hierboven is beschreven. Als u de code voor het verzenden van push-meldingen in de app hebt toegevoegd, kunt u Raak in het veld voor tekst om in te voeren Meldingsbericht voor een. Druk op de knop **verzenden** op het toetsenbord of de knop **Bericht verzenden** in de weergave voor het waarschuwingsbericht verzenden.

    ![iOS-App Push-meldingen Test verzenden][34]

3. De push-bericht wordt verzonden naar alle apparaten die zijn geregistreerd om de berichten van de Hub met bepaalde melding wilt ontvangen.

    ![iOS-App Push-meldingen ontvangen testen][35]


##<a name="next-steps"></a>Volgende stappen

In dit voorbeeld eenvoudige uitgezonden u push-meldingen voor al uw geregistreerde iOS-apparaten. Wordt u aangeraden een volgende stap in begrepen die u gaat u verder met de zelfstudie [Azure melding Hubs gebruikers waarschuwen voor iOS met .NET-end] , die u helpt bij het maken van een back-end om te verzenden push-meldingen-labels gebruiken. 

Als u wilt dat uw gebruikers door groepen rente in segmenten, kunt u bovendien op verplaatsen naar de zelfstudie [Gebruik melding Hubs naar het laatste nieuws verzenden] . 

Zie [Melding Hubs richtlijnen]voor algemene informatie over melding Hubs.



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile-Services iOS SDK versie 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Melding Hubs richtlijnen]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure melding Hubs gebruikers waarschuwen voor iOS met .NET-end]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Laatste nieuws verzenden via melding Hubs]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Lokale en Push-meldingen hoeft te programmeren handleiding]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure-Portal]: https://portal.azure.com