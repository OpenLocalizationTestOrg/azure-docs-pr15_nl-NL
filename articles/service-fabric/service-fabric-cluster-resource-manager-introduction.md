<properties
   pageTitle="Inleiding tot de Service stof Cluster resourcemanager | Microsoft Azure"
   description="Een inleiding tot de Service Cluster Resource Manager stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Inleiding tot de Service stof cluster resourcemanager
Traditioneel beheer van IT-systemen of een reeks services bedoeld aan een paar fysieke of virtuele machines die zijn toegewezen aan deze specifieke services of de systemen. Veel belangrijke services zijn onderverdeeld in een weblaag '' en een laag "gegevens" of "opslag" wellicht met een paar andere gespecialiseerde onderdelen zoals een cache. Andere typen toepassingen moet een SMS-berichten laag aanvragen gestroomd waar in-en uitfaden, verbonden met een laag werk voor alle analyse en nodig als onderdeel van de messaging-transformatie. Elk type werkbelasting hebt u een specifieke machines specifiek voor deze: de database hebt u enkele machines die zijn toegewezen, de endwebservers per weinig. Als een bepaald type werkbelasting de machines veroorzaakt deze was op om uit te voeren te warm en klikt u meer machines toegevoegd met dit type werkbelasting geconfigureerd om uit te voeren op is geïnstalleerd, of een paar van de machines vervangen door grotere machines. Eenvoudig. Als een machine is mislukt, is dit gedeelte van de algehele toepassing uitgevoerd om lagere capaciteit totdat de computer kan worden hersteld. Nog steeds vrij eenvoudig (als deze niet per se leuk).

Nu echter we Stel dat u hebt gevonden wilt schalen dat anderen en zijn geworden de containers en/of microservice zich aanmeldt. Ineens merkt u dat u met tientallen, honderden of zelfs duizenden machines, tientallen van verschillende soorten services, misschien honderden verschillende exemplaren van deze services, elk voorzien van een of meer exemplaren of replica's voor hoge beschikbaarheid (HA).

Ineens beheer van uw omgeving is niet zo eenvoudig als het een paar computers specifiek voor één soorten werkbelasting moet beheren. Uw servers virtueel zijn en niet meer namen hebt (u *hebt* mindsets van [huisdieren naar runderen](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nadat alle veranderd). Configuratie is kleiner over de machines en meer informatie over de services zelf. Speciale hardware is een nu tot het verleden en services geworden kleine gedistribueerde systemen, meerdere kleinere onderdelen van de rol hardware die in beslag nemen.

Als gevolg van uw app voorheen monolithische, doorverbonden verdelen in afzonderlijke services worden uitgevoerd op rol hardware, hebt u nu veel meer combinaties te handelen. Wie wordt bepaald welke soorten werkbelasting kunnen worden uitgevoerd op welke hardware of hoeveel? Welke werkbelasting ook aan dezelfde hardware werken en die conflicteren? Wanneer een machine afneemt... Wat is zelfs er uitgevoerd? Wie is verantwoordelijk ervoor te zorgen dat deze werkbelasting, begint u opnieuw uit te voeren? Wachten op de computer (virtual?) keert u terug of uw werkbelasting niet andere computers en behouden uitgevoerd? Menselijke tussenkomst vereist is? Hoe zit het met upgrades in deze sorteren van de omgeving?

Als ontwikkelaars en de operatoren die in deze sorteren van de wereld zitten, gaan we hulp bij het beheren van deze complexiteit en krijgt u de dat een indiensttreding binge en probeert op te nemen via de complexiteit papier met personen is niet het juiste antwoord te komen.

Wat moet u doen?

## <a name="introducing-orchestrators"></a>Inleiding tot orchestrators
Een "Orchestrator" is de algemene term naar een deel van de software waarmee beheerders van de volgende typen omgevingen beheren. Orchestrators zijn de onderdelen die in aanvragen halen, zoals 'Ik wil 5 kopieën van deze service uitgevoerd in Mijn omgeving', kunt u waar is, en kiest u vervolgens (probeer) te blijven op die manier.

Orchestrators (niet mensen) zijn wat bewegen in werking wanneer een machine mislukt of een werkbelasting om enkele onverwachte reden dan ook beëindigt. De meeste Orchestrators meer dan ze alleen maar handelen mislukt, zoals helpen met nieuwe implementaties, upgrades voor de verwerking en omgaan met resourceverbruik, maar alle zijn fundamenteel over het beheren van dat enkele gewenst status van de configuratie in de omgeving. Wilt u kunt een Orchestrator zien wat u wilt en hebt deze het dik werk te doen. Chronos of Marathon boven aan de Mesos, vloot Swarm, Kubernetes en Service stof ziet u alle voorbeelden van Orchestrators (of deze ingebouwd zijn). Meer altijd als de complexiteit van het beheren van de echte wereld implementaties in verschillende soorten omgevingen worden gemaakt en voorwaarden vergroten en wijzigen.

## <a name="orchestration-as-a-service"></a>Integratie als een service
De taak van de Orchestrator binnen een cluster Service stof wordt hoofdzakelijk door de Cluster Resource Manager verwerkt. De Service stof Cluster Resource Manager is een van de Services systeem binnen Service-structuur en binnen elke cluster automatisch is gestart.  De Cluster resourcemanager taak wordt in het algemeen wordt onderverdeeld in drie delen:

1. Regels afdwingen
2. Uw omgeving optimaliseren
3. Te helpen in andere processen

### <a name="what-it-isnt"></a>Wat is het geen
In traditionele N laag web-apps is er altijd sommige begrip van een 'taakverdeling', meestal waarnaar wordt verwezen als een netwerk laden verdeling (NLB) of een toepassing laden verdeling (ALB) afhankelijk van waar deze in de netwerken stapel sat. Sommige netwerktaakverdelers zijn gebaseerd zoals F5 van BigIP aanbod Hardware, anderen gebaseerd zoals Microsoft software NLB. In andere omgevingen ziet u mogelijk ongeveer zo HAProxy in deze rol. In deze architecturen is de taak van taakverdeling om ervoor te zorgen dat alle de front-end van verschillende stateless machines of de verschillende computers in het cluster (ongeveer) dezelfde hoeveelheid werk ontvangen. Strategieën voor deze variëren, vanuit elk ander doorschakelen naar een andere server, verzenden naar een sessie losmaken/kleverigheid, naar werkelijke schatting en gesprek toegewezen op basis van de verwachte kosten en de huidige machine laden.

Notitie die dit ten best was de om ervoor te zorgen dat de weblaag ongeveer blijft verdeeld. Strategieën voor het verdelen van de gegevenslaag zijn volledig verschillende en afhankelijk van de gegevens opslag, meestal Centreren rondom gegevens sharding, caching, beheerde weergaven en opgeslagen procedures, enz.

Hoewel enkele van deze strategieën interessante zijn, de Service stof Cluster Resource Manager is niet alles zoals een taakverdeling netwerk of een cache. Terwijl een netwerk taakverdeling zorgt ervoor dat de front-einden in overeenstemming zijn door te verplaatsen van verkeer naar waar de services worden uitgevoerd, het servicebeheer stof Cluster Resource heeft een volledig andere strategie – fundamenteel, Service stof *services* verplaatst naar waar u ze handig (en -verkeer is toegestaan of laden om te volgen verwacht). Dit is, bijvoorbeeld knooppunten die momenteel koudwatersystemen zijn, omdat de services die er zijn niet mee bezig zijn veel werk nu of die zijn verwijderd of verplaatst. Een ander voorbeeld kan de resourcemanager Cluster ook een service van een computer die is bijgewerkt of die overbelast is vanwege een Prikker in verbruik af door de services die zijn uitgevoerd verplaatsen. Omdat de resourcemanager Cluster is verantwoordelijk voor het verplaatsen van services rond (niet aan te bieden netwerkverkeer waar services al aanwezig zijn), bevat een sterk afwijkt functieset van vergeleken met wat u in een netwerk taakverdeling zoeken wilt en fundamenteel anders strategieën om ervoor te zorgen dat de hardware resources in het cluster ook worden gebruikt in dienst.

## <a name="next-steps"></a>Volgende stappen
- Voor informatie over de stroom architectuur en gegevens binnen de Cluster resourcemanager, raadpleegt u [in dit artikel](service-fabric-cluster-resource-manager-architecture.md)
- De Manager van de Resource Cluster heeft een groot aantal opties voor de beschrijving van het cluster. Als u wilt weten meer informatie over deze in dit artikel bij het [beschrijven van een Service stof cluster](service-fabric-cluster-resource-manager-cluster-description.md) hebt uitchecken
- Voor meer informatie over de andere opties die beschikbaar zijn voor het configureren van services uitchecken het onderwerp op de andere Cluster resourcemanager-configuraties beschikbaar [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Aan de doelstellingen zijn hoe de-stof Cluster Resource servicebeheer verbruik en capaciteit in het cluster beheert. Meer informatie over deze en hoe u deze configureert [in dit artikel](service-fabric-cluster-resource-manager-metrics.md) hebt uitchecken
- De Manager van de Resource Cluster werkt met de Service stof beheermogelijkheden. Meer informatie over dat integratie, Lees [dit artikel](service-fabric-cluster-resource-manager-management-integration.md)
- Voor meer informatie over hoe de resourcemanager Cluster beheert en saldi laden in het cluster, raadpleegt u het artikel over [het verdelen van laden](service-fabric-cluster-resource-manager-balancing.md)
