<properties
    pageTitle="Analyseren van gegevens over vertragingen flight met Hadoop in HDInsight | Microsoft Azure"
    description="Informatie over het gebruik van één Windows PowerShell-script maken van een cluster HDInsight, een component taak uitvoeren, een Sqoop taak uitvoeren en het cluster verwijderen."
    services="hdinsight"
    documentationCenter=""
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Gegevens over vertragingen flight analyseren met behulp van component in HDInsight

Component kunt u het uitvoeren van de Hadoop MapReduce taken van een SQL-achtige scripttaal genoemd * [HiveQL][hadoop-hiveql]*, die kunnen worden toegepast richting samenvatten en analyseren van grote hoeveelheden gegevens query's uitvoeren.

> [AZURE.NOTE] De stappen in dit document moet een cluster HDInsight op basis van Windows. Zie voor instructies die met een cluster Linux gebaseerde werken, [vluchtgegevens met vertraging van analyseren met behulp van component in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Een van de belangrijkste voordelen van Azure HDInsight is de scheiding van gegevensopslag en berekeningscluster. Azure-blobopslag HDInsight gebruikt voor gegevensopslag. Een normale taak bestaat uit drie delen:

1. **Opslag van gegevens in Azure-blobopslag.**  Bijvoorbeeld weerbericht gegevens, sensorgegevens, weblogs en in dit geval vluchtgegevens van een vertraging in Azure-blobopslag worden opgeslagen.
2. **Taken uitvoeren.** Wanneer het tijd om de gegevens te verwerken is, loopt u een Windows PowerShell-script (of een clienttoepassing) een cluster HDInsight maken en verwijderen van het cluster taken uitvoeren. De taken opslaan uitvoergegevens met Azure-blobopslag. De uitvoergegevens blijft behouden, zelfs nadat het cluster wordt verwijderd. Op deze manier u betalen alleen wat u hebt gebruikt.
3. **De uitvoer van Azure-blobopslag ophalen**, of in deze zelfstudie kunt u de gegevens exporteren naar een Azure SQL-database.

In het volgende diagram ziet u het scenario en de structuur van deze zelfstudie:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Opmerking**: de getallen in het diagram komen overeen met de naam van een sectie. **M** staat voor het belangrijkste proces. **A** staat voor de inhoud in de bijlage.

Het belangrijkste deel van de zelfstudie ziet u hoe u een Windows PowerShell-script met de volgende taken uitvoeren:

- Maak een cluster HDInsight.
- Een taak onderdeel uitvoeren op het cluster voor het berekenen van gemiddelde vertragingen op luchthavens. De gegevens over vertragingen flight wordt opgeslagen in een Azure Blob storage-account.
- Een taak Sqoop om de uitvoer van de taak component exporteren naar een Azure SQL-database uitvoeren.
- Verwijder het cluster HDInsight.

In de bijlagen vindt u de instructies voor het uploaden van vluchtgegevens van een vertraging, een queryreeks component maken/uploaden en de SQL Azure-database voorbereiden voor de taak Sqoop.

> [AZURE.NOTE] De stappen in dit document zijn specifiek voor Windows gebaseerde HDInsight clusters. Zie voor informatie die met een cluster Linux gebaseerde werken [analyseren flight gegevens over vertragingen met component in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, moet u de volgende items:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Bestanden die worden gebruikt in deze zelfstudie**

In deze zelfstudie wordt de prestaties op tijd van luchtvaartmaatschappijen vluchtgegevens kunnen [onderzoeken en innovatieve technologie beheer, Bureau van transport statistieken of RITA] [rita-website]. Een kopie van de gegevens is geüpload naar een Azure Blob storage container met de machtiging voor openbare Blob-toegang. Een deel van de PowerShell-script kopieert de gegevens van de container openbare blob aan de container standaard blob van uw cluster. Het script HiveQL wordt ook aan dezelfde Blob container gekopieerd. Als u weten hoe wilt u de gegevens in uw eigen account opslag get/uploaden en hoe u de HiveQL script-bestand maken/uploaden, raadpleegt u [bijlage A](#appendix-a) en [B van de bijlage](#appendix-b).

De volgende tabel bevat de bestanden die in deze zelfstudie gebruikt:

<table border="1">
<tr><th>Bestanden</th><th>Beschrijving</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Het HiveQL-scriptbestand gebruikt door de component-taak. Dit script is geüpload naar een Azure Blob opslag account in bij de toegang van het publiek. <a href="#appendix-b">Bijlage B</a> bevat instructies voor het voorbereiden en dit bestand uploaden naar uw eigen Azure Blob storage-account.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>De invoergegevens voor de taak component. De gegevens is geüpload naar een Azure Blob opslag account in bij de toegang van het publiek. <a href="#appendix-a">Bijlage A</a> bevat instructies voor de gegevens zijn opgehaald en de gegevens uploaden naar uw eigen Azure Blob storage-account.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Het uitvoerpad voor de taak component. De standaardcontainer wordt gebruikt voor het opslaan van de uitvoergegevens.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>De component taak status-map op de standaardcontainer.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Cluster maken en uitvoeren van component/Sqoop taken

Hadoop MapReduce is batchbestand. De meest efficiënte manier om het uitvoeren van een taak onderdeel is maken van een cluster voor de taak en de taak verwijderen nadat de taak is voltooid. Het volgende script behandelt het hele proces. Zie voor meer informatie over het maken van een cluster HDInsight en uitvoeren van taken component [maken Hadoop-clusters in HDInsight] [ hdinsight-provision] en [Gebruik component met HDInsight][hdinsight-use-hive].

**De component query's uitvoeren door Azure PowerShell**

1. Maak een Azure SQL-database en de tabel voor de uitvoer van de taak Sqoop met behulp van de instructies in de [Bijlage C](#appendix-c).
3. Open Windows filter wissen en voer het volgende script:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Verbinding maken met uw SQL-database en gemiddelde vluchtvertragingen te zien zijn op plaats in de tabel AvgDelays weergegeven:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Bijlage A - Upload vluchtgegevens vertraging met Azure-blobopslag
Het gegevensbestand en de HiveQL script-bestanden (Zie [Bijlage B](#appendix-b)) uploaden vereist een planning. Het idee is voor de opslag van de bestanden en het bestand HiveQL voordat u een cluster HDInsight maken en uitvoeren van de taak component. U hebt twee opties:

- **Gebruik hetzelfde opslag van Azure-account dat wordt gebruikt door het cluster HDInsight als het standaard-bestandssysteem.** Omdat het cluster HDInsight de toegangstoets voor opslag-account hebt wordt, moet u niet Breng eventuele aanvullende wijzigingen.
- **Gebruik een ander account Azure opslag van het bestandssysteem HDInsight cluster standaard.** Als dit het geval is, moet u het deel van het maken van de Windows PowerShell-script gevonden in [cluster HDInsight maken en uitvoeren component/Sqoop taken](#runjob) voor de opslag-account als een extra opslagruimte account koppelen wijzigen. Zie voor instructies [maken Hadoop clusters in HDInsight][hdinsight-provision]. Het cluster HDInsight kent de access-toets voor de opslag-account.

>[AZURE.NOTE] Het Blob storage pad voor het gegevensbestand is moeilijk gecodeerd in het HiveQL script-bestand. U moet deze dienovereenkomstig bijwerken.

**De vluchtgegevens downloaden**

1. Blader naar de [onderzoeks- en beheer van innovatieve technologie Bureau of transport Statistics][rita-website].
2. Klik op de pagina, selecteert u de volgende waarden:

    <table border="1">
    <tr><th>Naam</th><th>Waarde</th></tr>
    <tr><td>Filter jaar</td><td>2013 </td></tr>
    <tr><td>Filteren van de termijn</td><td>Januari</td></tr>
    <tr><td>Velden</td><td>*Jaar*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *oorsprong*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (Schakel alle andere velden)</td></tr>
    </table>

3. Klik op **downloaden**.
4. Pak het bestand naar de map **C:\Tutorials\FlightDelay\2013Data** . Elk bestand is van een CSV-bestand en ongeveer 60GB groot is.
5.  Naam van het bestand op de naam van de maand die deze gegevens voor bevat. Bijvoorbeeld het bestand met de gegevens januari naam *January.csv*.
6. Herhaal stap 2 en 5 om te downloaden van een bestand voor elk van de twaalf maanden in 2013. Moet u minimaal één bestand de zelfstudie uitvoeren.  

**De gegevens over vertragingen flight uploaden naar Azure-blobopslag**

1. De parameters voorbereiden:

    <table border="1">
    <tr><th>De naam van de variabele</th><th>Notities</th></tr>
    <tr><td>$storageAccountName</td><td>De opslag van Azure-account waar u de gegevens wilt uploaden.</td></tr>
    <tr><td>$blobContainerName</td><td>De container Blob is waar u de gegevens wilt uploaden.</td></tr>
    </table>
2. Open Azure filter wissen.
3. Plak de volgende script in het deelvenster script:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Druk op **F5** het script uitvoeren.

Als u besluit om een andere methode voor het uploaden van de bestanden gebruiken, Controleer of dat het bestandspad is zelfstudies/flightdelay/gegevens. De syntaxis voor toegang tot de bestanden is:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Het pad zelfstudies/flightdelay/gegevens is de virtuele map die u hebt gemaakt wanneer u de bestanden hebt geüpload. Controleer of er 12-bestanden, één voor elke maand.

>[AZURE.NOTE] U moet de component bijwerkquery te lezen van de nieuwe locatie.

> U moet op de container toegangsmachtiging openbaar of binden van de opslag-account aan het cluster HDInsight configureren. Anders is de queryreeks component niet mogelijk voor toegang tot de gegevensbestanden.

---
##<a id="appendix-b"></a>Bijlage B - maken en uploaden van een HiveQL-script

Azure PowerShell gebruikt, kunt u meerdere HiveQL instructies één tegelijkertijd worden uitgevoerd, of de instructie HiveQL in een scriptbestand als pakket inpakken. In dit gedeelte ziet u hoe u ook een script HiveQL maken en uploaden van het script met Azure-blobopslag via Azure PowerShell. Component is vereist voor de scripts HiveQL moet worden opgeslagen in Azure-blobopslag.

Het script HiveQL wordt als volgt te werk:

1. **Sleep de tabel delays_raw**, bestaat in geval de tabel al.
2. **De delays_raw externe component tabel maken** die wijst naar de opslaglocatie van Blob met de bestanden die flight vertraging. Deze query geeft aan dat de velden worden gescheiden door "," en dat lijnen met "\n eindigen". Dit is een probleem wanneer veldwaarden komma's bevatten omdat component kan geen onderscheid maken tussen een komma die is van een scheidingsteken voor velden en één die deel uitmaakt van de waarde van een veld (namelijk de hoofdletters/kleine letters in veldwaarden voor ORIGIN\_STAD\_naam en DEST\_STAD\_naam). Om dit op te lossen, wordt de query gemaakt TEMP kolommen bevatten gegevens die onjuist in kolommen splitsen is.  
3. **Sleep de tabel vertragingen**, bestaat in geval de tabel al.
4. **De tabel vertragingen maken**. Is het handig om op te schonen de gegevens voor nadere verwerking. Deze query maakt een nieuwe tabel, *vertragingen*uit de tabel delays_raw. Houd er rekening mee dat de kolommen TEMP (zoals eerder is vermeld) niet worden gekopieerd, en dat de functie **subtekenreeks** aanhalingstekens verwijderen uit de gegevens worden gebruikt.
5. **De gemiddelde weer vertraging en groepen de resultaten plaatsnaam berekenen.** Dit wordt ook de resultaten met Blob storage uitvoeren. Houd er rekening mee dat de query apostroffen worden verwijderd uit de gegevens en rijen waar de waarde voor **weather_delay** null is wordt uitgesloten. Dit is nodig omdat Sqoop, verderop in deze zelfstudie gebruikt niet zonder problemen deze waarden al dan niet standaard verwerken.

Zie voor een volledige lijst met de opdrachten HiveQL, [Component Data Definition Language][hadoop-hiveql]. Elke opdracht HiveQL moet eindigen met een puntkomma.

**Een HiveQL script-bestand maken**

1. De parameters voorbereiden:

    <table border="1">
    <tr><th>De naam van de variabele</th><th>Notities</th></tr>
    <tr><td>$storageAccountName</td><td>De opslag van Azure-account waarnaar u wilt uploaden van het script HiveQL naar.</td></tr>
    <tr><td>$blobContainerName</td><td>De container Blob is waar u het script HiveQL om te uploaden.</td></tr>
    </table>
2. Open Azure filter wissen.

3. Kopieer en plak de volgende script naar het deelvenster script:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Hier volgen de variabelen in het script gebruikt:

    - **$hqlLocalFileName** - slaat het script het HiveQL script-bestand lokaal voordat u deze uploadt naar Blob storage. Dit is de bestandsnaam. De standaardwaarde is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** - dit is de HiveQL script blob bestandsnaam in het Azure-blobopslag gebruikt. De standaardwaarde is tutorials/flightdelay/flightdelays.hql. Omdat het bestand zal rechtstreeks met Azure-blobopslag worden geschreven, is er een "/" aan het begin van de naam van de blob. Als u het bestand openen vanuit blobopslag wilt, moet u een "/" aan het begin van de bestandsnaam toevoegen.
    - **$srcDataFolder** en **$dstDataFolder** - = "flightdelay-zelfstudies-gegevens" = "zelfstudies/flightdelay/uitvoer"


---
##<a id="appendix-c"></a>Bijlage C - een Azure SQL-database voorbereiden voor de uitvoer van de taak Sqoop
**Voor het voorbereiden van de SQL-database (samenvoegen dit met het script Sqoop)**

1. De parameters voorbereiden:

    <table border="1">
    <tr><th>De naam van de variabele</th><th>Notities</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>De naam van de Azure SQL database-server. Typ niets als u wilt maken van een nieuwe server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>De aanmeldingsnaam voor de Azure SQL database-server. Als $sqlDatabaseServerName een bestaande server is, worden de aanmeldingsnaam en het wachtwoord voor aanmelden bij worden gebruikt om te verifiëren met de server. Ze worden anders gebruikt om een nieuwe server te maken.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Het aanmeldingswachtwoord voor de Azure SQL database-server.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Deze waarde wordt alleen gebruikt als u een nieuwe Azure-database-server maakt.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>De SQL-database gebruikt om de tabel AvgDelays voor de taak Sqoop te maken. Waardoor het leeg maakt een database HDISqoop genoemd. De naam van de tabel voor de uitvoer van de taak Sqoop is AvgDelays. </td></tr>
    </table>
2. Open Azure filter wissen.
3. Kopieer en plak de volgende script naar het deelvenster script:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Het script wordt een beschrijvende state doorverbinden (REST)-service, http://bot.whatismyipaddress.com, gebruikt om op te halen van uw externe IP-adres. Het IP-adres wordt gebruikt voor het maken van een firewallregel voor uw SQL database-server.  

    Hier volgen enkele variabelen in het script gebruikt:

    - **$ipAddressRestService** - de standaardwaarde is http://bot.whatismyipaddress.com. Een openbare IP-adres REST-service om uw externe IP-adres voor is. U kunt andere services als u wilt gebruiken. Het externe IP-adres dat is opgehaald door de service wordt gebruikt om een firewallregel voor uw Azure SQL database-server te maken zodat u toegang hebt tot de database vanuit uw werkstation (met behulp van een Windows PowerShell-script).
    - **$fireWallRuleName** - dit is de naam van de firewallregel van de Azure SQL database-server. De standaardnaam is <u>FlightDelay</u>. Als u wilt, kunt u deze wijzigen.
    - **$sqlDatabaseMaxSizeGB** - deze waarde wordt alleen gebruikt wanneer u een nieuwe Azure SQL database-server maakt. De standaardwaarde is 10GB. 10GB is voldoende voor deze zelfstudie.
    - **$sqlDatabaseName** - deze waarde wordt alleen gebruikt wanneer u een nieuwe Azure SQL-database maakt. De standaardwaarde is HDISqoop. Als u de naam gewijzigd, moet u het Sqoop Windows PowerShell-script dienovereenkomstig bijwerken.

4. Druk op **F5** om uit te voeren van het script.
5. Valideer de script-uitvoer. Zorg ervoor dat het script is uitgevoerd.

##<a id="nextsteps"></a>Volgende stappen
Nu weten u hoe u een bestand uploaden naar Azure-blobopslag, hoe u een tabel component wordt gevuld met behulp van de gegevens van Azure-blobopslag component query's uitvoeren en het gebruik van Sqoop gegevens exporteren vanuit HDFS met een Azure SQL-database. Meer informatie raadpleegt u de volgende artikelen:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Component gebruiken met HDInsight][hdinsight-use-hive]
* [Oozie gebruiken met HDInsight][hdinsight-use-oozie]
* [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]
* [Ontwikkel Java MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
