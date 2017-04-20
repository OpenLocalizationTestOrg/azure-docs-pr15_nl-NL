<properties
   pageTitle="Herstel voor Azure-toepassingen | Microsoft Azure"
   description="Technisch overzicht en gedetailleerde informatie over het ontwerpen van toepassingen voor herstel op Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Herstel voor toepassingen gebaseerd op Microsoft Azure

Dat beschikbaarheid informatie over het beheer van de tijdelijke fout is, is herstel (DR) over de kritieke verlies van functionaliteit. Stel het scenario waar een gebied uitvalt. In dit geval moet u een abonnement wilt uitvoeren van uw toepassing of toegang tot uw gegevens buiten het gebied Azure hebt. Uitvoering van dit abonnement heeft betrekking op personen, processen en ondersteunende applications waardoor het systeem-functie. Het niveau van de functionaliteit voor de service bepalen de bedrijven en technologie eigenaren die van het systeem operationele modus voor op een noodgevallen definiëren ook tijdens een noodgevallen. Het niveau van functionaliteit kan een paar vormen aannemen: helemaal niet beschikbaar, gedeeltelijk beschikbaar (niet beschikbaar is weergegeven functionaliteit of verwerking vertraagd), of volledig beschikbaar.

##<a name="azure-disaster-recovery-features"></a>Azure noodgevallen herstelfuncties

Als u met de beschikbaarheid van overwegingen, heeft Azure [tolerantie technische richtlijnen](./resiliency-technical-guidance.md) die heeft ter ondersteuning van herstel. Er is ook een relatie tussen enkele van de beschikbaarheidfuncties van Azure en noodherstel. Bijvoorbeeld: het beheer van rollen voor foutenstructuuranalyse domeinen Hiermee verhoogt u de beschikbaarheid van een toepassing. Een hardwarefout onverwerkte zou het "" fouten worden zonder dat beheer. Zodat de juiste toepassing van de van de beschikbaarheidsfuncties en strategieën een belangrijk onderdeel van noodgevallen taalprogramma is uw toepassing. In dit artikel gaat echter verder dan van problemen met de algemene beschikbaarheid op ernstige (en sommige) noodgevallen gebeurtenissen.

##<a name="multiple-datacenter-regions"></a>Meerdere datacenter regio 's

Azure onderhoudt datacenters in veel regio's overal ter wereld. Deze infrastructuur ondersteunt verschillende noodgevallen herstel scenario's, zoals de geleverde systeem geografische-replicatie van Azure-opslag voor secundaire gedeelten. Het betekent ook dat u eenvoudig en goedkoop een cloudservice op meerdere locaties overal ter wereld implementeren kunt. Vergelijk deze met de kosten en moeite van het uitvoeren van uw eigen datacenters in meerdere regio's. Gegevens en -services implementeren naar meerdere gebieden voorkomt dat primaire bijvoorbeeld in een één gebied op de toepassing.

##<a name="azure-traffic-manager"></a>Azure verkeer Manager

Wanneer een land / regiospecifieke fout optreedt, moet u verkeer omleiden naar services of implementaties in een andere regio. U kunt doen met dit routering handmatig, maar het is efficiënter een geautomatiseerde proces wilt gebruiken. Azure verkeer Manager is bedoeld voor deze taak. U kunt deze automatisch beheren de overname van gebruikersverkeer naar een andere regio geval de primaire regio is mislukt. Omdat verkeer management een belangrijk onderdeel van de algemene strategie is, is het is belangrijk is voor meer informatie over de basisbeginselen van verkeer Manager.

In het volgende diagram wordt gebruikers verbinding met een URL die is opgegeven voor het verkeer Manager (__http://myATMURL.trafficmanager.net__) en die samenvattingen van de werkelijke site-URL's (__http://app1URL.cloudapp.net__ en __http://app2URL.cloudapp.net__). Afhankelijk van hoe u de criteria voor configureren wanneer u zich aan de route gebruikers gebruikers verzonden naar de juiste werkelijke site wanneer het beleid bepaalt. De beleidsopties zijn round robin, prestaties of failover. Voor dit artikel, we worden weerspiegeld alleen de optie failover.

![Routering via Azure verkeer Manager](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Wanneer u verkeer Manager configureren bent, kunt u een nieuwe verkeer Manager DNS-voorvoegsel opgeven. Dit is de URL-voorvoegsel dat u aan uw gebruikers voor toegang tot uw service. Verkeer Manager abstracts nu één niveau omhoog en niet op het regioniveau van taakverdeling. De DNS verkeer Manager wordt toegewezen aan een CNAME voor alle implementaties die deze beheert.

Binnen verkeer Manager u de prioriteit van de implementaties die gebruikers worden doorgestuurd naar wanneer een fout optreedt. Verkeer Manager bewaakt de eindpunten van de implementaties en notities wanneer de primaire implementatie is mislukt. Mislukt, verkeer Manager analyseert de prioriteit lijst met implementaties en stuurt gebruikers naar het volgende document in de lijst.

Hoewel verkeer Manager wordt bepaald waar kunt u in een failover, kunt u bepalen of uw domein failover inactieve of actief is terwijl u niet in failover-modus bent. Deze functionaliteit heeft niets te doen met Azure verkeer Manager. Verkeer Manager wordt een fout in de primaire site gedetecteerd en uitgevouwen via tot de site failover. Verkeer Manager uitgevouwen via ongeacht of deze site momenteel gebruikers al dan niet fungeert.

Raadpleeg voor meer informatie over de werking van Azure verkeer Manager:

 * [Overzicht van verkeer Manager](../traffic-manager/traffic-manager-overview.md)
 * [Failover-mailroutering methode configureren](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure noodgevallen scenario 's

De volgende secties duidelijk verschillende typen scenario's voor noodgevallen. Regio hele serviceonderbrekingen zijn niet de enige oorzaak voor het hele toepassing mislukken. Slechte ontwerpen of beheer fouten kunnen ook leiden tot bijvoorbeeld. Het is belangrijk zijn de mogelijke oorzaken van een fout tijdens het ontwerp en de testfase van uw abonnement herstel aandachtspunten. Een goed plan maakt gebruik van Azure functies en wordt deze met toepassingsspecifieke strategieën. Het door u gekozen antwoord wordt bepaald door de urgentie van de toepassing, de herstel punt doelstelling (vrijgegeven Productieorder) en de herstel tijd doelstelling (RTO).

###<a name="application-failure"></a>Toepassing is mislukt

Azure verkeer Manager verwerkt automatisch mislukken als gevolg van de onderliggende besturingssysteem of hardware software in de virtuele host machine. Azure maakt een nieuw exemplaar van de rol op een server functioneel en toegevoegd aan de draaiing van taakverdeling. Als het aantal exemplaren van de rol groter dan één, Azure verschuivingen verwerken in de andere actieve rol exemplaren van het knooppunt moeten worden vervangen is.

Zijn er ernstige toepassingsfouten die afzonderlijk van het besturingssysteem of hardware fouten plaatsvinden. De toepassing mogelijk vast vanwege de fatale uitzonderingen die worden veroorzaakt door ongeldige logica of gegevens integriteit problemen. U moet voldoende telemetrielogboek opnemen in de code, zodat een systeem van toezicht kan mislukt voorwaarden detecteren en een beheerder van de toepassing een melding. Een beheerder die volledige kennis van het herstelproces noodgevallen heeft kunt een besluit om aan te roepen een failoverproces aanbrengen. Een beheerder kunt u kunt ook gewoon een storing beschikbaarheid om op te lossen de kritieke fouten.

###<a name="data-corruption"></a>Beschadiging

Azure automatisch de gegevens worden opgeslagen Azure SQL-Database en Azure opslagruimte driemaal toch toe in verschillende foutenstructuuranalyse domeinen in de dezelfde regio. Als u geografische-replicatie gebruikt, wordt de gegevens extra driemaal opgeslagen in een andere regio. Echter als beschadigd door die gegevens in de primaire kopie uw gebruikers- of uw toepassing, de gegevens snel wordt overgenomen door de andere exemplaren. Helaas, levert deze drie kopieën van beschadigde gegevens.

Als u wilt beheren mogelijke beschadigde bestanden van uw gegevens, hebt u twee opties. U kunt eerst een aangepaste back-up strategie beheren. U kunt uw back-ups opslaan in Azure of on-premises implementatie, afhankelijk van uw vereisten voor bedrijven of een beheermodel regelgeving. Er is een andere optie u met de nieuwe point-in-time herstellen-optie voor het herstellen van een SQL-database. Zie de sectie op [gegevens strategieën voor het herstel](#data-strategies-for-disaster-recovery)voor meer informatie.

###<a name="network-outage"></a>Netwerkstoring

Wanneer de onderdelen van de Azure netwerk niet toegankelijk zijn, kunt u mogelijk geen toegang hebt tot uw toepassing of gegevens. Als een of meer exemplaren van de rol niet beschikbaar vanwege netwerkproblemen zijn, wordt de resterende beschikbaar exemplaren van de toepassing in Azure gebruikt. Als uw toepassing geen toegang de gegevens vanwege een netwerkstoring Azure tot, kunt u mogelijk uitvoeren in slechter lokaal met behulp van in de cache opgeslagen gegevens. U moet de noodherstelplan voor het uitvoeren van in slechter in uw toepassing bouwen. Voor sommige-toepassingen, kan dit niet praktisch zijn.

Er is een andere optie voor de opslag van gegevens in een andere locatie, totdat de verbinding is hersteld. Als slechter een optie is, wordt de resterende opties zijn toepassing downtime of failover naar een andere regio. Het ontwerp van een toepassing wordt uitgevoerd in slechter is zo veel mogelijk een zakelijke beslissing als een technische. Hiermee wordt besproken verdere in de sectie op [niet beschikbaar is weergegeven functionaliteit van de toepassing](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Fout in een afhankelijke service

Azure biedt veel services die periodiek downtime kunnen ondervinden. Houd rekening met [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/) als voorbeeld. Deze meerdere tenant-service biedt in de cache mogelijkheden in uw toepassing. Het is belangrijk u rekening moet houden wat gebeurt er in uw toepassing als de afhankelijke service niet beschikbaar is. Dit scenario is op tal van manieren, vergelijkbaar met het netwerk storing scenario. Echter bezig elke service onafhankelijk resulteert in mogelijke verbeteringen aan de algemene indeling.

Azure bestand Vgx. Cache biedt caching aan uw toepassing vanuit de cloud-service implementatie waarmee noodgevallen herstel voordelen. Eerst de service wordt nu uitgevoerd op rollen die lokaal in uw implementatie zijn. U bent dus beter kunt controleren en de status van de cache als onderdeel van uw algehele processen voor de cloudservice beheren. Dit soort caching beschrijft ook nieuwe functies. Een van de nieuwe functies is in de cache opgeslagen gegevens voor maximale beschikbaarheid. Hiermee kunt u in de cache opgeslagen gegevens behouden als één knooppunt mislukt door het bijhouden van dubbele kopieën op andere knooppunten.

Houd er rekening mee dat beschikbaarheid wordt verlaagd doorvoer en latentie verhogen vanwege het bijwerken van de secundaire kopie bij het schrijven. Dit wordt ook de hoeveelheid geheugen die wordt gebruikt voor elk item verdubbeld, dus voor die plannen. In dit specifieke voorbeeld ziet u dat elke afhankelijke service mogelijkheden ter verbetering van de algemene beschikbaarheid en weerstand naar kritieke fouten optreden mogelijk hebben.

Met elke afhankelijke service, moet u de gevolgen van een service-onderbreking begrijpen. In het voorbeeld in de cache, is het mogelijk zijn toegang tot de gegevens rechtstreeks uit een database totdat u de cache herstellen. Dit zou slechter termen van prestatie maar volledige functionaliteit met betrekking tot de gegevens wilt opgeven.

###<a name="region-wide-service-disruption"></a>Regio hele service-onderbreking

De vorige fouten zijn hoofdzakelijk fouten die kunnen worden beheerd binnen dezelfde Azure regio. U moet echter ook de mogelijkheid dat er een service-onderbreking van het gehele gebied is voorbereiden. Als een regio hele service-onderbreking plaatsvindt, zijn de lokaal overbodige kopieën van uw gegevens niet beschikbaar. Als u geografische herhaling hebt ingeschakeld, zijn er drie extra exemplaren van uw BLOB's en tabellen in een andere regio. Als u Microsoft gedeclareerd de regio verloren zijn gegaan, remaps Azure alle DNS-vermeldingen naar het gebied geografische gerepliceerd.

>[AZURE.NOTE] Let erop dat u een controle over dit proces niet hebt en dit doet zich alleen voor regio hele service-onderbreking. Reden, moet u zijn afhankelijk van andere toepassingsspecifieke back-strategieën om te bereiken van het hoogste niveau van de beschikbaarheid van. Zie de sectie op [gegevens strategieën voor het herstel](#data-strategies-for-disaster-recovery)voor meer informatie.

###<a name="azure-wide-service-disruption"></a>Azure hele service-onderbreking

In de planning noodgevallen, moet u rekening houden met het volledige bereik van mogelijke systeemfouten. Een van de meest ernstige serviceonderbrekingen zou gebruikmaakt van alle Azure regio's tegelijk. Als u met de andere serviceonderbrekingen, mogelijk u besluit dat u de kans op pesterijen tijdelijke downtime in dat geval kunt uitvoeren. Algemene serviceonderbrekingen die regio's beslaan moet veel sommige dan geïsoleerd serviceonderbrekingen waarbij u gebruikmaakt van afhankelijke services of één regio's.

Echter voor enkele belangrijke toepassingen misschien u dat er een back-abonnement als u ook in dit scenario moeten zijn. Het gebruik van deze gebeurtenis plannen, bevatten mogelijk niet services in een [alternatief cloud](#alternative-cloud) of een [hybride on-premises en cloud oplossing](#hybrid-on-premises-and-cloud-solution).

###<a name="degraded-application-functionality"></a>De functionaliteit anders werken toepassing

Een goed ontworpen toepassing wordt meestal gebruikt voor een verzameling modules die met elkaar via de uitvoering van losse informatie-interchange patronen communiceren. Een toepassing voor de DR-vriendelijke vereist scheiding van taken op het moduleniveau van de. Dit is om te voorkomen dat de verstoring van een afhankelijke service waarmee u de hele toepassing. Stel een webtoepassing commerce voor bedrijf Y. De volgende modules mogelijk, vormen gezamenlijk de toepassing:

 * __Productcatalogus__ kunnen gebruikers om te bladeren producten.
 * __Winkelwagen__ kunnen gebruikers producten in hun winkelwagentje toevoegen/verwijderen.
 * __De Status van__ ziet u de verzendingsstatus van gebruiker orders.
 * __Volgorde indiening__ Hiermee wordt de winkelwagen sessie door in te dienen de volgorde met betaling.
 * De volgorde voor gegevensintegriteit is gevalideerd en voert een uit van de beschikbaarheid van de hoeveelheid __Volgorde verwerkt__ .

Wanneer een afhankelijke van een module in deze toepassing uitvalt, hoe de module werkt totdat dit gedeelte herstelt? Een goede architectuur systeem moeten worden geïsoleerd grenzen tot en met scheiding van taken die tijdens de ontwerpfase zowel gedurende runtime geïmplementeerd. U kunt elke fout als worden hersteld en niet-herstelbare categoriseren. Niet-herstelbare fouten duurt omlaag de module, maar u kunt een herstelbare fout tot en met alternatieven beperken. Zoals beschreven in de sectie hoge beschikbaarheid, kunt u enkele problemen van gebruikers verbergen door de verwerking van fouten en alternatieve acties uitvoert. Tijdens een meer ernstige service-onderbreking mogelijk aan de toepassing niet volledig beschikbaar. Een derde optie is echter blijven onderhouden van gebruikers in slechter.

Als de database voor het hosten van orders uitvalt, verliest de module doorvoeren bijvoorbeeld beter verkoop transacties verwerken. Afhankelijk van de architectuur, kan het zijn moeilijk of waardoor voor het indienen van de Order en doorvoeren onderdelen van de toepassing om door te gaan. Als de toepassing niet worden verwerkt in dit scenario is ontworpen, is de hele toepassing mogelijk gaat offline.

In dit scenario hetzelfde is het echter mogelijk dat de productgegevens in een andere locatie is opgeslagen. In dat geval kan de module productcatalogus nog steeds worden gebruikt voor het weergeven van de producten. In slechter blijft de toepassing beschikbaar zijn voor gebruikers voor beschikbare functies zoals de productcatalogus weergeven. Andere delen van de toepassing, zijn echter niet beschikbaar, zoals bestellen of voorraad query's.

Een andere variatie van slechter centers op prestaties in plaats van mogelijkheden. Stel een scenario waarbij de productcatalogus in de cache via Azure bestand Vgx. Cache. Als caching niet beschikbaar is, wordt de toepassing mogelijk Ga rechtstreeks naar de opslag van de server om op te halen catalogus productinformatie. Maar het is mogelijk dat deze toegang langzamer dan de in de cache opgeslagen versie. Reden de toepassingsprestaties is niet beschikbaar is weergegeven totdat de in de cache-service volledig is hersteld.

Bepalen hoeveel van een toepassing blijft functie in slechter is zowel een zakelijke beslissing en een technische besluit. De toepassing moet ook bepalen hoe op de hoogte brengt u de gebruikers van de tijdelijke problemen. In dit voorbeeld de toepassing mogelijk weergeven van producten en deze zelfs toe te voegen aan een winkelwagen. Echter wanneer de gebruiker probeert een aankoop, krijgt de toepassing de gebruiker dat de module sales tijdelijk niet toegankelijk is. Het is niet ideaal voor de klant, maar deze een hele toepassing service-onderbreking voorkomt.

##<a name="data-strategies-for-disaster-recovery"></a>Gegevens strategieën voor het herstel

Verwerking van gegevens correct is het moeilijkst gebied direct in gaan. Gegevens herstellen is ook het gedeelte van het herstelproces die meestal duurt de meeste tijd. Verschillende keuzen in verslechtering van modi resulteren in moeilijk uitdagingen om gegevens te herstellen uit is mislukt en consistentie na is mislukt.

Een van de factoren is dat u wilt terugzetten, of voor het behoud van een kopie van de gegevens van de toepassing. Deze gegevens wilt gebruiken voor verwijzing en transacties doeleinden op een secundaire site. Een on-premises instellen vereist een dure en lange planningsproces willen implementeren van een noodherstelplan meerdere regio. De meeste providers van de cloud, met inbegrip van Azure toestaan gemakkelijk gemakkelijk de distributie van toepassingen aan meerdere landen. Deze gebieden worden geografisch verspreid zodanig dat meerdere regio service-onderbreking zelden moet zijn. De strategie voor het afhandelen van gegevens tussen regio's is een van de factoren voor het al dan niet geslaagde gaan.

De volgende secties worden noodgevallen technieken betrekking hebben op back-ups van gegevens, referentiegegevens en transacties gegevens.

###<a name="backup-and-restore"></a>Back-up en herstellen

Regelmatige back-ups van toepassingsgegevens kunnen sommige noodgevallen herstel scenario's ondersteunen. Verschillende opslag resources vereisen verschillende technieken.

Voor de lagen Basic, Standard en Premium SQL-Database, kunt u profiteren van point-in-time herstellen herstellen van uw database. Zie voor meer informatie [Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md). Er is een andere optie actieve geografische-herhaling gebruiken voor SQL-Database. Dit automatisch wordt overgenomen door wijzigingen in de database secundaire databases in de dezelfde Azure regio of zelfs in een ander gebied van de Azure. Hiermee kunt u een mogelijke alternatieve deel van de meer handmatige synchronisatie-technieken voor gegevens is in dit artikel beschreven. Zie voor meer informatie [Overzicht: SQL actieve geografische-databasereplicatie](../sql-database/sql-database-geo-replication-overview.md).

U kunt ook meer handmatig gebruiken voor back-up en herstellen. Gebruik de opdracht DATABASE kopiëren om een kopie van de database te maken. U moet deze opdracht gebruiken om een back-up met transacties consistentie. U kunt ook de service importeren/exporteren van Azure SQL-Database. Dit ondersteunt geëxporteerd databases tot Bacpac-bestanden die zijn opgeslagen in Azure-blobopslag.

Ingebouwde redundante Azure Storage Hiermee maakt u twee kopieën van de back-upbestand in dezelfde regio. De frequentie van de uitvoering van de back-up bepaalt echter uw vrijgegeven Productieorder, namelijk de hoeveelheid gegevens die in noodgevallen scenario's kunnen verloren gaan. Stel dat u een back-up aan de bovenkant van het uur uitvoeren en een noodgevallen twee minuten voordat u de bovenkant van het uur plaatsvindt. U raakt 58 minuten gegevens die is er gebeurd nadat de laatste back-up is uitgevoerd. Als u wilt beveiligen tegen een regio hele service-onderbreking, moet u ook de Bacpac-bestanden kopiëren naar een andere regio. U hebt de optie van deze back-ups die u in de alternatieve regio terugzetten. Zie voor meer informatie, [Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md).

U kunt voor Azure-opslag, het ontwikkelen van uw eigen aangepaste back-up of gebruik een van de veel-back-hulpprogramma's van derden. Houd er rekening mee dat hebben de meeste toepassing ontwerpen extra complexiteit waar opslag resources verwijzen naar elkaar. Bekijk bijvoorbeeld een SQL-database die een kolom die is gekoppeld aan een blob in Azure opslag heeft. Als de back-ups niet tegelijkertijd worden uitgevoerd, is de database mogelijk de aanwijzer een blob niet back-upgegevens vóór de storing. De toepassing of gaan moet processen voor het verwerken van deze inconsistenties na een herstel implementeren.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Verwijzing gegevenspatroon voor herstel

Referentiegegevens is alleen-lezen-gegevens die ondersteuning biedt voor functionaliteit van de toepassing. Dit meestal niet vaak worden gewijzigd. Hoewel back-up en herstellen is één methode worden afgehandeld regio hele serviceonderbrekingen, is de RTO relatief lange. Wanneer u de toepassing naar een secundaire gebied implementeert, kan sommige strategieën de RTO voor referentiegegevens kunnen verbeteren.

Omdat verwijzen naar gegevenswijzigingen u kunt de RTO af en toe verbeteren door een kopie van de verwijzingsgegevens in de tweede regio. Hierdoor wordt de benodigde tijd voor het herstellen van back-ups van een storing. Om te voldoen aan meerdere regio noodgevallen herstel, moet u de toepassing en het samen referentiegegevens in meerdere regio's implementeren. Zoals in de [verwijzing gegevenspatroon beschikbaarheid](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability)vermeld, kunt u referentiegegevens implementeren aan de rol zelf, externe opslag of een combinatie van beide.

De verwijzing implementatie gegevensmodel in berekeningscluster knooppunten aansluiten impliciet op de vereisten van de herstel noodgevallen. Verwijzing gegevens implementatie met SQL-Database is vereist dat u een kopie van de referentiegegevens dashboard naar elke regio implementeren. De dezelfde strategie geldt voor de opslag van Azure. U moet een kopie van een verwijzingsgegevens die zijn opgeslagen in de opslagruimte van de Azure naar de gebieden primaire en secundaire implementeren.

![Verwijzing gegevens publicatie voor primaire en secundaire gedeelten](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

U moet uw eigen toepassingsspecifieke back-routines voor alle gegevens, waaronder referentiegegevens implementeren. Geografische gerepliceerd kopieën tussen regio's worden alleen gebruikt in een regio hele service-onderbreking. Als u wilt voorkomen dat uitgebreide downtime, de belangrijke onderdelen van de gegevens van de toepassing dashboard implementeren naar de tweede regio. Zie voor een voorbeeld van deze topologie, het [actieve-passieve model](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Transacties gegevenspatroon voor herstel

Uitvoering van een strategie van de modus volledig functioneel noodgevallen vereist asynchroon herhaling van de transacties gegevens naar de tweede regio. De praktische tijdvensters waarbinnen de replicatie kan plaatsvinden bepaalt de vrijgegeven Productieorder kenmerken van de toepassing. U kunt nog steeds de gegevens die zich in de primaire regio gaan verloren tijdens het venster herhaling herstellen. U kunt mogelijk ook later samenvoegen met de tweede regio.

De volgende voorbeelden van de architectuur bieden ideeën op verschillende manieren van het afhandelen van transacties gegevens in een scenario failover. Het is belangrijk in deze voorbeelden zijn niet volledig. Bijvoorbeeld worden tussenliggende opslaglocaties zoals wachtrijen vervangen door Azure SQL-Database. De wachtrijen zelf mogelijk Azure Storage of Azure Service Bus wachtrijen (Zie [Azure wachtrijen en Service Bus wachtrijen--vergeleken en in afgezet tegen](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Server opslag bestemmingen ook variëren, zoals Azure tabellen in plaats van de SQL-Database. Bovendien kunnen werknemer rollen worden ingevoegd als tussenpersonen in verschillende stappen. Het belangrijkste is niet op deze architecturen precies emuleert, maar u rekening moet houden de verschillende alternatieven bij het herstellen van transacties gegevens en gerelateerde modules.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replicatie van transacties gegevens voorbereiding op problemen oplossen

Houd rekening met een toepassing die gebruikmaakt van Azure Storage wachtrijen hebben voor transacties gegevens. Hiermee kunt medewerker rollen om de transacties gegevens met de server-database in een losgekoppelde architectuur te verwerken. U moet hiervoor de transacties naar een bepaalde vorm tijdelijke cache als de front rollen de onmiddellijke query van die gegevens vereisen gebruikt. Afhankelijk van het niveau van gegevensverlies tolerantie kunt u de wachtrijen, de database of alle opslagbronnen repliceren. Alleen databasereplicatie, als de primaire regio uitvalt, kunt u nog steeds terugzetten de gegevens in de wachtrijen wanneer de primaire regio terugkomt.

In het volgende diagram ziet u een architectuur waar de server-database wordt gesynchroniseerd tussen regio's.

![Replicatie van transacties gegevens voorbereiding op problemen oplossen](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

De grootste uitdaging voor de uitvoering van deze architectuur is de strategie herhaling tussen regio's. De synchronisatie van gegevens SQL Azure-service kan dit type herhaling. De service is echter nog steeds in de Preview-versie en wordt niet aanbevolen voor productieomgevingen. Zie voor meer informatie [Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md). Voor productietoepassingen, moet u uw eigen logica herhaling in code maken of investeren in een oplossing van derden. Afhankelijk van de architectuur, de replicatie mogelijk bidirectionele, namelijk ook complexere.

Een mogelijke implementatie mogelijk maken gebruik van de tussenliggende wachtrij in het vorige voorbeeld. De rol van de werknemer die de gegevens op de bestemming definitieve bewaring verwerkt mogelijk wijzigingen aanbrengen in zowel de primaire regio en de tweede regio. Dit zijn niet alledaagse taken en voltooid richtlijnen voor replicatie code valt buiten het bereik van dit artikel. Het belangrijk punt is dat een groot aantal uw tijd en testen moet richten op hoe u uw gegevens naar de tweede regio repliceren. Aanvullende processing en testen kunt ervoor zorgen dat de processen failover- en herstelbestanden correct mogelijke gegevensinconsistenties of dubbele transacties verwerken.

>[AZURE.NOTE] Meest van dit artikel bevat informatie over platform als een service (PaaS). Extra replicatie en beschikbaarheid van de opties voor hybride-toepassingen gebruiken echter Azure virtuele Machines. Deze toepassingen hybride infrastructuur gebruiken als een service (IaaS) voor het hosten van SQL Server op virtuele machines in Azure wordt aangegeven. Hierdoor wordt de beschikbaarheid van de traditionele methoden in SQL Server, zoals AlwaysOn beschikbaarheid van de groepen of Log verzending. Sommige technieken, zoals AlwaysOn, werken alleen tussen on-premises implementatie SQL Server-exemplaren en Azure virtuele machines. Zie [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)voor meer informatie.

####<a name="degraded-application-mode-for-transaction-capture"></a>Anders werken toepassingsmodus voor het vastleggen van transactie

Houd rekening met een tweede architectuur die wordt uitgevoerd in slechter. De toepassing op de secundaire regio deactiveert alle functies, zoals rapportage, business intelligence (BI), of wachtrijen leegmaken. Alleen de belangrijkste soorten transacties werkstromen worden geaccepteerd gedefinieerd door bedrijven vereisten. Het systeem wordt vastgelegd de transacties en schrijft ze naar wachtrijen. Het systeem mogelijk het verwerken van de gegevens in de eerste fase van de service-onderbreking uitstellen. Als het systeem op de primaire regio opnieuw binnen het verwachte tijdvenster wordt geactiveerd, kunnen de werknemer rollen in de primaire regio de wachtrijen corrigeren. Dit proces hoeven voor het samenvoegen van de database. Als de primaire regio service-onderbreking buiten het toegestane venster gaat, kan de toepassing kunt starten voor het verwerken van de wachtrijen.

In dit scenario bevat de database op de secundaire incrementele transacties gegevens die moeten worden samengevoegd nadat de primaire opnieuw wordt geactiveerd. In het volgende diagram ziet u deze strategie voor het tijdelijk opslaan van transacties gegevens totdat de primaire regio is hersteld.

![Anders werken toepassingsmodus voor het vastleggen van transactie](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Zie voor meer informatie over de data management technieken voor robuuste Azure-toepassingen, [Failsafe: richtlijnen voor het robuuste Cloud architecturen](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Implementatietopologieën voor herstel

U moet essentiële toepassingen voor de mogelijkheid van een regio hele service-onderbreking voorbereiden. Dit doet u door het opnemen van een implementatiestrategie meerdere regio-in de operationele planning.

Meerdere regio implementaties mogelijk gebruikmaakt van IT pro-processen voor het publiceren van de toepassing- en verwijzingsfuncties gegevens naar de tweede regio na een noodgevallen. Als de toepassing direct failover, kan het implementatieproces gebruikmaakt van een actieve/passieve-instelling of een actieve/actieve-instelling. Dit soort implementatie heeft bestaande exemplaren van de toepassing actief in de alternatieve regio. Een routeren hulpmiddel zoals Azure verkeer Manager biedt taakverdeling services op het niveau van DNS. Hiermee kunt serviceonderbrekingen gedetecteerd en de gebruikers doorsturen naar verschillende regio's als dat nodig is.

Onderdeel van een succesvolle Azure herstel is die herstel uitbreidt in de oplossing vanaf het begin. De cloud vindt u aanvullende opties voor het herstellen van fouten tijdens een noodgevallen die niet beschikbaar in een traditioneel hostingprovider zijn. Specifiek, kunt u snel en dynamisch resources toewijzen aan een ander gebied. Daarom betalen u veel voor niet-actieve resources terwijl u voor een fout wacht weergegeven.

De volgende secties duidelijk verschillende implementatietopologieën voor herstel. Er is meestal een verhouding in extra kosten of complexiteit voor extra beschikbaarheid.

###<a name="single-region-deployment"></a>Regio-implementatie

Een regio-implementatie is niet echt een noodgevallen herstel topologie, maar is bedoeld om het contrast met de andere architecturen weer. Regio-implementaties gelden voor toepassingen in Azure wordt aangegeven. Dit soort implementatie is echter niet een ernstige deelnemer voor een gaan.

In het volgende diagram ziet u een toepassing wordt uitgevoerd in een enkel Azure regio. Beschikbaarheid van de toepassing in het gebied vergroten Azure verkeer Manager en het gebruik van domeinen voor foutenstructuuranalyse en -upgrade

![Regio-implementatie](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Hier is het blijkt dat de database een potentieel risico. Hoewel de gegevens van Azure worden gerepliceerd verschillende verschillende foutenstructuuranalyse domeinen interne replica's, wordt dit alles weergegeven in dezelfde regio. De toepassing niet kunt een volledige uitval berekend. Als het gebied uitvalt, gaan alle foutenstructuuranalyse domeinen omlaag--inclusief alle exemplaren van de service en opslag resources.

Voor alle behalve de minst kritieke toepassingen, moet u een plan voor het implementeren van uw toepassingen tussen meerdere regio's ontwerpen. U moet ook kunt u RTO en kosten van de beperkingen bij het gebruik van de topologie van welke implementatie onderzoek.

Laten we eens kijken nu specifieke patronen ter ondersteuning van failover tussen de verschillende regio's. In deze voorbeelden alle gebruikt twee regio's om het proces te beschrijven.

###<a name="redeployment-to-a-secondary-azure-region"></a>Opnieuw installeren naar een secundaire Azure gebied

In het patroon van opnieuw installeren op een secundaire regio bevat alleen de regio van de primaire-toepassingen en databases die worden uitgevoerd. De tweede regio is niet ingesteld voor een automatische failover. Wanneer een noodgevallen plaatsvindt, kunt u moet dus voor kringvelden van alle onderdelen van de service in de nieuwe regio. Dit geldt ook voor een cloudservice uploaden naar Azure, de cloudservice implementeert, herstelt de gegevens en DNS het verkeer wordt omgeleid wijzigen.

Hoewel dit het meest betaalbare van de opties meerdere regio is, heeft dit de slechtste RTO-kenmerken. In dit model, de service-pakket en database back-ups zijn opgeslagen beide on-premises implementatie of in het Azure Blob storage-exemplaar van de tweede regio. U moet echter implementeren van een nieuwe service en de gegevens te herstellen voordat deze bewerking weer. Zelfs als u de overdracht van gegevens uit de back-opslag volledig automatiseren, gebruikt ronddraait om de nieuwe databaseomgeving veel tijd. Gegevens uit de back-upschijf opslag verplaatsen naar de lege database op de secundaire regio, is het meest dure gedeelte van het herstelproces. U moet dit echter doen om de nieuwe database naar een werkende status omdat deze niet wordt gerepliceerd.

De beste manier is voor de opslag van de servicepakketten in blobopslag in de tweede regio. Hierdoor hoeven het pakket uploaden naar Azure, dat wil wat gebeurt er zeggen wanneer u vanuit een on-premises implementatie ontwikkeling machine implementeren. Met behulp van de PowerShell-scripts kunt u snel de servicepakketten naar een nieuwe cloudservice van Blob storage implementeren.

Deze optie is alleen voor niet-kritieke toepassingen die mogelijk zonder een hoge RTO praktische. Bijvoorbeeld, werkt dit mogelijk voor een toepassing die kan worden omlaag voor enkele uren, maar er moet worden uitgevoerd binnen 24 uur opnieuw.

![Opnieuw installeren naar een secundaire Azure gebied](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Actieve-passieve

Het actieve-passieve patroon is de optie waarop veel bedrijven voorkeur. Dit patroon biedt verbeteringen aan de RTO met een relatief kleine toename van kosten via het patroon opnieuw installeren.
In dit scenario is er een primaire en een secundaire Azure regio. Al het verkeer gaat u naar de actieve implementatie van de primaire regio. De tweede regio is beter voorbereid op herstel, omdat de database op beide regio's wordt uitgevoerd. Bovendien is een om synchronisatie op hun plaats staan ertussen. Deze methode stand-by kunt gebruikmaakt van twee varianten: een database alleen-lezen-methode of een volledige implementatie in de tweede regio.

####<a name="database-only"></a>Alleen de database

In de eerste variatie van het patroon dat actieve-passieve bevat alleen de primaire regio een toepassing voor de service gedistribueerde cloud. In tegenstelling tot het patroon opnieuw installeren, worden beide regio's echter gesynchroniseerd met de inhoud van de database. (Zie de sectie op [transacties gegevenspatroon voor herstel](#transactional-data-pattern-for-disaster-recovery)voor meer informatie.) Als een noodgevallen plaatsvindt, zijn er minder activering vereisten. U start de toepassing in de tweede regio, verbindingstekenreeksen met de met de nieuwe database wijzigen en wijzigen van de DNS-vermeldingen verkeer wordt omgeleid.

Als het patroon opnieuw installeren, moet zijn al opgeslagen de servicepakketten in Azure-blobopslag in de tweede regio voor snellere implementatie. In tegenstelling tot het patroon opnieuw installeren maken niet u het grootste deel van de realiseren die moeten worden herstellen van database. De database is klaar en wordt uitgevoerd. Hiermee wordt een aanzienlijke hoeveelheid tijd, waardoor deze een betaalbare DR-patroon. Het is ook de populairste DR-patroon.

![Alleen actieve-passieve,-database](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Volledige replica

Hebben een volledige implementatie in de tweede variatie van het patroon dat actieve-passieve, zowel de primaire regio en de tweede regio. Deze installatie bevat de cloudservices en een gesynchroniseerde database. Alleen de primaire regio is echter actief afhandelen van netwerk aanmeldingsaanvragen van de gebruikers. De tweede regio actief alleen als de primaire regio er een service-onderbreking optreedt. In dat geval doorsturen verzoeken om het netwerk van alle nieuwe naar de tweede regio. Azure verkeer Manager kunt automatisch deze failover beheren.

Storing sneller dan de variatie database alleen-lezen omdat de services die al zijn geïmplementeerd. Dit patroon biedt een erg laag RTO. De regio van de secundaire failover moet wilt verzenden direct na mislukken van de primaire regio.

Samen met een sneller antwoord tijd heeft dit patroon het voordeel van vooraf toewijzen en implementeren van back-services. U hoeft niet te zorgen maken over een gebied dat niet hoeft de ruimte nieuwe exemplaren in een noodgevallen toewijzen. Dit is belangrijk als uw secundaire Azure regio capaciteit nadert. Er is geen garantie (service level agreement) dat u direct kunnen een aantal nieuwe cloudservices in elke regio implementeren.

Voor de beste antwoord tijd met dit model moet u dezelfde schaal (het aantal exemplaren van de rol) in de primaire en secundaire regio's. Ondanks de voordelen, betaalt voor niet-gebruikte berekeningscluster exemplaren is dure en dit mogelijk niet de meest voorzichtig financiële keuze. Reden is het komt vaker voor het gebruik van een enigszins gereduceerde versie van cloudservices op de tweede regio. Vervolgens kunt u snel mislukken en schalen uit de secundaire implementatie indien nodig. U kunt het failoverproces te automatiseren, zodat wanneer de primaire regio niet toegankelijk is, u extra exemplaren, afhankelijk van het selectievakje laden activeren. Dit mogelijk gebruikmaakt van het gebruik van een manier autoscaling zoals [VM schaal ingesteld](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

In het volgende diagram ziet u het model waar de primaire en secundaire regio's een volledig geïmplementeerd cloudservice in een actieve-passieve patroon bevatten.

![Actieve-passieve, volledige replica](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Actieve

Nu u bent waarschijnlijk uitzoeken de ontwikkeling van de patronen: verkleinen van de RTO verhogen kosten en complexiteit. De oplossing actieve eindemarkeringen daadwerkelijk deze altijd met aandacht kosten.

In een patroon actieve, worden de cloudservices en de database volledig geïmplementeerd in beide regio's. In tegenstelling tot het model actieve-passieve ontvangen beide regio's gebruiker-verkeer is toegestaan. Deze optie levert de snelste hersteltijd. De services zijn al schaal voor het verwerken van een gedeelte van het selectievakje laden bij elke regio. DNS is al ingeschakeld voor het gebruik van de tweede regio. Er is complexer bij het bepalen van hoe u gebruikers doorsturen naar de juiste regio. Round robin plannen mogelijk. Het is waarschijnlijk meer geneigd dat bepaalde gebruikers een bepaalde regio waarin de primaire kopie van hun gegevens zich bevindt zou gebruiken.

Voor het geval failover, moet u gewoon DNS uitschakelen de primaire regio. Hiermee stuurt alle verkeer door naar de tweede regio.

Zelfs in dit model, zijn er enkele variaties. Het volgende diagram ziet bijvoorbeeld een model waar de primaire regio eigenaar is van het originele exemplaar van de database. De cloudservices in beide regio's schrijven die primaire database. De secundaire implementatie kan van de primaire of gerepliceerde database lezen. Herhaling in dit voorbeeld gebeurt een manier.

![Actieve](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Er is een neerwaartse naar de actieve architectuur in het diagram. De tweede regio moet toegang heeft tot de database in het eerste gebied omdat het originele exemplaar er zich bevindt. Prestaties verdwijnt aanzienlijk wanneer u toegang gegevens die zich buiten een gebied tot. In meerdere regio database bellen, moet u overwegen een type batchen strategie om de prestaties van deze oproepen te verbeteren. Zie voor meer informatie [het gebruik van batchen om SQL-Database toepassingsprestaties te verbeteren](../sql-database/sql-database-use-batching-to-improve-performance.md).

Een alternatief architectuur mogelijk gebruikmaakt van elke regio rechtstreeks toegang tot een eigen database. In dat model, is een type bidirectionele replicatie vereist voor het synchroniseren van de databases in elke regio.

Klik in het actieve patroon moet u wellicht niet zo veel exemplaren van de primaire regio zoals u zou in het actieve-passieve patroon doen. Als u 10 exemplaren op de primaire regio in een actieve-passieve architectuur hebt, moet u mogelijk alleen en met 5 in elke regio in een actieve architectuur. Beide regio's delen nu het selectievakje laden. Dit mogelijk een kosten besparen via het patroon dat in actieve-passieve als u een warme stand-by ingeschakeld houden de passieve regio met 10 exemplaren wachten op failover.

En zich daarna realiseert dat totdat u de primaire regio terugzet, de tweede regio een plotselinge stijging van nieuwe gebruikers ontvangt mogelijk. Als er 10.000 gebruikers op elke server als er een service-onderbreking optreedt in de primaire regio, wordt de secundaire regio ineens worden afgehandeld 20.000 gebruikers heeft. Regels op de secundaire regio Monitoring moet worden gedetecteerd en deze grotere dubbel de exemplaren in de tweede regio. Zie het gedeelte [mislukt](#failure-detection)detectie voor meer informatie hierover.

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybride on-premises en cloud oplossing

Een extra strategie voor het herstel is aan het ontwerp van een hybride-toepassing die wordt uitgevoerd on-premises implementatie en in de cloud. De primaire regio mogelijk beide locaties, afhankelijk van de toepassing. Houd rekening met de vorige architecturen en stel de primaire of secundaire regio als een on-premises locatie.

Er zijn enkele uitdagingen in deze hybride architecturen. Meest van dit artikel heeft eerst PaaS architectuur patronen besproken. Normale PaaS toepassingen in Azure wordt aangegeven, is afhankelijk van Azure-specifieke constructies, zoals rollen, cloudservices en verkeer Manager. Als u wilt maken van een on-premises implementatie-oplossing voor dit type PaaS toepassing vereist een sterk afwijkt architectuur. Dit is mogelijk niet mogelijk gecombineerd uit een management of het perspectief van de kosten.

Een oplossing hybride voor herstel heeft echter minder uitdagingen voor traditionele architecturen die gewoon naar de cloud verplaatst. Dit geldt ook voor architecturen die IaaS gebruiken. IaaS toepassingen via virtuele machines in de cloud dat kunt equivalenten voor directe on-premises implementatie. U kunt ook virtuele netwerken gebruiken om verbinding maken met de computers in de cloud met on-premises implementatie netwerk resources. Hiermee opent u verschillende mogelijkheden die niet mogelijk met PaaS alleen-lezen-toepassingen zijn. Bijvoorbeeld: SQL Server kunt profiteren van noodgevallen hersteloplossingen zoals AlwaysOn beschikbaarheid van de groepen en spiegelen van de database. Zie [beschikbaarheid en herstel voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)voor meer informatie.

IaaS oplossingen kunt u ook een eenvoudiger pad voor on-premises implementatie-toepassingen Azure gebruiken als de optie failover opgeven. Mogelijk hebt u een volledig functioneel toepassing in een bestaande on-premises implementatie regio. Maar wat gebeurt er als u niet over de bronnen voor het behoud van een geografisch afzonderlijk gebied voor failover? U kunt virtuele machines en virtuele netwerken gebruiken om uw toepassing uitgevoerd in Azure wordt aangegeven. In dat geval definiëren processen die worden gesynchroniseerd naar de cloud. De Azure-implementatie vervolgens wordt het secundaire regio voor failover wilt gebruiken. De primaire regio blijft de on-premises implementatie-toepassing. Zie voor meer informatie over IaaS architecturen en mogelijkheden, de [virtuele Machines documentatie](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternatieve cloud

Er zijn situaties waarin zelfs de stabiliteit van het Microsoft Cloud voldoet niet aan interne naleving van regels of het beleid dat uw organisatie is vereist. Zelfs de beste voorbereiding en ontwerpen om te implementeren van back-systemen tijdens een noodgevallen vallen korte als er een algemene service-onderbreking van een cloud-provider is.

U moet de vereiste beschikbaarheid met de kosten en complexiteit van betere beschikbaarheid vergelijken. Een risicoanalyse uitvoeren en de RTO en vrijgegeven Productieorder definiëren voor uw oplossing. Als uw toepassing kan niet elke uitvaltijd zonder, kan het zinvol voor u overwegen om een andere cloud-oplossing. Tenzij het hele Internet uitvalt, een andere cloud-oplossing mogelijk nog steeds beschikbaar zijn als Azure niet algemeen toegankelijk zijn.

Net als met het scenario hybride kunnen ook de failover-implementaties in de vorige noodgevallen herstel architecturen bestaan binnen een oplossing van een andere cloud in. Alternatieve cloud DR sites moeten worden gebruikt alleen voor wie RTO erg weinig of geen, downtime kunt-oplossingen. Houd er rekening mee dat een oplossing op basis van een site DR buiten Azure worden nog genoeg moeten te configureren, ontwikkelen, implementeren en onderhouden. Het is ook moeilijker te implementeren aanbevolen procedures in de architectuur van een cross-cloud. Hoewel cloud platforms dezelfde op hoog niveau concepten hebben, zijn de API's en architecturen verschillende.

Als u besluit uw DR tussen verschillende platforms splitsen, zou het zinvol aan het ontwerp van abstraction lagen in het ontwerp van de oplossing. Als u dit doet, kunt u niet nodig om te ontwikkelen en voor het behoud van twee verschillende versies van dezelfde toepassing voor verschillende cloud platforms voor het geval noodgevallen. Als met het scenario hybride het gebruik van Azure virtuele Machines of Azure Container-Service mogelijk eenvoudiger in dat geval het gebruik van cloud-specifieke PaaS ontwerpen.

##<a name="automation"></a>Automatisering

Enkele van de patronen die we zojuist besproken vragen om snel de activering van offline implementaties en herstel van specifieke onderdelen van een systeem. Ondersteunt de mogelijkheid om te activeren resources op aanvraag en oplossingen snel implementeren automatisering of uitvoeren van scripts. In dit artikel DR-gerelateerde automatisering wordt vergeleken met [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx), maar de [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) is ook een optie.

Het ontwikkelen van scripts helpt bij het beheren van de onderdelen van DR die Azure niet transparant verwerken. Het voordeel van consistente resultaten produceert telkens wanneer, waarin de kans op menselijke fouten minimaliseert is. Lukt vooraf gedefinieerde DR-scripts ook minder tijd om een systeem en componenten ervan door een noodgevallen opnieuw te maken. U wilt niet dat u kunt proberen om te bepalen hoe u uw site herstelt terwijl dit omlaag en verliezende geld elke minuut handmatig.

Nadat u de scripts hebt gemaakt, test u ze herhaaldelijk van begin tot einde. Nadat u hun basisfunctionaliteit controleren, zorg dat u ze in [noodgevallen simulatie](#disaster-simulation)testen. Hiermee kunt schuiven fouten in de scripts of processen.

Een goede gewoonte met automatisering is het opzetten van een bibliotheek van de PowerShell-scripts of opdrachtregel-interface (CLI)-scripts voor Azure herstel. Duidelijk markeren en categoriseren ze voor eenvoudig opzoeken. Wijs een persoon om de bibliotheek en versiebeheer scripts te beheren. Ze het document ook met de beschrijving van parameters en voorbeelden van script gebruiken. Ook voor zorgen dat u deze documentatie synchroon met uw Azure implementaties houden. Dit onderstrepingstekens het doel van één persoon die verantwoordelijk is voor alle onderdelen van de bibliotheek met.

##<a name="failure-detection"></a>Fout bij detectie

Als u wilt oplossen met beschikbaarheid en herstel de juiste manier verwerkt, moet u het volgende kunnen ontdekken en fouten te analyseren. U moet doen geavanceerde server en het monitoren van implementatie, zodat u snel weet wanneer een systeem of onderdelen hiervan ineens niet kunt beschikbaar zijn. Hulpprogramma's waarmee u kijkt u naar de algemene status van de cloudservice en de bijbehorende afhankelijkheden cmdlets voor controle kan onderdeel van dit werk uitvoeren. Een Microsoft-hulpmiddel is [systeem Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Hulpprogramma's van derden biedt ook controlefuncties. De meeste controle kan worden uitgevoerd bijhouden prestatie-items en beschikbare service.

Hoewel deze hulpprogramma's essentieel zijn, vervang ze niet hoeft te plannen voor foutenstructuuranalyse detectie en rapportage binnen een cloudservice. U moet goed gebruik diagnostisch hulpprogramma Azure plannen. Aangepaste prestatie-items of logboekvermeldingen-kunnen ook deel uitmaken van de algemene strategie. Hier vindt u meer gegevens tijdens fouten kunt u snel een diagnose en volledige functionaliteit herstellen. Het biedt ook extra statistieken die de controle hulpmiddelen gebruiken kunnen om te bepalen de toepassingsstatus. Zie [Azure diagnostische hulpprogramma's in Azure Cloud Services inschakelen](../cloud-services/cloud-services-dotnet-diagnostics.md)voor meer informatie. Zie voor een beschrijving van het plannen van een algemene "systeemstatus-model" [Failsafe: richtlijnen voor het robuuste Cloud architecturen](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Noodgevallen simulatie

Testen van de simulatie heeft betrekking op kleine praktijk situaties maken op het werk floor om te zien hoe de teamleden reageren. Simulaties ook worden hoe effectieve de oplossingen in het abonnement herstel worden weergegeven. Simulaties zodanig dat de gemaakte scenario's werkelijke bedrijven tijdens het nog steeds gevoel dat reële situaties niet verstoort verrichten.

Houd rekening met een soort schakelbord"" in de toepassing kunt u problemen met de beschikbaarheid handmatig simuleren uitbreidt. Activeren bijvoorbeeld via een schakeloptie voor vloeiende, database access uitzonderingen voor een ordenen module door waardoor er problemen optreden. Op het niveau van netwerk kunt u vergelijkbare lightweight benaderingen voor andere modules meenemen.

De simulatie worden eventuele problemen die fi waren gemarkeerd. De gesimuleerd scenario's moet volledig beheerbare. Dit betekent dat, zelfs als het abonnement herstel lijkt te zijn opgetreden, u de situatie terug naar de normale herstellen kunt zonder aanzienlijk gevolgen. Het is ook belangrijk de hoogte te stellen op een hoger niveau management over wanneer en hoe de simulatieoefeningen wordt uitgevoerd. Dit abonnement moet informatie bevatten over de tijd of resources die niet-productieve worden kunnen terwijl de simulatie test wordt uitgevoerd. Wanneer u uw gaan aan een toets onderwerpen bent, is het ook belangrijk om te bepalen hoe succes wordt worden gemeten.

Zijn er enkele andere technieken die u gebruiken kunt om te testen noodgevallen herstel abonnementen. Deze meestal zijn echter gewoon gewijzigde versies van deze eenvoudige technieken. De belangrijkste motive achter deze testen wordt berekend hoe haalbaar is en hoe haalbaar is van het abonnement herstel. Herstel richt zich op de details te ontdekken gaten in het abonnement eenvoudige herstel testen.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks artikelen die zijn gericht op [herstel en betere beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Het vorige artikel in deze reeks is [beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](./resiliency-high-availability-azure-applications.md).
