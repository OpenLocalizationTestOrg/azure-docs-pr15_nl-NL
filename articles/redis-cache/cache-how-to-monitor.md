<properties 
    pageTitle="Het controleren van Azure bestand Vgx. Cache | Microsoft Azure" 
    description="Informatie over het controleren van de gezondheid en de prestaties van uw exemplaren van de Cache van Azure bestand Vgx." 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Het controleren van de Cache van Azure bestand Vgx.

Azure bestand Vgx. Cache vindt u verschillende opties voor het controleren van uw exemplaren cache. U kunt statistieken bekijken, vastmaken aan de doelstellingen grafieken naar de Startboard aanpassen van de datum- en tijdbereik van grafieken monitoring, toevoegen en verwijderen van de doelstellingen van de grafieken en waarschuwingen instellen wanneer bepaalde voorwaarden is voldaan. Deze hulpprogramma's kunnen u de status van uw exemplaren Azure bestand Vgx. Cache controleren en kunt u uw in de cache-toepassingen beheren.

Als cache diagnostische hulpprogramma's zijn ingeschakeld, zijn de doelstellingen voor Azure bestand Vgx. Cache exemplaren die worden verzameld ongeveer elke 30 seconden en opgeslagen, zodat ze kunnen worden weergegeven in de grafieken de doelstellingen en geëvalueerd met waarschuwingsregels.

Cache aan de doelstellingen zijn verzameld met het bestand Vgx. [INFO](http://redis.io/commands/info) opdracht. Voor meer informatie over de verschillende informatie waarden die worden gebruikt voor elk metrisch in cache, raadpleegt u [beschikbare maatstaven en rapportage intervallen](#available-metrics-and-reporting-intervals).

Cache aan de doelstellingen, [Blader](cache-configure.md#configure-redis-cache-settings) naar uw exemplaar van de cache in de [portal van Azure](https://portal.azure.com)weergeven. Aan de doelstellingen voor Azure bestand Vgx. Cache exemplaren zijn toegankelijk op het blad **bestand Vgx. aan de doelstellingen** .

![Bestand Vgx aan de doelstellingen.][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Als het volgende bericht wordt weergegeven in het blad **bestand Vgx. aan de doelstellingen** , volgt u de stappen in de sectie [diagnostische hulpprogramma's cache inschakelen](#enable-cache-diagnostics) om in te schakelen cache diagnostische gegevens.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Het **bestand aan de doelstellingen Vgx.** blad heeft **controle** grafieken die aan de doelstellingen cache worden weergegeven. Elke grafiek kan worden aangepast door toevoegen of verwijderen van de doelstellingen en de rapportagemogelijkheden interval wijzigen. Voor weergeven en configureren van bewerkingen en waarschuwingen, heeft het **Bestand Vgx. Cache** blad een sectie **bewerkingen** , waarin de cache **gebeurtenissen** en **waarschuwingsregels**.

## <a name="enable-cache-diagnostics"></a>Diagnostische hulpprogramma's cache inschakelen

Azure bestand Vgx. Cache biedt de mogelijkheid moet naar diagnostische gegevens die zijn opgeslagen in een opslag-account, zodat u kunt een hulpmiddelen die u wilt openen en de gegevens rechtstreeks verwerken. In de volgorde voor cache diagnostische gegevens moeten worden verzameld, moet opgeslagen en weergegeven in de portal Azure een opslag-account worden geconfigureerd. Cache in het hetzelfde land- en -abonnement delen dezelfde diagnostisch hulpprogramma opslag account en wanneer de configuratie is gewijzigd is geschikt voor alle cache in het abonnement die in die regio zijn.

Als u wilt inschakelen en configureren van cache diagnostische hulpprogramma's, Ga naar het blad **Bestand Vgx. Cache** voor uw exemplaar van de cache. Als diagnostische gegevens zijn nog niet ingeschakeld, wordt een bericht wordt weergegeven in plaats van een grafiek diagnostische gegevens.

![Diagnostische hulpprogramma's cache inschakelen][redis-cache-enable-diagnostics]

Klik op het bericht wilt weergeven van het blad **Metrisch** en **Diagnostische instellingen** wilt inschakelen en configureren van de diagnostische instellingen voor het exemplaar van de cache op.

![Instellingen diagnostische gegevens][redis-cache-diagnostic-settings]

![Diagnostische gegevens configureren][redis-cache-configure-diagnostics]

Klik op de knop **op** als u wilt inschakelen cache diagnostische hulpprogramma's en de configuratie van de diagnostische weer te geven.

Klik op de pijl rechts van **Opslag Account** een opslag-account hebben voor diagnostische gegevens selecteren. Selecteer een account opslagruimte in hetzelfde gebied, als de cache voor de beste prestaties.

Nadat de diagnostische instellingen zijn geconfigureerd, klikt u op **Opslaan** om op te slaan de configuratie. Houd er rekening mee dat het duurt even duren voordat de wijzigingen zijn doorgevoerd.

>[AZURE.IMPORTANT] Cache in het hetzelfde land- en -abonnement delen de instellingen voor opslag van dezelfde diagnostisch hulpprogramma en wanneer de configuratie gewijzigd (diagnostisch hulpprogramma ingeschakeld is/uitgeschakeld of het wijzigen van het account opslag) is geschikt voor alle cache in het abonnement die in die regio zijn.

Als u wilt bekijken van de doelstellingen van de opgeslagen, de tabellen in uw account opslagruimte met namen die met beginnen onderzoeken `WADMetrics`. Zie voor meer informatie over het gebruik van de doelstellingen van de opgeslagen buiten de portal van Azure de steekproef [gegevens Access bestand Vgx. Cache bewaken](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Alleen de doelstellingen die zijn opgeslagen in de geselecteerde opslag-account worden weergegeven in de portal van Azure. Als u opslag accounts wijzigt, worden de gegevens in de eerder geconfigureerde opslag-account blijft downloaden, maar deze niet wordt weergegeven in de portal van Azure.  

## <a name="available-metrics-and-reporting-intervals"></a>Beschikbare maatstaven en rapportage intervallen

Cache aan de doelstellingen zijn gerapporteerd op basis van meerdere rapportage intervallen, inclusief **afgelopen uur**, **vandaag**, **afgelopen week**en **aangepast**. Het blad **Metrisch** voor elke grafiek aan de doelstellingen geeft het gemiddelde, minimum en maximum waarden voor elke metrisch in de grafiek en sommige Maatstelsel van een totaal voor de rapportage interval weergeven. 

Elke metrisch bevat twee versies. Één meetwaarde metingen prestaties voor de hele cache en voor cache met [cluster](cache-how-to-premium-clustering.md), een tweede versie van de meetwaarde waarin `(Shard 0-9)` in de prestaties van de naam maatregelen voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 4 shards, `Cache Hits` is de totale hoeveelheid treffers voor de hele cache en `Cache Hits (Shard 3)` is alleen de treffers voor die shard van de cache.

>[AZURE.NOTE] Zelfs wanneer de cache niet actief is met geen verbonden actieve clienttoepassingen, ziet u mogelijk enkele cache-activiteit, zoals verbonden clients, geheugengebruik en bewerkingen dat wordt uitgevoerd. Deze activiteit is normaal tijdens het importeren van een exemplaar van de Azure bestand Vgx. Cache.

| Metrisch            | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Treffers in cache        | Het aantal geslaagde belangrijke zoekacties tijdens het opgegeven rapportage interval. Die wordt toegewezen aan `keyspace_hits` opdracht uit het bestand Vgx. [INFO](http://redis.io/commands/info) .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Cachemissers      | Het aantal mislukte belangrijke zoekacties tijdens het opgegeven rapportage interval. Die wordt toegewezen aan `keyspace_misses` met de opdracht bestand Vgx. INFO. Cache mislukte betekenen niet per se dat er is een probleem met de cache. Wanneer u met het patroon dat in cache grond programming, een toepassing ziet er bijvoorbeeld eerste in de cache voor een item. Als het item is er geen (cache ontbrekend), is het item opgehaald uit de database en toegevoegd aan de cache voor toekomstig gebruik. Cache mislukte zijn normale gedrag voor het patroon dat in cache grond programmeren. Als het aantal mislukte cache groter is dan verwacht, controleert u de toepassingslogica die wordt gevuld en leest uit de cache. Als u items zijn die wordt verwijderd uit de cache vanwege geheugendruk en er zijn enkele Cachemissers mogelijk, maar een betere meting om te controleren op geheugendruk wordt `Used Memory` of `Evicted Keys`. |
| Verbonden Clients | Het aantal verbindingen met de cache tijdens het opgegeven rapportage interval. Die wordt toegewezen aan `connected_clients` met de opdracht bestand Vgx. INFO. Als de [verbindingslimiet](cache-configure.md#default-redis-server-configuration) is bereikt, latere verbinding met de cache mislukt. Houd er rekening mee dat zelfs als er geen actieve clienttoepassing, er nog steeds mogelijk een paar exemplaren van verbonden cliënten vanwege interne processen en verbindingen.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Verwijderde sleutels      | Het aantal items verwijderd uit de cache tijdens het opgegeven rapportage interval einddatum voor de `maxmemory` limiet. Die wordt toegewezen aan `evicted_keys` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Verlopen toetsen      | Het aantal items verlopen uit de cache tijdens het opgegeven rapportage interval. Deze waarde wordt toegewezen aan `expired_keys` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Met deze eigenschap              | Het aantal get-bewerkingen uit de cache tijdens het opgegeven rapportage interval. Deze waarde is de som van de volgende waarden uit de INFO bestand Vgx. opdracht alle: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, en `cmdstat_getrange`, en is gelijk aan de som van de cachetreffers en mislukte tijdens het interval rapportage.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Bestand Vgx belasting van de Server. | Het percentage van maal waarin het bestand Vgx. server is bezet niet in afwachting van niet-actieve voor berichten en verwerkt. Als deze teller 100 die betekent dit de server bestand Vgx. heeft een ceiling prestaties limiet en kan niet worden verwerkt door de CPU moet u een sneller werken. Als u hoge belasting van bestand Vgx. Server ziet ziet u time-out uitzonderingen in de client. In dit geval beter schaalbaarheid van of uw gegevens in meerdere cache partitioneren.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Sets              | Het aantal set bewerkingen aan de cache tijdens het opgegeven rapportage interval. Deze waarde is de som van de volgende waarden uit de INFO bestand Vgx. opdracht alle: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, en `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Totale bewerkingen  | Het totale aantal opdrachten verwerkt door de cacheserver tijdens het opgegeven rapportage interval. Deze waarde wordt toegewezen aan `total_commands_processed` met de opdracht bestand Vgx. INFO. Houd er rekening mee dat wanneer Azure bestand Vgx. Cache wordt gebruikt voor publicatie/sub zuiver er worden geen aan de doelstellingen voor `Cache Hits`, `Cache Misses`, `Gets`, of `Sets`, maar er `Total Operations` aan de doelstellingen die overeenkomen met het gebruik van de cache voor publicatie/sub bewerkingen.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Gebruikte geheugen       | De hoeveelheid cachegeheugen voor sleutel/waardeparen in de cache in MB tijdens het opgegeven rapportage interval gebruikt. Deze waarde wordt toegewezen aan `used_memory` met de opdracht bestand Vgx. INFO. Dit is niet inbegrepen metagegevens of fragmentatie.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Gebruikte geheugen RSS   | De hoeveelheid cachegeheugen in MB gebruikt tijdens het opgegeven rapportage interval, inclusief fragmentatie en metagegevens. Deze waarde wordt toegewezen aan `used_memory_rss` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| CPU               | De CPU-gebruik van de server Azure bestand Vgx. Cache als een percentage tijdens het opgegeven rapportage interval. Deze waarde wordt toegewezen aan het besturingssysteem `\Processor(_Total)\% Processor Time` prestatie-item.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache lezen        | De hoeveelheid gegevens uit de cache in Megabytes per seconde (MB/s) tijdens het opgegeven rapportage interval gelezen. Deze waarde wordt afgeleid van het netwerk interface-kaarten die ondersteuning bieden voor de virtuele machine die fungeert als host de cache en niet is voorzien van een bestand Vgx. specifieke. **Deze waarde overeenkomt met de netwerkbandbreedte die worden gebruikt door deze cache. Als u instellen van waarschuwingen voor server kant netwerk bandbreedte limieten wilt, klikt u vervolgens maken met dit `Cache Read` item. Zie [deze tabel](cache-faq.md#cache-performance) van de limiet waargenomen bandbreedte voor verschillende cache lagen en de puntgrootten prijzen.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache schrijven       | De hoeveelheid gegevens die zijn opgeslagen in de cache in Megabytes per seconde (MB/s) tijdens het opgegeven interval rapportage. Deze waarde wordt afgeleid van het netwerk interface-kaarten die ondersteuning bieden voor de virtuele machine die fungeert als host de cache en niet is voorzien van een bestand Vgx. specifieke. Deze waarde overeenkomt met de bandbreedte van gegevens die zijn verzonden naar de cache van de client.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Het weergeven van de doelstellingen en grafieken aanpassen

U kunt een overzicht van de doelstellingen weergeven voor de cache op het blad **bestand Vgx. aan de doelstellingen** . Voor toegang tot het **bestand aan de doelstellingen Vgx.** blade Kies **alle instellingen** > **bestand Vgx. aan de doelstellingen**.

![Bestand Vgx aan de doelstellingen.][redis-cache-redis-metrics]


Het **bestand aan de doelstellingen Vgx.** blad bevat de volgende grafieken.

| Bestand aan de doelstellingen grafiek Vgx. | Aan de doelstellingen weergegeven     |
|------------------|-------------------|
| Cache-gelezen en Cache schrijven | Cache lezen |
|                            | Cache schrijven |
| Verbonden Clients      | Verbonden Clients |
| Treffers en mislukte  | Treffers in cache        |
|                  | Cachemissers      |
| Totale opdrachten   | Totale bewerkingen  |
| Haalt en sets weergegeven    | Met deze eigenschap              |
|                  | Sets              |
| CPU-gebruik         | CPU           |
| Geheugengebruik      | Gebruikte geheugen   |
|                   | Gebruikte geheugen RSS |
| Bestand Vgx belasting van de Server. | Belasting van de server   |
| Toets tellen | Totale aantal sleutels |
|           | Verwijderde sleutels |
|           | Verlopen toetsen |


Voor een gedetailleerde weergave van de doelstellingen op een bepaalde grafiek en de grafiek aanpassen, klikt u op de gewenste grafiek vanuit het blad **bestand Vgx. aan de doelstellingen** om het blad **Metrisch** voor die grafiek weer te geven.

![Metrische blade][redis-cache-metric-blade]

Waarschuwingen die zijn ingesteld op de weergegeven in een grafiek aan de doelstellingen worden vermeld onder aan het blad **Metrisch** voor die grafiek.

Als u wilt toevoegen of verwijderen van de doelstellingen of het rapportage interval wijzigen, kiest u de **Grafiek bewerken**.

Als u wilt toevoegen of verwijderen van de doelstellingen van de grafiek, klikt u op het selectievakje in naast de naam van de meetwaarde. De rapportage als interval wilt wijzigen, klikt u op het gewenste tijdsinterval. Het **grafiektype**wijzigen, klikt u op de gewenste stijl. Wanneer de gewenste wijzigingen zijn aangebracht, klikt u op **Opslaan**. 

![Grafiek bewerken][redis-cache-edit-chart]

Wanneer u op **Opslaan** klikt blijft uw wijzigingen totdat u het blad **Metrisch** verlaat. Wanneer u later terugkomen, ziet u de oorspronkelijke metrische en tijdsbereik opnieuw. Zie voor meer informatie over het aanpassen van grafieken [Monitor service de doelstellingen](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Als u wilt de statistieken voor een bepaalde periode bekijken in een grafiek, plaats u de muisaanwijzer op een van de specifieke Gantt-balken of punten op de grafiek die met de gewenste tijd overeenkomt en de doelstellingen voor dat interval worden weergegeven.

![Details van de grafiek weergeven][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Hoe u een premium-cache met cluster controleren

Premium-cache waarvoor [cluster](cache-how-to-premium-clustering.md) is ingeschakeld, kunnen maximaal 10 shards hebben. Elke shard heeft een eigen aan de doelstellingen en deze gegevens om te bieden aan de doelstellingen voor de cache als geheel worden samengevoegd. Elke metrisch bevat twee versies. Één meetwaarde metingen prestaties voor de hele cache en een tweede versie van de meetwaarde waarin `(Shard 0-9)` in de prestaties van de naam maatregelen voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 3 shards, `Cache Hits` is de totale hoeveelheid treffers voor de hele cache en `Cache Hits (Shard 2)` is alleen de treffers voor die shard van de cache.

Elke controle grafiek geeft het hoogste niveau aan de doelstellingen voor de cache samen met de doelstellingen voor elke shard cache.

![Monitor][redis-cache-premium-monitor]

De muis boven de gegevenspunten, ziet u de details voor dat punt in tijd. 

![Monitor][redis-cache-premium-point-summary]

De hogere waarden zijn meestal de statistische waarden voor de cache terwijl u de kleinere waarden zijn de afzonderlijke maatstaven voor de shard. Houd er rekening mee dat in dit voorbeeld zijn er drie shards en de cachetreffers gelijkmatig zijn verdeeld over de shards.

![Monitor][redis-cache-premium-point-shard]

Om te zien uitgebreider, klik op de grafiek om de uitgevouwen weergave op het blad **Metrisch** weer te geven.

![Monitor][redis-cache-premium-chart-detail]

Standaard bevat elke grafiek wordt het item op het hoogste niveau cache-prestaties, evenals de items voor de afzonderlijke shards. U kunt deze op het blad **Grafiek bewerken** .

![Monitor][redis-cache-premium-edit]

Zie voor meer informatie over de beschikbare items, [beschikbaar maatstaven en rapportage intervallen](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Bewerkingen en waarschuwingen

De sectie **bewerkingen** op het blad **Bestand Vgx. Cache** heeft **evenementen** en **waarschuwingsregels** secties.

![Oeprations][redis-cache-operations-events]

Om een lijst met recente cache bewerkingen, klikt u op de grafiek **gebeurtenissen** om het blad **gebeurtenissen** weer te geven. Voorbeelden van bewerkingen opnemen ophalen en opnieuw genereren toegangstoetsen, en de activering en de resolutie van waarschuwingsregels. Voor meer informatie over elke gebeurtenis, klikt u op de gebeurtenis in het blad **gebeurtenissen** .

Zie voor meer informatie over alle gebeurtenissen [gebeurtenissen weergeven en controlelogboeken bijhouden](../monitoring-and-diagnostics/insights-debugging-with-events.md).

De sectie **waarschuwingsregels** bevat het aantal waarschuwingen voor het exemplaar van de cache. Een waarschuwing regel kunt u uw exemplaar van de cache controleren en een e-mailbericht ontvangen wanneer een bepaalde waarde de drempel gedefinieerd in de regel bereikt. 

Waarschuwingsregels ongeveer elke vijf minuten worden geëvalueerd en wanneer een waarschuwing regel is geactiveerd, geen geconfigureerde meldingen worden verzonden. Waarschuwingsregels activeringen en meldingen worden niet onmiddellijk; verwerkt Er zijn mogelijk een vertraging van verschillende minuten voordat u een waarschuwing regel is geactiveerd en meldingen worden verzonden.

Waarschuwingsregels kunnen weergeven en instellen van het blad **Metrisch** voor een specifieke controle grafiek, of van het blad **waarschuwingsregels** .

Waarschuwingsregels hebben de volgende eigenschappen.

| De eigenschap waarschuwingsregels                 | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resource                            | De resource die door de huidige regel wordt geëvalueerd. Wanneer u een waarschuwing regel maakt uit een bestand Vgx. cache, is de cache voor de resource.                                                                                                                                                                                                                                                                                                                                                  |
| Naam                                | De naam die deze identificeert op de huidige regel binnen het huidige exemplaar van de cache.                                                                                                                                                                                                                                                                                                                                                                                       |
| Beschrijving                         | Optionele beschrijving van de huidige regel.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metrisch                              | De meetwaarde worden gevolgd door de huidige regel. Zie voor een lijst met cache aan de doelstellingen, beschikbaar maatstaven en rapportage intervallen.                                                                                                                                                                                                                                                                                                                                             |
| Voorwaarde                           | De voorwaardeoperator voor de huidige regel. Mogelijke opties zijn: groter is dan, groter is dan of gelijk is aan, minder dan, kleiner dan of gelijk aan                                                                                                                                                                                                                                                                                                                             |
| Drempelwaarde                           | De waarde die wordt gebruikt om te vergelijken met de meetwaarde met de operator opgegeven met de eigenschap voorwaarde. Afhankelijk van de meetwaarde, deze waarde wordt mogelijk bytes/tweede, bytes, percentage of aantal.                                                                                                                                                                                                                                                                                     |
| Termijn                              | Hiermee geeft u de periode aanduidt waarop de gemiddelde waarde van de meetwaarde voor de waarschuwingsregels vergelijking wordt gebruikt. Als de periode via het laatste uur is, kunt u voor de gemiddelde waarde van de meetwaarde voor het vorige uur interval, wordt gebruikt voor de vergelijking. Als u een melding ontvangen wilt wanneer de drempelwaarde in activiteit vanwege een Prikker is voldaan, klikt u vervolgens is een kortere periode nodig. Als u wilt worden gewaarschuwd wanneer er sprake is van continue activiteit boven de drempelwaarde, gebruikt u een langere periode. |
| E-mailservice en CO-beheerders | Waar is, wordt de service-beheerder en de collega beheerder gestuurd wanneer de melding wordt geactiveerd.                                                                                                                                                                                                                                                                                                                                                                    |
| De beheerder van de extra e-mail      | Optionele e-mailadres voor een aanvullende beheerder moeten meldingen ontvangen wanneer de melding wordt geactiveerd.                                                                                                                                                                                                                                                                                                                                                                    |

Er wordt slechts één melding verzonden per waarschuwingsregels activering. Zodra de drempelwaarde voor een regel is overschreden en wordt een melding verzonden, wordt de regel niet opnieuw geëvalueerd totdat de meetwaarde kleiner is dan de drempelwaarde. Als de meetwaarde vervolgens de drempel overschrijdt, de melding voor een opnieuw is geactiveerd en wordt een nieuwe melding verzonden.

Als u wilt weergeven op alle de waarschuwingsregels voor uw exemplaar van de cache, klikt u op het gedeelte **waarschuwingsregels** in het blad **Cache bestand Vgx.** . Als u wilt alleen de waarschuwingsregels met een specifieke metrische weergeven, Ga naar het blad **Metrisch** voor de grafiek die metrisch met.

![Waarschuwingsregels][redis-cache-alert-rules]

Een waarschuwing als regel wilt toevoegen, klikt u op **een waarschuwing voor toevoegen** van het blad **Metrisch** of van het blad **waarschuwingsregels** . 

Typ de gewenste regelcriteria in het blad van de regel **toevoegen een melding** en klik op **OK**. 

![Waarschuwing regel toevoegen][redis-cache-add-alert]

>[AZURE.NOTE] Wanneer een waarschuwing regel is gemaakt door te klikken op **een melding voor toevoegen** van het blad **Metrisch** , wordt alleen de cijfers weergegeven in de grafiek in die blade weergegeven in de vervolgkeuzelijst **Metrisch** . Wanneer u een waarschuwing regel is gemaakt door te klikken op **een melding voor toevoegen** van het blad **waarschuwingsregels** , zijn alle cache de doelstellingen beschikbaar in de vervolgkeuzelijst **Metrisch** .

Als een waarschuwing regel deze opgeslagen weergegeven op het blad **waarschuwingsregels** en klik op het blad **Metrisch** voor alle grafieken die de gegevens zijn die worden weergegeven in de huidige regel. Als u wilt een waarschuwing regel bewerken, klikt u op de naam van de huidige regel om weer te geven van het blad **Regel bewerken** . Van het blad **Regel bewerken** kunt u de eigenschappen van de regel bewerken, verwijderen of de huidige regel uitschakelen of opnieuw inschakelen van de regel als deze eerder hebt uitgeschakeld.

>[AZURE.NOTE] Wijzigingen die u in de eigenschappen van de regel aanbrengt kunnen het even voordat ze worden doorgevoerd op het blad **waarschuwingsregels** of het blad **Metrisch** duren.

Wanneer een waarschuwing regel is geactiveerd, een e-mailbericht wordt verzonden, afhankelijk van de configuratie van de huidige regel en een waarschuwingspictogram wordt weergegeven in het gedeelte **waarschuwingsregels** op het blad **Bestand Vgx. Cache** .

Een waarschuwing regel wordt beschouwd als worden omgezet wanneer de waarschuwing voorwaarde niet meer in waar resulteert. Als de voorwaarde waarschuwingsregels opgelost is, wordt het waarschuwingspictogram vervangen door een vinkje. Voor meer informatie over de waarschuwing activeringen en oplossingen, klikt u op het gedeelte **gebeurtenissen** op het blad **Cache bestand Vgx.** de gebeurtenissen weergeven op het blad **gebeurtenissen** .

Zie [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)voor meer informatie over waarschuwingen in Azure wordt aangegeven.

## <a name="metrics-on-the-redis-cache-blade"></a>Aan de doelstellingen op het blad Cache bestand Vgx.

Het **Bestand Vgx. Cache** blad bevat de volgende categorieën van de doelstellingen.

-   [Grafieken bewaken](#monitoring-charts)
-   [Gebruik grafieken](#usage-charts)

### <a name="monitoring-charts"></a>Grafieken bewaken

De sectie **controle** heeft **treffers en missers**, **haalt en stelt** **verbindingen**en **Totale opdrachten** grafieken.

![Grafieken bewaken][redis-cache-monitoring-part]

De **controle** -grafieken weergeven de doelstellingen van de volgende.

| Grafiek bewaken | Aan de doelstellingen cache     |
|------------------|-------------------|
| Treffers en mislukte  | Treffers in cache        |
|                  | Cachemissers      |
| Haalt en sets weergegeven    | Met deze eigenschap              |
|                  | Sets              |
| Verbindingen      | Verbonden Clients |
| Totale opdrachten   | Totale bewerkingen  |

Zie de volgende sectie voor [het weergeven van de doelstellingen en aan de doelstellingen grafieken aanpassen](#how-to-view-metrics-and-customize-charts) voor informatie over het weergeven van de doelstellingen en aanpassen van de afzonderlijke grafieken in deze sectie.

### <a name="usage-charts"></a>Gebruik grafieken

De sectie **Gebruik** heeft **Bestand Vgx. Server laden**, **Geheugengebruik** **Netwerkbandbreedte**en **CPU-gebruik** grafieken en de **prijzen laag** voor de cache-exemplaar wordt ook weergegeven.

![Gebruik grafieken][redis-cache-usage-part]

De **prijzen laag** toont de cache prijzen trapsgewijs en kan worden gebruikt op [schaal](cache-how-to-scale.md) van de cache naar een ander prijzen laag.

De grafieken **Gebruik** weergeven de doelstellingen van de volgende.

| Gebruik grafiek       | Aan de doelstellingen cache |
|-------------------|---------------|
| Bestand Vgx belasting van de Server. | Belasting van de server   |
| Geheugengebruik      | Gebruikte geheugen   |
| Netwerkbandbreedte | Cache schrijven   |
| CPU-gebruik         | CPU           |

Zie de volgende sectie voor [het weergeven van de doelstellingen en aan de doelstellingen grafieken aanpassen](#how-to-view-metrics-and-customize-charts) voor informatie over het weergeven van de doelstellingen en aanpassen van de afzonderlijke grafieken in deze sectie.
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



