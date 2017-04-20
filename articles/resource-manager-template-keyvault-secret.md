<properties
   pageTitle="Resourcemanager sjabloon voor een geheim in een belangrijke kluis | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor de implementatie van belangrijke kluis geheimen tot en met een sjabloon."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Belangrijke kluis geheime sjabloon schema

Hiermee maakt u een geheim dat is opgeslagen in een belangrijke kluis. In dit resourcetype wordt vaak geïmplementeerd als een onderliggende resource van [belangrijke kluis](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Schema-indeling

Als wilt maken van een belangrijke kluis geheim, voegt u het volgende schema aan uw sjabloon. Het geheim kan worden gedefinieerd als een onderliggende resource van een belangrijke kluis of op het hoogste niveau resource. U kunt deze definiëren als een resource onderliggende wanneer de belangrijkste kluis wordt geïmplementeerd in dezelfde sjabloon. U moet het geheim definiëren als op het hoogste niveau resource wanneer de belangrijkste kluis is niet geïmplementeerd in dezelfde sjabloon, of wanneer u moet meerdere geheimen maken in een lus van het resourcetype. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Waarde |
| ---- | ---- | 
| type | Opsommen<br />Vereist<br />**geheimen** (wanneer geïmplementeerd als een onderliggende resource van belangrijke kluis) of<br /> **Microsoft.KeyVault/vaults/secrets** (wanneer geïmplementeerd als een resource op het hoogste niveau)<br /><br />Het resourcetype om te maken. |
| apiVersion | Opsommen<br />Vereist<br />**2015-01-06** of **2014-12-19-preview**<br /><br />De API-versie te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Één woord wanneer geïmplementeerd als een onderliggende resource van een belangrijke kluis of in de indeling **{sleutel-kluis-naam} / {geheim-naam}** wanneer geïmplementeerd als op het hoogste niveau resource moet worden toegevoegd aan een bestaande belangrijke kluis.<br /><br />De naam van het geheim om te maken. |
| Eigenschappen | Object<br />Vereist<br />[Eigenschappen van object](#properties)<br /><br />Een object dat bevat de waarde van het geheim om te maken. |
| dependsOn | Matrix<br />Optionele<br />Een lijst door komma's gescheiden met een resource namen of unieke id's voor een resource.<br /><br />De verzameling afhankelijk van de koppeling deze resources. Als de sleutel kluis voor het geheim wordt geïmplementeerd in dezelfde sjabloon, moet u de naam van de belangrijkste kluis bevatten in met dit element om ervoor te zorgen dat deze eerst wordt geïmplementeerd. |

<a id="properties" />
### <a name="properties-object"></a>Eigenschappen van object

| Naam | Waarde |
| ---- | ---- | 
| waarde | Tekenreeks<br />Vereist<br /><br />De geheime waarde op te slaan in de belangrijkste kluis. Wanneer het doorgeven van een waarde in die voor deze eigenschap, gebruikt u een parameter van het type **securestring**.  |

    
## <a name="examples"></a>Voorbeelden

Het eerste voorbeeld implementeert een geheim als een onderliggende resource van een belangrijke kluis.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Het tweede voorbeeld implementeert het geheim als op het hoogste niveau resource die is opgeslagen in een bestaande belangrijke kluis.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Volgende stappen

- Zie [aan de slag met Azure toets kluis](./key-vault/key-vault-get-started.md)voor algemene informatie over de belangrijkste kluizen.
- Voor een voorbeeld van verwijst naar een belangrijke kluis geheim bij het distribueren van sjablonen, Zie [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).


