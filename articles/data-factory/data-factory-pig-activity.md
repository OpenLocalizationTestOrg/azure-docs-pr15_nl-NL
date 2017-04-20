<properties 
    pageTitle="Varken activiteit" 
    description="Leer hoe u kunt de varken activiteit in een fabriek Azure gegevens varken scripts worden uitgevoerd op een op aanvraag/uw eigen HDInsight cluster." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Varken activiteit
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De HDInsight varken activiteit in een Data Factory- [verkooppijplijn](data-factory-create-pipelines.md) voert varken query's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cluster van Windows, Linux-gebaseerde HDInsight. In dit artikel is gebaseerd op het artikel [gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) , waarbij een een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatie activiteiten wordt weergegeven.

## <a name="syntax"></a>Syntaxis

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Syntaxis van de details

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Beschrijving van wat de activiteit wordt gebruikt voor | Nee
type | HDinsightPig | Ja
ingangen | Één of meer ingevoerde gegevens dat door de activiteit varken | Nee
uitvoer | Een of meer uitvoer geproduceerd door de activiteit varken | Ja
linkedServiceName | Verwijzing naar de HDInsight-cluster dat is geregistreerd als een gekoppelde service in gegevens fabriek | Ja
script | Geef de varken script inline | Nee
scriptpad | Het script varken opslaan in een Azure-blobopslag en geef het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide worden niet samen gebruikt. De bestandsnaam is hoofdlettergevoelig. | Nee
Hiermee definieert u | Parameters opgeeft als sleutel/waardeparen voor verwijst naar binnen het script varken | Nee

## <a name="example"></a>Voorbeeld

Laten we eens een voorbeeld van spel logboeken analytics waar u wilt identificeren de tijd die is besteed door spelers spellen gestart door uw bedrijf wordt afgespeeld.
 
Het volgende voorbeeld spel-logboek is een komma (,) gescheiden bestand. Tabel bevat de volgende velden – profiel-id, SessionStart, duur, SrcIPAddress en GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Het **script varken** verwerkingstijd van deze gegevens:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Ga als volgt te werk als u wilt deze varken-script uitvoeren in een pijplijn Data Factory:

1. Maak een gekoppelde service registreren [uw eigen HDInsight cluster berekenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of configureren [op aanvraag HDInsight cluster berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Laten we dit gekoppelde service **HDInsightLinkedService**aanroepen.
2.  Maak een [gekoppelde service](data-factory-azure-blob-connector.md) configureren van de verbinding met Azure-blobopslag waarop de gegevens. Laten we dit gekoppelde service **StorageLinkedService**aanroepen.
3.  Maak [gegevenssets](data-factory-create-datasets.md) die wijst naar de invoer- en de uitvoergegevens. Laten we bellen de invoer gegevensset **PigSampleIn** en de uitvoer gegevensset **PigSampleOut**.
4.  Kopieer de query varken in een bestand van de Azure-blobopslag geconfigureerd in stap #2. Als de Azure opslag waarop de gegevens van de waarop het query-bestand verschilt, maakt u een aparte Azure gekoppeld opslagservice. Verwijzen naar de gekoppelde-service in de configuratie van de activiteit. Met **scriptPath **kunt u het pad naar varken script-bestand en **scriptLinkedService**opgeven. 
    
    > [AZURE.NOTE] U kunt ook de inline varken script in de definitie van de activiteit opgeven met behulp van de eigenschap **script** . Maar we raden niet deze methode als alle speciale tekens in de behoeften script worden overgeslagen en foutopsporing problemen kan veroorzaken. De beste manier is om te volgen stap #4.
5. Maak de pijplijn met de activiteit HDInsightPig. De invoergegevens verwerkt deze activiteit door het varken script uitvoeren op HDInsight cluster.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. De pijplijn implementeren. Zie [maken pijpleidingen](data-factory-create-pipelines.md) artikel voor meer informatie. 
7. Het gebruik van de factory voor controle en beheer gegevensweergaven verkooppijplijn bewaken. Zie [controle en beheren van gegevens Factory pijpleidingen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie.

## <a name="specifying-parameters-for-a-pig-script"></a>Parameters voor een script varken opgeven 

Houd rekening met het volgende voorbeeld: spel logboeken zijn ingenomen dagelijks in Azure-blobopslag en opgeslagen in een map gepartitioneerde op basis van datum en tijd. U wilt voorzien van het script varken en de locatie van de invoer dynamisch doorgeven tijdens runtime en ook resultaten de uitvoer partities met datum en tijd.
 
Ga als volgt te werk als u wilt gebruiken met parameters varken script:

- Definieer de parameters in **definieert**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- In het Script varken, gebruikt u verwijzen naar de parameters '**$parameterName**' gebruiken, zoals in het volgende voorbeeld:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Zie ook
- [Activiteit component](data-factory-hive-activity.md)
- [MapReduce activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Aanroepen van een programma 's](data-factory-spark.md)
- [R-scripts roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


