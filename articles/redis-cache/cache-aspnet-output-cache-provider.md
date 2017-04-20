<properties
    pageTitle="Cache ASP.NET-uitvoer Cache Provider"
    description="Leer hoe u ASP.NET pagina-uitvoer met Azure bestand Vgx. Cache in cache"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET-uitvoer Cache-Provider voor Azure bestand Vgx. Cache

De Provider uitvoer Cache bestand Vgx. is een out-van-process opslag om uitvoergegevens cache. Deze gegevens zijn specifiek voor volledige HTTP-antwoorden (pagina uitvoercaching van). De provider in de nieuwe uitvoer cache provider uitbreiden komma die is geïntroduceerd in ASP.NET-4 wordt geplaatst.

Als u wilt het bestand Vgx. uitvoer Cache Provider gebruikt, eerst configureren de cache en configureer uw ASP.NET-toepassing met het bestand Vgx. uitvoer Cache Provider NuGet-pakket. In dit onderwerp bevat richtlijnen over het configureren van uw toepassing voor het gebruik van de Provider bestand Vgx. uitvoer Cache. Zie voor meer informatie over het maken en configureren van een exemplaar van de Azure bestand Vgx. Cache [maken een cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Resultaat van ASP.NET-pagina in de cache opslaan

Als u wilt configureren een clienttoepassing in Visual Studio met het bestand Vgx. uitvoer Cache Provider NuGet-pakket, met de rechtermuisknop op het project in **Solution Explorer** en kies **NuGet-pakketten beheren**.

![Azure bestand Vgx. Cache NuGet-pakketten beheren](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Typ **RedisOutputCacheProvider** in het vak tekst zoeken, selecteert u deze in de resultaten en klik op **installeren**.

![Azure bestand Vgx. Cache uitvoer Cache Provider](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Het pakket bestand Vgx. uitvoer Cache Provider NuGet heeft een afhankelijkheid van het pakket StackExchange.Redis.StrongName. Als het pakket StackExchange.Redis.StrongName in uw project die wordt geïnstalleerd ontbreekt. Houd er rekening mee dat naast het pakket StackExchange.Redis.StrongName sterke naam ook de versie StackExchange.Redis niet-sterke naam is. Als uw project is met een niet-sterke naam StackExchange.Redis versie dat moet u dit verwijderen voor of na de installatie van het bestand Vgx. uitvoer Cache Provider NuGet-pakket, wordt anders u krijgen naamconflicten in uw project. Zie voor meer informatie over deze pakketten [configureren .NET cache-clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

Het pakket NuGet-downloads en voegt de vereiste constructie-verwijzingen en voegt u de volgende sectie in uw web.config-bestand met de vereiste configuratie voor uw ASP.NET-toepassing voor het gebruik van de Provider uitvoer Cache bestand Vgx..

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

De sectie opmerkingen bevat een voorbeeld van de kenmerken en voorbeelden van instellingen voor elk kenmerk.

De kenmerken configureren met de waarden uit de cache blade in de portal van Microsoft Azure en configureren van de andere waarden naar wens. Zie voor instructies over toegang tot uw cache-eigenschappen, [in de cache-instellingen configureren bestand Vgx.](cache-configure.md#configure-redis-cache-settings).

-   **host** : Geef uw cache-eindpunt.
-   **poort** – gebruik uw niet-SSL-poort of uw SSL poort, afhankelijk van de ssl-instellingen.
-   **accessKey** – de toets primaire of secundaire gebruiken voor de cache.
-   **ssl** -waar als u wilt beveiligen cache/client communicatie met ssl; of false. Zorg ervoor dat de juiste poort opgeven.
    -   De niet-SSL-poort is standaard uitgeschakeld voor nieuwe cache. Geef op waar deze optie om de SSL-poort. Zie de sectie [Toegang poorten](cache-configure.md#access-ports) in het onderwerp [een cache configureren](cache-configure.md) voor meer informatie over het inschakelen van de niet-SSL-poort.
-   **databaseId** – opgegeven welke database voor de uitvoergegevens cache wilt gebruiken. Als u niet opgeeft, wordt de standaardwaarde van 0 gebruikt.
-   **applicationName** – sleutels zijn opgeslagen in het bestand Vgx. Als <AppName>_<SessionId>_Data. Hiermee worden meerdere toepassingen delen dezelfde sleutel. Deze parameter is optioneel en als u deze niet opgeeft een standaardwaarde wordt gebruikt.
-   **connectionTimeoutInMilliseconds** – met deze instelling kunt u de instelling connectTimeout in de client StackExchange.Redis te negeren. Als u niet opgeeft, wordt de standaardinstelling voor connectTimeout van 5000 gebruikt. Zie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705)voor meer informatie.
-   **operationTimeoutInMilliseconds** – met deze instelling kunt u de instelling syncTimeout in de client StackExchange.Redis te negeren. Als u niet opgeeft, wordt de standaardinstelling voor syncTimeout van 1000 gebruikt. Zie [StackExchange.Redis configuratiemodel](http://go.microsoft.com/fwlink/?LinkId=398705)voor meer informatie.

Een instructie OutputCache toevoegen aan elke pagina waarvoor u de uitvoer cache wilt opslaan.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

In dit voorbeeld pagina in de cache blijven de gegevens in de cache voor 60 seconden en een andere versie van de pagina is opgeslagen in de cache voor elke parametercombinatie. Zie voor meer informatie over de instructie OutputCache, [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Nadat u deze stappen worden uitgevoerd, wordt uw toepassing is geconfigureerd voor gebruik van de Provider bestand Vgx. uitvoer Cache.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [Provider voor ASP.NET sessie staat voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md).
