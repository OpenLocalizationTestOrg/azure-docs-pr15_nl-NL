<properties
    pageTitle="Azure AD .NET aan de slag | Microsoft Azure"
    description="Het maken van een Web-API voor .NET MVC geïntegreerd met Azure AD voor verificatie en machtiging."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Een Web-API gebruiken vruchtdragende tokens afgeleid van Azure AD beveiligen

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een toepassing die toegang tot beveiligde bronnen, u weten hoe moet u deze resources beveiligen tegen onrechtmatige toegang biedt maakt.
Azure AD kunt u eenvoudig en eenvoudig een web API OAuth vruchtdragende 2.0 Access Tokens gebruikt met slechts een paar regels met code beveiligen.

In Asp.NET-web-apps, kunt u dit doen met Microsoft implementatie van de OWIN community op basis van hoeveelheid werk middleware opgenomen in .NET Framework 4.5.  Hier gebruiken we OWIN om te maken van een 'Takenlijst' web API die:
-   Hiermee geeft aan welke API's zijn beveiligd.
-   Dat de Web API-oproepen een geldige toegangstoken bevatten is gevalideerd.

Hiervoor moet u:

1. Een toepassing met Azure AD registreren
2. Uw app instellen voor het gebruik van de pijplijn OWIN-verificatie.
3. Een clienttoepassing om te bellen van de naar doen lijst Web API configureren

Om gestart, [de app-skelet downloaden](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Elk is een oplossing Visual Studio-2013.  U moet ook een Azure AD-tenant waarin uw toepassing registreren.  Als u geen sjabloon hebt, [leren hoe u een](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure AD registreren*
Als u wilt beveiligen uw toepassing, moet u eerst maken van een toepassing in uw tenant en Azure AD voorzien van een aantal belangrijke beantwoorden.

-   Meld u aan bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven.  Voer "Lijstjes Service".
    -   De **Uri omleiden** is een combinatie van kleurenschema en de tekenreeks die Azure AD gebruiken zou om terug te keren eventuele tokens uw app aangevraagd. Voer `https://localhost:44321/` voor deze waarde.
-   Wanneer u de registratie hebt uitgevoerd, Ga naar tabblad **configureren** en zoek naar het veld **App-ID-URI** .  Voer in een tenant-specifieke-id voor deze waarde, bijvoorbeeld`https://contoso.onmicrosoft.com/TodoListService`
- Sla de configuratie.  Open de portal laten: u moet ook de clienttoepassing kort registreren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. uw app instellen voor het gebruik van de pijplijn OWIN-verificatie*

Als u dat hebt gedaan een toepassing met Azure AD, moet u uw toepassing met Azure AD communiceren om te valideren binnenkomende aanvragen & tokens instellen.

-   Als u wilt beginnen, opent u de oplossing en de OWIN middleware NuGet-pakketten toevoegen aan het TodoListService project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Een klasse OWIN opstarten toevoegen aan het TodoListService project genoemd `Startup.cs`.  Rechts-Klik op het project-- **toevoegen**> --> **Nieuw Item** --> zoeken naar 'OWIN'.  De middleware OWIN wordt aangeroepen de `Configuration(…)` methode wanneer de app wordt gestart.
-   Wijzigen van de declaratie class naar `public partial class Startup` -we hebben al deel uitmaken van deze klasse geïmplementeerd voor u in een ander bestand.  In de `Configuration(…)` methode, een oproep door naar ConfgureAuth(...) verificatie voor uw web-app instellen.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Open het bestand `App_Start\Startup.Auth.cs` en implementeren de `ConfigureAuth(…)` methode.  De parameters die u opgeeft in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fungeert als coördinaten voor uw app kunt u communiceren met Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Nu kunt u `[Authorize]` kenmerken naar het beveiligen van uw controllers en heeft gedaan met JWT vruchtdragende verificatie.  Verfraaien de `Controllers\TodoListController.cs` klasse met een tag autoriseren.  Zorgt u ervoor dat de gebruiker aan te melden voor de toegang tot die pagina.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wanneer een geautoriseerde beller succes roept een van de `TodoListController` API's, de actie mogelijk toegang tot de informatie over de beller nodig.  OWIN biedt toegang tot de claims binnen de dragertoken via de `ClaimsPrincpal` object.  
- Een algemene vereiste voor web API's is voor het valideren van de aanwezig zijn in het token "bereiken": dit zorgt ervoor dat de gebruiker heeft ingestemd met de benodigde machtigingen voor toegang tot de Service van de lijst doen:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Ten slotte, opent u de `web.config` bestand in de hoofdmap van het project TodoListService en voer uw configuratiewaarden in de `<appSettings>` sectie.
  - De `ida:Tenant` is de naam van uw Azure AD-tenant, bijvoorbeeld "contoso.onmicrosoft.com".
  - Uw `ida:Audience` is de App-ID-URI van de toepassing die u hebt ingevoerd in de Portal Azure.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. een clienttoepassing configureren en uitvoeren van de service*
Voordat u de lijst-Service van de taak in actie zien kunt, moet u de Client van de lijst taak configureren zodat deze tokens ophalen uit AAD gesprekken te voeren met de service.

- Ga terug naar de [Beheerportal van Azure](https://manage.windowsazure.com)
- Maak een nieuwe toepassing in uw Azure AD-tenant en selecteer **Systeemeigen clienttoepassing** in de resulterende vraag.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   Voer `http://TodoListClient/` voor de waarde **Uri omleiden** .
- Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke **Id van de Client**. U moet deze waarde in de volgende stappen, zodat deze kopiëren op het tabblad configureren.
- Ook in het tabblad **configureren** , zoek de sectie 'Machtigingen aan andere toepassingen'. Klik op 'Toepassing toevoegen'. Selecteer 'Alle Apps' in de vervolgkeuzelijst 'Weergeven' en klik op het bovenste selectievakje is ingeschakeld. Zoek- en klikt u op uw naar doen lijst Service en klik op het vinkje onder als de toepassing wilt toevoegen. Selecteer 'Access naar doen lijst Service' in de vervolgkeuzelijst 'De machtigingen gedelegeerde' en sla de configuratie.


- Open in Visual Studio, `App.config` in de TodoListClient project en voer uw configuratiewaarden in de `<appSettings>` sectie.
  - De `ida:Tenant` is de naam van uw Azure AD-tenant, bijvoorbeeld "contoso.onmicrosoft.com".
  - Uw `ida:ClientId` app-ID die u hebt gekopieerd in de Portal Azure.
  - Uw `todo:TodoListResourceId` is de App-ID-URI van de naar doen lijst-Service-toepassing die u hebt ingevoerd in de Portal Azure.

Tot slot schonen, maken en uitvoeren van elk project!  Als u nog niet is gedaan, nu is de tijd om te maken van een nieuwe gebruiker in uw tenant met een *. onmicrosoft.com-domein.  Meld u aan bij de takenlijst-client met die gebruiker en sommige taken toevoegen aan van de gebruiker takenlijst.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  U kunt nu gaat u naar meer extra identiteit-scenario's.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
