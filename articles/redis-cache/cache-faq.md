<properties 
    pageTitle="Azure bestand Vgx. Cache Veelgestelde vragen over | Microsoft Azure" 
    description="Informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor de Cache van Azure bestand Vgx." 
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
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure bestand Vgx. Cache Veelgestelde vragen

Informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure bestand Vgx. Cache. 


## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?

Als uw vraag hier niet wordt weergegeven, laat het ons weten, zodat we u een antwoord te vinden.

-   U kunt stelt u een vraag in de [Disqus thread](#comments) aan het einde van deze Veelgestelde vragen en oefenen met het team van Azure Cache en andere communityleden over in dit artikel.
-   Als u wilt een grotere doelgroep bereiken, kunt u een vraag op het [Azure Cache MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) boeken en oefenen met het team van Azure Cache en andere leden van de community.
-   Als u maken van een verzoek voor een functie wilt, kunt u uw aanvragen en ideeën op [Azure bestand Vgx. Cache gebruiker stem](https://feedback.azure.com/forums/169382-cache)verzenden.
-   U kunt ook een e-mailbericht verzenden met ons op de [Externe Feedback van Azure Cache](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Basisbeginselen van Azure Cache bestand Vgx.

De veelgestelde vragen in deze sectie duidelijk enkele van de basisprincipes van Azure bestand Vgx. Cache.

-    [Wat is Azure bestand Vgx. Cache?](#what-is-azure-redis-cache)
-    [Hoe kan ik aan de slag met Azure bestand Vgx. Cache?](#how-can-i-get-started-with-azure-redis-cache)

Veelgestelde vragen over het volgende begeleidende basisbegrippen en vragen over Azure bestand Vgx. Cache en in een van de andere veelgestelde vragen over secties worden beantwoord.

-   [Welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
-   [Welke cache bestand Vgx. clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
-   [Is er een lokale emulator voor Azure bestand Vgx. Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hoe ik de gezondheid en de prestaties van mijn cache controleren?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Veelgestelde vragen over planning

-   [Welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
-   [Prestaties van Azure Cache bestand Vgx.](#azure-redis-cache-performance)
-   [In welke regio moet ik mijn cache vinden?](#in-what-region-should-i-locate-my-cache)
-   [Hoe ben ik gefactureerd voor Azure bestand Vgx. Cache?](#how-am-i-billed-for-azure-redis-cache)
-   [Kan ik Azure bestand Vgx. Cache met Azure overheid Cloud of Azure China Cloud gebruiken?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Ontwikkeling Veelgestelde vragen

-   [Wat moeten u met de configuratieopties StackExchange.Redis doen?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Welke cache bestand Vgx. clients kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
-   [Is er een lokale emulator voor Azure bestand Vgx. Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hoe kan ik bestand Vgx. opdrachten uitvoeren?](#how-can-i-run-redis-commands)
-   [Waarom geen Azure bestand Vgx. Cache een verwijzing MSDN class bibliotheek zoals enkele van de andere Azure services?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Kan ik Azure bestand Vgx. Cache als PHP sessiecache gebruiken?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Veelgestelde vragen over Security

-   [Wanneer moet ik de niet-SSL-poort voor de verbinding met het bestand Vgx. inschakelen?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Productie Veelgestelde vragen

-   [Wat zijn enkele aanbevolen procedures voor productie?](#what-are-some-production-best-practices)
-   [Wat zijn enkele van de overwegingen wanneer u een veelgebruikte opdrachten bestand Vgx. gebruikt?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Hoe kan ik gebruikelijke en testen van de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Belangrijke informatie over ThreadPool groei](#important-details-about-threadpool-growth)
-   [Server globale Catalogus om meer doorvoer op de client bij gebruik van StackExchange.Redis inschakelen](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Cmdlets voor controle en probleemoplossing Veelgestelde vragen

De veelgestelde vragen in deze sectie duidelijk algemene controle en probleemoplossing vragen. Zie voor meer informatie over cmdlets voor controle en probleemoplossing bij uw Azure bestand Vgx. Cache-sessies, [het controleren van Azure bestand Vgx. Cache](cache-how-to-monitor.md) en [het oplossen van Azure bestand Vgx. Cache](cache-how-to-troubleshoot.md).

-   [Hoe ik de gezondheid en de prestaties van mijn cache controleren?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Mijn cache diagnostisch hulpprogramma opslag Accountinstellingen gewijzigd, wat is er gebeurd?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Waarom is de diagnostische hulpprogramma's voor enkele nieuwe cache, maar andere niet ingeschakeld?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Waarom zie ik time-out?](#why-am-i-seeing-timeouts)
-   [Waarom is mijn client verbroken uit de cache?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Voorafgaande Cache aanbod Veelgestelde vragen

-   [Welke Azure Cache aanbod is geschikt voor mij?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Wat is Azure bestand Vgx. Cache?

Azure bestand Vgx. Cache is gebaseerd op de populaire open-source [bestand Vgx. cache](http://redis.io). Dit hebt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft en toegankelijk zijn vanuit een toepassing binnen Azure. Zie de productpagina [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/) op Azure.com voor een gedetailleerd overzicht.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Hoe kan ik aan de slag met Azure bestand Vgx. Cache?

Er zijn verschillende manieren waarop die u kunt aan de slag met Azure bestand Vgx. Cache.

-    U kunt een van onze zelfstudies beschikbaar raadplegen voor [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)en [Python](cache-python-get-started.md). 
-    U kunt bekijken [hoe u hoge prestaties Apps gebruik van Microsoft Azure bestand Vgx. Cache opbouwen](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    U kunt de clientdocumentatie voor de clients die overeenkomen met de taal van de ontwikkeling van uw project om te zien hoe u het bestand Vgx. raadplegen. Zijn er veel bestand Vgx. clients die kunnen worden gebruikt met Azure bestand Vgx. Cache. Zie voor een lijst met klanten bestand Vgx., [http://redis.io/clients](http://redis.io/clients).


Als u niet al een Azure-account hebt, kunt u het volgende doen:

-    [Gratis een Azure-account opent](/pricing/free-trial/?WT.mc_id=redis_cache_hero). U krijgt tegoeden die kunnen worden gebruikt om te proberen betaalde Azure Services. Zelfs nadat de tegoeden zijn gebruikt, kunt u deze kunt houden van het account en gebruiken van gratis Azure services en functies.
-    [Voordelen van de abonnee Visual Studio activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Uw MSDN-abonnement kunt u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?
Elke Azure bestand Vgx. Cache biedt verschillende niveaus van **grootte**, **bandbreedte**, **beschikbaarheid**en **SLA** opties.

Hier volgen aandachtspunten bij het kiezen van een aanbod Cache.

-   **Geheugen**: de eenvoudige en standaard lagen bieden 250 MB – 53 GB. De Premium-laag biedt tot 530 GB met meer beschikbaar [op verzoek](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Zie [Azure bestand Vgx. Cache prijzen](https://azure.microsoft.com/pricing/details/cache/)voor meer informatie.
-   **Netwerkprestaties**: als er een werkbelasting die moeten worden hoge doorvoer de laag Premium biedt meer bandbreedte vergeleken met standaard of Basic. Binnen elke laag hebt grotere formaten cache ook meer bandbreedte vanwege de onderliggende VM waarop de cache. Raadpleeg de [volgende tabel](#cache-performance) voor meer informatie.
-   **Doorvoer**: de Premium-laag biedt de maximumdoorvoer beschikbaar. Als de cacheserver of -client de limieten bandbreedte bereikt, ontvangt u time-out aan de clientzijde. Zie de volgende tabel voor meer informatie.
-   **Hoge beschikbaarheid/SLA**: Azure bestand Vgx. Cache zorgt ervoor dat de cache van een standaard/Premium beschikbaar ten minste 99,9% van de tijd zijn zal. Zie meer informatie over onze SLA, [Prijzen van Azure bestand Vgx. Cache](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). De SLA behandelt alleen connectiviteit met de Cache-eindpunten. De SLA besproken bescherming tegen verlies van gegevens niet. We raden u aan de functie gegevens permanente bestand Vgx. in de Premium-laag gebruiken om uit te breiden tolerantie tegen verlies van gegevens.
-   **Bestand Vgx. gegevens permanente**: de Premium-laag kunt u de cache-gegevens in een opslag van Azure-account. In de cache van een Basic/standaard alle gegevens alleen in het geheugen opgeslagen. Onderliggende infrastructuur voor het geval zijn er problemen potentieel gegevensverlies. We raden u aan de functie gegevens permanente bestand Vgx. in de Premium-laag gebruiken om uit te breiden tolerantie tegen verlies van gegevens. Azure bestand Vgx. Cache biedt RDB en AOF (binnenkort) opties voor permanente bestand Vgx.. Lees [hoe u het permanente voor een Premium Azure bestand Vgx. Cache-instellingen configureren](cache-how-to-premium-persistence.md)voor meer informatie.
-   **Bestand Vgx. Cluster**: maken in de cache opgeslagen groter zijn dan 53 GB of shard gegevens op verschillende bestand Vgx. knooppunten, kunt u bestand Vgx. cluster, die beschikbaar is in de Premium-laag. Elk knooppunt bestaat uit twee primaire/replica cache voor maximale beschikbaarheid. Zie [hoe u configureert voor een Premium Azure bestand Vgx. Cache cluster](cache-how-to-premium-clustering.md)voor meer informatie.
-   **Verbeterde beveiliging en netwerk moeten worden geïsoleerd**: implementatie van Azure virtuele netwerk (VNET) biedt verbeterde beveiliging en moeten worden geïsoleerd voor uw Azure bestand Vgx. Cache, evenals de subnetten, clienttoegangsbeleid besturingselement, en andere functies verder te beperken van toegang. Lees [hoe u het virtuele netwerkondersteuning voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-vnet.md)voor meer informatie.
-   **Bestand Vgx configureren.**: In de standaard- en Premium lagen, kunt u configureren bestand Vgx. voor Keyspace meldingen.
-   **Maximumaantal verbindingen van clients**: de Premium-laag biedt het maximum aantal clients die kunnen worden verbonden met het bestand Vgx., met een hoger aantal verbindingen voor grotere grote cache. [Neem verwijzen naar de pagina prijzen voor meer informatie](https://azure.microsoft.com/pricing/details/cache/).
-   **Speciale Core voor Server bestand Vgx.**: In de Premium-laag alle cachegrootte een speciale core hebt voor het bestand Vgx.. In de Basic/standaard lagen de C1 de grootte en hoger hebben een speciale core voor bestand Vgx. server.
-   **Single thread-bestand Vgx. is** dus met meer dan twee cores biedt geen extra voordeel via met slechts twee cores, maar grotere VM hebben meestal meer bandbreedte dan kleinere. Als de cacheserver of -client de limieten bandbreedte bereikt, ontvangt u time-out aan de clientzijde.
-   **Prestatieverbeteringen**: cache in de Premium-laag worden geïmplementeerd op hardware waarvoor sneller processors en biedt betere prestaties in vergelijking met de laag Basic of standaard. Premium laag cache hebben sneller worden verwerkt en lagere vertragingstijden.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Prestaties van Azure Cache bestand Vgx.

De volgende tabel ziet u de maximale bandbreedte waarden bij het testen van verschillende grootte van standaard en Premium in de cache opgeslagen met `redis-benchmark.exe` uit een VM Iaas ten opzichte van het eindpunt Azure bestand Vgx. Cache. Houd er rekening mee dat deze waarden zijn niet gegarandeerd en er is geen SLA voor deze getallen, maar moet typische. U moet worden geladen testen van uw eigen toepassing om te bepalen de juiste cachegrootte voor uw toepassing.

In deze tabel kunt we de volgende conclusies tekenen.

-   Doorvoer voor de cache die even groot zijn is hoger in de laag Premium ten opzichte van de standaard laag. Met een 6 GB Cache, bijvoorbeeld is doorvoer van P1 140K RPS ten opzichte van 49 K voor C3.
-   Met het bestand Vgx. cluster, doorvoer Hiermee verhoogt u Lineair het aantal shards (knooppunten) in het cluster te vergroten. Als u een P4 cluster van 10 shards maakt, klikt u vervolgens de beschikbare doorvoer is bijvoorbeeld 250K * 10 = 2,5 miljoen RPS.
-   Doorvoer voor groter te maken van belangrijke grootte is hoger in de laag Premium ten opzichte van de standaard laag.

| Prijzen van laag             | Grootte   | CPU-kernen | Beschikbare bandbreedte                                    | 1 KB sleutelgrootte                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standaard de cachegrootte** |        |           | **Megabits per seconde (Mb/s) / MB per seconde (MB/s)** | **Aanvragen per seconde (RPS)**            |
| C0                       | 250 MB | Gedeeld    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **De cachegrootte Premium**  |        | **CPU-kernen per shard**  |                                         | **Aanvragen per seconde (RPS), per shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Voor instructies over het downloaden van de hulpmiddelen voor het bestand Vgx. zoals `redis-benchmark.exe`, raadpleegt u de [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>In welke regio moet ik mijn cache vinden?

Zoek de Cache Azure bestand Vgx. in hetzelfde gebied, als de cache-clienttoepassing voor de beste prestaties en laagste latentie.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Hoe ben ik gefactureerd voor Azure bestand Vgx. Cache?

Azure bestand Vgx. Cache prijzen is [hier](https://azure.microsoft.com/pricing/details/cache/). De prijzen pagina vermeldt prijzen als uur. Cache zijn gefactureerd op basis van per minuut vanaf het moment dat de cache wordt gemaakt totdat de tijd die een cache wordt verwijderd. Er is geen optie voor het stoppen of onderbreken de facturering van een cache.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Kan ik Azure bestand Vgx. Cache met Azure overheid Cloud of Azure China Cloud gebruiken?

Ja, is Azure bestand Vgx. Cache beschikbaar in zowel Azure overheid Cloud en Azure China Cloud. Houd er rekening mee dat de URL's voor het openen en beheren van Azure bestand Vgx. Cache verschillend in Azure overheid Cloud zijn en Azure China Cloud vergeleken met Azure openbare Cloud. Zie voor meer informatie over overwegingen bij het gebruik van Azure bestand Vgx. Cache met Azure overheid Cloud en Azure China Cloud, [Azure-Databases voor de overheid - Azure bestand Vgx. Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache) en [Azure China Cloud - Azure bestand Vgx. Cache](https://www.azure.cn/documentation/services/redis-cache/).

Zie [hoe u verbinding maken met Azure overheid Cloud of Azure China Cloud](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)voor informatie over het gebruik van Azure bestand Vgx. Cache met PowerShell in Azure overheid Cloud en Azure China Cloud.


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Wat moeten u met de configuratieopties StackExchange.Redis doen?

StackExchange.Redis bevat veel opties. In dit gedeelte moment spreekt over enkele van de algemene instellingen. Zie voor meer gedetailleerde informatie over de opties voor StackExchange.Redis [StackExchange.Redis configuratie](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Beschrijving|Aanbeveling
---|---|---
AbortOnConnectFail|Wanneer wordt ingesteld op waar, de verbinding niet opnieuw na een netwerk is mislukt.|Ingesteld op onwaar en laat StackExchange.Redis automatisch opnieuw verbinding te maken.
ConnectRetry|Het aantal keren herhaald verbindingspogingen tijdens aanvankelijke moet verbinding maken.| Zie de volgende opmerkingen voor instructies. |
ConnectTimeout|Time-out in ms voor verbinding maken met bewerkingen.| Zie de volgende opmerkingen voor instructies. |

In de meeste gevallen zijn de standaardwaarden van de client voldoende. U kunt de opties op basis van uw werkzaamheden verfijnen.

-   **Nieuwe pogingen**
    -   Snelle voor ConnectRetry en ConnectTimeout de algemene richtlijnen is mislukt en probeer het opnieuw. Dit is gebaseerd op de werkbelasting en hoeveel tijd op gemiddelde van deze wordt voor uw client en een opdracht bestand Vgx. en u ontvangt een antwoord.
    -   Laat StackExchange.Redis automatisch opnieuw verbinding maakt in plaats van de verbindingsstatus van de controleren en opnieuw verbinding maakt zelf. **Vermijd het gebruik van de eigenschap ConnectionMultiplexer.IsConnected**.
    -   Snowballing - soms u mogelijk problemen optreden waar u zijn opnieuw en dit snowballs en nooit herstelt. In dit geval moet u rekening houden met behulp van een exponentiële backoff opnieuw-algoritme zoals is beschreven in de [algemene richtlijnen opnieuw](best-practices-retry-general.md) uitgegeven door de groep Microsoft Patterns en procedures.
-   **Time-out voor waarden**
    -   Houd rekening met uw werkzaamheden en stel de waarden dienovereenkomstig gewijzigd. Als u grote waarden opslaat, stelt u de time-out voor op een hogere waarde.
    -   Stel `AbortOnConnectFail` naar onwaar en laat StackExchange.Redis opnieuw voor u.
    -   Gebruik een enkel exemplaar van de ConnectionMultiplexer voor de toepassing. U kunt een LazyConnection gebruiken om te maken van één exemplaar die wordt geretourneerd door een verbindingseigenschap, zoals wordt weergegeven in [verbinding maken met de cache met de klasse ConnectionMultiplexer](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Stel de `ConnectionMultiplexer.ClientName` eigenschap waarmee een unieke exemplaarnaam app voor diagnose.
    -   Gebruik van meerdere `ConnectionMultiplexer` exemplaren voor aangepaste werkbelasting.
    -   Als u verschillende laden in uw toepassing hebt, kunt u dit model volgen. Bijvoorbeeld:
    -   U kunt een multiplexer voor omgaan met grote sleutels hebben.
    -   U kunt een multiplexer voor omgaan met kleine sleutels hebben.
    -   U kunt verschillende waarden voor verbinding time-out instellen en probeer opnieuw logica voor elke ConnectionMultiplexer dat u gebruikt.
    -   Stel de `ClientName` eigenschap op elk multiplexer om u te helpen met diagnostische gegevens.
    -   Dit leidt naar meer gestroomlijnd latentie per `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Welke cache bestand Vgx. clients kan ik gebruiken?

Een van de geweldige voordelen van het bestand Vgx. is dat er veel clients veel verschillende ontwikkeling talen ondersteunt. Zie [bestand Vgx. clients](http://redis.io/clients)voor een huidige lijst met klanten. Lees [hoe u met Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) voor zelfstudies over verschillende talen en klanten, en klik op de gewenste taal in de taal-overschakeling aan de bovenkant van het artikel.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Is er een lokale emulator voor Azure bestand Vgx. Cache?

Er is geen lokale emulator voor Azure bestand Vgx. Cache, maar kunt u de MSOpenTech-versie van bestand Vgx. server.exe uit het [bestand Vgx. hulpmiddelen voor de opdrachtregel](https://github.com/MSOpenTech/redis/releases/) uitvoeren op uw lokale computer en verbinding maken met deze u geen vergelijkbare ervaringen bij een lokale cache-emulator, zoals wordt weergegeven in het volgende voorbeeld.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


U kunt desgewenst een bestand [redis.conf](http://redis.io/topics/config) nauwer zodat deze overeenkomen met de [standaardinstellingen voor de cache](cache-configure.md#default-redis-server-configuration) voor uw online Azure bestand Vgx. Cache desgewenst configureren.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Hoe kan ik bestand Vgx. opdrachten uitvoeren?

U kunt een van de opdrachten op het [bestand Vgx. opdrachten](http://redis.io/commands#) , behalve voor de opdrachten op het [bestand Vgx. opdrachten die niet worden ondersteund in Azure bestand Vgx. Cache](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). U hebt verschillende opties om het bestand Vgx. opdrachten uitvoeren.

-   Als u een standaard- of Premium cache hebt, kunt u met de [Console bestand Vgx.](cache-configure.md#redis-console)bestand Vgx.-opdrachten kunt uitvoeren. Dit biedt een veilige manier bestand Vgx. opdrachten kunt uitvoeren in de portal van Azure.
-   U kunt ook de opdrachtregel bestand Vgx. hulpmiddelen gebruiken. Als u wilt gebruiken, moet u de volgende stappen uitvoeren.
-   Download het [bestand Vgx. hulpmiddelen voor de opdrachtregel](https://github.com/MSOpenTech/redis/releases/).
-   Verbinding maken met behulp van de cache `redis-cli.exe`. In de cache eindpunt gebruikt die de -h schakelen en de sleutel volgens - een zoals wordt weergegeven in het volgende voorbeeld doorgeven.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Opmerking dat de opdrachtregel bestand Vgx. hulpmiddelen niet met de SSL-poort werken, maar u een hulpprogramma zoals gebruiken kunt `stunnel` veilig met de hulpmiddelen voor de SSL-poort door de aanwijzigen in het blogbericht [Aankondigen ASP.NET sessie staat Provider voor de Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Waarom geen Azure bestand Vgx. Cache een verwijzing MSDN class bibliotheek zoals enkele van de andere Azure services?

Microsoft Azure bestand Vgx. Cache is gebaseerd op de bron voor populaire openen bestand Vgx. Cache en kan worden gebruikt door een groot aantal [bestand Vgx. clients](http://redis.io/clients) die beschikbaar zijn voor veel talen. Elke client heeft een eigen API waarmee oproepen naar de cache bestand Vgx. instantie met [bestand Vgx. opdrachten](http://redis.io/commands).

Omdat elke client verschilt, is er geen één centrale class verwijzing op MSDN; elke client onderhoudt in plaats daarvan een eigen naslagmateriaal. Naast de documentatie verwijzing, zijn er verschillende zelfstudies laat zien hoe u aan de slag met Azure bestand Vgx. Cache met verschillende talen en cache-clients. Lees [hoe u met Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) voor toegang tot deze zelfstudies, en klik op de gewenste taal in de taal-overschakeling aan de bovenkant van het artikel.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kan ik Azure bestand Vgx. Cache als PHP sessiecache gebruiken?

Ja, als u wilt gebruiken Azure bestand Vgx. Cache als PHP sessiecache, geef de verbindingsreeks op uw exemplaar van de Azure bestand Vgx. Cache in `session.save_path`.

>[AZURE.IMPORTANT] Wanneer u met Azure bestand Vgx. Cache als cache sessie PHP, moet u de URL coderen van de beveiligingssleutel waarmee verbinding maken met de cache, zoals wordt weergegeven in het volgende voorbeeld.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Als de sleutel niet URL is-codering, ontvangt u mogelijk een uitzondering ongeveer als volgt uit:`Failed to parse session.save_path`

Zie voor meer informatie over het gebruik van de Cache bestand Vgx. als PHP sessiecache met de client PhpRedis [PHP sessie](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wanneer moet ik de niet-SSL-poort voor de verbinding met het bestand Vgx. inschakelen?

Bestand Vgx. server biedt geen ondersteuning voor SSL afmelden bij het vak, maar Azure bestand Vgx. Cache werkt. Als u verbinding met Azure bestand Vgx. Cache maakt en de klant SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken.

Houd er rekening mee dat de niet-SSL-poort al dan niet standaard voor nieuwe exemplaren van Azure bestand Vgx. Cache is uitgeschakeld. Als de klant geen ondersteuning biedt voor SSL, moet u de niet-SSL-poort inschakelen door de aanwijzigen in het gedeelte [-poorten](cache-configure.md#access-ports) van het artikel [een cache in Azure bestand Vgx. Cache configureren](cache-configure.md) .

Bestand Vgx. hulpprogramma's zoals `redis-cli` werken niet met de SSL-poort, maar u kunt een hulpprogramma gebruiken zoals `stunnel` veilig met de hulpmiddelen voor de SSL-poort door de aanwijzigen in het blogbericht [Aankondigen ASP.NET sessie staat Provider voor de Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Zie voor instructies over het downloaden van de hulpmiddelen voor het bestand Vgx. het [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.



### <a name="what-are-some-production-best-practices"></a>Wat zijn enkele aanbevolen procedures voor productie?

-   [Aanbevolen procedures voor StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Configuratie en concepten](#configuration-and-concepts)
-   [Prestaties testen](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Aanbevolen procedures voor StackExchange.Redis

-   Stel `AbortConnect` ONWAAR, laat de ConnectionMultiplexer automatisch opnieuw verbinding te maken. [Zie voor meer informatie](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Opnieuw gebruiken de ConnectionMultiplexer: Maak een nieuw account voor elk verzoek om geen. De `Lazy<ConnectionMultiplexer>` patroon [Hier wordt getoond](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wordt ten zeerste aanbevolen.
-   Works best bestand Vgx. waaruit kleinere waarden, moet deze hakken up groter te maken van gegevens in meerdere toetsen. In [Dit bestand Vgx. discussie](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb wordt beschouwd als "grote". Lees [dit artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) voor een voorbeeld-probleem dat kan worden veroorzaakt door grote waarden bevatten.
-   Uw [ThreadPool instellingen](#important-details-about-threadpool-growth) om te voorkomen dat time-out configureren.
-   Gebruik ten minste de standaard-connectTimeout van 5 seconden. Dit geeft StackExchange.Redis voldoende tijd om de verbinding, voor het geval in een netwerk blip opnieuw te maken.
-   Let op van de kosten van de prestaties van verschillende bewerkingen die u uitvoert. Bijvoorbeeld de `KEYS` is een bewerking O(n) voor opdrachten en parameters te voorkomen. De [redis.io site](http://redis.io/commands/) heeft details rond de complexiteit van de tijd voor elke bewerking die wordt ondersteund. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.

#### <a name="configuration-and-concepts"></a>Configuratie en concepten

-   Gebruik standaard of Premium laag voor systemen. De eenvoudige laag is een systeem één knooppunt met geen gegevensreplicatie en geen SLA. Gebruik bovendien ten minste een cache C1. C0 cache zijn eigenlijk bedoeld voor eenvoudige ontwikkelaar/testen scenario's.
-   Onthoud dat bestand Vgx. een gegevensopslag **In het geheugen** is. Lees [dit artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) zodat u zich bewust bent van scenario's waarin gegevens verloren gaan kunnen.
-   Ontwikkel uw systeem zodat deze kan omgaan met verbinding blips [vanwege patches en overname bij storing](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Prestaties testen

-   Beginnen met behulp van `redis-benchmark.exe` te leren hoe mogelijke worden verwerkt voordat het schrijven van uw eigen perf getest. Houd er rekening mee dat bestand Vgx. vergelijkende biedt geen ondersteuning voor SSL, zodat u [de niet-SSL-poort via de portal van Azure inschakelen](cache-configure.md#access-ports) voordat u de test loopt hebt. Zie voor voorbeelden [hoe kan ik gebruikelijke en testen van de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   VM gebruikt voor het testen van de client moet in hetzelfde gebied, als het exemplaar van uw bestand Vgx. cache.
-   Het is raadzaam Dv2 VM reeks met voor de klant, terwijl ze nog beter hardware hebben en de beste resultaten krijgt.
-   Controleer of de klant VM die u kiest heeft ten minste in dezelfde mate computing en de mogelijkheid bandbreedte als de cache die u wilt testen. 
-   Schakel op de clientcomputer VRSS wanneer u in Windows. [Zie voor meer informatie](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium laag bestand Vgx. exemplaren wordt hebt beter netwerk latentie en doorvoer omdat ze op betere hardware worden uitgevoerd voor zowel CPU en netwerk.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Wat zijn enkele van de overwegingen wanneer u een veelgebruikte opdrachten bestand Vgx. gebruikt?

-   U moet bepaalde bestand Vgx. opdrachten die erg lang duren om uit te voeren zonder de invloed van deze opdrachten niet uitvoeren.
    -   Bijvoorbeeld, worden niet uitgevoerd de opdracht [TOETSEN](http://redis.io/commands/keys) in productie terwijl het kan lang duren om terug te keren al naargelang het aantal toetsen duren. Bestand Vgx. een single thread-server en opdrachten onder elkaar tegelijk worden verwerkt. Als er andere opdrachten na TOETSEN, worden ze niet verwerkt totdat de opdracht TOETSEN bestand Vgx. verwerkt. De [redis.io site](http://redis.io/commands/) heeft details rond de complexiteit van de tijd voor elke bewerking die wordt ondersteund. Klik op elke opdracht om te zien van de complexiteit voor elke bewerking.
-   Belangrijke grootte - nut kleine sleutelwaarden of grote sleutelwaarden? In het algemeen is de afhankelijk van het scenario. Als uw scenario vereist is voor grotere sleutels vervolgens aanpassen van het type time-out van de verbinding en probeer waarden en aanpassen van uw logica opnieuw. Een bestand Vgx. server vanuit het perspectief van waargenomen kleinere waarden zijn dat betere prestaties.
-   Dit betekent niet dat u hogere waarden kunt opslaan in het bestand Vgx.; u moet op de hoogte van de volgende overwegingen. Vertragingstijden wordt hoger zijn. Als er één reeks gegevens dat groter is en dat kleiner is, kunt u meerdere exemplaren van ConnectionMultiplexer, elk met een andere set waarden time-out en probeer het opnieuw configureren, zoals beschreven in de vorige sectie voor [Wat moeten de configuratieopties StackExchange.Redis doen](#cache-configuration) .



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Hoe kan ik gebruikelijke en testen van de prestaties van mijn cache?

-   [Diagnostische hulpprogramma's cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat u [monitor](cache-how-to-monitor.md) de status van de cache kunt. U kunt de statistieken bekijken in de portal van Azure en kunt u ook het [downloaden en controleer](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ze met de hulpmiddelen van uw keuze.
-   U kunt bestand Vgx. benchmark.exe gebruiken om te laden test uw server bestand Vgx..
-   Zorg ervoor dat het selectievakje laden testen van de client en de cache bestand Vgx. in dezelfde regio.
-   Gebruik bestand Vgx. cli.exe en controleren van de cache met de opdracht INFO.
-   Als uw laden hoog geheugenfragmentatie veroorzaakt moet vervolgens u uitgebreid naar een grotere cachegrootte.
-   Zie voor instructies over het downloaden van de hulpmiddelen voor het bestand Vgx. het [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.

Hier volgt een voorbeeld van het gebruik van bestand Vgx. benchmark.exe. Voor nauwkeurige resultaten, moet u deze opdracht uitvoeren vanuit een VM in hetzelfde gebied, als de cache.

-   Test Pipelined SET-aanvragen met een 1 k nettolading

    bestand Vgx. benchmark.exe - h **yourcache**. - een redis.cache.windows.net- **yourAccesskey** -t 1024 - P 50 - n 1000000 -d instellen
    
-   Test Pipelined krijgen, vraagt met een 1 k nettolading. 
    Opmerking: Uitgevoerd de SET testen wordt weergegeven boven eerst naar de cache vullen
    
    bestand Vgx. benchmark.exe - h **yourcache**. - een redis.cache.windows.net- **yourAccesskey** -t -n 1000000 - d 1024 - P 50 ophalen

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Belangrijke informatie over ThreadPool groei

De CLR-ThreadPool heeft twee soorten threads - 'Werknemer' en 'i/o-voltooiingspoort"(of IOCP) threads. 

-   Werk-threads worden gebruikt wanneer u zich voor de items, zoals verwerking `Task.Run(…)` of `ThreadPool.QueueUserWorkItem(…)` methoden. Deze threads worden ook gebruikt door de diverse onderdelen van de CLR wanneer werk moet worden uitgevoerd op een achtergrondthread.
-   IOCP threads worden gebruikt als asynchrone IO gebeurt (bijvoorbeeld lezen van het netwerk). 

De thread-groep biedt nieuwe werknemer threads of i/o-voltooiing threads op aanvraag (zonder een beperking) totdat de instelling "Minimum" voor elk type thread wordt bereikt. Het minimum aantal threads is standaard ingesteld op het aantal processors op een systeem. 

Wanneer het aantal bestaande (bezet) threads treffers "" van het minimumaantal threads, de ThreadPool wordt invoegt beperking is van de snelheid waarmee nieuwe threads naar één thread per 500 milliseconden. Dit betekent dat als uw systeem een burst van werk dat u een thread IOCP nodig hebt krijgt, worden verwerkt die kunnen worden geopend zeer snel. Echter als de omvang van het werk groter dan de geconfigureerde "Minimum"-instelling is, worden sommige vertraging deel van het werk verwerken wanneer de ThreadPool van twee dingen veroorzaken, wacht.

1. Een bestaande thread wordt gratis verwerkingstijd van het werk.
1. Geen bestaande thread wordt gratis uit gedurende 500ms, zodat u een nieuwe thread wordt gemaakt.

In principe, betekent dit dat wanneer het aantal bezet threads groter dan Min threads is, u waarschijnlijk een vertraging 500ms betaalt voordat netwerkverkeer wordt verwerkt door de toepassing. Het is ook Houd er rekening mee dat wanneer een bestaande thread niet langer dan 15 seconden (gebaseerd op wat ik onthouden) actief blijft, deze worden opgeschoond en deze cyclus van groei en inkrimping kunt herhalen.

Als we een voorbeeld van foutbericht uit StackExchange.Redis (1.0.450 maken of hoger), ziet u deze nu wordt afgedrukt ThreadPool statistieken (Zie IOCP en werknemer hieronder voor meer informatie).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Klik in het bovenstaande voorbeeld ziet u dat er 6 bezet threads voor IOCP thread en het systeem is geconfigureerd voor het toestaan van 4 minimaal threads. In dit geval de client zou hebben waarschijnlijk gezien twee 500 ms vertragingen omdat 6 > 4.

Houd er rekening mee dat StackExchange.Redis time-out bereikt kunt als groei van IOCP of werknemer threads wordt vertraagd.

### <a name="recommendation"></a>Aanbeveling

Basis van deze informatie wordt aangeraden dat klanten beginwaarde voor de minimale configuratie voor IOCP en werknemer threads te vestigen groter is dan de standaardwaarde. We niet kunt standaardoplossing voor alle richtlijnen voor deze waarde moet omdat de juiste waarde voor een toepassing te hoog/laag voor een andere toepassing is geven. Deze instelling kan ook van invloed zijn op de prestaties van andere delen van gecompliceerde toepassingen, zodat elke klant moet deze instelling aan de behoeften van hun specifieke verfijnen. Een goed beginpunt is 200 of 300, testen en naar wens aanpassen.

Hoe deze instelling wilt configureren:

-   In ASP.NET, gebruikt u de ["minIoThreads" configuratie-instelling][] onder de `<processModel>` configuratie-element in web.config. Als u binnen Azure WebSites uitvoert, zijn deze instelling is niet beschikbaar gemaakt door de configuratieopties. Echter u nog steeds moet kunnen deze optie via programmacode instellen (Zie hieronder) van de methode Application_Start in global.asax.cs.

> **Belangrijke opmerking:** de waarde die is opgegeven in deze configuratie-element is een *core -* instelling. Bijvoorbeeld als u een computer met 4 en wilt uw instelling minIOThreads 200 gedurende runtime, gebruikt u `<processModel minIoThreads="50"/>`.

-   Buiten ASP.NET, gebruikt u de [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Server ° c om meer doorvoer op de client bij gebruik van StackExchange.Redis inschakelen

Inschakelen van server ° c kunt optimaliseren van de client en kunnen betere prestaties en de doorvoer zijn met StackExchange.Redis. Zie de volgende artikelen voor meer informatie over de server ° c en hoe u deze inschakelen.

-   [Server globale Catalogus inschakelen](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Basisprincipes van opschonen](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Opschonen en prestaties](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Hoe ik de gezondheid en de prestaties van mijn cache controleren?

Microsoft Azure bestand Vgx. Cache exemplaren kunnen worden gecontroleerd in de [portal van Azure](https://portal.azure.com). U kunt statistieken bekijken, vastmaken aan de doelstellingen grafieken naar de Startboard aanpassen van de datum- en tijdbereik van grafieken monitoring, toevoegen en verwijderen van de doelstellingen van de grafieken en waarschuwingen instellen wanneer bepaalde voorwaarden is voldaan. Zie [Monitor Azure bestand Vgx. Cache](cache-how-to-monitor.md)voor meer informatie.

De sectie **ondersteuning + probleemoplossing** van het blad bestand Vgx. Cache- **Instellingen** bevat ook diverse hulpprogramma's voor controle en probleemoplossing bij uw cache. 

-   **Problemen oplossen met** vindt u informatie over veelvoorkomende problemen en strategieën voor het oplossen van deze.
-   **Controlelogboeken bijhouden** wordt aandacht besteed aan acties in de cache. U kunt ook filters gebruiken om uit te vouwen deze weergave als u wilt opnemen van andere bronnen.
-   **Status van de resource** toekijkt van de resource en kunt u zien als deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de service van de servicestatus Azure Resource [Servicestatus van Azure Resourceoverzicht](../resource-health/resource-health-overview.md).
-   **Nieuwe aanvraag ondersteuning** biedt opties om een ondersteuningsverzoek voor de cache te openen.

Deze hulpprogramma's kunnen u de status van uw exemplaren Azure bestand Vgx. Cache controleren en kunt u uw in de cache-toepassingen beheren. Zie [ondersteuning en probleemoplossing van instellingen](cache-configure.md#support-amp-troubleshooting-settings)voor meer informatie.

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Mijn cache diagnostisch hulpprogramma opslag Accountinstellingen gewijzigd, wat is er gebeurd?

Cache in het hetzelfde land- en -abonnement delen de instellingen voor opslag van dezelfde diagnostisch hulpprogramma en als de configuratie gewijzigd (diagnostisch hulpprogramma ingeschakeld is/uitgeschakeld of het wijzigen van het account opslag) is geschikt voor alle cache in het abonnement die in die regio zijn. Controleer als de instellingen diagnostische hulpprogramma's voor de cache hebt gewijzigd, als de diagnostische instellingen voor een andere cache in dezelfde abonnement en regio zijn gewijzigd. Een manier om te controleren is om weer te geven van de controlelogboeken voor de cache voor een `Write DiagnosticSettings` gebeurtenis. Zie voor meer informatie over het werken met controlelogboeken [gebeurtenissen weergeven en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) en [bewerkingen met resourcemanager controleren](../resource-group-audit.md). Zie voor meer informatie over het controleren van Azure bestand Vgx. Cache gebeurtenissen [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Waarom is de diagnostische hulpprogramma's voor enkele nieuwe cache, maar andere niet ingeschakeld?

Cache in dezelfde regio en abonnement delen dezelfde diagnostisch hulpprogramma opslaginstellingen. Als u een nieuwe cache in de regio en het abonnement als een andere cache met diagnostische gegevens die zijn ingeschakeld maken, worden diagnostische gegevens is ingeschakeld op de nieuwe cache met dezelfde instellingen.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Waarom zie ik time-out?

Time-out optreden in de client die u kunt met het bestand Vgx. communiceren. Voor het grootste deel biedt bestand Vgx. server geen time-out. Wanneer een opdracht wordt verzonden naar de server bestand Vgx., de opdracht is in de wachtrij geplaatst en server bestand Vgx. uiteindelijk de opdracht wordt opgehaald en deze uitvoert. Echter de client kunt time-out tijdens dit proces en als resultaat een uitzondering wordt verheven aan de bellen. Zie voor meer informatie over het oplossen van problemen time-out [Client probleemoplossing](cache-how-to-troubleshoot.md#client-side-troubleshooting) en [StackExchange.Redis time-out uitzonderingen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Waarom is mijn client verbroken uit de cache?

Hier volgen enkele veelvoorkomende reden voor een cache verbinding verbreken.

-   Aan de clientzijde oorzaken
    -   De clienttoepassing is geïmplementeerd.
    -   De clienttoepassing een schaal bewerking uitgevoerd.
        -   Bij Cloudservices of Web Apps, kan dit komen vanwege automatisch schalen.
    -   De netwerken laag aan de clientzijde is gewijzigd.
    -   Tijdelijke fouten opgetreden in de client of in het netwerkknooppunten tussen de client en de server.
    -   De grenswaarden bandbreedte zijn bereikt.
    -   CPU gebonden bewerkingen duurde te lang is om te voltooien.
-   Aan de clientzijde oorzaken
    -   Klik op de standaard cache aanbod, door de service Azure bestand Vgx. Cache gestarte een storing vanaf het primaire knooppunt naar het secundaire knooppunt.
    -   Azure is het exemplaar waar de cache is geïmplementeerd herstellen
        -   Dit is voor de server-updates bestand Vgx. of algemeen VM onderhoud.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Welke Azure Cache aanbod is geschikt voor mij?

>[AZURE.IMPORTANT]Aan de hand van vorig jaar [aankondiging](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), wordt op 30 November 2016 Azure-Service beheerd Cache en Azure In rol Cache service ingetrokken. Wij raden is via [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/). Zie voor informatie over het migreren van [migreren van beheerde Cache Service aan Azure bestand Vgx. Cache](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Cache Azure bestand Vgx.
Azure bestand Vgx. Cache is in het algemeen beschikbaar zijn in die groter zijn tot 53 GB en heeft een beschikbaarheid SLA van 99,9%. De nieuwe [premium laag](cache-premium-tier-intro.md) grootten maximaal biedt ondersteuning voor cluster, VNET en permanente, met een SLA 99,9% en 530 GB.

Azure bestand Vgx. Cache hebben gebruikers de mogelijkheid om te gebruiken van een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. Met deze aanbieding krijgt u gebruikmaken van de uitgebreide functies en -systeem verstrekt door het bestand Vgx., en betrouwbare hostingprovider en het controleren van Microsoft.

In tegenstelling tot traditionele cache die alleen sleutel-waardeparen afhandelen, is het populaire voor de bijbehorende ten zeerste zodat gegevenstypen bestand Vgx.. Bestand Vgx. ook ondersteunt uitgevoerd atomaire bewerkingen op deze typen, zoals toe te voegen aan een tekenreeks bevat; de waarde in een hash; verhogen Zet nieuwe aan een lijst. Computing set snijpunt, union en verschil; of het lid aan hoogste waarde in een set gesorteerd. Andere functies bieden ondersteuning voor transacties, Publisher/sub, Lua uitvoeren van scripts toetsen met een beperkte time to live, en configuratie-instellingen om te maken bestand Vgx. Als een traditionele cache fungeren.

Een ander belangrijke aspect voor succes bestand Vgx. is de orde, levendige bron openen-systeem ingebouwd eromheen. Dit is doorgevoerd in de diverse verzameling bestand Vgx. clients beschikbaar in meerdere talen. Dit kan worden gebruikt door vrijwel elke werkbelasting die u binnen Azure maken wilt. 

Zie [hoe u gebruik Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en [Azure bestand Vgx. Cache documentatie](https://azure.microsoft.com/documentation/services/redis-cache/)voor meer informatie over het aan de slag met Azure bestand Vgx. Cache.

### <a name="managed-cache-service"></a>Beheerde Cache-service
[Cache-service is ingesteld op 30 November 2016 worden teruggetrokken beheerd.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>In de rol Cache
[In de rol Cache is ingesteld op 30 November 2016 worden teruggetrokken.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





["minIoThreads" configuratie-instelling]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
