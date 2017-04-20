<properties
    pageTitle="Azure Active Directory-B2C: Een web API bellen vanuit een iOS-toepassing via van derden bibliotheken | Microsoft Azure"
    description="In dit artikel wordt uitgelegd hoe u een app van iOS 'takenlijst' die een Node.js web API-oproepen via OAuth 2.0 vruchtdragende tokens met behulp van een derde partij bibliotheek maken"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< labels ms.service= "actief-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "prominente-artikel"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Een web API bellen vanuit een iOS-toepassing met behulp van de bibliotheek van een derde partij

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Het Microsoft-platform identiteit wordt open standaarden zoals OAuth2 en verbindt u OpenID gebruikt. Hiermee kan ontwikkelaars om te profiteren van een bibliotheek die ze willen integreren met onze services. Als toelichting van ontwikkelaars ons platform gebruikt met andere bibliotheken we een paar walkthroughs zoals dit te tonen hebt geschreven het configureren van derden bibliotheken verbinding maken met het Microsoft-platform identiteit. De meeste bibliotheken waarin [de specificatie RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen verbinding maken met het Microsoft Identity-platform.


Als u geen ervaring met OAuth2 of OpenID verbinding maken met hebt logisch veel van deze steekproef-configuratie niet veel voor u. Het is raadzaam om dat u een beknopt [overzicht van het protocol dat we hier hebt beschreven](active-directory-b2c-reference-protocols.md)bekijkt.

> [AZURE.NOTE]
    Sommige functies van onze platform die een expressie in deze standaarden zoals voorwaardelijke toegang en Intune beheer hebben, moeten u de bron van onze openen Microsoft Azure identiteit bibliotheken gebruiken. 
   
Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het platform B2C.  Als u wilt bepalen als u het platform B2C moet gebruiken, lees meer over [B2C beperkingen](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant. Een map is een container voor al uw gebruikers, apps, groepen en meer. Als u deze niet hebt, worden al doorgaan [een map B2C maken](active-directory-b2c-get-started.md) voordat u.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. De app en het web API worden aangegeven met een enkel **Toepassings-ID** in dit geval omdat ze bestaan uit één logische app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken. Zorg ervoor dat:

- Een **mobiel apparaat** opnemen in de toepassing.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat één identiteit ervaring: een gecombineerde aanmelden en aanmelding. U moet maken van dit beleid van elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u het beleid maakt, moet u naar:

- Kies de **naam weer te geven** en aanmelding kenmerken in uw beleid.
- Kies de **naam weer te geven** en de **Object-ID** -toepassing claims in elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  U hebt de naam van het beleid later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u uw beleid hebt gemaakt, kunt u bent uw app maken.


## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie worden [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)bijgehouden.  Als u wilt volgen, kunt u [de app als een .zip downloaden](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) of klonen deze:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Of downloaden van de voltooide code en meteen aan de slag: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>De derde partij bibliotheek nxoauth2 downloaden en een werkruimte te starten

Voor dit scenario gebruiken we de OAuth2Client van GitHub, een bibliotheek OAuth2 voor Mac OS X & iOS (cacao & cacao Raak). Deze bibliotheek is gebaseerd op concept 10 van de specificatie OAuth2. Deze implementeert het profiel van de bijbehorende toepassing en het eindpunt van de autorisatie voor eindgebruikers ondersteunt. Hierna ziet u alle items die we nodig hebt in volgorde naar integrat met Microsoft identiteit platform.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>De bibliotheek toevoegen aan uw project met behulp van CocoaPods

CocoaPods is een manager afhankelijkheid voor Xcode projecten. De bovenstaande installatiestappen beheert automatisch.

```
$ vi Podfile
```
De volgende manieren te werk toevoegen aan deze podfile:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Nu de podfile met cocoapods geladen. Hiermee maakt u een nieuwe XCode-werkruimte worden geladen.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>De structuur van het project

We hebben de volgende structuur voor onze project in de skelet instellen:

* Een **Modelweergave** met een taakvenster
* Een **Taakweergave toevoegen** voor de gegevens over de geselecteerde taak
* Een **Weergave Login** waarmee een gebruiker kan aanmelden bij de app.

We springt naar verschillende bestanden in het project om toe te voegen verificatie. Andere delen van de code zoals de visuele code is niet belangrijkste aan identiteit en zijn opgegeven voor u.

## <a name="create-the-settingsplist-file-for-your-application"></a>Maak de `settings.plist` bestand voor uw toepassing

Het is eenvoudiger te configureren van de toepassing als er een centrale locatie voor de configuratiewaarden van onze. Ook kunt u meer informatie over wat betekent elke instelling in uw toepassing. We maken gebruik van de *Eigenschappenlijst* als een manier om deze waarden met de toepassing te leveren.

* Maken of/openen de `settings.plist` bestand onder `Supporting Files` in uw toepassingswerkruimte

* Voer in de volgende waarden (gaan we door ze in detail binnenkort)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Dit artikel leest deze uitgebreid beschreven.


Voor `authURL`, `loginURL`, `bhh`, `tokenURL` u ziet u moet de naam van de tenant invullen. Dit is de naam van de tenant van uw tenant B2C die aan u is toegewezen. Bijvoorbeeld `kidventusb2c.onmicrosoft.com`. Als u de bron van onze openen Microsoft Azure identiteit bibliotheken gebruiken zou doen we deze gegevens ophalen omlaag voor u onze eindpunt metagegevens gebruiken. We hebt het werk van deze waarden ophalen voor u gedaan.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

De `keychain` waarde is de container dat de bibliotheek NXOAuth2Client gebruiken wilt voor het maken van een sleutelhanger om op te slaan uw tokens. Als u wilt SSO helder over te brengen veel apps kunt u de dezelfde sleutelhanger opgeven in elk van uw toepassingen alsmede het gebruik van deze sleutelhanger aanvragen in uw entitements XCode. Hier vindt u in de Apple-documentatie.

De `<policy name>` aan het einde van elke URL zijn de plaatsen waar u zou doen plaatsen van het beleid die u hierboven hebt gemaakt. De app wordt dit beleid afhankelijk van de stroom belt.

De `taskAPI` wordt de REST-eindpunt we bellen met uw token B2C naar een toevoegen, taken of query bestaande taken. Dit is ingesteld zijn specifiek voor dit voorbeeld. U hoeft niet te wijzigen voor de steekproef om te werken.

De rest van deze waarden zijn vereist om de bibliotheek gebruiken en u moet uitvoeren waarden op de context gewoon te maken.

Nu de `settings.plist` bestand hebt gemaakt, moeten we code te lezen.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Een klasse AppData instellen voor het lezen van onze instellingen

Onszelf nog een eenvoudig bestanden die alleen parseert onze `settngs.plist` bestand we hiervoor hebt gemaakt en controleer de avaialble van deze instellingen in de toekomst voor alle klassen. Aangezien we niet dat een exemplaar van de gegevens maken telkens wanneer een klasse wordt gevraagd om deze wilt, we een patroon Singleton gebruiken en retourneert alleen het dezelfde exemplaar gemaakt wanneer er die een aanvraag is voorzien van de instellingen

* Maak een `AppData.h` bestand:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Maak een `AppData.m` bestand:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nu we gemakkelijk toegang hebt onze gegevens door te bellen gewoon `  AppData *data = [AppData getInstance];` in een van onze klassen als u ziet onder.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>De bibliotheek NXOAuth2Client in uw AppDelegate instellen

De bibliotheek NXOAuthClient is vereist voor te bereiden enkele waarden. Eenmaal dat wil zeggen voltooid kunt u de token aquired de REST API aan te roepen. Aangezien we dat weten de `AppDelegate` wordt aangeroepen elk gewenst moment we de toepassing laden dat relevant is dat we onze configuratiewaarden in naar dat bestand plaatsen.
* Open `AppDelegate.m` bestand

* Sommige we later gebruiken koptekst-Bestanden importeren.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Toevoegen de `setupOAuth2AccountStore` methode in de AppDelegate

We moet maken van een AccountStore en feed klikt u vervolgens de gegevens die we zojuist lezen in de `settings.plist` bestand.

Er zijn enkele dingen die u moet weten over de B2C service op dit punt waardoor deze code begrijpelijker:


1. Azure AD B2C gebruikt het *beleid* geleverd door de queryparameters uw aanvraag. Hierdoor Azure Active Directory fungeert als een onafhankelijke service alleen betrekking heeft op uw toepassing. Alleen worden aangeboden als deze extra queryparameters moeten we bieden de `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` methode met onze parameters aangepast beleid. 

2. Azure AD B2C gebruikt bereiken in veel dezelfde manier als andere OAuth2-servers. Echter omdat het gebruik van B2C zoveel over de verificatie van een gebruiker als bronnen sommige bereiken absoluut nodig zijn voor de stroom goed. Dit is de `openid` bereik. De identiteit van onze Microsoft SDK's automatisch bieden de `openid` bereik voor u zodat u dat niet in de configuratie van onze SDK ziet. Aangezien we een bibliotheek van derden gebruikt, echter moeten we in dit bereik opgeven.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Zorg er u Roep deze in de AppDelegate onder vervolgens `didFinishLaunchingWithOptions:` methode. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Maak een `LoginViewController` class dat we gebruiken wilt voor het verwerken van verificatieaanvragen

We gebruiken een webweergave voor account aanmelden. Deze manier kunnen wij de gebruiker gevraagd om aanvullende factoren zoals SMS-bericht (indien geconfigureerd) of de foutberichten aan de gebruiker weer te geven. We stelt u hier de webweergave omhoog en vervolgens de code voor het verwerken van de terugbellen gebeurt in de webweergave van de Service van Microsoft identiteit later te schrijven.

* Maak een `LoginViewController.h` class

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

We gaan elk van de volgende manieren onderstaande maken.

> [AZURE.NOTE] 
    Zorg ervoor dat u koppelt het `loginView` aan de werkelijke webweergave in uw storyboard. Anders hoeft u niet een webweergave die kan worden weergegeven wanneer het tijd om te verifiëren is.

* Maak een `LoginViewController.m` class

* Sommige variabelen voor het uitvoeren van staat, zoals we verifiëren toevoegen

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* De webweergave methoden voor het verwerken van verificatie overschrijven

Moeten we de webweergave Vertel het gedrag dat we horen wanneer een gebruiker moet aanmelden zoals hierboven wordt beschreven. U kunt knippen en plakken van de onderstaande code.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Code voor het verwerken van het resultaat van de aanvraag OAuth2 schrijven

Moeten we code waarmee de URL-omleiding die afkomstig zijn terug uit de webweergave worden verwerkt. Zo niet slaagt, probeert we het opnieuw. De bibliotheek, ondertussen vindt u de fout die u kunt in de console zien of asyncronously verwerken. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* De melding factory's instellen.

Maak de dezelfde methode die we hebben gedaan in de `AppDelegate` bovenaan, maar klik nu we enkele wordt toegevoegd `NSNotification`s naar Vertel ons wat er gebeurt in onze service. We instellen waarnemer die wordt Vertel ons wanneer met het token verandert. Als we de token we de gebruiker terug Terug naar de `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Code waarmee de gebruiker worden afgehandeld wanneer een nieuw vergaderverzoek wordt gestart voor aanmelding-eigen toevoegen

Laten we een methode die worden aangeroepen wanneer er een aanvraag voor verificatie maken. Dit is de methode die u een webweergave maakt

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Laten we bellen ten slotte al deze methoden we boven elke keer hebt geschreven de `LoginViewController` wordt geladen. We dit doen door het toevoegen van de volgende manieren naar onze `viewDidLoad` methode Apple ontstaat

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

U bent nu klaar met het maken van de belangrijkste manier waarop we wordt door de toepassing voor het aanmelden. Wanneer we hebt aangemeld, moeten we onze tokens die we hebben ontvangen gebruiken. Voor die gaan we enkele helper-code die telefonisch REST API's voor ons op deze bibliotheek maken.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Maak een `GraphAPICaller` class u omgaat met onze aanvragen voor een REST API

We hebben een configuratie geladen telkens als we onze app laden. Nu moeten we er iets mee te doen als er een token. 

* Maak een `GraphAPICaller.h` bestand

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

U ziet van deze code dat we twee methoden zal maken: een aan de taken van een API en een andere taken toevoegen aan de API.

Nu we onze interface hebt ingesteld, laten we toevoegen de daadwerkelijke uitvoering:

* Maak een`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Tot slot maken en uitvoeren van de app in Xcode. Registreren of meld u aan bij de app en taken maken voor een gebruiker die zijn aangemeld bij. Meld u af en meld u weer aan als een andere gebruiker en taken voor die gebruiker maken.

Zoals u ziet dat de taken die opgeslagen per gebruiker op de API zijn, omdat de API haalt de gebruikers id uit het toegangstoken die ze ontvangen.


## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar meer geavanceerde B2C onderwerpen. U kunt proberen:

[Een Node.js web API bellen vanuit een Node.js web-app]()

[De UX voor een app B2C aanpassen]()
