<properties 
    pageTitle="Verplaats gegevens van Sybase | Azure gegevens Factory" 
    description="Meer informatie over het verplaatsen van gegevens uit een Sybase-Database via van Azure gegevens Factory." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Verplaats gegevens van Azure gegevens Factory met Sybase 

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen uit Sybase naar een andere gegevensopslag. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

Factory-gegevensservice ondersteunt verbindingen met on-premises Sybase bronnen met de Data Management Gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway. 

> [AZURE.NOTE]
> Gateway is vereist, zelfs als de Sybase-database wordt gehost in een Azure IaaS VM. U kunt de gateway installeren op de dezelfde IaaS VM als de gegevensopslag of op een andere VM zo lang maken als de gateway kan worden verbonden met de database. 

Gegevens factory ondersteunt momenteel alleen zwevend gegevens van Sybase naar andere winkels gegevens, niet vanuit andere winkels gegevens met Sybase.

## <a name="installation"></a>Installatie

Voor Data Management Gateway verbinding maken met de Sybase-Database, moet u de [-gegevensprovider voor Sybase](http://go.microsoft.com/fwlink/?linkid=324846) op dezelfde computer als de Data Management Gateway installeren.

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit een Sybase-database naar een van de ondersteunde sink gegevensarchieven is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het kopiëren van gegevens uit Sybase-database naar Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Voorbeeld: Gegevens uit Sybase naar Azure Blob kopiëren
In dit voorbeeld ziet hoe u gegevens uit een Sybase-database naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Een liked service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  De [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit de resultaten van een query in Sybase-database naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

Als eerste stap de data management gateway-instelling. Er zijn de instructies in het artikel [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Sybase gekoppeld service:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MijnTabel" in Sybase en een kolom met de naam 'tijdstempel' voor time reeksgegevens bevat.

"Externe" instellen: waar de gegevens Factory-service wordt gemeld dat dit gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens. U ziet dat het **type** van de gekoppelde service is ingesteld op: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en om uit te voeren per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens uit de DBA. De ordertabel in de database.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Eigenschappen van de service Sybase die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor gekoppeld Sybase-service.

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet zijn ingesteld op: **OnPremisesSybase** | Ja
Server | De naam van de Sybase-server. | Ja
database | De naam van de Sybase-database. | Ja 
schema  | De naam van het schema in de database. | Nee
authenticationType | Type verificatie waarmee verbinding met de Sybase-database. Mogelijke waarden zijn: anoniem, Basic en Windows. | Ja
gebruikersnaam | Geef de gebruikersnaam in te voeren als u van Basic of Windows-verificatie gebruikmaakt. | Nee
wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |  Nee
gatewayName | De naam van de gateway die de gegevens Factory-service gebruiken moet voor het verbinding maken met de on-premises implementatie Sybase-database. | Ja 

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises Sybase-gegevensbron.

## <a name="sybase-dataset-type-properties"></a>Sybase gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie **typeProperties** voor dataset van het type **RelationalTable** (waaronder Sybase gegevensset) heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Tabelnaam | Naam van de tabel in het exemplaar Sybase-Database die gekoppeld service verwijst naar. | Geen (als **query** van **RelationalSource** wordt aangegeven)

## <a name="sybase-copy-activity-type-properties"></a>Eigenschappen van Sybase kopiëren activiteit type 

Voor een volledige lijst van secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, Zie [maken pijpleidingen](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleid zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Wanneer de bron is van het type **RelationalSource** (waaronder Sybase) is de volgende eigenschappen zijn beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Geen (als de **tabelnaam** van **dataset** is opgegeven)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>De toewijzing van het type voor Sybase

Zoals vermeld in het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , voert u de activiteit kopie automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende stappen 2-benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Sybase ondersteunt T-SQL en typen van de T-SQL. Zie [Azure SQL-Connector](data-factory-azure-sql-connector.md) voor een toewijzingstabel uit een sql-grafiektypen in .NET-type, artikel.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.