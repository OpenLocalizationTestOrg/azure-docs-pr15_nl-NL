<properties
    pageTitle="Hadoop, HBase of Storm clusters maken op Linux in met de CLI van meerdere platforms Azure HDInsight | Microsoft Azure"
    description="Leer hoe u met de platforms Azure CLI, Azure resourcemanager sjablonen en de REST API van Azure HDInsight Linux gebaseerde-clusters maken. U kunt het clustertype (Hadoop, HBase of Storm) opgeven of scripts gebruiken om aangepaste onderdelen te installeren..."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Linux gebaseerde clusters maken in met de CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

De CLI Azure is een platforms opdrachtregel hulpprogramma waarmee u Azure Services beheren. Dit kan worden gebruikt, samen met Azure Resource management sjablonen maken van een cluster HDInsight, samen met de bijbehorende opslag accounts en andere services.

Azure resourcebeheer sjablonen zijn JSON-documenten die beschrijven van een __resourcegroep__ en alle resources in deze (zoals HDInsight.) Deze methode op basis van een sjabloon kunt u alle bronnen die u nodig voor HDInsight in één sjabloon hebt definiëren. U kunt ook wijzigingen in de groep beheren als geheel tot en met __implementaties__welke wijzigingen toepassen op de hele groep.

De stappen in dit document is doorlopen van het proces van het maken van een nieuw HDInsight cluster met behulp van de Azure CLI en een sjabloon.

> [AZURE.IMPORTANT] De stappen in dit document gebruiken het standaardaantal werknemer knooppunten (4) voor een cluster HDInsight. Als u van plan bent meer dan 32 werknemer knooppunten (tijdens het maken van cluster of door het cluster schaalbaarheid), selecteert u de grootte van een hoofd knooppunt met ten minste 8 cores en 14 GB ram.
>
> Zie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/)voor meer informatie over het knooppunt grootte en de bijbehorende kosten.

##<a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. De stappen in dit document zijn laatst getest met Azure CLI versie 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

Volg de stappen beschreven in [verbinding maken met een Azure abonnement vanaf de opdrachtregel van Azure (Azure CLI)](../xplat-cli-connect.md) en verbinding maken met de methode __Meld u__ aan uw abonnement.

##<a name="create-a-cluster"></a>Een cluster maken

De volgende stappen moeten worden uitgevoerd vanuit een opdrachtprompt, shell of een sessie na het installeren en configureren van de Azure CLI.

1. Gebruik de volgende opdracht uit om te verifiëren bij uw Azure-abonnement:

        azure login

    U wordt gevraagd of u uw naam en wachtwoord op te geven. Als u meerdere Azure abonnementen hebt, gebruikt u `azure account set <subscriptionname>` voor het instellen van het abonnement waaraan de Azure CLI-opdrachten gebruiken.

3. Overschakelen naar resourcemanager Azure-modus met de volgende opdracht:

        azure config mode arm

4. Een resourcegroep maken. Deze resourcegroep bevat het cluster HDInsight en bijbehorende opslag-account.

        azure group create groupname location
        
    * __Groepsnaam__ vervangen door een unieke naam voor de groep. 
    * __Locatie__ vervangen door het geografische gebied dat u wilt maken van de groep in. 
    
        Voor een lijst met geldige locaties, gebruikt u de `azure location list` opdracht en gebruikt u een van de locaties in de kolom __naam__ .

5. Maak een account opslag. Dit account opslag wordt gebruikt als de standaard-opslag voor het cluster HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * __Groepsnaam__ vervangen door de naam van de groep in de vorige stap hebt gemaakt.
     * __Locatie__ vervangen op dezelfde locatie gebruikt in de vorige stap. 
     * __Storagename__ vervangen door een unieke naam voor de opslag-account.
     
     > [AZURE.NOTE] Voor meer informatie over de parameters in deze opdracht gebruikt, gebruikt u `azure storage account create -h` waarmee u help voor deze opdracht.

5. De sleutel die wordt gebruikt voor toegang tot het account opslag ophalen.

        azure storage account keys list -g groupname storagename
        
    * __Groepsnaam__ vervangen door de naam van de resource-groep.
    * __Storagename__ vervangen door de naam van de opslag-account.
    
    In de gegevens die worden geretourneerd, sla __de sleutelwaarde voor __sleutel1____ .

6. Maak een cluster HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * __Groepsnaam__ vervangen door de naam van de resource-groep.

    * __Hadoop__ vervangen door het clustertype die u wilt maken. Bijvoorbeeld `Hadoop`, `HBase`, `Storm` of `Spark`.

        > [AZURE.IMPORTANT] HDInsight clusters vele verschillende met typen, die met de werklast technologie die het cluster is afgestemd corresponderen op. Er is geen ondersteunde methode een cluster waarin meerdere typen, zoals Storm en HBase op één cluster zijn gecombineerd maken. 

    * __Locatie__ vervangen op dezelfde locatie in de vorige stappen gebruikt.

    * __Storagename__ vervangen door de naam van het opslag-account.

    * __Storagekey__ vervangen door de toets verkregen in de vorige stap. 

    * Voor de `--defaultStorageContainer` parameter, met dezelfde naam als u voor het cluster worden gebruikt.

    * __Beheerder__ en __httppassword__ vervangen door de naam en het wachtwoord dat u gebruiken wilt bij het openen van het cluster via HTTPS.

    * __Sshuser__ en __sshuserpassword__ vervangen door de gebruikersnaam en wachtwoord die u gebruiken wilt bij het openen van het cluster via SSH

    Het kan enkele minuten duren voor het maakproces cluster om te voltooien. Meestal ongeveer 15.

##<a name="next-steps"></a>Volgende stappen

Nu die u hebt gemaakt met een met de CLI Azure HDInsight-cluster, gebruikt u de volgende manieren te werk voor meer informatie over het werken met uw cluster:

###<a name="hadoop-clusters"></a>Hadoop clusters

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor HBase op HDInsight ontwikkelen](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm clusters

* [Ontwikkel Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Gebruik Python onderdelen in Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)
