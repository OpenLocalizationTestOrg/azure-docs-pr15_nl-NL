<properties
    pageTitle="HBase clusters maken in een virtueel netwerk | Microsoft Azure"
    description="Aan de slag met HBase in Azure HDInsight. Informatie over het maken van HDInsight HBase clusters op Azure Virtual Network."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>HBase clusters in Azure Virtual Network maken 

Informatie over het maken van Azure HDInsight HBase clusters in een [Azure Virtual Network][1].

Dankzij virtuele kunnen HBase clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen met HBase rechtstreeks communiceren kunnen. Biedt de volgende voordelen:

- Directe verbinding van de webtoepassing de knooppunten van het cluster HBase, waarmee communicatie via HBase Java remote procedure bellen (RPC)-API's.
- Verbeterde prestaties doordat u niet hoeft uw verkeer Ga over meerdere gateways en netwerktaakverdelers.
- De mogelijkheid om het verwerken van vertrouwelijke informatie op een veiligere manier zonder dat een openbare-eindpunt.

###<a name="prerequisites"></a>Vereisten voor
Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**. Zie [installeren en gebruik Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase cluster virtuele netwerk maken

In dit gedeelte maakt u een cluster Linux gebaseerde HBase met de afhankelijke Azure Storage-account in een Azure virtuele netwerk met een [resourcemanager Azure-sjabloon](../resource-group-template-deploy.md). De instellingen, Zie voor andere methoden voor het maken van cluster en het begrip [clusters HDInsight maken](hdinsight-hadoop-provision-linux-clusters.md). Zie voor meer informatie over het gebruik van een sjabloon maken Hadoop clusters in HDInsight [maken Hadoop-clusters in HDInsight Azure resourcemanager sjablonen gebruiken](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Bepaalde eigenschappen zijn hard gecodeerde in de sjabloon. Bijvoorbeeld:
>
> * __Locatie__: Oost Amerikaans 2
> * __Cluster versie: 3.4
> * __Cluster werknemer knooppunt tellen__: 4
> * __Opslag standaardaccount__: &lt;Clusternaam > opslaan
> * __Virtuele netwerknaam__: &lt;Clusternaam >-vnet
> * __Virtuele netwerk-adresruimte__: 10.0.0.0/16
> * __De naam van de subnet__: standaard
> * __Subnet adresbereiken__: 10.0.0.0/24
>
> &lt;Cluster naam > wordt vervangen door de naam van het cluster die u hebt opgegeven bij gebruik van de sjabloon.

1. Klik op de volgende afbeelding om te openen van de sjabloon in de portal van Azure. De sjabloon bevindt zich in een container openbare blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van het blad **aangepaste-implementatie** :

    - **Abonnement**: Selecteer een Azure-abonnement gebruikt het cluster HDInsight, het afhankelijke opslag-account en het Azure virtuele netwerk maken.
    - **Resourcegroep**: Selecteer **nieuwe maken**en geef een nieuwe groepsnaam voor de resource.
    - **Locatie**: Selecteer een locatie voor de resourcegroep.
    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster aanmeldingsnaam en wachtwoord**: de standaard-aanmeldingsnaam is **beheerder**.
    - **SSH gebruikersnaam en wachtwoord**: de standaard-gebruikersnaam is **sshuser**.  U kunt dit wijzigen. 
    - **Ik ga akkoord met de voorwaarden en de bovenstaande voorwaarden**: (Selecteer)
    

3. Klik op **aanschaffen**. Het duurt over ongeveer 20 minuten een cluster maken. Nadat het cluster is gemaakt, kunt u klikken op het blad cluster in de portal om dit te openen.

Nadat u de zelfstudie hebt voltooid, wilt u mogelijk het cluster verwijderen. Uw gegevens worden opgeslagen in Azure-opslag, met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer deze niet gebruikt wordt. U wordt ook geheven voor een cluster HDInsight, zelfs wanneer deze niet gebruikt wordt. Aangezien de kosten voor het cluster vaak meer dan de kosten voor opslag zijn, relevant dat is economic clusters verwijderen wanneer ze niet gebruikt worden. Zie de instructies voor het verwijderen van een cluster [beheren Hadoop clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-management-portal.md#delete-clusters).

Als u wilt gaan werken met uw nieuwe HBase cluster, kunt u de procedures die zijn gevonden in [aan de slag met HBase met Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Verbinding maken met de HBase cluster met HBase Java RPC API 's

1.  Maak een infrastructuur voor als een service (IaaS) virtuele machine in hetzelfde Azure virtuele netwerk en hetzelfde subnet. Zie voor instructies over het maken van een nieuwe IaaS virtuele machine [maken een virtuele Machine uitvoeren Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Wanneer u de stappen in dit document, moet u de volgende handelingen uit voor de netwerkconfiguratie:

    - __Virtuele netwerk__: &lt;Clusternaam >-vnet
    - __Subnet__: standaard

    > [AZURE.IMPORTANT] Vervang &lt;Clusternaam > met de naam die u hebt gebruikt bij het maken van het HDInsight cluster in de voorgaande stappen.

    Met deze waarden, wordt de virtuele machine voor hetzelfde virtuele netwerk en subnet gebruiken als het cluster HDInsight configureren. Hierdoor kunnen ze rechtstreeks met elkaar communiceren.

2.  Als u een Java-toepassing voor de verbinding maken met HBase televergaderen, moet u de FQDN-naam (Fully Qualified). Hiervoor moet u de verbinding DNS-achtervoegsel van het cluster HBase ophalen. Als u wilt dat doet, kunt u een van de volgende methoden gebruiken:

    * Een webbrowser gebruiken om een Ambari gesprek te voeren:
    
        Blader naar https://&lt;Clusternaam >.azurehdinsight.net/api/v1/clusters/&lt;Clusternaam > / hosts?minimal_response = true. Deze verandert een JSON-bestand met de DNS-achtervoegsels.

    * Gebruik de Ambari-website:

        1. Blader naar https://&lt;Clusternaam >. azurehdinsight.net.
        2. Klik op **Hosts** in het bovenste menu.

    * Gebruik krul REST gesprekken:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Zoek in de notatie voor JavaScript-Object (JSON) gegevens geretourneerd, het fragment "hostnaam". Hiermee wordt de FQDN-naam voor de knooppunten in het cluster bevatten. Bijvoorbeeld:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Het gedeelte van het domein naam die begint met de naam van het cluster is het achtervoegsel. Bijvoorbeeld mycluster.b1.cloudapp.net.

    * Azure PowerShell gebruiken
    
        Gebruik het volgende Azure PowerShell-script om te registreren van de functie **Get-ClusterDetail** , die kan worden gebruikt om te retourneren van het achtervoegsel:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Nadat het Azure PowerShell-script uitgevoerd, gebruikt u de volgende opdracht uit om te retourneren van het achtervoegsel met behulp van de functie **Get-ClusterDetail** . Geef uw HDInsight HBase clusternaam, de beheerdersnaam van de en beheerderswachtwoord wanneer u deze opdracht gebruikt.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Hiermee herstelt u de DNS-achtervoegsel. Bijvoorbeeld: **yourclustername.b4.internal.cloudapp.net**.

    * RDP gebruiken
    
        U kunt ook extern bureaublad verbinding maken met de HBase cluster (u wordt verbonden met het hoofd knooppunt) en **ipconfig** uitvoeren vanaf de opdrachtregel voor het verkrijgen van het achtervoegsel. Zie voor instructies over het inschakelen van Remote Desktop Protocol (RDP) en verbinding maken met het cluster met RDP, [Hadoop beheren clusters in met behulp van de portal Azure HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Als u wilt controleren of de virtuele machine kunt communiceren met het cluster HBase, gebruikt u de opdracht `ping headnode0.<dns suffix>` uit de virtuele machine. Bijvoorbeeld: ping headnode0.mycluster.b1.cloudapp.net.

Als u wilt deze gegevens gebruiken in een Java-toepassing, kunt u de stappen in [Maven Java-toepassingen die gebruikmaken van HBase met HDInsight (Hadoop) gebruiken](hdinsight-hbase-build-java-maven.md) om te maken van een toepassing. Als u wilt dat de toepassing die verbinding maken met een externe HBase-server, door het bestand **hbase-site.xml** in dit voorbeeld gebruik van de FQDN-naam voor de Zookeeper te wijzigen. Bijvoorbeeld:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Virtuele netwerken, waaronder over het gebruik van uw eigen DNS-server, Zie voor meer informatie over het naamresolutie in Azure wordt aangegeven [Name resolutie (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een cluster HBase maakt. Meer informatie raadpleegt u:

- [Aan de slag met HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Replicatie van HBase op HDInsight configureren](hdinsight-hbase-geo-replication.md)
- [Hadoop clusters in HDInsight maken](hdinsight-provision-clusters.md)
- [Aan de slag met HBase met Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Twitter sentiment met HBase in HDInsight analyseren](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Overzicht van Virtual Network][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Details van de voorziening voor het nieuwe HBase cluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Gebruik scriptactie om een cluster HBase aanpassen"

[azure-preview-portal]: https://portal.azure.com
