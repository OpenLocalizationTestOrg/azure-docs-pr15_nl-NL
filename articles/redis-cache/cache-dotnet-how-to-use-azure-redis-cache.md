<properties 
    pageTitle="Het gebruik van Azure bestand Vgx. Cache | Microsoft Azure" 
    description="Meer informatie over het verbeteren van de prestaties van uw Azure-toepassingen met Azure-Cache bestand Vgx." 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Het gebruik van de Cache Azure bestand Vgx.

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Deze handleiding ziet u hoe u aan de slag met **Azure bestand Vgx. Cache**. Microsoft Azure bestand Vgx. Cache is gebaseerd op de bron voor populaire openen bestand Vgx. Cache. Dit hebt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. Een cache gemaakt met behulp van Azure bestand Vgx. Cache is toegankelijk zijn vanuit een Microsoft Azure-toepassing.

Microsoft Azure bestand Vgx. Cache is beschikbaar in de volgende lagen:

-   **Eenvoudige** – één knooppunt. Veelvoud groter maximaal 53 GB.
-   **Standaard** – twee knooppunten primaire/Replica. Veelvoud groter maximaal 53 GB. 99,9 SLA %.
-   **Premium** – twee knooppunten primaire/Replica met maximaal 10 shards. Meerdere formaten vanuit 6 GB 530 GB (contact met ons opnemen voor meer informatie). Alle onderdelen van de standaard laag en meer inclusief ondersteuning voor het [bestand Vgx. cluster](cache-how-to-premium-clustering.md), [bestand Vgx. permanente](cache-how-to-premium-persistence.md)en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 SLA %.

Elke laag verschilt gebied van voorzieningen en prijzen. Zie voor informatie over prijzen, [Cache prijzen Details][].

Deze handleiding ziet u hoe u het gebruik van de [StackExchange.Redis][] -client met C\# code. De scenario's waarvoor zijn **maken en configureren van een cache** **cache clients configureren**en **toevoegen en verwijderen van objecten uit de cache**. Raadpleeg de sectie van de [Volgende stappen][] voor meer informatie over het gebruik van Azure bestand Vgx. Cache. Zie voor een stapsgewijze zelfstudie van het samenstellen van een ASP.NET-MVC WebApp met de Cache bestand Vgx. [het maken van een Web-App met bestand Vgx. Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Aan de slag met Cache Azure bestand Vgx.

Aan de slag met Azure bestand Vgx. Cache gaat heel eenvoudig. Als u wilt beginnen, moet u inrichten en configureren van een cache. Vervolgens configureert u de cache-clients zodat ze toegang hebt tot de cache. Nadat de cache-clients zijn geconfigureerd, kunt u werken met hen beginnen.

-   [De cache maken][]
-   [De cache-clients configureren][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Een cache maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Toegang tot de cache nadat deze gemaakt

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Zie voor meer informatie over het configureren van de cache [het configureren van Azure bestand Vgx. Cache](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>De cache-clients configureren

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Wanneer uw project client is geconfigureerd voor het in cache opslaan, kunt u de technieken beschreven in de volgende secties voor het werken met de cache.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Werken met cache

De stappen in deze sectie wordt uitgelegd hoe u het uitvoeren van algemene taken met Cache.

-   [Verbinding maken met de cache][]
-   [Toevoegen en objecten op te halen uit de cache][]
-   [Werken met .NET-objecten in de cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Verbinding maken met de cache

Via een programma werkt met een cache, moet u een verwijzing naar de cache. De volgende naar het begin van een bestand waaruit u gebruiken van de client StackExchange.Redis wilt voor toegang tot een Cache Azure bestand Vgx. toevoegen.

    using StackExchange.Redis;

>[AZURE.NOTE] .NET Framework 4 of hoger is vereist door de StackExchange.Redis-client.

De verbinding met de Cache Azure bestand Vgx. wordt beheerd door de `ConnectionMultiplexer` class. Deze klasse is ontworpen om te worden gedeeld en opnieuw kan worden gebruikt in uw clienttoepassing en niet hoeft te worden gemaakt op basis van de per bewerking. 

Verbinding maken met een Azure bestand Vgx. Cache en een exemplaar van een verbonden worden geretourneerd `ConnectionMultiplexer`, de statische bellen `Connect` methode en keer in de cache eindpunt en de toets, zoals in het volgende voorbeeld. Gebruik de toets die zijn gegenereerd op basis van de Azure-Portal als de wachtwoordparameter.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Waarschuwing: Nooit store-referenties in broncode. Als u wilt houden in dit voorbeeld eenvoudige, ben ik ze zichtbaar in de broncode. Zie [hoe tekenreeksen van toepassingen en verbinding tekenreeksen werken][] voor informatie over het opslaan van referenties.

Als u niet dat SSL gebruiken wilt, eenmaal instellen `ssl=false` of weglaat de `ssl` parameter.

>[AZURE.NOTE] De niet-SSL-poort is standaard uitgeschakeld voor nieuwe cache. Zie voor instructies over het inschakelen van de niet-SSL-poort de [Access-poorten](cache-configure.md#access-ports)...

Een methode tot het delen van een `ConnectionMultiplexer` exemplaar in uw toepassing is dat een statische eigenschap die een verbonden exemplaar, vergelijkbaar met het volgende voorbeeld retourneert. Thread-veilige manier kan slechts één verbonden geïnitialiseerd `ConnectionMultiplexer` exemplaar. In deze voorbeelden `abortConnect` is ingesteld op ONWAAR, wat betekent dat de oproep slagen zelfs als de verbinding met de Azure bestand Vgx. Cache is niet gemaakt. Een belangrijke functie van `ConnectionMultiplexer` is dat deze automatisch connectivity naar de cache eenmaal het netwerkprobleem in het herstellen wordt of andere oorzaken opgelost zijn.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Zie voor meer informatie over geavanceerde configuratie verbindingsopties, [StackExchange.Redis configuratiemodel][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Nadat de verbinding tot stand is gebracht, het resultaat van een verwijzing naar de cache bestand Vgx. database door de ondersteuning voor de `ConnectionMultiplexer.GetDatabase` methode. Het object dat het resultaat van de `GetDatabase` methode is een lightweight Pass Through-object en niet hoeft te worden opgeslagen.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Als u weet hoe u verbinding maken met een exemplaar van de Azure bestand Vgx. Cache en het resultaat een verwijzing naar de cachedatabase, we even verder kijken bij het werken met de cache.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Toevoegen en objecten op te halen uit de cache

Items kunnen worden opgeslagen in en opgehaald uit een cache met behulp van de `StringSet` en `StringGet` methoden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Bestand Vgx. winkels meeste gegevens als tekenreeksen bestand Vgx., maar deze tekenreeksen veel soorten gegevens, inclusief serienummer binaire gegevens, die kan worden gebruikt voor het opslaan van .NET objecten in de cache kan bevatten.

Bij het aanroepen van `StringGet`, als het object bestaat, wordt het geretourneerd, en als dat niet `null` als resultaat gegeven. In dit geval kunt u de waarde uit de gewenste gegevensbron ophalen en opslaan in de cache voor later gebruik. Dit is het patroon dat in cache grond genoemd.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Als u wilt opgeven de vervaldatum van een item in de cache, gebruikt u de `TimeSpan` -parameter van `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Werken met .NET-objecten in de cache

Azure bestand Vgx. Cache kunt cache .NET-objecten, evenals primitieve gegevenstypen, maar voordat een .NET-object kan worden opgeslagen in de cache deze modules moet worden ingedeeld. Dit is de verantwoordelijkheid van de ontwikkelaar van toepassingen en hebt de flexibiliteit ontwikkelaars in de keuze van de serializer.

Een eenvoudige manier objecten serialiseren is via de `JsonConvert` serialisatie methoden in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) en serialiseren en naar JSON. Het volgende voorbeeld ziet u een ophalen en het gebruik van de set een `Employee` exemplaar van het object.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure bestand Vgx. Cache.

-   Bekijk de ASP.NET-providers voor Azure bestand Vgx. Cache.
    -   [Azure bestand Vgx. Session State-Provider](cache-aspnet-session-state-provider.md)
    -   [Azure bestand Vgx. Cache ASP.NET-uitvoer Cache Provider](cache-aspnet-output-cache-provider.md)
-   [Diagnostische hulpprogramma's cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat u [monitor](cache-how-to-monitor.md) de status van de cache kunt. U kunt de statistieken bekijken in de Portal Azure en kunt u ook het [downloaden en controleer](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ze met de hulpmiddelen van uw keuze.
-   Bekijk de [StackExchange.Redis cache clientdocumentatie][].
    -   Azure bestand Vgx. Cache zijn toegankelijk vanaf veel bestand Vgx. clients en ontwikkeling talen. Zie [http://redis.io/clients][]voor meer informatie.
-   Azure bestand Vgx. Cache kunnen ook worden gebruikt met services van derden en hulpprogramma's zoals Redsmin en bestand Vgx. bureaublad Manager.
    -   Zie voor meer informatie over Redsmin, [hoe u een verbindingsreeks Azure bestand Vgx. ophalen en gebruiken met Redsmin][].
    -   Openen en uw gegevens in Azure bestand Vgx. Cache met een gebruikersinterface met [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)controleren.
-   Zie de documentatie [bestand Vgx.][] en lees meer over [bestand Vgx. gegevenstypen][] en [een vijftien minuten Inleiding tot het bestand Vgx. gegevenstypen][].



<!-- INTRA-TOPIC LINKS -->
[Volgende stappen]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[De cache maken]: #create-cache
[Configure the cache]: #enable-caching
[De cache-clients configureren]: #NuGet
[Working with Caches]: #working-with-caches
[Verbinding maken met de cache]: #connect-to-cache
[Toevoegen en objecten op te halen uit de cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Het ophalen van een verbindingsreeks Azure bestand Vgx. en deze versie gebruiken met Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuratiemodel]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Details prijzen]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache clientdocumentatie]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Bestand Vgx.]: http://redis.io/documentation
[Bestand Vgx gegevenstypen.]: http://redis.io/topics/data-types
[een vijftien minuten Inleiding tot gegevenstypen bestand Vgx.]: http://redis.io/topics/data-types-intro

[De werking van tekenreeksen van toepassingen en verbindingstekenreeksen]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


