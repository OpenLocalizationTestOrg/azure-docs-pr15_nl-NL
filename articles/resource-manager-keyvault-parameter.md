<properties
   pageTitle="Toets kluis geheim met resourcemanager sjabloon | Microsoft Azure"
   description="Laat zien hoe een geheim uit een belangrijke kluis als parameter doorgeven tijdens de implementatie."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Beveiligde waarden doorgeven tijdens de implementatie

Wanneer u een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie moet, kunt u die waarde opslaan als een geheim in een [Azure toets kluis](./key-vault/key-vault-whatis.md) en verwijzen naar de waarde in een andere Resource Manager-sjablonen. U opnemen alleen een verwijzing naar het geheim in uw sjabloon, zodat het geheim nooit worden weergegeven en u niet hoeft te handmatig Voer de waarde in voor het geheim telkens wanneer die u de resources implementeren. U opgeven welke gebruikers of service principes toegang het geheim tot hebben.  

## <a name="deploy-a-key-vault-and-secret"></a>Een belangrijke kluis en geheim implementeren

Als u wilt maken van belangrijke kluis die kan worden verwezen van andere sjablonen resourcemanager, moet u de eigenschap **enabledForTemplateDeployment** instellen op **true**en moet u toegang verlenen aan de gebruiker of service hoofdsom die de implementatie die verwijst naar het geheim wordt uitgevoerd.

Zie voor meer informatie over het implementeren van een belangrijke kluis en geheim, [sleutel kluis schema](resource-manager-template-keyvault.md) en een [toets kluis geheime schema](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Verwijzen naar een geheim met statische-id

U verwijzen naar het geheim uit vanuit een parameterbestand waarin waarden worden doorgegeven aan de sjabloon. U verwijzen naar het geheim door door te geven van de resource-id van de belangrijkste kluis en de naam van het geheim. In dit voorbeeld het geheim belangrijke kluis moet al bestaan en u gebruikt een statische waarde voor deze resource-id.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Een hele parameterbestand uitzien als volgt:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

De parameter weer die het geheim accepteert moet een **securestring**. Het volgende voorbeeld ziet u de betreffende secties van een sjabloon die een SQL server die is vereist een beheerderswachtwoord implementeert.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Verwijzen naar een geheim met dynamische-id

De vorige sectie blijkt hoe u kunt een statische resource-id voor de belangrijkste kluis geheim doorgeven. In sommige gevallen moet u echter verwijzen naar een belangrijke kluis geheim dat varieert op basis van de huidige distributie. U kunt in dat geval niet harde-code de resource-id in het parameterbestand. Helaas kunt kan u dynamisch genereren de resource-id in het parameterbestand Sjabloonexpressies zijn niet toegestaan in het parameterbestand.

Als u wilt de resource-id voor een belangrijke kluis geheim dynamisch te genereren, moet u de resource die nog moet worden het geheim in een geneste sjabloon verplaatsen. In de basispagina sjabloon die u kunt de geneste sjabloon toevoegen en een parameter met de dynamisch gegenereerde resource-id opgeven.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Volgende stappen

- Zie [aan de slag met Azure toets kluis](./key-vault/key-vault-get-started.md)voor algemene informatie over de belangrijkste kluizen.
- Zie voor informatie over het gebruik van een belangrijke kluis met een virtuele Machine [Beveiligingsoverwegingen voor Azure resourcemanager](best-practices-resource-manager-security.md).
- Zie voor volledige voorbeelden van verwijst naar belangrijke geheimen [toets kluis voorbeelden](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

