<properties 
    pageTitle="Inrichten WebApp met de Cache bestand Vgx." 
    description="Resourcemanager Azure-sjabloon gebruiken om te implementeren WebApp met bestand Vgx. Cache." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erickson-doug" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a>Een Web App plus bestand Vgx. Cache met een sjabloon maken

In dit onderwerp leert u hoe u een sjabloon Azure resourcemanager die een Azure-Web-App met bestand Vgx. cache implementeert maken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md).

Zie [Web App met de Cache bestand Vgx. sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json)voor de volledige sjabloon.

## <a name="what-you-will-deploy"></a>Wat u gaat implementeren

In deze sjabloon, die implementeert u:

- Azure WebApp
- Azure bestand Vgx. Cache.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Parameters om op te geven

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[AZURE.INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Variabelen voor namen

Variabelen in deze sjabloon worden gebruikt om namen voor de bronnen samen te stellen. Dit wordt de functie [uniqueString](../resource-group-template-functions.md#uniquestring) gebruikt om een waarde op basis van de resource groeps-id te maken.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a>Bestand Vgx Cache.

Hiermee maakt u de Azure bestand Vgx. Cache die wordt gebruikt met de web-app. De naam van de cache is opgegeven in de variabele **cacheName** .

De sjabloon wordt gemaakt van de cache op dezelfde locatie als de resourcegroep. 

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>In de browser

Hiermee maakt u de WebApp met de naam die zijn opgegeven in de variabele **Websitenaam** .

Zoals u ziet dat de web-app is geconfigureerd met eigenschappen van de app instellen dat deze voor gebruik met de Cache bestand Vgx. inschakelen. Deze app instellingen dynamisch worden gemaakt op basis van waarden die zijn opgegeven tijdens de implementatie.
        
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup


