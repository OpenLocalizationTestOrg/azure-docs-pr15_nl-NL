<properties
   pageTitle="Resourcemanager sjabloon voor het koppelen van resources | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor het implementeren van koppelingen tussen verwante resources tot en met een sjabloon."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Schema voor resource koppelingen

Hiermee maakt u een koppeling tussen twee resources. De koppeling wordt toegepast op een resource bekend als de bron-resource. De tweede bron in de koppeling wordt genoemd in de doel-resource.

## <a name="schema-format"></a>Schema-indeling

Toevoegen als u wilt een koppeling maakt, het volgende schema naar de sectie bronnen van de sjabloon.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Waarde |
| ---- | ---- |
| type | Opsommen<br />Vereist<br />**{naamruimte} / {Typ} / providers/koppelingen**<br /><br />Het resourcetype om te maken. De {naamruimte} en {type} waarden verwijzen naar de provider naamruimte en resource het type van de resource bron. |
| apiVersion | Opsommen<br />Vereist<br />**01-01-2015**<br /><br />De API-versie te gebruiken voor het maken van de resource. |  
| naam | Tekenreeks<br />Vereist<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> maximaal 64 tekens, en geen <>, %, &,?, of een stuurcodes.<br /><br />A waarde ter aanduiding van de naam van de resource bron zowel een naam voor de koppeling. |
| dependsOn | Matrix<br />Optionele<br />Een lijst door komma's gescheiden met een resource namen of unieke id's voor een resource.<br /><br />De verzameling afhankelijk van de koppeling deze resources. Als de bronnen die u koppelt zijn geïmplementeerd in dezelfde sjabloon, moet u deze resourcenamen opnemen in met dit element om ervoor te zorgen dat ze eerst worden geïmplementeerd. | 
| Eigenschappen | Object<br />Vereist<br />[Eigenschappen van object](#properties)<br /><br />Een object dat aangeeft van de resource om aan te koppelen, en opmerkingen over de koppeling. |  

<a id="properties" />
### <a name="properties-object"></a>Eigenschappen van object

| Naam | Waarde |
| ------- | ---- |
| targetId | Tekenreeks<br />Vereist<br />**{resource-id}**<br /><br />De id van de doelsite resource koppelen aan. |
| notities | Tekenreeks<br />Optionele<br />512 tekens<br /><br />Beschrijving van de vergrendeling. |


## <a name="how-to-use-the-link-resource"></a>Het gebruik van de resource koppeling

U toepassen een koppeling tussen twee resources wanneer de resources hebben een afhankelijkheid die nog steeds na implementatie. Een app kan bijvoorbeeld verbinding maken met een database in een andere resource-groep. U kunt deze afhankelijkheid definiëren door een koppeling vanuit de app in de database. Koppelingen kunt u de relatie tussen twee resources. Later, kunt u of iemand anders in uw organisatie een resource voor koppelingen om te ontdekken hoe de resource werkt met andere bronnen zoeken.

Alle gekoppelde resources moeten behoren tot hetzelfde abonnement. Elke resource kan worden gekoppeld aan 50 andere bronnen. Als een van de gekoppelde bronnen zijn verwijderd of verplaatst, moet de eigenaar van de koppeling de resterende koppeling opschonen.

Als u wilt werken met koppelingen door REST, Zie [Gekoppelde bronnen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Gebruik de volgende Azure PowerShell-opdracht om te zien van alle koppelingen in uw abonnement. Andere parameters om de resultaten te beperken, kunt u opgeven.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld wordt een alleen-lezen vergrendelen met een web-app.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>QuickStart sjablonen

De volgende quickstart sjablonen implementeren resources met een koppeling.

- [Waarschuwen voor wachtrij met logica-app](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Waarschuwing marge met logica-app](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Een app API met een bestaande gateway inrichten](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Een app API met een nieuwe gateway inrichten](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Een App logica plus API-app met een sjabloon maken](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logica-app die een SMS-bericht stuurt wanneer er een melding wordt gestart](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de sjabloonstructuur van de, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
