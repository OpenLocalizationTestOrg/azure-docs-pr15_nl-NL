<properties
    pageTitle="Een Service Bus naamruimte met een sjabloon resourcemanager maken | Microsoft Azure"
    description="Azure resourcemanager sjabloon gebruiken om te maken van een Service Bus naamruimte"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Een Service Bus naamruimte met behulp van een resourcemanager Azure-sjabloon maken

In dit artikel wordt beschreven hoe u een sjabloon van Azure resourcemanager die een Service Bus naamruimte van het type **Messaging** met een standaard/Basic SKU maakt gebruiken. Het artikel definieert ook de parameters die zijn opgegeven voor de uitvoering van de implementatie. U kunt deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Voor meer informatie over het maken van sjablonen, raadpleegt u [Azure resourcemanager Authoring sjablonen][].

Zie de [Service Bus naamruimte sjabloon][] op GitHub voor de sjabloon voltooid.

>[AZURE.NOTE] De volgende resourcemanager Azure-sjablonen zijn beschikbaar voor het downloaden en uitvoeren. 
>
>-    [Een gebeurtenis Hubs naamruimte maken met een gebeurtenis Hub en consumenten-groep](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Een Service Bus naamruimte met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een naamruimte Service Bus maken met het onderwerp en abonnementen](service-bus-resource-manager-namespace-topic.md)
>-    [Een Service Bus naamruimte met wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Sjablonen van Azure Quickstart][] en zoek naar de Service Bus.

## <a name="what-will-you-deploy"></a>Wat wordt u implementeren?

Met deze sjabloon implementeert u een naamruimte Service Bus met een [eenvoudige, standaard, of Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die worden altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd.

Deze sjabloon bepaalt de volgende parameters.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de naamruimte Service Bus maken.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

De naam van de Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) maken.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (Basic, Standard of Premium) en wijst u een standaardwaarde (standaard) als u geen waarde is opgegeven.

Zie voor meer informatie over het Service Bus prijzen, [Service prijzen en facturering][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

De Service Bus API-versie van de sjabloon.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

### <a name="service-bus-namespace"></a>Service Bus naamruimte

Hiermee maakt u een standaard-Service Bus naamruimte van het type **Messaging**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Volgende stappen

Nu u hebt gemaakt en geïmplementeerd bronnen resourcemanager Azure gebruikt, lees hoe u deze resources beheren met het lezen van deze artikelen:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Service Bus resources met de Service Bus Explorer beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
  [Service Bus naamruimte sjabloon]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Service Bus prijzen en facturering]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
