<properties
    pageTitle="Hadoop Sqoop gebruiken in HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van Azure PowerShell vanaf een werkstation uitvoeren Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL-database."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Sqoop gebruiken met Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Sqoop in HDInsight importeren en exporteren tussen HDInsight cluster en Azure SQL-database of SQL Server-database.

Hoewel Hadoop natuurlijke kiezen voor het verwerken van ongestructureerde en semistructured gegevens, zoals Logboeken en bestanden, is er mogelijk ook een nodig om gestructureerde gegevens die zijn opgeslagen in relationele databases te verwerken.

[Sqoop] [ sqoop-user-guide-1.4.4] is een hulpprogramma waarmee gegevens uitwisselen tussen Hadoop clusters en relationele databases. U kunt deze gegevens importeren uit een relationele database management (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop met MapReduce of component transformeren en exporteer het vervolgens de gegevens terug naar een RDBMS. In deze zelfstudie gebruikt u een SQL Server-database voor de relationele database.

Zie voor versies Sqoop die worden ondersteund op HDInsight clusters [Wat is er nieuw in de cluster versies geleverd door HDInsight?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Meer informatie over het scenario

HDInsight cluster wordt geleverd met voorbeeldgegevens. Gebruikt u de volgende twee voorbeelden:

- Het logboekbestand van een log4j, dat zich bevindt op */example/data/sample.log*. De logboeken aan de volgende worden opgehaald uit het bestand:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Een component-tabel met de naam *hivesampletable*, die verwijst naar het gegevensbestand */hive/warehouse/hivesampletable*zich bevindt. De tabel bevat enkele gegevens mobiel apparaat. 

  	| Veld | Gegevenstype |
  	| ----- | --------- |
  	| clientid | tekenreeks |
  	| querytime | tekenreeks |
  	| market | tekenreeks |
  	| deviceplatform | tekenreeks |
  	| devicemake | tekenreeks |
  	| devicemodel | tekenreeks |
  	| de staat | tekenreeks |
  	| land | tekenreeks |
  	| querydwelltime | dubbele |
  	| sessie-id | bigint |
  	| sessionpagevieworder | bigint |

U wordt *sample.log* en *hivesampletable* eerst exporteren naar de SQL Azure-database of SQL Server en importeer de tabel waarin de gegevens terug naar HDInsight op mobiele apparaten met behulp van het volgende pad:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Cluster en SQL-database maken

In dit gedeelte ziet u hoe u een cluster en de SQL-database schema's voor het uitvoeren van de zelfstudie gebruik van de Azure-portal en een resourcemanager Azure-sjabloon maken.  Als u liever Azure PowerShell gebruiken, raadpleegt u de [bijlage A](#appendix-a---a-powershell-sample).

1. Klik op de volgende afbeelding een resource manager als sjabloon wilt openen in de Portal Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De resource manager sjabloon bevindt zich in een openbare blob container, *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    De resource manager sjabloon roept een Bacpac-pakket als u wilt implementeren van de tabel schema's maken met SQL-database.  Het pakket Bacpac-bevindt zich ook in een openbare blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Als u een privé container gebruiken voor de Bacpac-bestanden wilt, gebruikt u de volgende waarden in de sjabloon:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Voer de volgende gegevens van het blad Parameters:

    - **Clusternaam**: Voer een naam voor het Hadoop-cluster dat u wilt maken.
    - **Cluster aanmeldingsnaam en wachtwoord**: de standaard-aanmeldingsnaam is beheerder.
    - **SSH-gebruikersnaam en wachtwoord**.
    - **Aanmeldingsnaam voor SQL database-server en wachtwoord**.

    De volgende waarden zijn vastgelegde in de sectie variabelen:
    
  	|Standaard-opslagaccountnaam|<CluterName>Store|
  	|----------------------------|-----------------|
  	|Azure SQL database-servernaam|<ClusterName>dbserver|
  	|Azure SQL database-naam|<ClusterName>DB|
    
    Noteer deze waarden.  U moet ze later in deze zelfstudie.
    
3. Klik op **OK** als u wilt opslaan van de parameters.

4. van het blad **aangepaste implementatie** op **resourcegroep** vervolgkeuzelijst in en klik vervolgens op **Nieuw** om een nieuwe resourcegroep te maken. De resourcegroep is een container waarin het cluster, het afhankelijke opslag-account en andere gekoppelde resource worden gegroepeerd.

5. op **juridische voorwaarden**en klik vervolgens op **maken**.

6. Klik op **maken**. Hier ziet u een nieuwe tegel getiteld Submitting implementatie voor implementatie van de sjabloon. Het duurt over ongeveer 20 minuten om het cluster en SQL-database te maken.

Als u bestaande Azure SQL-database of Microsoft SQL Server gebruiken

- **Azure SQL-database**: moet u een firewallregel voor de Azure SQL database-server om toegang te krijgen vanuit uw werkstation configureren. Voor instructies over het maken van een Azure SQL database en configureren van de firewall, raadpleegt u [aan de slag met Azure SQL-database][sqldatabase-get-started]. 

    > [AZURE.NOTE] Een Azure SQL-database staat standaard verbindingen van Azure services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u deze van de Azure portal ingeschakeld. Zie [maken en SQL-Database configureren]voor instructies over het maken van een Azure SQL-database en firewallregels configureren,[sqldatabase-create-configue].

- **SQL Server**: als uw cluster HDInsight op hetzelfde virtual netwerk in Azure wordt aangegeven als SQL Server, kunt u de stappen in dit artikel om te importeren en exporteren van gegevens naar een SQL Server-database.

    > [AZURE.NOTE] HDInsight ondersteunt alleen locatie gebaseerde virtuele netwerken en deze niet werkt momenteel met virtuele affiniteit groep-netwerken.

    * Als u wilt maken en configureren van een virtueel netwerk, Zie [maken een virtueel netwerk met behulp van de Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Wanneer u SQL Server in uw datacenter gebruikt, moet u het virtuele netwerk configureren als *site-naar-site* of *punt-naar-site*.

            > [AZURE.NOTE] Virtuele netwerken **punt-naar-site** , wordt SQL Server uitgevoerd de VPN-client configuratie-toepassing, dat beschikbaar via het **Dashboard** van uw Azure virtuele netwerkconfiguratie is.

        * Wanneer u SQL Server op een Azure virtuele machine gebruikt, kan een virtuele netwerkconfiguratie worden gebruikt als de virtuele machine hostingprovider van SQL Server een lid van het dezelfde virtuele netwerk als HDInsight is.

    * Zie [maken Hadoop clusters in HDInsight met aangepaste opties](hdinsight-provision-clusters.md) voor informatie over het maken van een cluster HDInsight op een virtueel netwerk

    > [AZURE.NOTE] SQL Server moet ook toestaan van verificatie. U moet een SQL Server-aanmelding gebruiken om de stappen in dit artikel te voltooien.


## <a name="run-sqoop-jobs"></a>Sqoop taken uitvoeren

HDInsight kunt Sqoop taken uitvoeren met behulp van verschillende methoden. Gebruik van de volgende tabel om te bepalen welke methode is geschikt voor u en voert u de koppeling voor stapsgewijze instructies.

| **Gebruik deze optie** als u wilt dat...                                   | .. .an **interactieve** shell | ... **batchbestand** | .. .door dit **cluster besturingssysteem** | .. .from dit **client-besturingssysteem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X of Windows        |
| [.NET-SDK voor Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux of Windows                          | Windows (voor nu)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux of Windows                          | Windows                                  |

##<a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - HDInsight met Linux gebaseerde, de Sqoop verbindingslijn die wordt gebruikt om gegevens te exporteren naar Microsoft SQL Server of Azure SQL-Database ondersteunt momenteel niet bulksgewijs invoegen.

* Batchen - met Linux gebaseerde HDInsight, bij gebruik van de `-batch` overschakelt bij het uitvoeren van wordt ingevoegd, Sqoop meerdere wordt ingevoegd in plaats van de invoegbewerkingen batchen wordt uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe Sqoop gebruiken. Meer informatie raadpleegt u:

- [Component gebruiken met HDInsight](hdinsight-use-hive.md)
- [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
- [Oozie gebruiken met HDInsight][hdinsight-use-oozie]: gebruik Sqoop actie in een werkstroom Oozie.
- [Analyseren van gegevens over vertragingen flight met HDInsight][hdinsight-analyze-flight-data]: component gebruiken om te analyseren flight uitstellen gegevens en gebruikt u Sqoop gegevens exporteren naar een Azure SQL-database.
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]: zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag.


## <a name="appendix-a---a-powershell-sample"></a>Bijlage A - een PowerShell-steekproef

De PowerShell-voorbeeld kunt u de volgende stappen uitvoeren:

1. Verbinding maken met Azure.
2. Maak een Azure resourcegroep. Zie voor meer informatie [Azure PowerShell gebruiken met Azure resourcemanager](../powershell-azure-resource-manager.md)
3. Een Azure SQL Database-server, een Azure SQL-database en twee tabellen maken. 

    Als u in plaats daarvan SQL Server gebruikt, gebruikt u de volgende instructies om de tabellen te maken:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    De eenvoudigste manier om te bekijken van de database en de tabellen is Visual Studio gebruiken. De database-server en de database kunnen worden onderzocht met behulp van de Azure portal.

4. Maak een cluster HDInsight.

    Als u wilt onderzoeken welke gevolgen het cluster, kunt u de Azure beheerportal of Azure PowerShell.

5. Vooraf verwerkt door het bron-gegevensbestand.

    In deze zelfstudie wordt u een logboekbestand log4j (een bestand met scheidingstekens) en een component-tabel exporteren naar een Azure SQL-database. Het bestand met scheidingstekens heet */example/data/sample.log*. Eerder in deze zelfstudie, hebt u enkele voorbeelden van log4j logboeken gezien. In het logboekbestand zijn er enkele lege regels en enkele regels zoals de volgende:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Dit is in orde zijn voor andere voorbeelden die deze gegevens gebruiken, maar deze uitzonderingen moet worden verwijderd voordat we in de SQL Azure-database of SQL Server kunt importeren. Sqoop exporteren, mislukt als er een lege tekenreeks of een lijn met een kleiner aantal elementen dan het aantal velden die zijn gedefinieerd in de tabel Azure SQL-database. De tabel log4jlogs heeft 7 velden van het type tekenreeks.

    Deze procedure wordt gemaakt van een nieuw bestand in het cluster: tutorials/usesqoop/data/sample.log. Als u wilt onderzoeken welke gevolgen het gewijzigde gegevensbestand, kunt u de Azure-portal, een Azure Storage explorer hulpmiddel of Azure PowerShell. [Aan de slag met HDInsight] [ hdinsight-get-started] heeft van een steekproef van de code voor het gebruik van Azure PowerShell downloaden van een bestand en de bestandsinhoud weergeven.

6. Een gegevensbestand exporteren met de SQL Azure-database.

    Het bronbestand is tutorials/usesqoop/data/sample.log. De tabel waar de gegevens worden geëxporteerd naar heet log4jlogs.
    
    > [AZURE.NOTE] De stappen in deze sectie moeten dan verbindingsinformatie werken voor een Azure SQL-database of SQL Server. Deze stappen zijn getest met behulp van de volgende configuratie:
    >
    > * **Azure virtuele punt-naar-site netwerkconfiguratie**: een virtueel netwerk het cluster HDInsight verbonden met een SQL Server in een privé-datacenter. Zie [een VPN-verbinding van het punt-naar-Site in de beheerportal configureren](../vpn-gateway/vpn-gateway-point-to-site-create.md) voor meer informatie.
    > * **Azure HDInsight 3.1**: Zie [maken Hadoop clusters in HDInsight met aangepaste opties](hdinsight-provision-clusters.md) voor informatie over het maken van een cluster op een virtueel netwerk.
    > * **SQL Server-2014**: is geconfigureerd voor het toestaan van verificatie en uit te voeren van de VPN-client configuratiepakket naar veilige verbinding met het virtuele netwerk.

7. Een component-tabel exporteren naar de SQL Azure-database.

8. De tabel mobiledata importeren met het cluster HDInsight.

    Als u wilt onderzoeken welke gevolgen het gewijzigde gegevensbestand, kunt u de Azure-portal, een Azure Storage explorer hulpmiddel of Azure PowerShell.  [Aan de slag met HDInsight] [ hdinsight-get-started] heeft van een steekproef code over het gebruik van Azure PowerShell downloaden van een bestand en de bestandsinhoud weergeven.


### <a name="the-powershell-sample"></a>De PowerShell-voorbeeld

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
