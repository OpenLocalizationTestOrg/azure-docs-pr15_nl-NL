<properties
    pageTitle="Beschikbaarheid van Hadoop clusters in HDInsight | Microsoft Azure"
    description="HDInsight implementeert zeer beschikbaar en betrouwbare clusters met een extra hoofd knooppunt."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Beschikbaarheid en betrouwbaarheid van Windows gebaseerde Hadoop clusters in HDInsight


>[AZURE.NOTE] De stappen in dit document zijn specifiek voor Windows gebaseerde HDInsight clusters. Als u een cluster Linux gebaseerde gebruikt, raadpleegt u [beschikbaarheid en betrouwbaarheid van Hadoop Linux gebaseerde clusters in HDInsight](hdinsight-high-availability-linux.md) voor Linux-specifieke informatie.

HDInsight kan klanten diverse typen cluster, voor verschillende gegevens analytics werkbelasting implementeren. Clustertypen aangeboden vandaag zijn Hadoop clusters voor query en analyse-werkbelastingen, HBase clusters voor NoSQL werkbelasting en Storm clusters voor de verwerking van de gebeurtenis realtime werkbelasting. Binnen een bepaald cluster het type, zijn er verschillende rollen voor de verschillende knooppunten. Bijvoorbeeld:



- Hadoop clusters voor HDInsight zijn geïmplementeerd met twee rollen:
    - Hoofd knooppunt (2 knooppunten)
    - Gegevensknooppunt (ten minste 1 knooppunt)

- HBase clusters voor HDInsight zijn geïmplementeerd met drie rollen:
    - Hoofd-servers (2 knooppunten)
    - Regio-servers (ten minste 1 knooppunt)
    - Basispagina/Zookeeper knooppunten (3 knooppunten)

- Storm clusters voor HDInsight zijn geïmplementeerd met drie rollen:
    - Nimbus knooppunten (2 knooppunten)
    - Toezichthouder servers (ten minste 1 knooppunt)
    - Zookeeper knooppunten (3 knooppunten)

Standaard implementaties van Hadoop clusters hebben meestal één hoofd knooppunt. HDInsight Hiermee verwijdert u deze potentieel risico met het toevoegen van een secundaire hoofd knooppunt/HEAD server/Nimbus knooppunt om uit te breiden de beschikbaarheid en betrouwbaarheid van de service die nodig zijn voor het beheren van werkbelasting. Deze kop knooppunten/hoofd-servers/Nimbus knooppunten zijn ontworpen voor het beheer van de fout van werknemer knooppunten soepel, maar elke bijvoorbeeld van basispagina services worden uitgevoerd op het hoofd knooppunt zou ertoe leiden dat het cluster te beëindigen om te werken.


[ZooKeeper](http://zookeeper.apache.org/ ) knooppunten (ZKs) zijn toegevoegd en worden gebruikt voor de leider van de verkiezingsuitslagen van hoofd knooppunten en om te garanderen dat die werknemer wanneer u naar de secundaire hoofd knooppunt (hoofd knooppunt1) mislukken wanneer het actieve hoofd knooppunt (hoofd Node0) inactief weet knooppunten en gateways (GWs).

![Diagram van de uiterst betrouwbaar hoofd knooppunten in de HDInsight Hadoop-implementatie.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Controleer de servicestatus actief hoofd knooppunt
Om te bepalen welke hoofd knooppunt actief is en om te controleren of de status van de services die op dat hoofd knooppunt wordt uitgevoerd, moet u verbinding maken met de Hadoop-cluster met behulp van de Remote Desktop Protocol (RDP). Zie de instructies voor het RDP [beheren Hadoop clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Nadat u hebt verbonden met het cluster, dubbelklik op het pictogram **Hadoop Service beschikbaar** op het bureaublad voor status over welke hoofd knooppunt de Namenode, Jobtracker, Templeton Oozieservice, Metastore en Hiveserver2 services zijn actief, of voor HDI 3.0, de Namenode resourcemanager, geschiedenis Server, Templeton, Oozieservice, Metastore en Hiveserver2 services.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Klik op de schermafbeelding is het actieve hoofd knooppunt *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Access-logboekbestanden op de secundaire hoofd knooppunt

Toegang tot taak logboeken op de secundaire hoofd knooppunt in het geval dat dit het actieve hoofd knooppunt, browsen op het JobTracker UI geworden nog steeds werkt als voor het primaire actieve knooppunt. Voor toegang tot JobTracker, moet u verbinding met het Hadoop-cluster met behulp van RDP zoals is beschreven in de vorige sectie. Nadat u verstuurd naar het cluster hebt, dubbelklik op het pictogram **Hadoop naam knooppunt Status** op het bureaublad en klik vervolgens op in de **Logboeken aan de NameNode** om te gaan naar de map van Logboeken op de secundaire hoofd knooppunt.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>De grootte van hoofd knooppunt configureren
De kop knooppunten zijn toegewezen als grote virtuele machines (VMs) al dan niet standaard. Deze grootte is voldoende voor het beheer van de meeste Hadoop taken uitvoeren op het cluster. Er zijn echter scenario's die zeer grote VMs mogelijk nodig voor de kop knooppunten. Een voorbeeld is wanneer het cluster bevat voor het beheren van een groot aantal kleine Oozie taken.

Zeer grote VMs kunnen worden geconfigureerd met behulp van Azure PowerShell-cmdlets of de SDK HDInsight.

Het maken en inrichten van een cluster met behulp van Azure PowerShell wordt beschreven in [HDInsight beheren via PowerShell](hdinsight-administer-use-powershell.md). De configuratie van een zeer grote hoofd knooppunt is vereist voor het toevoegen van de `-HeadNodeVMSize ExtraLarge` -parameter voor de `New-AzureRmHDInsightcluster` cmdlet die in deze code worden gebruikt.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Voor de SDK lijkt het verhaal. Het maken en inrichten van een cluster met behulp van de SDK wordt beschreven in [HDInsight .NET SDK gebruiken](hdinsight-provision-clusters.md#sdk). De configuratie van een zeer grote hoofd knooppunt is vereist voor het toevoegen van de `HeadNodeSize = NodeVMSize.ExtraLarge` -parameter voor de `ClusterCreateParameters()` methode die in deze code wordt gebruikt.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Volgende stappen

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Verbinding maken met HDInsight clusters RDP gebruiken](hdinsight-administer-use-management-portal.md#rdp)
- [Met behulp van HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)
