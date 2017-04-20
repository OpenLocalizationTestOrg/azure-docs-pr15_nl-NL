<properties
   pageTitle="Overzicht van de Service stof | Microsoft Azure"
   description="Een overzicht van de Service stof, waar toepassingen bestaan uit veel microservices schaal en flexibiliteit op te geven. Service stof is een platform gedistribueerde systemen voor het samenstellen van scalable, betrouwbare en eenvoudig beheerde toepassingen voor de cloud"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Overzicht van de Service stof
Service stof is een gedistribueerde systemen-platform die kunt u heel gemakkelijk inpakken, implementeert en scalable en betrouwbare microservices beheren. Service stof adressen ook de grote uitdagingen in ontwikkelen en cloud-toepassingen beheren. Ontwikkelaars en beheerders kunnen geen oplossen van problemen met complexe infrastructuur en de focus in plaats daarvan over het implementeren van essentiële, veeleisende werkbelasting weten dat het scalable, betrouwbare en beheerbare. Service stof staat voor het allernieuwste middleware-platform voor het maken en beheren van deze ondernemingen, laag-1-cloud-toepassingen.

In deze [korte video](https://aka.ms/servicefabricvideo) biedt een introductie over het Service stof en microservices.


## <a name="applications-composed-of-microservices"></a>Toepassingen op basis van microservices
Service stof kunt u maken en beheren van scalable en betrouwbare toepassingen op basis van microservices met zeer hoog dichtheid uitgevoerd op een gedeelde groep van machines (genoemd een cluster). Een geavanceerde runtime krijgen voor het samenstellen van gedistribueerde, scalable stateless en statuscontrole microservices. Het biedt uitgebreide toepassing beheermogelijkheden ook voor inrichting, implementeert, controle, een upgrade/herstellen en verwijderen van toepassingen geïmplementeerd.

Waarom is een aanpak microservices belangrijk? De twee belangrijkste redenen zijn:

1. Hiermee kunt u de schaal van verschillende gedeelten van uw toepassing afhankelijk van de behoeften aanpassen.

2. Ontwikkelteams bij mogen meer flexibel in het implementeren van wijzigingen zijn en kan dus functies bieden naar uw klanten sneller en vaker.

Service stof bevoegdheden veel Microsoft services vandaag, inclusief Azure SQL-Database, Azure DocumentDB, Cortana Power BI, Microsoft Intune, Azure gebeurtenis Hubs, Azure IoT, Skype voor bedrijven en veel core Azure services.

Service stof is toegespitst voor het maken van cloud systeemeigen services die u kunnen beginnen klein, indien nodig en uit te breiden naar enorme schalen met honderden of duizenden machines.

Vandaag Internet-schaal services worden gemaakt van microservices. Voorbeelden van microservices zijn protocol gateways, gebruikersprofielen, winkelen winkelwagentjes, voorraad processing, wachtrijen, en in de cache opgeslagen. Service stof is een microservices-platform die een unieke naam die stateless of statuscontrole worden kan van elke microservice oplevert.

Service stof biedt uitgebreide runtime en levenscyclus beheermogelijkheden naar toepassingen op basis van deze microservices. Deze host microservices binnen containers geïmplementeerd en geactiveerd over de Service stof cluster. Overstappen van VMs naar containers wordt mogelijk een grotere orde-van-grootte in dichtheid. Daarnaast een andere volgorde van grootte in dichtheid mogelijkheden komt dan beschikbaar door het verplaatsen van containers naar microservices. Een enkel Azure SQL Database-cluster omvat bijvoorbeeld honderden computers met tientallen duizenden containers hostingprovider een totaal van honderden duizenden databases. Elke database is een Service stof statuscontrole microservice. De geldt ook voor de andere services die eerder is vermeld, daarom de term "' hyperscale '" wordt gebruikt om te beschrijven Service stof mogelijkheden is. Als het containers hoge dichtheid, geef microservices u ' hyperscale '.

Lees voor meer informatie over de methode microservices [Waarom een aanpak microservices tot het bouwen van toepassingen?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Container implementatie- en integratie
Service stof is een [orchestrator](service-fabric-cluster-resource-manager-introduction.md) van microservices over een cluster van machines. Microservices kunnen op tal van manieren van het gebruik van de [Service stof programming modellen](service-fabric-choose-framework.md) op de implementatie van [Gast uitvoerbare bestanden](service-fabric-deploy-existing-app.md)worden ontwikkeld. Service stof kunt distribueren services in de container afbeeldingen en is van belang dat u zowel services in processen en services in containers samen in dezelfde toepassing kunt combineren. Als u alleen [gebruiken en beheren van de container afbeeldingen](service-fabric-containers-overview.md) in een cluster van machines wilt, is Service stof een perfecte keuze voor dit.


## <a name="create-service-fabric-clusters-anywhere"></a>Een willekeurige plaats Service stof clusters maken
U kunt Service stof clusters maken in veel omgevingen, met inbegrip van Azure of on-premises, Windows Server of Linux. Bovendien is de ontwikkelomgeving in de SDK identiek aan de productieomgeving met geen emulators die nodig zijn. Met andere woorden, als deze wordt uitgevoerd op uw lokale ontwikkeling cluster implementeren deze in hetzelfde cluster in andere omgevingen.

Voor meer informatie over het maken van clusters on-premises, [een cluster op Windows Server- of Linux](service-fabric-deploy-anywhere.md) maken of voor Azure maken van een cluster [via de portal van Azure](service-fabric-cluster-creation-via-portal.md).

![Service stof platform][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Stateless en statuscontrole Service stof microservices

Service stof kunt u toepassingen die bestaat uit microservices maken. Een veranderlijke staat buiten een bepaalde aanvraag en de reactie van de service wordt niet meer in stateless microservices (protocol gateways, web proxy's, enzovoort). Azure Cloudservices werknemer rollen zijn een voorbeeld van een stateless service. Statuscontrole microservices (gebruikersaccounts, databases, apparaten, winkelen on, wachtrijen, enzovoort) voor het behoud van een veranderlijke, gezaghebbende staat naast de aanvraag en de reactie. Vandaag, Internet-toepassingen bestaan uit een combinatie van stateless en statuscontrole microservices.

Waarom geen statuscontrole microservices samen met stateless bestanden? De twee belangrijkste redenen zijn:

1. De mogelijkheid om het maken van hoge gegevensdoorvoer, lage latentie, mislukt fouttolerantie online verwerking van transacties (OLTP) services doordat code en gegevens sluiten op dezelfde computer. Enkele voorbeelden zijn interactief winkelobjecten, zoeken, Internet van dingen (IoT)-systemen, werkbladen systemen, creditcard verwerking en fraude detectie systemen en persoonlijke record management.

2. Toepassing ontwerp vereenvoudigen. Statuscontrole microservices verwijderen dat u extra wachtrijen en in de cache opgeslagen, traditioneel nodig om de beschikbaarheid van de en latentie vereisten van een zuiver stateless toepassing. Statuscontrole services zijn natuurlijk hoge beschikbaarheid en lage latentie, het aantal bewegende onderdelen om te beheren in uw toepassing als geheel te verminderen.

Voor meer informatie over de toepassing patronen met Service stof, [toepassing scenario's](service-fabric-application-scenarios.md) en [een programming model framework kiezen](service-fabric-choose-framework.md) voor uw service begrijpen

## <a name="application-lifecycle-management"></a>Levenscyclus Toepassingsbeheer
Service stof biedt eersteklas ondersteuning voor de volledige toepassing levenscyclus passivabeheer van cloud-toepassingen--van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud eventuele buiten bedrijf stellen.

De mogelijkheden van de Service stof ALM kunnen toepassingsbeheerders / IT-operators voor het gebruik van eenvoudige, laag-touch werkstromen inrichten, implementeert, patch, en toepassingen controleren. Deze ingebouwde werkstromen is enorm verkleinen van de belasting voor IT-operators dat toepassingen continu beschikbaar.

De meeste toepassingen bestaan uit een combinatie van stateless en statuscontrole microservices en andere uitvoerbare bestanden/runtimes tegelijk worden geïmplementeerd. Service stof kunt doordat sterke typen van de toepassingen en verpakt microservices, de implementatie van meerdere toepassingsexemplaren. Elk exemplaar wordt beheerd en onafhankelijk bijgewerkt. Service stof is echter kunnen *alle* benodigde uitvoerbare bestanden of runtime implementeren en zodat ze betrouwbare. Bijvoorbeeld implementeren Service stof ASP.NET Core 1, Node.js, Java VMs, scripts of iets anders dat uw toepassing.

Voor meer informatie over de levenscyclus van Toepassingsbeheer, [levenscyclus van de toepassing](service-fabric-application-lifecycle.md) lezen en over het implementeren van een code, Zie [Deploy uitvoerbare Gast](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden
Met behulp van Service stof, kunt u het volgende doen:

- Sterk schaalbare toepassingen die zelfreparerende zijn ontwikkelen.

- Ontwikkel toepassingen op basis van microservices met de Service stof programming model. Of gewoon host Gast uitvoerbare bestanden en andere toepassingskaders van uw keuze, zoals ASP.NET Core 1 of Node.js.

- Ontwikkel uiterst betrouwbaar stateless en statuscontrole microservices.

- Implementeren en goedkeuringen containers Windows containers en Docker containers opnemen in een cluster. Deze containers kunnen container Gast uitvoerbare bestanden of betrouwbare stateless en statuscontrole microservices.  In beide gevallen krijgt u container poort host poorttoewijzing, zichtbaarheid van de container en geautomatiseerde failover.

- Het ontwerp van uw toepassing vereenvoudigen met behulp van statuscontrole microservices in plaats van de cache en wachtrijen.

- Implementeren naar Azure of naar de on-premises implementatie wolken waarop Windows Server- of Linux met nul codewijzigingen. Eenmaal schrijven en vervolgens ergens op een Service stof cluster toepassen.

- Ontwikkel met een 'datacenter op uw computer'-methode. De lokale ontwikkelomgeving is dezelfde code die wordt uitgevoerd in de Azure datacenters.

- Toepassingen implementeren in seconden.

- Toepassingen op hogere dichtheid dan virtuele machines, implementeert honderden of duizenden toepassingen per machine implementeren.

- Verschillende versies van dezelfde toepassing naast elkaar worden geïnstalleerd, elk afzonderlijk upgradebaar implementeren.

- De levenscyclus van uw statuscontrole toepassingen zonder eventuele uitvaltijd, inclusief breken en vaste upgrades beheren.

- Met .NET-API's, Java (Linux), PowerShell, Azure CLI (Linux) of via interfaces zoals REST-toepassingen beheren.

- Upgraden en microservices binnen toepassingen onafhankelijk patch.

- Controleren en een diagnose stellen bij de status van uw toepassingen en beleid voor de uitvoering van automatische reparaties instellen.

- Mogelijkheid om te schalen of schaal in het aantal knooppunten in een cluster, evenals de schaal-omhoog of de grootte van elk knooppunt, weet dat uw toepassingen automatisch schaal en worden gedistribueerd volgens de beschikbare financiële middelen schaal omlaag.

- Bekijk de verdeling van de zelfreparerende resource goedkeuringen door de distributie van toepassingen gehele cluster. Service stof herstelt van fouten en optimaliseert van de verdeling van de belasting op basis van beschikbare resources.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie:
    * [Waarom wordt een microservices benaderen tot het bouwen van toepassingen?](service-fabric-overview-microservices.md)
    * [Overzicht van de terminologie](service-fabric-technical-overview.md)
* Bij het instellen van uw Service stof [ontwikkelomgeving](service-fabric-get-started.md)  
* [Een programming model framework kiezen](service-fabric-choose-framework.md) voor uw service

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
