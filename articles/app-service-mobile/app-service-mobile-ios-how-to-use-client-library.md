<properties
    pageTitle="Hoe gebruik iOS SDK voor Azure Mobile-Apps"
    description="Hoe gebruik iOS SDK voor Azure Mobile-Apps"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Hoe gebruik iOS-Client-bibliotheek voor Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u uitvoeren van veelvoorkomende scenario's met de meest recente [Azure Mobile-Apps iOS SDK][1]. Als u nog niet eerder met Azure Mobile-Apps, eerste voltooid [Azure mobiele Apps snel aan de slag] met het maken van een back-end, een tabel maken en een vooraf gedefinieerde iOS Xcode project downloaden. In deze handleiding, richten we ons op de aan de clientzijde iOS SDK. Meer informatie over de SDK servers voor de backend, raadpleegt u de Server SDK HOWTOs.

## <a name="reference-documentation"></a>Naslagmateriaal

De documentatie bij de iOS-client SDK zich bevindt: [Azure Mobile-Apps iOS Client verwijzing][2].

## <a name="supported-platforms"></a>Ondersteunde platformen

De iOS SDK ondersteunt doelstelling-C projecten, Swift 2.2 projecten en Swift 2.3 projecten voor iOS versies 8.0 of hoger.

De verificatie 'server-mailstroom' wordt een webweergave gebruikt voor de gepresenteerde gebruikersinterface.  Als het apparaat niet mogen een UI WebView presenteren en vervolgens een andere methode verificatie is vereist is die buiten het bereik van het product.  
Deze SDK is dus niet geschikt voor controletype of op dezelfde manier beperkte apparaten.

##<a name="Setup"></a>Installatie en vereisten

Deze handleiding wordt ervan uitgegaan dat u een back-end met een tabel hebt gemaakt. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudies. Deze handleiding ook wordt ervan uitgegaan dat in uw code u verwijzingen maken naar `MicrosoftAzureMobile.framework` en importeer `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Hoe u: Client maken

Voor toegang tot een backend Azure Mobile-Apps in uw project, maken een `MSClient`. Vervang `AppUrl` met de URL van de app. U mag verlaten `gatewayURLString` en `applicationKey` leeg. Als u een gateway voor verificatie hebt ingesteld, vullen `gatewayURLString` met de URL van de gateway.

**Doelstelling-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Hoe u: tabelverwijzing maken

Met access of update gegevens, een verwijzing naar de backend-tabel maken. Vervang `TodoItem` met de naam van de tabel

**Doelstelling-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Hoe u: Query van gegevens

Als u wilt maken van een databasequery's, query de `MSTable` object. De volgende query wordt alle items in `TodoItem` en de tekst van elk item Logboeken.

**Doelstelling-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Hoe u: resultaatgegevens Filter

Als u wilt filteren resultaten, zijn er veel beschikbare opties.

Als u wilt filteren met een predikaat, gebruikt u een `NSPredicate` en `readWithPredicate`. De volgende filters als resultaat gegeven van de gegevens alleen onvoltooide taak items vinden.

**Doelstelling-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Hoe u: MSQuery gebruiken

Naar het uitvoeren van een complexe query (inclusief sorteren en paginering), maakt u een `MSQuery` object, rechtstreeks of met behulp van een predikaat:

**Doelstelling-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`kunt u verschillende query gedrag bepalen.

* Volgorde van zoekresultaten opgeven
* Welke velden om terug te keren beperken
* Hoeveel records om terug te beperken
* Totaal aantal in antwoord opgeven
* Aangepaste query tekenreeksparameters opgeeft in aanvraag
* Extra functies toepassen

Uitvoeren een `MSQuery` query door te bellen `readWithCompletion` op het object.

## <a name="sorting"></a>Hoe u: gegevens met MSQuery sorteren

Als u wilt sorteren van resultaten, kunt u een voorbeeld nu behandeld. Als u wilt sorteren op het veld 'tekst' oplopend, vervolgens op 'voltooid' aflopend, roepen `MSQuery` als volgt:

**Doelstelling-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Hoe u: beperkingen instellen voor velden en tekenreeks queryparameters met MSQuery uitvouwen

Als u wilt beperken velden moeten in een query worden geretourneerd, geeft u de namen van de velden in de eigenschap **selectFields** . In dit voorbeeld wordt alleen de tekst en de voltooide velden:

**Doelstelling-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Als u wilt opnemen extra queryreeks-parameters in het Serververzoek (bijvoorbeeld omdat deze worden gebruikt in een aangepast script van de serverzijde), vullen `query.parameters` als volgt:

**Doelstelling-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Hoe u: paginaformaat configureren

Het paginaformaat besturingselementen met Azure Mobile-Apps, het aantal records die zijn opgehaald uit de back-end-tabellen tegelijk. Een oproep naar `pull` gegevens zou vervolgens batch-ups van gegevens, op basis van deze paginaformaat, totdat er geen records meer om op te halen.

Het is mogelijk voor het configureren van een paginaformaat **MSPullSettings** gebruiken, zoals hieronder wordt weergegeven. Het standaardpaginaformaat 50 is en in het onderstaande voorbeeld wordt dit omgezet in 3.

U kunt een ander paginaformaat voor betere prestaties kan configureren. Als er een groot aantal kleine gegevensrecords, minder een hoge paginaformaat gegevens van de server. 

Met deze instelling wordt alleen het paginaformaat aan de clientzijde. Als de client wordt gevraagd om een pagina groter dan de Mobile-Apps-end ondersteunt, wordt het paginaformaat beperkt tot het maximale aantal dat de backend is geconfigureerd voor ondersteuning. 

Deze instelling is ook het _nummer_ van de gegevensrecords, niet de _grootte in bytes_.

Als u het paginaformaat client, [moet u ook het paginaformaat op de server verhogen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)vergroten.

**Doelstelling-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Hoe u: gegevens invoegen

Als u wilt een nieuwe tabelrij invoegen, maken een `NSDictionary` en roepen `table insert`. Als [Dynamische Schema] is ingeschakeld, genereert de mobiele App-Service van Azure-end automatisch nieuwe kolommen op basis van de `NSDictionary`.

Als `id` is niet opgegeven de backend automatisch een nieuwe unieke ID genereert. Geef uw eigen `id` e-mail te ontvangen adressen, gebruikersnamen, of uw eigen aangepaste waarden als ID. Uw eigen ID leveren gemakkelijker joins en zakelijke database logica.

De `result` het nieuwe item bevat dat is ingevoegd. Afhankelijk van de logica van uw server, kunnen er extra of gewijzigde gegevens ten opzichte van wat er is doorgegeven aan de server.

**Doelstelling-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Hoe u: gegevens wijzigen

Als u een bestaande rij bijwerken, het wijzigen van een item en de oproep `update`:

**Doelstelling-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

U kunt ook de rij-ID en het bijgewerkte veld opgeven:

**Doelstelling-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Ten minste de `id` attribuut moet worden ingesteld bij het maken van updates.

##<a name="deleting"></a>Hoe u: gegevens verwijderen

Als u wilt een item verwijdert, roepen `delete` met het item:

**Doelstelling-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

U kunt ook verwijderen door op te geven van een rij-ID:

**Doelstelling-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Ten minste de `id` kenmerk moet worden ingesteld wanneer aanbrengen worden verwijderd.

##<a name="customapi"></a>Hoe u: aangepaste API bellen

Met een aangepaste API, kunt u een back-end-functionaliteit laten zien. Deze geen toewijzen aan een tabel-bewerking. Niet alleen doen hebt u meer controle over messaging, maar u kunt zelfs gelezen/set kop en de opmaak van de hoofdtekst antwoord. Lees informatie over het maken van een aangepaste API op de backend, [Aangepaste API 's](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Als u wilt inbellen een aangepaste API, `MSClient.invokeAPI`. De vergaderverzoeken en antwoorden die inhoud worden behandeld als JSON. Gebruik van andere mediatypen [gebruik van de andere overbelasting van `invokeAPI` ] [ 5].  U een `GET` aanvragen in plaats van een `POST` aanvragen, set-parameter `HTTPMethod` naar `"GET"` en parameter `body` naar `nil` (Aangezien GET-aanvragen beschikt niet over de berichttekst.) Als uw aangepaste API andere HTTP-woorden ondersteunt, wijzigen `HTTPMethod` correct.

**Doelstelling-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Hoe u: Register push sjablonen om meerdere platforms meldingen te verzenden

Doorgeven als wilt registreren sjablonen, sjablonen met de methode **client.push registerDeviceToken** in uw client-app.

**Doelstelling-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Uw sjablonen zijn van het type NSDictionary en kunnen bevatten meerdere sjablonen in de volgende indeling:

**Doelstelling-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle markeringen worden verwijderd uit de aanvraag voor waardepapier.  Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps]voor informatie over het toevoegen van tags op installaties of sjablonen binnen installaties[4].  Als u wilt verzenden meldingen deze geregistreerde sjablonen gebruiken, werken met [Melding Hubs-API's][3].

##<a name="errors"></a>Hoe u: fouten

Wanneer u een mobiele App-Service van Azure-backend belt, de blokkering voltooid bevat een `NSError` parameter. Wanneer een fout optreedt, is deze parameter niet-nul. In code, moet u deze parameter inschakelt en afhandelen van de fout indien nodig, zoals u in de voorgaande codefragmenten.

Het bestand [`<WindowsAzureMobileServices/MSError.h>`] [6] definieert de constanten `MSErrorResponseKey`, `MSErrorRequestKey`, en `MSErrorServerItemKey`. Meer gegevens betrekking hebben op de fout opvragen:

**Doelstelling-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Het bestand definieert bovendien constanten voor elke foutcode:

**Doelstelling-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Hoe u: gebruikers met Active Directory-verificatie Library verifiëren

U kunt de Active Directory verificatie bibliotheek (ADAL) gebruikers aan te melden bij uw-toepassing via Azure Active Directory. Client-mailstroom verificatie met een identiteitsprovider SDK verdient over het gebruik van de `loginWithProvider:completion:` methode.  Client-mailstroom verificatie biedt een meer systeemeigen UX feel en kan voor aanvullende aanpassing.

1. Uw mobiele app backend voor aanmelding AAD configureren aan de hand van de [App-Service voor Active Directory-aanmelding configureren] [ 7] zelfstudie. Zorg ervoor dat de optionele stap voor het registreren van een clienttoepassing native uitvoert. Voor iOS, is het aanbevolen dat de omleiding URI aan het formulier is `<app-scheme>://<bundle-id>`. Zie voor meer informatie de [ADAL iOS quickstart][8].

2. Installeer ADAL Cocoapods gebruiken. Uw Podfile als u wilt opnemen van de volgende definitie van **Uw PROJECT** vervangen door de naam van uw project Xcode bewerken:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   en de Pod:

        pod 'ADALiOS'

3. Gebruik de Terminal voeren `pod install` dat uit de adreslijst van uw project bevat en open vervolgens de gegenereerde Xcode werkruimte (niet de project).

4. De volgende code hebt toegevoegd aan uw toepassing, op basis van de taal die u gebruikt. Controleer deze vervanging in elke wordt:

    * Vervang **Hier-instantie-invoegen** door de naam van de tenant waarin u uw toepassing ingericht. De opmaak moet https://login.windows.net/contoso.onmicrosoft.com. Deze waarde kan worden gekopieerd vanuit het tabblad van het domein in uw Azure Active Directory in de [Azure klassieke portal].
    * **Invoegen RESOURCE-ID-hier -** vervangen door de klant-ID voor de mobiele app backend. U kunt de client-ID aanvragen van het tabblad **Geavanceerd** onder **Azure Active Directory-instellingen** in de portal.
    * **Invoegen-CLIENT-ID-hier** vervangen door de client-ID die u hebt gekopieerd uit de clienttoepassing native.
    * **Invoegen-REDIRECT-URI-hier** vervangen door een van uw site _/.auth/login/done_ eindpunt, met het schema HTTPS. Deze waarde moet er ongeveer als _https://contoso.azurewebsites.net/.auth/login/done_.

**Doelstelling-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Hoe u: gebruikers met de Facebook-SDK voor iOS verifiëren

U kunt de SDK Facebook voor iOS gebruikers aan te melden bij uw toepassing met Facebook.  Het gebruik van een client stroom-verificatie is voorkeur boven de `loginWithProvider:completion:` methode.  De verificatie van de client-mailstroom biedt een meer systeemeigen UX feel en kan voor extra aanpassen.

1. Uw mobiele app backend voor Facebook-aanmelding configureren aan de hand van de [App-Service voor Facebook aanmelding configureren] [ 9] zelfstudie.

2. De Facebook SDK voor iOS installeren via de [Facebook-SDK voor iOS - aan de slag] [ 10] documentatie. In plaats van een app maken, kunt u de iOS-platform toevoegen aan uw bestaande registratie. 

3. De Facebook-documentatie bevat enkele doelstelling-C-code in de gemachtigde App. Als u **Swift**gebruikt, kunt u de volgende vertalingen voor AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Naast het toevoegen van `FBSDKCoreKit.framework` aan uw project, moet u ook een verwijzing naar toevoegen `FBSDKLoginKit.framework` op dezelfde manier. 

4. De volgende code hebt toegevoegd aan uw toepassing, op basis van de taal die u gebruikt. 

**Doelstelling-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Hoe u: gebruikers met Twitter-stof voor iOS verifiëren

Stof voor iOS kunt u gebruikers aan te melden bij uw-toepassing via Twitter. Client-mailstroom verificatie verdient over het gebruik van de `loginWithProvider:completion:` methode, zoals het biedt een meer systeemeigen UX feel en kan voor aanvullende aanpassing.

1. Uw mobiele app backend voor Twitter-aanmelding instellen door de zelfstudie voor [het configureren van App-Service voor Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Stof toevoegen aan uw project door de documentatie [stof voor iOS - aan de slag] uit te voeren en het instellen van TwitterKit.

    > [AZURE.NOTE] Standaard stof een Twitter-toepassing voor u gemaakt. U kunt voorkomen dat een toepassing maken door te registreren van de consumenten-toets en consumentgeheim die u eerder met de volgende codefragmenten hebt gemaakt.  U kunt ook de waarden voor consumenten-toets en consumentgeheim waarover u beschikt voor App Service met de waarden die u in het [Dashboard stof ziet]vervangen. Als u deze optie kiest, moet u de URL van de terugbellen zoals instellen op een aanduidingswaarde van de tijdelijke, `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Als u besluit om het gebruik van de geheimen die u eerder hebt gemaakt, kunt u de volgende code toevoegen aan uw gemachtigde App:
    
    **Doelstelling-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. De volgende code hebt toegevoegd aan uw toepassing, op basis van de taal die u gebruikt. 

**Doelstelling-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Hoe u: gebruikers met de Google aanmeldingsproblemen SDK voor iOS verifiëren

U kunt de Google aanmeldingsproblemen SDK voor iOS gebruikers aan te melden bij uw toepassing via een Google-account.  Google aangekondigd onlangs wijzigingen in hun OAuth beveiligingsbeleid voor apparaten.  Deze beleidswijzigingen moeten worden in de toekomst het gebruik van de Google-SDK.

1. Configureer uw backend mobiele app voor Google-aanmelding aan de hand van de zelfstudie voor [het configureren van App-Service voor Google login](app-service-mobile-how-to-configure-google-authentication.md) .

2. Installeer de Google SDK voor iOS door de documentatie [starten Google-aanmelding voor iOS - integratie](https://developers.google.com/identity/sign-in/ios/start-integrating) . U kunt de sectie "Verifiëren met een back-endserver" overslaan.

3. Voeg het volgende toe van de gemachtigde `signIn:didSignInForUser:withError:` methode, op basis van de taal die u gebruikt.

**Doelstelling-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Zorg ervoor dat u ook de volgende toevoegen `application:didFinishLaunchingWithOptions:` in uw app gemachtigde 'SERVER_CLIENT_ID' vervangen door de dezelfde-ID die u naar de App-Service configureren in stap 1 hebt gebruikt.

**Doelstelling-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. De volgende code hebt toegevoegd aan uw toepassing in een UIViewController waarmee de `GIDSignInUIDelegate` protocol, op basis van de taal die u gebruikt.  U bent afgemeld voordat u opnieuw worden aangemeld en hoewel u niet nodig hebt om in te voeren uw referenties opnieuw, wordt een dialoogvenster toestemming.  Deze methode alleen oproepen wanneer het sessietoken is verlopen.
 
 **Doelstelling-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobiele Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Stof Dashboard]: https://www.fabric.io/home
[Stof voor iOS - aan de slag]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
