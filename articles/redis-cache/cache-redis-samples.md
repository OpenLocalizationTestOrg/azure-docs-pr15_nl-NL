<properties 
    pageTitle="Voorbeelden van Azure Cache bestand Vgx. | Microsoft Azure" 
    description="Informatie over het gebruik van de Cache van Azure bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Voorbeelden van Azure Cache bestand Vgx. 

In dit onderwerp vindt u een lijst van Azure bestand Vgx. Cache steekproeven, die betrekking hebben op scenario's zoals verbinding maken met een cache, lezen en schrijven gegevens uit een cache en het bestand Vgx. ASP.NET-Cache-providers gebruiken. Enkele van de voorbeelden zijn downloadbare projecten en enkele bieden stapsgewijze instructies en codefragmenten opnemen maar geen koppelt aan een downloadbare project.

## <a name="hello-world-samples"></a>Hallo wereld voorbeelden

In de voorbeelden in deze sectie worden weergegeven de basisbeginselen van het verbinding maken met een exemplaar van de Azure bestand Vgx. Cache en lezen en schrijven van gegevens aan de cache met een verscheidenheid aan talen en bestand Vgx. clients.

De steekproef [Hallo allemaal](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ziet hoe u verschillende cache bewerkingen van de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-client uitvoeren.

In dit voorbeeld ziet u hoe u:

-   Gebruik van verschillende verbindingsopties
-   Lezen en schrijven van objecten en naar de cache bewerkingen synchroon en asynchroon
-   Bestand Vgx. MGET/MSET-opdrachten gebruiken om waarden van de opgegeven toetsen te retourneren
-   Bestand Vgx. transacties bewerkingen uitvoeren
-   Werken met lijsten bestand Vgx. en gesorteerd sets weergegeven
-   .NET-objecten met JsonConvert objectserializers opslaan
-   Gebruik bestand Vgx. sets willen implementeren labelen
-   Werken met Cluster bestand Vgx.

Zie de documentatie [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) op github en scenario's Zie voor meer gebruik de tests van de eenheid [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) voor meer informatie.

[Het gebruik van Azure bestand Vgx. Cache met Python](cache-python-get-started.md) ziet hoe u het aan de slag met Azure bestand Vgx. Cache Python en de client [bestand Vgx. kopiëren](https://github.com/andymccurdy/redis-py) gebruiken.

[Werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) ziet u een manier om te serialiseren .NET-objecten, zodat u kunt ze wilt schrijven en vanaf een exemplaar van de Azure bestand Vgx. Cache. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Cache bestand Vgx. Als een schaal out Backplane voor ASP.NET-SignalR gebruiken

Het [Bestand Vgx. Cache gebruiken als een schaal out Backplane voor ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) voorbeeld wordt gedemonstreerd hoe u Azure bestand Vgx. Cache als een backplane SignalR kunt gebruiken. Zie voor meer informatie over backplane, [SignalR Scaleout met het bestand Vgx.](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Bestand Vgx voorbeeld van een query Cache klant.

In dit voorbeeld wordt gedemonstreerd vergelijkt prestaties tussen toegang krijgen tot gegevens in een cache en toegang tot gegevens uit de permanente opslag. In dit voorbeeld bevat twee projecten.

-   [Demo bestand Vgx. Cache kunt hoe prestaties verbeteren door Caching van gegevens](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [De Database en de Cache voor de demo vullen](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-sessie staat en Caching van uitvoer

Het [Gebruik van Azure bestand Vgx. Cache voor de opslag van ASP.NET SessionState en OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) voorbeeld wordt gedemonstreerd hoe u met Azure bestand Vgx. Cache opslaan van ASP.NET-sessie en uitvoercache de SessionState en OutputCache-providers gebruiken voor het bestand Vgx..

## <a name="manage-azure-redis-cache-with-maml"></a>Beheer van Cache met MAML Azure bestand Vgx.

De [Cache voor Azure bestand Vgx. beheren met behulp van Azure Management bibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) -voorbeeld wordt gedemonstreerd hoe kunt u Azure Management bibliotheken gebruiken om te beheren - (maken / bijwerken / verwijderen) de Cache. 

## <a name="custom-monitoring-sample"></a>Aangepaste steekproef bewaken

De [gegevens bewaken van Access bestand Vgx. Cache](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) -voorbeeld wordt gedemonstreerd hoe u kunt toegang krijgen tot controlegegevens voor uw Azure bestand Vgx. Cache buiten de Azure-Portal.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Een stijl van een Twitter-klonen geschreven met PHP en bestand Vgx.

De steekproef [Retwis](https://github.com/SyntaxC4-MSFT/retwis) is het bestand Vgx. Hallo wereld. Het is een minimale Twitter-stijl sociaal netwerk klonen geschreven met bestand Vgx. en PHP de [Predis](https://github.com/nrk/predis) -client gebruiken. De broncode is bedoeld om heel eenvoudig zijn en tegelijkertijd inschakelen andere bestand Vgx. gegevensstructuren.

## <a name="bandwidth-monitor"></a>Monitor met een bandbreedte

De steekproef [bandbreedte monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) kunt u de bandbreedte gebruikt op de client volgen. Als u wilt meten de bandbreedte, de steekproef worden uitgevoerd op de clientcomputer cache, gesprekken te voeren naar de cache en de bandbreedte gemeld door de bandbreedte monitor steekproef toekijken.
