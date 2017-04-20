<properties
   pageTitle="Azure-Service stof clusters maken op Windows Server en Linux | Microsoft Azure"
   description="Service stof clusters uitvoeren op Windows Server en Linux, wat inhoudt dat u kunt wel om te implementeren en overal host stof Service-toepassingen die u kunt uitvoeren Windows Server- of Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Service stof clusters op Windows Server- of Linux maken

Azure-Service stof kunt voor het maken van Service stof clusters op VMs en computers met Windows Server- of Linux. Dit betekent dat u zich kunt implementeren en Service stof toepassingen uitvoeren in een omgeving waarin u een reeks Windows Server- of Linux-computer onderling hebt, worden deze on-premises Microsoft Azure of met een cloud-provider.

##<a name="create-service-fabric-clusters-on-azure"></a>Service stof clusters op Azure maken

Maken van een cluster op Azure is voltooid, hetzij via een sjabloon Resource Model of de Azure-portal. Lees [een Service stof cluster met behulp van een sjabloon resourcemanager maken](service-fabric-cluster-creation-via-arm.md) of [een Service stof cluster van de Azure-portal maken](service-fabric-cluster-creation-via-portal.md) voor meer informatie.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Welke besturingssystemen worden ondersteund voor clusters op Azure

U zijn kunt maken van clusters op VMs met deze besturingssystemen:

* Windows Server 2012 R2
* 2016 voor Windows-Server (nadat deze is aangekondigd als algemeen beschikbaar)
* Linux Ubuntu 16.04 (in openbare preview) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Service stof zelfstandige clusters on-premises of bij een provider cloud maken

Service stof biedt een pakket installeren op een willekeurige cloud-provider of voor u zelfstandige Service stof clusters on-premises implementatie maken

Lees voor meer informatie over het instellen van zelfstandige service stof clusters op Windows Server, [Service stof cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Een cloud-implementaties versus on-premises implementaties
Het proces voor het maken van een Service stof cluster on-premises implementatie is vergelijkbaar met het proces van het maken van een cluster van een wolk van uw keuze met een set VMs. De eerste stappen voor het inrichten van de VMs is afhankelijk van de cloud-provider of on-premises omgeving dat u gebruikt. Nadat u een reeks VMs met netwerkconnectiviteit ingeschakeld ertussen hebt, klikt u vervolgens de stappen voor het instellen van het pakket Service stof de clusterinstellingen bewerken en uitvoeren van het maken van het cluster en management scripts zijn identiek. Dit zorgt ervoor dat uw kennis en ervaring van besturingssysteem en beheren van Service stof clusters overdraagbaar als u besluit om het afstemmen van nieuwe hostingprovider omgevingen.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Voordelen van het maken van zelfstandige Service stof clusters
* U bent gratis elke cloud-provider voor het hosten van uw cluster kiezen.
* Stof service-toepassingen, eenmaal geschreven, kunnen worden uitgevoerd in meerdere hostingprovider omgevingen met minimale geen wijzigingen.
* Kennis van het bouwen van Service stof toepassingen weerspiegelt uit één hostomgeving naar de andere.
* Praktijkervaring van uitvoeren en het beheren van Service stof clusters uitvoert via van één omgeving naar een andere.
* Globaal klant bereik is niet-gebonden door te hosten omgeving beperkingen.
* Er bestaat een extra niveau van betrouwbaarheid en bescherming tegen de algemene bijvoorbeeld omdat u kunt de services Beweeg over naar een andere implementatieomgeving als een afdeling of cloud-gegevensprovider een zwart heeft.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Welke besturingssystemen worden ondersteund voor zelfstandige clusters
U zijn kunt clusters maken op VMs computers met deze besturingssystemen:

* Windows Server 2012 R2
* 2016 voor Windows-Server (nadat deze is aangekondigd als algemeen beschikbaar)
* Linux (binnenkort)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Voordelen van Service stof clusters op Azure via zelfstandige die service stof clusters gemaakt on-premises implementatie

Service stof clusters op Azure biedt voordelen ten opzichte van de on-premises implementatie optie, dus als u geen specifieke behoeften aan waar u uw kolomgroepen uitvoeren en vervolgens wordt u aangeraden dat u ze worden uitgevoerd op Azure. Wij bieden integratie met andere Azure-functies en services, waardoor u bewerkingen en beheer van het cluster begrip op Azure.

* **Azure-portal:** Azure-portal kunt u gemakkelijk maken en beheren van clusters.

* **Azure resourcemanager:** Gebruik van Azure resourcemanager kunt u eenvoudig beheren alle resources die zijn gedefinieerd door het cluster als tijdseenheid en eenvoudiger kosten bijhouden en facturering.
* **Service stof Cluster als een Azure-bron** Een Service stof cluster is een bron ARM, zodat u deze modelleren kunt net als andere bronnen op ARM in Azure wordt aangegeven.
* **Integratie met Azure-infrastructuur** Service stof coördinaten met de onderliggende Azure infrastructuur voor OS, netwerk- en andere upgrades beschikbaarheid en betrouwbaarheid van uw toepassingen te verbeteren.  
* **Diagnostische hulpprogramma's:** Op Azure bieden we integratie Azure diagnostisch hulpprogramma en Log Analytics.
* **Automatisch schalen:** Wij bieden ingebouwde automatisch schalen functionaliteit vanwege VM schaal-sets voor kolomgroepen op Azure. In de on-premises implementatie en andere cloud-omgevingen moet u ook uw eigen automatisch schalen functie of schaal handmatig met behulp van de API's die Service stof beschrijft voor schaalbaarheid van clusters samenstellen.

## <a name="next-steps"></a>Volgende stappen
Maak een cluster op VMs of computers met Windows Server: [Service stof cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Maak een cluster op VMs of computers waarop Linux: [Service stof op Linux](service-fabric-linux-overview.md)
