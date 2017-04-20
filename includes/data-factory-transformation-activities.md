Azure gegevens Factory ondersteunt de volgende transformatie activiteiten die kunnen worden toegevoegd aan pijpleidingen hetzij afzonderlijk of reeksen met een andere activiteit.

Gegevens transformatie activiteit |  Omgeving berekenen 
:----------------------- | :--------------------
[Component](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Varken](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Machine Learning activiteiten: Batch Execution en Update Resource](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Opgeslagen Procedure](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL Azure SQL datawarehouse of SQL Server |
[Gegevens Lake Analytics I-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure gegevens Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] of Azure Batch
   
> [AZURE.NOTE] 
> Een programma's uitvoeren op uw cluster HDInsight Spark kunt u MapReduce activiteit. Zie [een roepen programma's van Azure gegevens Factory](../articles/data-factory/data-factory-spark.md) voor meer informatie.
> U kunt een aangepaste activiteit om R script uitvoeren op uw cluster HDInsight met R geïnstalleerd maken. Zie [R Script uitvoeren Azure gegevens Factory gebruiken](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).