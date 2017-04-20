<properties 
    pageTitle="Problemen met Azure bestand Vgx. Cache | Microsoft Azure" 
    description="Informatie over het oplossen van veelvoorkomende problemen met Azure bestand Vgx. Cache." 
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
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Problemen met Azure-Cache bestand Vgx.

In dit artikel bevat richtlijnen voor het oplossen van de volgende categorieën van Azure bestand Vgx. Cache problemen.

-   [Client probleemoplossing](#client-side-troubleshooting) - deze sectie bevat richtlijnen op identificeren en oplossen van problemen die worden veroorzaakt door de toepassing verbinding maakt met Azure bestand Vgx. Cache.
-   [Server probleemoplossing](#server-side-troubleshooting) - deze sectie bevat richtlijnen op identificeren en oplossen van problemen die worden veroorzaakt op de server Azure bestand Vgx. Cache.
-   [StackExchange.Redis time-out uitzonderingen](#stackexchangeredis-timeout-exceptions) - deze sectie bevat informatie over het oplossen van problemen bij het gebruik van de client StackExchange.Redis.


>[AZURE.NOTE] Verscheidene van de stappen voor probleemoplossing in deze handleiding bevatten instructies voor het bestand Vgx. opdrachten uitvoeren en controleren van verschillende prestatiegegevens. Zie de artikelen in de sectie [aanvullende informatie](#additional-information) voor meer informatie en instructies.

## <a name="client-side-troubleshooting"></a>Client van probleemoplossing


In deze sectie wordt beschreven hoe het oplossen van problemen die vanwege een voorwaarde op de clienttoepassing optreden.

-   [Geheugendruk op de client](#memory-pressure-on-the-client)
-   [Burst verkeer](#burst-of-traffic)
-   [CPU-gebruik van hoge client](#high-client-cpu-usage)
-   [Client kant bandbreedte overschreden](#client-side-bandwidth-exceeded)
-   [Grote aanvraag en respons grootte](#large-requestresponse-size)
-   [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Geheugendruk op de client

#### <a name="problem"></a>Probleem

Geheugendruk op de clientcomputer leidt naar alle soorten prestatieproblemen die verwerking van gegevens die is verzonden door het bestand Vgx. exemplaar zonder eventuele vertraging mag vertragen. Wanneer het geheugendruk raakt, heeft het systeem meestal aan de paginagegevens van fysieke geheugen virtueel geheugen, dat wil op schijf zeggen. Deze *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen.

#### <a name="measurement"></a>Maateenheden 

1.  Geheugengebruik op machine om ervoor te zorgen dat deze niet beschikbare geheugen overschrijdt. 
2.  Monitor de `Page Faults/Sec` prestatie-item. De meeste systemen wordt sommige pagina's er zelfs tijdens normale werking, dus voor pieken in deze paginafouten prestatiemeteritems die met outs overeenkomen bekijken (Engelstalig).

#### <a name="resolution"></a>Resolutie

Uw client bijwerken naar een grotere client VM-grootte met meer geheugen of verdiepen in uw patronen voor het gebruik van geheugen geheugen consuption verkleinen.


### <a name="burst-of-traffic"></a>Burst verkeer

#### <a name="problem"></a>Probleem

Bursts verkeer gecombineerd met slecht `ThreadPool` instellingen kunnen resulteren in vertragingen in gegevensverwerking al is verzonden door de Server bestand Vgx. maar nog niet aan de clientzijde is verbruikt.

#### <a name="measurement"></a>Maateenheden 

Monitor hoe uw `ThreadPool` statistieken stadium met code [als volgt](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)worden gewijzigd. U kunt ook zoeken op de `TimeoutException` bericht van StackExchange.Redis. Hier volgt een voorbeeld:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Klik in het bovenstaande bericht zijn er verschillende problemen die interessant zijn:

 1. U ziet dat in de `IOCP` sectie en de `WORKER` sectie die u hebt een `Busy` waarde die groter is dan de `Min` waarde. Dit betekent dat uw `ThreadPool` instellingen moeten aanpassen.
 2. U kunt ook zien `in: 64221`. Hiermee wordt aangeduid dat 64211 bytes bij de kernel socket layer zijn ontvangen, maar nog niet zijn gelezen door de toepassing (bijvoorbeeld StackExchange.Redis). Dit betekent meestal dat de toepassing niet wordt het lezen van gegevens uit het netwerk zo snel de server is naar u te verzenden.

#### <a name="resolution"></a>Resolutie

Uw [Instellingen voor ThreadPool](https://gist.github.com/JonCole/e65411214030f0d823cb) om ervoor te zorgen dat uw thread-groep wordt schalen snel onder burst scenario's configureren.


### <a name="high-client-cpu-usage"></a>CPU-gebruik van hoge client

#### <a name="problem"></a>Probleem

Hoog CPU-gebruik op de client wordt aangegeven dat het systeem niet kan bijhouden met het werk dat deze is gevraagd om uit te voeren. Dit betekent dat de client mislukken kan verwerkingstijd van een antwoord van het bestand Vgx. tijdig, hoewel het antwoord zeer snel bestand Vgx. verzonden.

#### <a name="measurement"></a>Maateenheden

Het systeem breed CPU-gebruik via de Portal Azure of via de teller gekoppeld prestaties bewaken. Zorg ervoor dat u niet bewaken *proces* CPU omdat één proces lage CPU-gebruik kan hebben op hetzelfde moment die algehele systeemprestaties CPU hoog kan zijn. Bekijk pieken in CPU-gebruik die met time-out overeenkomen. Grond hoog CPU, ziet u mogelijk ook hoog `in: XXX` -waarden in `TimeoutException` foutberichten zoals is beschreven in de sectie [Burst verkeer](#burst-of-traffic) .

>[AZURE.NOTE] StackExchange.Redis 1.1.603 en later bevat de `local-cpu` metrische in `TimeoutException` foutberichten. Controleer of u met de nieuwste versie van het [StackExchange.Redis NuGet-pakket](https://www.nuget.org/packages/StackExchange.Redis/). Zijn er bugs bij te werken voortdurend wordt opgelost in de code om deze krachtiger te maken Time-out dus met de meest recente versie belangrijk is.

#### <a name="resolution"></a>Resolutie

Upgrade uitvoeren naar VM groter met meer CPU-capaciteit of onderzoeken wat CPU pieken veroorzaakt. 



### <a name="client-side-bandwidth-exceeded"></a>Client kant bandbreedte overschreden

#### <a name="problem"></a>Probleem

Verschillende grote clientcomputers beperkingen hebben op hoeveel netwerkbandbreedte ze beschikbaar hebben. Als de client groter is dan de beschikbare bandbreedte, worden gegevens niet verwerkt aan de clientzijde zo snel de server is verzend deze. Dit kan leiden tot time-out.

#### <a name="measurement"></a>Maateenheden

Controleren hoe uw bandbreedtegebruik stadium met code [als volgt](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)worden gewijzigd. Houd er rekening mee dat deze code mogelijk niet juist uitgevoerd in sommige omgevingen met beperkte machtigingen (zoals Azure websites).

#### <a name="resolution"></a>Resolutie 

Client VM vergroten of verkleinen netwerkbandbreedte.


### <a name="large-requestresponse-size"></a>Grote aanvraag en respons grootte

#### <a name="problem"></a>Probleem

Een grote aanvraag en respons kunnen leiden tot time-out. Stel de time-outwaarde op de client geconfigureerd is 1 seconde als voorbeeld. Uw toepassing vraagt twee sleutels (bijvoorbeeld "A" en "B") op hetzelfde moment (via de dezelfde fysieke netwerkverbinding). De meeste clients ondersteunen "Pipelining" van aanvragen, dat beide aanvragen "A" en "B" worden verzonden op de kabel op de server een na de andere zonder te wachten op de antwoorden. De server stuurt de antwoorden weer in dezelfde volgorde. Als reactie "A" groot kunt is het meeste van de time-out voor volgende aanvragen eten. 

Het volgende voorbeeld wordt dit scenario. In dit scenario verzoek "A" en "B" snel worden verzonden, de server wordt gestart met het verzenden van antwoorden naar "A" en "B" snel, maar vanwege gegevens doorverbinden tijden, "B" vastloopt achter de andere aanvraag en time-out Hoewel snel door de server heeft gereageerd.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Maateenheden

Dit is een moeilijk meten. U moet in principe instrument uw clientcode om bij te houden grote vergaderverzoeken en antwoorden. 

#### <a name="resolution"></a>Resolutie

1.  Bestand Vgx. is geoptimaliseerd voor een groot aantal kleine waarden, in plaats van een paar grote waarden bevatten. De beste oplossing is om uw gegevens in de gerelateerde kleinere waarden. Zie de [, wat het ideale grootte van het waardebereik voor het bestand Vgx. is? 100KB te groot is?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) voor meer informatie rond waarom kleinere waarden worden aanbevolen posten.
2.  De grootte van uw VM (voor de client en Server van Cache bestand Vgx.) als u hoger bandbreedte-functies, gegevens doorverbinden tijden voor grotere antwoorden verkleinen vergroten. Houd er rekening mee dat aan meer bandbreedte op alleen de server of alleen op de client mogelijk niet genoeg. Uw bandbreedtegebruik meten en vergelijk deze met de mogelijkheden van de grootte van VM die u momenteel hebt.
3.  Het aantal vergroten `ConnectionMultiplexer` objecten u round robin aanvragen en gebruiken op verschillende verbindingen.


### <a name="what-happened-to-my-data-in-redis"></a>Wat is er gebeurd met mijn gegevens in het bestand Vgx.?

#### <a name="problem"></a>Probleem

Ik verwacht voor bepaalde gegevens wilt invoegen in mijn exemplaar Azure bestand Vgx. Cache maar deze niet lijken te zijn ingevuld.

##### <a name="resolution"></a>Resolutie

Zie [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) voor mogelijke oorzaken en oplossingen.


## <a name="server-side-troubleshooting"></a>Server probleemoplossing

In deze sectie wordt beschreven hoe het oplossen van problemen die vanwege een voorwaarde in de cacheserver optreden.

-   [Geheugendruk op de server](#memory-pressure-on-the-server)
-   [Hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load)
-   [De bandbreedte van de kant server overschreden](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Geheugendruk op de server

#### <a name="problem"></a>Probleem

Geheugendruk op de server leidt naar alle soorten prestatieproblemen die verwerking van aanvragen mag vertragen. Wanneer het geheugendruk raakt, heeft het systeem meestal aan de paginagegevens van fysieke geheugen virtueel geheugen, dat wil op schijf zeggen. Deze *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen. Er zijn verschillende mogelijke oorzaken van deze geheugendruk: 

1.  U hebt de cache volledige capaciteit gevuld met gegevens. 
2.  Bestand Vgx. ziet hoog geheugenfragmentatie - meestal als gevolg van het opslaan van grote objecten (bestand Vgx. is geoptimaliseerd voor een kleine objecten - de [Zie Wat is het ideale grootte van het waardebereik voor het bestand Vgx.? 100KB te groot is?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) bericht voor meer informatie). 

#### <a name="measurement"></a>Maateenheden

Bestand Vgx. beschrijft twee maatstaven die kunt u dit probleem te bepalen. De eerste is `used_memory` en de andere `used_memory_rss`. [Deze gegevens](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) zijn beschikbaar in de Portal Azure of via de opdracht [Bestand Vgx. INFO](http://redis.io/commands/info) .

#### <a name="resolution"></a>Resolutie

Er zijn verschillende mogelijke wijzigingen die u aanbrengen kunt om te zorgen dat er geheugengebruik orde:

1. [Een beleid geheugen configureren](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) en tijden verloopbeleid instellen voor uw sleutels. Houd er rekening mee dat dit niet voldoende zijn mogelijk als er fragmentatie.
2. [Een waarde maxmemory gereserveerd configureren](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) die groot genoeg is ter voor geheugenfragmentatie.
3. Uw grote objecten in de cache in kleinere gerelateerde objecten opheffen.
4. [Schaal](cache-how-to-scale.md) naar een grotere cachegrootte.
5. Als u een [premium-cache met bestand Vgx. cluster ingeschakeld](cache-how-to-premium-clustering.md) gebruikt, kunt u [het aantal shards vergroten](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hoog CPU-gebruik / Server laden

#### <a name="problem"></a>Probleem

Hoog CPU-gebruik kan betekenen dat de client mislukken kan verwerkingstijd van een antwoord van het bestand Vgx. tijdig, hoewel het antwoord zeer snel bestand Vgx. verzonden.

#### <a name="measurement"></a>Maateenheden

Het systeem breed CPU-gebruik via de Portal Azure of via de teller gekoppeld prestaties bewaken. Zorg ervoor dat u niet bewaken *proces* CPU omdat één proces lage CPU-gebruik kan hebben op hetzelfde moment die algehele systeemprestaties CPU hoog kan zijn. Bekijk pieken in CPU-gebruik die met time-out overeenkomen.

#### <a name="resolution"></a>Resolutie

[Schaal](cache-how-to-scale.md) naar een grotere cache trapsgewijs met meer CPU-capaciteit of onderzoeken wat CPU pieken veroorzaakt. 

### <a name="server-side-bandwidth-exceeded"></a>De bandbreedte van de kant server overschreden

#### <a name="problem"></a>Probleem

Exemplaren van verschillende grootte cache beperkingen hebben op hoeveel netwerkbandbreedte ze beschikbaar hebben. Als de server groter is dan de beschikbare bandbreedte, wordt klikt u vervolgens gegevens niet verzonden naar de klant zo snel. Dit kan leiden tot time-out.

#### <a name="measurement"></a>Maateenheden

U kunt controleren het `Cache Read` metrisch, dat wil de hoeveelheid gegevens zeggen uit de cache in Megabytes per seconde (MB/s) tijdens het opgegeven rapportage interval gelezen. Deze waarde overeenkomt met de netwerkbandbreedte die worden gebruikt door deze cache. Als u instellen van waarschuwingen voor server kant netwerk bandbreedte limieten wilt, kunt u ze met deze `Cache Read` item. Vergelijk uw metingen met de waarden in [deze tabel](cache-faq.md#cache-performance) van de limiet waargenomen bandbreedte voor verschillende cache lagen en de puntgrootten prijzen.

#### <a name="resolution"></a>Resolutie

Als u consistente dicht bij de waargenomen maximale bandbreedte van uw prijzen laag en cache grootte bent, kunt u [schaalbaarheid](cache-how-to-scale.md) naar een prijzen laag of -grootte groter netwerkbandbreedte, waarbij de waarden in [deze tabel](cache-faq.md#cache-performance) als een hulplijn met.


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis time-out uitzonderingen

StackExchange.Redis gebruikmaakt van een configuratie-instelling benoemde `synctimeout` voor synchrone bewerkingen waarvoor een standaardwaarde van 1000 ms. Als een synchrone oproep niet voltooid in de aangegeven tijd, genereert de StackExchange.Redis-client een time-out die vergelijkbaar is met het volgende voorbeeld.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Dit foutbericht wordt weergegeven, bevat de doelstellingen waarmee kunt u verwijzen naar de oorzaak en mogelijke resolutie van het probleem. De volgende tabel bevat informatie over de doelstellingen van de fout bericht.

| Error-meetwaarde van bericht | Meer informatie                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst       | Klik in het laatste tijdsegment dat: 0-opdrachten zijn verleend.                                                                                                                                                                                              |
| Mgr        | De uitvoering van de manager socket `socket.select` hetgeen betekent dat wordt gevraagd het besturingssysteem om aan te geven van een socket met iets te maken; in principe: de lezer is niet actief lezen van het netwerk omdat deze niet denkt er niets dat te maken |
| wachtrij      | Er zijn 73 totale in uitvoering bewerkingen                                                                                                                                                                                                        |
| Qu         | 6 van de bewerkingen in uitvoering zijn in de niet-verzonden wachtrij en nog niet zijn geschreven met de uitgaande netwerk                                                                                                                                                           |
| Qs         | 67 van he in uitvoering bewerkingen zijn verzonden naar de server, maar een antwoord is nog niet beschikbaar. Het antwoord kan worden `Not yet sent by the server` of`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 van de bewerkingen in uitvoering antwoorden hebt gezien, maar nog niet zijn gemarkeerd als voltooid vanwege wachten op de voltooiing lus                                                                                                                                      |
| wR         | Er is een actieve schrijver (wat betekent dat de 6 niet-verzonden aanvragen worden niet genegeerd) bytes/activewriters                                                                                                                                                   |
| in         | Er zijn geen actieve lezers en nul bytes kunnen worden gelezen op het NIC bytes/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Stappen om te kunnen onderzoeken

1. Als u een goede gewoonte Zorg ervoor dat u het volgende patroon verbinding maakt via bij gebruik van de client StackExchange.Redis.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Zie [verbinding maken met de cache met StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)voor meer informatie.

2. Zorg ervoor dat uw Azure bestand Vgx. Cache en de clienttoepassing zich in dezelfde regio in Azure wordt aangegeven bevinden. Zoals u mogelijk worden er time-out wanneer de cache is in Oost-VS, maar de client in West VS en de aanvraag niet voltooid binnen de `synctimeout` interval of als u mogelijk worden er time-out wanneer u fouten van uw computer plaatselijke ontwikkeling opsporen wilt. 

    Het is raadzaam om de cache en in de client in dezelfde Azure regio. Als er een scenario waarbij cross regio oproepen bevat, moet u set de `synctimeout` interval naar een waarde die hoger zijn dan het interval standaard 1000 ms door een `synctimeout` eigenschap in de verbindingstekenreeks. Het volgende voorbeeld ziet u een StackExchange.Redis cache verbinding tekenreeks fragment met een `synctimeout` van 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Controleer of u met de nieuwste versie van het [StackExchange.Redis NuGet-pakket](https://www.nuget.org/packages/StackExchange.Redis/). Zijn er bugs bij te werken voortdurend wordt opgelost in de code om deze krachtiger te maken Time-out dus met de meest recente versie belangrijk is.

4. Als er aanvragen die zijn aan de gekoppeld door bandbreedtebeperkingen op de server of -client, is het duurt langer om aan te voltooien en daarmee ook ertoe leiden dat time-out. Als uw time-out vanwege netwerkbandbreedte op de server is, raadpleegt u [de bandbreedte van de kant Server overschreden](#server-side-bandwidth-exceeded). Als uw time-out client netwerkbandbreedte oplevert, raadpleegt u [Client kant bandbreedte overschreden](#client-side-bandwidth-exceeded).

6. U Processorsnelheid aan of zijn gekoppeld op de server op de client?
    -   Controleren als u aan de gebonden zijn door CPU op de client waardoor het verzoek niet worden verwerkt binnen de `synctimeout` interval, hetgeen een time-out. Verplaatsen naar de client groter of distributie van het selectievakje laden kan helpen dit instellen. 
    -   Controleren of u op het punt Processorsnelheid afhankelijke waarden op de server door controleren of de `CPU` [cache prestaties metrisch](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Aanvragen binnenkort terwijl bestand Vgx. CPU afhankelijke waarden is, kunnen leiden tot deze aanvragen voor time-out. Dit kunt u het selectievakje laden verdelen over meerdere shards in een premium-cache of upgrade uitvoeren naar een grotere tekengrootte of de prijzen laag. Zie de [Server kant bandbreedte overschreden](#server-side-bandwidth-exceeded)voor meer informatie.

7. Zijn er opdrachten lange tijd in beslag neemt op de server worden verwerkt? Langdurige op opdrachten die zijn het duurt lang op het bestand Vgx. server worden verwerkt kan leiden tot time-out. Voorbeelden van langdurige opdrachten zijn `mget` met grote aantallen toetsaanslagen `keys *` of slecht geschreven lua scripts. U kunt verbinding maken met uw Azure bestand Vgx. Cache-exemplaar van het bestand Vgx. cli-client of gebruikt u het [Bestand Vgx. Console](cache-configure.md#redis-console) en voert u de opdracht [SlowLog](http://redis.io/commands/slowlog) om te kijken of er aanvragen duurt langer dan verwacht. Bestand Vgx. Server en StackExchange.Redis zijn geoptimaliseerd voor veel kleine aanvragen in plaats van minder grote aanvragen. Uw gegevens splitsen in kleinere stukken mogelijk dingen hier verbeteren. 

    Zie het blogbericht [Aankondigen ASP.NET sessie staat Provider voor de Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) voor informatie over het verbinding maken met het gebruik van bestand Vgx. cli en stunnel Azure bestand Vgx. Cache SSL-eindpunt. Zie [SlowLog](http://redis.io/commands/slowlog)voor meer informatie.

8. Hoge belasting van bestand Vgx. server kan leiden tot time-out. U kunt de belasting van de server controleren door controleren of de `Redis Server Load` [cache prestaties metrisch](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Een serverbelasting van 100 (maximumwaarde) duidt aan dat het bestand Vgx. server is bezet, met geen niet-actieve tijd verwerken van aanvragen. Als u wilt zien als bepaalde verzoeken van alle de mogelijkheid van de server duurt, voert u de opdracht SlowLog, zoals wordt beschreven in de vorige alinea. Zie voor meer informatie [hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load).

9. Is er een willekeurige andere gebeurtenis aan de clientzijde die kan hebben gehad dat een blip netwerk? Controleren op de client (web, werknemer rol of een Iaas VM) als er een gebeurtenis is zoals het aantal exemplaren van de client schaalbaarheid omhoog of omlaag of een nieuwe versie van de client of automatisch schalen implementeert is ingeschakeld? Uitgaande netwerkconnectiviteit kan in onze testen die hebt gevonden dat automatisch schalen of omhoog/omlaag-schaalbaarheid kan leiden tot verloren zijn enkele seconden. StackExchange.Redis code is robuuste op gebeurtenissen en wordt opnieuw verbinding maakt. Tijdens deze periode opnieuw verbinding kunnen verzoeken in de wachtrij time-out.

10. Is er een verzoek voor een groot voorafgaand aan de verschillende kleine aanvragen aan het bestand Vgx. Cache waarvoor een time-out? De parameter `qs` in de fout bericht laat weten hoeveel aanvragen van de client naar de server zijn verzonden, maar een reactie nog niet hebt verwerkt. Deze waarde kunt houden groeiende omdat StackExchange.Redis gebruikmaakt van een enkele TCP-verbinding en één antwoord kunnen alleen worden gelezen tegelijk. Hoewel de eerste bewerking timed-out, stopt niet de gegevens die worden verzonden naar/vanaf de server en andere aanvragen worden geblokkeerd totdat dit is voltooid, de oorzaak van het tijd alles. Een mogelijke oplossing is de kans op time-out minimaliseren door om te garanderen dat de cache groot genoeg is voor uw werkbelasting en grote waarden splitsen in kleinere stukken. Een andere mogelijke oplossing is het gebruik van een groep `ConnectionMultiplexer` objecten in uw client en kies de minste geladen `ConnectionMultiplexer` bij het verzenden van een nieuwe vergaderverzoek. Dit moet voorkomen dat een time-out voor één veroorzaakt door andere aanvragen voor ook time-out.

11. Als u werkt met `RedisSessionStateprovider`, hebt u de time-out opnieuw correct ingesteld. `retrytimeoutInMilliseconds`hoger dan `operationTimeoutinMilliseonds`, anders kan geen nieuwe pogingen zal plaatsvinden. In het volgende voorbeeld `retrytimeoutInMilliseconds` is ingesteld op 3000. Zie [ASP.NET sessie staat Provider voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md) en [het gebruik van de configuratieparameters van sessie staat Provider en uitvoer Cache-Provider](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration)voor meer informatie.


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Geheugengebruik op de server Azure bestand Vgx. Cache controleren door te [controleren](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` en `Used Memory`. Als een beleid verwijdering op hun plaats staan, bestand Vgx. wordt gestart wanneer verwijderen pijltoetsen `Used_Memory` de cachegrootte bereikt. In het ideale geval `Used Memory RSS` moeten worden alleen iets hoger dan `Used memory`. Een grote verschil betekent dat er geheugenfragmentatie (intern of extern. Wanneer `Used Memory RSS` is minder dan `Used Memory`, betekent dit deel van het cachegeheugen heeft zijn vervangen door het besturingssysteem. Als dit gebeurt, kunt u sommige aanzienlijk vertragingstijden verwachten. Omdat het bestand Vgx. heeft geen controle over hoe de toewijzingen zijn toegewezen aan geheugen pagina's, hoog `Used Memory RSS` vaak het resultaat is van een Prikker in geheugengebruik. Wanneer het bestand Vgx. wordt geheugen vrijgemaakt, het geheugen wordt weer gegeven aan de allocator en de allocator mogelijk of mogelijk niet de geven het geheugen terug aan het systeem. Er zijn mogelijk een verschil is tussen de `Used Memory` waarde en geheugen verbruik van het besturingssysteem. Is het mogelijk vanwege de faculteit geheugen is gebruikt en vrijgegeven door het bestand Vgx., maar niet de opgegeven terug naar het systeem. U kunt de volgende stappen uitvoeren om te voorkomen dat geheugenproblemen.
    -   De cache bijwerken naar groter, zodat u niet zich verhouden tot de geheugenbeperkingen bij het worden uitgevoerd op het systeem.
    -   Instellen op de toetsen verlooptijd tijden zodat oudere waarden proactief zijn verwijderd.
    -   Monitor de de `used_memory_rss` metrisch in cache. Wanneer deze waarde de grootte van de cache nadert, bent u waarschijnlijk prestatieproblemen zien. Verdeel de gegevens over meerdere shards als u een premium-cache, of upgraden naar een grotere cachegrootte.

    Zie [Geheugendruk op de server](#memory-pressure-on-the-server)voor meer informatie.

## <a name="additional-information"></a>Aanvullende informatie

-   [Welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Hoe kan ik gebruikelijke en testen van de prestaties van mijn cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)
-   [Het controleren van de Cache van Azure bestand Vgx.](cache-how-to-monitor.md)