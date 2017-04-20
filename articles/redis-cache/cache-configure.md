<properties 
    pageTitle="Het configureren van Azure bestand Vgx. Cache | Microsoft Azure"
    description="Meer informatie over de standaard bestand Vgx. configuratie voor Azure bestand Vgx. Cache en leer hoe u uw Azure bestand Vgx. Cache-sessies configureren"
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
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Het configureren van Azure-Cache bestand Vgx.

In dit onderwerp wordt beschreven hoe u bekijken en bijwerken van de configuratie voor uw Azure bestand Vgx. Cache exemplaren en de standaard bestand Vgx. serverconfiguratie naar exemplaren van Azure bestand Vgx. Cache behandelt.

>[AZURE.NOTE] Zie voor meer informatie over het configureren en gebruiken van de premiumfuncties van cache [permanente voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-persistence.md), [configureren voor een Premium Azure bestand Vgx. Cache cluster](cache-how-to-premium-clustering.md)en [het configureren van Virtual Network ondersteuning voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Bestand Vgx. cache-instellingen configureren

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure bestand Vgx. Cache biedt de volgende instellingen op het blad **Instellingen** .

![Bestand Vgx Cache-instellingen.](./media/cache-configure/redis-cache-settings.png)



-   [Ondersteuning en probleemoplossing van instellingen](#support-amp-troubleshooting-settings)
-   [Algemene instellingen](#general-settings)
    -   [Eigenschappen](#properties)
    -   [Toegangstoetsen](#access-keys)
    -   [Geavanceerde instellingen](#advanced-settings)
    -   [Bestand Cache Advisor Vgx.](#redis-cache-advisor)
-   [Schaalinstellingen](#scale-settings)
    -   [Prijzen van laag](#pricing-tier)
    -   [Bestand Vgx clustergrootte.](#cluster-size)
-   [Instellingen voor het beheer van gegevens](#data-management-settings)
    -   [Bestand gegevens permanente Vgx.](#redis-data-persistence)
    -   [Importeren/exporteren](#importexport)
-   [Beheerinstellingen](#administration-settings)
    -   [Start opnieuw op](#reboot)
    -   [Updates plannen](#schedule-updates)
-   [Instellingen diagnostische gegevens](#diagnostics-settings)
-   [Netwerkinstellingen](#network-settings)
-   [Resource-beheerinstellingen](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Ondersteuning en probleemoplossing van instellingen

De instellingen in de sectie **ondersteuning + probleemoplossing** kunt u met opties voor het oplossen van problemen met de cache.

![Ondersteuning voor + problemen oplossen](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klik op **Diagnose en oplossen van problemen** te verstrekken met veelvoorkomende problemen en strategieën voor het oplossen van deze.

Klik op **log activiteit** om acties in de cache weer te geven. U kunt ook filters gebruiken om uit te vouwen deze weergave als u wilt opnemen van andere bronnen. Zie voor meer informatie over het werken met controlelogboeken [gebeurtenissen weergeven en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) en [bewerkingen met resourcemanager controleren](../resource-group-audit.md). Zie voor meer informatie over het controleren van Azure bestand Vgx. Cache gebeurtenissen [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

**Status van de resource** toekijkt van de resource en kunt u zien als deze wordt uitgevoerd zoals verwacht. Zie voor meer informatie over de service van de servicestatus Azure Resource [Servicestatus van Azure Resourceoverzicht](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Status van de resource kan momenteel rapporteren van de status van de Azure bestand Vgx. Cache-exemplaren die zijn ingesloten in een virtueel netwerk. Zie voor meer informatie [alle cache functies werken als een cache in een VNET hostingprovider?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Klik op **nieuwe ondersteuning verzoek** om een ondersteuningsverzoek voor de cache te openen.

## <a name="general-settings"></a>Algemene instellingen

De instellingen in het gedeelte **Algemeen** kunt u openen en configureren van de volgende instellingen voor de cache.

![Algemene instellingen](./media/cache-configure/redis-cache-general-settings.png)

-   [Eigenschappen](#properties)
-   [Toegangstoetsen](#access-keys)
-   [Geavanceerde instellingen](#advanced-settings)
-   [Bestand Cache Advisor Vgx.](#redis-cache-advisor)

### <a name="properties"></a>Eigenschappen

Klik op **Eigenschappen** om informatie over de cache, inclusief de cache eindpunt en poorten weer te geven.

![Bestand Vgx Cache eigenschappen.](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Toegangstoetsen

Klik op **toegangstoetsen** als u wilt weergeven of de toegangstoetsen voor de cache opnieuw te genereren. Deze toetsen worden samen met de hostnaam en poorten van het blad **Eigenschappen** gebruikt door de clients verbinding maken met de cache.

![Bestand Vgx toegangstoetsen Cache.](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Geavanceerde instellingen

De volgende instellingen zijn configureren op het blad **Geavanceerde instellingen** .

-   [Access-poorten](#access-ports)
-   [Maxmemory-beleid en maxmemory-gereserveerde](#maxmemory-policy-and-maxmemory-reserved)
-   [Keyspace meldingen (geavanceerde instellingen)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Access-poorten

Niet-SSL-toegang is standaard uitgeschakeld voor nieuwe cache. Om de niet-SSL-poort schakelen, klikt u op **Nee** voor **toegang toestaan alleen via SSL** in de **Geavanceerde instellingen blade** en klik vervolgens op **Opslaan**.

![Cache-poorten bestand Vgx.](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Maxmemory-beleid en maxmemory-gereserveerde

De **Maxmemory beleid** en **maxmemory-gereserveerde** instellingen op het blad **Geavanceerde instellingen** de geheugen beleidsregels configureren voor de cache. De **maxmemory -** beleidsinstelling configureert het beleid verwijdering voor de cache en het geheugen gereserveerd voor niet-cache processen **maxmemory gereserveerd** configureert.

![Bestand Vgx Cache Maxmemory beleid.](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory beleid** kunt u kiezen uit de volgende beleidsitems voor een verwijdering.

-   vluchtige-recentelijk - dit is de standaardwaarde.
-   AllKeys-recentelijk
-   vluchtige willekeurig
-   AllKeys-willekeurig
-   vluchtige-ttl
-   noeviction

Zie [een verwijdering beleid](http://redis.io/topics/lru-cache#eviction-policies)voor meer informatie over het beleid van maxmemory.

De instelling **maxmemory gereserveerd** configureert de hoeveelheid geheugen in MB die is gereserveerd voor niet-cache bewerkingen zoals herhaling tijdens een overname. Dit kan ook worden gebruikt als er een hoge fragmentatie-breedteverhouding. Als u deze waarde, kunt u een consistenter bestand Vgx. server ervaring hebben wanneer uw laden varieert. Deze waarde moet worden ingesteld voor werkbelasting waarin dik zijn schrijven hoger. Wanneer het geheugen is gereserveerd voor deze bewerkingen hoeft u niet beschikbaar voor de opslag van gegevens in de cache.

>[AZURE.IMPORTANT] De instelling **maxmemory gereserveerd** is alleen beschikbaar voor standaard en Premium in cache opgeslagen.

### <a name="keyspace-notifications-advanced-settings"></a>Keyspace meldingen (geavanceerde instellingen)

Bestand Vgx. keyspace meldingen op het blad **Geavanceerde instellingen** zijn geconfigureerd. Keyspace meldingen toestaan clients meldingen wilt ontvangen wanneer bepaalde gebeurtenissen.

![Bestand Vgx Cache geavanceerde instellingen.](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace meldingen en de instelling voor **melding-keyspace-gebeurtenissen** zijn alleen beschikbaar voor Standard en Premium in cache opgeslagen.

Zie [Bestand Vgx. Keyspace meldingen](http://redis.io/topics/notifications)voor meer informatie. Voorbeeld van code, raadpleegt u het bestand [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) in de steekproef [Hallo allemaal](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Bestand Cache Advisor Vgx.

Het blad **aanbevelingen** geeft aanbevelingen voor de cache. Tijdens normale bewerkingen worden geen aanbevelingen weergegeven. 

![Aanbevelingen](./media/cache-configure/redis-cache-no-recommendations.png)

Als er geen voorwaarden optreden tijdens de bewerkingen van de cache zoals hoog geheugengebruik, netwerkbandbreedte of belasting van de server, wordt een melding op het blad **Bestand Vgx. Cache** weergegeven.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations-alert.png)

Klik op het blad **aanbevelingen** vindt u meer informatie.

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations.png)

U kunt deze gegevens op de secties die [controle diagrammen](cache-how-to-monitor.md#monitoring-charts) en [grafieken voor gebruik](cache-how-to-monitor.md#usage-charts) van het blad **Bestand Vgx. Cache** controleren.

Elke prijzen laag heeft verschillende limieten voor de clientverbindingen, geheugen en bandbreedte. Als de cache maximale capaciteit voor deze gegevens gedurende een langere periode van tijd nadert, wordt een aanbeveling gemaakt. Zie voor meer informatie over de limieten laten nakijken door het hulpmiddel voor **aanbevelingen** voor de doelstellingen en, in de volgende tabel.

| Bestand Cache metrisch Vgx.      | Zie voor meer informatie                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Netwerkbandbreedte gebruikt | [Prestaties van de cache - bandbreedte](cache-faq.md#cache-performance) |
| Verbonden clients       | [Standaard bestand Vgx. serverconfiguratie - maxclients](#maxclients)            |
| Belasting van de server             | [Gebruik grafieken - Server laden bestand Vgx.](cache-how-to-monitor.md#usage-charts)  |
| Geheugengebruik            | [Prestaties van de cache - grootte](cache-faq.md#cache-performance)                |

Upgraden van de cache, klikt u op **Nu bijwerken** om te wijzigen van de [laag prijzen](#pricing-tier) en schaal van de cache. Zie voor meer informatie over het kiezen van een prijzen laag [welke bestand Vgx. Cache aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Schaalinstellingen

De instellingen in de sectie **schaal** kunt u openen en configureren van de volgende instellingen voor de cache.

![Netwerk](./media/cache-configure/redis-cache-scale.png)

-   [Prijzen van laag](#pricing-tier)
-   [Bestand Vgx clustergrootte.](#cluster-size)

### <a name="pricing-tier"></a>Prijzen van laag

Klik op **prijzen laag** weergeven of wijzigen van de prijzen laag voor de cache. Zie voor meer informatie over de schaal, [hoe u de schaal Azure bestand Vgx. Cache](cache-how-to-scale.md).

![Bestand Vgx Cache prijzen van laag.](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Bestand Vgx clustergrootte.

Klik op **(PREVIEW) bestand Vgx. clustergrootte** als u wilt wijzigen van de clustergrootte voor een voorlopig premium cache met cluster ingeschakeld.

>[AZURE.NOTE] Houd er rekening mee dat terwijl de laag Azure bestand Vgx. Cache Premium voor algemene beschikbaarheid vrijgegeven, de grootte van bestand Vgx. Cluster functie is momenteel in de proefversie.

![Bestand Vgx clustergrootte.](./media/cache-configure/redis-cache-redis-cluster-size.png)

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in het tekstvak **Shard tellen** en klik op **OK** om op te slaan.

>[AZURE.IMPORTANT] Bestand Vgx. cluster is alleen beschikbaar voor Premium cache. Zie [hoe u configureert voor een Premium Azure bestand Vgx. Cache cluster](cache-how-to-premium-clustering.md)voor meer informatie.


## <a name="data-management-settings"></a>Instellingen voor het beheer van gegevens

De instellingen in de sectie **gegevens beheren** kunt u openen en configureren van de volgende instellingen voor de cache.

![Gegevensbeheer](./media/cache-configure/redis-cache-data-management.png)

-   [Bestand gegevens permanente Vgx.](#redis-data-persistence)
-   [Importeren/exporteren](#importexport)

### <a name="redis-data-persistence"></a>Bestand gegevens permanente Vgx.

Klik op **bestand Vgx gegevens permanente** als u wilt inschakelen, uitschakelen of permanente gegevens voor de premium-cache configureren.

![Bestand gegevens permanente Vgx.](./media/cache-configure/redis-cache-persistence-settings.png)

Om te schakelen bestand Vgx. permanente, klikt u op **ingeschakeld** om in te schakelen RDB (database bestand Vgx.) back-up. Als u wilt uitschakelen bestand Vgx. permanente, klikt u op **uitgeschakeld**.

Om het back-interval configureren, selecteert u een **Back-frequentie** in de vervolgkeuzelijst. Kunt kiezen uit **15 minuten**, **30 minuten** **60 minuten**, **6 uur**, **12 uur**en **24 uur**. Dit interval begint te tellen omlaag nadat de vorige back-up is voltooid en wanneer deze verstrijkt een nieuwe back-up wordt gestart.

Op **Opslag-Account** als u wilt selecteren de rekening opslagruimte wilt gebruiken en kiest u de **primaire sleutel** of de **secundaire sleutel** uit de vervolgkeuzelijst **Opslag-toets** gebruiken. Moet u een account opslagruimte in hetzelfde gebied, als de cache en een **Premium-opslag** -account wordt aanbevolen, omdat de premium-opslag heeft sneller worden verwerkt. Als de sleutel opslagruimte voor uw account permanente wordt opnieuw gegenereerd, moet u de gewenste toets opnieuw kiezen in de vervolgkeuzelijst **Storage Key** .

Klik op **OK** om de configuratie permanente opslaan.

>[AZURE.IMPORTANT] Bestand Vgx. gegevens permanente is alleen beschikbaar voor Premium cache. Lees [hoe u het permanente voor een Premium Azure bestand Vgx. Cache-instellingen configureren](cache-how-to-premium-persistence.md)voor meer informatie.

### <a name="importexport"></a>Importeren/exporteren

Importeren/exporteren is een Azure bestand Vgx. Cache data management bewerking waarmee u gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache door te importeren en exporteren van een momentopname van het bestand Vgx. Cache Database (RDB) uit een premium-cache naar een blob pagina in een Azure opslag-Account. Hiermee kunt u migreren tussen verschillende exemplaren van Azure bestand Vgx. Cache of vullen de cache met gegevens voor gebruik.

Importeren kan worden gebruikt om bestand Vgx. compatibele RDB bestanden die vanaf een willekeurige bestand Vgx. server die wordt uitgevoerd in een cloud of -omgeving, inclusief bestand Vgx. waarop Linux, Windows, of cloud providers zoals Amazon Web Services en anderen weer te geven. Het importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache laadt de bestanden RDB van Azure opslagruimte in het geheugen en vervolgens de toetsen ingevoegd in de cache.

Exporteren kunt u de gegevens die zijn opgeslagen in Azure bestand Vgx. Cache naar het bestand Vgx. compatibele RDB-bestanden die worden geëxporteerd. U kunt deze functie gebruiken om gegevens te verplaatsen vanaf één exemplaar van de Azure bestand Vgx. Cache naar een andere of naar een ander bestand Vgx. server. Tijdens het exportproces dat een tijdelijk bestand wordt gemaakt op de VM die het Azure bestand Vgx. Cache server-exemplaar is gehost en het bestand wordt geüpload naar de aangewezen opslag-account. Wanneer de exportbewerking is voltooid met een van beide de status van slagen of mislukt, wordt het tijdelijke bestand verwijderd.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor Premium laag cache. Zie voor meer informatie en instructies, [importeren en exporteren van gegevens in de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Beheerinstellingen

De instellingen in het onderdeel **beheer** kunt u de volgende beheertaken uitvoeren voor de premium-cache. 

![Beheer](./media/cache-configure/redis-cache-administration.png)

-   [Start opnieuw op](#reboot)
-   [Updates plannen](#schedule-updates)

>[AZURE.IMPORTANT] De instellingen in deze sectie zijn alleen beschikbaar voor Premium laag cache.

### <a name="reboot"></a>Start opnieuw op

Het blad **opnieuw opstarten** kunt u een of meer knooppunten van de cache opnieuw. Hiermee kunt u uw toepassing voor tolerantie bij een storing testen.

![Start opnieuw op](./media/cache-configure/redis-cache-reboot.png)

Als u een premium-cache met cluster ingeschakeld hebt, kunt u welke shards van de cache op te starten.

![Start opnieuw op](./media/cache-configure/redis-cache-reboot-cluster.png)

Start opnieuw op een of meer knooppunten van de cache, selecteert u de gewenste knooppunten en klik op **Start opnieuw op**. Als u een premium-cache met cluster ingeschakeld hebt, selecteert u de shard(s) te starten en klik vervolgens op **Start opnieuw op**. Na een paar minuten, het geselecteerde knooppunten opnieuw opstarten en er wordt een back-online later een paar minuten.

>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor Premium laag cache. Zie voor meer informatie en instructies, [beheer van de Azure bestand Vgx. Cache - start opnieuw op](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Updates plannen

Het blad **planning updates** kunt u een onderhoudsvenster voor bestand Vgx. serverupdates voor de cache aanwijzen. 

>[AZURE.IMPORTANT] Houd er rekening mee dat het onderhoudsvenster is alleen van toepassing op bestand Vgx. serverupdates en niet op een Azure bijwerkt of bijgewerkt naar het besturingssysteem van de VMs die als de cache host.

![Updates plannen](./media/cache-configure/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren van de gewenste dagen opgeven van het onderhoud venster start uur per dag en klik op **OK**. Houd er rekening mee dat het onderhoud venster in UTC is. 

>[AZURE.IMPORTANT] Updates plannen is alleen beschikbaar voor Premium laag cache. Zie voor meer informatie en instructies, [beheer van de Azure bestand Vgx. Cache - updates plannen](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Instellingen diagnostische gegevens

De sectie **diagnostische hulpprogramma's** kunt u diagnostische gegevens configureren voor uw bestand Vgx. Cache.

![Diagnostische hulpprogramma 's](./media/cache-configure/redis-cache-diagnostics.png)

Klik op **diagnose** [de opslag-account configureren](cache-how-to-monitor.md#enable-cache-diagnostics) cache diagnostische gegevens opgeslagen.

![Bestand Vgx Cache diagnostische gegevens.](./media/cache-configure/redis-cache-diagnostics-settings.png)

Klik op **bestand Vgx. aan de doelstellingen** [Statistieken bekijken](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) voor uw cache en **waarschuwingsregels** voor het [instellen van waarschuwingsregels](cache-how-to-monitor.md#operations-and-alerts).

Zie voor meer informatie over Azure bestand Vgx. Cache diagnostische hulpprogramma's, [het controleren van Azure bestand Vgx. Cache](cache-how-to-monitor.md).


## <a name="network-settings"></a>Netwerkinstellingen

De instellingen in de sectie **netwerk** kunt u openen en configureren van de volgende instellingen voor de cache.

![Netwerk](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtuele netwerkinstellingen zijn alleen beschikbaar voor de premium-cache die zijn geconfigureerd met VNET ondersteuning tijdens het maken van de cache. Voor informatie over het maken van een premium-cache met VNET ondersteuning en de instellingen ervan wordt bijgewerkt, raadpleegt u [het configureren van virtuele netwerk ondersteuning voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Resource-beheerinstellingen

![Resourcebeheer](./media/cache-configure/redis-cache-resource-management.png)

De sectie **Tags** kunt u uw resources ordenen. Zie [werken met tags om u te organiseren van uw Azure resources](../resource-group-using-tags.md)voor meer informatie.

De sectie **vergrendelen als** kunt u een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie uit per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. Zie [vergrendelen resources met Azure resourcemanager](../resource-group-lock-resources.md)voor meer informatie.

De sectie **gebruikers** biedt ondersteuning voor Rolgebaseerd toegangsbeheer (RBAC) in de Azure-portal om te voldoen aan de vereisten van hun access management gewoon en nauwkeuriger organisaties. Zie [Rolgebaseerd toegangsbeheer in de portal van Azure](../active-directory/role-based-access-control-configure.md)voor meer informatie.

Klik op **exporteren sjabloon** maken en exporteren van een sjabloon van uw geïmplementeerd resources voor toekomstige implementaties. Zie [Deploy resources met Azure resourcemanager sjablonen](../resource-group-template-deploy.md)voor meer informatie over het werken met sjablonen.

## <a name="default-redis-server-configuration"></a>Standaard serverconfiguratie bestand Vgx.

Nieuwe exemplaren van Azure bestand Vgx. Cache zijn geconfigureerd met de volgende bestand Vgx. configuratie standaardwaarden.

>[AZURE.NOTE] De instellingen in deze sectie kunnen niet worden gewijzigd met de `StackExchange.Redis.IServer.ConfigSet` methode. Als deze methode wordt aangeroepen met een van de opdrachten in deze sectie, gegenereerd een uitzondering ongeveer als volgt uit:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Alle waarden die geconfigureerd, zoals **max-geheugen-beleid worden**, kunnen worden geconfigureerd door de Azure portal of opdrachtregel beheerprogramma's zoals Azure CLI of PowerShell.

|Instelling|Standaardwaarde|Beschrijving|
|---|---|---|
|databases|16|Het standaardaantal databases 16 is maar u kunt een ander nummer op basis van de prijzen laag configureren. <sup>1</sup> de standaard-database is DB 0, kunt u een andere naam op een per verbinding soort_jaar met `connection.GetDatabase(dbid)` waar database-id is een getal tussen `0` en `databases - 1`.|
|maxclients|Is afhankelijk van de prijzen fase<sup>2</sup>|Dit is het maximum aantal verbonden clients toegestaan op hetzelfde moment. Als de limiet is bereikt wordt bestand Vgx. de nieuwe verbindingen verzenden van een fout 'maximale aantal clients bereikt' gesloten.|
|maxmemory-beleid|vluchtige recentelijk|Maxmemory beleid is de instelling voor hoe bestand Vgx. wordt selecteren wat u moet verwijderen wanneer maxmemory (de grootte van de cache die dat u hebt geselecteerd bij het maken van de cache) is bereikt. De standaardinstelling is met Azure bestand Vgx. Cache vluchtige-recentelijk, waarmee u de toetsen verwijdert met een set met een algoritme Recentelijk verloopt. Deze instelling kan worden geconfigureerd in de portal van Azure. Zie voor meer informatie [Maxmemory-beleid en maxmemory-gereserveerde](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-voorbeelden|3|Recentelijk en minimale TTL algoritmen zijn niet nauwkeurig algoritmen maar bij benadering berekend machtreeks algoritmen (om op te slaan geheugen), zodat u kunt ook de grootte van de steekproef om te controleren. Bijvoorbeeld voor standaard wordt bestand Vgx. drie sleutels controleren en kies die is minder recent.|
|LUA-termijn|5.000|Maximale tijd van de uitvoering van een script Lua (in milliseconden). Als de maximale tijd die kan worden uitgevoerd is bereikt bestand Vgx. wordt Meld u aan dat een script zich nog in uitvoering nadat het maximale aantal toegestane tijd en beantwoorden voor query's met een fout wordt gestart.|
|LUA-gebeurtenis-limiet|500|Dit is de maximale grootte van script gebeurtenissenwachtrij.|
|client-uitvoer-buffer-limiet normalclient-uitvoer-buffer-limiet pubsub|0 0 032mb 8mb 60|De client uitvoer buffer limieten kunnen worden gebruikt afdwingen verbreken van clients die niet van gegevens van de server snel genoeg om welke reden lezen zijn (een algemene reden is dat een publicatie/Sub-client berichten zo snel als deze worden kunnen de uitgever produceren kan niet kunt gebruiken). Zie [http://redis.io/topics/clients](http://redis.io/topics/clients)voor meer informatie.|

<a name="databases"></a>
<sup>1</sup> De limiet voor `databases` verschilt voor elke Azure bestand Vgx. in Cache laag prijzen en kan worden ingesteld op cache maken. Als er geen `databases` instelling wordt opgegeven tijdens het maken van cache de standaard 16.

-   Eenvoudige en standaard cache
    -   C0 (250 MB) cache - maximaal 16-databases
    -   C1 (1 GB) cache - maximaal 16-databases
    -   C2 (2,5 GB) cache - maximaal 16-databases
    -   C3 (6 GB) cache - maximaal 16-databases
    -   C4 (13 GB) cache - snel aan de 32-databases
    -   C5 (26 GB) cache - maximaal 48 databases
    -   C6 (53 GB) cache - maximaal 64-databases
-   In de cache opgeslagen Premium
    -   P1 (6 GB - 60 GB) - maximaal 16-databases
    -   P2 (13 GB - 130 GB) - snel aan de 32-databases
    -   P3 (26 GB - 260 GB) - maximaal 48 databases
    -   P4 (53 GB - 530 GB) - maximaal 64-databases
    -   Alle premium cache met bestand Vgx. cluster ingeschakeld - bestand Vgx. cluster biedt alleen ondersteuning voor gebruik van database 0 zodat het `databases` beperken voor elke cache premium met bestand Vgx. cluster ingeschakeld effectief 1 is en de opdracht [Select](http://redis.io/commands/select) is niet toegestaan. Zie voor meer informatie [moet ik breng eventuele wijzigingen aan mijn clienttoepassing cluster?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] De `databases` instelling kan worden geconfigureerd alleen tijdens de cache maken en alleen met PowerShell CLI of andere clients management. Voor een voorbeeld van het configureren van `databases` tijdens het maken van de cache via PowerShell, raadpleegt u [Nieuw AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` verschilt voor elke Azure bestand Vgx. in Cache prijzen van laag.

-   Eenvoudige en standaard cache
    -   C0 (250 MB) cache - maximaal 256 verbindingen
    -   C1 (1 GB) cache - maximaal 1000 verbindingen
    -   C2 (2,5 GB) cache - maximaal 2000 verbindingen
    -   C3 (6 GB) cache - maximaal 5.000 verbindingen
    -   C4 (13 GB) cache - maximaal 10.000 verbindingen
    -   C5 (26 GB) cache - maximaal 15.000 verbindingen
    -   C6 (53 GB) cache - maximaal 20.000 verbindingen
-   In de cache opgeslagen Premium
    -   P1 (6 GB - 60 GB) - maximaal 7.500 verbindingen
    -   P2 (13 GB - 130 GB) - maximaal 15.000 verbindingen
    -   P3 (26 GB - 260 GB) - maximaal 30.000 verbindingen
    -   P4 (53 GB - 530 GB) - maximaal 40.000 verbindingen

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Bestand Vgx. opdrachten die niet worden ondersteund in de Cache van Azure bestand Vgx.

>[AZURE.IMPORTANT] De volgende opdrachten zijn uitgeschakeld omdat configuratie en beheer van Azure bestand Vgx. Cache exemplaren wordt beheerd door Microsoft. Als u probeert te roepen ze, ontvangt u een foutbericht vergelijkbaar met `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  ZOEKCONFIGURATIE
>-  FOUTEN OPSPOREN
>-  MIGREREN
>-  OPSLAAN
>-  AFSLUITEN
>-  SLAVEOF
>-  CLUSTER - opdrachten zijn uitgeschakeld schrijven, maar alleen-lezen Cluster opdrachten zijn toegestaan.

Zie voor meer informatie over opdrachten bestand Vgx., [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Bestand Vgx console.

U kunt veilig actie-opdrachten in de Cache van Azure bestand Vgx. exemplaren met behulp van het **Bestand Vgx. Console**, die beschikbaar voor de standaard is en Premium in cache opgeslagen.

>[AZURE.IMPORTANT] De Console bestand Vgx. werkt niet met VNET, cluster, en andere dan 0-databases. 
>
>-  [VNET](cache-how-to-premium-vnet.md) - wanneer de cache deel uit van een VNET maakt, alleen-clients in het VNET hebben toegang tot de cache. Omdat het bestand Vgx.-Console gebruikmaakt van het bestand Vgx. cli.exe-client die worden gehost op VMs die geen deel uitmaken van uw VNET, kan deze verbinden met uw cache.
>-  [Cluster](cache-how-to-premium-clustering.md) - Console van het bestand Vgx. werkt met het bestand Vgx. cli.exe-client cluster op dit moment niet worden ondersteund. Het hulpprogramma bestand Vgx. cli in de [vastloopt](http://redis.io/download) tak van de bibliotheek bestand Vgx. bij GitHub implementeert basisondersteuning wanneer de slag met de `-c` schakelen. Zie [afspelen met het cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) op [http://redis.io](http://redis.io) in het [bestand Vgx. cluster zelfstudie](http://redis.io/topics/cluster-tutorial)voor meer informatie.
>-  De Console bestand Vgx. maakt een nieuwe verbinding met de database 0 telkens wanneer die u een opdracht verzendt. U kunt niet gebruiken de `SELECT` opdracht om te selecteren van een andere database, omdat de database wordt teruggezet op 0 met elke opdracht. Zie voor informatie over het uitvoeren van bestand Vgx. opdrachten, waaronder wijzigen in een andere database, [hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)

Voor toegang tot de Console bestand Vgx. op **Console** van het blad **Bestand Vgx. Cache** .

![Bestand Vgx console.](./media/cache-configure/redis-console-menu.png)

Als u wilt verlenen opdrachten exemplaar van uw cache, alleen te typen in de gewenste opdracht in de console.

![Bestand Vgx console.](./media/cache-configure/redis-console.png)

Zie de vorige sectie voor het [bestand Vgx. opdrachten die niet worden ondersteund in Azure bestand Vgx. Cache](#redis-commands-not-supported-in-azure-redis-cache) voor lijst van bestand Vgx. opdrachten die worden uitgeschakeld voor Azure bestand Vgx. Cache. Zie voor meer informatie over opdrachten bestand Vgx., [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>De cache verplaatsen naar een nieuw abonnement

U kunt de cache verplaatsen naar een nieuw abonnement door te klikken op **verplaatsen**.

![Verplaats de Cache bestand Vgx.](./media/cache-configure/redis-cache-move.png)

Voor informatie over resources uit één resourcegroep naar een andere en uit één abonnement naar de andere verplaatst, Zie [verplaatsen van resources naar nieuwe resourcegroep-abonnement](../resource-group-move-resources.md).

## <a name="next-steps"></a>Volgende stappen
-   Zie voor meer informatie over het werken met opdrachten bestand Vgx. [hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands).
