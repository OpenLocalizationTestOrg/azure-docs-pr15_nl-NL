<properties 
    pageTitle="Activiteit component" 
    description="Leer hoe u kunt de component activiteit in een fabriek Azure gegevens component query's uitvoeren op een op aanvraag/uw eigen HDInsight cluster." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Activiteit component
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De HDInsight component activiteit in een Data Factory- [verkooppijplijn](data-factory-create-pipelines.md) component query's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight op basis van Windows/Linux cluster wordt uitgevoerd. In dit artikel is gebaseerd op het artikel [gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) , waarbij een een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatie activiteiten wordt weergegeven.

## <a name="syntax"></a>Syntaxis

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Syntaxis van de details

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Beschrijving van wat de activiteit wordt gebruikt voor | Nee
type | HDinsightHive | Ja
ingangen | Invoer worden gebruikt door de activiteit component | Nee
uitvoer | Uitvoer van geproduceerd door de activiteit component | Ja 
linkedServiceName | Verwijzing naar de HDInsight-cluster dat is geregistreerd als een gekoppelde service in gegevens fabriek | Ja 
script | Geef de component script inline | Nee
scriptpad | Het script component opslaan in een Azure-blobopslag en geef het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide worden niet samen gebruikt. De bestandsnaam is hoofdlettergevoelig. | Nee 
Hiermee definieert u | Parameters opgeeft als sleutel/waardeparen voor verwijst naar binnen het component script 'hiveconf' gebruiken  | Nee

## <a name="example"></a>Voorbeeld

Laten we eens een voorbeeld van spel logboeken analytics waar u wilt identificeren de tijd wordt gebruikt door gebruikers spellen gestart door uw bedrijf wordt afgespeeld. 

Het volgende logboek is een voorbeeld spel logboek komma (`,`) gescheiden en bevat de volgende velden – profiel-id, SessionStart, duur, SrcIPAddress en GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Het **script component** verwerkingstijd van deze gegevens:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Als u wilt deze component-script uitvoeren in een pijplijn Data Factory, moet u als volgt te werk

1. Maak een gekoppelde service registreren [uw eigen HDInsight cluster berekenen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of configureren [op aanvraag HDInsight cluster berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Laten we belt u deze gekoppelde service "HDInsightLinkedService".
2. Maak een [gekoppelde service](data-factory-azure-blob-connector.md) configureren van de verbinding met Azure-blobopslag waarop de gegevens. Laten we bellen deze gekoppelde service "StorageLinkedService"
3. Maak [gegevenssets](data-factory-create-datasets.md) die wijst naar de invoer- en de uitvoergegevens. Laten we bellen de invoer gegevensset 'HiveSampleIn' en de uitvoer gegevensset "HiveSampleOut"
4. Kopie van de query component als een bestand met Azure Blob Storage geconfigureerd in stap #2. Als de opslagruimte voor het hosten van de gegevens van degene die dit querybestand hostingprovider verschilt, een afzonderlijke Azure gekoppeld opslagservice maken en in de activiteit verwijzen. Met **scriptPath **kunt u opgeven van het pad naar de component query bestands- en **scriptLinkedService** om op te geven van de Azure opslag waarin het scriptbestand. 

    > [AZURE.NOTE] U kunt ook de component script inline in de definitie van de activiteit opgeven met behulp van de eigenschap **script** . We raden niet deze methode als alle speciale tekens in het script in de behoeften van het document JSON worden overgeslagen en foutopsporing problemen kan veroorzaken. De beste manier is om te volgen stap #4.
5.  Maak een pijplijn met de activiteit HDInsightHive. De activiteit processen/transformaties de gegevens.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  De pijplijn implementeren. Zie [maken pijpleidingen](data-factory-create-pipelines.md) artikel voor meer informatie. 
7.  Het gebruik van de factory voor controle en beheer gegevensweergaven verkooppijplijn bewaken. Zie [controle en beheren van gegevens Factory pijpleidingen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie. 


## <a name="specifying-parameters-for-a-hive-script"></a>Parameters voor een script component opgeven  
In dit voorbeeld spel logboeken dagelijks zijn ingenomen in Azure-blobopslag en zijn opgeslagen in een map met de datum en tijd partities. U wilt voorzien van het script component en de locatie van de invoer dynamisch doorgeven tijdens runtime en ook resultaten de uitvoer partities met datum en tijd.

Als u wilt gebruiken met parameters component script, als volgt te werk

- Definieer de parameters in **definieert**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- In het Script component raadpleegt u de parameter met **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Zie ook
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Aanroepen van een programma 's](data-factory-spark.md)
- [R-scripts roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









