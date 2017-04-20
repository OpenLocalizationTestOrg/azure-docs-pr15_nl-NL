<properties 
    pageTitle="Gegevens verplaatsen naar/van Oracle met gegevens Factory | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens uit Oracle-database die is on-premises implementatie Azure gegevens Factory gebruiken." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Verplaats gegevens van on-premises implementatie Oracle Azure gegevens Factory gebruiken 

In dit artikel wordt beschreven hoe u gegevens factory kopie activiteit kunt gebruiken om gegevens te verplaatsen naar/van Oracle om op te slaan van/naar een andere gegevens. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

## <a name="installation"></a>Installatie 
Voor de service Factory van Azure-gegevens kunnen verbinding maken met uw on-premises implementatie Oracle-database, moet u de volgende onderdelen installeren: 

- Data Management Gateway op dezelfde computer waarop de database of op een aparte computer om te voorkomen dat voor resources met de database. Data Management Gateway is een client-agent on-premises gegevensbronnen in een veilige en beheerde manier met cloudservices. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway. 
- Oracle-gegevensprovider voor .NET. Dit onderdeel is opgenomen in [Oracle Data Access onderdelen voor Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installeer de juiste versie (32/64 bits) op de hostcomputer waarop de gateway is geïnstalleerd. [Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) hebben toegang tot met Oracle-Database 10 g Release 2 of hoger.

    Als u 'XCopy installatie' kiest, voert u de stappen in de Leesmij.htm. Het is raadzaam om dat u kiest u het installatieprogramma met UI (niet-XCopy een). 
 
    Na de installatie van de provider, start u de Data Management Gateway Host-service op uw computer met Services-mailapplet (of) Data Management Gateway Configuration Manager.  

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die worden gegevens van/naar een Oracle-database naar een van de ondersteunde sink gegevensarchieven gekopieerd, is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Gegevens van/naar een Oracle-database van Azure-blobopslag kopiëren worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Voorbeeld: Gegevens uit Oracle naar Azure Blob kopiëren
In dit voorbeeld ziet hoe u gegevens uit een on-premises implementatie Oracle-database naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als bron- en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als sink gebruikt.

De steekproef kopieert gegevens uit een tabel in een on-premises implementatie Oracle-database naar een blob per uur. Zie voor meer informatie over de verschillende eigenschappen die worden gebruikt in de steekproef, documentatie in secties in de voorbeelden te volgen.

**Oracle gekoppeld service:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure-blobopslag gekoppeld service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MijnTabel" in Oracle en een kolom met de naam "timestampcolumn" voor time reeksgegevens bevat. 

Instellen "externe": "true" melding aan de Data Factory-service dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
            },
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

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). De map pad en de bestandsnaam voor het blob worden dynamisch geëvalueerd op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.
    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Pijplijn met kopie activiteit:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en om uit te voeren per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **OracleSource** en **sink** type is ingesteld op **BlobSink**.  De SQL-query die is opgegeven met de eigenschap **oracleReaderQuery** Hiermee selecteert u de gegevens in de afgelopen uur om te kopiëren.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }


U moet aanpassen van de queryreeks op basis van de manier waarop datums zijn geconfigureerd in uw Oracle-database. Als u het volgende foutbericht wordt weergegeven: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

mogelijk moet u de query te wijzigen, zoals wordt weergegeven in het onderstaande voorbeeld (via de functie to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Voorbeeld: Gegevens van Azure Blob naar Oracle kopiëren
In dit voorbeeld ziet hoe u gegevens uit een Azure-blobopslag kopiëren naar een on-premises implementatie Oracle-database. Gegevens kan echter gekopieerde **rechtstreeks** vanaf een van de bronnen vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) zoals [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als sink gegevensbron.

De steekproef opgehaald gegevens uit een blob aan een tabel in een on-premises implementatie Oracle-database per uur. Zie voor meer informatie over de verschillende eigenschappen die worden gebruikt in de steekproef, documentatie in secties in de voorbeelden te volgen.

**Oracle gekoppeld service:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure-blobopslag gekoppeld service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob invoer gegevensset**

Gegevens wordt opgehaald uit een nieuwe blob per uur (frequentie: uur, interval: 1). De map pad en de bestandsnaam voor het blob worden dynamisch geëvalueerd op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map beschikt over jaar, maand en dag deel uit van de begintijd en de bestandsnaam van het wordt gebruikt voor het uur deel van de begintijd. "externe": "true" instelling de gegevens Factory-service wordt gemeld dat deze tabel externe gegevens fabriek is en is niet geproduceerd door een activiteit in de fabriek gegevens.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Oracle uitvoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel "MijnTabel" in Oracle hebt gemaakt. De tabel in Oracle met hetzelfde aantal kolommen maken, zoals u verwacht dat de Blob CSV-bestand bevat. Nieuwe rijen worden toegevoegd aan de tabel per uur.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Pijplijn met kopie activiteit:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **BlobSource** en het type **sink** is ingesteld op **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }


## <a name="oracle-linked-service-properties"></a>Eigenschappen van de service Oracle die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor Oracle gekoppeld service. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet zijn ingesteld op: **OnPremisesOracle** | Ja
connectionString | Geef de gegevens die nodig zijn verbinding maken met de instantie Oracle-Database voor de eigenschap connectionString. | Ja 
gatewayName | Naam van de gateway die wordt gebruikt om te verbinden met de on-premises implementatie Oracle-server | Ja

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises implementatie Oracle-gegevensbron.
## <a name="oracle-dataset-type-properties"></a>Oracle gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Oracle, Azure blob, Azure tabel, enz.).
 
De sectie typeProperties verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor de gegevensset van het type OracleTable heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Tabelnaam | De naam van de tabel in de Oracle-Database die de gekoppelde service naar verwijst. | Geen (als **oracleReaderQuery** van **OracleSource** is opgegeven)

## <a name="oracle-copy-activity-type-properties"></a>Eigenschappen voor Oracle kopiëren activiteit type

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleid zijn beschikbaar voor alle typen activiteiten. 

> [AZURE.NOTE]
De activiteit kopie haalt slechts één invoer en genereert slechts één uitvoer.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

### <a name="oraclesource"></a>OracleSource
In de kopie activiteit, wanneer de bron van het type **OracleSource** zijn de volgende eigenschappen beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving |Toegestane waarden | Vereist
-------- | ----------- | ------------- | --------
oracleReaderQuery | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer *from MijnTabel <br/> <br/>als niet opgeeft, de SQL-instructie die wordt uitgevoerd: Selecteer* from MijnTabel | Geen (als de **tabelnaam** van **dataset** is opgegeven)

### <a name="oraclesink"></a>OracleSink
**OracleSink** ondersteunt de volgende eigenschappen:

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
writeBatchTimeout | Duurt het voordat de batch invoegbewerking is voltooid voordat er een optreedt time-out. | tijdspanne<br/><br/> Voorbeeld: 00:30:00 (30 minuten). | Nee
writeBatchSize | Hiermee voegt u gegevens in de SQL-tabel wanneer de grootte van de writeBatchSize bereikt.   | Geheel getal (aantal rijen)| Niet (standaard: 10000)  
sqlWriterCleanupScript | Een query voor kopie activiteit uit te voeren dat de gegevens van een specifieke segment opgeschoond opgeven. | Een queryinstructie. | Nee
sliceIdentifierColumnName | Geef de naam van de kolom voor kopie activiteit te vullen met de automatisch gegenereerde segment id, die wordt gebruikt om gegevens van een specifieke segment wanneer opnieuw op te schonen. | De kolomnaam van een kolom met het gegevenstype van binary(32). | Nee


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>De toewijzing van het type voor Oracle

Zoals vermeld voert de [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) artikel kopiëren activiteit automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende stappen 2-benadering:

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst van Oracle, worden de volgende toewijzingen van Oracle-gegevenstype in .NET-type en vice versa gebruikt.

Oracle-gegevenstype | .NET framework-gegevenstype
---------------- | ------------------------
BBESTAND | Byte]
BLOB | Byte]
TEKEN | Tekenreeks
CLOB | Tekenreeks
DATUM | Datum /
LATEN ZWEVEN | Decimaal
GEHEEL GETAL | Decimaal
INTERVAL JAAR-TOT-MAAND | Int32
INTERVAL DAG TWEEDE | Tijdspanne
LANGE | Tekenreeks
LANG ONBEWERKTE | Byte]
NCHAR | Tekenreeks
NCLOB | Tekenreeks
GETAL | Decimaal
NVARCHAR2 | Tekenreeks
ONBEWERKTE | Byte]
RIJ-ID | Tekenreeks
TIJDSTEMPEL | Datum /
TIJDSTEMPEL MET LOKALE TIJDZONE | Datum /
TIJDSTEMPEL MET TIJDZONE | Datum /
GEHEEL GETAL ZONDER VOORTEKEN | Getal
VARCHAR2 | Tekenreeks
XML | Tekenreeks

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

**Probleem:** U ziet het volgende **foutbericht**: kopie activiteit voldaan ongeldige parameters: UnknownParameterName, gedetailleerde bericht: niet vinden van de aangevraagde .net Framework-gegevensprovider. Dit kan niet worden geïnstalleerd'.  

**Mogelijke oorzaken:**

1. De .NET Framework-gegevensprovider voor Oracle is niet geïnstalleerd.
2. De .NET Framework-gegevensprovider voor Oracle .NET Framework 2.0 is geïnstalleerd en is niet gevonden in de mappen .NET Framework 4.0. 

**Resolutie/tijdelijke oplossing:**

1. Als u de .NET-Provider voor Oracle, [Installeer deze](http://www.oracle.com/technetwork/topics/dotnet/downloads/) nog niet hebt geïnstalleerd en probeer het scenario. 
2. Als u het foutbericht zelfs na de installatie van de provider, volgt u de volgende stappen uit: 
    1. Machine config van .NET 2.0 openen vanuit de map: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Zoeken voor **Oracle-gegevensprovider voor .NET**en u moet kunnen vinden van een vermelding zoals weergegeven in het volgende voorbeeld unwn in het volgende voorbeeld under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Dit item kopiëren naar het bestand machine.config in de volgende v4.0 map: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config en pas de versie moet 4.xxx.x.x.
3.  '< ODP.NET geïnstalleerde pad > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll' in de globale constructie-cache (GAC) installeren door te voeren `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.
