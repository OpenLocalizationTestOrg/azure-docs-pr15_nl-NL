<properties
    pageTitle="Automatisch inschakelen diagnostische instellingen met behulp van een sjabloon resourcemanager | Microsoft Azure"
    description="Informatie over het gebruik van een sjabloon resourcemanager diagnostische instellingen waarmee u kunt de diagnostische logboeken op gebeurtenis Hubs streamen of op te slaan in een opslag-account maken."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Diagnostische instellingen automatisch bij resource maken met een sjabloon resourcemanager inschakelen
In dit artikel ziet hoe u kunt een [sjabloon van Azure resourcemanager](../resource-group-authoring-templates.md) diagnostische instellingen configureren op een resource wanneer deze is gemaakt. Hiermee kunt u automatisch starten streaming uw diagnostische logboeken en maatstelsel gebeurtenis Hubs, archiveren in een opslag-Account of naar Log Analytics verzenden wanneer een resource wordt gemaakt.

De methode voor het inschakelen van de diagnostische logboeken met behulp van een sjabloon resourcemanager, is afhankelijk van het resourcetype.

- **Niet-berekeningscluster** resources (bijvoorbeeld netwerk beveiligingsgroepen kan logica Apps, automatisering) Gebruik [Diagnostische instellingen in dit artikel wordt beschreven](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Berekenen** Resources (af/LAD gebaseerde) Gebruik het [af/LAD configuratiebestand in dit artikel beschreven](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

In dit artikel worden beschreven hoe u diagnostische gegevens met een methode configureert.

Dit zijn de basisstappen:

1. Een sjabloon als een JSON-bestand dat wordt beschreven hoe u de resource maken en inschakelen van diagnostische gegevens maken.
2. [De sjabloon met behulp van een implementatiemethode Deploy](../resource-group-template-deploy.md).

We geven hieronder een voorbeeld van het sjabloonbestand JSON dat u nodig hebt om te genereren voor niet-berekeningscluster en berekeningscluster resources.

## <a name="non-compute-resource-template"></a>Niet-berekeningscluster resource-sjabloon
Voor niet-berekeningscluster resources moet u twee dingen doen:

1. Parameters toevoegen aan de blob parameters voor het opslagaccountnaam, service bus regel-ID en/of OMS Log Analytics werkruimte-ID (waardoor archiveren van diagnostische logboeken in een opslag-account, streaming van Logboeken op gebeurtenis Hubs en/of Logboeken verzenden naar een logboek Analytics).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. In de matrix resources van de resource waarvoor u wilt inschakelen diagnostische logboeken, een resource van het type toevoegen `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

De blob eigenschappen voor de diagnostische instelling volgt [de opmaak in dit artikel wordt beschreven](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Hier volgt een volledige voorbeeld die Hiermee maakt u een beveiligingsgroep netwerk en Hiermee schakelt u streaming gebeurtenis Hubs en opslagruimte in een opslag-account.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Resource-sjabloon berekenen
Als u wilt inschakelen diagnostische hulpprogramma's voor een berekeningscluster bron, bijvoorbeeld een virtuele Machine of Service stof cluster, moet u:

1. De uitbreiding Azure diagnostische hulpprogramma's voor de definitie van de resource VM toevoegen.
2. Geef een hub voor opslag-account en/of gebeurtenis als een parameter.
3. De inhoud van uw WADCfg XML-bestand in de eigenschap XMLCfg, alle XML-tekens correct ontsnappen toevoegen.

> [AZURE.WARNING] Deze laatste stap kan direct lastig zijn om. [Zie dit artikel](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) voor een voorbeeld van het Schema van de configuratie diagnostische gegevens worden gesplitst in variabelen die worden voorafgegaan en correct opgemaakt.

Het hele proces, steekproeven, zoals is beschreven [in dit document](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Volgende stappen
- [Lees meer over Azure diagnostische logboeken](./monitoring-overview-of-diagnostic-logs.md)
- [Azure diagnostische logboeken op gebeurtenis Hubs streamen](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
