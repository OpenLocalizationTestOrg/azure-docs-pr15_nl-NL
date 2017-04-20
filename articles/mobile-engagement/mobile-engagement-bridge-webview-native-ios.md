<properties 
    pageTitle="Brug iOS webweergave met systeemeigen Mobile betrokkenheid iOS SDK" 
    description="Wordt beschreven hoe u een brug tussen webweergave met Javascript en de systeemeigen Mobile betrokkenheid iOS SDK maken"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Brug iOS webweergave met systeemeigen Mobile betrokkenheid iOS SDK

> [AZURE.SELECTOR]
- [Brug voor android](mobile-engagement-bridge-webview-native-android.md)
- [iOS-brug](mobile-engagement-bridge-webview-native-ios.md)

Sommige mobiele apps zijn ontworpen als een app hybride waar de app zelf is ontwikkeld met behulp van systeemeigen iOS doelstelling-C ontwikkeling, maar sommige of zelfs alle schermen binnen een iOS webweergave worden weergegeven. U kunt nog steeds gebruiken Mobile betrokkenheid iOS SDK binnen deze apps en deze zelfstudie wordt beschreven hoe u gaat hierover. 

Er zijn twee manieren hiervoor Hoewel beide niet gedocumenteerd zijn:

- Eerst een wordt beschreven op deze [koppeling](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) waarbij registreren een `UIWebViewDelegate` op uw webweergave en variabel-en-direct-annuleren een locatie wijzigen in Javascript gedaan. 
- Tweede is een op basis van deze [WWDC 2013 sessie](https://developer.apple.com/videos/play/wwdc2013/615), een aanpak die duidelijkere dan de eerste en die we voor deze handleiding volgen. Houd er rekening mee dat deze methode alleen op iOS7 en hoger werkt. 

Volg de onderstaande stappen voor de iOS brug voorbeeld:

1. Ten eerste moet u ervoor zorgen dat u onze [aan de slag zelfstudie](mobile-engagement-ios-get-started.md) kunt integreren de betrokkenheid van mobiele iOS SDK in uw app hybride hebt doorlopen. (Optioneel) kunt u ook test logboekregistratie inschakelen als volgt zodat u kunt de methoden SDK zien kunt, zoals we zo activeren dat deze de webweergave. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Nu zorgen dat uw app hybride een scherm met een webweergave op is geïnstalleerd. U kunt toevoegen aan de `Main.storyboard` van de app. 

3. Deze webweergave koppelen aan uw **ViewController** door te klikken en slepen van de webweergave van de weergave Controller scène naar de `ViewController.h` scherm, het plaatsen bewerken net onder de `@interface` lijn. 

4. Zodra u dit doet, wordt een dialoogvenster weergegeven waarin wordt gevraagd voor een naam. Geef de naam als **webweergave**. Uw `ViewController.h` bestand ziet er als volgt te werk:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. We zullen bijwerken de `ViewController.m` bestand later, maar we gaan eerst het brug-bestand dat Hiermee maakt u een Wikkel voor enkele veelgebruikte Mobile betrokkenheid iOS SDK methoden maken. Maken van een nieuwe kop bestand met de naam van **EngagementJsExports.h** waarbij gebruik wordt de `JSExport` om die worden beschreven in de bovengenoemde [sessie](https://developer.apple.com/videos/play/wwdc2013/615) om weer te geven van de systeemeigen iOS-methoden. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. We gaan naast het tweede deel van het bestand brug maken. Maak een bestand met de naam van **EngagementJsExports.m** met het maken van de werkelijke aanvullende inhoud door de ondersteuning voor de Mobile-betrokkenheid iOS SDK methoden implementatie. Ook de notitie die we zijn parseren van de `extras` door de webweergave javascript worden doorgegeven en plaatsen die in een `NSMutableDictionary` object met de betrokkenheid SDK methode oproepen worden doorgegeven.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Nu we keert u terug naar de **ViewController.m** en bijwerken met de volgende code: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Houd rekening met de volgende punten met betrekking tot het bestand **ViewController.m** :

    - In de `loadWebView` methode, worden we een lokale HTML-bestand met de naam van **LocalPage.html** waarvan u de code wordt besproken naast geladen. 
    - In de `webViewDidFinishLoad` methode we toepassen de `JsContext` en onze klasse Wikkel koppelt. Hierdoor kunt bellen van onze Wikkel SDK methoden met de greep **EngagementJs** de webweergave. 

7. Maak een bestand met de naam van **LocalPage.html** met de volgende code:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Houd rekening met de volgende punten over de bovenstaande HTML-bestand:

    -   Bevat een reeks invoervakken waarin u de gegevens die moeten worden gebruikt als namen voor de gebeurtenis, taak, fout, AppInfo kunt geven. Als u op de knop ernaast klikt, wordt een oproep wordt uitgevoerd naar de Javascript die uiteindelijk de methoden uit het bestand brug aanroept voor het doorgeven van deze oproep door naar het mobiele betrokkenheid iOS SDK. 
    -   We zijn labelen op sommige statische extra info gebeurtenissen, taken en zelfs fouten om te laten zien hoe dit kan gebeuren. Deze extra info als een JSON tekenreeksexpressie die wordt verzonden als u zoeken in de `EngagementJsExports.m` bestand, is geparseerde en doorgegeven samen met het verzenden van gebeurtenissen, taken, fouten. 
    -   Een taak van de betrokkenheid Mobile wordt gestart met de naam die u opgeeft in het vak Invoerbereik uitvoeren voor 10 seconden en afgesloten. 
    -   Een mobiele betrokkenheid appinfo of tag wordt doorgegeven met 'customer_name' als de statische sleutel en de waarde die u hebt opgegeven in het vak invoerbereik als de waarde voor de tag. 
 
9. De app uitgevoerd en u ziet het volgende. Nu een naam voor een gebeurtenis test als volgt uit en klik op **verzenden** ernaast. 

    ![][1]

10. Als u naar het tabblad **Beeldscherm** van uw app en kijkt u onder **gebeurtenissen-Details >**gaat, ziet u nu deze gebeurtenis samen met de statische app-informatie die we verzendt worden weergegeven. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
