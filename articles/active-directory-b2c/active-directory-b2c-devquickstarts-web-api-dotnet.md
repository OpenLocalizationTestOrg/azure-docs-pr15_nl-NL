<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Het maken van een webtoepassing die een web API-oproepen via Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Een web API bellen vanuit een .NET-web-app

Met behulp van Azure Active Directory (Azure AD) B2C, kunt u krachtige selfservice-identiteit beheerfuncties toevoegen aan uw WebApps en web API's in een paar korte stappen. In dit artikel wordt beschreven hoe u een web-app voor .NET-Model-View-Controller (MVC) "takenlijst' die een web API-oproepen via vruchtdragende tokens maken

In dit artikel besproken niet hoe u implementeert aanmeldingsproblemen, aanmelding en Profielbeheer met Azure AD B2C. Dit is gericht op bellen web API's nadat de gebruiker al is geverifieerd. Als u nog niet is gedaan, moet u beginnen met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant.  Een map is een container voor alle gebruikers, apps, groepen en meer.  Als u niet een al hebt, [Maak een map B2C](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. In dit geval wordt de WebApp en de web API aangegeven door een enkel **Toepassings-ID**, omdat ze bestaan uit één logische app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken. Zorg ervoor dat:

- Een **web-app/web API** opnemen in de toepassing.
- Voer `https://localhost:44316/` als een **antwoord-URL**. De standaard-URL voor dit voorbeeld is.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze WebApp bevat drie identiteit ervaringen: registreren, aanmelden en profiel bewerken. U moet maken van één beleid van elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies de **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies de **naam weer te geven** en de **Object-ID** -toepassing claims in elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`. U hebt beleid namen later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u uw drie beleidsregels hebt gemaakt, kunt u bent uw app maken.

Houd er rekening mee dat in dit artikel niet hoe besproken u het gebruik van het beleid die u zojuist hebt gemaakt. Meer informatie over de werking van beleidsregels in Azure AD B2C, begint u met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Maak de steekproef terwijl u door kunt u [het skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het bestand Visual Studio .sln aan de slag.

## <a name="configure-the-task-web-app"></a>De taak WebApp configureren

Om `TaskWebApp` om te communiceren met Azure AD B2C, moet u een paar algemene parameters. In de `TaskWebApp` project, open de `web.config` bestand in de hoofdmap van het project en vervangen van de waarden in de `<appSettings>` sectie. Laat u de `AadInstance`, `RedirectUri`, en `TaskServiceUrl` waarden zoals ze zijn.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Access tokens krijgen en de taak API bellen

In deze sectie wordt beschreven hoe de token ontvangen tijdens het aanmelden met Azure AD B2C gebruiken om toegang te krijgen tot de API die ook is beveiligd met Azure AD B2C van een webpagina.

De details van het beveiligen van de API besproken in dit artikel niet. Meer informatie over hoe een web API veilig wordt geverifieerd aanvragen met behulp van Azure AD B2C, raadpleegt u de [web API aan de slag artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Het teken in token opslaan

Eerst verificatie van de gebruiker (via een van uw beleid) en u ontvangt een token aanmeldingsproblemen van Azure AD B2C.  Als u niet zeker weet hoe u het uitvoeren van beleidsregels, gaat u terug en probeert u de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

Open het bestand `App_Start\Startup.Auth.cs`.  Er is een belangrijke wijziging, moet u aanbrengen in de `OpenIdConnectAuthenticationOptions` -moet u instellen `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

return new OpenIdConnectAuthenticationOptions
{
    // For each policy, give OWIN the policy-specific metadata address, and
    // set the authentication type to the id of the policy
    MetadataAddress = String.Format(aadInstance, tenant, policy),
    AuthenticationType = policy,

    // These are standard OpenID Connect parameters, with values pulled from web.config
    ClientId = clientId,
    RedirectUri = redirectUri,
    PostLogoutRedirectUri = redirectUri,
    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Een token in de controllers ophalen

De `TasksController` verantwoordelijk is voor het communiceren met het web API, HTTP-aanvragen tot de API om te lezen, maken en verwijderen van taken verzenden.  Becuase die de API door Azure AD B2C is beveiligd, moet u eerst het token die u hebt opgeslagen in de bovenstaande stap ophalen.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

De `BootstrapContext` bevat de aanmelden token die u hebt aangeschaft door een van uw beleid B2C uit te voeren.

### <a name="read-tasks-from-the-web-api"></a>Taken van het web API lezen

Wanneer u een token hebt, kunt u deze toevoegen aan de HTTP `GET` aanvragen de `Authorization` koptekst veilig bellen `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Maken en verwijderen van taken op het web API

Ga als volgt het patroon dat in dezelfde wanneer u verzendt `POST` en `DELETE` aanvragen voor het web API, met de `BootstrapContext` om op te halen de aanmelden token. We geïmplementeerd waarvoor u de actie maken. U kunt de verwijderactie in afronden `TasksController.cs`.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Tot slot maken en uitvoeren van de app. Registreren en meld u aan taken maken voor de gebruiker die zijn aangemeld. Meld u af en meld u aan als een andere gebruiker. Taken voor die gebruiker maken. Zoals u ziet hoe de taken die zijn opgeslagen per gebruiker op de API, omdat de API haalt de gebruikers id uit het token dat deze ontvangt.

Voor verwijzing, in de voltooide voorbeeld [wordt weergegeven als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). U kunt dit ook klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
