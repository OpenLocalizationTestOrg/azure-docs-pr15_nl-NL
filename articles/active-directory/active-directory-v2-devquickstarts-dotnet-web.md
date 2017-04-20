<properties
    pageTitle="Azure AD v2.0 .NET Web-App | Microsoft Azure"
    description="Het maken van een Web-App voor .NET MVC gebruikers met beide persoonlijk Microsoft-Account ondertekent en accounts voor werk- of schoolaccount."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Aanmelden bij een .NET MVC web-app toevoegen

Met het eindpunt v2.0, kunt u snel verificatie naar uw WebApps gebruiken met ondersteuning voor beide persoonlijk Microsoft-accounts en werk- of schoolaccount accounts toevoegen.  In ASP.NET-web-apps, kunt u dit doen met Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

 Hier wordt we maken op een web-app waarin OWIN voor de gebruiker aanmelden, vindt u informatie over de gebruiker weergeven en meld u aan de gebruiker afmelden bij de app wordt gebruikt.
 
 ## <a name="download"></a>Downloaden
De code voor deze zelfstudie worden [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

De voltooide app wordt aangeboden aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- De **Web** -platform voor de app toevoegen.
- Voer het juiste **URI omleiden**. De omleidings-uri geeft aan dat Azure AD waar verificatie antwoorden moeten worden doorgestuurd: de standaardinstelling voor deze zelfstudie is `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installeren en configureren van OWIN verificatie
Hier wordt we de middleware OWIN voor gebruik van de verificatie OpenID verbinding protocol configureren.  OWIN worden gebruikt op aanmelden en afmelden verzoeken en informatie over de gebruiker, onder andere sessie van de gebruiker beheren.

-   Als u wilt beginnen, opent u de `web.config` bestand in de hoofdmap van het project en voer waarden in de configuratie van uw app in de `<appSettings>` sectie.
    -   De `ida:ClientId` is de **Toepassings-Id** die is toegewezen aan uw app in de registratie-portal.
    -   De `ida:RedirectUri` is de **Uri omleiden** die u hebt ingevoerd in de portal.

-   Voeg vervolgens de OWIN middleware NuGet-pakketten aan het project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Een "OWIN Opstartklasse toevoegen ' aan het project genoemd `Startup.cs` rechts Klik op het project--> **toevoegen** --> **Nieuw Item** --> zoeken naar 'OWIN'.  De middleware OWIN wordt aangeroepen de `Configuration(...)` methode wanneer de app wordt gestart.
-   Wijzigen van de declaratie class naar `public partial class Startup` -we hebben al deel uitmaken van deze klasse geïmplementeerd voor u in een ander bestand.  In de `Configuration(...)` methode, een oproep door naar ConfigureAuth(...) verificatie voor uw web-app instellen  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` fungeert als coördinaten voor uw app kunt u communiceren met Azure AD.  U moet ook Cookie-verificatie instellen - de middleware OpenID verbinding maken met cookies onder de voorbladen worden gebruikt.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Verificatie-aanvragen verzenden
Uw app is nu geconfigureerd om te communiceren met het v2.0 eindpunt via het protocol OpenID verbinding maken.  OWIN heeft die u hebt gemaakt om ervoor te zorgen alle niet uit details van besteed hier verificatieberichten, tokens afgeleid van Azure AD valideren en onderhouden van gebruikerssessie.  Alle die blijft is om uw gebruikers een manier aanmelden en afmelden.

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

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Open nu `Views\Shared\_LoginPartial.cshtml`.  Dit is waar u de gebruiker van uw app aanmelden en afmelden koppelingen weergeven en de naam van de gebruiker in een weergave af te drukken.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
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

## <a name="display-user-information"></a>Gebruikersgegevens weergeven
Bij het verifiëren van gebruikers met OpenID verbinden, retourneert het eindpunt v2.0 een id_token bij de app met [claims](active-directory-v2-tokens.md#id_tokens)of bevestigingen over de gebruiker.  U kunt deze claims gebruiken om af te stemmen van uw app:

- Open de `Controllers\HomeController.cs` bestand.  U hebt toegang tot claims van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` beveiligings-principal.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Uitvoeren

Tot slot maken en uitvoeren van uw app!   Meld u aan met een persoonlijk Microsoft-Account of een account voor werk of school en ziet u hoe de identiteit van de gebruiker is doorgevoerd in de bovenste navigatiebalk.  U hebt nu een web-app beveiligd met industriële standaard protocollen die gebruikers met hun persoonlijke en werk-, school account kunnen worden geverifieerd.

Voor een verwijzing naar kunt de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)of u klonen deze uit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar meer geavanceerde onderwerpen.  U kunt proberen:

[Een Web-API met Secure de het eindpunt v2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Raadpleeg voor meer bronnen:
- [De handleiding voor ontwikkelaars van v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure active directory" tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
