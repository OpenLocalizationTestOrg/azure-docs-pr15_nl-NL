<properties
    pageTitle="Azure Active Directory B2C: Gebruiken in de grafiek API | Microsoft Azure"
    description="Hoe u belt u met de grafiek-API voor een B2C-tenant met behulp van een toepassings-id om het proces te automatiseren."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Gebruiken in de grafiek-API

Azure Active Directory (Azure AD) B2C tenants zijn meestal zeer grote. Dit betekent dat veel veelvoorkomende beheertaken voor tenant moeten worden uitgevoerd via programmacode. Een primaire voorbeeld is Gebruikersbeheer. Mogelijk moet u een bestaande gebruiker store migreren naar een tenant B2C. U kunt hosten gebruikersregistratie op de pagina en gebruikersaccounts maken in Azure AD achter de schermen. Dit soort van taken vereisen de mogelijkheid om te maken, lezen, bijwerken en verwijderen van gebruikersaccounts. U kunt deze taken uitvoeren met behulp van de Azure AD Graph-API.

Voor B2C-tenants, zijn er twee primaire modi communiceren met de API Graph.

- Voor interactieve, eenmalige taken, moet u fungeren als een beheerdersaccount in de tenant B2C wanneer u de taken uitvoeren. In deze modus moet een beheerder aan te melden met referenties voordat deze beheerder oproepen aan de grafiek-API kunt uitvoeren.
- Voor automatische en continue taken zijn, moet u een type serviceaccount dat u opgeeft met de benodigde bevoegdheden beheertaken uitvoeren. In Azure AD, kunt u dit doen door te registreren van een toepassing en geverifieerd bij Azure AD. Dit gebeurt met behulp van een **Toepassings-ID** die wordt gebruikt de [referenties van de client OAuth 2.0 verlenen](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In dit geval fungeert de toepassing als zelf, niet als een gebruiker, de grafiek-API aan te roepen.

In dit artikel wordt besproken hoe u de automatische use-case uitvoert. U kunt zien, we een 4.5 .NET gaat maken `B2CGraphClient` die gebruiker voert maken, lezen, bijwerken en verwijderen (CRUD). De client heeft een Windows opdrachtregel-interface (CLI) waarmee u verschillende methoden. De code is echter geschreven om te werken in een niet-interactieve, geautomatiseerde manier.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een B2C van Azure AD-tenant ophalen

Voordat u kunt maken van toepassingen of gebruikers of interactie met Azure AD helemaal, moet u een B2C van Azure AD-tenant en een account voor globale beheerder in de tenant. Als u niet over een tenant al, [aan de slag met Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Een servicetoepassing registreren in uw tenant

Nadat u een tenant B2C hebt, moet u uw servicetoepassing maken met behulp van Azure AD PowerShell-cmdlets.
Eerst download en installeer de [Microsoft Online Services-aanmeldhulp](http://go.microsoft.com/fwlink/?LinkID=286152). Download en installeer de [64-bits Azure Active Directory-module voor Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Als u wilt de grafiek-API gebruiken met uw tenant B2C, moet u een specifieke toepassing registreren via PowerShell. Volg de instructies in dit artikel dat doen. U kunt de bestaande B2C toepassingen die u hebt geregistreerd in de portal van Azure niet opnieuw gebruiken.

Nadat u de PowerShell-module hebt geïnstalleerd, opent u PowerShell en maak verbinding met uw tenant B2C. Nadat u hebt uitgevoerd `Get-Credential`, wordt u gevraagd voor een gebruikersnaam en wachtwoord, voert u de gebruikersnaam en wachtwoord van uw B2C tenantbeheerderaccount.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Voordat u uw toepassing maken, moet u een nieuwe **geheim client**genereren.  Uw toepassing wordt de geheim client gebruikt om te verifiëren naar Azure AD en aan te schaffen access tokens. U kunt een geldige geheim genereren in PowerShell:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

De opdracht definitief moet uw nieuwe client geheim afdrukken. Kopieert u dit melden. U moet deze later. U kunt nu uw toepassing maken met behulp van de nieuwe client geheim als een referentie voor de app:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Als u de toepassing voor de knop ongedaan maken, moet deze eigenschappen van de toepassing zoals de bovenstaande afdrukken. U moet beide `ObjectId` en `AppPrincipalId`, zodat deze waarden, te kopiëren.

Nadat u een toepassing in uw tenant B2C maakt, moet u de machtigingen voor die deze gebruiker CRUD-bewerkingen uit te voeren nodig toewijzen. De toepassing drie rollen toewijzen: directory lezers (leesbaar gebruikers), schrijvers van de directory (om te maken en bijwerken van gebruikers), en de beheerder van een gebruiker (om gebruikers te verwijderen). Deze functies zijn gerenommeerde id's, zodat u kunt vervangen de `-RoleMemberObjectId` parameter met `ObjectId` dan hierboven en voer de volgende opdrachten. Voor een overzicht van alle directory-functies, probeer uit te voeren `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

U nu een toepassing hebt die daartoe gemachtigd is te maken, lezen, bijwerken en verwijderen van gebruikers van uw tenant B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Downloaden, configureren en maken van de steekproef-code

Eerst downloaden van de steekproef-code en Schaf het uitgevoerd. Vervolgens gaat we een nabij bekeken.  U kunt [de code van de steekproef een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). U kunt dit ook klonen in een map van uw keuze:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Open de `B2CGraphClient\B2CGraphClient.sln` Visual Studio-oplossing in Visual Studio. In de `B2CGraphClient` project, opent u het bestand `App.config`. Instellingen voor de drie app vervangen door uw eigen waarden:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Klik vervolgens met de rechtermuisknop op de `B2CGraphClient` oplossing en opnieuw opbouwen in het voorbeeld. Als u geslaagd bent, u hebt nu een `B2C.exe` uitvoerbaar bestand zich bevindt in `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Gebruiker CRUD-bewerkingen maken met behulp van de grafiek-API

Als u wilt de B2CGraphClient gebruiken, opent u een `cmd` activeert vragen in en u vanuit de map naar de `Debug` directory. Voer de `B2C Help` opdracht.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Een korte beschrijving van elke opdracht wordt weergegeven. Telkens wanneer u een van deze opdrachten roepen `B2CGraphClient` zorgt ervoor dat een verzoek tot de API Azure AD Graph.

### <a name="get-an-access-token"></a>Een toegangstoken ophalen

Elk verzoek tot de API Graph is een toegangstoken voor verificatie vereist. `B2CGraphClient`de open source Active Directory verificatie bibliotheek (ADAL) gebruikt om te helpen in het bezit van access tokens. ADAL vergemakkelijkt token overname door een eenvoudige API en verzorgen van belangrijke informatie, zoals caching van access tokens. U hoeft niet te ADAL gebruiken om tokens, maar. U kunt tokens ook openen door HTTP-aanvragen.

> [AZURE.NOTE]
    In dit voorbeeld wordt ADAL v2 gebruikt om te communiceren met de API Graph.  U moet ADAL v2 of v3 gebruiken om te krijgen van toegangstokens die kunnen worden gebruikt met de API Azure AD Graph.

Wanneer `B2CGraphClient` wordt uitgevoerd, dat wordt gemaakt van een exemplaar van de `B2CGraphClient` class. De constructor voor deze klasse is ingesteld van een steiger ADAL verificatie:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

We gebruiken de `B2C Get-User` command als voorbeeld. Wanneer `B2C Get-User` wordt aangeroepen zonder eventuele extra ingangen, de CLI oproepen de `B2CGraphClient.GetAllUsers(...)` methode. Deze methode roept `B2CGraphClient.SendGraphGetRequest(...)`, die een HTTP GET-aanvraag tot de API Graph verstuurt. Voordat u `B2CGraphClient.SendGraphGetRequest(...)` verzendt de GET-aanvragen, deze eerst wordt een access token met behulp van ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

U kunt een toegang krijgen token voor de grafiek-API door de ondersteuning voor de ADAL `AuthenticationContext.AcquireToken(...)` methode. ADAL wordt als resultaat een `access_token` die aangeeft dat de identiteit van de toepassing.

### <a name="read-users"></a>Alleen gebruikers

Als u een lijst met gebruikers of een bepaalde gebruiker krijgen van de grafiek-API wilt, kunt u een HTTP verzenden `GET` verzoek om deel te de `/users` eindpunt. Een verzoek voor alle gebruikers in een tenant ziet er zo uit:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Als u wilt deze aanvraag wordt weergegeven, voert u het:

 ```
 > B2C Get-User
 ```

Er zijn twee belangrijke punten onthouden:

- Het toegangstoken opgehaald via ADAL wordt toegevoegd aan de `Authorization` koptekst met behulp van de `Bearer` kleurenschema.
- Voor B2C-tenants, moet u de queryparameter `api-version=1.6`.

Beide van deze gegevens worden verwerkt in de `B2CGraphClient.SendGraphGetRequest(...)` methode:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Consumenten gebruikersaccounts maken

Als u gebruikersaccounts in uw tenant B2C maken, kunt u een HTTP verzenden `POST` verzoek om deel te de `/users` eindpunt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

De meeste van deze eigenschappen in deze aanvraag vereist voor het maken van consumenten gebruikers. Meer informatie, klikt u op [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Houd er rekening mee dat het `//` opmerkingen zijn opgenomen voor een afbeelding. Neem deze niet in een werkelijke staat.

Als u wilt zien van de aanvraag, voert u een van de volgende opdrachten:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

De `Create-User` opdracht wordt een bestand .json als invoerparameter. Dit document bevat een JSON-weergave van een gebruikersobject. Er zijn twee steekproeven .json bestanden in de steekproef-code: `usertemplate-email.json` en `usertemplate-username.json`. U kunt deze bestanden aan uw behoeften aanpassen. Naast de bovenstaande vereiste velden vindt u verschillende optionele velden die u kunt gebruiken in deze bestanden. Meer informatie over de optionele velden vindt u in de [verwijzing naar Azure AD Graph API entiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

U kunt zien hoe de POST-aanvraag is gebouwd `B2CGraphClient.SendGraphPostRequest(...)`.

- Deze gevoegd een toegangstoken naar de `Authorization` koptekst van de aanvraag kunt invullen.
- Wordt ingesteld `api-version=1.6`.
- Het bevat de JSON-gebruikersobject in de hoofdtekst van het verzoek.

> [AZURE.NOTE]
Als de accounts die u wilt migreren van een bestaande gebruiker winkel heeft minder wachtwoord kracht dan [sterk Wachtwoordsterkte afgedwongen door Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kunt u uitschakelen de sterk wachtwoord vereiste gebruiken de `DisableStrongPassword` waarde in de `passwordPolicies` eigenschap. Bijvoorbeeld, kunt u een aanvraag voor het maken gebruiker hierboven beschreven als volgt wijzigen: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Gebruikersaccounts voor consumenten bijwerken

Wanneer u gebruikersobjecten bijwerkt, is het proces vergelijkbaar met het account dat u kunt gebruikersobjecten maken. Maar dit proces wordt gebruikt voor de HTTP `PATCH` methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Probeer het bijwerken van een gebruiker door uw JSON-bestanden met nieuwe gegevens bij te werken. U kunt `B2CGraphClient` een van deze opdrachten uit te voeren:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Controleren het `B2CGraphClient.SendGraphPatchRequest(...)` methode voor meer informatie over het verzenden van dit verzoek.

### <a name="search-users"></a>Gebruikers van de zoekfunctie

U kunt zoeken voor gebruikers in uw tenant B2C op een aantal manieren oplossen. Een van de gebruiker met object-ID of twee, met behulp van de gebruiker aanmeldingsproblemen id (dat wil zeggen de `signInNames` eigenschap).

Voer een van de volgende opdrachten om te zoeken naar een specifieke gebruiker:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Hier volgen een paar voorbeelden van:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Gebruikers verwijderen

Het proces voor het verwijderen van een gebruiker is eenvoudig. Gebruik de HTTP `DELETE` methode en constructie de URL met de juiste object-ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Een voorbeeld bekijken, voer de volgende opdracht en bekijk de aanvraag voor het verwijderen die wordt afgedrukt naar de console:

```
> B2C Delete-User <object-id-of-user>
```

Controleren het `B2CGraphClient.SendGraphDeleteRequest(...)` methode voor meer informatie over het verzenden van dit verzoek.

U kunt allerlei andere acties met de API Azure AD Graph naast Gebruikersbeheer uitvoeren. De [Azure AD Graph API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) bevat informatie op elke actie, samen met de steekproef aanvragen.

## <a name="use-custom-attributes"></a>Aangepaste kenmerken gebruiken

De meeste consumenten toepassingen moeten een type aangepaste gebruikersprofielinformatie opslaan. Er is een manier kunt u dit doen definiëren van een aangepaste kenmerk in uw tenant B2C. U kunt dat kenmerk vervolgens dezelfde manier als u andere eigenschappen voor een gebruikersobject behandelen behandelen. U kunt het kenmerk bijwerken, verwijdert u het kenmerk, query door het kenmerk, het kenmerk verzenden als een claim in aanmeldingsproblemen tokens en meer.

Als u wilt een aangepaste kenmerk in uw tenant B2C definieert, raadpleegt u [B2C aangepaste kenmerkverwijzing](active-directory-b2c-reference-custom-attr.md).

U kunt de aangepaste kenmerken in uw tenant B2C gedefinieerd door middel van weergeven `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

De uitvoer van deze functies blijkt de details van alle aangepaste kenmerken, zoals:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

U kunt de volledige naam, zoals `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als een eigenschap op uw gebruikersobjecten.  Het bestand .json bijwerken met de nieuwe eigenschap en een waarde voor de eigenschap en vervolgens uitvoeren:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Met behulp van `B2CGraphClient`, u hebt een servicetoepassing dat uw gebruikers van de tenant B2C via een programma kunt beheren. `B2CGraphClient`een eigen toepassings-id gebruikt om te verifiëren tot de API Azure AD Graph. Deze ook krijgt tokens met behulp van een client geheim. Als u deze functionaliteit in uw toepassing opnemen, houd rekening met een paar hoofdpunten voor B2C apps:

- U moet de juiste machtigingen in de tenant van de toepassing verlenen.
- Voorlopig moet u ADAL v2 gebruiken om te krijgen van toegangstokens. (U kunt ook protocolberichten verzenden rechtstreeks, zonder een bibliotheek.)
- Wanneer u de grafiek-API belt, gebruikt u `api-version=1.6`.
- Wanneer u maken en bijwerken van consumenten gebruikers, zijn een paar eigenschappen vereist, zoals hierboven is beschreven.

Als u vragen of verzoeken om acties die u wilt uitvoeren met behulp van de grafiek-API op uw tenant B2C hebt, kan het beter in dit artikel bestanden of een probleem in de bibliotheek GitHub code steekproef.
