<properties
    pageTitle="Service Bus resources met Azure resourcemanager sjablonen maken | Microsoft Azure"
    description="Azure resourcemanager sjablonen gebruiken om het maken van Service Bus resources automatiseren"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Service Bus resources met Azure resourcemanager sjablonen maken

In dit artikel wordt beschreven hoe maken en implementeren van Service Bus en gebeurtenis Hubs resources met Azure resourcemanager sjablonen, PowerShell en de Service Bus resource-provider.

Azure resourcemanager sjablonen kunt u de bronnen om te implementeren voor een oplossing en om op te geven parameters en variabelen waarmee u kunt de invoerwaarden voor verschillende omgevingen definiëren. De sjabloon bestaat uit JSON en expressies die u gebruiken kunt om samen te stellen van de waarden voor de implementatie. Zie [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md)voor gedetailleerde informatie over het schrijven van Azure resourcemanager sjablonen en een discussie van de sjabloonindeling. 

>[AZURE.NOTE] De voorbeelden in dit artikel wordt het gebruik van Azure resourcemanager om een Service Bus naamruimte en SMS entiteit (wachtrij) te maken. Voor andere voorbeelden sjabloon, gaat u naar de [galerie met Azure Quickstart sjablonen][] en het zoeken naar "Service Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Service Bus en gebeurtenis Hubs resourcemanager sjablonen

Deze Service Bus en gebeurtenis Hubs Azure resourcemanager sjablonen zijn beschikbaar voor downloaden en implementatie. Klik op de volgende koppelingen voor meer informatie over elke record met koppelingen naar de sjablonen op GitHub: 

- [Een Service Bus naamruimte maken](service-bus-resource-manager-namespace.md)
- [Een Service Bus naamruimte met wachtrij maken](service-bus-resource-manager-namespace-queue.md)
- [Een naamruimte Service Bus maken met het onderwerp en abonnementen](service-bus-resource-manager-namespace-topic.md)
- [Een Service Bus naamruimte met wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
- [Een gebeurtenis Hubs naamruimte maken met een gebeurtenis Hub en consumenten-groep](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

De volgende procedure wordt beschreven hoe PowerShell gebruiken om te implementeren van een sjabloon Azure resourcemanager die Hiermee maakt u **een laag Service Bus naamruimte** en een wachtrij binnen die naamruimte. In dit voorbeeld is gebaseerd op de sjabloon [maken een naamruimte Service Bus met wachtrij](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . De niet-geheel exacte werkstroom is als volgt:

1. PowerShell installeren.
2. De sjabloon en (optioneel) een parameterbestand maken.
2. In PowerShell, meld u aan bij uw Azure-account.
3. Maak een nieuwe resourcegroep als dit nog niet bestaat.
4. Test de implementatie.
5. Indien gewenst kunt u de implementatie-modus.
6. De sjabloon implementeren.

Zie [Deploy resources met Azure resourcemanager sjablonen][]voor volledige informatie over de implementatie van Azure resourcemanager sjablonen.

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell volgens de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)installeren.

### <a name="create-a-template"></a>Een sjabloon maken

Klonen of de sjabloon [201-servicebus-maken-wachtrij](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) van GitHub kopiëren:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Maak een parameterbestand (optioneel)

Als u wilt gebruiken een optionele parameters-bestand, kopieert u het bestand [201-servicebus-maken-wachtrij](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) . Vervang de waarde van `serviceBusNamespaceName` met de naam van de Service Bus naamruimte die u wilt maken in deze installatie en vervang de waarde van `serviceBusQueueName` met de naam van de wachtrij die u wilt maken. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Zie het onderwerp van de [Parameter-bestand](../resource-group-template-deploy.md#parameter-file) voor meer informatie.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Meld u aan bij Azure en stel het Azure abonnement

Voer de volgende opdracht uit een PowerShell-prompt:

```
Login-AzureRmAccount
```

U wordt gevraagd of u aan te melden bij uw Azure-account. Voer de volgende opdracht om weer te geven van de beschikbare abonnementen na aanmelding.

```
Get-AzureRMSubscription
```

Deze opdracht geeft als resultaat een lijst met beschikbare Azure abonnementen. Kies een abonnement voor de huidige sessie door de volgende opdracht uit te voeren. Vervang `<YourSubscriptionId>` met de GUID voor het Azure abonnement dat u wilt gebruiken.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>De resourcegroep instellen

Als u een bestaande resourcegroep niet hebt, kunt u een nieuwe resourcegroep maken met de opdracht **Nieuw AzureRmResourceGroup** . Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Als dit lukt, wordt een samenvatting van de nieuwe resourcegroep wordt weergegeven.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>De implementatie testen

Uw implementatie valideren door de `Test-AzureRmResourceGroupDeployment` cmdlet. Wanneer u test de implementatie, bieden u parameters exact in zoals u zou doen bij het uitvoeren van de implementatie.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>De implementatie maken

Als u wilt de nieuwe implementatie maken, uitvoeren de `New-AzureRmResourceGroupDeployment` opdracht en geef de benodigde parameters wanneer u wordt gevraagd. De parameters bevatten een naam voor de implementatie, de naam van uw resourcegroep en het pad of de URL naar het sjabloonbestand. Als de parameter **modus** niet opgeeft is, wordt de standaardwaarde van **incrementele** gebruikt. Zie [Stapsgewijze en volledige implementaties](../resource-group-template-deploy.md#incremental-and-complete-deployments)voor meer informatie.

De volgende opdracht vraagt u om de drie parameters in het venster PowerShell:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Gebruik de volgende opdracht uit als u wilt opgeven in plaats daarvan een parameterbestand.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

U kunt ook inline parameters gebruiken wanneer u de implementatie-cmdlet uitvoeren. De opdracht is als volgt:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Als u wilt uitvoeren op een [volledige](../resource-group-template-deploy.md#incremental-and-complete-deployments) implementatie, door de parameter **modus** in te stellen op **voltooid**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Controleer of de implementatie

Als de resources zijn geïmplementeerd, wordt een samenvatting van de implementatie weergegeven in het venster PowerShell:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Volgende stappen

U hebt nu de eenvoudige werkstroom en de opdrachten voor het implementeren van een sjabloon Azure resourcemanager zichtbaar. Ga naar de volgende koppelingen voor meer informatie:

- [Azure resourcemanager-overzicht][]
- [Resources met Azure resourcemanager sjablonen implementeren][]
- [Cocreatie van sjablonen](../resource-group-authoring-templates.md)


[Azure resourcemanager-overzicht]: ../resource-group-overview.md
[Resources met Azure resourcemanager sjablonen implementeren]: ../resource-group-template-deploy.md
[Galerie met Azure Quickstart-sjablonen]: https://azure.microsoft.com/documentation/templates/?term=service+bus