<properties 
    pageTitle="Verplaats gegevens van Teradata | Azure gegevens Factory" 
    description="Meer informatie over Teradata-Connector voor de Data Factory-service waarmee u gegevens uit Teradata-Database verplaatsen" 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Verplaats gegevens van Azure gegevens Factory met Teradata

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens om op te slaan uit Teradata naar een andere gegevens te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

Gegevens factory ondersteunt verbindingen met on-premises Teradata bronnen via de Data Management Gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway. 

> [AZURE.NOTE]
Gateway is vereist, zelfs als de Teradata wordt gehost in een Azure IaaS VM. U kunt de gateway installeren op de dezelfde IaaS VM als de gegevensopslag of op een andere VM zo lang maken als de gateway kan worden verbonden met de database. 

Gegevens factory ondersteunt alleen zwevend gegevens van Teradata naar andere winkels gegevens, niet vanuit andere winkels gegevens naar Teradata.

## <a name="installation"></a>Installatie 

Voor Data Management Gateway verbinding maken met de Teradata-Database, moet u de [.NET-gegevensprovider voor Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) op dezelfde computer als de Data Management Gateway installeren.

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit Teradata naar een van de ondersteunde sink gegevensarchieven is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het kopiëren van gegevens uit Teradata naar Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Voorbeeld: Gegevens uit Teradata naar Azure Blob kopiëren

In dit voorbeeld ziet hoe u gegevens uit een Teradata-database naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  De [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit de resultaten van een query in Teradata-database naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

Als eerste stap de data management gateway-instelling. Er zijn de instructies in het artikel [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Gekoppeld Teradata-service:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure-blobopslag gekoppeld service:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Teradata invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MijnTabel" in Teradata en een kolom met de naam 'tijdstempel' voor time reeksgegevens bevat.

"Externe" instellen: waar de gegevens Factory-service wordt gemeld dat de tabel externe gegevens fabriek is en is niet geproduceerd door een activiteit in de fabriek gegevens.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Azure Blob uitvoer gegevensset:**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pijplijn met kopie activiteit:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en om uit te voeren per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert u de gegevens in de afgelopen uur om te kopiëren.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Eigenschappen van de service Teradata die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor gekoppeld Teradata-service. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet zijn ingesteld op: **OnPremisesTeradata** | Ja
Server | Naam van de Teradata-server. | Ja
authenticationType | Type verificatie waarmee verbinding met de Teradata-database. Mogelijke waarden zijn: anoniem, Basic en Windows. | Ja
gebruikersnaam | Geef de gebruikersnaam in te voeren als u van Basic of Windows-verificatie gebruikmaakt. | Nee 
wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. | Nee 
gatewayName | De naam van de gateway die de gegevens Factory-service gebruiken moet voor het verbinding maken met de on-premises implementatie Teradata-database. | Ja

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises implementatie Teradata-gegevensbron.

## <a name="teradata-dataset-type-properties"></a>Teradata gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. Er zijn geen type eigenschappen ondersteund voor de Teradata-gegevensset. 


## <a name="teradata-copy-activity-type-properties"></a>Eigenschappen van Teradata kopiëren activiteit type

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Wanneer de bron is van het type **RelationalSource** (waaronder Teradata) is de volgende eigenschappen zijn beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Ja

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>De toewijzing van het type voor Teradata

Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert u de activiteit kopie automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende stappen 2-benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst naar Teradata, worden de volgende toewijzingen van Teradata type gebruikt voor .NET-type.

Type Teradata-Database | .NET framework-type
----------------- | ---------------------------
Teken | Tekenreeks
CLOB | Tekenreeks
Afbeelding | Tekenreeks
VarChar | Tekenreeks
VarGraphic | Tekenreeks
BLOB | Byte]
Byte | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Decimaal | Decimaal
Dubbeltik | Dubbeltik
Geheel getal | Int32
Getal | Dubbeltik
DateTime | Int16
Datum | Datum /
Tijd | Tijdspanne
Tijd aan tijdzone | Tekenreeks
Tijdstempel | Datum /
Tijdstempel met tijdzone | DateTimeOffset
Interval dag | Tijdspanne
Interval dag op uur | Tijdspanne
Interval dag naar minuten | Tijdspanne
Interval dag tweede | Tijdspanne
Interval uur | Tijdspanne
Interval uur naar minuten | Tijdspanne
Interval uur tweede | Tijdspanne
Interval minuten | Tijdspanne
Interval minuut tweede | Tijdspanne
Interval tweede | Tijdspanne
Interval jaar | Tekenreeks
Interval jaar-tot-maand | Tekenreeks
Interval maand | Tekenreeks
Period(date) | Tekenreeks
Period(Time) | Tekenreeks
Termijn (tijd met tijdzone) | Tekenreeks
Period(timestamp) | Tekenreeks
Termijn (tijdstempel met tijdzone) | Tekenreeks
XML | Tekenreeks

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.