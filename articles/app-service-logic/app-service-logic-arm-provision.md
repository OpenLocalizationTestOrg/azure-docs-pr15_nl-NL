<properties 
    pageTitle="Een logica-App met Azure resourcemanager sjablonen in Azure App Service maken | Microsoft Azure" 
    description="Een sjabloon Azure Resource Manager gebruiken om te implementeren van een lege logica-App voor het definiëren van werkstromen." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Een logica-App met een sjabloon maken

Een sjabloon Azure Resource Manager gebruiken om een lege logica-toepassing die kan worden gebruikt om het definiëren van werkstromen te maken. U kunt definiëren welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven als de implementatie wordt uitgevoerd. U kunt het gebruik van deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Zie voor meer informatie over de eigenschappen van de app logica, [Logica App werkstroom Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Zie [auteur logica App definities](app-service-logic-author-definitions.md)voor voorbeelden van de definitie zelf. 

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md).

Zie [logica App-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json)voor de sjabloon voltooid.

## <a name="what-you-will-deploy"></a>Wat u gaat implementeren

Met deze sjabloon, moet u een app logica implementeren.

De implementatie automatisch wordt uitgevoerd, selecteert u de volgende knop:  

[![Implementeren naar Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

### <a name="logic-app"></a>Logica-app

Hiermee maakt u de logica-app.

De sjablonen een parameterwaarde op te geven voor de naam van de logica-app wordt gebruikt. De locatie van de app logica wordt ingesteld op dezelfde locatie als de resourcegroep. 

Deze bepaalde definitie wordt één keer per uur wordt uitgevoerd en Hiermee wordt de locatie die is opgegeven in de parameter **testUri** . 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
