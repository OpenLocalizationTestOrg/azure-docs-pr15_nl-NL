<properties
    pageTitle="Plannings- en uit te voeren met gegevens Factory | Microsoft Azure"
    description="Informatie over planning en uitvoering aspecten van Azure gegevens Factory-toepassingsmodel."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Gegevens Factory plannen en uitvoeren
In dit artikel wordt uitgelegd dat de plannings- en execution aspecten van het model Azure gegevens Factory-toepassing. 

## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u de basisbeginselen van gegevens Factory-toepassing model concepten, met inbegrip van de activiteit, pijpleidingen gekoppelde services en gegevenssets kennen. Zie de volgende artikelen voor basisconcepten van Azure gegevens Factory:

- [Inleiding tot Data Factory](data-factory-introduction.md)
- [Pijpleidingen](data-factory-create-pipelines.md)
- [Gegevenssets](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Een activiteit plannen

Met de sectie scheduler van de activiteit JSON, kunt u een terugkerende planning voor een activiteit opgeven. Bijvoorbeeld, kunt u een activiteit plannen per uur als volgt:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Voorbeeld van scheduler](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Zoals u in het diagram, geven een planning voor de activiteit Hiermee maakt u een reeks gewor windows. Gewor windows zijn een reeks vaste grootte, niet-overlappende, aaneengesloten tijdsintervallen. Deze vensters logische gewor voor de activiteit heten *activiteit windows*.

U kunt het tijdsinterval die is gekoppeld aan het activiteitvenster met [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) en [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) variabelen in de activiteit JSON openen voor het activiteitvenster uitgevoerd. U kunt deze variabelen voor verschillende doeleinden gebruiken in uw activiteit JSON. Bijvoorbeeld, kunt u deze gebruiken om gegevens van de invoer- en uitvoerbereik gegevenssets dat staat voor time reeksgegevens te selecteren.

De eigenschap **scheduler** ondersteunt de dezelfde subeigenschappen als de eigenschap **beschikbaarheid** in een gegevensset. Zie [de beschikbaarheid van de gegevensset](data-factory-create-datasets.md#Availability) voor meer informatie. Voorbeelden: plannen op een specifieke tijdverschil of het instellen van de modus verwerking aan het begin of einde van het interval voor het activiteitvenster uitlijnen.

U kunt **scheduler** eigenschappen opgeven voor een activiteit, maar deze eigenschap is **optioneel**. Als u een eigenschap opgeeft, moet deze overeenkomen met de cadence die u in de definitie van de gegevensset uitvoer opgeeft. Uitvoer gegevensset is momenteel wat stations het schema, zodat u kunt een gegevensset uitvoer maken moet, zelfs als de activiteit geen uitvoer levert. Als de activiteit niet wordt elke invoer duren, kunt u doorgaan met het maken van de invoer gegevensset.

## <a name="time-series-datasets-and-data-slices"></a>Tijd reeks gegevenssets en gegevens segmenten

Reeks tijdgegevens is een doorlopend reeks gegevenspunten die meestal uit opeenvolgende metingen via een tijdsinterval bestaat. Algemene voorbeelden van reeksen tijdgegevens zijn sensorgegevens en telemetriegegevens van toepassing.

Met gegevens Factory, kunt u tijd reeksgegevens in een batch manier met activiteit wordt uitgevoerd verwerken. Er is meestal een terugkerende cadence waarop invoergegevens binnenkomt en uitvoer van gegevens moet worden geproduceerd. Deze cadence is gebaseerd op door het opgeven van **beschikbaarheid** in de gegevensset als volgt:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Elke eenheid gegevens verbruikt en geproduceerd door het uitvoeren van een activiteit is een segment gegevens genoemd. In het volgende diagram ziet u een voorbeeld van een activiteit met één invoer gegevensset en een gegevensset weergeven. Deze gegevenssets hebben **beschikbaarheid** ingesteld op een per uur frequentie.

![Beschikbaarheid scheduler](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Het diagram ziet u de per uur segmenten van de gegevens voor de invoer- en uitvoerbereik gegevensset. Het diagram ziet u drie invoer segmenten die gereed voor de verwerking zijn. De activiteit 10-11 AM wordt uitgevoerd, het segment van de uitvoer 10-11 AM produceren.

U kunt het tijdsinterval die is gekoppeld aan het huidige segment worden geproduceerd in de gegevensset JSON met variabelen [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) en [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)openen.

Op dit moment gegevens Factory is vereist dat de planning die is opgegeven in de activiteit exact overeenkomt met de opgegeven in de **beschikbaarheid** van de gegevensset uitvoer planning. Daarom **WindowStart**, **WindowEnd** **SliceStart**en **SliceEnd** altijd toegewezen aan dezelfde periode en een segment één uitvoer.

Zie voor meer informatie over andere eigenschappen die beschikbaar zijn voor de sectie beschikbaarheid, [gegevenssets maken](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Verplaats gegevens van de SQL-Database naar Blob storage

Laten we eens enkele dingen die u bij elkaar en de zetten in actie door te maken van een pijplijn die opgehaald van gegevens uit een Azure SQL Database-tabel met Azure-blobopslag per uur.

**Invoer: Azure SQL-Database gegevensset**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**De frequentie** is ingesteld op **uur** en **interval** is ingesteld op **1** in de sectie mogelijkheid.

**Uitvoer: Azure Blob storage gegevensset**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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


**De frequentie** is ingesteld op **uur** en **interval** is ingesteld op **1** in de sectie mogelijkheid.



**Activiteit: Kopie activiteit**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Het voorbeeld ziet u de activiteitenplanning en gegevensset beschikbaarheid secties ingesteld op een per uur frequentie. Het voorbeeld ziet u hoe kunt u **WindowStart** en **WindowEnd** om relevante gegevens voor een activiteit te selecteren, uitvoeren en kopieer deze naar een blob met de juiste **mappad**. De **mappad** met parameters om een aparte map voor elk uur.

Als drie segmenten tussen 8 – 11 AM uitvoert, is de gegevens in Azure SQL-Database als volgt:

![De invoer van de steekproef](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Nadat de pijplijn implementeert, wordt als volgt de Azure blob gevuld:

-   Bestand mypath/2015/1/1/8/gegevens. &lt;Guid&gt;.txt met gegevens

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;GUID&gt; wordt vervangen door een werkelijke guid. Voorbeeld bestandsnaam: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Bestand mypath/2015/1/1/9/gegevens. &lt;Guid&gt;.txt met gegevens:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Bestand mypath/2015/1/1/10/gegevens. &lt;Guid&gt;.txt zonder gegevens.


## <a name="active-period-for-pipeline"></a>Actieve periode voor verkooppijplijn

Het concept van een actieve periode voor een opgegeven door het instellen van de eigenschappen van het **begin** en **einde** pijplijn [voor het maken van pijpleidingen](data-factory-create-pipelines.md) worden geïntroduceerd.

U kunt de begindatum voor de verkooppijplijn actieve periode instellen in het verleden. Gegevens Factory automatisch (achterste opvullingen) van alle gegevens segmenten in het verleden berekent en ze worden verwerkt.

## <a name="parallel-processing-of-data-slices"></a>Parallelle verwerking van gegevens segmenten
U kunt gegevens terug gevulde segmenten parallel worden uitgevoerd door de eigenschap **bij gelijktijdigheid** in de beleidssectie van de activiteit JSON configureren. Zie voor meer informatie over deze eigenschap, [pijpleidingen maken](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Opnieuw een segment mislukte gegevens uitvoeren 
U kunt de uitvoering van segmenten in een volwaardige, visuele manier controleren. Zie [controle- en beheren van pijpleidingen met Azure portal bladen](data-factory-monitor-manage-pipelines.md) of [controle en beheer-app](data-factory-monitor-manage-app.md) voor meer informatie.

Houd rekening met het volgende voorbeeld, waarin twee activiteiten. Activity1 levert een gegevensset tijd reeks met segmenten als uitvoer die wordt gebruikt als invoer door Activity2 tot de uiteindelijke uitvoer tijd reeks gegevensset.

![Mislukte segment](./media/data-factory-scheduling-and-execution/failed-slice.png)

Het diagram ziet die uit drie recente segmenten, er is een fout opgetreden bij het maken van het segment 9-10 AM voor Dataset2. Gegevens Factory afhankelijkheid voor de tijd reeks gegevensset automatisch worden bijgehouden. Hierdoor wordt de activiteit uitvoeren voor het volgende segment 9-10: 00 niet worden gestart.

Factory voor controle en beheer hulpmiddelen voor gegevens kunnen u inzoomen op de diagnostische logboeken voor het mislukte segment naar eenvoudig vinden van de onderliggende oorzaak voor het probleem en op te lossen. Nadat u het probleem opgelost hebt, kunt u gemakkelijk de activiteit uitvoeren om te produceren het mislukte segment starten. Zie voor meer informatie over het opnieuw uitvoeren en staat overgangen voor gegevens segmenten begrijpen [controle- en beheren van pijpleidingen met Azure portal bladen](data-factory-monitor-manage-pipelines.md) of [controle en beheer app](data-factory-monitor-manage-app.md).

Nadat u het segment 9-10: 00 voor **Dataset2**herhalen, begint Data Factory de uitvoeren voor de afhankelijke cirkelsegment 9-10 AM op de uiteindelijke gegevensset.

![Mislukte segment opnieuw uitvoeren](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Activiteiten uitvoeren in een reeks weergeven
U kunt twee activiteiten (één activiteit wordt uitgevoerd na de andere) koppelen door in te stellen van de gegevensset uitvoer van de ene activiteit als de invoer gegevensset van de andere activiteit. De activiteiten kunnen werken in de pijplijn dezelfde of verschillende pijpleidingen. De tweede activiteit wordt uitgevoerd alleen wanneer de eerste fase is voltooid.

Houd rekening met het volgende geval bijvoorbeeld:

1.  Verkooppijplijn P1 heeft activiteit A1, dat is vereist externe invoer gegevensset D1 en genereert uitvoer gegevensset D2.
2.  Verkooppijplijn P2 heeft activiteit A2 die is invoer van de gegevensset D2 en genereert uitvoer gegevensset D3.

In dit scenario wordt activiteiten A1 en A2 zijn in verschillende pijpleidingen. De activiteit A1 wordt uitgevoerd wanneer de externe gegevens beschikbaar is en de beschikbaarheid van de geplande frequentie is bereikt. De activiteit A2 wordt uitgevoerd wanneer de geplande segmenten uit D2 beschikbaar en de beschikbaarheid van de geplande frequentie is bereikt. Als er een fout in een van de segmenten in gegevensset D2, wordt niet A2 uitgevoerd voor segment totdat deze verandert in beschikbaar.

De diagramweergave eruit als in het volgende diagram:

![Activiteiten in twee pijpleidingen koppelen](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Zoals eerder is vermeld, kunnen de activiteiten in de dezelfde pijplijn zijn. De diagramweergave met beide activiteiten in de dezelfde pijplijn eruit als in het volgende diagram:

![Activiteiten in de dezelfde pijplijn koppelen](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Opeenvolging kopiëren
Het is mogelijk meerdere kopieerbewerkingen achter elkaar uitvoeren op een wijze opeenvolgende/besteld. Stel, hebt u twee kopie activiteiten in een pijplijn (CopyActivity1 en CopyActivity2) met de volgende invoergegevens uitvoer gegevenssets:   

CopyActivity1

Invoer: gegevensset. Uitvoer: Dataset2.

CopyActivity2

Invoer: Dataset2.  Uitvoer: Dataset3.

CopyActivity2 gebeurt alleen als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is.

Hier ziet u de steekproef pijplijn JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Zoals u ziet dat de gegevensset uitvoer van de eerste kopie activiteit (Dataset2) in het voorbeeld is opgegeven als invoer voor de tweede activiteit. De tweede activiteit wordt daarom uitgevoerd alleen wanneer de gegevensset uitvoer van de eerste activiteit klaar is.  

In het voorbeeld CopyActivity2 kan hebben een andere invoer, zoals Dataset3, maar u Dataset2 opgeven als invoer voor CopyActivity2, zodat de activiteit wordt niet uitgevoerd totdat CopyActivity1 is voltooid. Bijvoorbeeld:

CopyActivity1

Invoer: Dataset1. Uitvoer: Dataset2.

CopyActivity2

Ingangen: Dataset3, Dataset2. Uitvoer: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Zoals u ziet dat de twee invoer gegevenssets in het voorbeeld zijn opgegeven voor de tweede kopie activiteit. Als meerdere ingangen zijn opgegeven, wordt alleen de eerste invoer gegevensset wordt gebruikt voor het kopiëren van gegevens, maar andere gegevenssets worden gebruikt als afhankelijkheden. CopyActivity2 zou begint pas nadat u de volgende voorwaarden wordt voldaan:

- CopyActivity1 is voltooid en Dataset2 is beschikbaar. Deze dataset wordt niet gebruikt wanneer u gegevens kopieert naar Dataset4. Deze alleen fungeert als een planning afhankelijkheid voor CopyActivity2.   
- Dataset3 is beschikbaar. Deze dataset Hiermee geeft u de gegevens die wordt gekopieerd naar de bestemming.  



## <a name="model-datasets-with-different-frequencies"></a>Model gegevenssets frequentie

In de voorbeelden zijn de frequentie voor invoer- en uitvoerbereik gegevenssets en het venster van de planning activiteit hetzelfde. Sommige scenario's moet de mogelijkheid om de uitvoer met een anders dan de frequentie van een of meer invoeritems frequentie te produceren. Gegevens Factory ondersteunt modeling deze scenario's.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Voorbeeld 1: Een dagelijkse uitvoer-rapport voor invoergegevens die beschikbaar is per uur produceren

Bekijk een scenario waarin u hebt ingevoerd maateenheden gegevens van sensoren beschikbaar per uur in Azure-blobopslag. U wilt een dagelijkse statistische rapport met statistieken zoals gemiddelde, maximum en minimum voor de dag met [gegevens Factory component activiteit](data-factory-hive-activity.md)produceren.

Hier ziet u hoe u dit scenario met gegevens Factory aan het model kunt:

**Invoer gegevensset**

De per uur invoer bestanden worden niet weergegeven in de map voor de opgegeven dag. Beschikbaarheid om invoer bij **uur** is ingesteld (frequentie: uur, interval: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Uitvoer gegevensset**

Eén uitvoerbestand gemaakt elke dag in de map van de dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: dag en interval: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Activiteit: component activiteit in een pijplijn**

Het script component ontvangt over de vereiste gegevens voor *datum-/* als parameters die de variabele **WindowStart** gebruiken, zoals wordt weergegeven in het volgende fragment. Het script component gebruikt deze variabele laden van de gegevens uit de juiste map voor de dag en de aggregatie om te genereren van de uitvoer uitvoeren.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

In het volgende diagram ziet u het scenario vanuit het oogpunt van een gegevens-afhankelijkheid.

![Gegevensafhankelijkheid](./media/data-factory-scheduling-and-execution/data-dependency.png)

Het segment uitvoer voor elke dag, is afhankelijk van 24 uur segmenten uit een invoer gegevensset. Gegevens Factory berekent de afhankelijkheden automatisch door de invoergegevens uitzoeken met segmenten die binnen dezelfde periode als het segment uitvoer moet worden geproduceerd. Als een van de 24 invoer segmenten niet beschikbaar is, wacht Factory van gegevens voor de invoer segment moet klaar zijn voordat u begint met het dagelijkse werkzaamheden uitvoeren.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Voorbeeld 2: Geef afhankelijkheid met expressies en gegevens Factory-functies

Laten we eens een ander scenario. Stel dat u hebt een activiteit in een component waarmee twee invoer gegevenssets worden verwerkt. Een van deze nieuwe gegevens dagelijks heeft, maar een van deze nieuwe gegevens wordt elke week. Stel dat u wilt doen van een join in de twee invoeren en het verkrijgen van een uitvoer elke dag.

De eenvoudige benadering in welke Factory gegevens automatisch afbeeldingen af rechts invoeren segmenten om te verwerken met uitlijnen op de uitvoer van gegevens segment tijd periode niet werkt.

U moet opgeven dat voor elke activiteit uitvoeren, de fabriek gegevens van de laatste week gegevens segment voor de wekelijkse invoer gegevensset gebruiken moet. U Azure gegevens Factory-functies zoals wordt weergegeven in het volgende fragment willen implementeren van dit probleem.

**Input1: Azure blob**

De eerste invoer is de Azure blob dagelijks worden bijgewerkt.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure blob**

Input2 is de Azure blob wekelijkse worden bijgewerkt.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Uitvoer: Azure blob**

Eén uitvoerbestand is elke dag gemaakt in de map voor de dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: dag, interval: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Activiteit: component activiteit in een pijplijn**

De activiteit component wordt op de twee invoeren en genereert een segment uitvoer elke dag. Van elke dag uitvoer segment afhankelijk van de vorige week invoer segment om wekelijkse invoer als volgt, kunt u opgeven.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Gegevens Factory-functies en variabelen   

Zie [Data Factory-functies en variabelen](data-factory-functions-variables.md) voor een lijst met functies en variabelen die ondersteuning biedt voor gegevens Factory.

## <a name="data-dependency-deep-dive"></a>Gegevens afhankelijkheid uitgebreide Kennismaking

Als u wilt een gegevensset segment door het uitvoeren van een activiteit te genereren, worden gegevens Factory het volgende *afhankelijkheid model* gebruikt om te bepalen de relaties tussen gegevenssets verbruikt en geproduceerd door een activiteit.

Het tijdsbereik van de invoer gegevenssets vereist voor het genereren van het segment van de gegevensset uitvoer heet de *afhankelijkheid termijn*.

Een activiteit uitvoeren genereert een gegevensset segment alleen nadat de gegevens segmenten in invoer gegevenssets binnen de periode afhankelijkheid beschikbaar zijn. Alle invoer segmenten die bestaat uit de periode afhankelijkheid in **Gereed voor de activiteit** moeten zijn uitgevoerd met andere woorden, als u wilt een segment van de gegevensset uitvoer produceren.

Als u wilt het segment gegevensset [**begin**, **einde**] genereren, moet een functie het segment gegevensset toewijzen aan de periode afhankelijkheid. Deze functie is in principe een formule waarmee het begin en einde van het segment gegevensset naar het begin en einde van de termijn afhankelijkheid converteert. Meer formeel:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** en **g** zijn functies die het begin en einde van de termijn afhankelijkheid voor elke activiteit input berekenen toewijzen.

Zoals gezien in voorbeelden, is de periode afhankelijkheid doet u hetzelfde als de periode voor het segment dat wordt gegenereerd. In dat geval berekent gegevens Factory automatisch de invoer segmenten die in de periode afhankelijkheid vallen.  

In de steekproef aggregatie waar uitvoer dagelijks geproduceerd en invoergegevens per uur beschikbaar is, is de geselecteerde segment bijvoorbeeld 24 uur. Gegevens Factory vindt u de relevante per uur invoer segmenten voor deze periode en maakt u het segment uitvoer afhankelijk van de invoer segment.

U kunt ook aan te geven om uw eigen toewijzing voor de periode afhankelijkheid zoals wordt weergegeven in het voorbeeld, waarin een van de invoer wekelijks en het segment uitvoer is dagelijks is geproduceerd.

## <a name="data-dependency-and-validation"></a>Gegevensafhankelijkheid en gegevensvalidatie

Een gegevensset kan een beleid van gegevensvalidatie is gedefinieerd waarin wordt aangegeven hoe de gegevens die zijn gegenereerd door een uitvoering segment kunnen worden gevalideerd voordat deze gebruiksklare is hebben. Zie [gegevenssets maken](data-factory-create-datasets.md) voor meer informatie.

In dat geval nadat het segment is uitgevoerd, wordt de uitvoer segment status gewijzigd in **afwachting van** met een substatus van **gegevensvalidatie**. Nadat de segmenten worden gevalideerd, verandert de status segment op **Gereed**.

Als een segment gegevens is geproduceerd, maar zijn niet worden gevalideerd, worden de activiteit wordt uitgevoerd voor volgende segmenten die afhankelijk van dit segment zijn niet verwerkt.

[Beeldscherm en beheren van pijpleidingen](data-factory-monitor-manage-pipelines.md) behandelt de verschillende Staten van gegevens segmenten in Data Factory.

## <a name="external-data"></a>Externe gegevens

Een gegevensset kan worden gemarkeerd als externe (zoals weergegeven in het volgende JSON-fragment), wat dat niet is gegenereerd met gegevens Factory. In dat geval kan het beleid gegevensset hebben een extra set met parameters die met een beschrijving van validatieregels en probeer opnieuw beleid voor de gegevensset. Zie [pijpleidingen maken](data-factory-create-pipelines.md) voor een beschrijving van alle eigenschappen.

Vergelijkbaar met gegevenssets die worden geproduceerd door gegevens Factory, gegevens segmenten voor externe gegevens moeten klaar zijn voordat afhankelijke segmenten kunnen worden verwerkt.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Eenmalige verkooppijplijn
U kunt maken en plannen van een pijplijn periodiek wordt uitgevoerd (bijvoorbeeld: per uur of dagelijks) binnen de begin- en eindtijden die u in de verkooppijplijn definitie opgeeft. Zie [planning activiteiten](#scheduling-and-execution) voor meer informatie. U kunt ook een pijplijn die wordt uitgevoerd slechts één keer maken. Dit doet u de eigenschap **pipelineMode** instellen in de definitie van het verkooppijplijn wilt **eenmalige** zoals wordt weergegeven in het onderstaande voorbeeld van JSON. De standaardwaarde voor deze eigenschap is **gepland**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Houd rekening met het volgende:

- ** **Begin** - en** eindtijden voor de pijplijn zijn niet opgegeven.
- **Beschikbaarheid** van de invoer- en uitvoerbereik gegevenssets is opgegeven (**frequentie** en **interval**), maar wel Data Factory geen van de waarden gebruikmaakt.  
- Diagramweergave weergegeven eenmalige pijpleidingen niet. Dit probleem is inherent aan het ontwerp.
- Eenmalige pijpleidingen kunnen niet worden bijgewerkt. U kunt een eenmalige pijplijn klonen, wijzig de naam eigenschappen bijwerken en implementeren om een ander postvakbeleid maken.
