<properties 
    pageTitle="Een WebApp dat is gekoppeld aan een bibliotheek GitHub implementeren" 
    description="Gebruik een resourcemanager Azure-sjabloon aan een WebApp met een project uit een bibliotheek GitHub implementeren." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Een WebApp gekoppeld aan een bibliotheek GitHub implementeren

In dit onderwerp leert u hoe u een sjabloon Azure resourcemanager die een web-app dat is gekoppeld aan een project in een bibliotheek GitHub implementeert maken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt het gebruik van deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md).

Zie [Web App gekoppelde aan GitHub sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json)voor de volledige sjabloon.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Wat u gaat implementeren

Met deze sjabloon implementeert u een web-app de code uit een project in GitHub bevat.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

De URL voor GitHub-bibliotheek met het project om te implementeren. Deze parameter een standaardwaarde bevat, maar deze waarde is alleen bedoeld om aan te geven hoe u de URL voor de bibliotheek. U kunt deze waarde op bij het testen van de sjabloon, maar u wilt voorzien in de URL van uw eigen opslagplaats tijdens het werken met de sjabloon.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>tak

De tak van de bibliotheek wilt gebruiken wanneer u de toepassing implementeren. De standaardwaarde is basispagina, maar u kunt ook de naam van een tak in de bibliotheek die u wilt implementeren opgeven.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>In de browser

Hiermee maakt u de web-app dat is gekoppeld aan het project in GitHub. 

U opgeven de naam van de web-app via de parameter **sitenaam** en de locatie van de web-app via de parameter **siteLocation** . In het element **dependsOn** definieert de sjabloon de web-app als afhankelijk van de service hosten van abonnement. Omdat deze afhankelijk van het abonnement waarop hostingprovider is, wordt de web-app is niet gemaakt totdat het hostingprovider abonnement is voltooid wordt gemaakt. Het element **dependsOn** wordt alleen gebruikt voor implementatie volgorde opgeven. Als u de web-app als afhankelijk van het abonnement waarop hostingprovider niet inschakelt, Azure Resource Mananger probeert te maken van beide resources op hetzelfde moment en u kunt een fout kan optreden als de web-app wordt gemaakt voordat het abonnement waarop hostingprovider.

De web-app, heeft ook een onderliggende resource die is gedefinieerd in de sectie **bronnen** hieronder. Deze resource onderliggende definieert besturingselement voor gegevensbronnen voor het project web App is geïmplementeerd. In deze sjabloon, die wordt het besturingselement voor gegevensbronnen gekoppeld aan een bepaald GitHub-bibliotheek. De bibliotheek GitHub is gedefinieerd met behulp van de code **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** u mogelijk harde code de URL van de bibliotheek als u maken van een sjabloon die u net zo vaak één project wilt implementeert tijdens het vereisen van het minimum aantal parameters.
In plaats van de harde codering de URL van de bibliotheek, kunt u een parameter toevoegen voor de URL van de bibliotheek en die waarde voor de eigenschap **RepoUrl** gebruiken.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
