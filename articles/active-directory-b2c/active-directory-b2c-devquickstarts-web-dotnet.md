<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Het maken van een webtoepassing met aanmelden, aanmelding, en beheer van gebruikersprofielen met behulp van Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-build-a-net-web-app"></a>Azure AD B2C: Een .NET-web-app maken

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met behulp van Azure Active Directory (Azure AD) B2C, kunt u krachtige selfservice-identiteit beheerfuncties toevoegen aan uw web-app in een paar korte stappen. In dit artikel wordt beschreven hoe u het maken van een web-app voor .NET-Model-View-Controller (MVC) waarin gebruiker aanmelding, aanmelden en beheer van gebruikersprofielen. De app biedt ondersteuning voor aanmelding en aanmelden met behulp van een gebruikersnaam in te voeren of e-mailbericht en met behulp van de accounts van sociale zoals Facebook en Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant. Een map is een container voor al uw gebruikers, apps, groepen en meer.  Als u niet een al hebt, [Maak een map B2C](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken.  Zorg ervoor dat:

- Een **web-app/web API** opnemen in de toepassing.
- Voer `https://localhost:44316/` als een **URI omleiden**. De standaard-URL voor dit voorbeeld is.
- Kopieer omlaag de **Toepassings-ID** die is toegewezen aan uw app.  Moet u deze later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). In dit voorbeeld bevat drie identiteit ervaringen: registreren, aanmelden en profiel bewerken. U moet maken van één beleid van elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).

>[AZURE.NOTE] Azure AD B2C ondersteunt ook een gecombineerde registreren of teken in beleid die niet in deze zelfstudie wordt aanbevolen.  Het teken omhoog of aanmelden beleid wordt weergegeven in [deze zelfstudie equivalente](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies **Gebruikers-ID registreren** of het **e-aanmelding** in het blad identiteit-providers.
- Kies de **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies de **naam weer te geven** claim als een toepassing claim in elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. U hebt beleid namen later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u uw drie beleidsregels hebt gemaakt, kunt u bent uw app maken.  

## <a name="download-the-code-and-configure-authentication"></a>Downloaden van de code en verificatie configureren

De code voor dit voorbeeld [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet). Maak de steekproef terwijl u door kunt u [het skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

De voltooide steekproef is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het bestand Visual Studio .sln aan de slag.

Uw app communiceert met Azure AD B2C door te sturen verificatieberichten die het beleid dat ze wilt uitvoeren als onderdeel van de HTTP-aanvraag opgeeft. Voor .NET-webtoepassingen, kunt u Microsoft OWIN middleware voor het verzenden van aanvragen voor verificatie OpenID verbinding maken, uitvoeren beleidsregels, beheren sessies en meer.

Als u wilt beginnen, moet u de OWIN middleware NuGet-pakketten toevoegen aan het project met behulp van de Visual Studio Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

Open vervolgens de `web.config` bestand in de hoofdmap van het project en voer waarden in de configuratie van uw app in de `<appSettings>` sectie, worden vervangen door de bestaande waarden.

```
<configuration>
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
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Voeg nu een OWIN Opstartklasse aan het project genoemd `Startup.cs`. Met de rechtermuisknop op het project, selecteer **toevoegen** en **Nieuw Item**en zoek vervolgens naar "OWIN." **Controleer of de klassendeclaratie om te wijzigen `public partial class Startup` **. We deel uit van deze klasse geïmplementeerd voor u in een ander bestand. De middleware OWIN wordt aangeroepen de `Configuration(...)` methode wanneer de app wordt gestart. In deze methode bellen naar `ConfigureAuth(...)`, waar u verificatie voor de app instellen.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Open het bestand `App_Start\Startup.Auth.cs` en implementeren de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` fungeren als coördinaten voor uw app kunt u communiceren met Azure AD. U moet ook Cookieverificatie instellen. De middleware OpenID verbinding gebruikt cookies zodat sessies, onder andere.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignUpPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(ProfilePolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignInPolicyId));
    }

    // Used for avoiding yellow-screen-of-death
    private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        notification.HandleResponse();
        if (notification.Exception.Message == "access_denied")
        {
            notification.Response.Redirect("/");
        }
        else
        {
            notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
        }

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
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
                AuthenticationFailed = AuthenticationFailed,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
```

## <a name="send-authentication-requests-to-azure-ad"></a>Verificatieaanvragen verzenden naar Azure AD
Uw app is nu geconfigureerd om te communiceren met Azure AD B2C met behulp van het protocol verificatie OpenID verbinding maken.  OWIN heeft die u hebt gemaakt om ervoor te zorgen alle details van besteed hier verificatieberichten, tokens afgeleid van Azure AD valideren en onderhouden van gebruikerssessie.  Alle blijft is van elke gebruiker stroom initiëren.

Wanneer een gebruiker **registreren**, **Meld u aan**of **profiel bewerken** in de WebApp selecteert, de bijbehorende actie wordt aangeroepen in `Controllers\AccountController.cs`. In beide gevallen kunt u ingebouwde OWIN methoden voor het starten van het juiste beleid:

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties () { RedirectUri = "/" }, Startup.SignInPolicyId);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SignUpPolicyId);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.ProfilePolicyId);
    }
}
```

U kunt ook een `[Authorize]` label in uw domeincontrollers waarvoor de uitvoering van een bepaalde beleid als de gebruiker is niet aangemeld. Open `Controllers\HomeController.cs` en voeg de `[Authorize]` tag op de controller claims.  OWIN selecteert het laatste beleid geconfigureerd om uit te voeren wanneer de tag autoriseren wordt gestart.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a policy if the user is not already signed in the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

U kunt ook OWIN aan de gebruiker vanuit de app afmelden. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Gebruikersgegevens weergeven
Wanneer u gebruikers verifiëren via OpenID Connect, retourneert Azure AD een token ID bij de app met **claims**. Hierna ziet u bevestigingen over de gebruiker. U kunt claims personaliseren van uw app.  

Open de `Controllers\HomeController.cs` bestand. U hebt toegang tot claims van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

U kunt een claim die uw toepassing op dezelfde manier krijgt openen.  Een lijst met alle claims ontvangt van de app is beschikbaar voor u op de pagina **Claims** .

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

U kunt tot slot maken en uitvoeren van uw app. Zich registreren voor de app met behulp van de naam van een e-mailadres of de gebruiker. Meld u af en meld u weer aan als de gebruiker. Van die gebruiker profiel bewerken. Meld u af en meld u aan als een andere gebruiker. Houd er rekening mee dat de informatie die wordt weergegeven op het tabblad **Claims** hoort bij de informatie die u voor uw beleid hebt geconfigureerd.

## <a name="add-social-idps"></a>Sociale IDPs toevoegen

De app ondersteunt momenteel alleen gebruiker aanmeldings- en aanmelden met behulp van **lokale accounts**. Dit zijn opgeslagen in de map B2C accounts met een gebruikersnaam en wachtwoord. U kunt met behulp van Azure AD B2C ondersteuning voor andere **identiteitsproviders** (IDPs) toevoegen zonder een van uw code.

Om toe te voegen sociale IDPs naar uw app, gaat u eerst de gedetailleerde instructies in deze artikelen te volgen. Voor elke IDP moeten worden ondersteund, moet u een toepassing in dat systeem registreren en verkrijgen van een client-ID.

- [Facebook instellen als een IDP](active-directory-b2c-setup-fb-app.md)
- [Google instellen als een IDP](active-directory-b2c-setup-goog-app.md)
- [Amazon instellen als een IDP](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn instelt als een IDP](active-directory-b2c-setup-li-app.md)

Nadat u de identiteitsprovider aan uw adreslijst B2C toevoegt, moet u elk van uw drie beleid voor het opnemen van de nieuwe IDPs, bewerken, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md). Nadat u uw beleid opslaat, voert u de app opnieuw uit.  Ziet u de nieuwe IDPs toegevoegd als aanmelden en aanmeldingsopties in elk van uw identiteit ervaringen.

U kunt experimenteren met uw beleid en houd rekening met het effect op uw voorbeeld-app. Toevoegen of verwijderen van IDPs, manipuleren van toepassing op claims of aanmelding kenmerken wijzigen. Experimenteren totdat u kunt zien hoe beleidsregels, verificatieaanvragen en OWIN met elkaar verbinden.

Voor verwijzing, in de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip). U kunt dit ook klonen van GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
