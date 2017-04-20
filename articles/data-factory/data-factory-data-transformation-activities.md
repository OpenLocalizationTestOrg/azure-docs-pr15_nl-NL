<properties 
    pageTitle="Gegevenstransformatie: Het proces en transformeren gegevens | Microsoft Azure" 
    description="Leer hoe u de gegevens of procesgegevens in Azure gegevens fabriek met Hadoop Machine Learning of Azure gegevens Lake Analytics transformeren." 
    keywords="gegevenstransformatie, procesgegevens, gegevens, transformatie activiteit transformeren"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Transformeer gegevens in fabriek van Azure-gegevens
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Overzicht 
In dit artikel wordt uitgelegd gegevens transformatie activiteiten in fabriek voor Azure-gegevens die u gebruiken kunt om te transformeren en verwerking van uw onbewerkte gegevens op voorspellingen en inzichten. Een activiteit in een transformatie wordt uitgevoerd in een omgeving zoals Azure HDInsight cluster of een Batch Azure. Deze koppelingen naar artikelen met gedetailleerde informatie over elke activiteit transformatie.
 
Gegevens Factory ondersteunt de volgende gegevens transformatie activiteiten die kunnen worden toegevoegd aan [pijpleidingen](data-factory-create-pipelines.md) hetzij afzonderlijk of reeksen met een andere activiteit.

> [AZURE.NOTE] Voor een overzicht met stapsgewijze instructies, Zie [maken van een pijplijn met component transformatie](data-factory-build-your-first-pipeline.md) artikel.  

## <a name="hdinsight-hive-activity"></a>HDInsight component activiteit
De HDInsight component activiteit in een pijplijn Data Factory voert component query's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie [Component activiteit](data-factory-hive-activity.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-pig-activity"></a>HDInsight varken activiteit
De HDInsight varken activiteit in een pijplijn Data Factory voert varken query's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie [Varken activiteit](data-factory-pig-activity.md) artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce activiteit
De HDInsight MapReduce activiteit in een pijplijn Data Factory voert MapReduce-programma's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie [MapReduce activiteit](data-factory-map-reduce.md) artikel voor meer informatie over deze activiteit.

Een programma's uitvoeren op uw cluster HDInsight Spark kunt u MapReduce activiteit. Zie [een roepen programma's van Azure gegevens Factory](data-factory-spark.md) voor meer informatie.

## <a name="hdinsight-streaming-activity"></a>Activiteit HDInsight Streaming
De HDInsight Streaming activiteit in een pijplijn Data Factory voert Hadoop Streaming-programma's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie [HDInsight Streaming activiteit](data-factory-hadoop-streaming-activity.md) voor meer informatie over deze activiteit.

## <a name="machine-learning-activities"></a>Machine Learning activiteiten
Azure gegevens Factory kunt u eenvoudig maken pijpleidingen met een gepubliceerde Azure Machine Learning-webservice van blog analysegegevens. De [Batch Execution activiteit](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in een Azure gegevens Factory-verkooppijplijn gebruikt, kunt u een Machine Learning-webservice zodat voorspellingen van de gegevens in de batch oproepen.

Over een periode moeten de blog modellen in de Machine Learning scoren experimenten worden retrained nieuwe invoer gegevenssets opnieuw gebruiken. Nadat u klaar bent met nodig, die u wilt bijwerken van de score webservice met het retrained Machine Learning-model. U kunt de [Activiteit van Resource bijwerken](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) bij de webservice met het zojuist ervaren model.  

Zie [Gebruik Machine Learning-activiteiten](data-factory-azure-ml-batch-execution-activity.md) voor meer informatie over deze Machine Learning-activiteiten. 

## <a name="stored-procedure-activity"></a>Opgeslagen procedure activiteit
U kunt de opgeslagen Procedure van SQL Server-activiteit in een pijplijn Data Factory aanroepen van een opgeslagen procedure in een van de volgende gegevensarchieven: Azure SQL-Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een VM Azure. Zie [Opgeslagen Procedure activiteit](data-factory-stored-proc-activity.md) artikel voor meer informatie.  

## <a name="data-lake-analytics-u-sql-activity"></a>Gegevens Lake Analytics I-SQL activiteit
Gegevens Lake Analytics I-SQL activiteit een I-SQL-script op een Azure gegevens Lake Analytics-cluster wordt uitgevoerd. Zie [Gegevens Analytics I-SQL activiteit](data-factory-usql-activity.md) artikel voor meer informatie. 

## <a name="net-custom-activity"></a>Aangepaste activiteit .NET
Als u nodig hebt om gegevens op een manier die niet wordt ondersteund door gegevens Factory te transformeren, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en gebruiken van de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit om uit te voeren met behulp van een service van Azure Batch of een cluster Azure HDInsight. Zie [Gebruik aangepaste activiteiten](data-factory-use-custom-activities.md) artikel voor meer informatie. 

U kunt een aangepaste activiteit R om scripts te voeren op uw cluster HDInsight met R geïnstalleerd maken. Zie [R-Script uitvoeren Azure gegevens Factory gebruiken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Omgevingen berekenen
U maakt een gekoppelde service voor het milieu berekenen en gebruik vervolgens de gekoppelde service bij het definiëren van de activiteit in een transformatie. Er zijn twee soorten berekeningscluster omgevingen worden ondersteund door gegevens Factory. 

1. **Op aanvraag**: In dit geval de computer omgeving volledig wordt beheerd door gegevens Factory. Deze wordt automatisch gemaakt door de gegevens Factory-service voordat een taak is verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt configureren en beheren van gedetailleerde instellingen van de omgeving op aanvraag berekeningscluster voor uitvoering, cluster management en bootstrappen acties. 
2. **Uw eigen brengen**: In dit geval kunt u uw eigen computer (bijvoorbeeld HDInsight cluster)-omgeving registreren als een gekoppelde service in gegevens fabriek. De IT-omgeving wordt beheerd door u en de gegevens Factory-service gebruikt de activiteiten uitvoeren. 

Zie [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) artikel voor meer informatie over services worden ondersteund door gegevens Factory berekenen. 


## <a name="summary"></a>Overzicht
Azure gegevens Factory ondersteunt de volgende gegevens transformatie activiteiten en de omgevingen berekeningscluster voor de activiteiten. De activiteiten transformatie kunnen worden toegevoegd aan pijpleidingen hetzij afzonderlijk of reeksen met een andere activiteit.

Gegevens transformatie activiteit |  Omgeving berekenen 
:----------------------- | :--------------------
[Component](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Varken](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Machine Learning activiteiten: Batch Execution en Update Resource](data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md) | Azure SQL Azure SQL datawarehouse of SQL Server |
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md) | Azure gegevens Lake Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] of Azure Batch
   

