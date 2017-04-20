<properties
    pageTitle="Fouten opsporen en analyseren Hadoop-services met opslagruimte dumps | Microsoft Azure"
    description="Automatisch verzamelen opslagruimte dumps voor Hadoop-services en plaats binnen het Azure Blob storage-account voor foutopsporing en analyse."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Verzamelen opslagruimte wordt in-blobopslag fouten opsporen en analyseren Hadoop-services

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Opslagruimte dumps bevatten een momentopname van de toepassing geheugen, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Zodat ze bijzonder nuttig zijn zijn voor het oplossen van problemen die tijdens de uitvoering optreden. Opslagruimte dumps kunnen automatisch worden verzameld voor Hadoop-services en in het Azure Blob storage-account van een gebruiker onder HDInsightHeapDumps geplaatst /. 

De verzameling opslagruimte dumps voor verschillende services moet zijn ingeschakeld voor services op afzonderlijke clusters. De standaardindeling voor deze functie is moeten uitschakelen voor een cluster. Deze dumps opslagruimte zijn grote, is het raadzaam om de de Blob storage rekening te houden waar ze worden opgeslagen nadat de verzameling is ingeschakeld.

> [AZURE.NOTE] De informatie in dit artikel is alleen van toepassing op Windows gebaseerde HDInsight. Zie [opslagruimte dumps voor Hadoop-services op Linux gebaseerde HDInsight inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md) voor informatie over Linux gebaseerde HDInsight,

## <a name="eligible-services-for-heap-dumps"></a>In aanmerking komend services voor opslagruimte dumps

U kunt opslagruimte dumps voor de volgende services inschakelen:

*  **hcatalog** - tempelton
*  **component** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **garens** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Configuratie-elementen die opslagruimte dumps inschakelen

Als u wilt inschakelen opslagruimte dumps voor een service, moet u de juiste configuratie-elementen zijn ingesteld in de sectie voor deze service, die is opgegeven met **servicenaam**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

De waarde van **servicenaam** kan zijn van de bovengenoemde services: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, of namenode.

## <a name="enable-using-azure-powershell"></a>Inschakelen via Azure PowerShell

Bijvoorbeeld als u wilt opslagruimte dumps inschakelen via Azure PowerShell voor jobhistoryserver, zou u het volgende doen:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Met .NET SDK inschakelen

Bijvoorbeeld, om in te schakelen opslagruimte dumps met behulp van de Azure HDInsight .NET SDK voor jobhistoryserver, zou u het volgende doen:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
