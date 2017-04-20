<properties
   pageTitle="Azure SQL Database Azure casestudy - Umbraco | Microsoft Azure"
   description="Meer informatie over het gebruik van SQL-Database voor het inrichten van snel in Umbraco en schaal services voor duizenden tenants in de cloud"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco gebruikmaakt van Azure SQL-Database naar snel inrichten en schaal van services voor duizenden tenants in de cloud

![Umbraco-Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco is een populaire open source inhoudsbeheer systeem (CMS) die kan worden uitgevoerd kleine campagne of brochure sites naar complexe toepassingen voor Fortune 500 bedrijven en globale media-websites. 

> "We hebben helemaal een grote community van ontwikkelaars die gebruikmaken van het systeem, wordt met meer dan 100.000 ontwikkelaars op onze forums en meer dan 350.000 sites die zijn live, uitgevoerd Umbraco."

> — Morten Christensen, technische potentiële klant, Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Om te vereenvoudigen klant-implementaties, Umbraco Umbraco-als-een-Service (UaaS) toegevoegd: een aanbod voor software-als-een-service (SaaS) zodat het niet nodig voor on-premises implementaties, biedt ingebouwde schaalbaarheid en verwijdert u de management belasting doordat ontwikkelaars kunt richten op product vernieuwing in plaats van oplossing management. Umbraco is, kunnen alle voordelen van deze door te vertrouwen op het flexibele platform-als-een-service (PaaS) model door Microsoft Azure worden aangeboden.

UaaS kunnen SaaS klanten Umbraco CMS mogelijkheden die eerder afmelden bij hun bereik waren gebruiken. Deze klanten is met een CMS-omgeving voor het werken met een productiedatabase ingericht. Klanten kunnen maximaal twee extra databases voor ontwikkeling en tijdelijk opslaan omgevingen, afhankelijk van hun vereisten toevoegen. Als u een nieuwe omgeving moet worden geretourneerd, een geautomatiseerde proces wordt toegewezen die klant een database automatisch. De nieuwe database is klaar in seconden, omdat de database al vooraf door Umbraco uit een Azure elastische groep beschikbare databases ingericht is (Zie afbeelding 1).


![Umbraco inrichten lifecycle](./media/sql-database-implementation-umbraco/figure1.png)

Afbeelding 1. Levenscyclus voor Umbraco als een Service (UaaS) is geïnstalleerd
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastische pools en automatisering vereenvoudigen implementaties

Met Azure SQL-Database en andere Azure services, Umbraco klanten kunnen zelf inrichten van hun omgevingen en Umbraco eenvoudig kunt controleren en databases beheren als onderdeel van een werkstroom voor het intuïtieve:

1.  Inrichten

    Umbraco onderhoudt een capaciteit van 200 beschikbare vooraf ingerichte databases van elastische pools van. Wanneer een nieuwe klant bent zich registreert voor UaaS, biedt Umbraco de klant met een nieuwe CMS-omgeving nabije realtime door toe te wijzen een database uit de beschikbaarheid van de groep.

    Wanneer een beschikbaarheid van toepassingen de drempel bereikt, een nieuwe elastische toepassingen wordt gemaakt en nieuwe databases vooraf ingerichte moet worden toegewezen aan klanten naar wens zijn.

    Implementatie is volledig geautomatiseerd met C# management bibliotheken en Azure Service Bus wachtrijen.

2.  Gebruiken

    Klanten gebruiken één tot drie omgevingen (productie, tijdelijke en/of ontwikkeling), elk met een eigen database. Klantendatabases zijn in elastische database-toepassingen, waarmee Umbraco te leveren efficiënt schaalbaarheid zonder te veel inrichten.

    ![Umbraco projectoverzicht](./media/sql-database-implementation-umbraco/figure2.png)

    ![Details van Umbraco projecten](./media/sql-database-implementation-umbraco/figure3.png)

    Afbeelding 2. Umbraco-als-een-Service (UaaS) klant website, met de projectoverzicht en details

    Azure SQL-Database wordt Database transactie-eenheden (DTUs) gebruikt om aan te geven van de relatieve kracht vereist voor echte databasetransacties. Voor klanten met UaaS databases meestal op bijna 10 DTUs werken, maar elke de elasticiteit aan de nieuwe schaal op aanvraag heeft. Dat betekent dat er dat uaas kunt zorgen dat klanten altijd nodig resources, zelfs op tijdstippen piek. Bijvoorbeeld tijdens een recente zondag 's nachts sporten gebeurtenis wordt één UaaS ervaren klantendatabase waar de pieken maximaal 100 DTUs voor de duur van het spel. Azure elastische pools gesteld Umbraco ter ondersteuning van die hoge vraag zonder vertragingen.

3.  Monitor

    Umbraco monitoren database activiteit dashboards binnen de Azure-portal, samen met aangepaste e-mailwaarschuwingen gebruiken.

4.  Problemen oplossen

    Azure biedt twee opties voor herstel (DR): actieve geografische-replicatie en geografische herstellen. De optie DR die een bedrijf moet selecteert, is afhankelijk van de [bedrijfscontinuïteit doelstellingen](sql-database-business-continuity.md).

    Actieve geografische-herhaling biedt het snelste niveau antwoord in het geval van downtime. Actieve geografische-replicatie gebruikt, u kunt maximaal vier leesbare ontvangen op servers in verschillende regio's maken en u kunt vervolgens failover naar een van de secundaire servers starten bij een storing.

    Umbraco geografische-replicatie wordt niet vereist, maar deze profiteren van Azure geografische-herstellen om ervoor te zorgen minimale uitvaltijd in het geval van een storing. Geografische herstellen, is afhankelijk van back-ups in geografische-redundante Azure opslag. Waarmee gebruikers herstellen uit een back-up wanneer er sprake is van een storing in de primaire regio.

5.  Intrekken

    Wanneer een project-omgeving wordt verwijderd, worden alle gekoppelde databases (ontwikkeling, tijdelijk opslaan of live) tijdens Azure Service Bus wachtrij opschonen verwijderd. Dit geautomatiseerde proces herstelt u de niet-gebruikte databases met Umbraco van elastische database-beschikbaarheid van de groep, zodat ze beschikbaar zijn voor het inrichten van toekomstige behoud optimaal gebruik.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastische groepen kunt u UaaS aan de nieuwe schaal met gemak

Door te profiteren van Azure elastische database-toepassingen, kunt Umbraco prestaties optimaliseren voor klanten zonder dat u moet over- of onder inrichten. Umbraco heeft bijna 3000 databases momenteel over 19 elastische database-toepassingen, met de mogelijkheid om te eenvoudig schaal zo nodig zijn voor een van de bestaande klanten van 325,000 of nieuwe klanten die klaar zijn voor het implementeren van een CMS in de cloud.

Ja, op basis van de Morten Christensen, technische leiden bij Umbraco, "UaaS is nu met de groei van ongeveer 30 nieuwe klanten per dag. Onze klanten zijn zeer tevreden over het gemak van wordt kunt inrichten van nieuwe projecten in seconden, updates direct publiceren aan hun persoonlijke site van een ontwikkelomgeving met één klik-implementatie en breng wijzigingen net zo snel als ze fouten zoeken."

Als een klant een tweede en/of derde-omgeving niet meer nodig, deze hoeft te verwijderen die omgevingen. Die resources die kunnen worden gebruikt voor andere klanten als onderdeel van de Umbraco elastische database beschikbaarheid van toepassingen vrijgemaakt.

![Umbraco implementatiearchitectuur](./media/sql-database-implementation-umbraco/figure4.png)

Afbeelding 3. UaaS implementatiearchitectuur op Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Het pad van het datacenter naar cloud

Wanneer de ontwikkelaars Umbraco in eerste instantie besloten verplaatsen naar een model SaaS, wist ze dat ze een efficiënt en scalable manier om het bouwen van de service zou hebben.

> "Elastische database-toepassingen zijn past perfect onze SaaS aanbod omdat we capaciteit omhoog en omlaag naar wens kunt bellen. Inrichting gaat heel eenvoudig en met onze-instelling kan we gebruik houden met een maximum."

> — Morten Christensen, technische potentiële klant, Umbraco

'We wilde onze tijd besteden aan het oplossen van problemen van onze klanten, infrastructuur niet beheren. We wilt ervoor zorgen dat eenvoudig onze klanten Haal het meeste,"zegt Niels Hartvig, oprichter van Umbraco. "We in eerste instantie beschouwd als de servers onszelf hostingprovider, maar het plannen van capaciteit had een nachtmerrie." Coincidentally, wordt Umbraco niet gebruikmaken van een databasebeheerders, waarin een toegevoegde sleutelwaarde voor het gebruik van UaaS onderstrepingstekens.

Een belangrijke doel voor de ontwikkelaars Umbraco was zelf een formule voor UaaS klanten voor het inrichten van omgevingen snel en zonder beperkingen van de capaciteit. Maar biedt een speciale gehoste-service in Umbraco datacenters zou hebben nodig een groot aantal overtollige capaciteit worden afgehandeld bursts in verwerking. Die zou hebben bedoeld aanzienlijke berekeningscluster-infrastructuur die zou hebben is regelmatig volledig toevoegen.

Het ontwikkelteam Umbraco wilde daarnaast een oplossing waarmee ze zo veel mogelijk van hun bestaande code mogelijk opnieuw te gebruiken. Als de ontwikkelaar Umbraco aanwezigheidsstatussen Mikkel Madsen, "We zijn tevreden zijn met de hulpmiddelen voor het ontwikkelen van Microsoft die we al kent, zoals Microsoft SQL Server, Microsoft Azure SQL-Database, ASP.net en Internet informatieservices (IIS zijn). Cloud voordat te investeren in een IaaS of een PaaS oplossing, we het gewenste type om ervoor te zorgen dat zou ondersteund onze Microsoft hulpmiddelen en platforms, zodat we hebt gewerkt enorme wijzigingen aanbrengen in onze code basistoewijzing. "

Umbraco gezocht om te voldoen aan de criteria, een cloud-partner met de volgende situaties:

-   Voldoende capaciteit en betrouwbaarheid
-   Ondersteuning voor hulpmiddelen voor het ontwikkelen van Microsoft, zodat Umbraco engineers zou wordt gestart niet volledig doelen hun ontwikkelomgeving
-   Aanwezigheid in alle van de geografische markten waarin UaaS (bedrijven nodig hebben om ervoor te zorgen concurreert dat ze toegang hebben tot hun gegevens snel en dat hun gegevens zijn opgeslagen op een locatie die voldoet aan hun regionale wettelijke vereisten)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Waarom Umbraco voor hebt gekozen Azure UaaS

Volgens Morten Christensen "na bestudering van alle onze opties, we geselecteerd Azure omdat deze alle onze criteria, uit beheerbaarheid en schaalbaarheid bekend zijn en rentabiliteit voldaan. We de omgevingen op Azure VMs instellen en elke omgeving heeft een eigen exemplaar Azure SQL-Database met de overal in elastische database-toepassingen. Door te scheiden databases tussen ontwikkeling, tijdelijke en live-omgevingen, bieden we onze klanten robuuste prestaties moeten worden geïsoleerd afgestemd op schaal, een enorme win. "

Morten blijft, "voordat, hebben we voor het inrichten van servers voor Webdatabases handmatig. We hebben nu niet denken. Alles is geautomatiseerd, uit de inrichting van worden opgeschoond. "

Morten is ook tevreden met de schaal mogelijkheden van Azure. "Elastische database-toepassingen zijn past perfect onze SaaS aanbod omdat we capaciteit omhoog en omlaag naar wens kunt bellen. Inrichting gaat heel eenvoudig en met onze-instelling kan we gebruik houden met een maximum." Morten Staten, "de eenvoudig te elastische van toepassingen, samen met de assurance van service-laag gebaseerde DTUs, ontstaat de kracht en het nieuwe resourcegroepen op aanvraag inrichten. Onlangs, een van onze grotere klanten een piek naar 100 DTUs in de livebesturingsomgeving. Azure gebruikt, onze elastische toepassingen van de klant-databases met de resources die ze nodig in realtime zonder te hoeven DTU vereisten voorspellen aangeboden. Kortom, onze klanten krijgen de tijd inschakelen rond dat ze verwacht en we onze serviceovereenkomsten prestaties moeten voldoen."

Mikkel Madsen dit opgeteld: "We helemaal bent gewonnen voor de krachtige Azure algoritme dat verbinding maakt met een gebruikelijk van SaaS (onboarding nieuwe klanten in realtime bij het op schaal) naar het patroon van onze toepassing (vooraf inrichting-databases, beide ontwikkelen en live) boven aan de onderliggende technologie (via Azure Service Bus wachtrijen in combinatie met Azure SQL-Database)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Met Azure UaaS meer dan verwachtingen van klanten

Sinds het kiezen van Azure de cloud-partner, is Umbraco mogen UaaS klanten geoptimaliseerde inhoud personeel prestaties, bieden zonder de IT-resource investering vereist uit een selfservice gehoste oplossing. Zoals Morten zegt: "We al kent de ontwikkelaar gemak en schaalbaarheid die Azure ontstaat en onze klanten geweldig de functies en betrouwbaarheid zijn. Algemene, het is een uitstekende win voor ons!"
 
## <a name="more-information"></a>Meer informatie

- Zie voor meer informatie over groepen van Azure elastische database, [elastische database-toepassingen](sql-database-elastic-pool.md).

- Zie meer informatie over Azure Service Bus [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Zie voor meer informatie over Web rollen en werknemer rollen, [werknemer rollen](../fundamentals-introduction-to-azure.md#compute). 

- Zie voor meer informatie over virtuele netwerken, [virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Zie voor meer informatie over het back-up en herstellen, [bedrijfscontinuïteit](sql-database-business-continuity.md).  

- Meer informatie over het controleren van ppols Zie [controleren van toepassingen](sql-database-elastic-pool-manage-portal.md). 

- Meer informatie over Umbraco als een Service, raadpleegt u [Umbraco](https://umbraco.com/cloud).

