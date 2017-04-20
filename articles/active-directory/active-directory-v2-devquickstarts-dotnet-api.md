<properties
    pageTitle="Azure AD v2.0 .NET Web API | Microsoft Azure"
    description="Het maken van een .NET MVC Web Api die tokens afgeleid van beide persoonlijk Microsoft-Account en accounts voor werk- of schoolaccount accepteert."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Een MVC web API Secure

Met Azure Active Directory het eindpunt v2.0, u kunt beveiligen met een Web-API gebruiken [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) access tokens, zodat gebruikers met zowel persoonlijk Microsoft-account en werk of school-mailaccounts naar veilig toegang tot uw Web-API.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

In ASP.NET-webpagina API's, kunt u dit doen met Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.  We gebruiken hier OWIN maken van een 'Takenlijst' MVC Web API waarmee clients kunnen maken en lezen taken uit de takenlijst van een gebruiker.  Het web API worden gevalideerd dat verzoeken voor oproepen een geldige toegangstoken bevatten en verzoeken die niet zijn gevalideerd op een beveiligde route negeren.  In dit voorbeeld is gebouwd met Visual Studio-2015.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie worden [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

De skelet app bevat alle de standaardtekst-code voor een eenvoudige API, maar ontbreekt alle van de identiteit-gerelateerde onderdelen. Als u niet volgen wilt, kunt u in plaats daarvan kopiëren of [gedownload van de voltooide steekproef](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.

In dit visual studio-oplossing bevat ook een 'TodoListClient', dat wil een eenvoudige WPF-app zeggen.  De TodoListClient wordt gebruikt om te laten zien hoe een gebruiker positief of negatief in- en hoe een client aanvragen kan verlenen aan uw Web-API.  In dit geval worden de TodoListClient zowel de TodoListService aangegeven met de dezelfde app.  Als u wilt de TodoListClient configureren, moet u ook:

- De **Mobile** -platform voor de app toevoegen.


## <a name="install-owin"></a>OWIN installeren

Nu u een app hebt geregistreerd, moet u voor het instellen van uw app met het eindpunt v2.0 communiceren om te valideren binnenkomende aanvragen & tokens.

- Als u wilt beginnen, opent u de oplossing en de OWIN middleware NuGet-pakketten toevoegen aan het TodoListService project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth verificatie configureren

- Een klasse OWIN opstarten toevoegen aan het TodoListService project genoemd `Startup.cs`.  Rechts-Klik op het project-- **toevoegen**> --> **Nieuw Item** --> zoeken naar 'OWIN'.  De middleware OWIN wordt aangeroepen de `Configuration(…)` methode wanneer de app wordt gestart.
- Wijzigen van de declaratie class naar `public partial class Startup` -we hebben al deel uitmaken van deze klasse geïmplementeerd voor u in een ander bestand.  In de `Configuration(…)` methode, een oproep door naar ConfgureAuth(...) verificatie voor uw web-app instellen.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Open het bestand `App_Start\Startup.Auth.cs` en implementeren de `ConfigureAuth(…)` methode, die de Web-API wordt ingesteld op tokens afgeleid van het eindpunt v2.0 accepteren.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Nu kunt u `[Authorize]` kenmerken naar het beveiligen van uw controllers en heeft gedaan met OAuth 2.0 vruchtdragende verificatie.  Verfraaien de `Controllers\TodoListController.cs` klasse met een tag autoriseren.  Zorgt u ervoor dat de gebruiker aan te melden voor de toegang tot die pagina.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wanneer een geautoriseerde beller succes roept een van de `TodoListController` API's, de actie mogelijk toegang tot de informatie over de beller nodig.  OWIN biedt toegang tot de claims binnen de dragertoken via de `ClaimsPrincpal` object.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Ten slotte, opent u de `web.config` bestand in de hoofdmap van het project TodoListService en voer uw configuratiewaarden in de `<appSettings>` sectie.
  - Uw `ida:Audience` is de **Toepassings-Id** van de app die u hebt ingevoerd in de portal.

## <a name="configure-the-client-app"></a>De client-app configureren
Voordat u de lijst-Service van de taak in actie zien kunt, moet u de Client van de lijst taak configureren zodat deze tokens krijgen van het eindpunt v2.0 gesprekken te voeren met de service.

- Open in het project TodoListClient `App.config` en voer uw configuratiewaarden in de `<appSettings>` sectie.
  - Uw `ida:ClientId` toepassings-Id die u hebt gekopieerd in de portal.

Tot slot schonen, maken en uitvoeren van elk project!  U hebt nu een .NET MVC Web API die tokens afgeleid van beide persoonlijk Microsoft-accounts en werk- of schoolaccount accounts accepteert.  Meld u aan bij de TodoListClient en bellen van uw web api taken toevoegen aan de takenlijst van de gebruiker.

Voor een verwijzing naar kunt de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)of u klonen deze uit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Volgende stappen
U kunt nu verplaatsen naar aanvullende onderwerpen.  U kunt proberen:

[Een Web API bellen via een WebApp >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Raadpleeg voor meer bronnen:
- [De handleiding voor ontwikkelaars van v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure active directory" tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
