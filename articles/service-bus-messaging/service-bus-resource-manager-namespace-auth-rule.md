<properties
    pageTitle="Een Service Bus autorisatieregel maken met een sjabloon Azure resourcemanager | Microsoft Azure"
    description="Een regel van de Service Bus autorisatie voor naamruimte en wachtrij met Azure resourcemanager sjabloon maken"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Een regel van de Service Bus autorisatie voor naamruimte en wachtrij met een sjabloon van Azure resourcemanager maken

In dit artikel leest hoe u een sjabloon van Azure resourcemanager die Hiermee maakt u een [autorisatieregel voor](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) voor een Service Bus naamruimte en wachtrij gebruiken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon voor uw eigen implementaties of aanpassen aan uw vereisten voldoet.

Voor meer informatie over het maken van sjablonen, raadpleegt u [Azure resourcemanager Authoring sjablonen][].

Zie de [Service Bus auth regelsjabloon][] op GitHub voor de sjabloon voltooid.

>[AZURE.NOTE] De volgende resourcemanager Azure-sjablonen zijn beschikbaar voor het downloaden en uitvoeren.
>
>-    [Een gebeurtenis Hubs naamruimte maken met een gebeurtenis Hub en consumenten-groep](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Een Service Bus naamruimte met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
>-    [Een naamruimte Service Bus maken met het onderwerp en abonnementen](service-bus-resource-manager-namespace-topic.md)
>-    [Een Service Bus naamruimte maken](service-bus-resource-manager-namespace.md)
>
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Azure Quickstart sjablonen][] en het zoeken naar "Service Bus."

## <a name="what-will-you-deploy"></a>Wat wordt u implementeren?

Met deze sjabloon implementeert u een regel van de Service Bus autorisatie voor een naamruimte en SMS entiteit (in dit geval een wachtrij).

Deze sjabloon worden [Gedeeld Access handtekening (SA's)](service-bus-sas-overview.md) voor verificatie. SA's kan toepassingen om te verifiëren met een toegangstoets die is geconfigureerd op de naamruimte of op de SMS-berichten entity (wachtrij of onderwerp) waaraan specifieke rechten zijn gekoppeld bus voor Service. U kunt deze toets vervolgens gebruiken om een token SA's die clients beurtelings gebruiken kunnen om te verifiëren met Service Bus te genereren.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

De naam van de autorisatieregel voor de naamruimte.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

De naam van de wachtrij in de naamruimte Service Bus.

```
"serviceBusQueueName": {
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

Hiermee maakt u een standaard-Service Bus naamruimte van het type **Messaging**en een regel van de Service Bus autorisatie voor naamruimte en entiteit.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen

Nu u hebt gemaakt en geïmplementeerd bronnen resourcemanager Azure gebruikt, lees hoe u deze resources beheren door te bekijken van deze artikelen:

- [Service Bus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
- [Service Bus resources met de Service Bus Explorer beheren](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Service Bus verificatie en machtiging](service-bus-authentication-and-authorization.md)

  [Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
  [Azure Quickstart sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus auth regelsjabloon]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
