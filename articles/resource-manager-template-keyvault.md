<properties
   pageTitle="Resourcemanager sjabloon voor belangrijke kluis | Microsoft Azure"
   description="Ziet u het schema resourcemanager voor de implementatie van belangrijke kluizen tot en met een sjabloon."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Belangrijke kluis sjabloon schema

Hiermee maakt u een belangrijke kluis.

## <a name="schema-format"></a>Schema-indeling

Als wilt maken van een belangrijke kluis, voegt u het volgende schema naar de sectie bronnen van de sjabloon.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Waarden

De volgende tabellen worden de waarden die u nodig hebt om in te stellen in het schema beschreven.

| Naam | Waarde |
| ---- | ---- | 
| type | Opsommen<br />Vereist<br />**Microsoft.KeyVault/vaults**<br /><br />Het resourcetype om te maken. |
| apiVersion | Opsommen<br />Vereist<br />**2015-01-06** of **2014-12-19-preview**<br /><br />De API-versie te gebruiken voor het maken van de resource. | 
| naam | Tekenreeks<br />Vereist<br />Een naam die uniek is voor alle Azure.<br /><br />De naam van de belangrijkste kluis om te maken. Houd rekening met de functie [uniqueString](resource-group-template-functions.md#uniquestring) gebruiken met uw naamgevingsconventie te maken van een unieke naam, zoals in het onderstaande voorbeeld. |
| locatie | Tekenreeks<br />Vereist<br />Een geldige regio voor belangrijke kluizen. Om te bepalen geldige regio's, Zie [regio's die worden ondersteund](resource-manager-supported-services.md#supported-regions).<br /><br />Het gebied voor het hosten van de belangrijkste kluis. |
| Eigenschappen | Object<br />Vereist<br />[Eigenschappen van object](#properties)<br /><br />Een object dat aangeeft van het type key kluis maken. |
| resources | Matrix<br />Optionele<br />Waarden toegestaan: [toets kluis geheime resources](resource-manager-template-keyvault-secret.md)<br /><br />Onderliggende bronnen voor de belangrijkste kluis. |

<a id="properties" />
### <a name="properties-object"></a>Eigenschappen van object

| Naam | Waarde |
| ---- | ---- | 
| enabledForDeployment | Booleaanse waarde<br />Optionele<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u als de kluis voor VM of Service stof implementatie is ingeschakeld. |
| enabledForTemplateDeployment | Booleaanse waarde<br />Optionele<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u als de kluis is ingeschakeld voor gebruik in resourcemanager sjabloon implementaties. Zie voor meer informatie [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Booleaanse waarde<br />Optionele<br />**waar** of **Onwaar**<br /><br />Hiermee geeft u als de kluis voor volume-versleuteling is ingeschakeld. |
| tenantId | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De tenant-id voor het abonnement. Met de PowerShell-cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) of het **weergeven van azure-account** Azure CLI opdracht kunt u deze ophalen. |
| accessPolicies | Matrix<br />Vereist<br />[accessPolicies object](#accesspolicies)<br /><br />Een matrix van maximaal 16 objecten die de machtigingen voor de gebruiker of service principal opgeven. |
| SKU | Object<br />Vereist<br />[SKU object](#sku)<br /><br />De SKU voor de belangrijkste kluis. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies object

| Naam | Waarde |
| ---- | ---- | 
| tenantId | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De tenant-id van de Azure Active Directory-tenant met de **object-id** in dit beleid access |
| object-id | Tekenreeks<br />Vereist<br />**Globaal unieke id**<br /><br />De object-id van de Azure Active Directory-gebruiker of de service-principal die toegang heeft tot de kluis. U kunt de waarde ophalen van de [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) of de [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShell-cmdlets of de **azure ad-gebruiker** of **azure ad sp** Azure CLI-opdrachten. |
| machtigingen | Object<br />Vereist<br />[object met machtigingen](#permissions)<br /><br />De machtigingen op dit kluis het Active Directory-object. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions object

| Naam | Waarde |
| ---- | ---- | 
| toetsen | Matrix<br />Vereist<br />**alle**, **back-ups**, **maken**, **ontsleutelen**, **verwijderen**, **versleutelen**, **krijgen**, **importeren**, **lijst**, **herstellen**, **aanmelden**, **unwrapkey**, **bijwerken**, **verifiëren**, **wrapkey**<br /><br />De machtigingen op toetsen in deze kluis aan dit Active Directory-object. Deze waarde moet worden opgegeven als een matrix van een of meer toegestane waarden. |
| geheimen | Matrix<br />Vereist<br />** **alle**, **verwijderen**, **lijst**maken en **instellen** **<br /><br />De machtigingen op geheimen in deze kluis aan dit Active Directory-object. Deze waarde moet worden opgegeven als een matrix van een of meer toegestane waarden. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU object

| Naam | Waarde |
| ---- | ---- | 
| naam | Opsommen<br />Vereist<br />**standaard**of **premium** <br /><br />De servicelaag van KeyVault gebruiken.  Standaard ondersteunt geheimen en sleutels software is beveiligd.  Ondersteuning voor sleutels HSM-beveiliging toevoegen voor Premium. |
| familie | Opsommen<br />Vereist<br />**A** <br /><br />De familie sku gebruiken. |
 
    
## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld implementeert een belangrijke kluis en geheim.

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

## <a name="quickstart-templates"></a>QuickStart sjablonen

De volgende quickstart sjabloon implementeert een belangrijke kluis.

- [Belangrijke kluis maken](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Volgende stappen

- Zie [aan de slag met Azure toets kluis](./key-vault/key-vault-get-started.md)voor algemene informatie over de belangrijkste kluizen.
- Voor een voorbeeld van verwijst naar een belangrijke kluis geheim bij het distribueren van sjablonen, Zie [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).

