<properties
    pageTitle="Een naamruimte Service Bus maken met het onderwerp, abonnement, en regel met een sjabloon van Azure resourcemanager | Microsoft Azure"
    description="Een naamruimte Service Bus maken met het onderwerp, abonnement en regel met Azure resourcemanager sjabloon"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Een naamruimte Service Bus maken met het onderwerp, abonnement en regel met een sjabloon van Azure resourcemanager

In dit artikel leest hoe u een sjabloon van Azure resourcemanager die een naamruimte Service Bus met een onderwerp, abonnement en regel (filter maakt) gebruiken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt gebruik deze sjabloon voor uw eigen implementaties, of om te voldoen aan uw behoeften aanpassen

Zie voor meer informatie over het maken van sjablonen, [Azure resourcemanager Authoring sjablonen][].

Zie voor meer informatie over het oefenen en patronen op Azure Resources naamgevingsconventies, [Azure Resources naamgevingsconventies voor bestanden][].

Voor de volledige sjabloon, raadpleegt u de [Service Bus naamruimte met onderwerp, abonnement, en regel][] -sjabloon.

>[AZURE.NOTE] De volgende resourcemanager Azure-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een Service Bus naamruimte met wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Een Service Bus naamruimte met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een Service Bus naamruimte maken](service-bus-resource-manager-namespace.md)
>-    [Een naamruimte Service Bus maken met het onderwerp en abonnementen](service-bus-resource-manager-namespace-topic.md)
>
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Sjablonen van Azure Quickstart][] en zoek naar de Service Bus.

## <a name="what-will-you-deploy"></a>Wat wordt u implementeren?

Met deze sjabloon, moet u een naamruimte Service Bus met onderwerp, abonnement en regel (filter) implementeren.

Geef een een-op-veel-formulier van communicatie, in een patroon *publiceren/abonneren* [Service Bus onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) . Bij gebruik van onderwerpen en -abonnement, onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar communiceren in plaats daarvan uitwisselen deze berichten via een onderwerp dat bemiddelt. Een abonnement op een onderwerp lijkt op een virtuele wachtrij die ontvangt kopie van berichten die zijn verzonden naar het onderwerp. Een filter voor een abonnement kunt u opgeven welke berichten u verzonden naar een onderwerp dat moet worden weergegeven binnen een bepaald onderwerp-abonnement.

## <a name="what-are-rules-filters"></a>Wat zijn regels (filters)?

In veel gevallen moeten de berichten die specifieke kenmerken hebben op verschillende manieren worden verwerkt. U kunt abonnementen als u wilt zoeken naar berichten die u hebt de keuze eigenschappen en voer daarna bepaalde wijzigingen aanbrengen in deze eigenschappen configureren om dit mogelijk. Terwijl de Service Bus abonnementen ziet alle berichten die naar het onderwerp, kunt u alleen een subset van deze berichten kopiëren naar de wachtrij virtuele abonnement. Dit gebeurt abonnement filters gebruiken. Zie voor meer informatie op rules(filters), [Service Bus wachtrijen, onderwerpen en abonnementen][].

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager, moet u parameters voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd definiëren. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

De naam van de rule(filter) gemaakt in de naamruimte Service Bus.

```
   "serviceBusRuleName": {
   "type": "string",
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

Hiermee maakt u een standaard-Service Bus naamruimte van het type **Messaging**, met het onderwerp en abonnement en regels.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu u hebt gemaakt en geïmplementeerd bronnen resourcemanager Azure gebruikt, lees hoe u deze resources beheren door te bekijken van deze artikelen:

- [Azure-Service Bus met Azure automatisering beheren](service-bus-automation-manage.md)
- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Service Bus resources met de Service Bus Explorer beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure Resources naamgevingsconventies]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Service Bus naamruimte met onderwerp, abonnement en regel]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Service Bus wachtrijen, onderwerpen en abonnementen]:service-bus-queues-topics-subscriptions.md
  
