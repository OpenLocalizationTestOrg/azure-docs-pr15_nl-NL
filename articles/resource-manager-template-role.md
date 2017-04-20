<properties
   pageTitle="Resourcemanager sjabloon voor roltoewijzingen | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor de implementatie van een roltoewijzing tot en met een sjabloon."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Rol toewijzingen sjabloon schema

Een gebruiker, groep of service principal wordt toegewezen aan een rol op een opgegeven bereik.

## <a name="resource-format"></a>Resource-indeling

Als wilt maken van een roltoewijzing, voegt u het volgende schema naar de sectie bronnen van de sjabloon.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| type | Ja    | Het resourcetype om te maken.<br /><br /> Voor resourcegroep:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Voor resource:<br />**{provider-naamruimte} / {type resource} / providers/roleAssignments** |
| apiVersion |Ja | De API-versie te gebruiken voor het maken van de resource.<br /><br /> Gebruik **2015-07-01**. | 
| naam | Ja | Een globaal unieke id voor de functietoewijzing voor de nieuwe. |
| dependsOn | Nee | Een matrix door komma's gescheiden van een resource namen of unieke id's voor een resource.<br /><br />De verzameling resources die zijn afhankelijk van de roltoewijzing van deze. Als het toewijzen van een rol die beperkt tot een resource en zodat de resource wordt geïmplementeerd in dezelfde sjabloon, moet u de naam van deze resource opnemen in dit element kunt u zorgen dat de resource eerst wordt geïmplementeerd. |
| Eigenschappen | Ja | Het object met eigenschappen waarmee de roldefinitie, hoofdsom en bereik. |

### <a name="properties-object"></a>Eigenschappen van object

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| roleDefinitionId | Ja |  De id van de roldefinitie van een bestaande in de roltoewijzing moet worden gebruikt.<br /><br /> Gebruik de volgende indeling:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ja | De globaal unieke id voor een bestaande hoofdsom. Deze waarde wordt toegewezen aan de-id in de map en kunnen verwijzen naar een gebruiker, service principal of beveiligingsgroep. |
| bereik | Nee | Het bereik waarop deze roltoewijzing is van toepassing op.<br /><br />Gebruik voor resourcegroepen:<br />**/resourceGroups/ /Subscriptions/ {abonnements-id} {groep-bronnaam}**  <br /><br />Voor resources, gebruiken:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Het gebruik van de resource van de toewijzing rol

U kunt een roltoewijzing aan uw sjabloon toevoegen wanneer u moet een gebruiker, groep of service principal toevoegen aan een rol tijdens de implementatie. Roltoewijzingen worden overgenomen van hogere niveaus van reikwijdte, dus als u een principal al hebt toegevoegd aan een rol op het niveau van abonnement, u niet hoeft aan de resourcegroep of resources toewijzen.

Zijn er veel id-waarden die u nodig hebt bij het werken met roltoewijzingen. U kunt de waarden via PowerShell of Azure CLI ophalen.

### <a name="powershell"></a>PowerShell

De naam van roltoewijzing moet een globaal unieke id. U kunt een nieuwe id voor de **naam** met genereren:

    $name = [System.Guid]::NewGuid().toString()

U kunt de id voor de roldefinitie van de met ophalen:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

U kunt de id ophalen voor de hoofdsom met een van de volgende opdrachten.

Voor een groep met de naam van de **revisoren**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Voor een gebruiker met de naam **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Voor een service benoemde hoofdsom **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

U kunt de id voor de roldefinitie van de met ophalen:

    azure role show Reader --json | jq .[].Id -r

U kunt de id ophalen voor de hoofdsom met een van de volgende opdrachten.

Voor een groep met de naam van de **revisoren**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Voor een gebruiker met de naam **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Voor een service benoemde hoofdsom **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Voorbeelden

De volgende sjabloon ontvangt een id voor een rol en een id voor een gebruiker, groep of service principal. De rol op het niveau van de groep resource wilt toewijzen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



De volgende sjabloon wordt gemaakt van een opslag-account en wordt de functie Lezer toegewezen aan het account opslag. De id's voor twee groepen en de rol van de lezer zijn opgenomen in de sjabloon om te vereenvoudigen implementatie. Deze waarden kunnen worden opgehaald voor implementatie via script en doorgegeven als parameters.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>QuickStart sjablonen

De volgende sjablonen weergeven het gebruik van de rol toewijzing resource:

- [Ingebouwde rol toewijzen aan de resourcegroep](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Ingebouwde rol toewijzen aan bestaande VM](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Ingebouwde rol toewijzen aan meerdere bestaande VMs](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over de sjabloonstructuur van de, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over Rolgebaseerd toegangsbeheer [Toegangsbeheer op basis van Azure Active Directory-rol](./active-directory/role-based-access-control-configure.md).
