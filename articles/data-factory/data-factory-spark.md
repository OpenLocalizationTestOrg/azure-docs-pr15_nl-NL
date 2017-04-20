<properties 
    pageTitle="Een programma's van Azure gegevens Factory roepen" 
    description="Leer hoe u een programma's van een Azure gegevens factory gebruik van de activiteit MapReduce roepen." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Een programma's van gegevens Factory roepen
## <a name="introduction"></a>Inleiding
U kunt de MapReduce activiteit in een pijplijn Data Factory elektrische programma's uitvoeren op uw cluster HDInsight Spark. Zie [MapReduce activiteit](data-factory-map-reduce.md) artikel voor gedetailleerde informatie over het gebruik van de activiteit voordat u lezen in dit artikel. 

## <a name="spark-sample-on-github"></a>Een voorbeeld op GitHub
De [een - gegevens Factory voorbeeld op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) ziet u hoe u een programma een aanroepen met MapReduce activiteit. Het programma elektrische kopieert alleen gegevens uit één Azure Blob container naar een andere. 

## <a name="data-factory-entities"></a>Gegevens Factory entiteiten
De map **Elektrische-ADF/src/ADFJsons** bevat voor Data Factory entiteiten (gekoppelde services, gegevenssets, verkooppijplijn).  

Er zijn twee **gekoppelde services** in dit voorbeeld: Azure opslagruimte en Azure HDInsight. Geef uw naam Azure gegevensopslag en sleutelwaarden in **StorageLinkedService.json** en clusterUri, gebruikersnaam en wachtwoord in **HDInsightLinkedService.json**.

Er zijn twee **gegevenssets** in dit voorbeeld: **input.json** en **output.json**. Deze bestanden bevinden zich in de map **gegevenssets** .  Deze bestanden vertegenwoordigen invoer- en uitvoerbereik gegevenssets voor de activiteit MapReduce

U vinden steekproef pijpleidingen in de map **ADFJsons/verkooppijplijn** . Bekijk een pijplijn als u wilt weten hoe u een een-programma met behulp van de activiteit MapReduce aanroepen. 

De activiteit MapReduce is geconfigureerd om aan te roepen **com.adf.sparklauncher.jar** in de container **adflibs** in uw Azure-opslag (opgegeven in de StorageLinkedService.json). De broncode voor dit programma is in een-ADF/src/Hoofdgegeven/java/com/adf/map en deze roept een indienen en een taken uitvoeren. 

> [AZURE.IMPORTANT] 
> [Leesmij doorlezen. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) voor de meest recente en aanvullende informatie vóór het gebruik van de steekproef. 
>  
> Uw eigen HDInsight Spark cluster met deze methode gebruiken om aan te roepen met de activiteit MapReduce elektrische-programma's. Gebruik een op aanvraag HDInsight cluster wordt niet ondersteund.   


## <a name="see-also"></a>Zie ook
- [Activiteit component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [R-scripts roepen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
