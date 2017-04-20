<properties
   pageTitle="Maken op basis van Windows Hadoop clusters in met CLI Azure HDInsight"
    description="Informatie over het maken van clusters voor Azure HDInsight via Azure CLI."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Maken op basis van Windows Hadoop clusters in met CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Leer hoe u HDInsight clusters met Azure CLI maken. Voor het maken van andere cluster hulpprogramma's en functies klikt u op het tabblad Selecteer boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Vereisten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Voordat u de instructies in dit artikel, hebt u het volgende:

- **Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Verbinding maken met Azure

Met de volgende opdracht verbinding maken met Azure:

    azure login

Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de Azure CLI](../xplat-cli-connect.md).

Gebruik de volgende opdracht uit om te schakelen naar de modus ARM:

    azure config mode arm

Als u help, gebruikt u de schakeloptie **-h** .  Bijvoorbeeld:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Clusters maken

U moet een Azure Resource Management (ARM) en een Azure Blob storage-account voordat u een cluster HDInsight kunt maken. Als u wilt een cluster HDInsight maakt, moet u het volgende opgeven:

- **Azure resourcegroep**: A gegevens Lake Analytics-account moet worden gemaakt in een groep Azure Resource. Azure resourcemanager kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.

    Voor een overzicht van de resource van groepen in uw abonnement:

        azure group list

    Een nieuwe resourcegroep maken:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **De naam van de cluster HDInsight**

- **Locatie**: een van de Azure datacenters die ondersteuning biedt voor HDInsight clusters. Zie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/)voor een lijst met ondersteunde locaties aan.

- **Opslag standaardaccount**: HDInsight een Azure Blob storage container gebruikt als het standaard-bestandssysteem. Er is een opslag van Azure-account vereist voordat u een cluster HDInsight kunt maken.

    Een nieuwe Azure opslag-account maken:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Het account opslag moet worden collocated met HDInsight in het datacenter.
    > Het accounttype opslag kan ZRS, niet, omdat ZRS biedt geen ondersteuning voor tabel.

    Zie voor informatie over het maken van een Azure Storage-account met behulp van de Portal Azure [maken, beheren of verwijderen van een account opslag] [azure-maken-storageaccount].

    Als u al een opslag-account hebt maar niet de naam van het account en accountsleutel kent, kunt u de volgende opdrachten de gegevens ophalen:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Zie het gedeelte "Uw account opslag beheren" [over Azure opslag](../storage/storage-create-storage-account#manage-your-storage-account)accounts voor meer informatie over het gebruik van de gegevens met behulp van de Azure-Portal.

- **(Optioneel) standaard Blob container**: de opdracht **azure hdinsight cluster maken** Hiermee maakt u de container als deze niet bestaat. Als u de container vooraf maakt, kunt u de volgende opdracht uit:

    Azure opslag container maken--accountnaam "<Storage Account Name>"--accountsleutel <Storage Account Key> [ContainerName]

Nadat u het voorbereid opslag-account hebt, kunt u bent een cluster maken:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Clusters met configuratiebestanden maken
Meestal u maakt een cluster HDInsight, taken uitvoeren op is geïnstalleerd, en verwijder vervolgens het cluster Beperk de kosten. Een interface voor de opdrachtregel kunt u de optie voor het opslaan van de configuraties in een bestand, zodat u deze opnieuw gebruiken kunt telkens wanneer u een cluster maakt.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Voorbeeld: Een configuratiebestand met de scriptactie in een uitvoeren bij het maken van een cluster maken.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Clusters met scriptactie maken

Een configuratiebestand met de scriptactie in een uitvoeren bij het maken van een cluster maken.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Een cluster met een scriptactie voor een maken

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Zie voor algemene script actie informatie [aanpassen HDInsight clusters met de actie Script (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Clusters met ARM sjablonen maken

U kunt CLI clusters maken door te bellen ARM sjablonen. Zie [implementeren met Azure CLI](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Zie ook

- [Aan de slag met Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - informatie over het werken met uw cluster HDInsight starten
- [Hadoop verzenden via programmacode taken](hdinsight-submit-hadoop-jobs-programmatically.md) - leren via programmacode taken kunnen verzenden naar HDInsight
- [Hadoop clusters in met de CLI Azure HDInsight beheren](hdinsight-administer-use-command-line.md)
- [Met behulp van de Azure CLI voor Mac, Linux en Windows met Azure servicebeheer](../virtual-machines-command-line-tools.md)
