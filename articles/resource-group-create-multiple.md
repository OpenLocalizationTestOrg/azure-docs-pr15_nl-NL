<properties
   pageTitle="Meerdere exemplaren van Resources implementeren | Microsoft Azure"
   description="Kopiëren en matrices gebruiken in een sjabloon Azure resourcemanager om meerdere keren bij het implementeren van resources."
   services="azure-resource-manager"
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
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Meerdere exemplaren van resources in Azure resourcemanager maken

In dit onderwerp ziet u hoe u in uw sjabloon Azure resourcemanager maken van meerdere exemplaren van een resource.

## <a name="copy-copyindex-and-length"></a>kopiëren, copyIndex en lengte

Binnen de resource kan meerdere keren maken, kunt u een object **kopiëren** waarmee wordt opgegeven hoe vaak u definiëren. De kopie heeft de volgende indeling:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

U kunt de huidige waarde iteratie met de functie **copyIndex()** kunt openen, zoals hieronder wordt weergegeven in de functie samenvoegen.

    [concat('examplecopy-', copyIndex())]

Wanneer u meerdere resources maakt van een matrix van waarden, kunt u de functie **lengte** kunt u het aantal opgeven. U vindt u de matrix als de parameter voor de functie lengte.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Gebruik van de indexwaarde van de in het vak naam

U kunt de kopie bewerking meerdere exemplaren van een resource die uniek zijn benoemde op basis van de volgnummers index maken. U wilt bijvoorbeeld een uniek nummer toevoegen aan het einde van de naam van elke resource die wordt geïmplementeerd. Om te implementeren drie websites met de naam:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

De volgende sjabloon gebruikt:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Verschuiving indexwaarde

U ziet in het vorige voorbeeld die de indexwaarde van nul naar 2 gaat. Als u wilt verschuiven de indexwaarde, kunt u een waarde in de functie **copyIndex()** , zoals **copyIndex(1)**doorgeven. Het aantal iteraties om uit te voeren nog steeds is opgegeven in het element kopie, maar de waarde van copyIndex is verschoven met de opgegeven waarde. Zo is, gebruikt dezelfde sjabloon als het vorige voorbeeld, maar **copyIndex(1)** precisie zou drie websites met de naam implementeren:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Kopiëren gebruiken met matrix
   
De kopieeropdracht is met name handig tijdens het werken met matrices, omdat u elk element in de matrix kunt doorlopen. Om te implementeren drie websites met de naam:

- examplecopy-Contoso
- examplecopy-Fabrikam
- examplecopy-Coho

De volgende sjabloon gebruikt:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Uiteraard kunt instellen u het aantal op een andere waarde dan de lengte van de matrix. U kunt bijvoorbeeld maken van een matrix met veel waarden en vervolgens doorgeven in een parameterwaarde die aangeeft hoeveel matrixelementen om te implementeren. In dat geval stelt u de kopie telling zoals weergegeven in het eerste voorbeeld. 

## <a name="depending-on-resources-in-a-loop"></a>Afhankelijk van de resources in een lus

U kunt opgeven dat een resource worden geïmplementeerd na een andere resource met behulp van het element **dependsOn** . Wanneer u een resource die afhankelijk van de siteverzameling van resources in een lus is implementeren moet, kunt u de naam van de lus kopiëren in het element **dependsOn** . Het volgende voorbeeld wordt getoond hoe 3 opslag accounts implementeren voordat de virtuele Machine wordt geïmplementeerd. De volledige VM definitie wordt niet weergegeven. Zoals u ziet het element kopie heeft ingesteld op **storagecopy** de **naam** en het element **dependsOn** voor de virtuele Machines ook is ingesteld op **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Klik op een geneste resource herhalen

U kunt een lus kopie niet gebruiken voor een geneste resource. Als u meerdere exemplaren van een resource die u meestal als genest in een andere resource instelt maken moet, moet u in plaats daarvan de resource als op het hoogste niveau resource maken, en de relatie met de bovenliggende resource tot en met de eigenschappen van het **type** en de **naam** definiëren.

Stel dat u meestal een gegevensset instelt als een geneste resource binnen een fabriek gegevens.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Als u wilt maken van meerdere exemplaren van gegevenssets, moet u een sjabloon wijzigen, zoals hieronder wordt weergegeven. Kennisgeving het type volledig gekwalificeerde en de naam de naam van de factory gegevens bevat.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Meerdere exemplaren maken wanneer kopiëren werkt niet

U kunt alleen **kopiëren** op resourcetypen en niet op eigenschappen in een resourcetype. Dit kan problemen maken voor u, als u wilt maken van meerdere exemplaren van iets dat deel uitmaakt van een resource. Een gebruikelijk is het opzetten van meerdere gegevensschijven voor een virtuele Machine. U kunt **kopiëren** met de gegevensschijven niet gebruiken omdat **dataDisks** een eigenschap op de virtuele Machine, niet een eigen resourcetype is. U kunt in plaats daarvan een matrix maken met zo veel gegevensschijven terwijl u nodig hebt en in het werkelijke aantal gegevensschijven doorgeven maken. In de definitie VM gebruikt u de functie **uitvoeren** om alleen het aantal elementen die u wel wilt bewaren van de matrix.

Een volledige voorbeeld van dit patroon is weergeven in de sjabloon voor het [maken van een VM met een dynamische selectie van gegevensschijven](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Hieronder ziet u de betreffende secties van de implementatiesjabloon. Veel van de sjabloon is verwijderd als u wilt markeren in de secties die nodig zijn voor het maken van een aantal gegevensschijven dynamisch. U ziet u de parameter **numDataDisks** waarmee u kunt het doorgeven van het aantal schijven maken. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Volgende stappen
- Als u meer informatie over de secties van een sjabloon wilt, raadpleegt u [Authoring Azure resourcemanager sjablonen](./resource-group-authoring-templates.md).
- U kunt gebruiken in een sjabloon, Zie [Azure resourcemanager sjabloon functies](./resource-group-template-functions.md)voor alle functies.
- Als u wilt weten hoe u het implementeren van de sjabloon, raadpleegt u [een toepassing met Azure resourcemanager sjabloon Deploy](resource-group-template-deploy.md).
