<properties 
    pageTitle="Het maken van een Web-App met Cache bestand Vgx. | Microsoft Azure" 
    description="Informatie over het maken van een Web-App met Cache bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Het maken van een Web-App met Cache bestand Vgx.

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Deze zelfstudie wordt het maken en implementeren van een ASP.NET-webtoepassingen in een WebApp in Azure App-Service weergegeven met behulp van Visual Studio-2015. De voorbeeldtoepassing bevat een overzicht van team statistieken uit een database en ziet u verschillende manieren om te gebruiken van Azure bestand Vgx. Cache opslaan en gegevens ophalen uit de cache. U hebt een actieve web-app worden gelezen en geschreven met een database, geoptimaliseerd met Azure bestand Vgx. Cache en gehost in Azure wordt aangegeven wanneer u de zelfstudie hebt voltooid.

U leert:

-   Het maken van een webtoepassing ASP.NET MVC 5 in Visual Studio.
-   Hoe u toegang tot gegevens uit een database met behulp van entiteit Framework.
-   Hoe gegevensdoorvoer verbeteren en database laden beperken door te slaan en het ophalen van gegevens met behulp van Azure bestand Vgx. Cache.
-   Het gebruik van een bestand Vgx. gesorteerd instellen op de bovenste 5 teams ophalen.
-   Klik hier voor meer informatie over het inrichten van de Azure bronnen voor de toepassing met een sjabloon resourcemanager.
-   Hoe u de toepassing Azure gebruik van Visual Studio publiceert.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt de zelfstudie hebt voltooid, moet u de volgende vereisten hebben.

-   [Azure-account](#azure-account)
-   [Visual Studio-2015 met de Azure SDK voor .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-account

U moet een Azure-account om te voltooien van de zelfstudie. U kunt:

* [Gratis een Azure-account opent](/pricing/free-trial/?WT.mc_id=redis_cache_hero). U krijgt tegoeden die kunnen worden gebruikt om te proberen betaalde Azure Services. Zelfs nadat de tegoeden zijn gebruikt, kunt u deze kunt houden van het account en gebruiken van gratis Azure services en functies.
* [Voordelen van de abonnee Visual Studio activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Uw MSDN-abonnement kunt u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio-2015 met de Azure SDK voor .NET

De zelfstudie is geschreven voor Visual Studio-2015 met de [Azure SDK voor .NET](../dotnet-sdk.md) punt 2.8.2 of hoger. [De meest recente Azure SDK voor Visual Studio-2015 hier downloaden](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio wordt automatisch geïnstalleerd met de SDK als u nog geen hebt.

Als u Visual Studio-2013 hebt, kunt u [de meest recente Azure SDK voor Visual Studio-2013 downloaden](http://go.microsoft.com/fwlink/?LinkID=324322). Sommige schermen kunnen er anders uit in de afbeeldingen die in deze zelfstudie wordt weergegeven.

>[AZURE.NOTE] Afhankelijk van hoeveel van de afhankelijkheden SDK al op uw computer, kan het installeren van de SDK lang duren, enkele minuten aan een half uur of meer.

## <a name="create-the-visual-studio-project"></a>De Visual Studio-project maken

1. Open Visual Studio en klik op **bestand**, **Nieuw** **Project**.

2. Vouw het knooppunt **Visual C#** in de lijst met **sjablonen** , **Cloud**selecteren en op **ASP.NET-webtoepassing**. Zorg ervoor dat **.NET Framework 4.5.2** is geselecteerd.  Typ **ContosoTeamStats** in **het tekstvak** en klik op **OK**.
 
    ![Project maken][cache-create-project]

3. Selecteer **MVC** als het projecttype. Schakel het selectievakje **Host in de cloud** . U gaat [inrichten van de Azure bronnen](#provision-the-azure-resources) en [Publiceer de toepassing naar Azure](#publish-the-application-to-azure) in opeenvolgende stappen in deze zelfstudie. Zie [aan de slag met Web Apps in Azure App-Service, ASP.NET en Visual Studio gebruiken](../app-service-web/web-sites-dotnet-get-started.md)voor een voorbeeld van de inrichting van een App Service-web-app in Visual Studio **Host in de cloud** ingeschakeld laat.

    ![Project-sjabloon selecteren][cache-select-template]

4. Klik op **OK** om het project te maken.

## <a name="create-the-aspnet-mvc-application"></a>De toepassing ASP.NET MVC maken

In dit gedeelte van de zelfstudie maakt u de eenvoudige toepassing die worden gelezen en team statistieken uit een database weergegeven.

-   [Het model toevoegen](#add-the-model)
-   [De controller toevoegen](#add-the-controller)
-   [De weergaven configureren](#configure-the-views)

### <a name="add-the-model"></a>Het model toevoegen

1. Met de rechtermuisknop op **modellen** in **Solution Explorer**en klikt u op **toevoegen**, **Class**. 

    ![Model toevoegen][cache-model-add-class]

2. Voer `Team` voor de klas Geef een naam en klik op **toevoegen**.

    ![Model klasse toevoegen][cache-model-add-class-dialog]

3. Vervangen de `using` instructies boven aan de `Team.cs` bestand met de volgende instructies.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Vervang de definitie van de `Team` klasse met het volgende codefragment met een bijgewerkte `Team` klasse definitie, evenals enkele andere entiteit Framework helper klassen. Zie voor meer informatie over de eerste methode code aan entiteit-Framework die wordt gebruikt in deze zelfstudie, [Code eerst naar een nieuwe database](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Dubbelklik in **Solution Explorer**op **web.config** om deze te openen.

    ![Web.config][cache-web-config]

3.  Toevoegen van de volgende verbindingsreeks naar de `connectionStrings` sectie. De naam van de verbindingsreeks moet overeenkomen met de naam van de entiteit Framework database context klas, dat wil zeggen `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Nadat u hebt toegevoegd, de `connectionStrings` sectie ziet er nu het volgende voorbeeld.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>De controller toevoegen

1. Druk op **F6** om te maken van het project. 
2. Met de rechtermuisknop op de map **Controllers** en klikt u op **toevoegen**, **Controller**in **Solution Explorer**.

    ![Controller toevoegen][cache-add-controller]

3. Kies **MVC 5 Controller met weergaven, met entiteit Framework**en klikt u op **toevoegen**. Als u een fout wordt weergegeven nadat u hebt geklikt **toevoegen**, moet u ervoor zorgen dat u eerst het project hebt gemaakt.

    ![Controller klasse toevoegen][cache-add-controller-class]

5. Selecteer **Team (ContosoTeamStats.Models)** in de vervolgkeuzelijst **Model class** . Selecteer **TeamContext (ContosoTeamStats.Models)** in de vervolgkeuzelijst **gegevens context class** . Type `TeamsController` in het tekstvak **Controller** (als deze niet automatisch wordt gevuld). Klik op **toevoegen** om te maken van de controller-klasse en de standaardweergaven toevoegen.

    ![Controller configureren][cache-configure-controller]

4. In **Solution Explorer** **Global.asax** uitvouwen en dubbelklik op **Global.asax.cs** om deze te openen.

    ![Global.asax.cs][cache-global-asax]

5. De volgende twee toevoegen met instructies boven aan het bestand onder de andere instructies.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Voeg de volgende regel met code aan het einde van de `Application_Start` methode.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Vouw in **Solution Explorer** `App_Start` en dubbelklik op `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Vervang `controller = "Home"` in de volgende code in de `RegisterRoutes` methode met `controller = "Teams"` zoals wordt weergegeven in het volgende voorbeeld.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>De weergaven configureren

1. Vouw de map **weergaven** en klik vervolgens op de map **gedeeld** in **Solution Explorer**, en dubbelklik op **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Wijzigen van de inhoud van de `title` element en vervangen `My ASP.NET Application` met `Contoso Team Stats` zoals wordt weergegeven in het volgende voorbeeld.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. In de `body` sectie, het bijwerken van de eerste `Html.ActionLink` instructie en vervangen `Application name` met `Contoso Team Stats` en vervangen `Home` met `Teams`.
    -   Voor:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Na:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Codewijzigingen][cache-layout-cshtml-code]

4. Druk op **Ctrl + F5** om te maken en uitvoeren van de toepassing. In deze versie van de toepassing leest de resultaten rechtstreeks vanuit de database. Houd rekening met de acties op **Nieuwe maken**, **bewerken**, **Details**en **verwijderen** die automatisch worden toegevoegd aan de toepassing door de scaffold **MVC 5 Controller met weergaven, met entiteit Framework** . U voegt in het volgende gedeelte van de zelfstudie Cache bestand Vgx. Als u wilt optimaliseren van de toegang tot gegevens en bieden extra voorzieningen voor de toepassing.

![Starter-toepassing][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>De toepassing configureren voor gebruik van de Cache bestand Vgx.

In dit gedeelte van de zelfstudie hebt u de voorbeeldtoepassing om te slaan en Contoso team statistieken ophalen uit een exemplaar van de Azure bestand Vgx. Cache met behulp van de client van de cache [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) configureren.

-   [De toepassing configureren voor gebruik van StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [De klasse TeamsController zodat resultaten worden geretourneerd uit de cache of de database bijwerken](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [De methoden maken, bewerken en verwijderen om te werken met de cache bijwerken](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [De weergave Teams Index voor gebruik met de cache bijwerken](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>De toepassing configureren voor gebruik van StackExchange.Redis

1. Als u wilt configureren een clienttoepassing in Visual Studio met het pakket StackExchange.Redis NuGet, met de rechtermuisknop op het project in **Solution Explorer** en kies **NuGet-pakketten beheren**. 

    ![NuGet-pakketten beheren][redis-cache-manage-nuget-menu]

2. Typ **StackExchange.Redis** in het vak tekst zoeken, selecteer de gewenste versie van de resultaten en klik op **installeren**.

    ![StackExchange.Redis NuGet pakket][redis-cache-stack-exchange-nuget]

    Het pakket NuGet-downloads en wordt de vereiste constructie verwijzingen voor de clienttoepassing voor toegang tot Azure bestand Vgx. Cache met de client van de cache StackExchange.Redis toegevoegd. Als u liever een versie sterke naam van de bibliotheek van de client **StackExchange.Redis** wilt gebruiken, kiest u **StackExchange.Redis.StrongName**; Kies anders **StackExchange.Redis**.

3. In **Oplossingverkenner**de map **Controllers** uitvouwen en dubbelklik op **TeamsController.cs** om deze te openen.

    ![Teams controller][cache-teamscontroller]

4. De volgende twee toevoegen met instructies naar **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Toevoegen van de volgende twee eigenschappen naar de `TeamsController` class.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Maak een bestand op uw computer met de naam `WebAppPlusCacheAppSecrets.config` en plaats het op een locatie die won't worden gecontroleerd met de broncode van de steekproef-toepassing, moet u besluit om dit te controleren in ergens. In dit voorbeeld de `AppSettingsSecrets.config` bestand zich bevindt op `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Bewerken de `WebAppPlusCacheAppSecrets.config` -bestand en de volgende inhoud toevoegen. Als u de toepassing lokaal uitvoert wordt deze informatie wordt gebruikt verbinding maken met uw exemplaar van de Azure bestand Vgx. Cache. Verderop in deze zelfstudie u inrichten van een exemplaar van de Azure bestand Vgx. Cache en de Cachenaam en het wachtwoord bijwerken. Als u niet wilt uitvoeren van de toepassing van de steekproef lokaal kunt u het maken van het bestand en de volgende stappen die verwijzen naar het bestand, overslaan omdat wanneer u Azure implementeert de toepassing de verbindingsgegevens cache opgehaald van de app-instelling voor de Web-App en niet uit dit bestand. Aangezien de `WebAppPlusCacheAppSecrets.config` niet wordt geïmplementeerd naar Azure met uw toepassing, hoeft u deze tenzij u gaat uitvoeren van de toepassing lokaal.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Dubbelklik in **Solution Explorer**op **web.config** om deze te openen.

    ![Web.config][cache-web-config]

3. Voeg de volgende `file` kenmerk naar de `appSettings` element. Als u een andere naam of locatie gebruikt, vervangt u door deze waarden voor de kleuren die u in het voorbeeld.
    -   Voor:`<appSettings>`
    -   Na:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    De ASP.NET-runtime samengevoegd de inhoud van het externe bestand met de opmaak van de `<appSettings>` element. Als het opgegeven bestand kan niet worden gevonden, worden in de runtime het kenmerk genegeerd. Geheimen voor uzelf (de verbindingsreeks aan uw cache) zijn niet opgenomen als onderdeel van de broncode voor de toepassing. Wanneer u uw web-app dashboard naar Azure implementeren, de `WebAppPlusCacheAppSecrests.config` bestand won't worden geïmplementeerd (dit is wat u wilt). Er zijn verschillende manieren om op te geven van deze geheimen in Azure wordt aangegeven, en in deze zelfstudie deze zijn geconfigureerd automatisch voor u wanneer u [het inrichten van de Azure resources](#provision-the-azure-resources) in een latere zelfstudie stap. Zie [Aanbevolen procedures voor het implementeren wachtwoorden en andere gevoelige gegevens ASP.NET en Azure App-Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)voor meer informatie over het werken met geheimen in Azure wordt aangegeven.


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>De klasse TeamsController zodat resultaten worden geretourneerd uit de cache of de database bijwerken

In dit voorbeeld wordt de team statistieken kunnen worden opgehaald uit de database of uit de cache. Team statistieken worden opgeslagen in de cache als een serienummer `List<Team>`, en ook als een gesorteerde set bestand Vgx. gegevenstype. Wanneer u items ophaalt uit een set gesorteerd, kunt u sommige, alle ophalen of query voor bepaalde items. In dit voorbeeld kunt u de gesorteerd instellen voor de bovenste 5 teams rangschikking door het aantal wins zoeken.

>[AZURE.NOTE] Dit is niet verplicht voor het opslaan van de team-statistische gegevens in meerdere indelingen in de cache pas Azure bestand Vgx. Cache gebruiken. Deze zelfstudie gebruikt verschillende indelingen voor ziet u enkele van de verschillende manieren en verschillende gegevenstypen die u cachegegevens kunt.



1. Voeg de volgende instructies naar de `TeamsController.cs` bestand aan de bovenkant met de andere met instructies.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. De huidige vervangen `public ActionResult Index()` methode met de volgende implementatie.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. De volgende drie manieren toevoegen de `TeamsController` class willen implementeren de `playGames`, `clearCache`, en `rebuildDB` actietypen van de schakeloptie-instructie in het vorige codefragment hebt toegevoegd.

    De `PlayGames` methode de team-statistieken worden bijgewerkt door een season van spellen simuleren, slaat u de resultaten in de database en Hiermee wist u het nu verouderde gegevens uit de cache.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    De `RebuildDB` methode wordt de database met de standaardset met teams opnieuw geïnitialiseerd, statistieken voor hen genereert en Hiermee wist u het nu verouderde gegevens uit de cache.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    De `ClearCachedTeams` methode wordt in de cache team statistieken verwijderd uit de cache.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. De volgende vier manieren toevoegen de `TeamsController` class willen implementeren van de verschillende manieren voor het ophalen van de team-statistieken uit de cache en de database. Elk van de volgende manieren retourneert een `List<Team>` die wordt weergegeven in de weergave.

    De `GetFromDB` methode wordt de team-statistieken gelezen uit de database.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    De `GetFromList` methode wordt de team-statistieken gelezen uit de cache als een serienummer `List<Team>`. Als er een gemiste cache, zijn de team-statistieken lezen uit de database en klik vervolgens in de cache zijn opgeslagen voor toekomstig gebruik. In dit voorbeeld gebruiken we JSON.NET serialisatie serialiseren de .NET-objecten en naar de cache. Zie voor meer informatie [over het werken met .NET-objecten in de Cache van Azure bestand Vgx.](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    De `GetFromSortedSet` methode wordt de team-statistieken gelezen uit een set met in de cache gesorteerd. Als er een gemiste cache, zijn de team-statistieken lezen uit de database en die zijn opgeslagen in de cache als een set gesorteerd.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    De `GetFromSortedSetTop5` methode wordt de top 5 teams gelezen uit de cache gesorteerd set. Begint door te schakelen van de cache voor de aanwezigheid van de `teamsSortedSet` sleutel. Als deze toets niet aanwezig zijn, is de `GetFromSortedSet` methode aangeroepen om de statistieken team lezen en ze opslaan in de cache. Naast de in de cache gesorteerd set is een query wordt uitgevoerd voor de bovenste 5 teams die worden geretourneerd.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>De methoden maken, bewerken en verwijderen om te werken met de cache bijwerken

De steigers-code die is gegenereerd als onderdeel van dit voorbeeld bevat methoden voor het toevoegen, bewerken en verwijderen van teams. Als een team is toegevoegd, bewerkt of verwijderd, wordt de gegevens in de cache verouderde. In deze sectie hebt u deze drie methoden als u wilt de teams in de cache wissen zodat de cache niet gesynchroniseerd met de database te wijzigen.

1. Blader naar de `Create(Team team)` methode in de `TeamsController` class. Een oproep toevoegen aan de `ClearCachedTeams` methode, zoals wordt weergegeven in het volgende voorbeeld.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Blader naar de `Edit(Team team)` methode in de `TeamsController` class. Een oproep toevoegen aan de `ClearCachedTeams` methode, zoals wordt weergegeven in het volgende voorbeeld.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Blader naar de `DeleteConfirmed(int id)` methode in de `TeamsController` class. Een oproep toevoegen aan de `ClearCachedTeams` methode, zoals wordt weergegeven in het volgende voorbeeld.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>De weergave Teams Index voor gebruik met de cache bijwerken

1. In **Solution Explorer**, vouwt u de map **weergaven** , klikt u vervolgens de map met het **Teams** en dubbelklik op **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Klik boven aan het bestand, zoekt u het volgende alinea-element.

    ![Actietabel][cache-teams-index-table]

    Dit is de koppeling naar een nieuw team maken. Vervang de alinea-element door de volgende tabel. In deze tabel bevat Actiekoppelingen voor het maken van een nieuw team, een nieuwe season van spellen wordt afgespeeld, de cache wissen, de teams ophalen uit de cache in verschillende indelingen, de teams ophalen uit de database en de database met vers voorbeeldgegevens opnieuw.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Schuif naar de onderkant van het bestand **Index.cshtml** en voeg de volgende `tr` element zodat deze de laatste rij in de laatste tabel in het bestand.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Deze rij geeft de waarde van `ViewBag.Msg` daarin een statusrapport over de huidige bewerking die wordt ingesteld wanneer u op een van de Actiekoppelingen uit de vorige stap.   

    ![Statusbericht][cache-status-message]

4. Druk op **F6** om te maken van het project.

## <a name="provision-the-azure-resources"></a>Inrichten van de Azure resources

Als host voor uw toepassing in Azure wordt aangegeven, moet u eerst de Azure services die moeten worden uw toepassing inrichten. De toepassing van de steekproef in deze zelfstudie gebruikt de volgende Azure services.

-   Cache Azure bestand Vgx.
-   App-Service WebApp
-   SQL-Database

Als u wilt deze services implementeren naar een nieuw of bestaand resourcegroep van uw keuze, klikt u op de volgende **distribueren naar Azure** -knop.

[! [Implementeren naar Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

De sjabloon van de [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) [maken een Web-App plus bestand Vgx. Cache en SQL-Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) deze knop **distribueren naar Azure** gebruikt om inrichten van deze services en de verbindingsreeks voor de SQL-Database en de toepassingsinstelling voor de verbindingsreeks van Azure bestand Vgx. Cache te zetten.

>[AZURE.NOTE] Als u geen een Azure-account hebt, kunt u [een gratis Azure-account maken](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in een paar minuten.

Te klikken op de knop **distribueren naar Azure** , gaat u naar de Azure-portal en start het proces van het maken van de resources die worden beschreven in de sjabloon.

![Implementeren naar Azure][cache-deploy-to-azure-step-1]

1. Klik op het blad **aangepaste implementatie** , selecteer het Azure abonnement gebruikt, en selecteer een bestaande resourcegroep of een nieuw account te maken en geef de locatie van de groep resource.
2. Geef op het blad **Parameters** , de naam van een administrator-account (**ADMINISTRATORLOGIN** - **beheerder**niet gebruiken), login beheerderswachtwoord (**ADMINISTRATORLOGINPASSWORD**) en naam van de database (**DATABASENAAM**). De andere parameters worden geconfigureerd voor een gratis App-Service plannen en lagere kosten-opties voor de SQL-Database en de Cache van Azure bestand Vgx., die niet met een gratis laag komen hostingprovider.
3. Een van de andere instellingen desgewenst wijzigen of de standaardwaarden behouden en klik op **OK**.


![Implementeren naar Azure][cache-deploy-to-azure-step-2]

1. Klik op **de juridische voorwaarden controleren**.
2. Lees de gebruiksrechtovereenkomst op het blad **aanschaffen** en klik op **aanschaffen**.
3. Klik op **maken** op het blad **aangepaste implementatie** om te beginnen met de inrichting van de resources.

De voortgang van uw implementatie bekijken, klikt u op het meldingspictogram en klik op **de implementatie is gestart**.

![Implementatie gestart][cache-deployment-started]

U kunt de status van uw implementatie bekijken op het blad **Microsoft.Template** .

![Implementeren naar Azure][cache-deploy-to-azure-step-3]

Wanneer de inrichting van is voltooid, kunt u de toepassing naar Azure Visual Studio publiceren.

>[AZURE.NOTE] Eventuele fouten die tijdens het inrichten optreden kunnen worden weergegeven op het blad **Microsoft.Template** . Veelvoorkomende fouten zijn te veel SQL-Servers of te veel gratis App-Service plannen per abonnement hostingprovider. Oplossen van fouten en start het proces door te klikken op **Implementeer deze opnieuw** op het blad **Microsoft.Template** of de knop **distribueren naar Azure** in deze zelfstudie opnieuw.

## <a name="publish-the-application-to-azure"></a>De toepassing Azure publiceren

In deze stap van de zelfstudie, u de toepassing Azure publiceren en in de cloud worden uitgevoerd.

1. Met de rechtermuisknop op het project **ContosoTeamStats** in Visual Studio en kies **publiceren**.

    ![Publiceren][cache-publish-app]

2. Klik op **Microsoft Azure App-Service**.

    ![Publiceren][cache-publish-to-app-service]

3. Selecteer het abonnement dat wordt gebruikt bij het maken van de Azure bronnen, uitvouwen van het resourceveld groep met de resources, selecteer de gewenste Web-App en klik op **OK**. Als u de knop **distribueren naar Azure** gebruikt is de naam van uw Web-App wordt gestart met **webSite** , gevolgd door enkele extra tekens.

    ![Selecteer WebApp][cache-select-web-app]

4. Klik op **Verbinding valideren** om uw instellingen te controleren en klik vervolgens op **publiceren**.

    ![Publiceren][cache-publish]

    Na een paar seconden het publicatieproces wordt voltooid en een browser wordt gestart met de actieve voorbeeld van de toepassing. Als u een DNS-fout bij valideren of publiceren en het inrichten van de Azure resources voor de toepassing onlangs is voltooid, wacht even en probeer het opnieuw.

    ![Cache toegevoegd][cache-added-to-application]

De volgende tabel wordt elke actiekoppeling uit de voorbeeldtoepassing.

| Actie                  | Beschrijving                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nieuwe maken              | Maak een nieuw Team.                                                                                                                                               |
| Season afspelen             | Afspelen van een season van spellen, de team-stat bijwerken en schakelt u een verouderde gegevens uit de cache-teamsites.                                                                          |
| Cache wissen             | Het team statistieken uit de cache wissen.                                                                                                                             |
| Lijst met Cache         | De team-stat van de cache ophalen. Als er een gemiste cache, de Stat laden uit de database en aan de cache voor toekomstig gebruik opslaan.                                        |
| Gesorteerde Set uit Cache   | De team-stat van de cache met behulp van een gesorteerde set ophalen. Als er een gemiste cache, de Stat laden uit de database en opslaan in de cache met behulp van een gesorteerde set.  |
| Top 5 Teams uit Cache  | De bovenste 5 teams van de cache met behulp van een gesorteerde set ophalen. Als er een gemiste cache, de Stat laden uit de database en opslaan in de cache met behulp van een gesorteerde set. |
| Laden vanuit de database            | De team-stat ophalen uit de database.                                                                                                                       |
| Bouw die tabellen opnieuw DB              | De database opnieuw opbouwen en opnieuw vullen met voorbeeldgegevens team.                                                                                                        |
| Bewerken / Details / verwijderen | Bewerken van een team, details weergeven voor een team, een team verwijderen.                                                                                                             |


Klik op enkele van de acties te experimenteren met de gegevens ophaalt uit verschillende bronnen. Niet de verschillen in de tijd die nodig is voor het voltooien van de verschillende manieren voor het ophalen van de gegevens uit de database en de cache.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>De bronnen verwijderen wanneer u klaar met de toepassing bent

Als u klaar met het voorbeeld Zelfstudievideo-toepassing bent, kunt u de Azure resources die worden gebruikt om te kunnen besparen kosten en resources verwijderen. Als u de knop **distribueren naar Azure** in de sectie [inrichten van de Azure bronnen gebruikt](#provision-the-azure-resources) en alle resources zijn opgenomen in dezelfde resourcegroep, kunt u verwijderen ze samen in één bewerking door te verwijderen van het resourceveld groep.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) en klikt u op **resourcegroepen**.
2. Typ de naam van uw resourcegroep in het tekstvak **... items filteren** .
3. Klik op **...** aan de rechterkant van het resourceveld groep.
4. Klik op **verwijderen**.

    ![Verwijderen][cache-delete-resource-group]

5. Typ de naam van het resourceveld groep en klik op **verwijderen**.

    ![Verwijderen bevestigen][cache-delete-confirm]

Na een paar seconden worden de resourcegroep en alle bijbehorende container bronnen verwijderd.

>[AZURE.IMPORTANT] Houd er rekening mee dat het verwijderen van een resourcegroep onomkeerbaar is en dat de resourcegroep en alle resources in deze permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de bronnen voor het hosten van dit voorbeeld in een bestaande resourcegroep gemaakt, kunt u elke resource afzonderlijk verwijderen uit de desbetreffende bladen.

## <a name="run-the-sample-application-on-your-local-machine"></a>De toepassing van de steekproef worden uitgevoerd op uw lokale computer

Als u wilt uitvoeren van de toepassing lokaal op uw computer, moet u een exemplaar van de Azure bestand Vgx. Cache waarin uw gegevens in cache. 

-   Als u kunt uw toepassing Azure hebt gepubliceerd, zoals wordt beschreven in de vorige sectie, kunt u het bestand Vgx. Cache van Azure-exemplaar die tijdens deze stap is ingericht.
-   Als u een ander exemplaar van bestaande Azure bestand Vgx. Cache hebt, kunt u die in dit voorbeeld lokaal uitvoeren.
-   Als u een exemplaar van de Azure bestand Vgx. Cache maken moet, kunt u de stappen in [een cache maken](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Zodra u hebt geselecteerd of de cache voor gebruik wordt gemaakt, blader naar de cache in de portal van Azure en de [hostnaam](cache-configure.md#properties) en de [toegangstoetsen](cache-configure.md#access-keys) voor de cache ophalen. Zie de [cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings)voor instructies.

1. Open de `WebAppPlusCacheAppSecrets.config` bestand dat u tijdens de stap [configureren de toepassing Cache bestand Vgx. Gebruik](#configure-the-application-to-use-redis-cache) van deze zelfstudie met de editor van uw keuze hebt gemaakt.

2. Bewerken de `value` kenmerk en vervangen `MyCache.redis.cache.windows.net` door de [hostnaam](cache-configure.md#properties) van uw cache, en geeft u op de [toets primaire of secundaire](cache-configure.md#access-keys) van de cache als het wachtwoord.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Druk op **Ctrl + F5** om de toepassing te starten.

>[AZURE.NOTE] Houd er rekening mee dat omdat de toepassing, inclusief de database, lokaal wordt uitgevoerd en de Cache bestand Vgx. wordt gehost in Azure wordt aangegeven, de cache kan worden weergegeven om uit te voeren onder de database. Voor de beste prestaties het clienttoepassing en Azure bestand Vgx. Cache exemplaar moeten op dezelfde locatie. 

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over het [Aan de slag met ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) op de [ASP.NET](http://asp.net/) -site.
-   Zie voor meer voorbeelden van het maken van een ASP.NET-Web-App in de App Service [maken en implementeren van een ASP.NET-web-app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) uit de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinding maken met [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Zie voor meer QuickStart uit de demo HealthClinic.biz [Azure ontwikkelaars hulpmiddelen voor QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Meer informatie over de [Code eerst naar een nieuwe database](https://msdn.microsoft.com/data/jj193542) aanpak entiteit Framework die in deze zelfstudie wordt gebruikt.
-   Meer informatie over [WebApps in Azure App-Service](../app-service-web/app-service-web-overview.md).
-   Leer hoe u [monitor](cache-how-to-monitor.md) de cache in de portal van Azure.

-   Azure bestand Vgx. Cache premium-functies verkennen
    -   [Het configureren van permanente voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-persistence.md)
    -   [Het configureren van cluster voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-clustering.md)
    -   [Het configureren van Virtual Network ondersteuning voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-vnet.md)
    -   Raadpleeg de [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte met premium cache.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

