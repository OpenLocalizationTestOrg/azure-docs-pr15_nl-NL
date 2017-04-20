<properties
    pageTitle="Hadoop, HBase, Storm of een clusters maken op Linux in via PowerShell Azure HDInsight | Microsoft Azure"
    description="Informatie over het maken van Hadoop, HBase, Storm of een clusters op Linux voor HDInsight via Azure PowerShell."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Linux gebaseerde clusters in HDInsight maken met behulp van Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell is een krachtige uitvoeren van scripts-omgeving waarin u gebruiken kunt om te bepalen en de implementatie en het beheer van uw werkbelasting in Microsoft Azure automatiseren. In dit document bevat informatie over het maken van een cluster Linux gebaseerde HDInsight via Azure PowerShell. Het bevat ook een voorbeeldscript.

> [AZURE.NOTE] Azure PowerShell is alleen beschikbaar op Windows-clients. Als u een Linux, Unix of Mac OS X-client gebruikt, raadpleegt u [een HDInsight Linux gebaseerde cluster met Azure CLI maken](hdinsight-hadoop-create-linux-clusters-azure-cli.md) voor informatie over het gebruik van de Azure CLI een cluster maken.

## <a name="prerequisites"></a>Vereisten voor
U moet het volgende voordat u deze procedure begint:

- Een Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Zie voor meer informatie over het gebruik van Azure PowerShell met HDInsight, [HDInsight beheren via PowerShell](hdinsight-administer-use-powershell.md). Zie [HDInsight-cmdlet-naslaginformatie](https://msdn.microsoft.com/library/azure/dn858087.aspx)voor de lijst met HDInsight Windows PowerShell-cmdlets.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Clusters maken

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Als u wilt een HDInsight-cluster met behulp van Azure PowerShell maakt, moet u de volgende procedures uitvoeren:

- Een Azure resourcegroep maken
- Maak een account Azure Storage
- Een container Azure Blob maken
- Een cluster HDInsight maken

De twee belangrijkste parameters die u moet ingesteld op Linux clusters maken is de gebruiker die het OS en de details van de gebruiker SSH opgeven:

- Zorg ervoor dat u de parameter **- Waardereeksen** als **Linux**.
- Als u wilt gebruiken SSH voor externe sessies op clusters, kunt u het wachtwoord van de gebruiker SSH of de openbare sleutel SSH opgeven. Als u het wachtwoord van de gebruiker SSH zowel de SSH openbare sleutel opgeeft, wordt de toets genegeerd. Als u de SSH-toets gebruiken voor externe sessies wilt, moet u een lege SSH wachtwoord wanneer u wordt gevraagd om een opgeven. Zie een van de volgende artikelen voor meer informatie over het gebruik van SSH met HDInsight:

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

De volgende script laat zien hoe u een nieuw cluster maakt:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

De waarden die u voor **$clusterCredentials opgeeft** worden gebruikt voor het maken van het Hadoop-gebruikersaccount voor het cluster. Met dit account verbinding maken met het cluster.

De waarden die u voor **$sshCredentials opgeeft** worden gebruikt voor het maken van de gebruiker SSH voor het cluster. Gebruik dit account voor het starten van een externe SSH-sessie voor het cluster en uitvoeren van taken.

> [AZURE.IMPORTANT] In dit script, moet u het aantal knooppunten van werknemer die in de cluster. Als u van plan bent het gebruik van meer dan 32 werknemer knooppunten (hetzij bij maken van het cluster of door het cluster schaalbaarheid nadat het is gemaakt), moet u ook de grootte van een hoofd knooppunt opgeven met ten minste 8 cores en 14 GB RAM.
>
> Zie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/)voor meer informatie over het knooppunt grootte en de bijbehorende kosten.

Het kan een cluster maken maximaal 20 minuten duren.

In het onderstaande voorbeeld wordt getoond hoe een extra opslagruimte-account toevoegen:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Clusters aanpassen

- Zie [HDInsight aanpassen clusters Bootstrap gebruiken](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Zie [Windows aanpassen gebaseerde HDInsight clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

Nu die u hebt gemaakt met een cluster HDInsight, gebruikt u de volgende bronnen voor meer informatie over het werken met uw cluster.

### <a name="hadoop-clusters"></a>Hadoop clusters

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor HBase op HDInsight ontwikkelen](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm clusters

* [Ontwikkel Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Gebruik Python onderdelen in Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Elektrische clusters

* [Een zelfstandige toepassing maken met Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren op een elektrische cluster met hier](hdinsight-apache-spark-livy-rest-interface.md)
* [Elektrische met BI: interactieve gegevensanalyses elektrische in HDInsight met hulpmiddelen voor BI uitvoeren](hdinsight-apache-spark-use-bi-tools.md)
* [Elektrische met Machine Learning: gebruik een in HDInsight eten controleresultaten voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Een Streaming: Gebruik een in HDInsight voor het samenstellen van realtime streaming-toepassingen](hdinsight-apache-spark-eventhub-streaming.md)
