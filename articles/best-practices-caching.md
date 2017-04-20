<properties
   pageTitle="Richtlijnen caching | Microsoft Azure"
   description="Richtlijnen voor het in cache opslaan om te verbeteren de prestaties en schaalbaarheid."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>In de cache richtlijnen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

In cache opslaan, is een algemene techniek die ernaar verbeteren de prestaties en schaalbaarheid van een systeem. Dit doet u dit door tijdelijk veelgebruikte gegevens kopiëren naar snel opslag dat zich dicht bij de toepassing. Als dit snel gegevensopslag dichter naar de toepassing dan de oorspronkelijke bron bevindt, kunt vervolgens caching aanzienlijk verbeteren antwoord tijden voor clienttoepassingen door gegevens sneller treden.

Caching wordt optimaal worden weergegeven wanneer een clientexemplaar herhaaldelijk dezelfde gegevens leest, met name als alle volgende voorwaarden van toepassing op de oorspronkelijke gegevensopslag:
- Relatief statische blijft.
- Het is traag vergeleken met de snelheid van de cache.
- Het is onderhevig aan een hoog niveau van een conflict.
- Dit is ver wanneer netwerklatentie kan leiden tot access traag verlopen.

## <a name="caching-in-distributed-applications"></a>In cache opslaan in gedistribueerde toepassingen

Gedistribueerde toepassingen implementeren meestal een of beide van de volgende strategieën wanneer caching van gegevens:

- Een persoonlijke cache, waar gegevens lokaal is opgeslagen op de computer waarop een exemplaar van een toepassing of service wordt gebruikt.
- Een gedeelde cache, fungeren als een algemene bron kan worden geopend door meerdere processen en/of machines gebruiken.

In beide gevallen kan caching worden uitgevoerd aan de clientzijde en/of servers. Aan de clientzijde caching wordt uitgevoerd door het proces waarmee de gebruikersinterface voor een systeem, zoals een webbrowser of -bureaubladtoepassing hebt.
Caching op de server wordt uitgevoerd door het proces waarmee de business-services die extern worden uitgevoerd.

### <a name="private-caching"></a>Privé caching

Het belangrijkste type cache is een archief in het geheugen. Er heeft in de adresruimte van één proces gehouden en ze rechtstreeks worden gebruikt door de code die wordt uitgevoerd in het proces. Dit soort cache is zeer snelle toegang. Dit kan ook om er zelf een uiterst effectieve betekent voor het opslaan van kleine hoeveelheden statische gegevens, omdat de grootte van een cache meestal wordt beperkt door het volume van geheugen die beschikbaar is op de computer aan waarop het proces.

Als u meer informatie wilt dan fysiek mogelijk in het geheugen in cache, kunt u in de cache opgeslagen gegevens schrijven naar het lokale bestandssysteem. Dit langzamer voor toegang tot dan gegevens die in het geheugen wordt gehouden, maar nog steeds moet worden sneller en meer betrouwbaar is dan het ophalen van gegevens in een netwerk.

Als er meerdere exemplaren van een toepassing die gebruikmaakt van dit model tegelijkertijd worden uitgevoerd, heeft elk toepassingsexemplaar een eigen onafhankelijke cache vasthouden van een eigen kopie van de gegevens.

Als het ware een cache een momentopname van de oorspronkelijke gegevens op een gegeven moment in het verleden. Als deze gegevens niet statisch is, is het waarschijnlijk dat exemplaren van de andere toepassing houdt u verschillende versies van de gegevens in de cache. Dezelfde query uitgevoerd door deze exemplaren kan daarom verschillende resultaten retourneren, zoals wordt weergegeven in de afbeelding 1.

![Gebruik van een cache in het geheugen in verschillende exemplaren van een toepassing](media/best-practices-caching/Figure1.png)

_Afbeelding 1: Een cache in het geheugen in verschillende exemplaren van een toepassing gebruiken_

### <a name="shared-caching"></a>Gedeeld in cache opslaan

Met behulp van een gedeelde cache kunt verhelpen dat gegevens in elke cache zich verschillen kunnen bij in het geheugen caching voordoen kan bezwaren. Gedeelde caching zorgt ervoor dat exemplaren van de andere toepassing dezelfde weergave in de cache opgeslagen gegevens zien. Dit gebeurt door te zoeken van de cache op een andere locatie, meestal wordt gehost als onderdeel van een afzonderlijke service, zoals wordt weergegeven in de afbeelding 2.

![Gebruik van een gedeelde cache](media/best-practices-caching/Figure2.png)

_Afbeelding 2: Gebruik een gedeelde cache_

Een belangrijk voordeel van de gedeelde in de cache benadering is de schaalbaarheid biedt. Veel gedeelde cache services worden geïmplementeerd met behulp van een cluster van servers en software die Hiermee stelt u de gegevens over het cluster op transparante wijze gebruiken. Een toepassingsexemplaar wordt gewoon een aanvraag verzonden naar de cache-service.
De onderliggende infrastructuur is verantwoordelijk voor het bepalen van de locatie van de gegevens in de cache in het cluster. U kunt de cache eenvoudig schalen door meer servers toe te voegen.

Er zijn twee belangrijke nadelen van het gedeelde in de cache methode:
- De cache is langzamer voor toegang tot omdat deze is niet langer die lokaal zijn opgeslagen op elk toepassingsexemplaar.
- De vereiste een afzonderlijke cache-service implementeren mogelijk complexiteit toevoegen aan de oplossing.

## <a name="considerations-for-using-caching"></a>Overwegingen bij het gebruik van de cache opslaan

In de volgende secties worden de overwegingen voor het ontwerpen en gebruiken van een cache uitgebreider beschreven.

### <a name="decide-when-to-cache-data"></a>Bepalen wanneer u kunt gegevens in cache

Caching kunt aanzienlijk verbeteren prestaties en schaalbaarheid beschikbaarheid. Hoe meer gegevens die u hebt en des te groter het aantal gebruikers dat toegang hebben tot deze gegevens, des te groter de voordelen moeten van caching worden. Dat komt doordat caching Hiermee reduceert u de latentie en conflicten dat is gekoppeld aan het afhandelen van grote hoeveelheden gelijktijdige aanvragen in de oorspronkelijke gegevensopslag.

Een database misschien bijvoorbeeld een beperkt aantal verbindingen ondersteunen. Gegevens ophalen uit een gedeelde cache, echter maakt in plaats van de onderliggende database, het mogelijk een clienttoepassing voor toegang tot deze gegevens ook als het aantal beschikbare verbindingen is momenteel leeg. Bovendien als de database niet beschikbaar is, clienttoepassingen mogelijk om door te gaan met behulp van de gegevens die opgeslagen in de cache.

Houd rekening met caching van gegevens die is vaak lezen maar af en toe (bijvoorbeeld gegevens die bestaan uit een hoger aantal gelezen bewerkingen dan schrijven bewerkingen) is gewijzigd. Echter aanbevolen niet dat u de cache als de gezaghebbende opslag van cruciale informatie gebruiken. In plaats daarvan ervoor te zorgen dat alle wijzigingen die uw toepassing niet mag kwijtraken altijd worden opgeslagen op een permanente gegevensopslag. Dit betekent dat als de cache niet beschikbaar is, wordt uw toepassing nog steeds functioneren blijven met behulp van de gegevensopslag en belangrijke informatie niet verloren gaat.

### <a name="determine-how-to-cache-data-effectively"></a>Bepalen hoe u gegevens effectief in cache

De toets om effectief gebruiken van een cache ligt in de meest geschikte gegevens aan cache vaststellen en caching van deze op het juiste moment. De gegevens kunnen worden toegevoegd aan de cache op aanvraag de eerste keer dat wordt overgenomen door een toepassing. Dit betekent dat de toepassing moet ophalen van de gegevens slechts eenmaal vanaf de gegevensopslag en latere toegang kan worden voldaan met behulp van de cache.

U kunt ook kan een cache worden geheel of gedeeltelijk gevuld met gegevens van tevoren meestal wanneer de toepassing wel wordt gestart (een aanpak seeding genoemd). Echter deze mogelijk niet wenselijk willen implementeren seeding voor een grote cache omdat deze methode een plotselinge, hoge belasting van de oorspronkelijke gegevensopslag opleggen kunt wanneer de toepassing wordt gestart.

Vaak kunt een analyse van gebruikspatronen u bepalen of geheel of gedeeltelijk vooraf een cache en kiest u de gegevens die moeten cache. Het kan bijvoorbeeld handig om aan te vullen de cache met de profielgegevens statische gebruiker voor klanten die regelmatig de toepassing (bijvoorbeeld elke dag) gebruiken, maar niet voor klanten die gebruikmaken van de toepassing slechts eenmaal per week zijn.

Meestal caching werkt ook met de gegevens die is onveranderlijke of die niet vaak veranderen. Voorbeelden hiervan zijn naslaginformatie zoals product en prijsinformatie in een e-commerce-toepassing of gedeelde statische resources die zijn dure om samen te stellen. In de cache bij het opstarten van de toepassing te minimaliseren ten vraag op resources en prestaties te verbeteren kan sommige of alle van deze gegevens worden geladen. Het ook misschien een achtergrondproces die regelmatig verwijzing bijwerkt hebben gegevens in de cache om ervoor te zorgen dat deze is bijgewerkt, of dat de cache vernieuwt wanneer verwijzen naar gegevens wijzigingen.

Caching van is minder handig voor dynamische gegevens, maar er enkele uitzonderingen deze aandacht zijn (Zie de sectie Cache ten zeerste dynamische gegevens verderop in dit artikel voor meer informatie). Wanneer de oorspronkelijke gegevens regelmatig worden gewijzigd, de gegevens in de cache is zeer snel verlopen of de realiseren van de cache synchroniseren met de oorspronkelijke gegevensopslag Hiermee reduceert u de effectiviteit van caching.

Houd er rekening mee dat een cache geen om de volledige gegevens voor een entiteit te nemen. Bijvoorbeeld als een gegevensitem een object met meerdere waarden zoals een klant bank met een naam, adres en balance '-account vertegenwoordigt, enkele van deze elementen mogelijk blijven statische (zoals de naam en adres), terwijl de anderen (zoals de saldo) mogelijk meer dynamische. In deze situaties, kan het handig zijn de statische gedeelten van de gegevens in de cache en alleen de overige gegevens ophalen (of berekenen) wanneer dit vereist is.

Het is raadzaam dat u prestaties testen en het gebruik analyse om te bepalen of oude populatie of op aanvraag laden van de cache of een combinatie van beide, passende uitvoeren. Het besluit moet worden gebaseerd op de volatiliteit en gebruikspatroon van de gegevens. Cache-gebruik en prestaties analyse is vooral belangrijk in toepassingen die optreden dik laadtijd en moeten ten zeerste scalable. Bijvoorbeeld in hoge mate van schaalbaarheid scenario's kan het zinvol te vullen de cache als u wilt verkleinen van de belasting op de gegevensopslag tijde piek.

Caching kan ook worden gebruikt om te voorkomen berekeningen herhalende terwijl de toepassing wordt uitgevoerd. Als een bewerking gegevens transformeert of een gecompliceerde berekening wordt uitgevoerd, kunt deze de resultaten van de bewerking opslaan in de cache. Als dezelfde berekening daarna vereist is, kan de toepassing gewoon de resultaten ophalen uit de cache.

Een toepassing kunt wijzigen van gegevens die in een cache wordt bewaard. We raden u echter aan ik denk aan de cache als een tijdelijke gegevensopslag die op elk gewenst moment kan verdwijnen. Sla geen waardevolle gegevens in de cache. Zorg ervoor dat u de gegevens in als u ook de oorspronkelijke gegevensopslag beheren. Dit betekent dat als de cache niet beschikbaar is, u minimaliseren de kans dat gegevens verloren gaan.

### <a name="cache-highly-dynamic-data"></a>Cache ten zeerste dynamische gegevens

Wanneer u gegevens snel veranderende opslaan in een permanente gegevensopslag, kan deze voor het systeem opleggen. Stel een apparaat dat status of enkele andere maten continu-rapporten. Als een toepassing niet wil deze gegevens op basis van de dat de gegevens in de cache bijna altijd mogelijk verouderd in cache, klikt u vervolgens hetzelfde mogelijk retourneert true wanneer het opslaan en deze gegevens ophalen van de gegevensopslag. In de tijd die nodig is om te slaan en deze gegevens ophalen, deze mogelijk zijn gewijzigd.

Houd rekening met de voordelen van de dynamische gegevens rechtstreeks in de cache in plaats van in de winkel permanente gegevens zijn opgeslagen in een situatie zoals dit. Als de gegevens niet-kritieke worden en niet vereist is voor het controleren, klikt u vervolgens het maakt niet uit als u de wijziging van incidentele gaat verloren.

### <a name="manage-data-expiration-in-a-cache"></a>Vervaldatum van de gegevens in een cache beheren

Gegevens die in een cache wordt gehouden is in de meeste gevallen een kopie van de gegevens die in de oorspronkelijke gegevensopslag wordt bewaard. De gegevens in de oorspronkelijke gegevensopslag mogelijk wijzigen nadat deze is opgeslagen in de cache, veroorzaakt door de gegevens in de cache om vertrouwd te verlopen. Veel in de cache systemen kunnen u de cache als u wilt gegevens verloopt en verkleinen van de periode waarvoor gegevens zijn mogelijk verouderd configureren.

Als in de cache opgeslagen gegevens verloopt, wordt verwijderd uit de cache en de toepassing moet de gegevens ophalen uit het oorspronkelijke gegevensopslag (deze kunt plaatsen de zojuist opgehaald gegevens terug in cache). Als u de cache configureert, kunt u een verloopbeleid standaard instellen. In veel cache-services, kunt u ook de verloopdatum voor afzonderlijke objecten bepalen wanneer u ze hebt opgeslagen via een programma in de cache.
Sommige cache kunnen u de verloopdatum opgeven als een absolute waarde of een schuifregelaar waarde die ervoor zorgt dat het item dat moet worden verwijderd uit de cache als dit niet binnen de opgegeven tijdsduur is geopend. Deze instelling overschrijft eventuele cache hele verloopbeleid, maar alleen voor de opgegeven objecten.

> [AZURE.NOTE] Houd rekening met de verloopdatum voor de cache en de objecten die deze zorgvuldig bevat. Als u deze te kort maken, objecten te snel verlopen en verkleint u de voordelen van het gebruik van de cache. Als u de periode te lang maakt, kunt u de gegevens die verouderd risico.

Het is ook mogelijk dat aan de cache opvullen mogelijk als gegevens mag woont gedurende een lange tijd. In dit geval aanvragen voor nieuwe items toevoegen aan de cache kunnen ertoe leiden dat sommige items worden automatisch verwijderd in een proces bekend als een verwijdering. Cache services weghalen meestal gegevens op basis (Recentelijk) kleinste recent gebruikte, maar u kunt meestal dit beleid overschrijven en voorkomen dat items worden verwijderd. Echter als u overstapt op deze methode, kunnen het overschrijden van het geheugen dat beschikbaar is in de cache. Een toepassing die u probeert een item toevoegen aan de cache, mislukt met een uitzondering.

Sommige in de cache implementaties mogelijk extra verwijdering beleidsregels bieden. Er zijn verschillende soorten beleidsregels voor een verwijdering. Hierbij:
- Een recent gebruikte beleid (in de verwachting dat de gegevens niet uitgevoerd opnieuw moeten worden).
- Een eerste-in-first-out beleid (oudste gegevens verwijderd eerst).
- Een beleid voor expliciete verwijderen op basis van een gebeurtenis (zoals de gegevens worden gewijzigd) te activeren.

### <a name="invalidate-data-in-a-client-side-cache"></a>Gegevens in een cache aan de clientzijde ongeldig

Gegevens die in een cache aan de clientzijde wordt gehouden is gewoonlijk beschouwd als buiten het kader van de service waarmee de gegevens naar de klant. Een service niet rechtstreeks zo instellen dat een klant toevoegen of verwijderen van gegevens uit een aan de clientzijde cache.

Dit betekent dat het is mogelijk een client die een slecht geconfigureerde cache gebruikt om door te gaan met behulp van verouderde informatie. Als het verloopbeleid van de cache zijn niet juist geïmplementeerd, kan een client bijvoorbeeld verouderde gegevens die wordt opgeslagen in de cache lokaal wanneer de informatie in de oorspronkelijke gegevensbron is gewijzigd gebruiken.

Als u een webtoepassing dat gegevens via een HTTP-verbinding fungeert maakt, kunt u impliciet een webclient (zoals een browser of webproxy) dwingen kunt u de meest recente gegevens ophalen. U kunt dit doen als een resource wordt bijgewerkt door een wijziging in de URI voor die resource. De URI van een resource webclients meestal gebruikt als de sleutel in de cache aan de clientzijde, zodat de webclient worden genegeerd als de URI wijzigt, een eerder versies van een resource: in cache opgeslagen en de nieuwe versie in plaats daarvan ophaalt.

## <a name="managing-concurrency-in-a-cache"></a>Bij gelijktijdigheid in een cache beheren

Cache zijn vaak ontworpen moet worden gedeeld door meerdere exemplaren van een toepassing. Elk toepassingsexemplaar kunt lezen en wijzigen van gegevens in de cache. Daarom dezelfde gelijktijdigheid problemen die zich met een gedeelde gegevensopslag voordoen ook toegepast op een cache. Mogelijk moet u ervoor zorgen dat updates die zijn aangebracht door één exemplaar van de toepassing niet de wijzigingen die zijn aangebracht door een ander exemplaar overschrijven in een situatie waarin een toepassing moet wijzigen van gegevens die in de cache wordt bewaard.

Afhankelijk van de aard van de gegevens en de kans van conflicten, kunt u overstapt op een van twee manieren om te gelijktijdigheid:

- __Optimistische.__ Direct voordat u gegevens wilt bijwerken, wordt de toepassing controleert om te zien of de gegevens in de cache is gewijzigd sinds deze is opgehaald. Als de gegevens zich nog steeds hetzelfde, kan de wijziging kan worden gemaakt. Anders heeft de toepassing te bepalen of deze bij te werken. (De bedrijfslogica die deze beschikking worden toepassingsspecifieke.) Deze methode is geschikt voor situaties waarin de updates zijn onregelmatig of waar conflicten zijn optreden.
- __Pessimistische.__ Bij het ophalen van de gegevens, wordt het in de toepassing vergrendelen in de cache om te voorkomen dat een ander exemplaar wijzigt. Dit proces zorgt ervoor dat conflicten niet kunnen zijn, maar ze kunnen ook andere exemplaren die hoeft te verwerken van dezelfde gegevens blokkeren. Pessimistische gelijktijdigheid invloed kan zijn op de schaalbaarheid van een oplossing en alleen voor tijdelijk bewerkingen wordt aanbevolen. Het is mogelijk dat deze methode geschikt te maken voor situaties waarin conflicten waarschijnlijk meer geneigd, zijn met name als een toepassing meerdere items in de cache wordt bijgewerkt en ervoor zorgen moet dat deze wijzigingen consistent zijn toegepast.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Beschikbaarheid en schaalbaarheid implementeren, en prestaties verbeteren

Vermijden dat een cache als primaire opslagplaats van gegevens; Dit is de rol van de oorspronkelijke gegevensopslag waaruit de cache wordt gevuld. De oorspronkelijke gegevensopslag is verantwoordelijk voor het gebruiken van de gegevens.

Zorg ervoor dat u niet kritieke afhankelijkheden van de beschikbaarheid van een gedeelde cache-service in uw oplossingen introduceren. Een toepassing moet kunnen blijven functioneren als de service waarmee de gedeelde cache niet beschikbaar is. De toepassing moet niet vastloopt of mislukt tijdens het wachten tot de service cache hervatten.

De toepassing daarom u moet voorbereiden om te bepalen de beschikbaarheid van de service cache en terugschakelen naar de oorspronkelijke gegevensopslag als de cache niet toegankelijk is. De [functie Circuitlijnen patroon](http://msdn.microsoft.com/library/dn589784.aspx) is handig voor het afhandelen van dit scenario. De service waarmee de cache kan worden hersteld en zodra deze beschikbaar, de cache opnieuw zoals gegevens wordt gelezen formulier van de oorspronkelijke gegevens store, in overeenstemming een strategie zoals de [Cache grond patroon](http://msdn.microsoft.com/library/dn589799.aspx)kunt zijn gevuld.

Echter er mogelijk een schaalbaarheid invloed op het systeem als de toepassing terug naar de oorspronkelijke gegevens store valt wanneer de cache tijdelijk niet beschikbaar is is.
Terwijl de gegevensopslag worden hersteld, wordt de oorspronkelijke gegevensopslag kan worden veel tijd gaan zitten met aanvragen voor gegevens, time-out en is mislukt verbindingen.

Houd rekening met een lokale cache implementeren in elk exemplaar van een toepassing, samen met de gedeelde cache waartoe alle toepassingsexemplaren toegang hebben. Wanneer u een item wordt opgehaald, u kunt controleren eerst in de lokale cache en klik vervolgens in de gedeelde cache en ten slotte in de oorspronkelijke gegevens opslaan. De lokale cache kan worden gevuld met behulp van de gegevens in de gedeelde cache of in de database als de gedeelde cache niet beschikbaar is.

Hiervoor is dat u daarbij configuratie om te voorkomen dat de lokale cache te verouderd met betrekking tot de gedeelde cache. De lokale cache fungeert echter als een buffer als de gedeelde cache niet bereikbaar is. Afbeelding 3 ziet deze structuur.

![Een lokale cache gebruikt met een gedeelde cache](media/best-practices-caching/Caching3.png)
_afbeelding 3: een lokale cache gebruikt met een gedeelde cache_

Sommige cache-services bieden ter ondersteuning van grote cache die relatief lang duren gegevens bevatten, een optie voor hoge beschikbaarheid die wordt geïmplementeerd automatische failover als de cache niet meer beschikbaar. Deze methode betekent meestal dat de in de cache opgeslagen gegevens die zijn opgeslagen op een cacheserver primaire op een cacheserver secundaire repliceren en overschakelen naar de secundaire server als de primaire server mislukt of verbinding is verbroken.

Als u wilt de latentie dat is gekoppeld aan het schrijven naar meerdere bestemmingen verkleinen, de replicatie naar de secundaire server zich kan voordoen asynchroon bij gegevens in de cache op de primaire server vastgelegd. Deze methode leidt tot de mogelijkheid dat sommige gegevens in de cache gaan mogelijk verloren bij een storing, maar de verhouding van deze gegevens klein vergeleken met de totale grootte van de cache moet.

Als u een gedeelde cache groot is, is het mogelijk dat deze nuttig om gegevens in de cache op knooppunten te verkleinen van de kans van een conflict en te verbeteren schaalbaarheid partitioneren. Veel gedeelde cache ondersteuning voor knooppunten dynamisch toevoegen (en verwijderen) en de gegevens opnieuw meerdere partities. Deze methode mogelijk gebruikmaakt van cluster, waarin de verzameling knooppunten wordt gepresenteerd voor clienttoepassingen als naadloze, één cache. Intern, maar de gegevens worden verdeeld over tussen knooppunten een vooraf gedefinieerde verdeling strategie die de werklast gelijkmatig te volgen. De [partities leidraad voor gegevens](http://msdn.microsoft.com/library/dn589795.aspx) op de Microsoft-website vindt u meer informatie over mogelijke partities strategieën.

Cluster, kan de beschikbaarheid van de cache ook vergroten. Als een knooppunt mislukt, wordt de rest van de cache nog steeds toegankelijk is.
Cluster wordt vaak gebruikt in combinatie met herhaling en overname bij storing. Elk knooppunt kan worden gerepliceerd en de replica snel online kunt brengen als het knooppunt mislukt.

Veel lezen en schrijven bewerkingen hebben waarschijnlijk gebruikmaakt van één gegevenswaarden of objecten. Maar soms kan het nodig zijn om te slaan of grote hoeveelheden gegevens snel ophalen.
Bijvoorbeeld kan een cache seeding gebruikmaakt van honderden of duizenden items schrijven naar de cache. Een toepassing moet mogelijk ook een groot aantal gerelateerde items uit de cache als onderdeel van de dezelfde aanvraag ophalen.

Veel grootschalige cache bieden batchbewerkingen voor deze toepassing. Hiermee kunt een clienttoepassing voor het pakket van een groot aantal items in één aanvraag en Hiermee reduceert u de realiseren dat is gekoppeld aan de uitvoering van een groot aantal kleine aanvragen.

## <a name="caching-and-eventual-consistency"></a>In de cache en eventuele consistentie

Voor het patroon dat in cache grond om te werken, moet het exemplaar van de toepassing waarmee wordt gevuld de cache hebben toegang tot de meest recente en consistente versie van de gegevens. In een systeem dat eventuele consistentie (zoals een gerepliceerde gegevensopslag) implementeert dit mogelijk niet de hoofdletters/kleine letters.

Één exemplaar van een toepassing kan een gegevensitem wijzigen en de in de cache opgeslagen versie van dat item ongeldig. Een ander exemplaar van de toepassing probeert te lezen dit item uit de cache, waardoor een cache-gemiste, zodat deze de gegevens van de gegevensopslag leest en toegevoegd aan de cache. Als de gegevensopslag is niet volledig is gesynchroniseerd met de andere replica's, kan het toepassingsexemplaar van de lezen en vullen de cache met de oude waarde.

Zie voor meer informatie over het afhandelen van consistentie van de gegevens, de pagina [gegevens consistentie handleiding](http://msdn.microsoft.com/library/dn589800.aspx) op de website van Microsoft.

### <a name="protect-cached-data"></a>In de cache opgeslagen gegevens beschermen

Ongeacht de cache-service die u kunt gebruiken, houd rekening met het beveiligen van de gegevens die in de cache tegen ongeoorloofde toegang wordt bewaard. Er zijn twee belangrijke bezwaren:

- De privacy van de gegevens in de cache
- De privacy van gegevens die doorloopt tussen de cache en de toepassing waarin de cache wordt gebruikt

Als u wilt beveiligen gegevens in de cache, kan de service cache een verificatiemethode die is vereist dat toepassingen Geef het volgende implementeren:
- Welke identiteiten toegang heeft tot gegevens in de cache.
- Welke bewerkingen (lezen en schrijven) die deze identiteiten zijn toegestaan om uit te voeren.

Als u wilt verkleinen belasting die is gekoppeld aan lezen en schrijven van gegevens, nadat een identiteit is verleend schrijven en/of alleen toegang tot de cache, die identiteit alle gegevens in de cache kunt gebruiken.

Als u nodig hebt voor het beperken van toegang aan subsets van gegevens in de cache, kunt u het volgende doen:

- De cache splitsen in partities (met behulp van verschillende cache servers) en alleen toegang verlenen tot de identiteit voor de partities die ze moeten kunnen gebruiken.
- Versleutelt u de gegevens in elke subset met verschillende sleutels en geef de toetsen versleuteling alleen voor identiteiten die toegang tot elke subset nodig hebt. Een clienttoepassing nog steeds mogelijk om op te halen alle gegevens in de cache, maar deze is alleen mogelijk de gegevens waarvoor de toetsen ontsleutelen.

U moet ook de gegevens beveiligen zoals en afmelden bij de cache loopt. Dit doet afhankelijk u van de beveiligingsfuncties van de netwerkinfrastructuur die clienttoepassingen met verbinding maken met de cache. Als de cache wordt geïmplementeerd met de server op een locatie in dezelfde organisatie die als host fungeert voor de clienttoepassingen, klikt u vervolgens de moeten worden geïsoleerd van het netwerk zelf niet moet u mogelijk extra stappen uitvoeren. Als de cache zich bevindt op afstand en een TCP of HTTP-verbinding via een openbaar netwerk (zoals Internet) is vereist, kunt u de implementatie van SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Overwegingen voor de uitvoering van caching met Microsoft Azure

Azure biedt de Cache voor het bestand Vgx. van Azure. Dit is een implementatie van de bron openen bestand Vgx. cache die wordt uitgevoerd als een service in een Azure datacenter. Dit is een in de cache service waarmee zijn toegankelijk vanaf een Azure-toepassing, of de toepassing wordt geïmplementeerd als een cloudservice, een website of in een Azure virtuele machines. Cache kunnen worden gedeeld door clienttoepassingen die beschikt over de juiste-sleutel.

Azure bestand Vgx. Cache is een krachtige in de cache oplossing waarmee beschikbaarheid, schaalbaarheid en beveiliging. Dit is meestal wordt uitgevoerd als een service verdeeld over een of meer specifieke computers. Wordt geprobeerd om op te slaan zoveel informatie kan het in het geheugen om ervoor te zorgen snelle toegang. Deze architectuur is bedoeld om u te bieden lage latentie en hoge doorvoer door te verminderen dat u een langzame i/o-bewerkingen uit te voeren.

 Azure bestand Vgx. Cache is compatibel met veel van de verschillende API's die worden gebruikt door clienttoepassingen. Als er bestaande toepassingen die al Azure bestand Vgx. Cache gebruiken on-premises implementatie actief is, biedt de Cache Azure bestand Vgx. snelle migratiepad naar de cache opslaan in de cloud.

> [AZURE.NOTE] Azure bevat ook de Service Cache beheerd. Deze service is gebaseerd op de engine Azure-Service stof Cache. U kunt een gedistribueerde cache die kan worden gedeeld door flexibele toepassingen maken. De cache wordt gehost op servers van krachtige uitgevoerd in een Azure datacenter.
Deze optie is echter niet meer wordt aanbevolen en is alleen beschikbaar voor de ondersteuning van bestaande toepassingen die zijn gebouwd als u wilt gebruiken. Voor alle nieuwe ontwikkeling, Azure bestand Vgx. Cache in plaats hiervan te gebruiken.
>
> Bovendien ondersteunt Azure in rol caching. Deze functie kunt u een cache die specifiek is voor een cloudservice maken.
De cache wordt gehost door exemplaren van een rol web of werknemer en kan alleen worden geopend voor de functies die zijn die deel uitmaken van dezelfde cloud service implementatie. (Een eenheid implementatie is de set rol-exemplaren die zijn geïmplementeerd als een cloudservice naar een specifieke gebied.) De cache is gegroepeerd, en alle exemplaren van de rol binnen dezelfde implementatie eenheid waarop de cache deel uitmaken van hetzelfde cache cluster. Deze optie is echter niet meer wordt aanbevolen en is alleen beschikbaar voor de ondersteuning van bestaande toepassingen die zijn gebouwd als u wilt gebruiken. Voor alle nieuwe ontwikkeling, Azure bestand Vgx. Cache in plaats hiervan te gebruiken.
>
> Azure-Service beheerd Cache zowel Azure In rol Cache zijn momenteel ondersteuningsperiode voor buitengebruikstelling op 16de November 2016.
Het wordt aanbevolen om te migreren naar Azure bestand Vgx. Cache voorbereiding op deze buitengebruikstelling. Ga naar de pagina voor meer informatie   [Wat is Azure bestand Vgx. Cache aanbod en welke grootte moet ik gebruiken?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) op de website van Microsoft.


### <a name="features-of-redis"></a>Functies van het bestand Vgx.

 Bestand Vgx. is groter dan een eenvoudige cacheserver. Deze biedt een gedistribueerde in het geheugen-database met een uitgebreide opdrachtset die ondersteuning biedt voor veel veelvoorkomende scenario's. Deze worden verderop in dit document, klikt u in de sectie gebruiken bestand Vgx. caching beschreven. In deze sectie bevat een overzicht van enkele van de belangrijkste functies waarmee bestand Vgx..

### <a name="redis-as-an-in-memory-database"></a>Bestand als een database in het geheugen Vgx.

Bestand Vgx. ondersteunt zowel lezen en schrijven bewerkingen. In het bestand Vgx., schrijft bescherming tegen systeemfouten hetzij regelmatig worden opgeslagen in een lokale snapshot-bestand of in een logboekbestand alleen toevoegen. Dit is niet het geval in veel cache (die moeten worden beschouwd als lange gegevensopslag).

 Alle schrijft zijn asynchroon en Blokkeer niet werknemers van lezen en schrijven van gegevens. Wanneer bestand Vgx. starten, leest u de gegevens van het bestand momentopname of log en wordt deze gebruikt om de cache in het geheugen te maken. Zie [bestand Vgx. permanente](http://redis.io/topics/persistence) op de website van bestand Vgx. voor meer informatie.

> [AZURE.NOTE] Bestand Vgx. garandeert dat alle schrijft, worden opgeslagen in het geval van een volledige uitval, maar ten slechtste u slechts een paar seconden zegt van gegevens kwijtraken. Houd er rekening mee dat een cache is niet bedoeld fungeren als een gezaghebbende gegevensbron en de taak van de toepassingen met behulp van de cache om ervoor te zorgen dat belangrijke gegevens met succes is opgeslagen op een juiste gegevensopslag. Zie de [cache grond patroon](http://msdn.microsoft.com/library/dn589799.aspx)voor meer informatie.

#### <a name="redis-data-types"></a>Bestand Vgx gegevenstypen.

Bestand Vgx. is een sleutelwaarden store, waar waarden typen eenvoudige of complexe gegevensstructuren zoals hashes, lijsten bevatten, en wordt ingesteld. Een reeks atomaire bewerkingen op deze gegevenstypen ondersteund. Toetsen zijn permanente of gemarkeerd met een beperkte time to live, waarna de sleutel en de bijbehorende waarde worden automatisch verwijderd uit de cache. Ga naar de pagina [een inleiding tot het bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx. voor meer informatie over het bestand Vgx. sleutels en waarden.

#### <a name="redis-replication-and-clustering"></a>Bestand Vgx replicatie en cluster.

Bestand Vgx. ondersteunt outmodel/medewerkers herhaling om u te helpen zodat deze zeker beschikbaar en voor het behoud van doorvoer. Schrijf bewerkingen naar een bestand Vgx. basispagina knooppunt worden gerepliceerd naar een of meer van de onderliggende knooppunten. Alleen bewerkingen opgehaald kunnen worden door het model of een van de onderliggende niveaus.

In het geval van een partition netwerk kunnen ondergeschikten blijven dienen gegevens en klik vervolgens transparant synchronisatie met het model wanneer de verbinding is hersteld. Ga naar de pagina [herhaling](http://redis.io/topics/replication) op de website van bestand Vgx. voor meer gedetailleerde informatie.

Bestand Vgx. Bovendien cluster, waarmee u kunnen worden gegevens verdelen in shards voor servers en verspreiden van het selectievakje laden. Deze functie verbetert schaalbaarheid, omdat het nieuwe bestand Vgx. servers kunnen worden toegevoegd en de gegevens opnieuw gepartitioneerd als de grootte van de cache verhogen.

Bovendien kan elke server in het cluster worden gerepliceerd met behulp van basispagina/medewerkers herhaling. Dit zorgt ervoor dat beschikbaarheid over elke knooppunt in het cluster. Ga naar het [bestand Vgx. cluster Zelfstudievideo pagina](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx. voor meer informatie over cluster en sharding.

### <a name="redis-memory-use"></a>Bestand Vgx geheugengebruik.

Een bestand Vgx. cache heeft een eindige grootte die afhankelijk van de bronnen die beschikbaar zijn op de host is. Als u een bestand Vgx. server configureert, kunt u de maximale hoeveelheid geheugen die deze kunt gebruiken. U kunt ook een sleutel configureren in een bestand Vgx. cache heeft een verlooptijd, waarna deze automatisch verwijderd uit de cache. Deze functie kan helpen voorkomen dat de cache in het geheugen vullen met oude of verouderde gegevens.

Als het geheugen vol raakt, kunt bestand Vgx. automatisch weghalen sleutels en hun waarden volgens een aantal beleidsregels. De standaardinstelling is Recentelijk (laatst gebruikte), maar u kunt ook andere beleidsregels zoals toetsen willekeurig verwijderen of een verwijdering helemaal uitschakelen (in welke, hoofdletters/kleine letters pogingen items toevoegen aan de cache-fail als het volledige) selecteren. De pagina [Gebruik van het bestand Vgx. Als een cache Recentelijk](http://redis.io/topics/lru-cache) vindt u meer informatie.

### <a name="redis-transactions-and-batches"></a>Bestand Vgx transacties en batches.

Bestand Vgx., kunt een clienttoepassing voor het indienen van een reeks bewerkingen die gegevens lezen en schrijven in de cache als een atomaire transactie. Alle opdrachten in de transactie zijn gegarandeerd opeenvolgend worden uitgevoerd en worden geen opdrachten uitgegeven door andere gelijktijdige clients zijn ingebouwd ertussen.

Dit zijn echter niet waar transacties een relationele database zou doen om uit te voeren ze. Verwerking van transacties bestaat uit twee fasen--de eerste is wanneer de opdrachten in de wachtrij en het tweede is wanneer de opdrachten worden uitgevoerd. De opdracht wachtrij plaatsen fase, worden de opdrachten waaruit de transactie verzonden door de client. Als er fout optreedt op dit moment (zoals een syntaxisfout of het verkeerd aantal parameters) vervolgens bestand Vgx. weigert verwerkingstijd van de hele transactie en verwijdert alle.

Tijdens het uitvoeren fase bestand Vgx. elke wachtrijtaken opdracht uitgevoerd in de reeks. Als een opdracht tijdens deze fase mislukt, bestand Vgx. blijft met de volgende opdracht in de wachtrij en niet hersteld de effecten van alle opdrachten die al zijn uitgevoerd. Dit vereenvoudigd formulier van transactie helpt bij het behoud van prestaties en prestatieproblemen die worden veroorzaakt door een conflict voorkomen.

Bestand Vgx., wordt een vorm van beperkt vergrendelen om u te helpen bij het consistentie van geïmplementeerd. Ga naar de [pagina transacties](http://redis.io/topics/transactions) op de website van bestand Vgx. voor gedetailleerde informatie over transacties en vergrendelen met het bestand Vgx..

Bestand Vgx. ondersteunt ook niet batchen van aanvragen. Het bestand Vgx. protocol dat clients gebruiken om opdrachten te verzenden naar een bestand Vgx. server kan clients voor het verzenden van een reeks bewerkingen als onderdeel van één verzoek. Dit kan helpen verkleinen pakket fragmentatie op het netwerk. Wanneer de batch wordt verwerkt, wordt elke opdracht wordt uitgevoerd. Als er een van deze opdrachten onjuist zijn, ze geweigerd (wat niet gebeuren met een transactie), maar de resterende opdrachten wordt uitgevoerd. Er is ook geen garantie over de volgorde waarin u de opdrachten in de batch wordt verwerkt.

### <a name="redis-security"></a>Bestand Vgx beveiliging.

Bestand Vgx. is gericht zuiver op snel toegang tot gegevens en is ontworpen om uit te voeren in een vertrouwde omgeving die alleen door vertrouwde clients kan worden geopend. Bestand Vgx. ondersteunt een beperkte beveiligingsmodel op basis van wachtwoordverificatie. (Dit is mogelijk te verwijderen verificatie volledig, hoewel dit wordt niet aanbevolen.)

Alle geverifieerde clients hetzelfde globale wachtwoord delen en toegang hebben tot dezelfde resources. Als u meer uitgebreide aanmeldingsproblemen beveiliging nodig hebt, moet u uw eigen beveiligingsniveau vóór de server bestand Vgx. implementeren en alle aanvragen van clients moeten geven door deze extra laag. Bestand Vgx. moet niet rechtstreeks blootgesteld aan niet-vertrouwde of niet-geverifieerde klanten.

U kunt toegang tot opdrachten beperken door ze uit te schakelen of naam wijzigen (en door alleen bevoegdheden clients voorzien van de nieuwe namen).

Bestand Vgx. ondersteunt niet direct elke vorm van gegevenscodering, zodat alle codering moet worden uitgevoerd door clienttoepassingen. Daarnaast kunnen biedt bestand Vgx. geen elk formulier voor transport waardepapier. Als u nodig hebt om gegevens te beveiligen zoals via het netwerk loopt, wordt u aangeraden een SSL-proxy implementeren.

Ga naar de pagina [bestand Vgx. beveiliging](http://redis.io/topics/security) op de website van bestand Vgx. voor meer informatie.

> [AZURE.NOTE] Azure bestand Vgx. Cache biedt een eigen beveiligingslaag waarmee clients verbinding maken. De onderliggende bestand Vgx.-servers worden niet blootgesteld aan het openbare netwerk.

### <a name="using-the-azure-redis-cache"></a>Gebruik van de cache Azure bestand Vgx.

De Cache Azure bestand Vgx. biedt toegang tot bestand Vgx. servers op servers gehost bij een Azure datacenter; Deze fungeert als een gevel waarmee beheren in access en beveiliging. U kunt een cache inrichten met behulp van de Azure-beheerportal. De portal biedt een aantal vooraf gedefinieerde configuraties, die variëren van een 53GB cache wordt uitgevoerd met een speciale service die u ondersteunt SSL communicatie (voor privacy) en outmodel/medewerkers replicatie met een SLA van beschikbaarheid aan 99,9%, naar beneden af op een 250 MB zonder herhaling (geen garanties op beschikbaarheid) uitvoert op gedeelde hardware in cache.

De beheerportal van Azure gebruikt, kunt u ook het beleid verwijdering van de cache configureren en toegang tot de cache door gebruikers toe te voegen aan de rollen die is opgegeven. Eigenaar, Inzender, Reader. Deze rollen definiëren de bewerkingen die leden kunnen uitvoeren. Bijvoorbeeld leden van de rol van de eigenaar van de volledige controle over de cache (inclusief beveiliging) en de inhoud ervan hebt leden van de rol Inzender kunnen lezen en schrijven van informatie in de cache en leden van de rol van de lezer kunnen alleen gegevens ophalen uit de cache.

De meeste beheertaken worden uitgevoerd via de portal Azure Management en daarom veel van de administratieve opdrachten beschikbaar in de standaardversie van het bestand Vgx. zijn niet beschikbaar, waaronder de mogelijkheid voor het wijzigen van de configuratie via programmacode, afsluiten de server bestand Vgx. extra slaves of automatisch opslaan van gegevens naar schijf configureren.

De portal Azure management bevat een handige grafische weergave waarmee u kunt de prestaties van de cache controleren. Bijvoorbeeld, kunt u het aantal verbindingen plaatsvindt weergeven, het aantal aanvragen uitgevoerd, het volume van lezen en schrijven, en het aantal cache treffers versus Cachemissers. Gebruik van deze informatie kunt u de effectiviteit van de cache bepalen en als nodig naar een andere configuratie overschakelen of wijzig het beleid verwijdering. Bovendien kunt u meldingen die e-mailberichten naar een beheerder verzenden als u een of meer van de kritieke doelstellingen valt buiten een verwachte bereik. Bijvoorbeeld, als het aantal mislukte cache groter is dan een opgegeven waarde in het laatste uur, kan een beheerder worden gewaarschuwd wanneer de cache mogelijk te klein of gegevens zijn dat u te snel verwijdert kan.

U kunt ook controleren processor en geheugen netwerkgebruik voor de cache.

Voor meer informatie en voorbeelden waarin wordt getoond hoe maken en configureren van een Azure bestand Vgx. Cache gaat u naar de pagina [Lap rond Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) in het blog van Azure.

## <a name="caching-session-state-and-html-output"></a>In de cache sessie- en HTML-uitvoer

Als u het bouwen van ASP.NET webtoepassingen dat met behulp van Azure web rollen uitvoert, kunt u informatie over de status van de sessie en opslaan HTML-uitvoer in een Azure bestand Vgx. Cache. De sessie staat-Provider voor Azure bestand Vgx. Cache kunt u delen van informatie over de sessie tussen verschillende exemplaren van een ASP.NET-webtoepassing en is handig in web-farm situaties waarin client / server-affiniteit is niet beschikbaar en caching van sessie gegevens in het geheugen niet gepast.

Gebruik van de sessie staat Provider met Azure bestand Vgx. Cache biedt verschillende voordelen, zoals:

- Deze kunt status van de sessie onder een groot aantal exemplaren van een ASP.NET-webtoepassing, verbeterde schaalbaarheid, leveren delen
- Gecontroleerde, gelijktijdige toegang tot dezelfde sessie staat gegevens worden ondersteund voor meerdere lezers en een één auteur, en
- Deze compressie geheugen opslaan en kunt de netwerkprestaties verbeteren.

Bezoek de pagina [ASP.NET sessie staat-Provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-session-state-provider.md) op de Microsoft-website voor meer informatie.

> [AZURE.NOTE] Gebruik niet de sessie staat-Provider voor Azure bestand Vgx. Cache voor ASP.NET-toepassingen die buiten de Azure-omgeving worden uitgevoerd. De latentie van toegang tot de cache van buiten Azure kunt de prestatievoordelen van caching van gegevens voorkomen.

De uitvoer Cache-Provider voor Azure bestand Vgx. Cache op dezelfde manier kunt u opslaan van de HTTP-antwoorden gegenereerd door een ASP.NET-webtoepassing. Gebruik van de Provider van de Cache uitvoer met Azure bestand Vgx. Cache kunt verbeteren de tijden antwoord van toepassingen waarmee complexe HTML-uitvoer; toepassingsexemplaren genereren vergelijkbare antwoorden kunnen maken gebruik van de gedeelde uitvoer fragmenten in de cache in plaats van deze nieuwe uitvoer HTML genereren.  Bezoek de pagina [ASP.NET uitvoer Cache-Provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) op de Microsoft-website voor meer informatie.

### <a name="azure-redis-cache"></a>Azure cache bestand Vgx.

Azure bestand Vgx. Cache biedt toegang tot bestand Vgx. servers die worden gehost op een Azure datacenter. Deze fungeert als een gevel waarmee beheren in access en beveiliging. U kunt een cache inrichten met behulp van de Azure-portal.

De portal biedt een aantal vooraf gedefinieerde configuraties. Deze bereik uit de cache van een 53 GB wordt uitgevoerd met een speciale service met ondersteuning voor SSL-communicatie (voor privacy) en outmodel/medewerkers replicatie met een SLA van beschikbaarheid aan 99,9%, naar beneden af op een 25 0 MB cache zonder herhaling (geen garanties op beschikbaarheid) uitvoert op gedeelde hardware.

De portal van Azure gebruikt, kunt u ook het beleid verwijdering van de cache configureren en toegang tot de cache door gebruikers toe te voegen aan de rollen die is opgegeven.  Deze rollen, waarin de bewerkingen die leden kunnen uitvoeren te definiëren, kunnen u de eigenaar, Inzender en Reader. Bijvoorbeeld leden van de rol van de eigenaar van de volledige controle over de cache (inclusief beveiliging) en de inhoud ervan hebt leden van de rol Inzender kunnen lezen en schrijven van informatie in de cache en leden van de rol van de lezer kunnen alleen gegevens ophalen uit de cache.

De meeste beheertaken worden uitgevoerd via de portal van Azure. Daarom zijn veel van de administratieve opdrachten die beschikbaar in de standaardversie van het bestand Vgx zijn. niet beschikbaar, waaronder de mogelijkheid te de configuratie via programmacode wijzigen, de server bestand Vgx. afsluiten, aanvullende ondergeschikten configureren of gegevens automatisch op schijf opslaan.

De Azure-portal bevat een handige grafische weergave waarmee u kunt de prestaties van de cache controleren. U kunt bijvoorbeeld het aantal verbindingen worden aangebracht, het aantal aanvragen wordt uitgevoerd, het volume van lezen en schrijven, en het aantal cachetreffers versus Cachemissers weergeven. Deze gegevens gebruikt, kunt u de effectiviteit van de cache vaststellen en overschakelen naar een andere configuratie of wijzig het beleid verwijdering indien nodig.

Bovendien kunt u meldingen die e-mailberichten naar een beheerder verzenden als u een of meer van de kritieke doelstellingen valt buiten een verwachte bereik. U wilt bijvoorbeeld Waarschuw beheerder als het aantal mislukte cache groter is dan een opgegeven waarde in het laatste uur, omdat dit betekent dat de cache misschien te klein of gegevens mogelijk wordt verwijderen te snel.

U kunt ook de CPU, het geheugen en het netwerkgebruik voor de cache controleren.

Voor meer informatie en voorbeelden waarin wordt getoond hoe maken en configureren van een Azure bestand Vgx. Cache gaat u naar de pagina [Lap rond Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) in het blog van Azure.

## <a name="caching-session-state-and-html-output"></a>In de cache sessie- en HTML-uitvoer

Als u samenstelt beschrijft ASP.NET-webtoepassingen dat met behulp van Azure web rollen uitvoert, kunt u opslaan sessie informatie en HTML-uitvoer in een Azure bestand Vgx. Cache. De sessie staat-provider voor Azure bestand Vgx. Cache kunt u delen van informatie over de sessie tussen verschillende exemplaren van een ASP.NET-webtoepassing en is handig in web-farm situaties waarin client / server-affiniteit is niet beschikbaar en caching van sessie gegevens in het geheugen niet gepast.

Gebruik van de sessie staat provider met Azure bestand Vgx. Cache biedt verschillende voordelen, zoals:

- Status van de sessie delen met een groot aantal exemplaren van ASP.NET-webtoepassingen.
- Verbeterde schaalbaarheid leveren.
- Ondersteunende gecontroleerde, gelijktijdige toegang tot dezelfde gegevens voor de sessie hebben voor meerdere lezers en een enkel schrijver.
- Met behulp van compressie geheugen besparen en netwerkprestaties te verbeteren.

Bezoek de [ASP.NET-sessie staat-provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-session-state-provider.md) op de Microsoft-website voor meer informatie.

> [AZURE.NOTE] Gebruik niet de sessie staat-provider voor Azure bestand Vgx. Cache in ASP.NET-toepassingen die buiten de Azure-omgeving worden uitgevoerd. De latentie van toegang tot de cache van buiten Azure kunt de prestatievoordelen van caching van gegevens voorkomen.

De uitvoer cache-provider voor Azure bestand Vgx. Cache op dezelfde manier kunt u de HTTP-antwoorden gegenereerd door een ASP.NET-webtoepassing opslaan. Gebruik van de provider van de cache uitvoer met Azure bestand Vgx. Cache kunt de tijden antwoord van toepassingen waarmee complexe HTML-uitvoer verbeteren. Toepassingsexemplaren die vergelijkbaar antwoorden genereren Maak gebruik van de gedeelde uitvoer fragmenten in de cache in plaats van deze nieuwe uitvoer HTML genereren. Bezoek de [ASP.NET uitvoer cache-provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) op de Microsoft-website voor meer informatie.

## <a name="building-a-custom-redis-cache"></a>Samenstellen van een aangepaste cache voor het bestand Vgx.

Azure bestand Vgx. Cache fungeert als een gevel met de onderliggende bestand Vgx.-servers. Momenteel ondersteunt een vaste reeks configuraties maar biedt geen voor het bestand Vgx. cluster. U kunt maken en hosten van uw eigen servers bestand Vgx. met behulp van Azure virtuele machines als u een geavanceerde configuratie die buiten de cache Azure bestand Vgx. (zoals een cache groter is dan 53 GB valt) vereist.

Dit is een mogelijk complex proces omdat u maken verschillende VMs moet mogelijk fungeert als diamodel en de onderliggende knooppunten als u wilt implementeren herhaling. Bovendien als u maken van een cluster wilt, moet u meerdere modellen en onderliggende servers. De topologie van een minimale gegroepeerd replicatie vindt u een hoge mate van beschikbaarheid en schaalbaarheid bestaat uit ten minste zes VMs geordend als drie paar outmodel/medewerkers servers (een cluster moet ten minste drie basispagina knooppunten bevatten).

Elk paar outmodel/medewerkers zich dicht bij elkaar bevinden om te beperken latentie. Echter actief elke set paren zijn in verschillende Azure datacenters zich in verschillende gebieden, als u zoeken in de cache opgeslagen gegevens dicht bij de toepassingen die waarschijnlijk wilt gebruiken. De pagina [Waarop het bestand Vgx. Klik op een CentOS Linux VM in Azure wordt aangegeven](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) op de website van Microsoft begeleidt bij een voorbeeld waarin wordt uitgelegd hoe maken en configureren van een bestand Vgx. knooppunt wordt uitgevoerd met een VM Azure.

[AZURE.NOTE] Houd er rekening mee dat als u uw eigen cache bestand Vgx. op deze manier implementeert, u verantwoordelijk bent voor controle, beheren en beveiligen van de service.

## <a name="partitioning-a-redis-cache"></a>Partitioneren een cache bestand Vgx.

De cache partitioneren heeft betrekking op de cache op meerdere computers splitsen. Deze structuur kunt u diverse voordelen via een server gebruikt één cache, waaronder:

- Een cache maken die is veel groter dan op één server kan worden opgeslagen.
- Het distribueren van gegevens voor servers, het verbeteren van de beschikbaarheid van. Als één server mislukt of niet toegankelijk zijn, de gegevens die deze bevat niet beschikbaar is, maar de gegevens op de overige servers nog steeds toegankelijk. Voor een cache is dit niet essentieel omdat de in de cache opgeslagen gegevens alleen een tijdelijke kopie van de gegevens die in een database wordt bewaard. In plaats daarvan kunnen in de cache opgeslagen gegevens op een server die niet toegankelijk zijn opgeslagen op een andere server.
- Het selectievakje laden spreiden over servers, waardoor de prestaties en schaalbaarheid te verbeteren.
- De gegevens van de Geolocating dicht bij de gebruikers die toegang toe, zodat latentie wordt beperkt.

Voor een cache is de meest voorkomende vorm van partitioneren sharding. In deze strategie is elke partition (of shard) een bestand Vgx. cache in een eigen rechts. Gegevens is gericht op een specifieke partition met behulp van sharding bedrijfslogica, die de beschikking over tal van manieren distributie van de gegevens. Het [patroon Sharding](http://msdn.microsoft.com/library/dn589797.aspx) vindt u meer informatie over het implementeren van sharding.

Als u wilt implementeren in een bestand Vgx. cache partitioneren, kunt u een van de volgende manieren uitvoeren:

- _Aan de clientzijde query-mailroutering._ In deze techniek stuurt een clienttoepassing een aanvraag naar een van de servers voor bestand Vgx. waaruit de cache (waarschijnlijk de dichtstbijzijnde server). Elke server bestand Vgx. slaat metagegevens die goed de partition beschrijft dat zij bevat en bevat ook informatie over welke partities bevinden zich op andere servers bevinden. De server bestand Vgx. worden onderzocht, aanvraag van de client. Als deze kan lokaal worden opgelost, wordt deze bewerking uitvoeren. Anders wordt de aanvraag kunnen de juiste server doorgestuurd. Dit model wordt geïmplementeerd door het bestand Vgx. cluster en klik op de pagina [bestand Vgx. cluster zelfstudie](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx. uitgebreider wordt beschreven. Bestand Vgx. cluster is transparant voor clienttoepassingen en extra bestand Vgx. servers kunnen worden toegevoegd aan het cluster (en de gegevens opnieuw partitioneren) zonder dat u opnieuw de clients configureren.

- _Aan de clientzijde partitioneren._ In dit model, bevat de clienttoepassing logica (mogelijk in de vorm van een bibliotheek) waarmee aanvragen naar de juiste bestand Vgx.-server. Deze methode kan worden gebruikt met Azure bestand Vgx. Cache. Meerdere Azure bestand Vgx. cache (één voor elke partition gegevens) maken en implementeren van de logica aan de clientzijde waarmee de aanvragen aan de juiste cache. Als de partities kleurenschema veranderd (als er extra Azure bestand Vgx. cache worden gemaakt, bijvoorbeeld), clienttoepassingen moet mogelijk opnieuw worden geconfigureerd.

- _Proxy ondersteunde partitioneren._ In dit schema client toepassingen verzenden aanvragen bij een tussenliggende proxyservice die begrijpt hoe de gegevens is partitioneren en stuurt vervolgens de aanvraag door naar de juiste bestand Vgx. server. Deze methode kan ook worden gebruikt met Azure bestand Vgx. Cache. de proxy-service kan worden geïmplementeerd als een Azure-cloudservice. Deze methode is vereist voor een aanvullende complexiteit willen implementeren van de service en aanvragen om uit te voeren dan aan de clientzijde partitioneren langer kunnen duren.

De pagina [partitionering: het splitsen van gegevens tussen meerdere exemplaren bestand Vgx.](http://redis.io/topics/partitioning) op het bestand Vgx. website vindt u meer informatie over het implementeren van partitioneren met het bestand Vgx..

### <a name="implement-redis-cache-client-applications"></a>Implementeren cache-clienttoepassingen bestand Vgx.

Bestand Vgx. ondersteunt clienttoepassingen in veel talen geschreven. Als u nieuwe toepassingen samenstelt met behulp van .NET Framework, wordt de aanbevolen werkwijze is voor het gebruik van de bibliotheek van de client StackExchange.Redis. Deze bibliotheek bevat een .NET Framework-objectmodel dat de gegevens voor verbinding maken met een server bestand Vgx., opdrachten verzenden en ontvangen van antwoorden abstracts. Dit is beschikbaar in Visual Studio als een pakket NuGet. Verbinding maken met een Azure bestand Vgx. Cache of een aangepast bestand Vgx. cache die worden gehost op een VM kunt u deze dezelfde bibliotheek.

Verbinding maken met een bestand Vgx. server die u gebruikt de statische `Connect` methode van de `ConnectionMultiplexer` class. De verbinding die Hiermee maakt u deze methode is ontworpen moet worden gebruikt in de levensduur van de clienttoepassing en de verbinding die kan worden gebruikt door meerdere gelijktijdige threads. Niet opnieuw verbinding maken en verbreken van telkens wanneer u een bestand Vgx.-bewerking uitvoeren, omdat dit de prestaties kan afnemen.

U kunt de verbindingsparameters, zoals het adres van de host bestand Vgx. en het wachtwoord opgeven. Als u Azure bestand Vgx. Cache gebruikt, is het wachtwoord op de primaire of secundaire opgeven die u voor Azure bestand Vgx. Cache wordt gegenereerd met behulp van de Azure-beheerportal.

Nadat u hebt verbonden met de server bestand Vgx., kunt u een greep aan de bestand Vgx. database die als de cache fungeert. De verbinding bestand Vgx. biedt de `GetDatabase` methode u dit wilt doen. U kunt vervolgens items uit de cache ophalen en opslag van gegevens in de cache met behulp van de `StringGet` en `StringSet` methoden. Deze methoden verwachten van een sleutel als een parameter en het item in de cache met een overeenkomstige waarde te retourneren (`StringGet`) of het item toevoegen aan de cache met deze toets (`StringSet`).

Afhankelijk van de locatie van de server bestand Vgx. kunnen veel bewerkingen sommige latentie oplopen terwijl een aanvraag naar de server verzonden en een antwoord wordt geretourneerd naar de klant. De bibliotheek StackExchange biedt asynchroon versies van veel van de methoden die worden getoond om u te helpen clienttoepassingen blijven heeft gereageerd. Deze methoden ondersteuning voor het [Asynchrone patroon op basis van een taak](http://msdn.microsoft.com/library/hh873175.aspx) in .NET Framework.

Het volgende codefragment toont een methode met de naam `RetrieveItem`. Deze ziet u een implementatie van het patroon dat in cache grond op basis van bestand Vgx. en de StackExchange-bibliotheek. De methode wordt de waarde van een tekenreeks sleutel en probeert op te halen van het bijbehorende item uit de cache bestand Vgx. door de ondersteuning voor de `StringGetAsync` methode (de asynchroon versie van `StringGet`).

Als het item niet wordt gevonden, wordt opgehaald uit de onderliggende gegevensbron gebruikt de `GetItemFromDataSourceAsync` methode (dit is een lokale methode en geen deel uitmaakt van de bibliotheek StackExchange). Toegevoegd aan de cache met behulp van de `StringSetAsync` methode zodat deze kan worden opgehaald sneller zodra.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

De `StringGet` en `StringSet` methoden worden niet beperkt tot ophalen of tekenreekswaarden opslaan. Elk item dat wordt omgezet als een matrix van bytes kunnen nemen. Als u moet een .NET-object op te slaan, u kunt deze als een gegevensstroom byte serialiseren en gebruiken de `StringSet` methode te schrijven aan de cache.

U kunt een object op dezelfde manier lezen uit de cache met behulp van de `StringGet` methode en deze als een object .NET deserialiseren. De volgende code toont een set extensie methoden voor de interface IDatabase (de `GetDatabase` methode van een verbinding bestand Vgx. geeft als resultaat een `IDatabase` object), en enkele voorbeelden van code waarbij deze methoden wordt gebruikt om te lezen en schrijven een `BlogPost` object naar de cache:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

De volgende code ziet u een methode met de naam `RetrieveBlogPost` die deze methoden extensie gebruikt om te lezen en schrijven een serialiseerbaar `BlogPost` object naar de cache voor het patroon dat in cache grond te volgen:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Bestand Vgx. ondersteunt opdracht mogelijk als een clienttoepassing meerdere asynchrone opdrachten verzendt. Bestand Vgx., kan de aanvragen dezelfde verbinding gebruiken in plaats van ontvangen en reageert op opdrachten in een strikte reeks multiplexen.

Deze methode helpt bij het verkleinen van latentie doordat efficiënter gebruik van het netwerk. Het volgende codefragment toont een voorbeeld waarmee de details van twee klanten gelijktijdig worden opgehaald. De code dient twee aanvragen en voert enkele andere verwerking (niet wordt weergegeven) voordat het wachten om de resultaten te ontvangen. De `Wait` methode van het object cache is vergelijkbaar met .NET Framework `Task.Wait` methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

De pagina [Azure bestand Vgx. Cache documentatie](https://azure.microsoft.com/documentation/services/cache/) op de Microsoft-website vindt u meer informatie over het schrijven van clienttoepassingen die de Cache Azure bestand Vgx. kunnen gebruiken. Aanvullende informatie is beschikbaar op de [pagina basisgebruik](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) op de website van StackExchange.Redis.

De pagina op de website van dezelfde [pijpleidingen en multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) vindt u meer informatie over asynchrone bewerkingen en mogelijk met bestand Vgx. en de StackExchange-bibliotheek.  De volgende sectie in dit artikel, bestand Vgx. Caching van gebruikt, bevat voorbeelden van bepaalde meer geavanceerde technieken beschreven die u kunt toepassen op de gegevens die in een bestand Vgx. cache wordt bewaard.

## <a name="using-redis-caching"></a>Gebruik caching van bestand Vgx.

De eenvoudigste gebruik van het bestand Vgx. voor caching van bezwaren is sleutel-waardeparen waarvan de waarde is een-tekenreeks van willekeurige lengte die een binaire gegevens bevatten. (Dit is in principe een matrix van bytes die kan worden behandeld als een tekenreeks). Dit scenario is geïllustreerd in de sectie implementeren bestand Vgx. Cache-clienttoepassingen eerder in dit artikel.

Houd er rekening mee dat toetsen ook gegevens bevatten, zodat u de binaire gegevens als de sleutel gebruiken kunt. Hoe langer de sleutel is echter de meer ruimte het duurt om op te slaan en het langer duurt opzoeken bewerkingen uit te voeren. Voor de bruikbaarheid en onderhoud te vereenvoudigen, uw keyspace zorgvuldig ontwerp en gebruik duidelijke (maar niet uitgebreide).

Gebruik bijvoorbeeld gestructureerde pijl zoals 'klant: 100' om aan te geven van de toets voor de klant met ID-100 in plaats van gewoon '100'. Dit schema kunt u eenvoudig onderscheid maken tussen waarden die verschillende gegevenstypen opslaat. U kunt de toets 'orders: 100' bijvoorbeeld ook gebruiken om aan te geven van de sleutel voor de volgorde met ID-100.

Eendimensionale binaire tekenreeksen, met uitzondering van kan een waarde in een paar sleutelwaarden bestand Vgx. ook bevatten meer gestructureerde gegevens, lijsten, waaronder sets (gesorteerd en niet gesorteerd) en ervoor zorgt. Bestand Vgx. biedt een uitgebreide opdrachtset die dit soort kunt bewerken, en veel van deze opdrachten zijn beschikbaar voor .NET Framework-toepassingen via de clientbibliotheek van een zoals StackExchange. De pagina [een inleiding tot het bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx. biedt een gedetailleerd overzicht van deze typen en de opdrachten die u gebruiken kunt om te bewerken.

In deze sectie bevat een overzicht van enkele veelvoorkomende gevallen gebruiken voor deze gegevenstypen en opdrachten.

### <a name="perform-atomic-and-batch-operations"></a>Atomaire uitvoeren en batch-bewerkingen

Een reeks atomaire get-en-set bewerkingen op tekenreekswaarden bestand Vgx. wordt ondersteund. Deze bewerkingen verwijderen de mogelijke raceauto risico's die optreden kunnen wanneer u afzonderlijke `GET` en `SET` opdrachten. De bewerkingen die beschikbaar zijn onder andere:

- `INCR`, `INCRBY`, `DECR`, en `DECRBY`, waarin atomaire verhogen of verlagen bewerkingen uitvoeren op de numerieke gegevenswaarden geheel getal. De bibliotheek StackExchange biedt overbelasting versies van de `IDatabase.StringIncrementAsync` en `IDatabase.StringDecrementAsync` methoden om deze bewerkingen uitvoeren en de resulterende waarde die is opgeslagen in de cache terug te keren. Het volgende codefragment ziet u het gebruik van de volgende manieren:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, waarin wordt de waarde die is gekoppeld aan een sleutel en wordt dit omgezet in een nieuwe waarde opgehaald. De bibliotheek StackExchange zorgt ervoor dat deze bewerking beschikbaar via de `IDatabase.StringGetSetAsync` methode. De onderstaande codefragment toont een voorbeeld van deze methode. Deze code retourneert de huidige waarde die is gekoppeld aan de toets "gegevens: teller" uit het vorige voorbeeld. Vervolgens wordt de waarde voor deze toets terug naar nul, Alles als onderdeel van dezelfde bewerking hersteld:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`en `MSET`, waarin of te wijzigen als een set tekenreekswaarden als een enkele bewerking. De `IDatabase.StringGetAsync` en `IDatabase.StringSetAsync` methoden overbelast zijn om deze functionaliteit, zoals wordt weergegeven in het volgende voorbeeld:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

U kunt ook meerdere bewerkingen in één bestand Vgx. transactie combineren, zoals in het bestand Vgx. transacties batches sectie en eerder in dit artikel wordt beschreven. De bibliotheek StackExchange biedt ondersteuning voor transacties tot en met de `ITransaction` interface.

U maakt een `ITransaction` object met behulp van de `IDatabase.CreateTransaction` methode. Aanroepen van opdrachten aan de transactie met behulp van de methoden die door de `ITransaction` object.

De `ITransaction` interface biedt toegang tot een set methoden die vergelijkbaar is met die toegankelijk voor de `IDatabase` interface, behalve dat alle methoden asynchroon zijn. Dit betekent dat ze alleen worden uitgevoerd wanneer de `ITransaction.Execute` methode is geactiveerd. De waarde die wordt geretourneerd door de `ITransaction.Execute` methode geeft aan of de transactie is gemaakt (waar) of als dit is mislukt (ONWAAR).

Het volgende codefragment toont een voorbeeld die verhoogd of verlaagd twee items als onderdeel van dezelfde transactie:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Denk eraan dat bestand Vgx. transacties in tegenstelling tot transacties in relationele databases. De `Execute` methode wachtrijen gewoon alle opdrachten waaruit de transactie moet worden uitgevoerd en als een van deze is onjuist wordt de transactie is gestopt. Als alle opdrachten wachtrij is geplaatst is, wordt elke opdracht asynchroon uitgevoerd.

Als een opdracht niet werkt, blijven de anderen nog steeds verwerking. Als u nodig hebt om te bevestigen dat een opdracht is voltooid, moet u de resultaten van de opdracht ophalen met behulp van de eigenschap van het **resultaat** van de bijbehorende taak, zoals wordt weergegeven in het bovenstaande voorbeeld. Lezen van de eigenschap van het **resultaat** wordt de thread geblokkeerd totdat de taak is voltooid.

Zie de pagina [transacties in het bestand Vgx.](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) op de website van StackExchange.Redis voor meer informatie.

Wanneer u batchbewerkingen uitvoert, kunt u de `IBatch` interface van de bibliotheek StackExchange. Deze interface biedt toegang tot een set methoden die overeenkomen met die toegankelijk voor de `IDatabase` interface, behalve dat alle methoden asynchroon zijn.

U maakt een `IBatch` object met behulp van de `IDatabase.CreateBatch` methode en voer de batch met behulp van de `IBatch.Execute` methode, zoals wordt weergegeven in het volgende voorbeeld. Deze code gewoon Hiermee stelt u een tekenreekswaarde, verhoogd of verlaagd dezelfde items in het vorige voorbeeld gebruikt, en de resultaten worden weergegeven:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Het is belangrijk om te begrijpen dat in tegenstelling tot een transactie als een opdracht in een batch mislukt omdat het onjuiste, de andere opdrachten mogelijk nog steeds uitvoeren. De `IBatch.Execute` methode retourneert geen een vermelding van het slagen of mislukken.

### <a name="perform-fire-and-forget-cache-operations"></a>Fire hebt uitgevoerd en vergeet cache bewerkingen

Bestand Vgx. ondersteunt fire en vergeet bewerkingen met behulp van de opdracht markeringen. In dit geval de client gewoon een bewerking start, maar heeft geen rente in het resultaat en wacht niet de opdracht moet zijn voltooid. Het volgende voorbeeld ziet u hoe u de opdracht INCR als een fire uitvoeren en vergeet bewerking:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Automatisch verlopen toetsen opgeven

Wanneer u een item in een bestand Vgx. cache opslaat, kunt u een time-out waarna het item wordt automatisch verwijderd uit de cache. U kunt ook zoeken hoe veel meer tijd een sleutel heeft voordat het verloopt met behulp van de `TTL` opdracht. Deze opdracht is beschikbaar voor StackExchange toepassingen met behulp van de `IDatabase.KeyTimeToLive` methode.

Het volgende codefragment ziet u hoe u een verlooptijd van 20 seconden ingesteld op een sleutel en de resterende levensduur van de sleutel query:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

U kunt ook de verlooptijd om een specifieke datum en tijd instellen met de opdracht VERLOPEN, dat beschikbaar in de bibliotheek StackExchange als is de `KeyExpireAsync` methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tip:_ U kunt handmatig een item uit de cache verwijderen met behulp van de opdracht DEL, die beschikbaar via de bibliotheek StackExchange als is de `IDatabase.KeyDeleteAsync` methode.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Markeringen gebruiken om te cross-verwijzen in de cache opgeslagen items

Een set bestand Vgx. is een verzameling meerdere items die één sleutel deelt. U kunt een set maken met de opdracht SADD. U kunt de items in een set met de opdracht SMEMBERS ophalen. De bibliotheek StackExchange implementeert de opdracht SADD met de `IDatabase.SetAddAsync` opdracht methode, en de SMEMBERS met de `IDatabase.SetMembersAsync` methode.

U kunt ook bestaande sets maken van nieuwe sets met behulp van de SDIFF (set verschil), GESINTERD (set snijpunt) en SUNION (set Unie) opdrachten combineren. De bibliotheek StackExchange worden deze bewerkingen waarbij de `IDatabase.SetCombineAsync` methode. De eerste parameter voor deze methode bepaalt de bewerking instellen om uit te voeren.

De volgende codefragmenten weergeven hoe sets is handig voor het snel opslaan en ophalen van verzamelingen van verwante items. In deze code wordt de `BlogPost` type dat is beschreven in de sectie clienttoepassingen implementeren bestand Vgx. Cache eerder in dit artikel.

A `BlogPost` object vier velden bevat, een ID, een titel, een score classificatie en een verzameling tags. Het eerste codefragment hieronder ziet u de voorbeeldgegevens die wordt gebruikt om een lijst C# met `BlogPost` objecten:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

U kunt de labels opslaan voor elk `BlogPost` object aan als een set in een bestand Vgx. cache en elke set koppelen aan de ID van de `BlogPost`. Hierdoor wordt een toepassing om snel alle tags die deel uitmaakt van een specifiek blogbericht te berekenen. Als u wilt zoeken in de tegenovergestelde richting inschakelen en alle blogberichten die delen van een bepaald label kunt vinden, kunt u een andere set waarin de blogberichten verwijst naar de tag-ID in de sleutel maken:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Deze structuren kunnen u veel algemene query's zeer efficiënt uitvoeren. U kunt bijvoorbeeld zoeken en weergeven van alle codes voor blogbericht 1 als volgt:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

U vindt alle tags die gebruikt in een blog worden publiceren 1 en weblog posten 2 door een set snijpunt-bewerking als volgt uit te voeren:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

En u vindt alle blogberichten die een bepaald label bevatten:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Zoeken naar recent gebruikte items

Een algemene taak van de vele toepassingen vereist is om te zoeken met de meest recent gebruikte items. Een site bloggen wil bijvoorbeeld informatie over de meest recent gelezen blogberichten weergeven.

Met behulp van een bestand Vgx. lijst kunt u deze functionaliteit implementeren. Een lijst bestand Vgx. bevat meerdere items die dezelfde sleutel deelt. De lijst fungeert als een wachtrij tweepuntige. U kunt items push aan beide uiteinden van de lijst met behulp van de LPUSH (druk op links) en opdrachten waarmee RPUSH (druk op rechts). U kunt items aan beide uiteinden van de lijst met behulp van de opdrachten LPOP en RPOP ophalen. U kunt ook een reeks elementen retourneren met behulp van de opdrachten LRANGE en SCHIKKEN.

De onderstaande codefragmenten weergeven hoe kunt u deze bewerkingen uitvoeren met behulp van de bibliotheek StackExchange. In deze code wordt de `BlogPost` type in de voorgaande Column. Als een blogbericht wordt gelezen door een gebruiker, de `IDatabase.ListLeftPushAsync` methode worden de titel van het blogbericht op een lijst die is gekoppeld aan de 'blog:recent_posts' in de cache bestand Vgx.-toets.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Als u meer blogberichten worden gelezen, worden de titels verplaatst naar dezelfde lijst. De lijst is gesorteerd door de volgorde waarin de titels zijn toegevoegd. De meest recent gelezen blogberichten zijn naar de linkerkant van de lijst. (Als het blogbericht dat dezelfde meer dan één keer wordt gelezen, heeft dit meerdere items in de lijst.)

U kunt de titels van de meest recent gelezen berichten weergeven met behulp van de `IDatabase.ListRange` methode. Deze methode heeft de sleutel die de lijst, een beginpunt en een eindpunt bevat. De volgende code haalt de titels van de 10 blogberichten (items van 0 tot en met 9) aan het einde van de meest linkse van de lijst:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Houd er rekening mee dat het `ListRangeAsync` methode worden geen items uit de lijst verwijderd. Hiervoor kunt u de `IDatabase.ListLeftPopAsync` en `IDatabase.ListRightPopAsync` methoden.

Als u wilt voorkomen dat de lijst voor onbepaalde tijd groeiende, kunt u items regelmatig ten door het inkorten van de lijst. Het codefragment Hieronder leest u hoe alles verwijderen, maar de vijf meest linkse items in de lijst:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Een bord opvulteken implementeren

Standaard worden niet de items in een set gehouden in een bepaalde volgorde staan. U kunt een geordende set maken met de opdracht ZADD (de `IDatabase.SortedSetAdd` methode in de bibliotheek StackExchange). De items worden geordend met behulp van een numerieke waarde met de naam van een score die is opgegeven als een parameter voor de opdracht.

Het volgende codefragment wordt de titel van een blogbericht toegevoegd als een geordende lijst. In dit voorbeeld heeft elke blogbericht ook een score-veld met de classificatie van het blogbericht.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

U kunt ophalen de blog berichttitels en scores in oplopende volgorde score met behulp van de `IDatabase.SortedSetRangeByRankWithScores` methode:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] De bibliotheek StackExchange bevat ook de `IDatabase.SortedSetRangeByRankAsync` methode, die geeft als resultaat de gegevens in score volgorde, maar niet de scores weer.

U kunt ook items in aflopende volgorde van scores ophalen en Beperk het aantal items die worden geretourneerd door te bieden aanvullende parameters de `IDatabase.SortedSetRangeByRankWithScoresAsync` methode. Het volgende voorbeeld worden de titels en scores van de bovenste 10 rangschikking blogberichten:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Het volgende voorbeeld wordt de `IDatabase.SortedSetRangeByScoreWithScoresAsync` methode, die u kunt de items die worden geretourneerd die binnen een bepaald score vallen beperken bereik:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Bericht met behulp van kanalen

Afgezien van fungeert als een gegevenscache, biedt een server bestand Vgx. messaging via een krachtige publisher/abonnee. Clienttoepassingen kunnen u abonneren op een kanaal en andere toepassingen of services berichten kunnen publiceren naar het kanaal. Abonneren toepassingen, klikt u vervolgens deze berichten ontvangt, en ze kunnen worden verwerkt.

Bestand Vgx., vindt u de opdracht ABONNEREN voor clienttoepassingen gebruiken om u te abonneren op kanalen. Deze opdracht verwacht dat de naam van een of meer kanalen waarop de toepassing berichten worden geaccepteerd. De bibliotheek StackExchange bevat de `ISubscription` interface, waarmee een .NET Framework-toepassing abonneren en publiceren naar kanalen.

U maakt een `ISubscription` object met behulp van de `GetSubscriber` methode van de verbinding met de server bestand Vgx.. Klik u op berichten op een kanaal met behulp van beluisteren de `SubscribeAsync` methode van dit object. Het volgende voorbeeld ziet u hoe u een abonnement nemen op een kanaal met de naam 'berichten: blogPosts':

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

De eerste parameter voor de `Subscribe` methode is de naam van het kanaal. Deze naam volgt dezelfde conventies die worden gebruikt door de toetsen in de cache. De naam bevatten een binaire gegevens, hoewel het raadzaam relatief korte, passende tekenreeksen gebruiken om ervoor te zorgen goede prestaties en onderhoud is.

Houd er ook rekening mee dat de naamruimte die wordt gebruikt door kanalen los van dat wordt gebruikt door de toetsen staat. Dit betekent dat u kunt Hoewel het is mogelijk uw toepassingscode meer moeilijk dat te onderhouden kanalen en sleutels zijn met dezelfde naam hebben.

De tweede parameter is een gemachtigde actie. Deze gemachtigde worden asynchroon uitgevoerd wanneer een nieuw bericht wordt weergegeven op het kanaal. In dit voorbeeld wordt het bericht gewoon weergegeven op de console (het bericht bevat de titel van een blogbericht).

Als u wilt publiceren naar een kanaal, kan een toepassing kan de opdracht bestand Vgx. publiceren gebruiken. De bibliotheek StackExchange biedt de `IServer.PublishAsync` methode voor deze bewerking. Het volgende codefragment ziet u hoe het publiceren van een bericht naar het kanaal 'berichten: blogPosts':

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Er zijn verschillende punten die u over de om publicaties/abonnementen weten moet:

- Meerdere abonnees kunnen u abonneren op hetzelfde kanaal en ontvangt deze persoon alle berichten die zijn gepubliceerd naar dat kanaal.
- Abonnees ontvangen alleen berichten die zijn gepubliceerd wanneer ze zich hebt aangemeld. Kanalen niet buffer worden opgeslagen en als een bericht is gepubliceerd, de infrastructuur bestand Vgx. verplaatst u het bericht naar elke abonnee en vervolgens verwijderd.
- Standaard worden berichten ontvangen door abonnees in de volgorde waarin deze worden verzonden. In een zeer actieve systeem met een groot aantal berichten en veel abonnees en uitgevers kunt garandeert opeenvolgende aflevering van berichten vertragen van het systeem. Als u elk bericht onafhankelijk is en de volgorde niet belangrijk is, kunt u gelijktijdige verwerking inschakelen door het bestand Vgx. systeem helpen kan om reactiesnelheid. U kunt dit bereiken in een client StackExchange door in te stellen van de PreserveAsyncOrder van de verbinding die wordt gebruikt door de abonnee onwaar:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen

Het volgende patroon zijn mogelijk ook relevant zijn voor uw scenario wanneer u implementeert in cache opslaan in uw toepassingen:

- [Cache grond patroon](http://msdn.microsoft.com/library/dn589799.aspx): dit patroon wordt beschreven hoe u de gegevens op aanvraag laden in de cache van een gegevensopslag. Dit patroon kunt u ook consistentie tussen gegevens die in de cache wordt bewaard en de gegevens in de oorspronkelijke gegevensopslag.
- Het [patroon Sharding](http://msdn.microsoft.com/library/dn589797.aspx) vindt u informatie over het implementeren van horizontaal partitioneren om u te helpen verbeteren schaalbaarheid bij het opslaan en openen van grote hoeveelheden gegevens.

## <a name="more-information"></a>Meer informatie

- De pagina [MemoryCache class](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) op de website van Microsoft
- De pagina [Azure bestand Vgx. Cache documentatie](https://azure.microsoft.com/documentation/services/cache/) op de website van Microsoft
- De pagina [Azure bestand Vgx. Cache Veelgestelde vragen over](redis-cache/cache-faq.md) op de website van Microsoft
- De pagina [configuratiemodel](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) op de website van Microsoft
- De pagina [Asynchroon patroon op basis van een taak](http://msdn.microsoft.com/library/hh873175.aspx) op de website van Microsoft
- De pagina [pijpleidingen en multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) op de cessies‑retrocessies StackExchange.Redis GitHub
- De pagina [bestand Vgx. permanente](http://redis.io/topics/persistence) op de website van bestand Vgx.
- De [replicatie-pagina](http://redis.io/topics/replication) op de website van bestand Vgx.
- De pagina [bestand Vgx. cluster zelfstudie](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx.
- De [partitionering: het splitsen van gegevens tussen meerdere exemplaren bestand Vgx.](http://redis.io/topics/partitioning) pagina op de website bestand Vgx.
- De pagina [Gebruik van het bestand Vgx. Als een Recentelijk-Cache](http://redis.io/topics/lru-cache) op de website van bestand Vgx.
- De pagina [transacties](http://redis.io/topics/transactions) op de website van bestand Vgx.
- De pagina [bestand Vgx. beveiliging](http://redis.io/topics/security) op de website van bestand Vgx.
- De pagina [Lap rond Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) in het blog van Azure
- De pagina [Waarop het bestand Vgx. Klik op een CentOS Linux VM in Azure wordt aangegeven](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) op de website van Microsoft
- De pagina [ASP.NET-sessie staat-provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-session-state-provider.md) op de website van Microsoft
- De pagina [ASP.NET uitvoer cache-provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) op de website van Microsoft
- De pagina [Een inleiding tot het bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx.
- De pagina [basisgebruik](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) op de website van StackExchange.Redis
- De pagina [transacties in het bestand Vgx.](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) Klik op de cessies‑retrocessies StackExchange.Redis
- De [Data partities guide](http://msdn.microsoft.com/library/dn589795.aspx) op de website van Microsoft
