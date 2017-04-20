<properties
   pageTitle="Maken op basis van Windows Hadoop clusters in via PowerShell Azure HDInsight | Microsoft Azure"
    description="Informatie over het maken van clusters voor Azure HDInsight via Azure PowerShell."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Maken op basis van Windows Hadoop clusters in via PowerShell Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Leer hoe u via PowerShell Azure HDInsight-clusters maken. Azure PowerShell is een module die cmdlets uit om het beheren van Azure met Windows PowerShell biedt. Voor het maken van andere cluster hulpprogramma's en functies klikt u op het tabblad Selecteer boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Vereisten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Voordat u de instructies in dit artikel, hebt u het volgende:

- Azure-abonnement. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Clusters maken
Azure PowerShell is een krachtige uitvoeren van scripts omgeving die u gebruiken kunt om te beheren en automatiseren de implementatie en het beheer van uw werkbelasting in Azure wordt aangegeven. In deze sectie bevat instructies voor het maken van een HDInsight cluster met behulp van Azure PowerShell. Zie voor informatie over het configureren van een werkstation om uit te voeren HDInsight Windows PowerShell-cmdlets [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Zie voor meer informatie over het gebruik van Azure PowerShell met HDInsight, [HDInsight beheren via PowerShell](hdinsight-administer-use-powershell.md). Zie [HDInsight cmdlet-naslaginformatie](https://msdn.microsoft.com/library/azure/dn858087.aspx)voor de lijst van de HDInsight Windows PowerShell-cmdlets.


De volgende procedures nodig zijn voor het maken van een HDInsight-cluster met behulp van Azure PowerShell:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Clusters met ARM sjabloon maken

U kunt Azure PowerShell gebruiken om te implementeren van een sjabloon ARM Hiermee maakt u een cluster HDInsight.  Zie [sjablonen via Azure PowerShell bellen](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Clusters aanpassen

- Zie [HDInsight aanpassen clusters Bootstrap gebruiken](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Zie [Windows aanpassen gebaseerde HDInsight clusters met de Script-actie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Volgende stappen
U kunt op verschillende manieren om te maken van een cluster HDInsight hebt geleerd in dit artikel. Meer informatie raadpleegt u de volgende artikelen:

* [Aan de slag met Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - informatie over het werken met uw cluster HDInsight starten
* [Hadoop verzenden via programmacode taken](hdinsight-submit-hadoop-jobs-programmatically.md) - leren via programmacode taken kunnen verzenden naar HDInsight
* [Hadoop beheren clusters in HDInsight via PowerShell](hdinsight-administer-use-powershell.md) - informatie over het werken met HDInsight via Azure PowerShell
* [Azure HDInsight SDK-documentatie]  [ hdinsight-sdk-documentation] -kennismaken met de SDK HDInsight




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
