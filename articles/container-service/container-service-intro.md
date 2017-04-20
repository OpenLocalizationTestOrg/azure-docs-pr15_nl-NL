<properties
   pageTitle="Azure Container Service Inleiding | Microsoft Azure"
   description="Azure Container-Service biedt een manier om te vereenvoudigen de maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure Container Service-Inleiding

Azure Container Service kunt u eenvoudiger kunt maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen. Een geoptimaliseerde configuratie van populaire open source plannings- en integratie's wordt gebruikt. Hiermee kunt u uw bestaande vaardigheden gebruiken of na een grote en groeiende hoofdtekst van de community expertise, te implementeren en beheren van de container-toepassingen op Microsoft Azure tekenen.


![Azure Container-Service biedt een manier om beperkte toepassingen op meerdere hosts op Azure te beheren.](./media/acs-intro/acs-cluster.png)


Azure Container Service maakt gebruik van de indeling van de container Docker om ervoor te zorgen dat uw toepassing containers volledig geschikter zijn. Ondersteunt ook uw keuze Marathon en domeincontroller/OS of Docker Swarm zodat u kunt deze toepassingen duizenden containers of zelfs tientallen duizendtallen schalen.

Met behulp van Azure Container-Service, kunt u profiteren van de enterprise-grade-functies van Azure, zonder dat zij de toepassing overdraagbaarheid--overdraagbaarheid bij de lagen integratie waaronder.

<a name="using-azure-container-service"></a>Gebruik van Azure Container-Service
-----------------------------

Ons doel met Azure Container-Service is om te creëren van een container hostomgeving met behulp van open source hulpprogramma's en technologieën die populair bij onze klanten vandaag zijn. Daartoe, laten we de standaard API-eindpunten zien voor de door u gekozen orchestrator (domeincontroller/OS of Docker Swarm). Met behulp van deze eindpunten, kunt u gebruikmaken van software die kan deze eindpunten spreekt. In het geval van het eindpunt Docker Swarm kunt u bijvoorbeeld de opdrachtregel Docker (CLI) gebruiken. Voor domeincontroller/OS kunt u de CLI DCOS gebruiken.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Maken van een Docker cluster met behulp van Azure Container-Service
-------------------------------------------------------

Om te beginnen met Azure Container Service implementeren u een cluster Azure Container-Service via de portal (zoeken naar 'Azure Container Service'), met behulp van een resourcemanager Azure-sjabloon ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) of [Domeincontroller/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) of met de [CLI](/documentation/articles/xplat-cli-install/). De meegeleverde quickstart sjablonen kunnen aanpassen om aanvullende of geavanceerde Azure configuratie. Zie voor meer informatie over het implementeren van een cluster Azure Container-Service [Deploy een cluster Azure Container-Service](container-service-deployment.md).

<a name="deploying-an-application"></a>Een toepassing implementeren
------------------------

Azure Container-Service biedt een keuze van Docker Swarm of domeincontroller/OS voor integratie. Hoe u uw toepassing implementeren, is afhankelijk van uw keuze van orchestrator.

### <a name="using-dcos"></a>Gebruik van de domeincontroller/OS

Domeincontroller/OS is een gedistribueerde besturingssysteem wordt uitgevoerd op basis van de Apache Mesos gedistribueerde systemen kernel. Apache Mesos is ondergebracht bij de Apache Software Foundation en worden enkele van de [grootste namen in IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) als gebruikers en inzenders.

![Azure Container-Service is geconfigureerd voor Swarm met agenten en outmodellen.](media/acs-intro/dcos.png)

Domeincontroller/OS en Apache Mesos bevatten een indrukwekkende functieset:

-   Bewezen schaalbaarheid

-   Fouttolerantie gerepliceerd diamodel en slaves Apache ZooKeeper gebruiken

-   Ondersteuning voor containers Docker-indeling

-   Systeemeigen moeten worden geïsoleerd tussen taken met Linux containers

-   Multiresource plannen (geheugen, CPU, schijf en poorten)

-   Java, Python en C++ APIs voor het ontwikkelen van nieuwe parallelle toepassingen

-   Een web gebruikersinterface voor het weergeven van cluster staat

Standaard bevat domeincontroller/OS Azure Container-Service waarop het platform Marathon integratie voor het plannen van werkbelasting. Wordt geleverd bij de implementatie van de domeincontroller/OS van ACS is echter de universum Mesosphere van services die kunnen worden toegevoegd aan uw service, hierbij een, Hadoop, Cassandra en nog veel meer.

![Domeincontroller/OS universum in Azure Container-Service](media/dcos/universe.png)

#### <a name="using-marathon"></a>Marathon gebruiken

Marathon is een cluster hele initialisatie en systeem voor services in cgroups-- of, in het geval van Azure-Service voor Container, containers Docker-indeling. Marathon voorzien van een web-gebruikersinterface van waaruit u uw toepassingen kunt implementeren. U kunt dit ook openen via een URL die er ongeveer zo `http://DNS_PREFIX.REGION.cloudapp.azure.com` waar DNS\_VOORVOEGSEL en regio zijn gedefinieerd bij de implementatie. U kunt de naam van uw eigen DNS-natuurlijk ook opgeven. Zie voor meer informatie over het uitvoeren van een container via het web Marathon UI [Container management via de gebruikersinterface van het web](container-service-mesos-marathon-ui.md).

![Lijst met toepassingen marathon](media/dcos/marathon-applications-list.png)

U kunt ook de REST API's gebruiken voor communicatie met Marathon. Er zijn een aantal clientbibliotheken die beschikbaar voor elk hulpmiddel zijn. Hierin zijn diverse talen-- en uiteraard kunt u het HTTP-protocol kunt gebruiken in een taal. Daarnaast bieden veel populaire DevOps extra ondersteuning voor Marathon. Dit biedt maximale flexibiliteit voor uw team bewerkingen wanneer u met een cluster Azure Container-Service werkt. Zie voor meer informatie over het uitvoeren van een container met behulp van de Marathon REST API [het beheer van de Container met de REST API](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Docker Swarm gebruiken

Docker Swarm biedt systeemeigen cluster voor Docker. Omdat Docker Swarm de standaard Docker-API fungeert, kunnen een hulpmiddel die al met een daemon Docker communiceert Swarm kunt gebruiken om transparant aan meerdere hosts op Azure Container-Service.

![Azure Container-Service is geconfigureerd voor gebruik van de domeincontroller/OS--met jumpbox, agenten en outmodellen.](media/acs-intro/acs-swarm2.png)

Ondersteunde hulpprogramma's voor het beheren van containers op een cluster Swarm opnemen, maar zijn niet beperkt tot, het volgende:

-   Dokku

-   Docker CLI en Docker opstellen

-   Krane

-   Jenkins

<a name="videos"></a>Video 's
------

Aan de slag met Azure Container-Service (101):  

> [AZURE.VIDEO azure-container-service-101]

Toepassingen maken met de Container Azure-Service (opbouwen 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
