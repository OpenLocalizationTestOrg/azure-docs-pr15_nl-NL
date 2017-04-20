<properties
   pageTitle="Azure SQL Database Azure casestudy - Daxko/CSI | Microsoft Azure"
   description="Meer informatie over hoe Daxko/CSI SQL-Database gebruikt de ontwikkelingscyclus en en de klantenservices en de prestaties verbeteren"
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
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI Azure gebruikt de ontwikkelingscyclus en en de klantenservices en de prestaties verbeteren

![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI Software geconfronteerd een uitdaging: klantendatabase van geschiktheid en opnieuw gemaakt, is groeien snel, met behulp van het succes van de oplossing volledig enterprise-software, maar houden met de behoeften van de IT-infrastructuur voor die groeiende klant basistoewijzing is testen van het bedrijf IT-afdeling. Het bedrijf is steeds beperkt door dalende bewerkingen realiseren, met name voor het beheer van de groeiende databases. Deze bewerkingen realiseren is slechter, knippen in development resources voor nieuwe initiatieven, zoals de nieuwe functies voor mobiliteit voor software van het bedrijf.

Op basis van de David Molina, directeur van productontwikkeling bij Daxko/CSI, aangeboden Azure CSI Software met het platform-als-een-service (PaaS)-model die deze nodig hebt voor het databasebeheer van de te vereenvoudigen, schaalbaarheid vergroten en bronnen kunt richten op software in plaats van ops vrij te maken. "Azure SQL-Database is een handige optie voor ons. Hoeven niet bang te onderhouden van een SQL Server, een failovercluster en alle andere de infrastructuur moet is ideaal voor ons."

Sinds gemigreerd naar Azure, moet CSI Software een beheerders van slechts twee voor het beheren van meer dan 600 klantendatabases. Het bedrijf van elastische toepassingen Azure SQL-Database wordt verplaatst van de klantendatabases op basis van grootte en nodig.

Molina zich blijft voordoen, "onze klanten ervaren de wijziging meteen. Voordat u Elastische van toepassingen hadden af en toe ze-outs en andere problemen tijdens burst perioden. Met Azure elastische toepassingen, kunnen ze burst naar wens en gebruikt u de software zonder problemen."

Naast het verbeteren van de prestaties voor klanten, database Azure elastische vrijgemaakt CSI Software resources kunt richten op het ontwikkelen van nieuwe services en functies, in plaats van omgaan met bewerkingen en beheer van toepassingen. De IT-resources geholpen CSI Software de enterprise-software aanbod, SpectrumNG, om te oefenen sportschool leden, personeel efficiënter, verbeteren en personeel en leden mobiele toegang geven voor interactieve taken en realtime meldingen.

Azure heeft ook heeft geholpen CSI Software versnellen en de ontwikkeling en kwaliteitscontrole (QA) cyclus verbeteren doordat automatisering opties. Met Azure implementatie van het bedrijf, kunnen opbouwen managers pakket onderdelen met een muisklik. Als Molina wordt beschreven, "als onderdeel van het release-cyclus, q & a kan nu implementeren naar een testomgeving in Azure wordt aangegeven, die sterk lijkt op de stapel van onze productie. We kunnen builds onmiddellijk dashboard implementeren naar onze ontwikkelaar-omgeving wilt vet wijzigingen. Dat is een groot win voor ons, omdat er geen overeenkomst voor het testen voordat u die."

## <a name="offloading-to-the-cloud"></a>Offloading naar de cloud

Voordat u verdergaat met de cloud, had CSI Software is opgebouwd eigen multitenant infrastructuur voor een lokale datacenter in Houston. Het bedrijf is uitgevouwen, het toeneemt groeipijn uit kopen, inrichting en onderhouden van alle hardware en software die nodig is voor de ondersteuning van zijn klanten te maken. IT personeel om bewerkingen kwam een andere knelpunt die tot een vertraging van de inrichting van nieuwe resources en implementeren van nieuwe services naar klanten hebben geleid.

CSI Software gezocht in de cloud-opties voor het verwijderen van die realiseren, zodat deze op de code, in plaats van de bewerkingen richten kan. Het bedrijf ontdekt dat veel van de bovenste cloud-providers alleen infrastructuur-als-een-service (IaaS) oplossingen waarvoor nog steeds een grote IT-afdeling voor het beheren van de stapel IaaS bieden. Uiteindelijk bepaald CSI Software dat de oplossing Azure PaaS de meest geschikte optie voor de behoeften is. Molina wordt uitgelegd, "Azure krijgt de hardware en software uit de weg, zodat we kunt richten op onze software is voltooid, terwijl het verkleinen van onze IT-realiseren."

## <a name="making-the-transition-to-azure"></a>Waardoor de overgang naar Azure

Nadat u Azure selecteert voor de oplossing PaaS, CSI Software van start is gegaan de backend-infrastructuur en databases migreren naar de cloud. Voordat u Azure SpectrumNG klanten die nodig zijn voor het installeren van een clienttoepassing die uitgewisseld met een Windows Communication Foundation (WCF)-service op de back-end. Op basis van de Molina, "Hoewel sommige klanten alles in hun eigen datacenters gehost, we ingebouwd uit het product worden multitenant. We gehost alles in een datacenter in Houston, SQL Server als de gegevensopslag gebruiken.

"Ons product ook aanbod opgenomen een lid-omlaag webportal (geschreven in ASP.net) dat is ontworpen om te worden wit gelabelde zodat deze overeenkomt met de website van de klant en een SOAP-API ter ondersteuning van de online pagina's en de integratie van een derde partij."

De migratie naar de cloud hebt niet lang duurt voor de architectuur. Op basis van de Molina, "Het grootste deel van de hoeveelheid afgehandeld aanpassing van de manier dat we config bestandsinformatie, een wijziging gecentraliseerde verbindingsreeks en de verpakking automatiseren uploaden en implementatie van onze releases opgelezen."

CSI softwareontwikkelaars wordt het ontwikkelen van de automatisering opbouwen en Azure PowerShell en REST API's gebruikt voor pakketten maken en te uploaden naar een testomgeving voor release elke 's nachts.
De algehele overgang naar een Azure cloudgebaseerde-implementatie is een snel en soepel voor het CSI Software IT-team. Molina wordt uitgelegd, "In alle, hebben we een bèta-omgeving in de cloud binnen drie tot vier weken te nemen aan het project. Dat is een verrassend win voor ons."

Na het configureren en testen van de omgeving, CSI Software van start is gegaan migreren klanten. Voor klanten die al CSI Software hostingprovider, is de overgang vrijwel naadloze. Voor klanten met migreren vanaf een on-premises implementatie de migratie naar de cloud hebt gemaakt, wat extra tijd, maar is nog steeds hoofdzakelijk pijn-vrij te geven voor klanten en CSI Software.

Voor nieuwe klanten, CSI Software van IT-afdeling het volgende proces voor het ingebouwde ze gebruiken om te Azure:

1.  Azure PowerShell-scripts worden gebruikt om te draaien van een nieuwe database voor de klant. alle klanten beginnen op een laag premium om ervoor te zorgen voldoende aanvankelijke doorvoer voor de overgang.
2.  Indien mogelijk, CSI Software maakt gebruik van de Wizard migreren van Azure SQL bestaande gegevens verplaatsen naar een exemplaar van de Azure SQL-Database.
3.  Tot slot Microsoft SQL Server Integration Services (SSIS) gebruikt om af te stemmen eventuele verschillen in de gegevens of om uit te voeren van alle gegevens opschonen zoals vereist.

Tegenwoordig kunnen worden ongeveer 99% van de Software CSI klanten gehost in Azure wordt aangegeven, over vier regionale datacenters (Noord centraal, Zuid centraal, Oost en West). Latentie is doordat datacenters in geografische regio van elke klant, tot een minimum beperkt.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure elastische database pools vrijgemaakt IT-resources

Verschillende functies van Azure CSI Software hebben geholpen shift worden infrastructuur en bewerkingen die zijn gericht aan de functie en ontwikkeling gericht. Misschien is het grootste voordeel van elastische database pools van.

Over 550 databases biedt CSI Software momenteel voor klanten. Voordat u Elastische van toepassingen is het moeilijk te beheren dat veel databases binnen de structuur van een laag. OPS managers moest prestaties lagen op basis van de behoeften burst van klanten, dat aanzienlijk IT-resource belasting vereist toewijzen. Met elastische database van toepassingen, kunnen projectmanagers tenants een premium of standaard toepassingen, zo nodig, toewijzen en verplaats klanten op basis van grootte en nodig hebt. Klanten ervaren de effecten van de groepen elastische database bijna onmiddellijk; voordat elastische van toepassingen, klanten had-outs en andere problemen tijdens burst-gebruik perioden, maar met elastische van toepassingen, klanten activiteit bursts naar wens kunnen ondervinden en kunnen ze doorgaan met het gebruik van SpectrumNG zonder problemen.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active geografische-replicatie versneld rapportage

Verschillende CSI Software klanten ook profiteert van Azure Active geografische-replicatie. Met actieve geografische-herhaling, kan maximaal vier leesbare secundaire databases worden geconfigureerd in de gebieden dezelfde of een andere datacenter. CSI Software gebruikmaakt van actieve geografische-replicatie op twee manieren: eerst de secundaire databases zijn beschikbaar in het geval van een storing datacenter of problemen met de verbinding maken met de primaire database. en vervolgens de secundaire databases kunnen worden gelezen en laten alleen-lezen-werkbelastingen zoals rapportage taken kunnen worden gebruikt. Sommige klanten CSI Software Gebruik deze korting en rapportage werkstromen versnellen.

## <a name="csi-software-application-logic-and-architecture"></a>CSI Software toepassingslogica en architectuur

SpectrumNG gebruikt web rollen. Omdat de toepassing meerdere tenant is, wordt een WCF-service wordt gebruikt om de eerste verbinding tot stand aanvraag van klanten te verwerken. Zoals Molina aanwezigheidsstatussen, identificatie"het verzoek van elke klant, waarmee vervolgens we een verbindingsreeks af tot hun databases te doen wat we moet doen."

Voor de weblaag van de service maakt CSI Software gebruik van Azure automatisch schalen, op basis van datum en tijd. Beschikbare resources worden automatisch verhoogd zodat hoger gebruik tijdens kantooruren, op basis van de tijdzone van elke regionale datacenter. Resources zijn ook instellen in het weekend, wanneer de behoeften van de klant lagere zijn verkleinen.

     
![Daxko/CSI architectuur](./media/sql-database-implementation-daxko/figure1.png)

Afbeelding 1. Een rol cloud services werknemer Hiermee haalt u gestructureerde gegevens van Azure SQL-Database en gedeeltelijk gestructureerde gegevens uit tabelopslag. SpectrumNG gebruikers werken met die gegevens via een wolk Webrol services.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Met behulp van WebApps en een laag web-abonnement voor mobiele apps

Gebruik van Azure SQL-Database vrijgemaakt resources voor CSI Software voor het inschakelen van nieuwe initiatieven, met inbegrip van een voltooid mobiel platform op basis van een aangepaste API gehost in Azure web-apps. Het platform kunt sportschool leden en personeel gebruik van mobiele apparaten kunt planningen controleren, klassen boeken en ontvangen van berichten.

Het platform service gerichte architectuur (SOA) gebruikt om u te maken van één onderdeel, zoals een POS-systeem (POS) of een verkoopsysteem, verplaatst u deze in de browser naar een ander abonnement van het web, en klik vervolgens kringvelden van een service ter ondersteuning van dat onderdeel, terwijl rest van de oorspronkelijke web-abonnement. Deze mogelijkheid biedt enorme flexibiliteit CSI Software en is het handig kosten ingedrukt houden.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure kunt CSI Software ontwikkelaars zich richten op apps en services

Azure SQL-Database is niet alleen een weblog SpectrumNG klanten, die te rekenen op de snelle, betrouwbare-service, het is ook een groot win voor CSI-Software IT-afdeling en ontwikkelaars. Offloading ops naar Azure in de cloud, CSI Software de belasting voor resources en de infrastructuur van beperkte, enorm versneld de productontwikkeling, en niet meer behoeften aan databases micromanage optimaliseren voor de tenants.

## <a name="more-information"></a>Meer informatie

- Zie voor meer informatie over groepen van Azure elastische database, [elastische database-toepassingen](sql-database-elastic-pool.md).

- Zie voor meer informatie over de hulpmiddelen voor databases en elastische schaal, [elastische Databasehulpmiddelen en elastische schaal](sql-database-elastic-scale-get-started.md).

- Meer informatie over het migreren van een SQL Server-database, raadpleegt u de [Wizard Azure SQL-migratie](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Zie voor meer informatie over actieve geografische-replicatie, [Actieve geografische-replicatie](sql-database-geo-replication-overview.md).

- Zie voor meer informatie over Web rollen en werknemer rollen, [werknemer rollen](../fundamentals-introduction-to-azure.md#compute). 

- Zie meer informatie over Azure Service Bus [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Zie meer informatie over automatisch schalen, [schaalbaarheid van cloudservices](../cloud-services/cloud-services-how-to-scale.md).
