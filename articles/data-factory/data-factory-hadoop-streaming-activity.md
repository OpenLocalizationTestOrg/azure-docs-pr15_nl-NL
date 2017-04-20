<properties 
    pageTitle="Hadoop Streaming activiteit" 
    description="Leer hoe u kunt de Hadoop Streaming activiteit in een fabriek Azure gegevens Hadoop Streaming programma's uitvoert op een op aanvraag/uw eigen HDInsight cluster." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming activiteit
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

U kunt de activiteit HDInsightStreamingActivity roept een taak Hadoop Streaming uit een verkooppijplijn Factory van Azure-gegevens. Het codefragment van de volgende JSON ziet de syntaxis van de voor het gebruik van de HDInsightStreamingActivity in een verkooppijplijn JSON-bestand. 

De HDInsight Streaming activiteit in een Data Factory- [verkooppijplijn](data-factory-create-pipelines.md) voert Hadoop Streaming-programma's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cluster van Windows, Linux-gebaseerde HDInsight. In dit artikel is gebaseerd op het artikel [gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) , waarbij een een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatie activiteiten wordt weergegeven.

## <a name="json-sample"></a>Voorbeeld van JSON
Het cluster HDInsight wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Standaard is de naam van de container die wordt gebruikt door het cluster HDInsight de naam van het cluster zelf. Bijvoorbeeld als de clusternaam van uw myhdicluster is, zou de naam van de blob container die is gekoppeld myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Houd rekening met de volgende punten:

1. Stel de **linkedServiceName** op de naam van de gekoppelde service die verwijst naar uw HDInsight cluster waarop de streaming mapreduce taak wordt uitgevoerd.
2. Stel het type van de activiteit op **HDInsightStreaming**.
3. Geef de naam van de toewijzing uitvoerbare voor de eigenschap **toewijzing** . In het voorbeeld is cat.exe de uitvoerbare-toewijzing.
4. Geef de naam van reducer uitvoerbare voor de eigenschap **reducer** . In het voorbeeld is wc.exe het uitvoerbare reducer.
5. Geef het invoer-bestand (inclusief de locatie) voor de toewijzing voor de eigenschap **input** type. In het voorbeeld: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample is de container blob, voorbeeld/gegevens/Gutenberg is de map en davinci.txt is de blob.
6. Geef het uitvoerbestand (inclusief de locatie) voor de reducer voor de eigenschap van het type **uitvoer** . De uitvoer van de taak Hadoop Streaming is geschreven naar de locatie die is opgegeven voor deze eigenschap.
7. Geef de paden voor de toewijzing en reducer uitvoerbare bestanden in de sectie **filePaths** . In het voorbeeld: "adfsample/example/apps/wc.exe", adfsample is de container blob, voorbeeld/apps is de map en wc.exe is het uitvoerbare bestand.
8. Geef de Azure gekoppeld opslagservice die aangeeft van de Azure opslag met de bestanden die zijn opgegeven in de sectie filePaths voor de eigenschap **fileLinkedService** .
9. Geef de argumenten voor de streaming taak voor de eigenschap **argumenten** .
10. De eigenschap **getDebugInfo** is een optioneel element. Als er fout is ingesteld, wordt de logboeken worden gedownload alleen op mislukt. Als deze is ingesteld op alle, worden altijd logboeken gedownload ongeacht de status kan worden uitgevoerd.

> [AZURE.NOTE] Zoals u in het voorbeeld, kunt u een gegevensset uitvoer opgeven voor de Hadoop Streaming activiteit voor de eigenschap **uitvoer** . Deze dataset is alleen een pop gegevensset dat nodig is voor de verkooppijplijn planning wordt aangestuurd. U hoeft niet te geven van een invoer gegevensset voor de activiteit voor de eigenschap **invoeritems** .  

    
## <a name="example"></a>Voorbeeld
De pijplijn in dit scenario wordt het aantal woorden streaming kaart/verkleinen programma op uw cluster Azure HDInsight uitgevoerd. 

### <a name="linked-services"></a>Gekoppelde services

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice
U maakt eerst een gekoppelde service om een koppeling van de Azure-opslag die wordt gebruikt door het cluster Azure HDInsight fabriek Azure-gegevens. Vergeet niet te vervangen door de naam en de toets van uw Azure-opslag accountnaam en accountsleutel als kopiëren en plakken van de volgende code. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld-service
U maakt vervolgens een gekoppelde service uw cluster Azure HDInsight om aan te koppelen de fabriek Azure-gegevens. Als kopiëren en plakken van de volgende code HDInsight clusternaam vervangen door de naam van uw cluster HDInsight en waarden van gebruikersnaam en wachtwoord wijzigen. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Gegevenssets

#### <a name="output-dataset"></a>Uitvoer gegevensset
De pijplijn in dit voorbeeld vindt niet alle invoeritems. U kunt een gegevensset uitvoer opgeven voor de activiteit HDInsight Streaming. Deze dataset is alleen een pop gegevensset dat nodig is voor de verkooppijplijn planning wordt aangestuurd. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Verkooppijplijn

De pijplijn in dit voorbeeld heeft slechts één activiteit dat van het type: **HDInsightStreaming**. 

Het cluster HDInsight wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Standaard is de naam van de container die wordt gebruikt door het cluster HDInsight de naam van het cluster zelf. Bijvoorbeeld als de clusternaam van uw myhdicluster is, zou de naam van de blob container die is gekoppeld myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Zie ook
- [Activiteit component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce activiteit](data-factory-map-reduce.md)
- [Aanroepen van een programma 's](data-factory-spark.md)
- [R-scripts roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

