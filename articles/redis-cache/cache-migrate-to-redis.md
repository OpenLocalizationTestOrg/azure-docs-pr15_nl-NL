<properties 
    pageTitle="Cache migreren naar het bestand Vgx. | Microsoft Azure"
    description="Informatie over het migreren van beheerde Cache servicetoepassingen aan Azure-Cache bestand Vgx."
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
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migreren van Service beheerde Cache naar Cache Azure bestand Vgx.

Migreren van uw toepassingen die gebruikmaken van Azure-Service beheerd Cache aan Azure bestand Vgx. Cache kan worden uitgevoerd met minimale wijzigingen aan uw toepassing, afhankelijk van de Service voor beheerde Cache-functies die worden gebruikt door uw toepassing in de cache. Terwijl de API's niet precies hetzelfde zijn ze lijken en veel van uw bestaande code die Service Cache beheerd voor toegang tot een cache opnieuw kan worden gebruikt met minimale wijzigingen. In dit onderwerp ziet u hoe u de benodigde configuratie en wijzigingen in de toepassing te migreren van uw beheerde Cache servicetoepassingen gebruik van Azure bestand Vgx. Cache en ziet u hoe enkele van de functies van Azure bestand Vgx. Cache kunnen worden gebruikt om de functionaliteit van de cache van een Service Cache beheerd implementeren.

## <a name="migration-steps"></a>Migratiestappen

De volgende stappen zijn vereist voor het migreren van een beheerd Cache servicetoepassing Azure bestand Vgx. Cache gebruiken.

-   Cache-Service beheerd functies toewijzen aan Azure-Cache bestand Vgx.
-   Kies een aanbod Cache
-   Een Cache maken
-   De Cache-Clients configureren
    -   De configuratie van de Service beheerde Cache verwijderen
    -   Een cache-client met het StackExchange.Redis NuGet-pakket configureren
-   Migreren van Service Cache beheerde code
    -   Verbinding maken met de cache voor de klas ConnectionMultiplexer gebruiken
    -   Access primitieve gegevenstypen in de cache
    -   Werken met .NET-objecten in de cache
-   ASP.NET-sessie State en uitvoercaching aan Azure bestand Vgx. Cache migreren 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Cache-Service beheerd functies toewijzen aan Azure-Cache bestand Vgx.

Azure Service Cache beheerd en Azure bestand Vgx. Cache lijken, maar enkele van de functies op verschillende manieren implementeren. In deze sectie worden enkele van de verschillen en bevat richtlijnen over het implementeren van de functies van Service Cache beheerd in Azure bestand Vgx. Cache.

|Beheerde Service Cache-functie|Beheerde Service Cache-ondersteuning|Ondersteuning van Azure Cache bestand Vgx.|
|---|---|---|
|Benoemde cache|Een standaardcache is geconfigureerd en in de cache Standard en Premium is voltooid, maximaal negen aanvullende benoemde cache kunnen worden geconfigureerd desgewenst.|Azure bestand Vgx. cache hebben een configureerbare aantal databases (de standaardinstelling van 16) die kunnen worden gebruikt om een vergelijkbare functionaliteit biedt naar benoemde cache implementeren. Zie [Serverconfiguratie standaard bestand Vgx.](cache-configure.md#default-redis-server-configuration)voor meer informatie.|
|Beschikbaarheid|Beschikbaarheid biedt voor items in de cache in de cache Standard en Premium aanbiedingen. Als artikelen verloren vanwege een fout, zijn back-ups van de items in de cache nog steeds beschikbaar. Aan de secundaire cache worden geschreven synchroon.|Beschikbaarheid is beschikbaar in de standaard- en Premium cache is voltooid, waarvoor een primaire/Replica-configuratie van twee knooppunten (elke shard in een Premium-cache heeft een paar primaire/replica). Aan de replica worden geschreven asynchroon. Zie [Azure bestand Vgx. Cache prijzen](https://azure.microsoft.com/pricing/details/cache/)voor meer informatie.|
|Meldingen|Kunnen clients asynchrone meldingen wilt ontvangen wanneer allerlei cache bewerkingen optreden op een benoemde cache.|Clienttoepassingen kunnen bestand Vgx. Publisher-/ sub of [Keyspace meldingen](cache-configure.md#keyspace-notifications-advanced-settings) gebruiken om een vergelijkbare functionaliteit biedt op meldingen.|
|Lokale cache|Een kopie van objecten in de cache lokaal opgeslagen op de client voor zeer snelle toegang.|Clienttoepassingen moeten deze functionaliteit met een woordenlijst of een vergelijkbare gegevensstructuur implementeren.|
|Een verwijdering beleid|Geen of Recentelijk. Het standaardbeleid is Recentelijk.|Azure bestand Vgx. Cache ondersteunt de volgende verwijdering beleidsitems: vluchtige recentelijk, allkeys-recentelijk, vluchtige willekeurig, allkeys-willekeurig vluchtige-ttl, noeviction. Het standaardbeleid is vluchtige recentelijk. Zie [Serverconfiguratie standaard bestand Vgx.](cache-configure.md#default-redis-server-configuration)voor meer informatie.|
|Verloopbeleid|Het verloopbeleid standaard is de Absolute en het verloopbeleid interval is standaard tien minuten. Schuiven en nooit beleidsregels zijn ook beschikbaar.|Standaard items in de cache niet verlopen, maar een verloopbeleid kan worden geconfigureerd een per schrijven via cache set overbelastingen. Zie [toevoegen en ophalen objecten uit de cache](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache)voor meer informatie.|
|Regio's en labelen|Gebieden zijn subgroepen voor items die in de cache opgeslagen. Regio's ondersteunen ook de aantekening in de cache opgeslagen items met extra beschrijvende tekenreeksen tags genoemd. Regio's ondersteunen de mogelijkheid om te zoekbewerkingen uitvoeren op gemarkeerde items in dat gebied. Alle items in een gebied zich bevinden binnen één knooppunt van het cluster cache.|Een bestand Vgx. cache bestaat uit één knooppunt, (tenzij bestand Vgx. cluster is ingeschakeld) zodat het concept van Service Cache beheerd regio's is niet van toepassing. Bestand Vgx. ondersteunt zoeken en jokertekens bewerkingen bij het ophalen van toetsen zodat beschrijvende labels kunnen worden ingesloten in de belangrijkste namen en gebruikt voor het ophalen van de items later. Zie [implementeren cache labelen met het bestand Vgx.](http://stackify.com/implementing-cache-tagging-redis/)voor een voorbeeld van een sociaalnetwerklabels solution met bestand Vgx. implementeren.|
|Serialisatie|Beheerde Cache ondersteunt NetDataContractSerializer BinaryFormatter en het gebruik van aangepaste objectserializers. De standaardinstelling is NetDataContractSerializer.|Dit is de verantwoordelijkheid van de clienttoepassing .NET-objecten serialiseren voordat ze in de cache, met de keuze uit de serializer snel aan de ontwikkelaar van de client-toepassing te plaatsen. Voor meer informatie en voorbeelden van code, raadpleegt u [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Cache emulator | Beheerde Cache biedt een emulator lokale cache. | Azure bestand Vgx. Cache heeft geen een emulator, maar u kunt [uitvoeren van de build MSOpenTech van bestand Vgx.-server.exe lokaal](cache-faq.md#cache-emulator) een emulator-ervaring beschikken. |

## <a name="choose-a-cache-offering"></a>Kies een aanbod Cache

Microsoft Azure bestand Vgx. Cache is beschikbaar in de volgende lagen:

-   **Eenvoudige** – één knooppunt. Veelvoud groter maximaal 53 GB.
-   **Standaard** – twee knooppunten primaire/Replica. Veelvoud groter maximaal 53 GB. 99,9 SLA %.
-   **Premium** – twee knooppunten primaire/Replica met maximaal 10 shards. Meerdere formaten vanuit 6 GB 530 GB (contact met ons opnemen voor meer informatie). Alle onderdelen van de standaard laag en meer inclusief ondersteuning voor het [bestand Vgx. cluster](cache-how-to-premium-clustering.md), [bestand Vgx. permanente](cache-how-to-premium-persistence.md)en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 SLA %.

Elke laag verschilt gebied van voorzieningen en prijzen. De functies verderop in deze handleiding, en voor meer informatie over de prijzen worden behandeld, raadpleegt u [Cache prijzen Details](https://azure.microsoft.com/pricing/details/cache/).

Een beginpunt voor migratie is kiest u de grootte die overeenkomt met de grootte van uw vorige Service Cache beheerd cache en vervolgens omhoog of omlaag te schalen afhankelijk van de vereisten van uw toepassing. Zie voor meer hulp bij het kiezen van de juiste Azure bestand Vgx. Cache aanbod, [welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Een Cache maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>De Cache-Clients configureren

Wanneer de cache gemaakt en geconfigureerd, wordt de volgende stap is de configuratie van beheerde Cache Service verwijderen en het toevoegen van de configuratie Azure bestand Vgx. Cache toevoegen en verwijzingen zodat cache clients toegang hebt tot de cache.

-   De configuratie van de Service beheerde Cache verwijderen
-   Een cache-client met het StackExchange.Redis NuGet-pakket configureren

### <a name="remove-the-managed-cache-service-configuration"></a>De configuratie van de Service beheerde Cache verwijderen

Voordat de clienttoepassingen kunnen worden geconfigureerd voor Azure bestand Vgx. Cache, de bestaande Cache-Service beheerd-configuratie en constructie verwijzingen moeten worden verwijderd door het pakket beheerde Cache Service NuGet ongedaan te maken.

Als u wilt het pakket beheerde Cache Service NuGet wilt verwijderen, met de rechtermuisknop op het project client in **Solution Explorer** en kies **NuGet-pakketten beheren**. Selecteer het knooppunt **geïnstalleerde-pakketten** en typ W**indowsAzure.Caching** in het zoekvak van de geïnstalleerde pakketten. Selecteer **Windows** **Azure-Cache** (of een **Windows** **Azure Caching** afhankelijk van de versie van het pakket NuGet), klik op **verwijderen**en klik vervolgens op **sluiten**.

![Azure beheerde Cache NuGet servicepakket verwijderen](./media/cache-migrate-to-redis/IC757666.jpg)

Het pakket beheerde Cache Service NuGet verwijderen Hiermee verwijdert u de verzamelingen Service Cache beheerd en de Service voor beheerde Cache-vermeldingen in de app.config of web.config van de clienttoepassing. Omdat sommige aangepaste instellingen kunnen niet worden verwijderd wanneer u het pakket NuGet verwijdert, open web.config of app.config en zorg ervoor dat de volgende elementen volledig zijn verwijderd.

Zorg ervoor dat de `dataCacheClients` vermelding is verwijderd uit de `configSections` element. Verwijder niet de gehele `configSections` element; alleen verwijderen de `dataCacheClients` vermelding, indien aanwezig.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Zorg ervoor dat de `dataCacheClients` sectie wordt verwijderd. De `dataCacheClients` sectie is vergelijkbaar met het volgende voorbeeld.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Zodra de configuratie van beheerde Cache-Service is verwijderd, kunt u de cache-client kunt configureren, zoals wordt beschreven in de volgende sectie.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Een cache-client met het StackExchange.Redis NuGet-pakket configureren

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migreren van Service Cache beheerde code

De API voor de client van de cache StackExchange.Redis is vergelijkbaar met de Service Cache beheerd. Deze sectie bevat een overzicht van de verschillen.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Verbinding maken met de cache voor de klas ConnectionMultiplexer gebruiken

In Cache-Service beheerd verbindingen met de cache zijn afgehandeld door de `DataCacheFactory` en `DataCache` klassen. In de Cache van Azure bestand Vgx., deze verbindingen worden beheerd door de `ConnectionMultiplexer` class.

Voeg de volgende met de instructie naar het begin van een bestand waaruit u wilt toegang tot de cache.

    using StackExchange.Redis
                                
Als deze naamruimte niet wordt opgelost, moet u dat u het pakket StackExchange.Redis NuGet hebt toegevoegd zoals is beschreven in [de cache-clients configureren](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Houd er rekening mee dat de client StackExchange.Redis .NET Framework 4 of hoger vereist.

Als u wilt verbinden met een exemplaar van de Azure bestand Vgx. Cache, belt u het statische `ConnectionMultiplexer.Connect` methode en wachtwoord in het eindpunt en van toetsen. Een methode tot het delen van een `ConnectionMultiplexer` exemplaar in uw toepassing is dat een statische eigenschap die een verbonden exemplaar, vergelijkbaar met het volgende voorbeeld retourneert. Thread-veilige manier kan slechts één verbonden geïnitialiseerd `ConnectionMultiplexer` exemplaar. In dit voorbeeld `abortConnect` is ingesteld op ONWAAR, wat betekent dat de oproep slagen zelfs als de verbinding met de cache is niet gemaakt. Een belangrijke functie van `ConnectionMultiplexer` is dat deze automatisch connectivity naar de cache eenmaal het netwerkprobleem in het herstellen wordt of andere oorzaken opgelost zijn.

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

De cache eindpunt, sleutels en poorten kunnen worden opgehaald uit het blad **Bestand Vgx. Cache** voor uw exemplaar van de cache. Zie [Eigenschappen voor bestand Vgx. Cache](cache-configure.md#properties)voor meer informatie.

Nadat de verbinding tot stand is gebracht, het resultaat van een verwijzing naar de cache bestand Vgx. database door de ondersteuning voor de `ConnectionMultiplexer.GetDatabase` methode. Het object dat het resultaat van de `GetDatabase` methode is een lightweight Pass Through-object en niet hoeft te worden opgeslagen.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

De client StackExchange.Redis gebruikt de `RedisKey` en `RedisValue` typen voor toegang tot, en items in de cache opslaan. Dit soort naar meest primitieve taaltypen, met inbegrip van de tekenreeks, toewijzen en vaak worden niet gebruikt rechtstreeks. Bestand Vgx. tekenreeksen zijn de belangrijkste soort waarde bestand Vgx. en veel soorten gegevens, inclusief serienummer binaire streams, kunnen bevatten en terwijl u het type niet rechtstreeks gebruikt mogelijk, wordt u methoden met `String` in de naam. Voor de meest primitieve gegevenstypen u opslaan en items weer ophalen vanuit de cache met de `StringSet` en `StringGet` methoden, tenzij u verzamelingen of andere gegevenstypen bestand Vgx. in de cache opslaat. 

`StringSet`en `StringGet` zijn vergelijkbaar met de Service Cache beheerde `Put` en `Get` methoden, met een primaire verschil dat voordat u instellen en krijg een .NET-object in de cache u deze eerst serialiseren moet. 

Bij het aanroepen van `StringGet`, als het object bestaat, wordt deze geretourneerd en als dit niet het geval is, null-waarden geretourneerd. In dit geval kunt u de waarde uit de gewenste gegevensbron ophalen en opslaan in de cache voor later gebruik. Dit is het patroon dat in cache grond genoemd.

Als u wilt opgeven de vervaldatum van een item in de cache, gebruikt u de `TimeSpan` -parameter van `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure bestand Vgx. Cache kunt werken met .NET-objecten, evenals primitieve gegevenstypen, maar voordat een .NET-object kan worden opgeslagen in de cache deze modules moet worden ingedeeld. Dit is de verantwoordelijkheid van de ontwikkelaar van toepassingen. Dit hebt de flexibiliteit ontwikkelaars in de keuze van de serializer. Voor meer informatie en voorbeelden van code, raadpleegt u [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.NET-sessie State en uitvoercaching aan Azure bestand Vgx. Cache migreren

Azure bestand Vgx. Cache heeft providers voor zowel ASP.NET sessie- en pagina-uitvoer caching. Voor het migreren van uw toepassing die gebruikmaakt van de Service voor beheerde Cache-versies van deze providers, eerst de bestaande secties verwijderen uit uw web.config en configureer de Azure bestand Vgx. Cache-versies van de providers. Zie voor instructies over het gebruik van de Azure bestand Vgx. Cache ASP.NET-providers [ASP.NET sessie staat Provider voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md) en [ASP.NET uitvoer Cache Provider voor Azure bestand Vgx. Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Volgende stappen

Verken de [Cache van Azure bestand Vgx. documentatie](https://azure.microsoft.com/documentation/services/cache/) voor zelfstudies, voorbeelden, video's en meer.

