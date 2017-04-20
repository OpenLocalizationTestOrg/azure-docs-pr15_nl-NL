<properties
    pageTitle="Het werken met de .NET-endserver SDK voor Mobile-Apps | Azure App-Service"
    description="Informatie over het werken met de .NET-endserver SDK voor Azure App Service Mobile-Apps."
    keywords="App-service, azure app-service, mobiele app, mobiele service, schaal scalable, app implementatie, azure app-implementatie"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Werken met de .NET-endserver SDK voor Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

In dit onderwerp ziet u hoe u de .NET-endserver SDK in belangrijke scenario's van Azure App Service Mobile-Apps gebruiken. De SDK van Azure Mobile-Apps kunt u werken met mobiele clients van uw ASP.NET-toepassing.

>[AZURE.TIP] De [.NET server SDK voor Azure Mobile-Apps] [ 2] bron openen op GitHub is. De bibliotheek bevat alle broncode, inclusief de volledige server SDK eenheid test-suite en sommige projecten steekproef.

## <a name="reference-documentation"></a>Naslagmateriaal

De documentatie bij de server SDK zich bevindt: [Azure mobiele Apps .NET verwijzing][1].

## <a name="create-app"></a>Hoe u: een backend .NET Mobile-App maken

Als u een nieuw project start, kunt u een App-servicetoepassing via de [portal van Azure] - of Visual Studio maken. U kunt de App-servicetoepassing lokaal uitvoeren of het project publiceren naar uw cloudgebaseerde mobiele App Service-app.  

Als u mobiele mogelijkheden aan een bestaand project toevoegt, raadpleegt u de sectie [downloaden en de SDK geïnitialiseerd](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Een .NET-backend met behulp van de Azure portal maken

Als u wilt maken van een mobiele App Service-backend, hetzij volgt u de [Quickstart zelfstudie] [ 3] of als volgt te werk:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in het blad _aan de slag_ , klik onder **een tabel API maken**, kiest u **C#** als de **Backend-taal**. Klik op **downloaden**en openen van de oplossing in Visual Studio extraheren van de gecomprimeerde project-bestanden naar uw lokale computer.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Maken van een .NET-backend met Visual Studio 2013 en Visual Studio-2015

Installeren van de [Azure SDK voor .NET] [ 4] (versie 2.9.0 of hoger) maken van een project Azure Mobile-Apps in Visual Studio. Nadat u de SDK hebt geïnstalleerd, maakt u een ASP.NET-toepassing met de volgende stappen:

1. Open het dialoogvenster **Nieuw Project** (uit *bestand* > **Nieuw** > **Project...**).
2. **Sjablonen** > **Visual C#**en selecteer **Web**.
3. Selecteer **ASP.NET-webtoepassing**.
4. Voer de naam van het project. Klik vervolgens op **OK**.
5. Klik onder _ASP.NET 4.5.2 sjablonen_, selecteert u **Azure Mobile-App**. Controleer de **Host in de cloud** als u wilt maken van een mobiele backend in de cloud waaraan u dit project kunt publiceren.
6. Klik op **OK**.

## <a name="install-sdk"></a>Hoe u: downloaden en de SDK geïnitialiseerd

De SDK is beschikbaar op [NuGet.org]. Dit pakket bevat de basisfunctionaliteit vereist om aan de slag met de SDK. Als u wilt de SDK geïnitialiseerd, moet u acties uitvoeren op het object **HttpConfiguration** .

### <a name="install-the-sdk"></a>De SDK installeren

Als u wilt de SDK hebt geïnstalleerd, met de rechtermuisknop op het serverproject in Visual Studio, selecteer **NuGet-pakketten beheren**, zoekt u het pakket [Microsoft.Azure.Mobile.Server] en klikt u op **installeren**.

###<a name="server-project-setup"></a>Initialisatie van het serverproject

Een project .NET backend-server is vergelijkbaar met andere projecten ASP.NET geïnitialiseerd door een OWIN Opstartklasse op te nemen. Zorg ervoor dat u verwijst naar het pakket NuGet `Microsoft.Owin.Host.SystemWeb`. Als u wilt deze klasse kunt toevoegen in Visual Studio, met de rechtermuisknop op uw serverproject en selecteer **toevoegen** > 
**Nieuw Item**en klik vervolgens **Web** > **algemene** > **OWIN opstarten class**.  Een klasse is gegenereerd met het volgende kenmerk:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

In de `Configuration()` methode van uw OWIN Opstartklasse, een object **HttpConfiguration** gebruiken voor het configureren van de omgeving Azure Mobile-Apps.
Het volgende voorbeeld wordt de serverproject met geen extra functies geïnitialiseerd:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Afzonderlijke als functies wilt inschakelen, moet u extensie methoden aanroepen op het object **MobileAppConfiguration** voordat u **ApplyTo**. Bijvoorbeeld de standaardroutes in de volgende code wordt toegevoegd aan elke API afzonderlijk met het kenmerk `[MobileAppController]` tijdens initialisatie:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

De server quickstart van de Azure portal oproepen **UseDefaultConfiguration()**. Dit is gelijk aan de volgende instellingen:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

De extensie methoden gebruikt zijn:

* `AddMobileAppHomeController()`biedt de standaardstartpagina van Azure Mobile-Apps.
* `MapApiControllers()`aangepaste API mogelijkheden biedt voor WebAPI controllers versierd met de `[MobileAppController]` kenmerk.
* `AddTables()`biedt een toewijzing van de `/tables` eindpunten tabel controller.
* `AddTablesWithEntityFramework()`een korte handje voor de toewijzing is de `/tables` eindpunten met entiteit Framework op basis van controllers.
* `AddPushNotifications()`biedt een eenvoudige methode voor het registreren van apparaten voor melding Hubs van.
* `MapLegacyCrossDomainController()`biedt standaard CORS kopteksten voor plaatselijke ontwikkeling.

### <a name="sdk-extensions"></a>SDK extensies

De volgende extensie NuGet gebaseerde-pakketten bieden verschillende mobiele voorzieningen die kunnen worden gebruikt door uw toepassing. U inschakelen extensies tijdens initialisatie met behulp van het object **MobileAppConfiguration** .

- [Microsoft.Azure.Mobile.Server.Quickstart] ondersteunt de basisinstellingen Mobile-Apps. Toegevoegd aan de configuratie door het aanroepen van de methode van de extensie **UseDefaultConfiguration** tijdens initialisatie. Dit toestel bevat de volgende van extensies: meldingen, verificatie, entiteit, tabellen, tussen domeinen en start-pakketten. Dit pakket wordt gebruikt door de mobiele Apps Quickstart die beschikbaar zijn op de Azure-portal.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   implementeert standaard *deze mobiele app actief is pagina* voor de hoofdsite van de website. Toevoegen aan de configuratie door de methode van de extensie   **AddMobileAppHomeController** roepen.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   klassen voor het werken met gegevens bevat en sets-ups van de pijplijn gegevens. Toevoegen aan de configuratie door de methode van de extensie **AddTables** roepen.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   kunt u het kader entiteit aan access-gegevens in de SQL-Database. Toevoegen aan de configuratie door de methode van de extensie **AddTablesWithEntityFramework** roepen.

- [Microsoft.Azure.Mobile.Server.Authentication] schakelt verificatie en sets-ups van de OWIN middleware gebruikt om tokens te valideren. Toevoegen aan de configuratie door de ondersteuning voor de **AddAppServiceAuthentication**  
   en **IAppBuilder**. **UseAppServiceAuthentication** extensie methoden.

- [Microsoft.Azure.Mobile.Server.Notifications] kunt push-meldingen en een push registratie-eindpunt definieert. Toevoegen aan de configuratie door de methode van de extensie **AddPushNotifications** roepen.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   Hiermee maakt u een controller dat gegevens naar de oudere webbrowsers uit uw Mobile-App fungeert. Toevoegen aan de configuratie door de methode van de extensie   **MapLegacyCrossDomainController** roepen.

- [Microsoft.Azure.Mobile.Server.Login] biedt de methode AppServiceLoginHandler.CreateToken(), die een statische methode gebruikt tijdens scenario's voor aangepaste verificatie is.   

## <a name="publish-server-project"></a>Hoe: het serverproject publiceren

In dit gedeelte ziet u hoe u uw project .NET backend van Visual Studio publiceren. U kunt ook uw backend-project met behulp van cijfer of een van de andere methoden beschreven in de [documentatie over de implementatie van Azure App Service](../app-service-web/web-sites-deploy.md)implementeren.

1. Visual Studio, bouw die tabellen opnieuw het project als u wilt herstellen NuGet-pakketten.

2. Klik in Solution Explorer met de rechtermuisknop op het project, klik op **publiceren**. De eerste keer dat u publiceert, moet u een publicatie profiel definiëren. Wanneer u een profiel is gedefinieerd voor het al hebt, kunt u selecteert u deze en klik op **publiceren**.

2. Als u wordt gevraagd om te selecteren van een doel publiceren, klikt u op **Microsoft Azure App Service** > **volgende**, en vervolgens (indien nodig) aanmelden met uw Azure referenties. 
   Visual Studio veilig worden opgeslagen en downloads van uw publicatie-instellingen rechtstreeks vanuit Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Kiest u uw **abonnement**, selecteert u **Type Resource** vanuit de **weergave**, uitvouwen **Mobile-App**, en uw backend Mobile-App, klik op **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Controleer of de profielgegevens publiceren en klik op **publiceren**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Wanneer uw backend Mobile-App heeft gepubliceerd, ziet u een aantekening toevoegen die aangeeft dat u succes.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Hoe u: een tabel controller definiëren

Definieer de Controller van een tabel om een SQL-tabel met mobiele clients weer te geven.  Configureren van een tabel Controller, moet drie stappen:

1. Maak een klasse gegevens overbrengen Object (DTO).
2. Een tabelverwijzing configureren in de klas Mobile DbContext.
3. Maak een tabel controller.

Een gegevens overbrengen Object (DTO) is een gewone C#-object dat van overneemt `EntityData`.  Bijvoorbeeld:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

De DTO wordt gebruikt voor het definiëren van de tabel in de SQL-database.  U maakt de invoer van de database toevoegen een `DbSet<>` eigenschap waarmee de DbContext die u gebruikt.  In de project standaardsjabloon voor Azure Mobile-Apps, de DbContext heet `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Als u de SDK Azure is geïnstalleerd, kunt u een sjabloon tabel controller nu als volgt maken:

1. Met de rechtermuisknop op de map Controllers en selecteer **toevoegen** > **Controller...**.
2. Selecteer de optie **Azure mobiele Apps tabel Controller** , klik vervolgens op **toevoegen**.
3. Klik in het dialoogvenster **Controller toevoegen** :
    * Selecteer in de vervolgkeuzelijst **Model class** uw nieuwe DTO.
    * Selecteer in de vervolgkeuzelijst **DbContext** de klas Mobile Service DbContext.
    * De naam van de domeincontroller wordt voor u gemaakt.
4. Klik op **toevoegen**.

Het project van de server quickstart bevat een voorbeeld voor een eenvoudige **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Hoe u: de grootte van de tabel paginering aanpassen

Azure Mobile-Apps retourneert standaard 50 records per aanvraag.  Paginering zorgt ervoor dat de client wordt niet in beslag nemen hun UI-thread of op de server te lang, zorgen dat een goede gebruikerservaring. Als u wilt wijzigen van de grootte van de tabel paginering, de serverzijde "gemachtigd query grootte" vergroten en de pagina aan de clientzijde de serverzijde grootte "gemachtigd query grootte" is aangepast met de `EnableQuery` kenmerk:

    [EnableQuery(PageSize = 500)]

Controleer of de PageSize is dezelfde of groter is dan de aangevraagd door de klant.  Raadpleeg de specifieke procedure documentatie voor meer informatie over het wijzigen van het formaat van de client-client.

## <a name="how-to-define-a-custom-api-controller"></a>Hoe u: een aangepaste API-besturing definiëren

De aangepaste API-besturing beschikt over de basisfuncties naar uw backend Mobile-App door een eindpunt weergeeft. U kunt een mobiele / regiospecifieke API controller met het kenmerk [MobileAppController] registreren. De `MobileAppController` kenmerk registreert de route, de Mobile-Apps JSON serializer ingesteld en schakelt u over het [controleren van client-versie](app-service-mobile-client-and-server-versioning.md).

1. Visual Studio, met de rechtermuisknop op de map Controllers en klik op **toevoegen** > **Controller**, selecteer **Web API 2 Controller&mdash;lege** en klik op **toevoegen**.

2. Geef een **naam van de domeincontroller**, zoals `CustomController`, en klik op **toevoegen**.

3. Voeg het volgende in het nieuwe controller-klassebestand met instructie:

        using Microsoft.Azure.Mobile.Server.Config;

4. Het kenmerk **[MobileAppController]** toepassen op de definitie API controller class, zoals in het volgende voorbeeld:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. In App_Start/Startup.MobileApp.cs-bestand, een oproep toevoegen aan de methode **MapApiControllers** toestelnummer, zoals in het volgende voorbeeld:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

U kunt ook de `UseDefaultConfiguration()` extensie methode in plaats van `MapApiControllers()`. Een controller die geen **MobileAppControllerAttribute** toegepast nog steeds toegankelijk door clients, maar deze mogelijk niet worden correct worden gebruikt door klanten met behulp van een mobiele App-client SDK.

## <a name="how-to-work-with-authentication"></a>Hoe u: werken met verificatie

Azure Mobile-Apps worden gebruikt voor verificatie van de App-Service / autorisatie voor het beveiligen van uw mobiele backend.  In dit gedeelte ziet u hoe u de volgende verificatie taken uitvoeren in uw project .NET backend-server:

+ [Hoe u: verificatie toevoegen aan een serverproject](#add-auth)
+ [Hoe u: aangepaste verificatie gebruiken voor uw toepassing](#custom-auth)
+ [Hoe u: ophalen geverifieerd gebruikersgegevens](#user-info)
+ [Hoe u: beperken van toegang tot de gegevens voor geautoriseerde gebruikers](#authorize)

### <a name="add-auth"></a>Hoe u: verificatie toevoegen aan een serverproject

U kunt verificatie toevoegen aan uw serverproject door het object **MobileAppConfiguration** uitbreiden en OWIN middleware te configureren. Wanneer u het pakket [Microsoft.Azure.Mobile.Server.Quickstart] installeren en de **UseDefaultConfiguration** extensie aanroept, kunt u verder met stap 3.

1. Installeer het pakket [Microsoft.Azure.Mobile.Server.Authentication] in Visual Studio.

2. In het projectbestand Startup.cs toevoegen de volgende regel met code aan het begin van de **configuratie** -methode:

        app.UseAppServiceAuthentication(config);

    Dit onderdeel van de middleware OWIN tokens uitgegeven door de bijbehorende App Service gateway is gevalideerd.

3. Toevoegen de `[Authorize]` kenmerk aan elke controller of de methode die is verificatie vereist. 

Zie voor meer informatie over het verifiëren van uw Mobile-Apps backend-clients, [verificatie toevoegen aan uw app](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Hoe u: aangepaste verificatie gebruiken voor uw toepassing

Als u niet gebruiken op een van de App Service verificatie/autorisatie-providers wilt, kunt u uw eigen systeem login implementeren. Installeer het pakket [Microsoft.Azure.Mobile.Server.Login] om u te helpen met verificatie token genereren.  Geef uw eigen code voor het valideren van gebruikersreferenties. U kunt bijvoorbeeld controleren tegen gezouten en opgedeelde wachtwoorden in een database. In het onderstaande voorbeeld de `isValidAssertion()` methode (elders is gedefinieerd) is verantwoordelijk voor deze controles.

De aangepaste verificatie worden weergegeven door te maken van een ApiController en dat `register` en `login` acties. De client moet een aangepaste gebruikersinterface gebruiken voor het verzamelen van de gegevens van de gebruiker.  De gegevens wordt vervolgens naar de API met een standaard HTTP POST-oproep verstuurd. Wanneer de server is gevalideerd met de bevestiging, een token is verleend met de `AppServiceLoginHandler.CreateToken()` methode.  Het gebruik van ApiController **niet moet** de `[MobileAppController]` kenmerk. 

Een voorbeeld `login` actie:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Klik in het voorgaande voorbeeld zijn LoginResult en LoginResultUser serialiseerbaar objecten die zijn die vereiste eigenschappen weergeven. De client verwacht login antwoorden moeten worden geretourneerd als JSON-objecten van het formulier dat:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

De `AppServiceLoginHandler.CreateToken()` methode bevat een _publiek_ en een _uitgever_ -parameter. Beide parameters zijn ingesteld op de URL van de hoofdmap van de toepassing, met het schema HTTPS. U moet de _secretKey_ moeten de waarde van uw toepassing sleutel bevindt zich aanmelden op dezelfde manier instellen. Distribueer het bijbehorende sleutel in een client niet zoals deze kan worden gebruikt om toetsen u voorkomt en uitgeven voor gebruikers. Kunt u de bijbehorende sleutel terwijl ingesloten in een App Service door te verwijzen naar de _WEBSITE\_AUTH\_ONDERTEKENEN\_sleutel_ omgevingsvariabele. Volg de instructies in de [lokale foutopsporing met verificatie](#local-debug) sectie naar de sleutel ophalen en deze opslaan als een toepassingsinstelling indien nodig in een lokale foutopsporing context.

Het gepubliceerde token eventueel ook andere claims en een vervaldatum.  Het gepubliceerde token moet minimaal, het claimen van een onderwerp (**sub**) bevatten.

U kunt de standaard-client ondersteunt `loginAsync()` methode door overbelasting de route verificatie.  Als de client aanroept `client.loginAsync('custom');` als u zich hebt aangemeld, wilt u de route moet `/.auth/login/custom`.  U kunt instellen dat de route voor de aangepaste verificatie controller met `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Met de `loginAsync()` methode zorgt ervoor dat de verificatietoken aan elke volgende aanroep van de service is gekoppeld.

###<a name="user-info"></a>Hoe u: ophalen geverifieerd gebruikersgegevens

Wanneer een gebruiker is geverifieerd door de Service-App, kunt u de toegewezen gebruikers-ID en andere gegevens in uw .NET backend-code openen. Gegevens van de gebruiker kan worden gebruikt voor het maken van autorisatiebeslissingen in de backend. De volgende code wordt opgehaald van de gebruikers-ID die is gekoppeld aan een verzoek om:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

De beveiligings-id wordt afgeleid van de providerspecifieke gebruikers-ID en statische voor een bepaalde gebruiker en login-provider is.  De beveiligings-id is null voor ongeldige verificatietokens.

App Service kunt u ook specifieke claims aanvraagt bij uw provider login. Meer informatie toe aan de identiteitsprovider SDK kan worden verstrekt door elke identiteitsprovider.  U kunt bijvoorbeeld de Facebook-API Graph voor vrienden informatie.  Vorderingen die worden aangevraagd kunt u opgeven in het blad provider in de portal van Azure. Sommige claims vereisen extra configuratie heeft met de identiteitsprovider.

De volgende code wordt de methode **GetAppServiceIdentityAsync** extensie als u de aanmeldingsreferenties, waaronder het toegangstoken aanvragen ten opzichte van de Facebook-API Graph nodig:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Een met toevoegen-instructie voor `System.Security.Principal` op te geven van de methode van de extensie **GetAppServiceIdentityAsync** .

### <a name="authorize"></a>Hoe u: beperken van toegang tot de gegevens voor geautoriseerde gebruikers

In de vorige sectie bevat het ophalen van de gebruikers-ID van een geverifieerde gebruiker. U kunt de toegang tot gegevens en andere resources op basis van deze waarde beperken. Een gebruikers-id-kolom toevoegen aan tabellen en filteren van de queryresultaten door de gebruikers-ID is bijvoorbeeld een eenvoudige manier geretourneerde gegevens alleen op geautoriseerde gebruikers beperken. De volgende code retourneert gegevensrijen alleen wanneer de beveiligings-id overeenkomt met de waarde in de kolom gebruikersnaam in de tabel TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

De `Query()` methode geeft als resultaat een `IQueryable` die kan worden bewerkt door LINQ worden afgehandeld filteren.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Hoe u: push toevoegen meldingen aan een serverproject

Push-meldingen toevoegen aan uw serverproject door het object **MobileAppConfiguration** uitbreiden en het maken van een melding Hubs-client.

1. Visual Studio, met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**, zoekt `Microsoft.Azure.Mobile.Server.Notifications`, klik vervolgens op **installeren**. 

2. Herhaal deze stap voor het installeren de `Microsoft.Azure.NotificationHubs` pakket, waaronder de bibliotheek Hubs melding-client.

3. In App_Start/Startup.MobileApp.cs, en een oproep toevoegen aan de methode van de extensie **AddPushNotifications()** tijdens initialisatie:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Voeg de volgende code die Hiermee maakt u een melding Hubs-client:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Nu kunt u in de melding Hubs-client push-meldingen verzenden naar geregistreerde apparaten. Zie [push-meldingen toevoegen aan uw app](app-service-mobile-ios-get-started-push.md)voor meer informatie. Zie voor meer informatie over melding Hubs, [Melding Hubs overzicht](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Hoe u: inschakelen gericht push-labels gebruiken

Melding Hubs kunt u gerichte meldingen verzenden naar specifieke registraties met behulp van tags. Verschillende labels worden automatisch gemaakt:

* De installatie-ID kunt u een specifiek apparaat identificeren.
* De gebruikers-Id die is gebaseerd op de geverifieerde beveiligings-id, wordt een specifieke gebruiker.

De installatie-ID zijn toegankelijk vanaf de **installatie-id** -eigenschap op de **MobileServiceClient**.  Het volgende voorbeeld ziet hoe u een tag toevoegen aan een specifieke installatie in de melding Hubs met een installatie-ID:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Gewenste tags die is verstrekt door de client tijdens de registratie van push-meldingen worden genegeerd door de end bij het maken van de installatie. Als u wilt dat een client naar markeringen toevoegen aan de installatie, moet u een aangepaste API die worden toegevoegd met de voorgaande patroon tags maken. 

Zie [Client toegevoegd push melding tags] [ 5] in de steekproef van de voltooide quickstart App Service Mobile-Apps voor een voorbeeld.

##<a name="push-user"></a>Hoe u: push-meldingen verzenden naar een geverifieerde gebruiker

Als een geverifieerde gebruiker voor push-meldingen registreert, wordt automatisch een gebruikers-ID-tag toegevoegd aan de registratie. U kunt met behulp van dit label, push-meldingen verzenden naar alle apparaten die zijn geregistreerd die persoon. De volgende code wordt de beveiligings-id van de gebruiker die de aanvraag en stuurt een melding van de push sjabloon aan elke apparaatregistratie voor die persoon:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Wanneer u registreert voor push-meldingen uit een geverifieerde client, zorg dat de verificatie is voltooid voordat u probeert de registratie. Zie voor meer informatie [Push aan gebruikers] [ 6] in de steekproef van de voltooide quickstart App Service Mobile-Apps voor .NET backend.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Hoe u: fouten opsporen en oplossen van de Server .NET SDK

Azure App-Service biedt diverse foutopsporing en probleemoplossing technieken voor ASP.NET-toepassingen:

- [Een Azure-App-Service controleren](../app-service-web/web-sites-monitor.md)
- [Diagnostische gegevens vastleggen in Azure App-Service](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Problemen met een Azure App-Service in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Logboekregistratie

U kunt aan de diagnostische logboeken App Service schrijven met behulp van de standaard ASP.NET doelcellen schrijven. Voordat u naar de logboeken schrijft kunt, moet u diagnostische gegevens in de Mobile-App backend inschakelen.

Diagnostische hulpprogramma's inschakelen en naar de logboeken schrijven:

1. Volg de stappen in [het inschakelen van diagnostische gegevens](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Voeg de volgende instructie gebruiken in uw codebestand:

        using System.Web.Http.Tracing;

3. Maak een schrijver doelcellen te schrijven van de .NET-end de diagnostische logboeken, als volgt:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Uw serverproject publiceren en toegang tot de backend Mobile-App voor het uitvoeren van het codepad met de logboekregistratie.

5. Download en evalueren van de logboeken, zoals wordt beschreven in [hoe: Download logboeken](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokale foutopsporing met verificatie

U kunt uw toepassing lokaal om wijzigingen te testen voordat u ze naar de cloud publiceert uitvoeren. Druk op *F5* terwijl u in Visual Studio voor de meeste Azure Mobile-Apps backends. Er zijn echter enkele extra overwegingen wanneer u een verificatie gebruikt.

U moet een cloudgebaseerde mobiele app met App Service verificatie/autorisatie geconfigureerd en de klant moet hebben de cloud-eindpunt dat is opgegeven als het alternatieve aanmeldings-host. Zie de documentatie voor uw platform client voor de specifieke stappen vereist.

Zorg ervoor dat uw mobiele backend [Microsoft.Azure.Mobile.Server.Authentication] is geïnstalleerd. Klik in van de toepassing OWIN opstarten klas, voegt u de volgende opties, in na `MobileAppConfiguration` is toegepast op uw `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

In het voorgaande voorbeeld, moet u de instellingen van de toepassing _authAudience_ en _authIssuer_ configureren vanuit uw bestand Web.config op elk worden de URL van de hoofdmap van de toepassing, met het schema HTTPS. U moet de _authSigningKey_ moeten de waarde van uw toepassing sleutel bevindt zich aanmelden op dezelfde manier instellen. Als u de bijbehorende sleutel:

1. Navigeer naar de app in de [portal van Azure] 
2. Klik op **Extra**, **Kudu**, **gaat u**.
3. Klik in de site Kudu Management op **omgeving**.
4. De waarde voor _WEBSITE\_AUTH\_ONDERTEKENEN\_sleutel_. 

Gebruik de ondertekend toets voor de parameter _authSigningKey_ in uw lokale toepassing config.  Uw mobiele backend is nu uitgerust om tokens wanneer u zich lokaal, waarin de token vanuit het eindpunt cloudgebaseerde wordt opgehaald door de client te valideren.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

