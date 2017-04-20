<properties
    pageTitle="Een gebeurtenis Hubs naamruimte maken met gebeurtenis Hub en consumenten groep met een sjabloon van Azure resourcemanager | Microsoft Azure"
    description="Een gebeurtenis Hubs naamruimte maken met gebeurtenis Hub en consumenten groep met Azure resourcemanager-sjabloon"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Een gebeurtenis Hubs naamruimte met gebeurtenis Hub en consumenten groep met een sjabloon van Azure resourcemanager maken

In dit artikel leest hoe u een sjabloon van Azure resourcemanager die een gebeurtenis Hubs naamruimte met een Hub voor de gebeurtenis en een groep consumenten maakt gebruiken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt gebruik deze sjabloon voor uw eigen implementaties, of om te voldoen aan uw behoeften aanpassen

Voor meer informatie over het maken van sjablonen, raadpleegt u [Azure resourcemanager Authoring sjablonen][].

Zie de [gebeurtenis Hub en consumenten groep sjabloon][] op GitHub voor de sjabloon voltooid.

>[AZURE.NOTE]
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Sjablonen van Azure Quickstart][] en zoek naar gebeurtenis Hubs.

## <a name="what-will-you-deploy"></a>Wat wordt u implementeren?

Met deze sjabloon implementeert u een naamruimte gebeurtenis Hubs met een Hub voor de gebeurtenis en een groep consumenten.

[Gebeurtenis Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) is een gebeurtenis service gebruikte geven gebeurtenis en telemetrielogboek ingress Azure op grote schaal met lage latentie en hoge betrouwbaarheid verwerken.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die worden altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

De naam van de gebeurtenis Hubs naamruimte maken.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

De naam van de gebeurtenis Hub gemaakt in het logboek Hubs naamruimte.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

De naam van de consumenten groep gemaakt voor de gebeurtenis Hub.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

De API-versie van de sjabloon.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

Hiermee maakt u een naamruimte van het type **EventHubs**, met een Hub voor de gebeurtenis en een groep consumenten.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
[Azure Quickstart sjablonen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Sjabloon voor gebeurtenis Hub en consumenten groep]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
