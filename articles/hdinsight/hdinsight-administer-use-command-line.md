<properties
    pageTitle="Hadoop clusters met Azure CLI beheren | Microsoft Azure"
    description="Het gebruik van de Azure CLI Hadoop clusters in HDIsight beheren"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Hadoop clusters in met de CLI Azure HDInsight beheren

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informatie over het gebruik van de [Interface van Azure-opdrachtregel](../xplat-cli-install.md) voor het beheren van Hadoop clusters in Azure HDInsight. De CLI Azure is geïmplementeerd in Node.js. Dit kan worden gebruikt op een willekeurig platform die ondersteuning biedt voor Node.js, met inbegrip van Windows, Mac en Linux.

In dit artikel beschreven hoe u met alleen de CLI Azure met HDInsight. Zie voor een algemene gids over het gebruik van Azure CLI [installeren en configureren van Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** - Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md) voor installatie en configuratie.
- **Verbinding maken met Azure**, met de volgende opdracht:

        azure login

    Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de Azure CLI](xplat-cli-connect.md).
    
- **Overschakelen naar de modus Azure resourcemanager**, met de volgende opdracht:

        azure config mode arm

Als u help, gebruikt u de schakeloptie **-h** .  Bijvoorbeeld:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Clusters maken

Zie [maken Linux gebaseerde clusters in met de CLI Azure HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Een lijst met en cluster details weergeven
Gebruik de volgende opdrachten weergeven en cluster details weergeven:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Clusters verwijderen

Gebruik de volgende opdracht uit om te verwijderen van een cluster:

    azure hdinsight cluster delete <Cluster Name>

U kunt ook een cluster verwijderen door te verwijderen van de resourcegroep die het cluster bevat. Let op: Hiermee verwijdert u alle resources in de groep met inbegrip van het standaardaccount voor de opslag.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Schaal clusters

De grootte van de cluster Hadoop wijzigen:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>HTTP-toegang voor een cluster in-/ uitschakelen

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>RDP toegang voor een cluster in-/ uitschakelen

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Volgende stappen
In dit artikel, hebt u geleerd hoe verschillende HDInsight cluster beheerderstaken uitvoeren. Meer informatie raadpleegt u de volgende artikelen:

* [HDInsight beheren met behulp van de Azure-Portal] [hdinsight-admin-portal]
* [HDInsight beheren met behulp van Azure PowerShell] [hdinsight-admin-powershell]
* [Aan de slag met Azure HDInsight] [hdinsight-get-started]
* [Het gebruik van de Azure CLI] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Een lijst met en clusters weergeven"
