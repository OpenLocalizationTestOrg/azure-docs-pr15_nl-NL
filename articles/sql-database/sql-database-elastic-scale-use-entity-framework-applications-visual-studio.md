<properties 
    pageTitle="Gebruik van elastische database client-bibliotheek met entiteit Framework | Microsoft Azure" 
    description="Elastische Database clientbibliotheek en entiteit Framework gebruiken voor het coderen van databases" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastische Database client-bibliotheek met entiteit Framework 
 
In dit document ziet u de wijzigingen in een Entity Framework-toepassing die nodig zijn voor integratie met de [Hulpmiddelen voor databases elastische](sql-database-elastic-scale-introduction.md). De focus heeft [shard toewijzen management](sql-database-elastic-scale-shard-map-management.md) en [gegevens-afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met de **Code eerste** entiteit Framework methode voor het bericht. De [Eerste Code – nieuwe Database](http://msdn.microsoft.com/data/jj193542.aspx) zelfstudie voor EF fungeert als onze actieve voorbeeld in dit document. De voorbeeldcode bijbehorende van dit document maakt deel uit van elastische hulpmiddelen voor databases set voorbeelden in de voorbeelden van de Visual Studio-Code.
  
## <a name="downloading-and-running-the-sample-code"></a>Downloaden en uitvoeren van de steekproef-Code
Downloaden van de code voor in dit artikel:

* Visual Studio 2012 of hoger is vereist. 
* Visual Studio starten. 
* In Visual Studio-Selecteer Bestand > Nieuw Project. 
* Klik in het dialoogvenster 'Nieuw Project' Navigeer naar de **Online voorbeelden** voor **Visual C#** en typ 'elastische db' in het zoekvak in de rechterbovenhoek.
    
    ![Entiteit Framework en elastische database steekproef-app][1] 

    Selecteer de steekproef **Elastische DB-hulpprogramma's voor Azure SQL-entiteit Framework integratie**genoemd. Nadat de licentie is geaccepteerd, de steekproef wordt geladen. 

Als u wilt uitvoeren in het voorbeeld, moet u drie lege databases maken in Azure SQL-Database:

* Shard kaart Manager-database
* Shard 1-database
* Shard 2-database

Nadat u deze databases hebt gemaakt, voer de locatie-houders in **Program.cs** met de naam van de DB van Azure SQL-server, de namen van de database en uw referenties verbinding maken met de databases. Maak de oplossing in Visual Studio. Visual Studio, wordt de vereiste NuGet-pakketten voor de bibliotheek van de client elastische database entiteit Framework en tijdelijke foutenstructuuranalyse afhandelen als onderdeel van het proces opbouwen gedownload. Zorg ervoor dat NuGet-pakketten herstellen is ingeschakeld voor uw oplossing. U kunt deze instelling inschakelen door met de rechtermuisknop op het oplossingsbestand in Visual Studio Solution Explorer. 

## <a name="entity-framework-workflows"></a>Entiteit Framework werkstromen 

Entiteit Framework ontwikkelaars vertrouwen op een van de volgende vier werkstromen voor het samenstellen van toepassingen en om ervoor te zorgen permanente voor toepassingsobjecten: 

* **Code eerste (nieuwe Database)**: de EF ontwikkelaars Hiermee maakt u het model in de toepassingscode en EF genereert vervolgens de database uit. 
* **Code eerste (bestaande Database)**: de ontwikkelaar kunt EF genereren van de toepassingscode voor het model van een bestaande database.
* **Eerste model**: de ontwikkelaar maakt het model in de ontwerpfunctie voor EF en EF maakt vervolgens de database uit het model.
* **Eerste database**: de ontwikkelaar EF gereedschap om te leiden van het model van een bestaande database wordt gebruikt. 

Alle volgende manieren, is afhankelijk van de klas DbContext transparant databaseverbindingen en databaseschema voor een toepassing beheren. Database bootstrappen en schemabestanden maken zoals wordt later in het document uitgebreider worden besproken verschillende constructors op de klasse DbContext base toestaan voor verschillende detailniveaus controle over het maken van verbinding. Uitdagingen zich voordoen hoofdzakelijk uit het feit dat het beheer van de database-verbinding verstrekt door EF met de verbinding beheermogelijkheden van de gegevens afhankelijke routeren interfaces verstrekt snijdt door de bibliotheek van de client elastische database. 

## <a name="elastic-database-tools-assumptions"></a>Elastische database extra hypothesen 

Zie voor term definities [elastische Database extra woordenlijst](sql-database-elastic-scale-glossary.md).

Met elastische database client-bibliotheek definieert u delen van de toepassingsgegevens van uw shardlets genoemd. Shardlets worden aangegeven met een sleutel sharding en zijn toegewezen aan specifieke databases. Een toepassing mogelijk zo veel databases zo nodig hebt en de shardlets bieden onvoldoende capaciteit of prestaties gegeven van de huidige bedrijfsbehoeften distribueren. De toewijzing van sharding waarden van de database is opgeslagen door een shard overzicht van de databaseclient elastische API's. Verwijst naar deze mogelijkheid **Shard kaart Management**of SMM voor korte. De kaart shard oefent ook de makelaar van databaseverbindingen voor aanvragen die een sleutel sharding uitvoeren. We raadpleegt u deze mogelijkheid als **gegevens-afhankelijke-mailroutering**. 
 
De manager van de map shard voorkomt gebruikers inconsistente weergaven op shardlet-gegevens die optreden mogelijk bij gelijktijdige shardlet beheertaken uit te voeren (zoals gegevens uit één shard verplaatsen naar een andere) zijn er. Klik hiervoor beheerd de toewijzingen shard door de client bibliotheek makelaar de databaseverbindingen voor een toepassing. Hiermee kunt de functionaliteit van de map shard aan een database-verbinding automatisch afbreken wanneer shard management bewerkingen van invloed op de shardlet die zijn kunnen voor de verbinding is gemaakt. Deze methode moet integreren met enkele van de functionaliteit van EF, zoals het maken van nieuwe verbindingen van een bestaande publicatie wilt controleren op bestaan van database. In het algemeen, is onze waarnemingen dat de standaard DbContext constructors alleen werk betrouwbaar voor gesloten databaseverbindingen die veilig kunnen worden gekopieerd voor EF werken. Het ontwerpprincipe van elastische database is in plaats daarvan alleen broker geopende verbindingen. Een denkt dat het sluiten van een verbinding brokered door de clientbibliotheek voordat afstaat naar de DbContext EF probleem mogelijk oplossen. Echter door te sluiten van de verbinding en te vertrouwen op EF opnieuw om dit te openen, foregoes een validatie en consistentie controles uitgevoerd door de bibliotheek. De functionaliteit voor migraties in EF, wordt deze verbindingen echter gebruikt voor het beheren van het onderliggende databaseschema op een manier die transparant met de toepassing is. In het ideale geval willen we behouden en het combineren van alle deze mogelijkheden van de elastische database clientbibliotheek en de EF in dezelfde toepassing. De volgende sectie wordt beschreven hoe deze eigenschappen en vereisten uitgebreider. 


## <a name="requirements"></a>Vereisten 

Tijdens het werken met de elastische database clientbibliotheek en de entiteit Framework-API's, die we wilt bewaren van de volgende eigenschappen: 

* **Schalen**: toevoegen aan of verwijderen van databases uit de gegevenslaag van de een laptopgeheugen toepassing zo nodig voor de eisen van de capaciteit van de toepassing. Dit betekent dat besturingselement via de het maken en verwijderen van databases en het gebruik van de shard elastische database manager-API's voor het beheren van databases, en de toewijzingen van shardlets toewijzen. 

* **Consistentie**: de toepassing sharding in dienst en gebruikt u de gegevens afhankelijke routeren mogelijkheden van de clientbibliotheek. Om te voorkomen beschadigde bestanden of verkeerde queryresultaten, verbindingen zijn brokered tot en met de manager van de map shard. Dit houdt ook gegevensvalidatie en consistentie.
 
* **Eerste code**: moeten worden bewaard het gemak van de EF code eerste model. Klassen in de toepassing zijn in eerste Code, transparant toegewezen aan de onderliggende databasestructuren. De toepassingscode communiceert met DbSets die de meeste aspecten van de onderliggende database verwerking masker.
 
* **Schema**: entiteit Framework verwerkt maken van de eerste database schema en de ontwikkeling van de volgende schema via migraties. Door te bewaren deze mogelijkheden, is aan te passen uw app gemakkelijk als de gegevens ontwikkeld. 

Hoe u aan deze vereisten voldoen voor Code eerste toepassingen met hulpmiddelen voor databases elastische Hiermee geeft u de volgende richtlijnen. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Gegevens afhankelijke routering EF DbContext gebruiken 

Databaseverbindingen met entiteit Framework worden gewoonlijk beheerd via subcategorieën van **DbContext**. Maak deze subcategorieën door die voortvloeien uit **DbContext**. Dit is waar u uw **DbSets** die de database-back-verzamelingen van CLR-objecten voor uw toepassing implementeren definiëren. In de context van gegevens afhankelijke routering, kunnen we vaststellen om wat verschillende handig eigenschappen die niet per se voor andere EF code eerste toepassing scenario's bewaart: 

* De database al bestaat en in de database elastische shard kaart is geregistreerd. 
* Het schema van de toepassing is al geïmplementeerd in de database (Zie hieronder). 
* Gegevens-afhankelijke verbindingen met de database zijn brokered door de kaart shard. 

**DbContexts** integreren met gegevens-afhankelijke omleiding voor schalen:

1. Verbindingen van de fysieke database via de elastische database client-interfaces van de manager van de map shard maken 
2. De verbinding met de subcategorie **DbContext** teruglopen
3. Laat de verbinding door in het grondtal klassen **DbContext** om ervoor te zorgen dat de verwerking aan de kant EF gebeurt er als u ook. 

Het volgende voorbeeld ziet u deze methode. (Deze code is ook in het bijbehorende Visual Studio-project)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Belangrijkste punten
* Een nieuwe constructor vervangt de standaardconstructor in de subcategorie DbContext 
* De nieuwe constructor kost de argumenten die vereist zijn voor afhankelijke routering van gegevens via elastische database client-bibliotheek:
    * de kaart shard voor toegang tot de gegevens-afhankelijke routeren interfaces,
    * de toets sharding de shardlet, identificeren
    * een verbindingsreeks met de referenties voor de gegevens-afhankelijke routeren verbinding met de shard. 
 
* De oproep door naar de basisklassenconstructor rekening een detour een statische methode waarmee alle stappen uitgevoerd nodig voor gegevens-afhankelijke-mailroutering. 
   * Deze gebruikt de OpenConnectionForKey-aanroep van de client-interfaces elastische database op de kaart shard tot stand brengen van een open verbinding.
   * De kaart shard Hiermee maakt u de verbinding met de shard waarin de shardlet voor de opgegeven sharding-sleutel openen.
   * Deze open verbinding wordt doorgegeven terug naar de basisklassenconstructor van DbContext om aan te geven dat deze verbinding wordt gebruikt door EF in plaats van laten EF automatisch een nieuwe verbinding maken. Deze manier de verbinding heeft getagd door de databaseclient elastische API, zodat deze consistentie onder shard kaart management bewerkingen kunt garanderen.
 
  
Gebruik de nieuwe constructor uw subcategorie DbContext in plaats van de standaardconstructor in uw code. Hier volgt een voorbeeld: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

De nieuwe constructor wordt geopend de verbinding met de shard dat de gegevens voor de shardlet die wordt aangeduid met de waarde van **tenantid1**bevat. De code in het blok **gebruiken** blijft voor toegang tot de **DbSet** voor weblogs EF met op de shard voor **tenantid1**ongewijzigd. Hiermee wordt semantiek voor de code in het met blokkeren zodat alle databasebewerkingen zijn nu beperkt tot het één shard waar **tenantid1** wordt bewaard. Een query LINQ via de blogs **DbSet** retourneert bijvoorbeeld alleen blogs die zijn opgeslagen op de huidige shard, maar niet de bestanden die zijn opgeslagen op andere shards.  

#### <a name="transient-faults-handling"></a>Tijdelijke fouten verwerken
Het team van Microsoft Patterns en procedures die zijn gepubliceerd de [Het tijdelijke foutenstructuuranalyse afhandelen toepassing blokkeren](https://msdn.microsoft.com/library/dn440719.aspx). De bibliotheek wordt met elastische schalen client-bibliotheek in combinatie met EF gebruikt. Echter ervoor te zorgen dat een tijdelijke uitzondering keert terug naar de locatie waar we ervoor zorgen kunt dat de nieuwe constructor wordt gebruikt na een tijdelijke foutenstructuuranalyse zodat nieuwe verbinding wordt geprobeerd het gebruik van de constructors die we hebt toegepast. Anders een verbinding met de juiste shard is niet gegarandeerd en er zijn geen garanties van die de verbinding wordt bijgehouden als er wijzigingen aan de map shard optreden. 

Het volgende voorbeeld ziet u hoe een SQL opnieuw beleid rond de nieuwe **DbContext** subcategorie constructors kan worden gebruikt: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen nieuwe pogingen. Deze methode is vergelijkbaar met de richtlijnen voor EF en door de gebruiker geactiveerde transacties (Zie [beperkingen met opnieuw kan worden uitgevoerd strategieën (hoger EF6)](http://msdn.microsoft.com/data/dn307226). Beide situaties vereisen dat de toepassing besturingselementen voor het bereik waarnaar de tijdelijke uitzondering geeft als resultaat: de transactie opent of maak opnieuw de context van de juiste constructor (zoals wordt getoond) die de bibliotheek van de client elastische database gebruikt.

De nodig om te bepalen waar tijdelijke uitzonderingen in beslag terug in de reikwijdte wordt ook het gebruik van de ingebouwde **SqlAzureExecutionStrategy** die wordt geleverd met EF uitsluit. **SqlAzureExecutionStrategy** zou opnieuw opent een verbinding, maar niet **OpenConnectionForKey** gebruik en kunnen daarom alle validatie die wordt uitgevoerd als onderdeel van het gesprek **OpenConnectionForKey** over te slaan. In het voorbeeld gebruikt in plaats daarvan de ingebouwde **DefaultExecutionStrategy** die ook wordt geleverd met EF. In tegenstelling tot **SqlAzureExecutionStrategy**werkt correct in combinatie met het beleid opnieuw tijdelijke foutenstructuuranalyse verwerkt. Het beleid is ingesteld in de klas **ElasticScaleDbConfiguration** . Houd er rekening mee dat we besloten niet te **DefaultSqlExecutionStrategy** gebruiken omdat deze stappen worden voorgesteld als u wilt gebruiken **SqlAzureExecutionStrategy** als tijdelijke uitzonderingen optreden - die tot verkeerde gedrag leiden kunnen zoals is beschreven. Zie voor meer informatie over het beleid van de verschillende opnieuw en EF [Verbinding tolerantie in EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Constructor herschreven
De van bovenstaande codevoorbeelden ziet u de standaard constructor opnieuw schrijft vereist voor uw toepassing om te kunnen gebruiken van gegevens afhankelijke routering met het kader entiteit. De volgende tabel generaliseert deze methode naar andere constructors verdwijnt. 


Huidige Constructor  | Herschreven Constructor voor gegevens | Basis Constructor | Notities
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding moet een functie van de kaart shard en de gegevens-afhankelijke routeren-toets. U moet omloopleiding automatisch verbinding maken met EF maar in plaats daarvan shard tekens gebruiken om het broker de verbinding. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |De verbinding is een functie van de kaart shard en de gegevens-afhankelijke routeren-toets. Een vaste database naam of verbindingstekenreeks werkt niet als ze omloopleiding validatie door de kaart shard. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boole) |De verbinding wordt gemaakt voor de opgegeven shard kaart en sharding sleutel het model. Het gecompileerde model wordt worden doorgegeven aan de basis c'tor.
MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, Boole) |DbContext (DbConnection, bool) |De verbinding moet worden afgeleid van de kaart shard en de toets. Dit kan niet worden verstrekt als invoer (tenzij deze invoer al de shard-kaart en de toets gebruikt is). De Booleaanse waarde wordt doorgegeven op. 
MyContext (tekenreeks, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boole) |De verbinding moet worden afgeleid van de kaart shard en de toets. Dit kan niet worden verstrekt als invoer (tenzij deze invoer is voorzien van de kaart shard en de toets). Klik op het gecompileerd model doorgegeven. 
MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |De nieuwe constructor nodig heeft om ervoor te zorgen dat een verbinding in de ObjectContext doorgegeven als invoer omgeleid naar een verbinding beheerd door elastische schaal wordt. Gedetailleerde informatie over de ObjectContexts valt buiten het bereik van dit document.
MyContext (DbConnection, DbCompiledModel, Boole) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, bool)| DbContext (DbConnection, DbCompiledModel, Boole); |De verbinding moet worden afgeleid van de kaart shard en de toets. De verbinding kan niet worden verstrekt als invoer (tenzij deze invoer al de shard-kaart en de toets gebruikt is). Model en Booleaanse waarde worden aan doorgegeven grondtal klasse. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard schema implementatie via EF migraties 

Automatische schema management is verstrekt door het kader entiteit gemakkelijker te maken. In de context van toepassingen gebruiken elastische databasehulpmiddelen, willen we deze mogelijkheid voor het inrichten van het schema gemaakte shards automatisch wanneer databases worden toegevoegd aan de toepassing een laptopgeheugen behouden. De primaire use-case is om uit te breiden capaciteit in de gegevenslaag voor een laptopgeheugen toepassingen EF gebruiken. Afhankelijk van de mogelijkheden van EF om uw schema te beheren Hiermee reduceert u de hoeveelheid van de beheer van de database met een een laptopgeheugen toepassing gebouwd op EF. 

Schema implementatie via EF migraties werkt het beste **ongeopend verbindingen**. Dit is in tegenstelling tot het scenario voor afhankelijk van de gegevens routering die afhankelijk zijn van de geopende verbinding opgegeven door de databaseclient elastische API. Een ander verschil is de vereiste consistentie: terwijl wenselijk voor alle gegevens-afhankelijke routeren verbindingen ter bescherming tegen gelijktijdige shard kaart voor een consistente, het niet uitmaakt eerste schema implementatie naar een nieuwe database die nog niet is geregistreerd in de map shard en nog niet zijn toegewezen aan shardlets houdt. We kunnen daarom vertrouwen op normale databaseverbindingen voor dit scenario's, in tegenstelling tot gegevens-afhankelijke-mailroutering.  

Dit leidt tot een aanpak waar schema implementatie via EF migraties is nauw verbonden met de registratie van de nieuwe database als een shard in van de toepassing shard kaart. Dit is afhankelijk van de volgende vereisten: 

* De database is al gemaakt. 
* De database leeg is, maar deze bevat geen gebruikersschema en geen gebruikersgegevens.
* De database niet nog toegankelijk via de elastische databaseclient-API voor gegevens-afhankelijke-mailroutering. 

Met deze vereisten op hun plaats staan, kunnen we een normale niet geopende **SqlConnection** op gang EF migraties voor de implementatie van schema maken. Het volgende voorbeeld ziet u deze methode. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

In dit voorbeeld ziet u de methode **RegisterNewShard** die de shard registreert in de map shard, het schema via EF migraties implementeert, worden opgeslagen en een toewijzing van een sleutel sharding naar de shard. Dit is afhankelijk van een constructor van de **DbContext** subcategorie (**ElasticScaleContext** in de steekproef) waarmee een SQL-verbindingsreeks als invoer. De code van deze opbouwfunctie is uitgestreken doorsturen, zoals het volgende voorbeeld: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Een mogelijk de versie van de constructor overgenomen van de klasse base gebruikt. Maar de code nodig heeft om ervoor te zorgen dat de standaard-initialisatiefunctie voor EF wordt gebruikt wanneer u verbinding maakt. De korte detour daarom in de statische methode aan voordat u in de basisklassenconstructor met de verbindingsreeks. Houd er rekening mee dat de registratie van shards wordt uitgevoerd in een andere app domein of het proces om ervoor te zorgen dat de instellingen initialisatiefunctie voor EF niet conflicteren. 


## <a name="limitations"></a>Beperkingen 

De methoden beschreven in dit document leiden tot een aantal beperkingen: 

* EF-toepassingen die gebruikmaken van **LocalDb** moeten eerst om te migreren naar een gewone SQL Server-database voordat u Elastische database client-bibliotheek. Een toepassing tot en met sharding schalen met elastische schaal is niet mogelijk met **LocalDb**. Houd er rekening mee dat ontwikkeling **LocalDb**nog steeds kunt gebruiken. 

* Wijzigingen in de toepassing die database schemawijzigingen houdt in dat moeten EF migraties op alle shards doorlopen. De voorbeeldcode voor dit document bevat niet laten zien hoe u dit wilt doen. Overweeg het gebruik van de Update-Database met een parameter ConnectionString worden herhaald voor alle shards; of te extraheren van de T-SQL-script voor de in behandeling migratie Update-Database gebruikt met het gedeelte-Script option en de T-SQL-script toepassen op uw shards.  

* Een verzoek voor een gegeven, wordt uitgegaan dat alle de verwerking van de database zich in een enkel shard zoals die wordt aangeduid met de toets sharding is verstrekt door het verzoek. Echter wordt deze verondersteld niet altijd houdt waar. Bijvoorbeeld wanneer deze is niet mogelijk een sharding-toets om beschikbaar te maken. Om dit op te lossen, biedt de clientbibliotheek de **MultiShardQuery** -klasse die een verbinding abstraction voor query's uitvoeren op meerdere shards implementeert. Leren de **MultiShardQuery** gebruiken in combinatie met EF valt buiten het bereik van dit document

## <a name="conclusion"></a>Sluiten

Via de stappen in dit document, kunnen EF toepassingen van de elastische database clientbibliotheek de mogelijkheid voor afhankelijke routering van gegevens door refactoring constructors van de **DbContext** subcategorieën gebruikt in de toepassing EF gebruiken. Hiermee wordt de wijzigingen die zijn vereist naar deze locaties waar **DbContext** klassen reeds bestaande beperkt. Bovendien kunnen EF toepassingen profiteren van automatische schema implementatie door een combinatie van de stappen die de benodigde EF migraties met de registratie van nieuwe shards en toewijzingen in de toewijzing shard roepen blijven. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 