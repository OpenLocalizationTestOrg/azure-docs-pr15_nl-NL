<properties
    pageTitle="Hoge dichtheid hosting op Azure App Service | Microsoft Azure"
    description="Hoge dichtheid hosting op Azure App-Service"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Hoge dichtheid hosting op Azure App-Service

Wanneer u een App Service gebruikt, wordt uw toepassing van de capaciteit door twee concepten toegekende ontkoppelde:

- **De toepassing:** Hiermee geeft u de app en de configuratie runtime. Het bevat bijvoorbeeld de versie van .NET die de runtime moet hebt geladen, de instellingen van de app, enzovoort.

- **Het App-abonnement:** Hiermee definieert u de kenmerken van de capaciteit, functieset van beschikbaar en locatie van de toepassing. Bijvoorbeeld kenmerken mogelijk grote (vier cores) machine, vier exemplaren, Premium-functies in Oost-VS.

Een app is altijd gekoppeld aan een App Service-abonnement, maar een App Service-abonnement kunt zelf capaciteit om een of meer apps.

Het platform biedt daardoor de flexibiliteit om te isoleren van een app van één of meerdere apps resources delen door het delen van een App Service-abonnement hebt.

Echter wanneer meerdere apps een App Service-abonnement deelt, wordt een exemplaar van deze app op elk exemplaar van deze App serviceplan.

## <a name="per-app-scaling"></a>Per app schaalbaarheid
*Per app schaalbaarheid* is een functie die kan worden ingeschakeld op het niveau van de App Service-abonnement en klik vervolgens per toepassing gebruikt.

Schaalbaarheid per app aangepast een app onafhankelijk van de App serviceplan die als host fungeert. Op deze manier een App Service-abonnement kan worden geconfigureerd voor het leveren van 10 exemplaren, maar een app voor alleen 5 van deze kan worden ingesteld.

De volgende resourcemanager Azure-sjabloon maakt u een App Service-abonnement dat wordt aangepast aan 10 exemplaren en een app die geconfigureerd voor gebruik per app schaalbaarheid en schaal in slechts 5-exemplaren.

Het instellen van het App-abonnement is de eigenschap **schaalbaarheid van per-site** op waar ( `"perSiteScaling": true`). De app is als het **aantal werknemers** met 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Aanbevolen configuratie voor het hosten van hoge dichtheid

Per app schaalbaarheid is een functie die in zowel openbare Azure regio's en omgevingen met App-Service is ingeschakeld. De aanbevolen strategie is echter App serviceomgevingen gebruiken om te profiteren van hun geavanceerde functies en de grotere groepen capaciteit meenemen.  

Volg deze stappen om de hostingprovider voor uw apps met hoge dichtheid configureren:

1. De App-Service-omgeving geconfigureerd en kies een medewerker van toepassingen die is toegewezen aan het hoge dichtheid hostingprovider scenario.

1. Maak een enkel App Service-plan en schalen dat deze naar de beschikbare capaciteit op de werknemer van toepassingen gebruiken.

1. De website-schaal vlag ingesteld op waar de App-serviceplan.

1. Nieuwe sites worden gemaakt en toegewezen aan deze App Service-abonnement met de eigenschap **numberOfWorkers** is ingesteld op **1**. Deze configuratie levert de hoogste dichtheid van mogelijke in deze groep werknemer.

1. Het aantal werknemers kan afzonderlijk worden geconfigureerd per site om te verlenen aanvullende informatie naar wens. Een site intensief gebruik mogelijk bijvoorbeeld **numberOfWorkers** ingesteld op **3** als u wilt meer verwerkingscapaciteit voor deze app hebt terwijl laag gebruik sites zou **numberOfWorkers** ingesteld op **1**.
