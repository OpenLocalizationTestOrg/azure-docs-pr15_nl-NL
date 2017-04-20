<properties
    pageTitle="Azure AD .NET aan de slag | Microsoft Azure"
    description="Het maken van een Web-App voor .NET MVC geïntegreerd met Azure AD voor het aanmelden."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET-Web-App aanmelden en afmelden met Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD kunt u eenvoudige en duidelijke naar uw web-app identiteitsbeheer, uitbesteden leveren één aanmelden en afmelden met slechts een paar regels met code.  In Asp.NET-web-apps, kunt u dit doen met Microsoft implementatie van de OWIN community op basis van hoeveelheid werk middleware opgenomen in .NET Framework 4.5.  Hier gebruiken we OWIN naar:
-   Meld u aan de gebruiker in de app met Azure AD als de identiteitsprovider.
-   Vindt u informatie over de gebruiker weergeven.
-   Meld u aan de gebruiker afmelden bij de app.

Hiervoor moet u:

1. Een toepassing met Azure AD registreren
2. Uw app instellen voor het gebruik van de pijplijn OWIN-verificatie.
3. Gebruik OWIN op aanmelden en afmelden aanvragen op Azure AD.
4. Gegevens over de gebruiker te drukken.

Om gestart, [de app-skelet downloaden](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  U moet ook een Azure AD-tenant waarin uw toepassing registreren.  Als u geen sjabloon hebt, [leren hoe u een](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure AD registreren*
Als u wilt dat uw app om gebruikers te verifiëren, moet u eerst een nieuwe toepassing registreren in uw tenant.

- Meld u aan bij de Portal Azure Management.
- Klik in de navigatiebalk linkerpagina op **Active Directory**.
- Selecteer de tenant waar u wilt registreren van de toepassing.
- Klik op het tabblad **toepassingen** en klik op toevoegen in de onder-vel.
- Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
    - De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **URL voor eenmalige aanmelding** is de basis-URL van uw app.  De standaardwaarde van het skelet is `https://localhost:44320/`.
    - De **App-ID-URI** is een unieke id voor uw toepassing.  De overeenkomst is via `https://<tenant-domain>/<app-name>`, bijvoorbeeld`https://contoso.onmicrosoft.com/my-first-aad-app`
- Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad configureren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. uw app instellen voor het gebruik van de pijplijn OWIN-verificatie*
Hier wordt we de middleware OWIN voor gebruik van de verificatie OpenID verbinding protocol configureren.  OWIN worden gebruikt op aanmelden en afmelden verzoeken en informatie over de gebruiker, onder andere sessie van de gebruiker beheren.

-   Toevoegen als u wilt beginnen, de OWIN middleware NuGet pakketten aan het project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Een klasse OWIN opstarten toevoegen aan het project genoemd `Startup.cs` rechts Klik op het project-- **toevoegen**> --> **Nieuw Item** --> zoeken naar 'OWIN'.  De middleware OWIN wordt aangeroepen de `Configuration(...)` methode wanneer de app wordt gestart.
-   Wijzigen van de declaratie class naar `public partial class Startup` -we hebben al deel uitmaken van deze klasse geïmplementeerd voor u in een ander bestand.  In de `Configuration(...)` methode, een oproep door naar ConfgureAuth(...) verificatie voor uw web-app instellen  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIDConnectAuthenticationOptions` fungeert als coördinaten voor uw app kunt u communiceren met Azure AD.  U moet ook Cookie-verificatie instellen - de middleware OpenID verbinding maken met cookies onder de voorbladen worden gebruikt.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Ten slotte, opent u de `web.config` bestand in de hoofdmap van het project en voer uw configuratiewaarden in de `<appSettings>` sectie.
    -   Van uw app `ida:ClientId` is de Guid die u hebt gekopieerd in de Portal Azure in stap 1.
    -   De `ida:Tenant` is de naam van uw Azure AD-tenant, bijvoorbeeld "contoso.onmicrosoft.com".
    -   Uw `ida:PostLogoutRedirectUri` geeft aan dat Azure AD waarin een gebruiker wordt omgeleid nadat een verzoek voor een afmelden succes is voltooid.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Gebruik OWIN op aanmelden en afmelden aanvragen op Azure AD*
Uw app is nu geconfigureerd om te communiceren met Azure AD via het protocol OpenID verbinding maken.  OWIN heeft die u hebt gemaakt om ervoor te zorgen alle niet uit details van besteed hier verificatieberichten, tokens afgeleid van Azure AD valideren en onderhouden van gebruikerssessie.  Alle die blijft is om uw gebruikers een manier aanmelden en afmelden.

- U kunt labels in uw domeincontrollers vereisen die gebruiker zich aanmeldt voor de toegang tot een bepaalde pagina Autoriseer.  Open `Controllers\HomeController.cs`, en voeg de `[Authorize]` label om de besturing over te geven.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   U kunt ook OWIN gebruiken om rechtstreeks aanmeldingsaanvragen van de verificatie van uw code.  Open `Controllers\AccountController.cs`.  Actie respectievelijk OpenID verbinding vraag- en afmelden aanvragen, in de acties SignIn() en SignOut().

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Open nu `Views\Shared\_LoginPartial.cshtml`.  Dit is waar u de gebruiker van uw app aanmelden en afmelden koppelingen weergeven en de naam van de gebruiker in een weergave af te drukken.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="4--display-user-information"></a>*4. gebruikersgegevens weergeven*
Bij het verifiëren van gebruikers met OpenID verbinden, retourneert Azure AD een id_token naar het programma waarin "claims" of bevestigingen over de gebruiker.  U kunt deze claims gebruiken om af te stemmen van uw app:

- Open de `Controllers\HomeController.cs` bestand.  U hebt toegang tot claims van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Tot slot maken en uitvoeren van uw app!  Als u nog niet is gedaan, nu is de tijd om te maken van een nieuwe gebruiker in uw tenant met een *. onmicrosoft.com-domein.  Meld u aan met die gebruiker en ziet u hoe de identiteit van de gebruiker is doorgevoerd in de bovenste navigatiebalk.  Meld u af en meld u weer aan als een andere gebruiker in uw tenant is ingeschakeld.  Als u bent gevoel vooral ambitieuze, registreren en een ander exemplaar van deze toepassing (met een eigen clientId) en controle Zie eenmalige aanmelding op in actie uitvoeren.

Voor verwijzing, in de voltooide steekproef (zonder de configuratiewaarden) [hier is opgegeven](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

U kunt nu verplaatsen naar meer geavanceerde onderwerpen.  U kunt proberen:

[Een Web API met Azure AD Secure >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
