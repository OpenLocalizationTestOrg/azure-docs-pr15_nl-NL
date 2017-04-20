<properties 
    pageTitle="Inrichten van een bestand Vgx. Cache | Microsoft Azure" 
    description="Resourcemanager Azure-sjabloon gebruiken om te implementeren van een Azure bestand Vgx. Cache." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Een bestand Vgx. Cache met een sjabloon maken

In dit onderwerp leert u hoe u een sjabloon Azure resourcemanager die een Azure bestand Vgx. Cache implementeert maken. De cache kan worden gebruikt met een bestaand account voor de opslag diagnostische gegevens op te slaan. Ook leert u hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Diagnostische instellingen zijn op dit moment gedeeld voor alle cache in hetzelfde gebied voor een abonnement. Bijwerken van één cache in de regio van invloed op alle andere cache in de regio.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md).

Zie [bestand Vgx. Cache sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json)voor de volledige sjabloon.

>[AZURE.NOTE] Resourcemanager sjablonen voor de nieuwe [Premium laag](cache-premium-tier-intro.md) zijn beschikbaar. 
>
>-    [Een Premium bestand Vgx. Cache met cluster maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Premium bestand Vgx. Cache maken met gegevens permanente](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Premium bestand Vgx. Cache met VNet en optioneel cluster maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Als u wilt controleren of er voor de meest recente sjablonen, Zie [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/) en zoek naar `Redis Cache`.

## <a name="what-you-will-deploy"></a>Wat u gaat implementeren

U kunt een Azure bestand Vgx. Cache die gebruikmaakt van een bestaande opslag-account voor diagnostische gegevens gaat implementeren in deze sjabloon.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters die alle van de parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

De locatie van de Cache bestand Vgx.. Voor de beste prestaties dezelfde locatie als de app te gebruiken om te worden gebruikt met de cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

De naam van de bestaande opslag-account voor diagnostische gegevens wilt gebruiken. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Een Booleaanse waarde die of aangeeft om toegang te krijgen via een niet-SSL-poorten.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Een waarde die aangeeft of diagnostische gegevens is ingeschakeld. Gebruik aan of uit.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

### <a name="redis-cache"></a>Bestand Vgx Cache.

Hiermee maakt u de Cache Azure bestand Vgx..

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


