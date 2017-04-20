<properties
    pageTitle="Een naamruimte Service Bus maken met het onderwerp en abonnement met een sjabloon van Azure resourcemanager | Microsoft Azure"
    description="Een naamruimte Service Bus maken met het onderwerp en abonnement met Azure resourcemanager-sjabloon"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Een naamruimte Service Bus maken met het onderwerp en abonnement met een sjabloon van Azure resourcemanager

In dit artikel leest hoe u een Azure resourcemanager sjabloon gebruikt die Hiermee maakt u een naamruimte Service Bus met een onderwerp en een abonnement. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt gebruik deze sjabloon voor uw eigen implementaties, of om te voldoen aan uw behoeften aanpassen

Voor meer informatie over het maken van sjablonen, raadpleegt u [Azure resourcemanager Authoring sjablonen][].

Voor de volledige sjabloon, raadpleegt u de sjabloon [Service Bus naamruimte met het onderwerp en abonnementen][] .

>[AZURE.NOTE] De volgende resourcemanager Azure-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een Service Bus naamruimte met wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Een Service Bus naamruimte met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een Service Bus naamruimte maken](service-bus-resource-manager-namespace.md)
>-    [Een gebeurtenis Hubs naamruimte maken met een gebeurtenis Hub en consumenten-groep](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en het zoeken naar "Service Bus."

## <a name="what-will-you-deploy"></a>Wat wordt u implementeren?

Met deze sjabloon implementeert u een naamruimte Service Bus met het onderwerp en abonnementen.

Geef een een-op-veel-formulier van communicatie, in een patroon *publiceren/abonneren* [Service Bus onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) .

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die worden altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de naamruimte Service Bus maken.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

De naam van het onderwerp in de naamruimte Service Bus hebt gemaakt.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

De naam van het abonnement dat is gemaakt in de naamruimte Service Bus.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

De Service Bus API-versie van de sjabloon.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

Hiermee maakt u een standaard-Service Bus naamruimte van het type **Messaging**, met het onderwerp en abonnementen.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu u hebt gemaakt en geïmplementeerd bronnen resourcemanager Azure gebruikt, lees hoe u deze resources beheren door te bekijken van deze artikelen:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Service Bus resources met de Service Bus Explorer beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus naamruimte met het onderwerp en abonnementen]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
