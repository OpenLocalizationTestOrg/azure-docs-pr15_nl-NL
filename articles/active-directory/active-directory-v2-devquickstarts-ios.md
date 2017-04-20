<properties
    pageTitle="Azure AD v2.0-App voor iOS | Microsoft Azure"
    description="Het maken van een iOS-app die zich aanmeldt gebruikers met beide persoonlijk Microsoft-account en accounts voor werk- of schoolaccount met behulp van derden bibliotheken."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmeldingsproblemen toevoegen aan een iOS-app met behulp van een derde partij-bibliotheek met behulp van het eindpunt v2.0 Graph-API

Het Microsoft-platform identiteit wordt open standaarden zoals OAuth2 en verbindt u OpenID gebruikt. Ontwikkelaars kunnen een bibliotheek die ze willen integreren met onze services gebruiken. Ontwikkelaars ons platform gebruiken met andere bibliotheken, zodat hebt we een paar walkthroughs zoals dit het configureren van derden bibliotheken verbinding maken met het Microsoft-platform identiteit aantonen geschreven. De meeste bibliotheken waarin [de specificatie RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen koppelen aan het Microsoft-platform identiteit.

Zorg dat de toepassing die deze procedure wordt gemaakt, kunnen gebruikers kunnen aanmelden bij hun organisatie en zoek naar anderen in hun organisatie met behulp van de grafiek-API.

Als u geen ervaring met OAuth2 of OpenID verbinden hebt, logisch veel van deze steekproef-configuratie niet voor u. Het is aan te raden wij u [v2.0 protocollen - OAuth 2.0 autorisatie Code Flow](active-directory-v2-protocols-oauth-code.md) voor achtergrond lezen.


> [AZURE.NOTE]
    Sommige functies van onze platform die een expressie in de standaarden OAuth2 of OpenID verbinden, zoals voorwaardelijke toegang en beheer van Intune hebben, moeten u de bron van onze openen Microsoft Azure identiteit bibliotheken gebruiken.

Het eindpunt v2.0 biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen.

> [AZURE.NOTE]
    Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Code downloaden van GitHub
De code voor deze zelfstudie worden [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

U kunt ook gewoon downloaden van de steekproef en meteen aan de slag:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app bij de [portal voor registratie van toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)maken of de gedetailleerde stappen voor [het registreren van een app aan het eindpunt v2.0](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopieer de **Toepassings-Id** die toegewezen aan uw app omdat u deze snel moet.
- De **Mobile** -platform voor de app toevoegen.
- Kopieer de **URI omleiden** van de portal. Moet u de standaardwaarde van `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Downloaden van de bibliotheek van derden NXOAuth2 en een werkruimte maken

Voor deze procedure gebruikt u de OAuth2Client van GitHub, dat wil zeggen een bibliotheek OAuth2 voor Mac OS X en iOS (cacao en cacao Raak). Deze bibliotheek is gebaseerd op concept 10 van de specificatie OAuth2. Deze implementeert het profiel van de bijbehorende toepassing en het eindpunt autorisatie van de gebruiker ondersteunt. Hierna ziet u alle dingen die u moet integreren met het Microsoft-platform identiteit.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>De bibliotheek toevoegen aan uw project met behulp van CocoaPods

CocoaPods is een manager afhankelijkheid voor Xcode projecten. De vorige installatiestappen beheert automatisch.

```
$ vi Podfile
```
1. De volgende manieren te werk toevoegen aan deze podfile:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Laad de podfile met behulp van CocoaPods. Hiermee maakt u een nieuwe Xcode werkruimte die u hebt wordt geladen.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>De structuur van het project verkennen

De volgende structuur is ingesteld voor onze project in de skelet:

- Een diamodel-weergave met een zoekopdracht UPN
- Een gedetailleerde weergave voor de gegevens over de geselecteerde gebruiker
- Een Login weergave waarin een gebruiker bij de app aanmelden kan om query's in de grafiek

We gaat naar verschillende bestanden in de skelet om toe te voegen verificatie. Andere delen van de code in, zoals de visuele code, niet van toepassing zijn op de identiteit, maar zijn opgegeven voor u.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Het bestand settings.plst in de bibliotheek instellen

-   Open in het project QuickStart de `settings.plist` bestand. Vervang de waarden van de elementen in de sectie om de waarden die u hebt gebruikt in de portal van Azure aan te geven. Uw code verwijst naar deze waarden wanneer de site gebruikmaakt van de bibliotheek voor verificatie van Active Directory.
    -   De `clientId` is de client-ID van de toepassing die u hebt gekopieerd vanaf de portal.
    -   De `redirectUri` is de omleidings-URL die de portal.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>De bibliotheek NXOAuth2Client in uw LoginViewController instellen

De bibliotheek NXOAuth2Client is vereist voor te bereiden enkele waarden. Nadat u deze taak uitvoert, kunt u de opgehaalde token de API van de grafiek aan te roepen. Omdat `LoginView` worden aangeroepen elk gewenst moment we nodig om te verifiëren, is het verstandig om te zetten configuratiewaarden in naar dat bestand.

- Laten we eens enkele waarden naar toevoegen voor de `LoginViewController.m` bestand voor het instellen van de context voor de verificatie en machtiging. Meer informatie over de waarden volgt u de code.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Bekijk meer informatie over de code in.

De eerste tekenreeks is bedoeld voor `scopes`.  De `User.Read` waarde kunt u lezen van het profiel van de basis van de gebruiker aangemeld.

U kunt meer informatie over alle beschikbare bereiken bij [Microsoft Graph machtiging bereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Voor `authURL`, `loginURL`, `bhh`, en `tokenURL`, moet u de waarden die eerder zijn ingevoerd. Als u de bron openen Microsoft Azure identiteit bibliotheken gebruikt, ophalen we deze gegevens omlaag voor u met behulp van onze eindpunt metagegevens. We hebt het werk van deze waarden ophalen voor u gedaan.

De `keychain` waarde is de container dat de bibliotheek NXOAuth2Client gebruiken wilt voor het maken van een sleutelhanger om op te slaan uw tokens. Als u helder eenmalige aanmelding (SSO) over te brengen veel apps wilt, kunt u de dezelfde sleutelhanger opgeven in elk van uw toepassingen en het gebruik van deze sleutelhanger aanvragen in uw rechten Xcode. Dit wordt uitgelegd in de Apple-documentatie.

De rest van deze waarden zijn vereist om de bibliotheek gebruiken en u moet uitvoeren waarden op de context te maken.

### <a name="create-a-url-cache"></a>Een URL-cache maken

Binnen `(void)viewDidLoad()`, die altijd wordt aangeroepen nadat de weergave is geladen, de volgende code primes een cache voor onze gebruik.

Voeg de volgende code:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Maken van een webweergave voor aanmelden

Een webweergave kan de gebruiker gevraagd om aanvullende factoren zoals SMS-bericht (indien geconfigureerd) of foutberichten teruggaan naar de gebruiker. Hier stelt u de webweergave omhoog en vervolgens de code voor het verwerken van de terugbellen in de webweergave van de services identiteit gebeurt later te schrijven.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>De webweergave methoden voor het verwerken van verificatie overschrijven

Als u wilt de webweergave zien wat er gebeurt wanneer een gebruiker moet zich aanmelden zoals eerder besproken, kunt u de volgende code plakken.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Code voor het verwerken van het resultaat van de aanvraag OAuth2 schrijven

De volgende code verwerkt door de URL-omleiding die resulteert in de webweergave. Als verificatie niet slaagt, probeert de code het opnieuw. De bibliotheek, ondertussen vindt u de fout die u kunt zien in de console of asynchroon verwerken.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Het OAuth-Context (account store genoemd) instellen

Hier kunt u bellen `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` in voor elke service die u wilt de toepassing toegang tot gedeelde account store. Het accounttype is een tekenreeks die wordt gebruikt als een aanduiding voor een bepaalde service. Omdat u toegang krijgt de API Graph tot, wordt de code verwijst naar als `"myGraphService"`. U vervolgens instellen een waarnemer die aangeeft wanneer iets verandert met het token. Nadat u het token ontvangen, u afzender de gebruiker terug naar de `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>De weergave diamodel wilt zoeken en weergeven van de gebruikers van de grafiek-API instellen

Een diamodel-weergave-Controller (MVC)-app waarin de geretourneerde gegevens worden weergegeven in het raster valt buiten het bereik van deze procedure en veel online zelfstudies wordt uitgelegd hoe u een te maken. Alle deze code is in het skelet bestand. U hoeft echter omgaan met een paar dingen in deze toepassing MVC:

* Wanneer een gebruiker is getypt iets in het zoekveld snijpunt
* Een object met gegevens terug naar de MasterView bieden, zodat deze de resultaten in het raster weergeven kunt

We doen die hieronder.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Een selectievakje om te zien als u bent aangemeld toevoegen

De toepassing biedt fractie als de gebruiker niet is aangemeld, dus is het verstandig om te controleren of er al een token in de cache. Als dit niet het geval is, wordt u omgeleid naar de LoginView voor de gebruiker aan te melden. Als u meer weet, de beste manier als u wilt doen wanneer een weergave wordt geladen is gebruik van de `viewDidLoad()` methode Apple vindt u ons.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>De weergave van de tabel bijwerken wanneer gegevens worden ontvangen

Wanneer de API Graph haalt gegevens op, moet u de gegevens worden weergegeven. Voor eenvoudig volgt de code om de tabel te werken. U kunt alleen de juiste waarden plakken in MVC standaardtekst code.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Zelf een formule om te bellen van de grafiek-API wanneer iemand in het zoekveld typt

Wanneer een gebruiker een zoekopdracht in het vak, moet u shove die boven aan de grafiek-API. De `GraphAPICaller` klasse, die u in de volgende code wordt maakt, worden gescheiden door de functionaliteit voor het opzoeken van de presentatie. Nu we de code die-feeds worden tekens zoeken tot de API Graph schrijven. We gebeurt dit met behulp van een methode genoemd `lookupInGraph`, waarin de tekenreeks die we zoeken wilt naar duurt.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Een helperklasse voor toegang tot de API Graph schrijven

Dit is een belangrijk onderdeel van de toepassing. Terwijl de rest code in het standaard MVC van Apple invoegen is, schrijven hier u code query graph tijdens het typen en ga vervolgens terug die gegevens. Hier ziet u de code en een gedetailleerde uitleg volgt deze.

### <a name="create-a-new-objective-c-header-file"></a>Een nieuw doel C koptekst-bestand maken

Geef het bestand een naam `GraphAPICaller.h`, en voeg de volgende code toe.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Hier ziet u dat een opgegeven methode een tekenreeks en geeft als een completionBlock resultaat. Deze completionBlock, terwijl u mogelijk hebt raden, bijgewerkt in de tabel met behulp van een object die ingevulde gegevens realtime als de gebruiker zoekopdrachten.


### <a name="create-a-new-objective-c-file"></a>Een nieuw doel C-bestand maken

Geef het bestand een naam `GraphAPICaller.m`, en voegt u de volgende methode.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

In dit artikel leest u over deze methode uitgebreid beschreven.

De kerninhoud van deze code is in de `NXOAuth2Request`, methode waarmee de parameters die u al hebt gedefinieerd in het bestand settings.plist.

De eerste stap is om de juiste grafiek API-oproep te maken. Omdat u belt `/users`, u opgeven dat door deze te voegen aan de resource Graph API samen met de versie. Is het handig om te zetten in een externe instellingenbestand omdat deze wijzigen kunnen, zoals de API ontwikkeld.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Vervolgens moet u parameters opgeven die u ook u in de grafiek API-oproep vindt. Het is *Belangrijk* dat u plaats niet de parameters in het eindpunt van de resource omdat die wassen om voor alle niet-URI die voldoen tekens gedurende runtime achtergrondkoolwaterstoffen is. Alle querycode moet worden opgegeven in de parameters.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

U ervaart mogelijk dit roept een `convertParamsToDictionary` methode die u nog niet hebt geschreven. Laten we dit nu doen aan het einde van het bestand:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Vervolgens gebruiken we de `NXOAuth2Request` methode om gegevens uit de API in de indeling van JSON terug te gaan.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Tot slot we bekijken hoe u de gegevens terug naar de MasterViewController. De gegevens worden geretourneerd als zodanig en moet worden gedeserialiseerd en geladen in een object dat de MainViewController kunt gebruiken. Dien het skelet heeft een `User.m/h` -bestand dat wordt gemaakt van een gebruikersobject. U vult die gebruikersobject met gegevens uit de grafiek.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>De steekproef worden uitgevoerd

Als u hebt het skelet gebruikt of gevolgd samen met het overzicht moet de toepassing nu worden uitgevoerd. De simulator Start en klik op **aanmelden** als de toepassing wilt gebruiken.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor onze product

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken het [Security TechCenter](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
