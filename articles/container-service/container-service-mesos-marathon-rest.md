<properties
   pageTitle="Azure Container container servicebeheer via de REST API | Microsoft Azure"
   description="Containers dashboard implementeren naar een Azure Container Service Mesos cluster met behulp van de Marathon REST API."
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

# <a name="container-management-through-the-rest-api"></a>Containerbeheer van de met de REST API

Domeincontroller/OS biedt een omgeving voor de implementatie en schaalbaarheid van gegroepeerd werkbelastingen, terwijl de onderliggende hardware samenvatten. Boven aan de domeincontroller/OS, moet u er een kader die worden beheerd plannen en uitvoeren van berekeningscluster werkbelasting is.

Hoewel kaders beschikbaar voor veel populaire werkbelasting zijn, wordt dit document beschreven hoe u kunt maken en container implementaties schalen met behulp van Marathon. Voordat u deze voorbeelden moet u een domeincontroller/OS cluster die is geconfigureerd in Azure Container-Service. U moet ook externe connectiviteit met dit cluster zijn. Zie de volgende artikelen voor meer informatie over deze items:

- [Een cluster Azure Container-Service implementeren](container-service-deployment.md)
- [Verbinding maken met een cluster Azure Container-Service](container-service-connect.md)

Nadat u met het cluster Azure Container-Service verbonden bent, kunt u de domeincontroller/OS en de gerelateerde REST API's kunt openen via http://localhost:local-poort. De voorbeelden in dit document wordt ervan uitgegaan dat u via tunneling op poort 80. Bijvoorbeeld het eindpunt Marathon bereikbaar `http://localhost/marathon/v2/`. Zie voor meer informatie over de verschillende-API's, de Mesosphere-documentatie bij de [Marathon-API](https://mesosphere.github.io/marathon/docs/rest-api.html) en de [Chronos-API](https://mesos.github.io/chronos/docs/api.html)en de Apache documentatie voor de [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Verzamel informatie van de domeincontroller/OS en Marathon

Voordat u containers bij het cluster domeincontroller/OS implementeert, verzamel vindt u informatie over het cluster domeincontroller/OS, zoals de namen en de huidige status van de domeincontroller/OS agenten. Klik hiervoor query de `master/slaves` eindpunt van de domeincontroller/OS REST API. Als alles goed gaat, ziet u een lijst met domeincontroller/OS agenten en verschillende eigenschappen voor elk label.

```bash
curl http://localhost/mesos/master/slaves
```

Nu, gebruikt u de Marathon `/apps` eindpunt wilt controleren op de huidige toepassing implementaties aan het cluster domeincontroller/OS. Als dit een nieuw cluster, ziet u een lege matrix voor apps.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Een container Docker-indeling implementeren

U implementeren containers via Marathon Docker-indeling met behulp van een JSON-bestand met een beschrijving van de beoogde implementatie. In het onderstaande voorbeeld wordt de container Nginx, binding poort 80 van de domeincontroller/OS-agent poort 80 van de container implementeren. Houd er ook rekening mee dat de eigenschap 'acceptedResourceRoles' is ingesteld op 'slave_public'. Hiermee wordt de container dashboard implementeren naar een agent in het openbare agent schaal instellen.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Uw eigen JSON-bestand maken om te implementeren in een container Docker-indeling, of gebruik van de steekproef gegeven aan [demo Azure Container-Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Opslaan in een toegankelijke locatie. Als u wilt implementeren de container, voer de volgende opdracht uit. Geef de naam van de JSON-bestand.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

De uitvoer is ongeveer als volgt uit:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Als u een query Marathon voor toepassingen uitvoert, wordt nu deze nieuwe toepassing in de uitvoer weergegeven.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>De schaal van uw containers aanpassen

U kunt ook de Marathon-API gebruiken om te schalen af of schaal in implementaties van toepassing. In het vorige voorbeeld, moet u één exemplaar van een toepassing geïmplementeerd. Laten we schalen dat deze out naar drie exemplaren van een toepassing. Klik hiervoor een JSON-bestand maken met behulp van de volgende JSON-tekst en opslaan in een toegankelijke locatie.

```json
{ "instances": 3 }
```

Voer de volgende opdracht aan de nieuwe schaal van de toepassing.

>[AZURE.NOTE] De URI zijn http://localhost/marathon/v2/apps/ en klik vervolgens op de ID van de toepassing aan de nieuwe schaal. Als u de steekproef Nginx die is verstrekt hier gebruikt, zou de URI http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Tot slot indexquery het eindpunt Marathon voor toepassingen. U ziet dat er nu drie van de containers Nginx.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>PowerShell gebruiken voor deze oefening: Marathon REST API interactie met PowerShell

U kunt deze dezelfde acties uitvoeren via PowerShell-opdrachten op een Windows-bestandssysteem.

Voer de volgende opdracht verzamelt gegevens over de domeincontroller/OS cluster, zoals agent namen en agent status.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

U implementeren containers via Marathon Docker-indeling met behulp van een JSON-bestand met een beschrijving van de beoogde implementatie. In het onderstaande voorbeeld wordt de container Nginx, binding poort 80 van de domeincontroller/OS-agent poort 80 van de container implementeren.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Maak uw eigen JSON-bestand of gebruiken van de steekproef gegeven aan [demo Azure Container-Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Opslaan in een toegankelijke locatie. Als u wilt implementeren de container, voer de volgende opdracht uit. Geef de naam van de JSON-bestand.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

U kunt ook de Marathon-API gebruiken om te schalen af of schaal in implementaties van toepassing. In het vorige voorbeeld, moet u één exemplaar van een toepassing geïmplementeerd. Laten we schalen dat deze out naar drie exemplaren van een toepassing. Klik hiervoor een JSON-bestand maken met behulp van de volgende JSON-tekst en opslaan in een toegankelijke locatie.

```json
{ "instances": 3 }
```

Voer de volgende opdracht aan de nieuwe schaal van de toepassing.

> [AZURE.NOTE] De URI zijn http://localhost/marathon/v2/apps/ en klik vervolgens op de ID van de toepassing aan de nieuwe schaal. Als u de Nginx steekproef opgegeven hier gebruikt, zou de URI http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Volgende stappen

- [Lees meer over de Mesos HTTP-eindpunten]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Lees meer over de Marathon REST API]( https://mesosphere.github.io/marathon/docs/rest-api.html).
