<properties 
    pageTitle="Verplaats gegevens van DB2 | Azure gegevens Factory" 
    description="Meer informatie over het verplaatsen van gegevens uit een DB2-Database via van Azure gegevens Factory" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Gegevens uit DB2 met Azure gegevens Factory verplaatsen
In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens om op te slaan vanuit DB2 naar een andere gegevens te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

Gegevens factory ondersteunt verbindingen met on-premises DB2 bronnen met de Data Management Gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway. 

> [AZURE.NOTE]
Met de gateway verbinding maken met DB2, zelfs als die wordt gehost in Azure IaaS VMs. Als u probeert verbinding maken met een exemplaar van DB2 gehost in de cloud, kunt u ook het exemplaar van de gateway installeren in IaaS VM.

Gegevens factory ondersteunt momenteel alleen zwevend gegevens van DB2 naar andere winkels gegevens, niet vanuit andere winkels gegevens naar DB2. 

## <a name="installation"></a>Installatie 

De Data Management Gateway biedt een ingebouwde DB2-stuurprogramma die ondersteuning biedt voor het volgende: 

- SQLAM 9 / 10 / 11
- DB2 voor LUW (Linux, Unix-Windows)
- DB2 voor z/OS en DB2 voor ik (ook bekend als / 400)

Daarom moet u niet langer de stuurprogramma's handmatig installeren als u gegevens uit DB2.

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit een DB2-Database is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

De volgende voorbeelden bieden definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Gegevens kopiëren uit DB2-database en Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Voorbeeld: Gegevens uit DB2 naar Azure Blob kopiëren

In dit voorbeeld ziet hoe u gegevens uit een on-premises implementatie DB2-database naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

De steekproef kopieert gegevens uit de resultaten van een query in een DB2-database naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

Als eerste stap installeren en configureren van een data management gateway. Instructies vindt u in het artikel [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**DB2 gekoppeld service:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
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

**DB2 invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MijnTabel" in DB2 en een kolom met de naam 'tijdstempel' voor time reeksgegevens bevat.

"Externe" instellen: waar de gegevens Factory-service wordt gemeld dat dit gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens. U ziet dat het **type** is ingesteld op **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
            "typeProperties": {},
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
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pijplijn met kopie activiteit:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** Hiermee selecteert u de gegevens uit de tabel Orders.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Eigenschappen van de service DB2 die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor de service DB2 die zijn gekoppeld. 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **OnPremisesDB2** | Ja |
| Server | De naam van de DB2-server. | Ja |
| database | De naam van de DB2-database. | Ja |
| schema | De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. | Nee |
| authenticationType | Type verificatie waarmee verbinding met de DB2-database. Mogelijke waarden zijn: anoniem, Basic en Windows. | Ja |
| gebruikersnaam | Geef de gebruikersnaam in te voeren als u van Basic of Windows-verificatie gebruikmaakt. | Nee |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. | Nee |
| gatewayName | De naam van de gateway die de gegevens Factory-service gebruiken moet voor het verbinding maken met de on-premises implementatie DB2-database. | Ja |

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises DB2-gegevensbron. 


## <a name="db2-dataset-type-properties"></a>DB2 gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type RelationalTable (waaronder DB2 gegevensset) heeft de volgende eigenschappen.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| Tabelnaam | Naam van de tabel in het exemplaar DB2-Database die gekoppeld service verwijst naar. De tabelnaam is hoofdlettergevoelig. | Geen (als **query** van **RelationalSource** is opgegeven) |

## <a name="db2-copy-activity-type-properties"></a>Eigenschappen van DB2 kopiëren activiteit type

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Voor een activiteit kopiëren als bron van het type **RelationalSource** (waaronder DB2) zijn de volgende eigenschappen beschikbaar in de sectie typeProperties:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | -------------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""`. | Geen (als de **tabelnaam** van **dataset** is opgegeven)|

> [AZURE.NOTE] Schema- en tabelnamen zijn hoofdlettergevoelig. Plaats de namen in "" (dubbele aanhalingstekens) in de query.  

**Voorbeeld:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>De toewijzing van het type voor DB2
Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert u de activiteit kopie automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende stappen 2-benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst naar DB2, worden de volgende toewijzingen van DB2 type gebruikt voor .NET-type.

Type DB2-Database | .NET framework-type 
----------------- | ------------------- 
DateTime | Int16
Geheel getal | Int32
BigInt | Int64
Reële | Eén
Dubbeltik | Dubbeltik
Laten zweven | Dubbeltik
Decimaal | Decimaal
DecimalFloat | Decimaal
Numerieke | Decimaal
Datum | Datum /
Tijd | Tijdspanne
Tijdstempel | Datum /
XML | Byte]
Teken | Tekenreeks
VarChar | Tekenreeks
LongVarChar | Tekenreeks
DB2DynArray | Tekenreeks
Binaire | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Afbeelding | Tekenreeks
VarGraphic | Tekenreeks
LongVarGraphic | Tekenreeks
CLOB | Tekenreeks
BLOB | Byte]
DbClob | Tekenreeks
DateTime | Int16
Geheel getal | Int32
BigInt | Int64
Reële | Eén
Dubbeltik | Dubbeltik
Laten zweven | Dubbeltik
Decimaal | Decimaal
DecimalFloat | Decimaal
Numerieke | Decimaal
Datum | Datum /
Tijd | Tijdspanne
Tijdstempel | Datum /
XML | Byte]
Teken | Tekenreeks


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.


