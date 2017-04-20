<properties 
    pageTitle="Gegevens verplaatsen naar/van Azure tabel | Microsoft Azure" 
    description="Leer hoe u de gegevens van Azure Table Storage met Azure gegevens Factory verplaatsen." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Gegevens verplaatsen naar en vanuit Azure-tabel met Azure gegevens Factory

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen naar/van Azure tabel van/naar een andere gegevensopslag. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens en ondersteunde store combinaties met kopie activiteit wordt weergegeven.

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die wordt gekopieerd gegevens van Azure Table Storage is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

De volgende voorbeelden bieden definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Gegevens kopiëren naar en vanuit Azure Table Storage en Azure Blob Database worden ze weergegeven. Gegevens kan echter gekopieerde **rechtstreeks** vanaf een van de bronnen aan de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Voorbeeld: Gegevens van Azure tabel kopiëren naar Azure Blob

Het volgende voorbeeld ziet:

1.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (gebruikt voor zowel tabel & blob).
2.  Een invoer [dataset](data-factory-create-datasets.md) van het type [Azure Table](#azure-table-dataset-type-properties).
3.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  De [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [AzureTableSource](#azure-table-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

De steekproef kopieert gegevens die deel uitmaken van de standaard-partition in een Azure-tabel naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen.

**Azure gekoppeld opslagservice:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure gegevens Factory ondersteunt twee typen opslag van Azure gekoppeld services: **AzureStorage** en **AzureStorageSas**. Voor de eerste fase, geeft u de verbindingsreeks waarin de account-toets en voor de later, die u opgeeft de Uri gedeeld Access handtekening (SA's). Zie de sectie van de [Gekoppelde Services](#linked-services) voor meer informatie.  

**Azure tabel invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel "MijnTabel" in Azure-tabel hebt gemaakt.
 
Instellen "externe": "true" melding aan de Data Factory-service dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

**Azure Blob uitvoer gegevensset:**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd. 

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

**Pijplijn met de activiteit kopiëren:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **AzureTableSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven met de eigenschap **AzureTableSourceQuery** Hiermee selecteert u de gegevens van de standaard-partition per uur om te kopiëren.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Voorbeeld: Gegevens van Azure Blob naar Azure tabel kopiëren

Het volgende voorbeeld ziet:

1.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (gebruikt voor zowel tabel & blob)
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [Azure Table](#azure-table-dataset-type-properties). 
4.  De [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) en [AzureTableSink](#azure-table-copy-activity-type-properties). 


De kopieën van de steekproef tijdreeks gegevens uit een Azure blob naar een Azure tabel per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen.

**Azure (voor zowel Azure tabel & Blob) gekoppeld opslagservice:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure gegevens Factory ondersteunt twee typen opslag van Azure gekoppeld services: **AzureStorage** en **AzureStorageSas**. Voor de eerste fase, geeft u de verbindingsreeks waarin de account-toets en voor de later, die u opgeeft de Uri gedeeld Access handtekening (SA's). Zie de sectie van de [Gekoppelde Services](#linked-services) voor meer informatie. 

**Azure Blob invoer gegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob per uur (frequentie: uur, interval: 1). De map pad en de bestandsnaam voor het blob worden dynamisch geëvalueerd op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map beschikt over jaar, maand en dag deel uit van de begintijd en de bestandsnaam van het wordt gebruikt voor het uur deel van de begintijd. "externe": "true" instelling de gegevens Factory-service wordt gemeld dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.
    
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

**Azure tabel uitvoer gegevensset:**

De steekproef worden gegevens naar een tabel "MijnTabel ' in de tabel Azure gekopieerd. Zoals u verwacht dat de Blob CSV-bestand bevat, kunt u een Azure-tabel maken met hetzelfde aantal kolommen. Nieuwe rijen worden toegevoegd aan de tabel per uur. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pijplijn met de activiteit kopiëren:**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **BlobSource** en **sink** type is ingesteld op **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Gekoppelde Services
Er zijn twee soorten gekoppelde services die u kunt een Azure-blobopslag koppelen aan een factory Azure-gegevens. Ze zijn: **AzureStorage** gekoppeld service en **AzureStorageSas** gekoppeld-service. De opslag van Azure gekoppeld-service biedt de fabriek gegevens met globale toegang naar de opslag van de Azure. Dat het Azure opslag SA's (gedeeld Access handtekening) gekoppeld biedt de service de fabriek gegevens met beperkte/tijd-gebonden toegang voor de opslag van Azure. Er zijn geen andere verschillen tussen deze twee gekoppelde services. Kies de gekoppelde service die aan uw behoeften voldoet. De volgende secties vindt u meer informatie over deze twee gekoppelde services.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure tabel gegevensset type eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie **typeProperties** voor de gegevensset van het type **Azure Table** heeft de volgende eigenschappen.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | De naam van de tabel in de tabel Azure-Database-instantie waarmee gekoppelde service naar verwijst. | Ja. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |

### <a name="schema-by-data-factory"></a>Schema door gegevens Factory
Voor gegevens schema winkels zoals Azure tabel afleidt de gegevens Factory-service het schema in een van de volgende manieren:

1.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de gegevensset opgeeft, wordt in de gegevens Factory-service deze structuur respecteert als het schema. In dit geval als een rij niet een waarde voor een kolom bevat, is een null-waarde opgegeven voor dit.
2. Als u niet de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de gegevensset opgeeft, afleidt Data Factory het schema met behulp van de eerste rij in de gegevens. Als de eerste rij geen het volledige schema bevat, worden sommige kolommen in dit geval gemist in het resultaat kopiëren.

Daarom voor schema-vrije gegevensbronnen is de beste manier is om op te geven van de structuur van gegevens met behulp van de eigenschap **structuur** .

## <a name="azure-table-copy-activity-type-properties"></a>Azure tabel kopiëren activiteit type eigenschappen

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

**AzureTableSource** ondersteunt de volgende eigenschappen in de sectie typeProperties:

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | De aangepaste query gebruiken om gegevens te lezen. | Azure tabel queryreeks. Zie de voorbeelden in het volgende gedeelte. | Nee. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming.
azureTableSourceIgnoreTableNotFound | Aangeven of slikken uitzondering van de tabel niet bestaat. | WAAR<br/>ONWAAR | Nee |

### <a name="azuretablesourcequery-examples"></a>Voorbeelden van azureTableSourceQuery

Als Azure tabelkolom van het tekenreekstype is: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Als Azure tabelkolom van datum-/ type: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** ondersteunt de volgende eigenschappen in de sectie typeProperties:


Eigenschap | Beschrijving | Toegestane waarden | Vereist  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Standaard partition sleutelwaarde die kan worden gebruikt door de sink. | Een tekenreekswaarde. | Nee 
azureTablePartitionKeyName | Geef de naam van de kolom waarvan u de waarden worden gebruikt als partition toetsen. Als u niet opgeeft, wordt AzureTableDefaultPartitionKeyValue gebruikt als de partitiesleutel. | De naam van een kolom. | Nee |
azureTableRowKeyName | Geef de naam van de kolom waarvan kolomwaarden worden gebruikt als rijsleutel. Als niet opgeeft, gebruikt u een GUID voor elke rij. | De naam van een kolom. | Nee  
azureTableInsertType | De modus naar gegevens in Azure tabel invoegen.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met identieke partition en rij toetsen hun waarden vervangen of samengevoegd hebben. <br/><br/>Zie voor meer informatie over de werking van deze instellingen (samenvoegen en vervangen), [invoegen of entiteit samenvoegen](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of entiteit vervangen](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. <br/><br> Deze instelling is van toepassing op het rijniveau van de, niet de tabelniveau en geen van beide optie verwijdert rijen in de uitvoertabel die niet in de invoer voorkomen. | samenvoegen (standaard)<br/>vervangen | Nee 
writeBatchSize | Hiermee voegt u gegevens in de tabel Azure wanneer de writeBatchSize of writeBatchTimeout is bereikt. | Geheel getal (aantal rijen)| Niet (standaard: 10000) 
writeBatchTimeout | Hiermee voegt u gegevens in de tabel Azure wanneer de writeBatchSize of writeBatchTimeout is bereikt | tijdspanne<br/><br/>Voorbeeld: "00: 20:00" (20 minuten) | Niet (standaard opslag client standaard time-out waarde 90 sec)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Een bronkolom toewijzen aan een doelkolom met de Minivertaler JSON eigenschap voordat u de doelkolom als de azureTablePartitionKeyName gebruiken kunt.

In het volgende voorbeeld, bronkolom DivisionID is toegewezen aan de doelkolom: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

De DivisionID is opgegeven als de partitiesleutel. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>De toewijzing van het type voor Azure tabel

Zoals vermeld in het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , kunt u automatische typeconversies van de typen gegevensbronnen om op te vangen typen met de volgende twee stappen benadering kopie activiteit uitvoeren.

1. Van systeemeigen gegevensbronnen van het type converteren naar .NET-type
2. Van .NET type converteren naar systeemeigen sink type

Wanneer u gegevens verplaatst naar & van Azure-tabel, worden de volgende [toewijzingen gedefinieerd door de service Azure-tabel](https://msdn.microsoft.com/library/azure/dd179338.aspx) uit een OData-Azure tabel grafiektypen in .NET-type en vice versa gebruikt. 

| OData-gegevenstype | .NET-type | Meer informatie |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | Een reeks bytes maximaal 64 KB. |
| Edm.Boolean | BOOL | Een Booleaanse waarde. |
| Edm.DateTime | Datum / | Een 64-bits waarde uitgedrukt als Coordinated Universal Time (UTC). De ondersteunde DateTime-bereik begint met 12:00 uur januari 1, 1601 N.C. (C. E.), UTC. Het bereik eindigt op 31 December 9999. |
| Edm.Double | dubbele | Een van 64-bits zwevende puntwaarde. |
| Edm.Guid | GUID | Een 128-bit globaal unieke id. |
| Edm.Int32 | Int32 of int | Een 32-bits geheel getal. |
| Edm.Int64 | Int64 of een lang | Een 64-bits geheel getal. |
| Edm.String | Tekenreeks | Een waarde UTF-16-codering. Tekenreekswaarden mogelijk maximaal 64 KB zijn. |

### <a name="type-conversion-sample"></a>Type conversie steekproef

In het onderstaande voorbeeld is voor het kopiëren van gegevens uit een Azure Blob aan Azure-tabel met typeconversies. 

Stel de gegevensset Blob is in een CSV-indeling en drie kolommen bevat. Een van deze is een datetime-kolom met een aangepaste datetime-notatie met Franse afkortingen voor dag van de week. 

Definieer de gegevensset Blob bron als volgt samen met de definities voor afhankelijkheidstype voor de kolommen.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

De toewijzing van het type van Azure tabel OData type aan .NET type doorgegeven, zou u de tabel definiëren in Azure-tabel met het volgende schema. 

**Schema van Azure tabel:**

Kolomnaam | Type
----------- | --------
gebruikers-id | Edm.Int64
naam | Edm.String 
lastlogindate | Edm.DateTime

Definieer vervolgens als volgt de gegevensset Azure-tabel. U hoeft niet te "structuur" sectie met de gegevens opgeven, omdat de gegevens al is opgegeven in de onderliggende gegevensopslag.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

In dit geval gegevens Factory automatisch gegevenstypeconversies met inbegrip van het datum/tijd-veld met de aangepaste datetime-notatie de cultuur "Frans-nl" gebruiken bij het verplaatsen van gegevens uit Blob naar Azure-tabel.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren, Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md).







