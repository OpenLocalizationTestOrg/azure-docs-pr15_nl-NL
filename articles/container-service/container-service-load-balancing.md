<properties
   pageTitle="Laden balance '-containers in een Container-Azure-Service cluster | Microsoft Azure"
   description="Taken te verdelen over meerdere containers in een cluster Azure Container-Service."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, domeincontroller/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Laden balance '-containers in een cluster Azure Container-Service

In dit artikel bekijken we het maken van een interne taakverdeling in een een domeincontroller/OS beheerde Azure Container-Service met Marathon kg. Hiermee kunt u horizontaal schalen dat uw toepassingen. Dit ook kunt u profiteren van de openbare en persoonlijke-agent clusters door uw netwerktaakverdelers in het openbare cluster en uw toepassing containers op de persoonlijke cluster plaatsen.

## <a name="prerequisites"></a>Vereisten voor

Met orchestrator type domeincontroller/OS [Deploy een exemplaar van Azure Container-Service](container-service-deployment.md) en [Zorg ervoor dat de klant verbinding met uw cluster maken kunt](container-service-connect.md). 

## <a name="load-balancing"></a>Taakverdeling

Er zijn twee taakverdeling lagen in de Container Service cluster maakt: 

  1. Azure taakverdeling biedt openbare vermelding punten (de bouwstenen die eindgebruikers wordt raken). Hiermee wordt automatisch geleverd door Azure Container-Service en is standaard poort 80, 443 en 8080 geconfigureerd.
  2. De taakverdeling Marathon (marathon kg) stuurt binnenkomende aanvragen in container-exemplaren die deze serviceaanvragen. Als we de containers leveren onze webservice wilt verkleinen, past marathon kg dynamisch. Deze taakverdeling niet wordt opgegeven al dan niet standaard in uw Service Container, maar het is heel eenvoudig te installeren.

## <a name="marathon-load-balancer"></a>Marathon taakverdeling

Marathon taakverdeling opnieuw dynamisch op basis van de containers die u hebt geïmplementeerd. Het is ook tegen verlies van een container of een agent - als dit gebeurt, Apache Mesos gewoon de container elders opnieuw en marathon kg wordt aanpassen.

Als u wilt installeren web de Marathon taakverdeling kunt u een van beide de domeincontroller/OS UI of de opdrachtregel.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Installeren Marathon kg domeincontroller/OS Web UI gebruiken

  1. Klik op 'Universum'
  2. Zoeken naar "Marathon kg"
  3. Klik op 'Installeren'

![Installatie van marathon kg via de domeincontroller/OS Web-Interface](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Gebruik van de domeincontroller/OS CLI Marathon kg installeren

U kunt uw cluster, voer de volgende opdracht vanaf de clientcomputer verbinden na het installeren van de domeincontroller/OS CLI en ervoor zorgen dat:

```bash
dcos package install marathon-lb
```

Deze opdracht wordt automatisch de taakverdeling geïnstalleerd op de openbare agenten cluster.

## <a name="deploy-a-load-balanced-web-application"></a>Een laden implementeren verdeeld webtoepassing

Nu dat we het pakket marathon kg hebben, kunnen we een toepassingscontainer die we willen taakverdeling implementeren. In dit voorbeeld wordt we een eenvoudige webserver implementeren met behulp van de volgende configuratie:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * De waarde van `HAProxy_0_VHOST` naar de FQDN-naam van de verdeling van de belasting voor uw agenten. Dit is in het formulier `<acsName>agents.<region>.cloudapp.azure.com`. Als u een Container Service cluster maken met de naam bijvoorbeeld `myacs` in regio `West US`, de FQDN-naam zou `myacsagents.westus.cloudapp.azure.com`. U kunt dit ook vinden door te zoeken naar de verdeling van de belasting met 'agent' in de naam wanneer u op zoek bent door de resources in de resourcegroep die u hebt gemaakt voor de Container-Service in de [portal van Azure](https://portal.azure.com).
  * De servicePort instellen op een poort > = 10.000. Hiermee wordt de service die wordt uitgevoerd in deze container--geïdentificeerd marathon kg gebruikmaakt van deze services die deze alle moet balans identificeren.
  * Stel de `HAPROXY_GROUP` label tot 'externe'.
  * Stel `hostPort` 0. Dit betekent dat Marathon willekeurig een beschikbare poort wordt toegewezen.
  * Stel `instances` voor het aantal exemplaren dat u wilt maken. U kunt altijd schalen deze omhoog en omlaag later.

Het verdient aanbeveling noing dat al dan niet standaard die marathon aan het privé cluster implementeert, dit betekent dat de bovenstaande implementatie alleen toegankelijk via uw taakverdeling, welke is meestal het gedrag die we nodig hebben.

### <a name="deploy-using-the-dcos-web-ui"></a>Gebruik van de gebruikersinterface van de Web domeincontroller/OS implementeren

  1. Bezoek de Marathon op http://localhost/marathon (na het instellen van uw [SSH tunnel](container-service-connect.md) en klik op`Create Appliction`
  2. In de `New Application` dialoogvenster Klik `JSON Mode` in de rechterbovenhoek
  3. Plak de bovenstaande JSON in de editor
  4. Klik op`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Gebruik van de domeincontroller/OS CLI implementeren

Implementatie van deze toepassing met de domeincontroller/OS CLI gewoon Kopieer de bovenstaande JSON naar een bestand met de naam `hello-web.json`, en uitvoeren:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure taakverdeling

Standaard beschrijft Azure taakverdeling poorten 80, 8080 en 443. Als u gebruikmaakt van een van deze drie poorten (zoals we in het voorbeeld hierboven doen), dan is er niets hoeft te doen. U moet kunnen te raken uw agent taakverdeling FQDN-- en telkens wanneer die u vernieuwt, u een van de drie endwebservers op een wijze round robin wordt raken. Als u een andere poort gebruikt, moet u een regel round robin en een test toevoegen op de verdeling van de belasting voor de poort die u gebruikt. U kunt dit doen vanuit de [Azure CLI](../xplat-cli-azure-resource-manager.md), met de opdrachten `azure network lb rule create` en `azure network lb probe create`. U kunt ook deze stap met behulp van de Azure-Portal.


## <a name="additional-scenarios"></a>Extra scenario 's

U kunt een scenario waar u verschillende domeinen gebruiken om weer te geven van de verschillende services hebben. Bijvoorbeeld:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Hiertoe uitchecken [virtuele hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), die zelf een formule domeinen tot specifieke marathon kg paden koppelen.

U kunt ook laten zien verschillende poorten en deze naar de juiste service achter marathon kg toewijzen. Bijvoorbeeld:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Volgende stappen

Zie de documentatie domeincontroller/OS voor meer aan [marathon kg](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).
