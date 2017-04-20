<properties 
    pageTitle="Verplaats gegevens van Amazon eenvoudige Storage-Service via Data Factory | Microsoft Azure" 
    description="Meer informatie over het verplaatsen van gegevens uit de Amazon eenvoudige Storage-Service (S3) met behulp van Azure gegevens Factory." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Verplaats gegevens van Amazon eenvoudige Storage-Service via Azure gegevens Factory

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens voor het opslaan van Amazon eenvoudige Storage-Service (S3) naar een andere gegevens te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) die een algemeen overzicht van de verplaatsing van gegevens en een lijst met ondersteunde gegevensbronnen/sink gegevens winkels met kopie activiteit presenteert.  

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit de Amazon S3 naar andere winkels gegevens, maar niet voor het verplaatsen van gegevens uit andere winkels gegevens naar Amazon S3.

## <a name="required-permissions"></a>Vereiste machtigingen

Als gegevens wilt kopiëren van Amazon S3, moet dat u onderstaande machtigingen hebt gekregen:

- **S3:GetObject** en **s3:GetObjectVersion** voor Amazon S3 Object bewerkingen
- **S3:ListBucket** en **s3:ListAllMyBuckets** (gebruikt alleen wizard kopiëren) voor Amazon S3 Emmertje bewerkingen 

U vindt de volledige lijst van Amazon S3 machtigingen met details van [Machtigingen geven in een beleid](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit Amazon S3, is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Deze ziet u hoe u gegevens uit de Amazon S3 naar Azure-blobopslag kopiëren. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Voorbeeld: Gegevens uit de Amazon S3 naar Azure Blob kopiëren
In dit voorbeeld ziet hoe u gegevens uit een S3 Amazon naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

- Een gekoppelde service van het type [AwsAccessKey](#linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [AmazonS3](#dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [FileSystemSource](#copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens van Amazon S3 naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

**Amazon S3 gekoppeld-service**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Amazon S3 invoer gegevensset**

Instelling **"externe": waar** wordt de Data Factory-service wordt gemeld dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens. Deze eigenschap instelt op waar in een invoer dataset die niet wordt geleverd door een activiteit in de pijplijn.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
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
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat een beschrijving voor specifieke JSON-elementen aan Amazon S3 (**AwsAccessKey**) gekoppeld service.

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID van de geheime toegangstoets. | tekenreeks | Ja |
| secretAccessKey | De geheime toegangstoets zelf. | Versleutelde geheime tekenreeks | Ja | 


## <a name="dataset-type-properties"></a>Eigenschappen van de gegevensset type

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **AmazonS3** (waaronder Amazon S3 gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | ------ | 
| bucketName | De naam van het emmertje S3. | Tekenreeks | Ja |
| toets | De object-toets S3. | Tekenreeks | Nee | 
| voorvoegsel | Voorvoegsel voor de S3 object-sleutel. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Dit geldt alleen als sleutel is leeg. | Tekenreeks | Nee | 
| Versie | De versie van S3 object als S3 versiebeheer is ingeschakeld. | Tekenreeks | Nee |  
| indeling | De volgende indelingstypen worden ondersteund: **tekstopmaak**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Stel de eigenschap **type** onder Opmaak op een van deze waarden. Zie [Tekstopmaak precisie](#specifying-textformat), [AvroFormat precisie](#specifying-avroformat) [Precisie JsonFormat](#specifying-jsonformat), [OrcFormat opgeven](#specifying-orcformat)en [Precisie ParquetFormat](#specifying-parquetformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de sectie indeling in de definities van beide invoer- en uitvoerbereik gegevensset overslaan.| Nee
| compressie | Geef het type en compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. De compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |

> [AZURE.NOTE] Hiermee wordt de locatie van het object S3 waarbij Emmertje is de container hoofdmap voor S3 objecten en sleutel is het volledige pad naar S3 object bucketName + toets.

### <a name="sample-dataset-with-prefix"></a>Voorbeeld gegevensset met voorvoegsel

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Verzameling voorbeeldgegevens (met versie)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamische paden voor S3

In de steekproef gebruiken we vaste waarden voor de sleutel en bucketName eigenschappen in de gegevensset Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

U kunt gegevens Factory de sleutel en bucketName dynamisch gedurende runtime berekenen met behulp van variabelen zoals SliceStart hebben.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

U kunt doen hetzelfde is voor de eigenschap voorvoegsel van een gegevensset Amazon S3. Zie [Data Factory-functies en variabelen](data-factory-functions-variables.md) voor een lijst met ondersteunde functies en variabelen. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Activiteit type eigenschappen kopiëren

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Als de bron in de kopie activiteit van het type **FileSystemSource** (waaronder Amazon S3), is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- | 
| recursieve | Hiermee geeft u op of naar recursief S3 lijst objecten onder de map. | waar/onwaar | Nee | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 
- [Kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 
