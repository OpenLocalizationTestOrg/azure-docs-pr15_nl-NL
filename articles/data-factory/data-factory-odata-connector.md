<properties 
    pageTitle="Verplaats gegevens van de OData-bronnen | Azure gegevens Factory" 
    description="Meer informatie over het verplaatsen van gegevens uit OData-bronnen met Azure gegevens Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Gegevens uit een OData-bron met Azure gegevens Factory verplaatsen
In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen uit een OData-bron naar een andere gegevensopslag. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

> [AZURE.NOTE] Deze verbindingslijn OData ondersteuning kopiëren van gegevens uit beide cloud OData en on-premises OData bronnen. Voor de laatste moet u de Data Management Gateway installeren. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway.

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit een OData-bron is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

De volgende voorbeelden bieden definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het kopiëren van gegevens uit een OData-bron naar een Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Voorbeeld: Gegevens uit OData-bron naar Azure Blob kopiëren

In dit voorbeeld ziet hoe u gegevens uit een OData-bron naar Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OData](#odata-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [ODataResource](#odata-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](#odata-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef worden gegevens gekopieerd van query's uitvoeren met een OData-bron naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

**OData gekoppeld service** Dit voorbeeld wordt de basisverificatie. Zie sectie [OData gekoppelde service](#odata-linked-service-properties) voor verschillende soorten verificatie die u kunt gebruiken. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
            }
        }
    }


**Azure gekoppeld opslagservice**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**OData invoer gegevensset**

Instellen "externe": "true" melding aan de Data Factory-service dat de gegevensset de fabriek gegevens buiten en niet is geproduceerd door een activiteit in de fabriek gegevens.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

**Pad** opgeven in de gegevensset is definitie optioneel. 


**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pijplijn met activiteit kopiëren**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** Hiermee selecteert u de meest recente (nieuwste) gegevens uit de OData-bron.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


**Query** opgeven in de definitie verkooppijplijn is optioneel. De **URL** die de gegevens Factory-service wordt gebruikt om op te halen van gegevens: de URL in de gekoppelde service (vereist) + pad opgegeven in de gegevensset (optioneel) + query in de pijplijn (optioneel). 

## <a name="odata-linked-service-properties"></a>OData gekoppeld Service-eigenschappen

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor OData gekoppeld service.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **OData** | Ja |
| URL| De URL van de OData-service. | Ja |
| authenticationType | Type verificatie waarmee verbinding met de OData-bron. <br/><br/> Mogelijke waarden zijn voor cloud OData, anoniem en als Basic. Mogelijke waarden zijn voor OData-on-premises implementatie, anoniem, Basic en Windows. | Ja | 
| gebruikersnaam | Geef de gebruikersnaam in te voeren als u basisverificatie gebruikt. | Ja (alleen als u van basisverificatie gebruikmaakt) | 
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. | Ja (alleen als u van basisverificatie gebruikmaakt) | 
| gatewayName | De naam van de gateway die de gegevens Factory-service verbinding maken met de OData-service voor on-premises implementatie moet gebruiken. Geef alleen als u gegevens vanuit on-premises OData bron kopieert. | Nee |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Anonieme verificatie gebruikt
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Windows-verificatie toegang krijgen tot het on-premises implementatie OData-bron gebruiken

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>OData gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **ODataResource** (waaronder OData gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| pad | Pad naar de OData-bron | Nee | 

## <a name="odata-copy-activity-type-properties"></a>OData kopie activiteit type eigenschappen

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleid zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Wanneer de bron is van het type **RelationalSource** (waaronder OData) is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Voorbeeld | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | "? $select = naam, beschrijving en $top = 5" | Nee | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>De toewijzing van het type voor OData

Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert kopie activiteit automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende stappen 2-benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer zwevend gegevens uit OData-gegevens worden opgeslagen, worden OData-gegevenstypen zijn toegewezen aan .NET-gegevenstypen.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

