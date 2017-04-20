<properties
    pageTitle="Azure AD v2.0 .NET Web-App | Microsoft Azure"
    description="Het maken van een .NET MVC Web App dat oproepen web services met persoonlijke Microsoft-accounts en werk of schoolaccounts voor aanmelden."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Een web API bellen vanuit een .NET-web-app

Met het eindpunt v2.0, kunt u snel verificatie toevoegen aan uw WebApps en API's met webonderdelen met ondersteuning voor beide persoonlijk Microsoft-accounts en werk- of schoolaccount accounts.  Hier wordt we een MVC web-app die gebruikers in het gebruik van OpenID verbinding kunt maken, met wat hulp van Microsoft OWIN middleware ondertekent maken.  De web-app wordt OAuth 2.0 access tokens krijgen voor een web api beveiligd door OAuth 2.0 waarmee maken, lezen en verwijderen op een van de opgegeven gebruiker 'takenlijst'.

Deze zelfstudie wordt hoofdzakelijk over het gebruik van MSAL opvragen en access tokens gebruiken in een WebApp, die worden beschreven in volledige [hier](active-directory-v2-flows.md#web-apps)richten.  Als de vereisten, wilt u mogelijk eerst informatie over het [toevoegen van eenvoudige aanmelden bij een web-app](active-directory-v2-devquickstarts-dotnet-web.md) of hoe u [een web API optimaal is beveiligd](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Voorbeeld van een code downloaden

De code voor deze zelfstudie worden [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

U kunt ook [de voltooide app als een .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) of klonen van de voltooide app:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- Een **App geheim** van het type **wachtwoord** maken en kopiëren naar beneden de waarde voor later
- De **Web** -platform voor de app toevoegen.
- Voer het juiste **URI omleiden**. De omleidings-uri geeft aan dat Azure AD waar verificatie antwoorden moeten worden doorgestuurd: de standaardinstelling voor deze zelfstudie is `https://localhost:44326/`.


## <a name="install-owin"></a>OWIN installeren
De OWIN middleware NuGet pakketten naar toevoegen de `TodoList-WebApp` project met de Package Manager-Console.  De middleware OWIN worden gebruikt op aanmelden en afmelden verzoeken en informatie over de gebruiker, onder andere sessie van de gebruiker beheren.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Meld u aan de gebruiker
Nu configureren de middleware OWIN om de [verificatie-protocol OpenID verbinding](active-directory-v2-protocols.md#openid-connect-sign-in-flow)te gebruiken.  

-   Open de `web.config` bestand in de hoofdmap van de `TodoList-WebApp` project en voer waarden in de configuratie van uw app in de `<appSettings>` sectie.
    -   De `ida:ClientId` is de **Toepassings-Id** die is toegewezen aan uw app in de registratie-portal.
    - De `ida:ClientSecret` is de **App geheim** die u hebt gemaakt in de registratie-portal.
    -   De `ida:RedirectUri` is de **Uri omleiden** die u hebt ingevoerd in de portal.
- Open de `web.config` bestand in de hoofdmap van de `TodoList-Service` project en vervang het `ida:Audience` met dezelfde **Toepassings-Id** als hierboven.


- Open het bestand `App_Start\Startup.Auth.cs` en toevoegen `using` instructies voor de bibliotheken dan hierboven.
- Implementeren in hetzelfde bestand, de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIDConnectAuthenticationOptions` fungeert als coördinaten voor uw app kunt u communiceren met Azure AD.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>MSAL gebruiken om te krijgen van toegangstokens
In de `AuthorizationCodeReceived` melding die we wilt [OAuth 2.0 samen met OpenID Connect](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) aan inwisselen de authorization_code voor een toegangstoken met de Service van de lijst taak gebruiken.  MSAL kunt doen met dit proces eenvoudig voor:

- Installeer eerst de preview-versie van MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- En toevoegen van een andere `using` instructie naar de `App_Start\Startup.Auth.cs` bestand voor MSAL.
- Deze toegevoegd aan een nieuwe methode de `OnAuthorizationCodeReceived` gebeurtenis-handler.  Deze handler MSAL wordt gebruikt om een toegangstoken tot de API van de lijst taak en het token wordt opgeslagen in de MSAL token cache voor later:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- In web-apps heeft MSAL een extensible token cache die kan worden gebruikt voor de opslag van tokens.  In dit voorbeeld wordt geïmplementeerd de `NaiveSessionCache` HTTP-sessie opslag naar cache tokens gebruikt.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Het Web API bellen
Nu is het tijd dat is wel het gebruik van de access_token die u in stap 3 hebt aangeschaft.  Open de WebApp `Controllers\TodoListController.cs` bestand, waardoor u alle verzoeken van CRUD tot de API van de lijst taak.

- U kunt MSAL opnieuw hier access_tokens ophalen uit de cache MSAL.  Eerst toevoegen een `using` -instructie voor MSAL aan dit bestand.

    `using Microsoft.Identity.Client;`

- In de `Index` van de actie, gebruik MSAL `AcquireTokenSilentAsync` methode om een access_token die kunnen worden gebruikt voor het lezen van gegevens uit de takenlijst-service:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Vervolgens wordt het resulterende token aan de HTTP GET-aanvraag als de `Authorization` kop, die de takenlijst-service wordt gebruikt om te verifiëren het verzoek.
- Als de takenlijst-service retourneert een `401 Unauthorized` antwoord, de access_tokens in MSAL niet meer geldig om welke reden.  In dit geval moet u eventuele access_tokens neerzetten uit de cache MSAL en weergeven van de gebruiker een bericht dat ze moet u mogelijk aan te melden opnieuw, waarin de stroom token overname opnieuw wordt gestart.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Op dezelfde manier als MSAL niet mogelijk om terug te keren een access_token voor welke reden dan ook is, moet u moet de gebruiker opnieuw aanmelden.  Dit is net zo eenvoudig als het een vangen `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- De exacte dezelfde `AcquireTokenSilentAsync` oproep is implementd in de `Create` en `Delete` acties.  In web-apps, kunt u deze methode MSAL om access_tokens wanneer u deze nodig in uw app hebt.  MSAL wordt kunt verrichten bij het verkrijgen van, caching en tokens vernieuwen voor u.

Tot slot maken en uitvoeren van uw app!  Meld u aan met een Microsoft-Account of een Azure AD-Account en ziet u hoe de identiteit van de gebruiker is doorgevoerd in de bovenste navigatiebalk.  Toevoegen en sommige items verwijderen uit de takenlijst van de gebruiker dat het OAuth-2.0 beveiligd API oproepen in actie zien.  U hebt nu een WebApp & web API, beide beveiligd met industriële standaard protocollen, die gebruikers met hun persoonlijke en werk-, school accounts kunnen verifiëren.

Voor verwijzing, in de voltooide steekproef (zonder de configuratiewaarden) [hier is opgegeven](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer bronnen:
- [De handleiding voor ontwikkelaars van v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure active directory" tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
