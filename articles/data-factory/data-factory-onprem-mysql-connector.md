<properties 
    pageTitle="Verplaats gegevens van MySQL | Azure gegevens Factory" 
    description="Meer informatie over het verplaatsen van gegevens uit een MySQL-database via van Azure gegevens Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Gegevens uit MySQL-met Azure gegevens Factory verplaatsen

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens om op te slaan uit MySQL naar een andere gegevens te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

Factory-gegevensservice ondersteunt verbindingen met on-premises MySQL bronnen met de Data Management Gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway. 

> [AZURE.NOTE] Gateway is vereist, zelfs als de MySQL-database wordt gehost op een IaaS Azure virtuele machine (VM). U kunt de gateway installeren op de dezelfde VM als de gegevensopslag of op een andere VM zo lang maken als de gateway kan worden verbonden met de database.  

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit MySQL naar andere winkels gegevens, maar niet voor het verplaatsen van gegevens uit andere winkels gegevens naar MySQL.

## <a name="installation"></a>Installatie 
Voor Data Management Gateway verbinding maken met de MySQL-Database, moet u de [MySQL-Connector/Net 6.6.5 voor Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) op dezelfde computer als de Data Management Gateway installeren.

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit een MySQL-database naar een van de ondersteunde sink gegevensarchieven is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat definities van een steekproef JSON die u gebruiken kunt om te maken van een pijplijn. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor een volledige scenario met stapsgewijze instructies. Gegevens kunnen worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Voorbeeld: Gegevens uit MySQL naar Azure Blob kopiëren
In dit voorbeeld ziet hoe u gegevens uit een on-premises implementatie MySQL-database naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  

> [AZURE.IMPORTANT] In dit voorbeeld biedt JSON fragmenten. Deze bevat geen stapsgewijze instructies voor het maken van de fabriek gegevens. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor stapsgewijze instructies. 
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit de resultaten van een query in MySQL-database naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

Als eerste stap de data management gateway-instelling. Er zijn de instructies in het artikel [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**Gekoppeld MySQL-service**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**MySQL invoer gegevensset**

Het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MijnTabel" in MySQL en een kolom met de naam "timestampcolumn" voor time reeksgegevens bevat.

Instellen "externe": "true" melding aan de Data Factory-service dat de tabel de fabriek gegevens buiten en niet is geproduceerd door een activiteit in de fabriek gegevens.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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



**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Eigenschappen van de service MySQL die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor gekoppeld MySQL-service.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **OnPremisesMySql** | Ja |
| Server | De naam van de MySQL-server. | Ja |
| database | De naam van de MySQL-database. | Ja | 
| schema  | De naam van het schema in de database. | Nee | 
| authenticationType | Type verificatie waarmee verbinding met de MySQL-database. Mogelijke waarden zijn: anoniem, Basic en Windows. | Ja | 
| gebruikersnaam | Geef de gebruikersnaam in te voeren als u van Basic of Windows-verificatie gebruikmaakt. | Nee | 
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. | Nee | 
| gatewayName | De naam van de gateway die de gegevens Factory-service gebruiken moet voor het verbinding maken met de on-premises implementatie MySQL-database. | Ja |

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises MySQL-gegevensbron.

## <a name="mysql-dataset-type-properties"></a>MySQL gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **RelationalTable** (waaronder MySQL gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | Naam van de tabel in het exemplaar MySQL-Database die gekoppeld service verwijst naar. | Geen (als **query** van **RelationalSource** is opgegeven) | 

## <a name="mysql-copy-activity-type-properties"></a>Eigenschappen van MySQL kopiëren activiteit type

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer- en uitvoerbereik tabellen, zijn beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Wanneer de bron in de kopie activiteit van het type **RelationalSource** (waaronder MySQL) is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Geen (als de **tabelnaam** van **dataset** is opgegeven) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>De toewijzing van het type voor MySQL

Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert kopie activiteit automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende twee stappen benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst naar MySQL, worden de volgende toewijzingen uit MySQL-typen gebruikt voor .NET-gegevenstypen.

| Type MySQL-Database | .NET framework-type |
| ------------------- | ------------------- | 
| bigint niet ondertekend | Decimaal |
| bigint | Int64 |
| bits | Decimaal |
| BLOB | Byte] |
| BOOL |  Booleaanse waarde | 
| teken | Tekenreeks |
| datum | Datum / |
| datum / | Datum / |
| decimaal | Decimaal |
| dubbele precisie | Dubbeltik |
| dubbele | Dubbeltik |
| opsommen | Tekenreeks |
| toegestane achterstand | Eén |
| int niet ondertekend | Int64 |
| int | Int32 |
| geheel getal niet ondertekend | Int64 |
| geheel getal | Int32 | 
| lange varbinary | Byte] |
| lange varchar | Tekenreeks |
| longblob | Byte] |
| LONGTEXT | Tekenreeks | 
| mediumblob |  Byte] | 
| mediumint niet ondertekend | Int64 |
| mediumint | Int32 | 
| mediumtext | Tekenreeks |
| numerieke | Decimaal |
| reële |  Dubbeltik |
| instellen | Tekenreeks |
| DateTime niet ondertekend | Int32 |
| DateTime | Int16 |
| tekst | Tekenreeks |
| tijd | Tijdspanne |
| tijdstempel | Datum / |
| tinyblob | Byte] |
| tinyint niet ondertekend |  Int16 | 
| tinyint | Int16 |
| tinytext | Tekenreeks | 
| varchar | Tekenreeks | 
| jaar | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

