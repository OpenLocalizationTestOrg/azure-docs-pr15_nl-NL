<properties
   pageTitle="Service stof architectuur | Microsoft Azure"
   description="Service stof is een gedistribueerde systemen-platform voor het samenstellen van scalable, betrouwbare en eenvoudig worden beheerd toepassingen voor de cloud. In dit artikel leest de architectuur van Service stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Service stof architectuur

Service stof is gebouwd met gelaagde subsystemen. Deze subsystemen kunnen u voor het schrijven toepassingen die zijn:

* Maximaal beschikbaar
* Scalable
* Beheerbare
* Testable

In het volgende diagram ziet u de belangrijkste subsystemen van Service stof.

![Diagram van Service stof architectuur](media/service-fabric-architecture/service-fabric-architecture.png)

In een gedistribueerd systeem is het veilig communiceren tussen knooppunten in een cluster essentieel. Is het product transport, die beveiligde communicatie tussen knooppunten biedt bij het grondtal voor de stapel. Boven het transport subsysteem wordt veroorzaakt door het product Federatie, die de verschillende knooppunten in een enkele entiteit (naam clusters) clusters zodat Service stof kunt fouten opsporen, opvulteken verkiezingsuitslagen uitvoeren en consistente routeren. Het subsysteem betrouwbaarheid, gelaagde boven aan het subsysteem Federatie is verantwoordelijk voor de betrouwbaarheid van Service configuratieservices tot en met regelingen zoals herhaling, resourcebeheer en failover. Het subsysteem Federatie aan ook ten grondslag ligt het hosten en activering subsysteem waarop de levensduur van een toepassing op één knooppunt beheert. Het subsysteem management beheert de levensduur van toepassingen en -services. Het subsysteem testbaarheid helpt softwareontwikkelaars testen van hun diensten met gesimuleerd fouten voor en na het implementeren van toepassingen en services naar productieomgevingen. Service stof biedt de mogelijkheid om op te lossen servicelocaties tot en met de subsysteem communicatie. De application programming modellen blootgesteld aan ontwikkelaars liggen in lagen op deze subsystemen samen met het toepassingsmodel tooling inschakelen.

## <a name="transport-subsystem"></a>Transport subsysteem
Het subsysteem transport implementeert een point-to-point communicatie datagramkanaal. Dit kanaal wordt gebruikt voor communicatie binnen service stof clusters en communicatie tussen de service stof cluster en clients. Wordt ondersteund in één richting en verzoek beantwoorden communicatiepatronen, die de basis voor de uitvoering van broadcast en multicast in de laag Federatie. Het subsysteem transport beveiligt communicatie met behulp van X509 certificaten of Windows-beveiligingsgroepen. Dit subsysteem intern wordt gebruikt door de Service stof en is niet rechtstreeks toegankelijk zijn voor ontwikkelaars toepassing hoeft te programmeren.

## <a name="federation-subsystem"></a>Federatie subsysteem
Om te kunnen reden over een aantal knooppunten in een gedistribueerd systeem, moet u beschikken over een consistente weergave van het systeem. Het subsysteem Federatie beschikt over de communicatie primitieven verstrekt door het subsysteem transport en dan de verschillende knooppunten in een geïntegreerd cluster dat deze over kunt reden aan elkaar gehecht. De gedistribueerde systemen primitieven nodig zijn voor de andere subsystemen - detectie is mislukt, opvulteken verkiezingsuitslagen en consistente routering krijgen. Het subsysteem Federatie is gebouwd met verdeelde hashtabellen met een 128-bit token spatie. Het subsysteem Hiermee maakt u een topologie bellen via de knooppunten, met elk knooppunt in de bellen een subset van de token ruimte wordt toegewezen aan uw eigendom. Voor de detectie is mislukt, wordt de laag gebruikt een lease om op basis van hart kloppen in de race en arbitrage. Het subsysteem Federatie garandeert ook via complexe join en vertrek protocollen die slechts één eigenaar van een token op elk gewenst moment bestaat. Dit opvulteken verkiezingsuitslagen en consistente routeren garanties bieden.

## <a name="reliability-subsystem"></a>Betrouwbaarheid subsysteem
De subsysteem betrouwbaarheid biedt de om de status van een service-Service stof ten zeerste als beschikbaar wilt maken door middel van de _distributeur_, _Failover Management_en _De verdeling van de Resource_.

* Replicatie zorgt ervoor dat de staat wijzigingen in de primaire service replica automatisch moeten worden gerepliceerd naar secundaire replica's consistentie tussen de primaire en secundaire replica's in een replicaset service van. Replicatie is verantwoordelijk voor het beheer van quorum tussen de replica's in de replicaset. Deze samenwerkt met de eenheid failover om de lijst met bewerkingen repliceren en de nieuwe configuratie-agent wordt dit aangeboden met de configuratie voor de database. Deze configuratie wordt aangegeven welke replica de bewerkingen moeten worden gerepliceerd. Service stof biedt een standaard-distributeur stof distributeur, die door de programming model-API kunnen worden gebruikt om de servicestatus ten zeerste beschikbaar en betrouwbare genoemd.
* De Manager Failover zorgt ervoor dat wanneer knooppunten worden toegevoegd aan of verwijderd uit het cluster, het selectievakje laden is automatisch opnieuw verdeeld over de beschikbare knooppunten. Als een knooppunt in het cluster mislukt, wordt het cluster automatisch de service replica's voor het behoud van beschikbaarheid configureren.
* De resourcemanager service replica's aantal_tekens hebt opgegeven in domein is mislukt in het cluster en zorgt ervoor dat alle failover eenheden operationele zijn. De resourcemanager hoeveelheid ook serviceresources over de onderliggende knooppunten om te bereiken optimale uniform werkbelasting verdeling van gedeelde toepassingen.

## <a name="management-subsystem"></a>Management subsysteem
Het subsysteem management biedt end-to-end-service en Toepassingsbeheer levenscyclus. PowerShell-cmdlets en administratieve API's kunt u inrichten, implementeren patch, upgraden en maak het inrichten van toepassingen zonder detailverlies van de beschikbaarheid van. Het subsysteem management kunt dit tot en met de volgende services uitvoeren.

* **Cluster Manager**: dit is de primaire-service waarin interactie met de Failover Manager van betrouwbaarheid als u wilt de toepassingen op de knooppunten op basis van de beperkingen van de plaatsing service plaatsen. De resourcemanager in failover subsysteem zorgt ervoor dat de beperkingen nooit verbroken worden. De manager cluster beheert de levenscyclus van de toepassingen van verstrekken voor het inrichten van opgeheven. Dit werkt naadloos samen met de systeemstatus manager om ervoor te zorgen dat de beschikbaarheid van de toepassing niet verloren semantic systeemstatus vanuit het perspectief van bij upgrades.
* **Servicestatus Manager**: met deze service kan statuscontrole van toepassingen, services en cluster entiteiten. Cluster entiteiten (zoals knooppunten, service partities en replica's) kunnen systeemstatus informatie, die vervolgens wordt samengevoegd in het archief gecentraliseerde systeemstatus melden. Deze status informatie vindt u een momentopname van de algemene point-in-time status van de services en knooppunten verdeeld over meerdere knooppunten in het cluster, zodat u alle benodigde corrigerende acties. Systeemstatus query dat API 's kunnen u de status gebeurtenissen query die de systeemstatus subsysteem gerapporteerd. De systeemstatus query API's retourneren de onbewerkte systeemstatus-gegevens die zijn opgeslagen in de systeemstatus store of de samengevoegde beschouwd gegevens van de servicestatus voor een specifieke cluster entiteit.
* **Afbeelding van de Store**: deze service biedt opslag- en distributiegroepen van de toepassing binaire bestanden. Deze service biedt een eenvoudige verdeelde bestandslocatie waar de toepassingen zijn geüpload naar en gedownload van.


## <a name="hosting-subsystem"></a>Hostingprovider subsysteem
De manager cluster wordt geïnformeerd het hostingprovider subsysteem (uitgevoerd op elk knooppunt) welke services deze nodig voor een bepaald knooppunt te beheren. Het hostingprovider subsysteem beheert vervolgens de levenscyclus van de toepassing op dat knooppunt. Deze samenwerkt met de betrouwbaarheid en systeemstatus onderdelen om ervoor te zorgen dat de replica's correct worden geplaatst en in orde zijn.

## <a name="communication-subsystem"></a>Communicatie subsysteem
Dit subsysteem biedt betrouwbare messaging binnen de cluster en service discovery via de Naming service. De Naming service servicenamen worden omgezet in een locatie in het cluster en waarmee gebruikers kunnen servicenamen en eigenschappen beheren. Gebruik van de Naming service communiceren clients veilig met een van de knooppunten in het cluster voor het oplossen van een servicenaam en service metagegevens ophalen. Met een eenvoudige Naming-client API, kunnen gebruikers van Service stof ontwikkelen services en clients met het oplossen van de huidige netwerklocatie ondanks knooppunt dynamiek of het wijzigen van het formaat van het cluster.

## <a name="testability-subsystem"></a>Testbaarheid subsysteem
Testbaarheid is een verzameling hulpprogramma's die speciaal zijn ontworpen voor het testen van services is gebaseerd op de Service stof. De hulpmiddelen voor laat een ontwikkelaar eenvoudig duidelijke fouten veroorzaken en scenario's testen om te oefenen en valideren van de vele Staten en overgangen die een service overal in de levensduur, alle beheerde en veilige wijze ervaart uitvoeren. Testbaarheid bevat ook een om om uit te voeren langer tests die u kunnen doorgaan met het ontwikkelen tot en met verschillende mogelijke fouten zonder beschikbaarheid kwijt te raken. Dit biedt u een test productieomgeving.
