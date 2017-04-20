<properties
    pageTitle="Een gebeurtenis Hubs naamruimte maken met de Hub van de gebeurtenis en inschakelen van archiveren met een sjabloon van Azure resourcemanager | Microsoft Azure"
    description="Maken van een gebeurtenis Hubs naamruimte met gebeurtenis Hub en archiveren met Azure resourcemanager sjabloon inschakelen"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Maken van een gebeurtenis Hubs naamruimte met gebeurtenis Hub en archiveren met een sjabloon van Azure resourcemanager inschakelen

In dit artikel leest hoe u een sjabloon van Azure resourcemanager die een gebeurtenis Hubs naamruimte maakt met een Hub voor de gebeurtenis en waarmee archief op uw Hub gebeurtenis gebruiken. U leert hoe bepalen welke resources zijn geïmplementeerd en het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt gebruik deze sjabloon voor uw eigen implementaties, of om te voldoen aan uw behoeften aanpassen

Zie voor meer informatie over het maken van sjablonen, [Azure resourcemanager Authoring sjablonen][].

Voor meer informatie over het oefenen en patronen op Azure Resources naamgevingsconventies, raadpleegt u [Azure Resources naamgevingsconventies voor bestanden][].

Zie de [gebeurtenis Hub en inschakelen archief sjabloon][] op GitHub voor de sjabloon voltooid.

>[AZURE.NOTE]
>Als u wilt controleren of er voor de meest recente sjablonen, gaat u naar de galerie met [Sjablonen van Azure Quickstart][] en zoek naar gebeurtenis Hubs.

## <a name="what-you-deploy"></a>Wat u wilt implementeren?

Met deze sjabloon, die u kunt implementeren van een gebeurtenis Hubs naamruimte met een Hub voor de gebeurtenis en archief inschakelen.

[Gebeurtenis Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) is een gebeurtenis service gebruikte geven gebeurtenis en telemetrielogboek ingress Azure op grote schaal met lage latentie en hoge betrouwbaarheid verwerken. Gebeurtenis Hubs archief kunt u voor het leveren van de streaming gegevens automatisch in uw Hubs gebeurtenis met Azure-blobopslag van uw keuze binnen een bepaald tijdsinterval of het interval van de grootte van uw keuze.

De implementatie automatisch wordt uitgevoerd, klikt u op de volgende knop:

[![Implementeren naar Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd.

De sjabloon bepaalt de volgende parameters.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

De naam van de gebeurtenis Hubs naamruimte maken.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

De naam van de gebeurtenis Hub gemaakt in het logboek Hubs naamruimte.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Het aantal dagen dat u wilt dat de berichten behouden in de Hub van de gebeurtenis. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Het aantal partities die u wilt dat in de Hub van de gebeurtenis.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Het archief op uw Hub gebeurtenis inschakelen.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

De codering-indeling die u opgeeft als u wilt de gegevens van de gebeurtenis.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Het tijdsinterval waarop het archief begint de gegevens in Azure-blobopslag archiveren.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

De grootte interval waarop het archief begint de gegevens in Azure-blobopslag archiveren.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Het archief vereist een opslagruimte Account resource-id archief naar de gewenste Azure-opslag inschakelen.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

De blob container waarin u de gegevens van de gebeurtenis wilt worden gearchiveerd.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

De API-versie van de sjabloon.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Bronnen die u wilt implementeren

Hiermee maakt u een naamruimte van het type **EventHubs**, met een Hub voor de gebeurtenis en kunt archief.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Opdrachten voor het uitvoeren van implementatie

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Een presentatie resourcemanager Azure-sjablonen]: ../resource-group-authoring-templates.md
[Azure Quickstart sjablonen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure Resources naamgevingsconventies]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Gebeurtenis-Hub en inschakelen archief sjabloon]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
