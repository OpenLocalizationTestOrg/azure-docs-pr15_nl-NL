<properties 
    pageTitle="MapReduce programma van Azure gegevens Factory aanroepen" 
    description="Informatie over het verwerken van gegevens door MapReduce-programma's uit te voeren op een Azure HDInsight cluster van een fabriek Azure-gegevens." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Roepen MapReduce programma's van gegevens Factory
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De HDInsight MapReduce activiteit in een Data Factory- [verkooppijplijn](data-factory-create-pipelines.md) voert MapReduce-programma's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cluster van Windows, Linux-gebaseerde HDInsight. In dit artikel is gebaseerd op het artikel [gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) , waarbij een een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatie activiteiten wordt weergegeven.

## <a name="introduction"></a>Inleiding 
Gegevens in gekoppelde opslagservices verwerkt een pijplijn in een fabriek Azure gegevens door middel van gekoppelde berekeningscluster services. De presentatie bevat een reeks activiteiten waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel wordt beschreven hoe de activiteit van de MapReduce HDInsight.
 
Zie [varken](data-factory-pig-activity.md) en [component](data-factory-hive-activity.md) voor meer informatie over het uitvoeren van scripts varken/component op een Windows/Linux gebaseerde HDInsight cluster vanaf een pijplijn met behulp van HDInsight varken en component activiteiten. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON voor HDInsight MapReduce activiteit 

In de JSON-definitie voor de activiteit HDInsight: 
 
1. Het **type** van de **activiteit** instellen met **HDInsight**.
3. Geef de naam van de klasse voor **klassenaam** eigenschap.
4. Geef het pad naar het oppervlak-bestand inclusief de bestandsnaam voor **jarFilePath** eigenschap.
5. Geef de gekoppelde service die verwijst naar de Azure-blobopslag waarin het bestand oppervlak voor **jarLinkedService** eigenschap.   
6. Geef alle argumenten voor het programma MapReduce in de sectie **argumenten** . Gedurende runtime, raadpleegt u een paar extra argumenten (bijvoorbeeld: mapreduce.job.tags) van het kader MapReduce. Als u wilt onderscheiden van de argumenten van de argumenten MapReduce, kunt u zowel de optie en de waarde als argumenten gebruiken, zoals wordt weergegeven in het volgende voorbeeld (- s,--invoer,--uitvoer enzovoort, opties onmiddellijk gevolgd door de overeenkomstige waarden zijn).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

U kunt de activiteit van de MapReduce HDInsight gebruiken om uit te voeren van elk MapReduce oppervlak-bestand op een cluster HDInsight. In het volgende voorbeeld JSON definitie van een pijplijn, is de activiteit HDInsight geconfigureerd voor het uitvoeren van een bestand Mahout JAR.

## <a name="sample-on-github"></a>Voorbeeld op GitHub
U kunt een steekproef voor het gebruik van de activiteit HDInsight MapReduce uit downloaden: [Voorbeelden van de gegevens Factory op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Woorden tellen wordt uitgevoerd
De pijplijn in dit voorbeeld wordt het aantal woorden kaart/verkleinen-programma op uw cluster Azure HDInsight uitgevoerd.   

### <a name="linked-services"></a>Gekoppelde Services
U maakt eerst een gekoppelde service om een koppeling van de Azure-opslag die wordt gebruikt door het cluster Azure HDInsight fabriek Azure-gegevens. Vergeet niet te vervangen door de naam en de toets van uw Azure-opslag **accountnaam** en **accountsleutel** als kopiëren en plakken van de volgende code. 

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice

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
U maakt vervolgens een gekoppelde service uw cluster Azure HDInsight om aan te koppelen de fabriek Azure-gegevens. Als kopiëren en plakken van de volgende code **HDInsight clusternaam** vervangen door de naam van uw cluster HDInsight en waarden van gebruikersnaam en wachtwoord wijzigen.   

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
De pijplijn in dit voorbeeld vindt niet alle invoeritems. U kunt een gegevensset uitvoer opgeven voor de activiteit van de MapReduce HDInsight. Deze dataset is alleen een pop gegevensset dat nodig is voor de verkooppijplijn planning wordt aangestuurd.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Verkooppijplijn
De pijplijn in dit voorbeeld heeft slechts één activiteit dat van het type: HDInsightMapReduce. Enkele belangrijke eigenschappen in de JSON zijn: 

Eigenschap | Notities
:-------- | :-----
type | Het type moet zijn ingesteld op **HDInsightMapReduce**. 
Klassenaam | Naam van de klasse is: **wordcount**
jarFilePath | Pad naar het oppervlak-bestand met de klas. Als u kopiëren en plakken de volgende code, vergeet niet de naam van het cluster wijzigen. 
jarLinkedService | Azure gekoppeld opslagservice die het oppervlak-bestand bevat. Deze gekoppelde service verwijst naar de opslag die is gekoppeld aan het cluster HDInsight. 
argumenten | Het programma wordcount heeft twee argumenten, een invoer en een uitvoer. De invoer bestand is de davinci.txt-bestand.
frequentie/interval | De waarden voor deze eigenschappen overeenkomen met de uitvoer gegevensset. 
linkedServiceName | verwijst naar de HDInsight gekoppeld-service die u eerder hebt gemaakt.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Een programma's uitvoeren
Een programma's uitvoeren op uw cluster HDInsight Spark kunt u MapReduce activiteit. Zie [een roepen programma's van Azure gegevens Factory](data-factory-spark.md) voor meer informatie.  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Zie ook
- [Activiteit component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Aanroepen van een programma 's](data-factory-spark.md)
- [R-scripts roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
