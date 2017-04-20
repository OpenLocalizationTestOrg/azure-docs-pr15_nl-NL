<properties 
    pageTitle="Verplaats gegevens van Amazon Redshift met gegevens Factory | Microsoft Azure" 
    description="Meer informatie over het verplaatsen van gegevens van Amazon Redshift met Azure gegevens Factory." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Verplaats gegevens van Amazon Redshift, met Azure gegevens Factory

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens voor het opslaan van Amazon Redshift naar een andere gegevens te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en een lijst met bron/sink gegevens winkels wordt weergegeven.  

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit de Amazon Redshift naar andere winkels gegevens, maar niet voor het verplaatsen van gegevens uit andere winkels gegevens naar Amazon Redshift.

## <a name="prerequisites"></a>Vereisten voor

- Als u gegevens naar een on-premises implementatie gegevensopslag verplaatst, Data Management Gateway (Gebruik IP-adres van de computer) de toegang verlenen tot Amazon Redshift cluster. Zie [autoriseren toegang tot het cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) voor instructies. 
- Als u gegevens naar een Azure gegevensopslag verplaatst, raadpleegt u [Azure Data Center IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653) voor het berekenen IP-adresbereiken (inclusief SQL bereiken) die wordt gebruikt door de Microsoft Azure-datacenters. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit Amazon Redshift is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Deze ziet u hoe u gegevens uit de Amazon Redshift naar Azure-blobopslag kopiëren. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Voorbeeld: Gegevens uit de Amazon Redshift naar Azure Blob kopiëren
In dit voorbeeld ziet hoe u gegevens uit een database Amazon Redshift kopiëren naar een Azure-blobopslag. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

- Een gekoppelde service van het type [AmazonRedshift](#linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](#copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit de resultaten van een query in Amazon Redshift naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

**Amazon-Redshift gekoppeld-service**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Amazon Redshift invoer gegevensset**

Instelling **"externe": waar** wordt de Data Factory-service wordt gemeld dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens. Deze eigenschap instelt op waar in een invoer dataset die niet wordt geleverd door een activiteit in de pijplijn.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert u de gegevens in de afgelopen uur om te kopiëren.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor de service Amazon Redshift gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **AmazonRedshift**. | Ja |
| Server | IP-adres of host naam van de server Amazon Redshift. | Ja |
| poort | Het nummer van de TCP-poorten waarop de server Amazon Redshift luistert voor clientverbindingen. | Nee, standaardwaarde: 5439 |
| database | De naam van de database Amazon Redshift. | Ja |
| gebruikersnaam | Naam van de gebruiker aan wie toegang tot de database heeft.| Ja |
| wachtwoord | Wachtwoord voor het gebruikersaccount.| Ja |


## <a name="dataset-type-properties"></a>Eigenschappen van de gegevensset type

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **RelationalTable** (waaronder Amazon Redshift gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | Naam van de tabel in de database Amazon Redshift die gekoppeld service verwijst naar. | Geen (als **query** van **RelationalSource** is opgegeven) | 

## <a name="copy-activity-type-properties"></a>Activiteit type eigenschappen kopiëren

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Wanneer de bron van de activiteit kopie van het type **RelationalSource** (waaronder Amazon Redshift) is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Geen (als de **tabelnaam** van **dataset** is opgegeven) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>De toewijzing van het type voor Amazon Redshift

Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert kopie activiteit automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende twee stappen benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst naar Amazon Redshift, wordt de volgende toewijzingen van Amazon Redshift typen worden gebruikt voor .NET-gegevenstypen.

Amazon Redshift Type | .NET op basis van Type
-------------------- | ---------------
DATETIME | Int16
GEHEEL GETAL | Int32
BIGINT | Int64
DECIMAAL | Decimaal
REËLE | Eén
DUBBELE PRECISIE | Dubbeltik
BOOLEAANSE WAARDE | Tekenreeks
TEKEN | Tekenreeks
VARCHAR | Tekenreeks
DATUM | Datum /
TIJDSTEMPEL | Datum /
TEKST | Tekenreeks



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 
- [Kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 