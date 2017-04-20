<properties 
    pageTitle="Gebruik resourcemanager sjablonen in Data Factory | Microsoft Azure" 
    description="Informatie over het maken en Azure resourcemanager sjablonen gebruiken om gegevens Factory entiteiten maken." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Sjablonen gebruiken om Azure gegevens Factory entiteiten maken

## <a name="overview"></a>Overzicht
Bij het gebruik van Azure gegevens Factory voor uw behoeften van de integratie van gegevens, vindt u mogelijk zelf hergebruiken hetzelfde patroon over verschillende omgevingen of dezelfde taak wanneer binnen dezelfde oplossing implementeren. Sjablonen te implementeren en deze scenario's op een eenvoudige manier beheren. Sjablonen in Azure gegevens Factory zijn ideaal voor scenario's voor hergebruik en terugkeerpatroon.
 
Neem de situatie waarbij een organisatie 10 manufacturing-installaties in de wereld heeft. De logboeken van elke plant worden opgeslagen in een afzonderlijk lokale SQL Server-database. Het bedrijf wil een één data-warehouse in de cloud voor ad-hoc-analyses maken. Deze wil ook hebben de dezelfde logica maar verschillende configuraties voor ontwikkeling, test- en -omgevingen. 

In dit geval moet een taak worden herhaald binnen dezelfde omgeving, maar met verschillende waarden in de gegevens van 10 factory's voor elke fabriek. In feite is **Terugkeerpatroon** aanwezig. Templating kunnen de onttrekking van deze algemene overdracht (dat wil zeggen pijpleidingen met dezelfde activiteiten in elke data factory genoemd), maar een afzonderlijke parameter-bestand voor elke fabriek wordt gebruikt.

Bovendien kunnen de organisatie wil deze bedrijven 10 gegevens meerdere keren implementeren in verschillende omgevingen, sjablonen Gebruik deze **hergebruik** met behulp van afzonderlijke parameterbestanden voor de ontwikkeling, test- en -omgevingen.

## <a name="templating-with-azure-resource-manager"></a>Templating met Azure resourcemanager
[Resourcemanager Azure-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment) zijn een uitstekende manier om te bereiken templating in fabriek van Azure-gegevens. Resourcemanager sjablonen definiëren de infrastructuur en configuratie van uw Azure-oplossing via een JSON-bestand. Omdat Azure resourcemanager sjablonen met alle/meest Azure-services werken, kan deze sterk uiteen worden gebruikt voor het beheren van alle resources van uw Azure activa eenvoudig. Zie [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md) voor meer informatie over de Resource Manager-sjablonen in het algemeen. 

## <a name="tutorials"></a>Zelfstudies
Zie de volgende zelfstudies voor stapsgewijze instructies Data Factory entiteiten maken met behulp van resourcemanager sjablonen:

- [Zelfstudie: Een pijplijn om gegevens te kopiëren met behulp van Azure resourcemanager-sjabloon maken](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Zelfstudie: Een pijplijn om gegevens te verwerken met behulp van Azure resourcemanager-sjabloon maken](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Gegevens Factory-sjablonen op Github
Raadpleeg de volgende Azure snel aan de slag-sjablonen op Github: 

- [Maken van een fabriek gegevens om gegevens te kopiëren van Azure-blobopslag met Azure SQL-Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Maken van een fabriek gegevens met component activiteit op Azure HDInsight cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Maken van een fabriek gegevens om gegevens uit Salesforce naar Azure BLOB's kopiëren](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Je mag rustig delen van uw Azure gegevens Factory-sjablonen op [Azure snel starten](https://azure.microsoft.com/documentation/templates/). Zie de [bijdrage handleiding](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) tijdens het ontwikkelen van sjablonen die kunnen worden gedeeld via deze opslagplaats. 

De volgende secties vindt meer informatie over het definiëren van gegevens Factory resources in een sjabloon resourcemanager. 

## <a name="defining-data-factory-resources-in-templates"></a>Gegevens Factory resources definiëren in sjablonen
Het hoogste niveau sjabloon voor het definiëren van een fabriek gegevens luidt als volgt:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Gegevens factory definiëren

U definieert een factory gegevens in de sjabloon resourcemanager, zoals wordt weergegeven in het onderstaande voorbeeld:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

De dataFactoryName is in 'variabelen' gedefinieerd als:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Gekoppelde services definiëren 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Zie [Gekoppelde opslagservice](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [Gekoppelde Services berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over de JSON-eigenschappen voor de specifieke gekoppelde service die u wilt implementeren. De parameter "dependsOn" bevat de naam van de bijbehorende gegevens fabriek. Een voorbeeld van het definiëren van een gekoppelde service voor de opslag van Azure worden weergegeven in de volgende JSON-definitie:

### <a name="define-datasets"></a>Gegevenssets definiëren

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Raadpleeg [gegevensopslag ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor meer informatie over de JSON-eigenschappen voor het specifieke gegevensset type die u wilt implementeren. Opmerking dat de "dependsOn"-parameter geeft de naam van de bijbehorende gegevens fabriek en de opslag gekoppeld service. Een voorbeeld van het definiëren van de gegevensset type Azure-blobopslag wordt weergegeven in de volgende JSON-definitie:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Pijpleidingen definiëren

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Raadpleeg [pijpleidingen definiëren](data-factory-create-pipelines.md#pipeline-json) voor meer informatie over de JSON-eigenschappen voor het definiëren van de specifieke verkooppijplijn en activiteiten die u wilt implementeren. Opmerking de "dependsOn"-parameter geeft de naam van de gegevens fabriek en alle bijbehorende gekoppeld services of gegevenssets. Een voorbeeld van een pijplijn die Hiermee kopieert de gegevens van Azure-blobopslag met Azure SQL-Database wordt weergegeven in het volgende JSON-fragment:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Gegevens Factory-sjabloon van parameters voorzien
Voor procedures parameters voorzien, Zie [Aanbevolen procedures voor het maken van sjablonen van Azure resourcemanager](../resource-manager-template-best-practices.md#parameters) artikel. In het algemeen, moet parametergebruik worden geminimaliseerd, vooral als variabelen in plaats daarvan kunnen worden gebruikt. Alleen bieden parameters in de volgende scenario's:

- Instellingen is afhankelijk van het omgeving (voorbeeld: ontwikkeling, test- en)
- Geheimen (zoals wachtwoorden)

Als u nodig hebt om op te halen geheimen uit [Kluis van Azure-sleutel](../key-vault/key-vault-get-started.md) bij de implementatie van Azure gegevens Factory entiteiten sjablonen gebruiken, geeft u de **belangrijkste kluis** en **geheime naam** , zoals wordt weergegeven in het volgende voorbeeld:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Tijdens het exporteren van sjablonen voor bedrijven van bestaande gegevens is op dit moment nog niet ondersteund, is in het identiteitsprogramma werkt. 


