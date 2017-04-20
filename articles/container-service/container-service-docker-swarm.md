<properties
   pageTitle="Azure Container container servicebeheer met Docker Swarm | Microsoft Azure"
   description="Containers implementeren naar een Docker Swarm in Azure Container-Service"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>Containerbeheer met Docker Swarm

Docker Swarm biedt een omgeving voor het implementeren van beperkte werkbelasting via een set gegroepeerde Docker hosts. Docker Swarm gebruikt de systeemeigen Docker-API. De werkstroom voor het beheren van containers op een Docker Swarm is bijna gelijk is aan wat het is op een enkele container-host. In dit document bevat eenvoudige voorbeelden van beperkte werkbelasting in een Container-Azure-Service-exemplaar van Docker Swarm implementeren. Zie voor meer gedetailleerde documentatie op Docker Swarm [Docker Swarm op Docker.com](https://docs.docker.com/swarm/).

Vereisten voor de oefeningen in dit document:

[Maken van een Swarm cluster in Azure Container-Service](container-service-deployment.md)

[Verbinding maken met het cluster Swarm in Azure Container-Service](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Een nieuwe container implementeren

Als u wilt een nieuwe container maken in de Docker Swarm, gebruikt u de `docker run` opdracht (en zorg ervoor dat u een tunnel SSH de modellen aan de hand van de bovenstaande voorwaarden hebt geopend). In dit voorbeeld wordt een container uit de `yeasy/simple-web` afbeelding:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Nadat de container is gemaakt, kunt u via `docker ps` om terug te keren informatie over de container. Hier kunt u ziet dat de Swarm-agent die als host de container fungeert wordt vermeld:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

U kunt nu toegang tot de toepassing die in deze container tot en met de naam van de verdeling van de Swarm agent belasting wordt uitgevoerd. U vindt deze informatie in de portal van Azure:  


![Reële bezoek resultaten](media/real-visit.jpg)  

Standaard is de taakverdeling poorten 80, 8080 en 443 openen. Als u koppelen op een andere poort wilt, moet u die poort op de verdeling van de Azure belasting voor de Agent-toepassingen openen.

## <a name="deploy-multiple-containers"></a>Meerdere containers implementeren

Als u meerdere containers zijn gestart, door te voeren 'docker uitvoeren' meerdere keren, kunt u de `docker ps` opdracht om te zien die de containers gehost op worden uitgevoerd. Klik in het onderstaande voorbeeld zijn drie containers verspreid gelijkmatig over de drie Swarm kunt vinden:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Containers implementeren met Docker opstellen

U kunt Docker opstellen gebruiken om de implementatie en configuratie van meerdere containers te automatiseren. Zorg ervoor dat een tunnel Secure Shell (SSH) is gemaakt en dat de DOCKER_HOST-variabele is ingesteld (Zie de bovenstaande minimumvereisten) hiervoor.

Een bestand docker-compose.yml op uw lokale systeem maken. Gebruik dit [voorbeeld](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml)hiervoor.

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Uitvoeren `docker-compose up -d` de container-implementaties starten:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Ten slotte worden de lijst met actieve containers geretourneerd. Deze lijst geeft de containers die zijn geïmplementeerd via Docker opstellen:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Natuurlijk kunt u `docker-compose ps` te onderzoeken alleen de containers die zijn gedefinieerd in uw `compose.yml` bestand.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Docker Swarm](https://docs.docker.com/swarm/)
