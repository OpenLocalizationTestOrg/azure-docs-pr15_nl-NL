<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Het maken van een .NET Web API met behulp van Azure Active Directory B2C, beveiligd met behulp van OAuth 2.0 access tokens voor verificatie."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory-B2C: Een .NET web API maken

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met Azure Active Directory (Azure AD) B2C, kunt u een web API beveiligen door het OAuth 2.0 access tokens gebruiken. Deze tokens toestaan uw client-apps die gebruikmaken van Azure AD B2C om te verifiëren tot de API. In dit artikel leest u het maken van een .NET-Model-View-Controller (MVC) "takenlijst" API waarmee gebruikers kunnen CRUD-taken. Het web API is beveiligd met Azure AD B2C en kan alleen geverifieerde gebruikers hun takenlijst beheren.

## <a name="create-an-azure-ad-b2c-directory"></a>Een AD-B2C Azure-directory maken

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant. Een map is een container voor al uw gebruikers, apps, groepen en meer. Als u niet een al hebt, [Maak een map B2C](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken. Zorg ervoor dat:

- Een **WebApp** of **web API** opnemen in de toepassing.
- Gebruik de **uniform resource identifier omleiden** `https://localhost:44316/` voor de web-app. Dit is de standaardlocatie van de web-app-client voor dit voorbeeld.
- Kopieer de **toepassings-ID** die is toegewezen aan uw app. U moet deze later.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). De client in dit voorbeeld bevat drie identiteit ervaringen: registreren, aanmelden en profiel bewerken. U moet maken van een beleid voor elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies **Gebruikers-ID aanmelden** of **registreren E-mail** in het blad identiteit-providers.
- Kies **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies **weergavenaam** en **Object-ID** claims als toepassing vorderingen die voortvloeien uit elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. U hebt de volgende namen beleid later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u bent uw app maken.

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Maak de steekproef terwijl u door kunt u [een skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het bestand Visual Studio .sln aan de slag. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp`een webtoepassing MVC waarin de gebruiker interactie met is. `TaskService`is van de app back-enddatabase web API die worden opgeslagen van elke gebruiker takenlijst te klikken.

## <a name="configure-the-task-web-app"></a>De taak WebApp configureren

Wanneer een gebruiker met communiceert `TaskWebApp`, de client verzendt aanvragen naar Azure AD en krijgt terug tokens die kunnen worden gebruikt om te bellen de `TaskService` API met webonderdelen. Als u wilt de gebruiker melden en kennis van tokens, moet u bieden `TaskWebApp` met vindt u informatie over uw app. In de `TaskWebApp` project, open de `web.config` bestand in de hoofdmap van het project en vervangen van de waarden in de `<appSettings>` sectie.  Laat u de `AadInstance`, `RedirectUri`, en `TaskServiceUrl` als een waarde-is.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

In dit artikel niet besproken building de `TaskWebApp` client.  Als u wilt weten hoe u een WebApp met Azure AD B2C maken, raadpleegt u [onze zelfstudie .NET web app](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>De API Secure

Wanneer u een client die de API-namens gebruikers oproepen hebt, kunt u beveiligen `TaskService` met behulp van OAuth 2.0 vruchtdragende tokens. Uw API kan accepteren en tokens valideren met behulp van Microsoft Open Web Interface voor .NET (OWIN)-bibliotheek.

### <a name="install-owin"></a>OWIN installeren
Beginnen met het installeren van de pijplijn OWIN OAuth-verificatie:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Geef de details van uw B2C
Open de `web.config` bestand in de hoofdmap van de `TaskService` project en vervangen van de waarden in de `<appSettings>` sectie. Deze waarden worden gebruikt in de bibliotheek API en OWIN.  Laat u de `AadInstance` waarde ongewijzigd.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Een OWIN Opstartklasse toevoegen
Een OWIN opstarten toevoegen aan de `TaskService` project genoemd `Startup.cs`.  Met de rechtermuisknop op het project, selecteer **toevoegen** en **Nieuw Item**en zoek vervolgens naar OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0-verificatie configureren
Open het bestand `App_Start\Startup.Auth.cs`, en implementeren de `ConfigureAuth(...)` methode:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>De taakcontroller Secure
Nadat de app is geconfigureerd voor het OAuth 2.0-verificatie gebruikt, kunt u uw web API beveiligen door het toevoegen van een `[Authorize]` tag op de taakcontroller. Dit is de controller waar alle Takenbalk lijst bewerken plaatsvindt, dus moet u de hele controller uit op het niveau van class beveiligen. U kunt ook toevoegen de `[Authorize]` tag aan afzonderlijke acties voor meer fijnmazige controle.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Gebruikersgegevens ophalen uit het token
`TasksController`Er worden taken opgeslagen in een database waarin elke taak een gekoppelde gebruiker van wie "" de taak heeft. De eigenaar van de wordt aangegeven door de gebruiker zijn **object-ID**. (Dit is de reden waarom u die nodig zijn voor het toevoegen van de object-ID als een toepassing claim in alle van uw beleid.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Tot slot maken en uitvoeren van beide `TaskWebApp` en `TaskService`. Zich registreren voor de app met behulp van een e-mailadres of de gebruikersnaam. Sommige taken maken in de takenlijst van de gebruiker, waarna u ziet hoe ze worden doorgevoerd in de API zelfs nadat u stoppen en opnieuw starten van de client.

## <a name="edit-your-policies"></a>Uw beleid bewerken

Nadat u hebt een API beveiligd met behulp van Azure AD B2C, kunt u experimenteren met de beleidsregels van uw app en weergeven van de effecten (of ontbreken daarvan) op de API. U kunt de toepassing op claims in het beleid bewerken en wijzig de gegevens van de gebruiker die beschikbaar is in het web API. Een op claims die u toevoegt, zijn beschikbaar voor uw .NET MVC web API in de `ClaimsPrincipal` object, zoals eerder in dit artikel beschreven.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
