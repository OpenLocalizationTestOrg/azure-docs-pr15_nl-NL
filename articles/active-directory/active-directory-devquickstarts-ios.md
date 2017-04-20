<properties
    pageTitle="Azure AD iOS aan de slag | Microsoft Azure"
    description="Het maken van een iOS-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en u belt Azure AD beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Azure AD integreren in een App voor iOS

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD biedt de Active Directory-verificatie Library of ADAL, voor iOS-clients die nodig hebt voor toegang tot beveiligde bronnen.  De enige doel van ADAL in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen.  U kunt zien hoe gemakkelijk het is, gaat hier we maken een toepassing voor de doelstelling C-takenlijst die:

-   Haalt toegang tot tokens voor het bellen van de Azure AD Graph-API met het [OAuth 2.0 verificatie-protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hiermee wordt gezocht in een map voor gebruikers met een opgegeven alias.

Als u wilt de volledige werken-toepassing maakt, moet u:

2. Uw toepassing met Azure AD registreren.
3. Installeren en configureren van ADAL.
5. Gebruik ADAL tokens ophalen van Azure AD.

Om gestart, [de app-skelet downloaden](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  U moet ook een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren.  Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

> [AZURE.TIP] Probeer het voorbeeld van onze nieuwe [developer portal](https://identity.microsoft.com/Docs/iOS) waarmee u kunt aan de slag met Azure Active Directory in slechts een paar minuten!  De developer portal wordt u begeleid bij het proces van het registreren van een app en Azure AD integreren in uw code.  Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw tenant en een backend waarmee u kunnen tokens accepteren en gevalideerd kan verifiëren. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. bepalen welke uw URI omleiden worden voor iOS*

Om te kunnen veilig starten uw toepassingen in bepaalde scenario's voor eenmalige aanmelding vereist dat u een **URI omleiden** in een bepaalde indeling maken. Een URI omleiden wordt gebruikt om ervoor te zorgen dat de tokens terugkeren naar de juiste-toepassing waarin wordt gevraagd om ze.

De iOS-indeling voor een URI omleiden is:

```
<app-scheme>://<bundle-id>
```

-   **aap-schema** - dit in uw project XCode is geregistreerd. Het is hoe andere toepassingen, u kunnen bellen. U vindt deze onder Info.plist -> typen van URL -> URL-id. Als u nog geen een of meer geconfigureerd, moet u een maken.
-   **bundel-id** - dit is de bundel-id aangegeven onder "ID" Schakel de projectinstellingen van uw in XCode.

Een voorbeeld voor deze code Snelstartgids: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. de toepassing DirectorySearcher registreren*
Als u wilt uw app om tokens inschakelen, moet u eerst registreren in uw Azure AD-tenant en deze toestemming voor toegang tot de API Azure AD Graph verlenen:

-   Meld u aan bij de Portal Azure Management
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **Systeemeigen clienttoepassing**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **Uri omleiden** is een combinatie van kleurenschema en de tekenreeks die Azure AD wordt gebruikt om terug te keren token antwoorden.  Voer een bepaalde waarde aan uw toepassing op basis van de bovenstaande gegevens.
-   Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad **configureren** .
- Ook in het tabblad **configureren** , zoek de sectie 'Machtigingen aan andere toepassingen'.  Toevoegen voor de toepassing 'Azure Active Directory' en de **toegang van uw organisatie Directory** -machtigingen onder **Machtigingen gedelegeerde**.  Hiermee schakelt u de toepassing om query's in de grafiek-API voor gebruikers.

## <a name="3-install--configure-adal"></a>*3. installeren en configureren van ADAL*
U kunt nu dat u een toepassing in Azure AD hebt, ADAL installeren en uw identiteit-gerelateerde programmacode schrijven.  In de volgorde voor ADAL moeten kunnen communiceren met Azure AD, moet u deze voorzien van vindt u informatie over de registratie van uw app.
-   Beginnen met het toevoegen van ADAL aan het DirectorySearcher project Cocapods gebruiken.

```
$ vi Podfile
```
De volgende manieren te werk toevoegen aan deze podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Nu de podfile met cocoapods geladen. Hiermee maakt u een nieuwe XCode-werkruimte worden geladen.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Open het bestand plist in het project QuickStart `settings.plist`.  Vervang de waarden van de elementen in de sectie om de waarden die u in de Portal Azure invoert aan te geven.  Uw code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd in de portal.
    -   De `redirectUri` is de omleiding url die u in de portal geregistreerd.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. Gebruik ADAL Tokens vanuit AAD*
Het eenvoudige principe achter ADAL is dat als uw app een toegangstoken moet, een completionBlock aanroept `+(void) getToken : `, en ADAL doet de rest.  

-   In de `QuickStart` project, open `GraphAPICaller.m` en zoek naar de `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` opmerking aan de bovenkant.  Dit is waar u ADAL doorgeven de coördinaten via een CompletionBlock om te communiceren met Azure AD en hoe deze tokens in cache.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Nu moeten we gebruiken deze token om gebruikers in de grafiek te zoeken. Zoek de `// TODO: implement SearchUsersList` commentThis methode zorgt ervoor dat een GET-aanvraag tot de API Azure AD Graph op query voor gebruikers waarvan UPN met het opgegeven zoekterm begint.  Query's op de grafiek-API, moet u een access_token in opnemen, maar de `Authorization` koptekst van de aanvraag - dit is waar ADAL in wordt geleverd.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Wanneer uw app een token aanvraagt door te bellen `getToken(...)`, ADAL wordt geprobeerd om terug te keren een token zonder dat de gebruiker wordt gevraagd om referenties.  Als ADAL vaststelt dat de gebruiker aanmelden moet bij een token ophalen, wordt deze weergeven van een aanmeldingsdialoogvenster, referenties van de gebruiker verzamelen en retourneren een token na verificatie is geslaagd.  Als ADAL kan niet een token retourneren voor welke reden dan ook is, deze in de weergave van een `AdalException`.
- U ziet dat de `AuthenticationResult` object bevat een `tokenCacheStoreItem` object die kan worden gebruikt voor het verzamelen van gegevens u uw app moet mogelijk.  In de snelstartgids, `tokenCacheStoreItem` wordt gebruikt om te bepalen als authenitcation al is opgetreden.


## <a name="step-5-build-and-run-the-application"></a>Stap 5: Maken en uitvoeren van de toepassing



Gefeliciteerd! U nu een iOS-App voor het werken met de mogelijkheid om te verifiëren van gebruikers, veilig bellen Web API's met OAuth 2.0 hebt en basisinformatie over de gebruiker.  Als u nog niet is gedaan, is nu de tijd aan het vullen van uw tenant met enkele gebruikers.  Uw app QuickStart uitvoeren en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van hun UPN.  De app te sluiten en opnieuw worden uitgevoerd.  Zoals u ziet hoe sessie van de gebruiker blijft behouden.

ADAL kunt u heel gemakkelijk al deze algemene identiteit functies opnemen in uw toepassing.  Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer.  Alles wat u moet weten is één aanroep API, `getToken`.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Extra scenario 's
U kunt nu gaat u naar extra scenario's.  U kunt proberen:

- [Een Node.JS Web API met Azure AD Secure](active-directory-devquickstarts-webapi-nodejs.md)
- Informatie over [het inschakelen van eenmalige aanmelding van cross-app op ADAL met iOS](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
