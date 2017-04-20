<properties
    pageTitle="Cache ASP.NET Session State Provider | Microsoft Azure"
    description="Meer informatie over het opslaan van de status van de ASP.NET-sessie met Azure-Cache bestand Vgx."
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Status van de sessie ASP.NET-Provider voor Azure bestand Vgx. Cache

Azure bestand Vgx. Cache biedt een sessie staat-provider waarmee u kunt de status van de sessie opslaan in een cache in plaats van in het geheugen of in een SQL Server-database. Als u wilt gebruiken in de cache sessie staat-provider, eerst configureren de cache en configureer uw ASP.NET-toepassing voor het bestand Vgx. Cache sessie staat NuGet-pakket met cache.

Het praktisch vaak niet in een echte cloud-app om te voorkomen dat sommige formulier van staat op te slaan voor de sessie van een gebruiker, maar sommige benaderingen invloed hebben op prestaties en schaalbaarheid meer dan andere. Als er voor de opslag van staat, is de beste oplossing te behouden de hoeveelheid staat kleine in cookies opslaan. Als dat niet haalbaar is, wordt de volgende aanbevolen oplossing ASP.NET-sessie state gebruiken met een provider voor verdeeld, in het geheugen cache. De slechtste oplossing van een prestaties en schaalbaarheid oogpunt is het gebruik van een database back sessie staat provider. In dit onderwerp bevat richtlijnen over het gebruik van de ASP.NET-sessie staat-Provider voor Azure bestand Vgx. Cache. Zie [ASP.NET Session State opties](#aspnet-session-state-options)voor informatie over andere opties van sessie staat.

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.NET-sessie staat in de cache opslaan

Als u wilt configureren een clienttoepassing in Visual Studio met het bestand Vgx. Cache sessie staat NuGet-pakket, met de rechtermuisknop op het project in **Solution Explorer** en kies **NuGet-pakketten beheren**.

![Azure bestand Vgx. Cache NuGet-pakketten beheren](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

Typ **RedisSessionStateProvider** in het vak tekst zoeken, selecteert u deze in de resultaten en klik op **installeren**.

>[AZURE.IMPORTANT] Als u de clustering functie van de laag premium gebruikt, moet u [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 of hoger of een uitzondering wordt gegenereerd. Dit is een recente wijziging; Zie [v2.0.0 Details te verbreken wijzigen](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details)voor meer informatie.

![Azure bestand Vgx. Cache Session State-Provider](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Het pakket bestand Vgx. sessie staat Provider NuGet heeft een afhankelijkheid van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName in uw project die wordt geïnstalleerd ontbreekt. Houd er rekening mee dat naast het pakket StackExchange.Redis.StrongName sterke naam ook de versie StackExchange.Redis niet-sterke naam is. Als uw project is met een niet-sterke naam StackExchange.Redis versie dat moet u dit verwijderen voor of na de installatie van het bestand Vgx. sessie staat Provider NuGet-pakket, wordt anders u krijgen naamconflicten in uw project. Zie voor meer informatie over deze pakketten [configureren .NET cache-clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Het pakket NuGet downloads en voegt de vereiste constructie verwijzingen en voegt dat het volgende voegt u de volgende sectie in uw web.config-bestand met de vereiste configuratie voor ASP.NET-toepassingen naar het bestand Vgx. Cache sessie staat Provider gebruikt.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

De sectie opmerkingen bevat een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache blade in de Portal van Microsoft Azure en configureren van de andere waarden naar wens. Zie voor instructies over toegang tot uw cache-eigenschappen, [in de cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings).

-   **host** : Geef uw cache-eindpunt.
-   **poort** – gebruik uw niet-SSL-poort of uw SSL poort, afhankelijk van de ssl-instellingen.
-   **accessKey** – de toets primaire of secundaire gebruiken voor de cache.
-   **ssl** -waar als u wilt beveiligen cache/client communicatie met ssl; of false. Zorg ervoor dat de juiste poort opgeven.
    -   De niet-SSL-poort is standaard uitgeschakeld voor nieuwe cache. Geef op waar deze optie om de SSL-poort. Zie de sectie [Toegang poorten](cache-configure.md#access-ports) in het onderwerp [een cache configureren](cache-configure.md) voor meer informatie over het inschakelen van de niet-SSL-poort.
-   **throwOnError** – waar als u wilt dat een uitzondering gemaakt in het geval van een storing of ONWAAR als u wilt dat de bewerking stilte mislukt. U kunt op een fout controleren door de statische Microsoft.Web.Redis.RedisSessionStateProvider.LastException eigenschap te controleren. De standaardinstelling is voldaan.
-   **retryTimeoutInMilliseconds** – bewerkingen die niet zijn opnieuw geprobeerd tijdens dit interval wordt opgegeven (in milliseconden). De eerste poging doet zich na 20 milliseconden, en vervolgens nieuwe pogingen elke tweede totdat het interval retryTimeoutInMilliseconds is verstreken. Direct na dit interval, is de bewerking opnieuw geprobeerd definitief eenmalig. Als het nog steeds mislukt, wordt de uitzondering terug naar de beller, afhankelijk van de instelling throwOnError gegenereerd. De standaardwaarde is aan 0, wat betekent er geen nieuwe pogingen dat.
-   **databaseId** – bepaalt welke database wilt gebruiken voor de cache gegevens uitvoeren. Als u niet opgeeft, wordt de standaardwaarde van 0 gebruikt.
-   **applicationName** – sleutels zijn opgeslagen in het bestand Vgx. Als `{<Application Name>_<Session ID>}_Data`. Hiermee worden meerdere toepassingen delen dezelfde sleutel. Deze parameter is optioneel en als u deze niet opgeeft een standaardwaarde wordt gebruikt.
-   **connectionTimeoutInMilliseconds** – met deze instelling kunt u de instelling connectTimeout in de client StackExchange.Redis te negeren. Als u niet opgeeft, wordt de standaardinstelling voor connectTimeout van 5000 gebruikt. Zie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705)voor meer informatie.
-   **operationTimeoutInMilliseconds** – met deze instelling kunt u de instelling syncTimeout in de client StackExchange.Redis te negeren. Als u niet opgeeft, wordt de standaardinstelling voor syncTimeout van 1000 gebruikt. Zie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705)voor meer informatie.

Zie voor meer informatie over deze eigenschappen, de oorspronkelijke weblog posten aankondiging via [Aankondigen ASP.NET sessie staat Provider voor het bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Vergeet niet de standaard InProc sessie staat provider sectie in uw web.config opmerkingen te maken.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Nadat u deze stappen worden uitgevoerd, worden uw toepassing is geconfigureerd voor het bestand Vgx. Cache sessie staat Provider gebruikt. Wanneer u de status van de sessie in uw toepassing gebruikt, wordt deze opgeslagen in een exemplaar van de Azure bestand Vgx. Cache.

>[AZURE.NOTE] Houd er rekening mee dat in de cache opgeslagen gegevens moet serialiseerbaar, in tegenstelling tot de gegevens die kunnen worden opgeslagen in de standaard staat in het geheugen ASP.NET-sessie Provider. Wanneer de sessie staat-Provider voor het bestand Vgx. wordt gebruikt, moet u dat de gegevenstypen die worden opgeslagen in de status van de sessie serialiseerbaar zijn.

## <a name="aspnet-session-state-options"></a>Status van de sessie ASP.NET-opties

- In het geheugen Session State-Provider - opgeslagen deze provider de status van de sessie in het geheugen. Het voordeel van het gebruik van deze provider is dat het snel en eenvoudig is. U kunt geen echter de schaal van uw Web-Apps als u in het geheugen provider gebruikt omdat deze niet wordt gedistribueerd.

- SQL Server-sessie status Provider - deze provider wordt de sessie-status opgeslagen in Sql Server. Als u wilt behouden de status van de sessie in een permanente opslag, moet u deze provider gebruiken. U kunt uw Web-App schalen, maar met behulp van Sql Server voor sessie heeft invloed op de prestaties op uw Web-App.

- Verspreid In geheugen sessie staat serviceprovider zoals bestand Vgx. Cache sessie staat Provider - deze provider geeft u het beste van beide wereld. Uw Web-App kan een eenvoudige, snelle en scalable sessie staat Provider hebben. U moet wel behouden in aanmerking dat deze provider wordt de status van de sessie in een Cache opgeslagen en uw app maken in de aandacht van alle kenmerken die zijn gekoppeld moet wanneer een gedistribueerde In geheugencache zoals tijdelijke netwerkstoringen spreekt. Zie voor aanbevelingen over het gebruik van de Cache, [cache richtlijnen](../best-practices-caching.md) uit Microsoft Patterns & procedures [Azure Cloud toepassing ontwerpen en implementatiehandleiding voor ondernemingen](https://github.com/mspnp/azure-guidance).

Zie [Aanbevolen procedures voor de ontwikkeling van Web (Building echte Cloud-Apps gebruiken met Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices)voor meer informatie over de status van de sessie en andere aanbevolen procedures.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [ASP.NET uitvoer Cache-Provider voor Azure bestand Vgx. Cache](cache-aspnet-output-cache-provider.md).
