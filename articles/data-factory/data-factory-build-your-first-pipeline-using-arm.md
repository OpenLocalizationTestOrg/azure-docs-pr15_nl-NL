<properties
    pageTitle="Uw eerste gegevens factory (resourcemanager sjabloon) bouwen | Microsoft Azure"
    description="In deze zelfstudie kunt u een voorbeeld Azure gegevens Factory pijplijn met een sjabloon van Azure resourcemanager maken."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Zelfstudie: Uw eerste Azure gegevens factory met Azure resourcemanager-sjabloon maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel kunt u een sjabloon resourcemanager Azure gebruikt om te maken van uw eerste Azure gegevens factory.

## <a name="prerequisites"></a>Vereisten voor
- [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) artikel en voer de **vereiste** stappen uit.
- Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) -artikel naar de nieuwste versie van Azure PowerShell installeren op uw computer.
- Zie [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md) voor meer informatie over resourcemanager Azure-sjablonen. 

## <a name="in-this-tutorial"></a>In deze zelfstudie
Entiteit | Beschrijving  
------ | ----------- 
Azure gekoppeld opslagservice | Uw opslag van Azure-account bevat koppelingen naar de fabriek gegevens. De opslag van Azure-account bevat de invoer- en uitvoerbereik gegevens voor de pijplijn in dit voorbeeld. 
Gekoppelde HDInsight op aanvraag-service| Koppelingen een HDInsight op aanvraag cluster de fabriek gegevens. Het cluster om gegevens te verwerken automatisch voor u wordt gemaakt en wordt verwijderd wanneer de verwerking klaar is.
Azure Blob invoer gegevensset | Verwijst naar de service Azure-opslag die zijn gekoppeld. De gekoppelde service verwijst naar een Azure Storage-account en de gegevensset Azure Blob geeft de container, de map en de bestandsnaam in de opslagruimte waarin de invoergegevens. 
Azure Blob uitvoer gegevensset | Verwijst naar de service Azure-opslag die zijn gekoppeld. De gekoppelde service verwijst naar een Azure Storage-account en de gegevensset Azure Blob geeft de container, de map en de bestandsnaam in de opslagruimte waarin de uitvoergegevens. 
Gegevens verkooppijplijn | De pijplijn beschikt over een activiteit van het type HDInsightHive verbruikt van de invoer gegevensset en de gegevensset uitvoer oplevert.   

Een factory gegevens kan een of meer pijpleidingen bevatten. Een pijplijn kan een of meer activiteiten in deze hebben. Er zijn twee soorten activiteiten: [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) en [gegevens-transformatie activiteiten](data-factory-data-transformation-activities.md). In deze zelfstudie kunt u een pijplijn maken met een activiteit (kopiëren).

De volgende sectie bevat de volledige resourcemanager-sjabloon voor het definiëren van gegevens Factory entiteiten, zodat u deze snel kunt via de zelfstudie uitvoeren en testen van de sjabloon. Als u wilt weten hoe elke Data Factory-entiteit is gedefinieerd, Zie de sectie [gegevens Factory entiteiten in de sjabloon](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Gegevens Factory JSON-sjabloon
De hoogste niveau resourcemanager-sjabloon voor het definiëren van een fabriek gegevens luidt als volgt: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Maak een JSON-bestand met de naam **ADFTutorialARM.json** in **C:\ADFGetStarted** -map met de volgende inhoud:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    }
                }
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] U kunt een ander voorbeeld van resourcemanager sjabloon vinden voor het maken van een fabriek Azure-gegevens op [Zelfstudie: een pijplijn maken met kopie activiteit met een sjabloon van Azure resourcemanager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parameters JSON 
Een JSON-bestand met de naam **ADFTutorialARM-Parameters.json** waarin de parameters voor de resourcemanager Azure-sjabloon maken.  

> [AZURE.IMPORTANT] Geef de naam en de toets van uw account Azure-opslag voor de parameters **storageAccountName** en **storageAccountKey** in dit parameterbestand. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] U moet mogelijk afzonderlijke parameter JSON-bestanden voor de ontwikkeling, testen en productieomgevingen die u met dezelfde gegevens Factory JSON sjabloon gebruiken kunt. Met behulp van een Power-shellscript, kunt u de gegevens Factory entiteiten in deze omgevingen automatiseren. 

## <a name="create-data-factory"></a>Gegevens factory maken

1. **Azure PowerShell** starten en uitvoeren van de volgende opdracht uit: 
    - Uitvoeren `Login-AzureRmAccount` en voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij de portal van Azure.  
    - Uitvoeren `Get-AzureRmSubscription` om weer te geven van alle abonnementen voor dit account.
    - Uitvoeren `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` om te selecteren van het abonnement waaraan u wilt werken. Dit abonnement moet hetzelfde als de relatie die u in de portal van Azure gebruikt.
1. Voer de volgende opdracht om te implementeren Data Factory entiteiten met de sjabloon resourcemanager dat u in stap 1 hebt gemaakt. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor verkooppijplijn
 
1.  Na het aanmelden bij de [portal van Azure](https://portal.azure.com/), klik op **Bladeren** en selecteer **gegevens factory's**.
        ![Bladeren -> gegevens factory 's](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Klik in het blad **Gegevens factory's** op de gegevens fabriek (**TutorialFactoryARM**) die u hebt gemaakt.   
2.  Klik in het blad **Gegevens Factory** voor uw gegevens factory, op **Diagram**.
        ![Diagram-tegel](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  In de **Diagramweergave te klikken**ziet u een overzicht van de pijpleidingen en gegevenssets in deze zelfstudie gebruikt.
    
    ![Diagramweergave](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Dubbelklik in de diagramweergave te klikken, op de gegevensset **AzureBlobOutput**. U ziet dat het segment die momenteel wordt verwerkt.

    ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Wanneer verwerking klaar is, ziet u het segment in **Gereed** . Maken van een op aanvraag HDInsight cluster duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijplijn te nemen **ongeveer 30 minuten** verwerkingstijd van het segment.

    ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Wanneer het segment in **Gereed** is, controleert u of de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  

Zie [Monitor gegevenssets en pijplijn](data-factory-monitor-manage-pipelines.md) voor instructies over het gebruik van de Azure portal bladen de verkooppijplijn en gegevenssets die u in deze zelfstudie hebt gemaakt.

U kunt ook controleren en beheren-App gebruiken om uw gegevens pijpleidingen de te houden. Zie [beeldscherm en beheren van Azure gegevens Factory pijpleidingen met App Monitoring](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van de toepassing. 

> [AZURE.IMPORTANT] De invoer bestand wordt verwijderd wanneer het segment wordt verwerkt. Daarom desgewenst kunt u het segment opnieuw uit te voeren of de zelfstudie opnieuw doen het invoer bestand uploaden (input.log) naar de map inputdata van de container adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Gegevens Factory entiteiten in de sjabloon
### <a name="define-data-factory"></a>Gegevens factory definiëren
U definieert een factory gegevens in de sjabloon resourcemanager, zoals wordt weergegeven in het onderstaande voorbeeld:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

De dataFactoryName luidt als volgt: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Dit is een unieke tekenreeks op basis van de resource-ID.  

### <a name="defining-data-factory-entities"></a>Gegevens Factory entiteiten definiëren
De volgende gegevens Factory-entiteiten zijn gedefinieerd in de JSON-sjabloon: 

- [Azure gekoppeld opslagservice](#azure-storage-linked-service)
- [Gekoppelde HDInsight op aanvraag-service](#hdinsight-on-demand-linked-service)
- [Azure blob invoer gegevensset](#azure-blob-input-dataset)
- [Azure blob uitvoer gegevensset](#azure-blob-output-dataset)
- [Gegevens verkooppijplijn met een activiteit kopiëren](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice
U opgeven de naam en de toets van uw account Azure opslag in deze sectie. Zie [Azure Storage gekoppeld service](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een service van Azure-opslag die zijn gekoppeld. 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

De **connectionString** gebruikmaakt van de parameters storageAccountName en storageAccountKey. De waarden voor deze parameters doorgegeven met behulp van een configuratiebestand. De definitie wordt ook gebruikt voor variabelen: azureStroageLinkedService en dataFactoryName gedefinieerd in de sjabloon. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>Gekoppelde HDInsight op aanvraag-service
Zie [gekoppelde services berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikel voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een gekoppelde HDInsight op aanvraag-service.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Houd rekening met de volgende punten: 

- De gegevens fabriek wordt een HDInsight **op basis van Windows** cluster voor u gemaakt met de bovenstaande JSON. U kunt ook het maken van een HDInsight **Linux gebaseerde** cluster hebben. Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
- U kunt **uw eigen HDInsight cluster** in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
- Het cluster HDInsight Hiermee maakt u een **standaard-container** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight worden deze container niet verwijderd wanneer het cluster wordt verwijderd. Dit probleem is inherent aan het ontwerp. Met op aanvraag HDInsight gekoppeld-service, een HDInsight cluster wordt gemaakt telkens wanneer een segment worden verwerkt moet, tenzij er een bestaande is live cluster (**timeToLive**) en wordt verwijderd wanneer de verwerking is voltooid.

    Als u meer segmenten worden verwerkt, ziet u veel containers in uw Azure-blobopslag. Als u niet nodig ze hebt voor het oplossen van problemen met de taken, wilt u mogelijk verwijdert u deze als u wilt doen om de opslag te beperken. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Gebruik hulpprogramma's zoals [Microsoft opslag Explorer](http://storageexplorer.com/) containers in uw Azure-blobopslag verwijderen.

Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.



#### <a name="azure-blob-input-dataset"></a>Azure blob invoer gegevensset
U opgeven de namen van blob container, mappen en het bestand met de ingevoerde gegevens. Zie [Azure Blob gegevensset eigenschappen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een gegevensset Azure Blob. 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Deze definitie gebruikmaakt van de volgende parameters die zijn gedefinieerd in de parameter sjabloon: blobContainer, inputBlobFolder, en inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure Blob uitvoer gegevensset
U de namen van de container blob en een map waarin de uitvoergegevens. Zie [Azure Blob gegevensset eigenschappen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een gegevensset Azure Blob.  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Deze definitie gebruikmaakt van de volgende parameters die zijn gedefinieerd in de sjabloon parameter: blobContainer en outputBlobFolder. 

#### <a name="data-pipeline"></a>Gegevens verkooppijplijn
U definiëren een pijplijn omzetten van gegevens door Component script uitvoeren op een op aanvraag Azure HDInsight cluster. Zie [Verkooppijplijn JSON](data-factory-create-pipelines.md#pipeline-json) voor beschrijvingen van de JSON-elementen gebruikt om te definiëren van een pijplijn in dit voorbeeld. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>De sjabloon opnieuw gebruiken 
In deze zelfstudie, moet u een sjabloon voor het definiëren van gegevens Factory entiteiten en een sjabloon voor het doorgeven van waarden voor parameters gemaakt. Als u dezelfde sjabloon wilt implementeren Data Factory entiteiten in verschillende omgevingen, moet u een parameterbestand voor elke omgeving maken en gebruik deze methode als u de implementatie in die omgeving.     

Voorbeeld:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Zoals u ziet dat de eerste opdracht parameterbestand voor de ontwikkelomgeving, tweede één voor de testomgeving en de derde één voor de productieomgeving gebruikt.  

U kunt ook opnieuw gebruiken voor de sjabloon als herhaalde taken wilt uitvoeren. Bijvoorbeeld, moet u veel gegevens factory's maken met een of meer pijpleidingen die dezelfde logica implementeren, maar elke factory gegevens maakt gebruik van verschillende Azure opslag en Azure SQL Database-accounts. In dit scenario gebruikt u dezelfde sjabloon in dezelfde omgeving (ontwikkelaar, test of productie) met verschillende parameterbestanden gegevens factory's maken. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Resourcemanager sjabloon voor het maken van een gateway
Hier ziet een Resource Manager voorbeeldsjabloon voor het maken van een logische gateway in de achtergrond. Een gateway installeren op uw lokale computer of de Azure IaaS VM en de gateway hebt geregistreerd met behulp van een sleutel Data Factory-service. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie.

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Deze sjabloon maakt u een gegevens factory met GatewayUsingArmDF met een gateway met de naam de naam: GatewayUsingARM. 

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) | In dit artikel vindt u een lijst met gegevens transformatie activiteiten (zoals HDInsight component transformatie u in deze zelfstudie gebruikt) worden ondersteund door Factory van Azure-gegevens. |
| [Plannings- en kan worden uitgevoerd](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de plannings- en execution aspecten van Azure gegevens Factory-toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel vindt u meer informatie over pijpleidingen en activiteiten in Factory van Azure-gegevens en hoe ze worden gebruikt om samen te stellen end-to-end gegevensgestuurde werkstromen voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) | Dit artikel vindt u meer informatie over gegevenssets in fabriek van Azure-gegevens.
| [Bewaken en pijpleidingen met Monitoring App beheren](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in pijpleidingen met behulp van de controle Management-App. 

  

