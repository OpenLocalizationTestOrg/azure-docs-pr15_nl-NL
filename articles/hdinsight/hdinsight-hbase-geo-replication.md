<properties 
   pageTitle="HBase replicatie tussen twee datacenters configureren | Microsoft Azure" 
   description="Informatie over het configureren van HBase herhaling over twee datacenters en over het gebruik aanvragen voor cluster replicatie." 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>HBase geografische-replicatie op HDInsight configureren

> [AZURE.SELECTOR]
- [VPN-connectiviteit configureren](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 
 
Informatie over het configureren van HBase herhaling over twee datacenters. Sommige gevallen gebruiken voor cluster herhaling toe te voegen:

- Back-up en noodgevallen herstel
- Het samenvoegen van gegevens
- Geografische gegevens verdeling
- Online-gegevens opname gecombineerd met offline gegevens analytics

Cluster herhaling maakt gebruik van een bron-push-methode. Een cluster HBase kan een bron, een bestemming, of beide rollen in één keer kunt voldoen. Replicatie is asynchroon, en het doel van herhaling eventuele consistentie. Wanneer de bron een bewerken in een kolom familie met herhaling ingeschakeld ontvangt, wordt deze bewerken wordt doorgegeven aan alle bestemming clusters. Wanneer gegevens worden gerepliceerd uit één cluster naar een andere, worden het bron-cluster en alle clusters waarin de gegevens hebben al worden gebruikt om te voorkomen dat replicatie lussen worden bijgehouden. Voor meer informatie wordt In deze zelfstudie u een bron-doel-replicatie configureren.  Zie voor andere clustertopologieën [Apache HBase Naslaggids voor](http://hbase.apache.org/book.html#_cluster_replication).

Dit is het derde onderdeel van de reeks:

- [Een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-replication-vnet]
- [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns]
- HBase geografische replicatie (deze zelfstudie) configureren

In het volgende diagram ziet u de twee virtuele netwerken en de netwerkverbinding die u hebt gemaakt in [een VPN-verbinding tussen twee virtuele netwerken configureren] [ hdinsight-hbase-geo-replication-vnet] en [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns]: 

![HDInsight HBase herhaling virtuele netwerkdiagram][img-vnet-diagram]

## <a id="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    Als u wilt uitvoeren PowerShell-scripts, moet u Azure PowerShell als administrator uitvoeren en instellen van het beleid kan worden uitgevoerd op *RemoteSigned*. Zie Werken met de cmdlet Set-ExecutionPolicy.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Twee Azure virtuele netwerk met een VPN-verbinding en DNS-geconfigureerd**.  Zie [een VPN-verbinding tussen twee Azure virtuele netwerken configureren]voor instructies[hdinsight-hbase-replication-vnet], en [DNS configureren tussen twee Azure virtuele netwerken][hdinsight-hbase-replication-dns].


    Voordat u PowerShell-scripts, zorg dat er verbinding is met uw Azure-abonnement met de volgende cmdlet:

        Add-AzureAccount

    Als u meerdere Azure abonnementen hebt, gebruikt u de volgende cmdlet voor het instellen van het huidige abonnement:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>HBase clusters in HDInsight inrichten

In [een VPN-verbinding tussen twee Azure virtuele netwerken configureren][hdinsight-hbase-replication-vnet], u kunt een virtueel netwerk hebt gemaakt in een Europa datacenter en een virtueel netwerk in een datacenter van de VS. De twee virtuele netwerk bent verbonden via VPN. In deze sessie, wordt u een HBase cluster in elk van de virtuele netwerken inrichten. Verderop in deze zelfstudie brengt u een van de clusters HBase naar het andere HBase cluster repliceren.

De klassieke Azure-Portal biedt geen ondersteuning inrichten HDInsight clusters met aangepaste configuratieopties. Bijvoorbeeld *hbase.replication* ingesteld op *waar*. Als u de waarde in het configuratiebestand ingesteld nadat een cluster is ingericht, kunt u de instelling kwijt nadat het cluster is wordt reimaged. Zie voor meer informatie [inrichten Hadoop clusters in HDInsight][hdinsight-provision]. Een van de opties voor het inrichten van HDInsight cluster met aangepaste opties Azure PowerShell gebruikt.


**Voor het inrichten van een HBase Cluster in Contoso-VNet-EU** 

1. Open vanuit uw werkstation Windows filter wissen.
2. Stel de variabelen aan het begin van het script en vervolgens het script uitvoeren.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Voor het inrichten van een HBase Cluster in Contoso-VNet-VS** 

- Gebruik hetzelfde script met de volgende waarden:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Omdat u al een verbinding met uw Azure-account hebt gemaakt, moet u niet meer de volgende comlets uitvoeren:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>DNS-voorwaardelijk doorsturen configureren

In [DNS configureren voor de virtuele netwerken][hdinsight-hbase-replication-dns], u DNS-servers voor de twee netwerken hebt geconfigureerd. De clusters HBase hebben verschillende domeinachtervoegsels. U moet dus extra voorwaardelijke DNS-doorstuurservers configureren.

Als u wilt configureren voorwaardelijk doorsturen, moet u de domeinachtervoegsels van de twee HBase clusters weten. 

**De domeinachtervoegsels van de twee HBase clusters zoeken**

1. RDP in **Contoso-HBase-Europese Unie**.  Zie voor instructies [beheren Hadoop clusters in HDInsight via de Portal van Azure-klassieke][hdinsight-manage-portal]. Werkelijk headnode0 van het cluster is.
2. Open een Windows PowerShell-console of een opdrachtprompt.
3. **Ipconfig**uitvoeren en noteer **verbinding specifieke DNS-achtervoegsel**.
4. U moet de RDP-sessie niet sluiten.  Moet u deze later wilt testen domeinnamen omzetten.
5. Herhaal deze stappen om vast te stellen het **achtervoegsel verbinding / regiospecifieke** van **Contoso-HBase-nl**.


**DNS-doorstuurservers configureren**
 
1.  RDP in **Contoso-DNS-Europese Unie**. 
2.  Klik op de Windows-toets in de linkerbenedenhoek.
2.  Klik op **Systeembeheer**.
3.  Klik op **DNS**.
4.  Vouw in het linkerdeelvenster **DSN**, **Contoso-DNS-Europese Unie**.
5.  Met de rechtermuisknop op **Voorwaardelijke doorstuurservers**en klik vervolgens op **Nieuwe voorwaardelijk doorsturen**. 
5.  Voer de volgende gegevens:
    - **DNS-domein**: Voer het achtervoegsel van de Contoso-HBase-VS. Bijvoorbeeld: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-adressen van de basispagina servers**: 10.2.0.4, dat wil de Contoso-DNS-VS van IP-adres zeggen invoeren. Controleer of de IP. Uw DNS-server kan hebben een ander IP-adres.
6.  Druk op **ENTER**en klik vervolgens op **OK**.  Nu is mogelijk om op te lossen de Contoso-DNS-VS van IP-adres van Contoso-DNS-Europese Unie.
7.  Herhaal de stappen voor het toevoegen van een DNS-voorwaardelijk doorsturen naar de DNS-service op de Contoso-DNS-nl virtuele machine met de volgende waarden:
    - **DNS-domein**: Voer het achtervoegsel van de Contoso-HBase-EU. 
    - **IP-adressen van de basispagina servers**: 10.2.0.4, dat wil de Contoso-DNS-EU van IP-adres zeggen invoeren.

**Test domein mailnamen omzetten**

1. Overschakelen naar de Contoso-HBase-Europese Unie RDP-venster.
2. Open een opdrachtprompt.
3. De opdracht ping uitvoeren:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Het protocol ICM is de knooppunten werknemer van de clusters HBase ingeschakeld

4. Sluit de RDP-sessie niet. Nog steeds moet u deze later in deze zelfstudie.
5. Herhaal dezelfde stappen als u wilt de headnode0 van de Contoso-HBase-EU van de Contoso-HBase-VS ping.

>[AZURE.IMPORTANT] DNS moet werken voordat u met de volgende stappen verdergaat.

## <a name="enable-replication-between-hbase-tables"></a>Replicatie tussen HBase tabellen inschakelen

Nu kunt u een voorbeeld HBase-tabel maken, herhaling inschakelen en test deze met enkele gegevens. U wilt gebruiken op de voorbeeldtabel heeft twee kolom gezinnen: Personal en Office. 

In deze zelfstudie brengt u de cluster Europe HBase als het bron-cluster en het cluster Amerikaans HBase als het cluster bestemming.

Maak HBase tabellen met de dezelfde namen en de kolom servicepakketten op de bron- en doeltabellen kolomgroepen zodat het doel-cluster weet opslaglocatie van gegevens ontvangt. Zie voor meer informatie over het gebruik van de shell HBase, [aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started].

**Maken van een tabel HBase op Contoso-HBase-Europese Unie**

1. Overschakelen naar de **Contoso-HBase-Europese Unie** RDP-venster.
2. Vanaf het bureaublad, klikt u op **de opdrachtregel Hadoop**.
2. De map naar de basismap HBase wijzigen:

        cd %HBASE_HOME%\bin
3. De HBase shell opent:

        hbase shell
4. Maak een tabel HBase:

        create 'Contacts', 'Personal', 'Office'
5. Niet, sluit u de RDP-sessie of de opdrachtregel Hadoop-venster. Nog steeds moet u deze later in deze zelfstudie.
    
**Maken van een tabel HBase op Contoso-HBase-nl**

- Herhaal dezelfde stappen als u wilt maken van dezelfde tabel op Contoso-HBase-nl.


**Contoso-HBase-nl toevoegen als een peer herhaling**

1. Overschakelen naar het **Contso-HBase_EU** RDP-venster.
2. Vanuit het venster HBase shell toevoegen u de bestemming cluster (Contoso-HBase-nl) als een peer, bijvoorbeeld:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    In de steekproef is het domeinachtervoegsel *contoso-hbase-us.d4.internal.cloudapp.net*. U moet bijwerken zodat deze overeenkomen met uw domeinachtervoegsel van het cluster ons HBase. Controleer of dat er is geen spaties tussen de hostnamen.

**Elke kolom familie worden gerepliceerd op het bron-cluster configureren**

1. Configureer elke kolom familie worden gerepliceerd vanuit het venster HBase shell van de **Contso-HBase-Europese Unie** RDP-sessie:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Upload-gegevens aan de tabel HBase bulksgewijs**

Een voorbeeld van gegevensbestand is geüpload naar een openbare Azure Blob container met de volgende URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

De inhoud van het bestand:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

U kunt het bestand uploaden naar uw cluster HBase en de gegevens importeren vanaf hier.

1. Overschakelen naar de **Contoso-HBase-Europese Unie** RDP-venster.
2. Vanaf het bureaublad, klikt u op **de opdrachtregel Hadoop**.
3. De map naar de basismap HBase wijzigen:

        cd %HBASE_HOME%\bin

4. Upload de gegevens in:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Controleer of dat gegevensreplicatie plaatsvindt

U kunt controleren of dat replicatie plaatsvindt door te scannen van de tabellen uit beide clusters met de volgende HBase shell-opdrachten:

        Scan 'Contacts'


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe HBase replicatie over twee datacenters configureren. Meer informatie over HDInsight en HBase, raadpleegt u:

- [Pagina van de service HDInsight](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight documentatie](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Aan de slag met Apache HBase in HDInsight][hdinsight-hbase-get-started]
- [HDInsight HBase overzicht][hdinsight-hbase-overview]
- [HBase clusters op Azure Virtual Network inrichten][hdinsight-hbase-provision-vnet]
- [Realtime Twitter sentiment met HBase analyseren][hdinsight-hbase-twitter-sentiment]
- [Analyseren van sensorgegevens met Storm en HBase in HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
