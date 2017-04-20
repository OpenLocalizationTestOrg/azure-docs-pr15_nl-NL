<properties
    pageTitle="Zelfstudie: Een pijplijn met resourcemanager-sjabloon maken | Microsoft Azure"
    description="In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van Azure resourcemanager sjabloon."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Zelfstudie: Een pijplijn met kopie activiteit met Azure resourcemanager-sjabloon maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure resourcemanager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Deze zelfstudie ziet u hoe u maken en een Azure gegevens factory met een sjabloon van Azure resourcemanager controleren. De pijplijn in de fabriek gegevens kopieert gegevens van Azure-blobopslag met Azure SQL-Database.

## <a name="prerequisites"></a>Vereisten voor
- Ga via [zelfstudie overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) en voer de **vereiste** stappen uit.
- Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) -artikel naar de nieuwste versie van Azure PowerShell installeren op uw computer. In deze zelfstudie kunt u PowerShell gebruiken om te implementeren Data Factory entiteiten. 
- (optioneel) Zie [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md) voor meer informatie over resourcemanager Azure-sjablonen.


## <a name="in-this-tutorial"></a>In deze zelfstudie

In deze zelfstudie kunt u een factory gegevens met de volgende gegevens Factory-entiteiten maken:

Entiteit | Beschrijving  
------ | ----------- 
Azure gekoppeld opslagservice | Uw opslag van Azure-account bevat koppelingen naar de fabriek gegevens. Azure opslag is de bron-gegevensopslag en Azure SQL-database is de sink gegevensopslag voor de activiteit kopiëren in de zelfstudie. Hiermee geeft u de opslagruimte rekening met de ingevoerde gegevens voor de activiteit kopiëren. 
Azure SQL-Database gekoppeld-service| Uw Azure SQL-database bevat koppelingen naar de fabriek gegevens. Hiermee geeft u de Azure SQL-database waarin de uitvoergegevens voor de activiteit kopiëren. 
Azure Blob invoer gegevensset | Verwijst naar de service Azure-opslag die zijn gekoppeld. De gekoppelde service verwijst naar een Azure Storage-account en de gegevensset Azure Blob geeft de container, de map en de bestandsnaam in de opslagruimte waarin de invoergegevens. 
Azure SQL-uitvoer gegevensset | Verwijst naar de gekoppelde Azure SQL-service. De gekoppelde Azure SQL-service verwijst naar een Azure SQL-server en de SQL Azure-gegevensset bevat de naam van de tabel waarin de uitvoergegevens. 
Gegevens verkooppijplijn | De pijplijn heeft een activiteit van het type kopiëren waarmee de gegevensset Azure blob als invoer en de SQL Azure-gegevensset als een uitvoer. De activiteit kopie gegevens opgehaald uit een Azure blob aan een tabel in de SQL Azure-database.  

Een factory gegevens kan een of meer pijpleidingen bevatten. Een pijplijn kan een of meer activiteiten in deze hebben. Er zijn twee soorten activiteiten: [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) en [gegevens-transformatie activiteiten](data-factory-data-transformation-activities.md). In deze zelfstudie kunt u een pijplijn maken met een activiteit (kopiëren).

![Azure Blob kopiëren met Azure SQL-Database](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Maak een JSON-bestand met de naam **ADFCopyTutorialARM.json** in **C:\ADFGetStarted** -map met de volgende inhoud:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parameters JSON 
Een JSON-bestand met de naam **ADFCopyTutorialARM-Parameters.json** waarin de parameters voor de resourcemanager Azure-sjabloon maken. 

> [AZURE.IMPORTANT] Geef de naam en de toets van uw account Azure-opslag voor **storageAccountName** en **storageAccountKey** parameters.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] U moet mogelijk afzonderlijke parameter JSON-bestanden voor de ontwikkeling, testen en productieomgevingen die u met dezelfde gegevens Factory JSON sjabloon gebruiken kunt. Met behulp van een Power-shellscript, kunt u de gegevens Factory entiteiten in deze omgevingen automatiseren.  

## <a name="create-data-factory"></a>Gegevens factory maken
1. **Azure PowerShell** starten en uitvoeren van de volgende opdracht uit:
    - Uitvoeren `Login-AzureRmAccount` en voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij de portal van Azure.  
    - Uitvoeren `Get-AzureRmSubscription` om weer te geven van alle abonnementen voor dit account.
    - Uitvoeren `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` om te selecteren van het abonnement waaraan u wilt werken. 
2. Voer de volgende opdracht om te implementeren Data Factory entiteiten met de sjabloon resourcemanager dat u in stap 1 hebt gemaakt.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor verkooppijplijn
1. Meld u aan bij de [portal van Azure](https://portal.azure.com) via uw Azure-account.
2. Klik op **gegevens factory's** in het linkermenu (of) Klik op **meer services** en **gegevens factory's** op onder **INTELLIGENCE + ANALYTICS** -categorie.

    ![Het menu gegevens factory 's](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Op de pagina **gegevens factory's** zoeken en vinden van uw gegevens factory. 

    ![Zoeken naar gegevens factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klik op de fabriek Azure-gegevens. U ziet de introductiepagina van de fabriek gegevens.

    ![Startpagina voor gegevens factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klik op de tegel **Diagram** als u wilt zien van de diagramweergave van uw gegevens factory.

    ![Diagramweergave van gegevens factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Dubbelklik in de diagramweergave te klikken, op de gegevensset **SQLOutputDataset**. U ziet dat de status van het segment. Wanneer de kopieerbewerking is voltooid, wordt u de status instellen op **Gereed**.

    ![Uitvoer segment in gereed](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Als het segment **klaar** is, controleer dan of dat de gegevens wordt gekopieerd naar de **emp** -tabel in de SQL Azure-database.

Zie [Monitor gegevenssets en pijplijn](data-factory-monitor-manage-pipelines.md) voor instructies over het gebruik van de Azure portal bladen de verkooppijplijn en gegevenssets die u in deze zelfstudie hebt gemaakt.

U kunt ook controleren en beheren-App gebruiken om uw gegevens pijpleidingen de te houden. Zie [beeldscherm en beheren van Azure gegevens Factory pijpleidingen met App Monitoring](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van de toepassing.


## <a name="data-factory-entities-in-the-template"></a>Gegevens Factory entiteiten in de sjabloon

### <a name="define-data-factory"></a>Gegevens factory definiëren
U kunt een factory gegevens definiëren in de resource manager sjabloon zoals wordt weergegeven in het onderstaande voorbeeld:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

De dataFactoryName luidt als volgt: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Dit is een unieke tekenreeks op basis van de resource-ID.  

### <a name="defining-data-factory-entities"></a>Gegevens Factory entiteiten definiëren
De volgende gegevens Factory-entiteiten zijn gedefinieerd in de JSON-sjabloon: 

1. [Azure gekoppeld opslagservice](#azure-storage-linked-service)
2. [Azure gekoppelde SQL-service](#azure-sql-database-linked-service)
3. [Azure blob gegevensset](#azure-blob-dataset)
4. [Azure SQL-gegevensset](#azure-sql-dataset)
5. [Gegevens verkooppijplijn met een activiteit kopiëren](#data-pipeline)

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

De connectionString gebruikmaakt van de parameters storageAccountName en storageAccountKey. De waarden voor deze parameters doorgegeven met behulp van een configuratiebestand. De definitie wordt ook gebruikt voor variabelen: azureStroageLinkedService en dataFactoryName gedefinieerd in de sjabloon. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL-Database gekoppeld-service
U opgeven de naam van de Azure SQL-server, de databasenaam, de gebruikersnaam in te voeren en het gebruikerswachtwoord in deze sectie. Zie [Azure SQL gekoppeld service](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een gekoppelde Azure SQL-service.  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

De connectionString wordt gebruikt voor naamsqlserver, databasenaam sqlServerUserName en sqlServerPassword parameters waarvan de waarden worden doorgegeven met behulp van een configuratiebestand. De definitie wordt ook gebruikt voor de volgende variabelen van de sjabloon: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure blob gegevensset
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-gegevensset
U opgeven de naam van de tabel in de SQL Azure-database waarin de gekopieerde gegevens van de Azure-blobopslag. Zie [Eigenschappen van Azure SQL-gegevensset](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) voor meer informatie over de eigenschappen van JSON gebruikt voor het definiëren van een Azure SQL-gegevensset. 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Gegevens verkooppijplijn
U definiëren een pijplijn die gegevens van de gegevensset Azure blob naar de SQL Azure-gegevensset kopiëren. Zie [Verkooppijplijn JSON](data-factory-create-pipelines.md#pipeline-json) voor beschrijvingen van de JSON-elementen gebruikt om te definiëren van een pijplijn in dit voorbeeld. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>De sjabloon opnieuw gebruiken 
In deze zelfstudie, moet u een sjabloon voor het definiëren van gegevens Factory entiteiten en een sjabloon voor het doorgeven van waarden voor parameters gemaakt. De pijplijn kopieert gegevens van een Azure Storage-account naar een opgegeven via parameters Azure SQL-database. Als u dezelfde sjabloon wilt implementeren Data Factory entiteiten in verschillende omgevingen, moet u een parameterbestand voor elke omgeving maken en gebruik deze methode als u de implementatie in die omgeving.     

Voorbeeld:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Zoals u ziet dat de eerste opdracht parameterbestand voor de ontwikkelomgeving, tweede één voor de testomgeving en de derde één voor de productieomgeving gebruikt.  

U kunt ook opnieuw gebruiken voor de sjabloon als herhaalde taken wilt uitvoeren. Bijvoorbeeld, moet u veel gegevens factory's maken met een of meer pijpleidingen die dezelfde logica implementeren, maar elke factory gegevens maakt gebruik van verschillende Azure opslag en Azure SQL Database-accounts. In dit scenario gebruikt u dezelfde sjabloon in dezelfde omgeving (ontwikkelaar, test of productie) met verschillende parameterbestanden gegevens factory's maken.   

