<properties 
   pageTitle="Aan de slag met Azure gegevens Lake Analytics via Azure PowerShell | Azure" 
   description="Informatie over het gebruik van de Azure PowerShell te maken van een account voor gegevensopslag Lake, maakt u een gegevens Lake Analytics-taak met I-SQL en de taak verstuurt. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Zelfstudie: aan de slag met Azure gegevens Lake Analytics via Azure PowerShell

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van de Azure PowerShell Azure gegevens Lake Analytics-accounts maken en verzenden van taken die moeten worden gegevens Lake analytische accounts gegevens Lake Analytics taken definiëren in [I-SQL](data-lake-analytics-u-sql-get-started.md). Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md)voor meer informatie over gegevens Lake Analytics.

In deze zelfstudie wordt u een taak die leest een tabblad gescheiden waarden ()-bestand en zet deze in een bestand met door komma's gescheiden waarden (CSV) ontwikkelen. Ga via de dezelfde zelfstudie met andere ondersteunde hulpprogramma's, te klikken op de tabbladen boven aan deze sectie.

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Een workstation met Azure PowerShell**. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken

Voordat u kunt alle taken uitvoeren, moet u een gegevens Lake Analytics-account hebben. Als u wilt een gegevens Lake Analytics-account hebt gemaakt, moet u het volgende opgeven:

- **Azure resourcegroep**: A gegevens Lake Analytics-account moet worden gemaakt in een groep Azure Resource. [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.  

    De resource opsommen van groepen in uw abonnement:
    
        Get-AzureRmResourceGroup
    
    Een nieuwe resourcegroep maken:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Gegevens Lake Analytics-accountnaam**
- **Locatie**: een van de Azure datacenters die ondersteuning biedt voor gegevens Lake Analytics.
- **Standaard gegevens Lake account**: elke gegevens Lake Analytics-account heeft een standaardaccount voor gegevens Lake.

    Een nieuwe gegevens Lake-account maken:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] De accountnaam gegevens Lake mag alleen kleine letters en cijfers bevatten.



**Een gegevens Lake Analytics-account maken**

1. Open PowerShell ISE vanaf uw Windows-werkstation.
2. Voer het volgende script:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Gegevens uploaden naar gegevens Lake

In deze zelfstudie wordt u enkele logboeken zoeken verwerken.  Het logboek zoeken kan worden opgeslagen in gegevens Lake store of Azure-blobopslag. 

Een logboekbestand van de steekproef zoeken is gekopieerd naar een openbare Azure Blob container. Gebruik het volgende PowerShell-script het bestand te downloaden naar uw werkstation en klikt u vervolgens het bestand uploaden naar het standaardaccount voor gegevensopslag Lake van uw gegevens Lake Analytics-account.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Het volgende PowerShell-script ziet u hoe u de standaardnaam Lake gegevensopslag voor een gegevens Lake Analytics-account:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] De Portal Azure biedt een gebruikersinterface voor het kopiëren van voorbeeldgegevens naar het standaardaccount voor gegevensopslag Lake. Zie [Aan de slag met Azure gegevens Lake analyses met behulp van Azure Portal](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account)voor instructies.

Gegevens Lake Analytics ook toegang tot Azure-blobopslag.  Zie [Azure PowerShell gebruiken met Azure-opslag](../storage/storage-powershell-guide-full.md)voor het uploaden van gegevens met Azure-blobopslag.

##<a name="submit-data-lake-analytics-jobs"></a>Gegevens Lake Analytics taken indienen

De gegevens Lake Analytics-taken zijn in de I-SQL-taal geschreven. Zie [aan de slag met I-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [I-SQL-Naslaggids](http://go.microsoft.com/fwlink/?LinkId=691348)meer informatie over het I-SQL.

**Een script van de taak gegevens Lake analyses maken**

- Maak een tekstbestand met de volgende I-SQL-script en sla het tekstbestand op uw werkstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Deze I-SQL-script leest het bronbestand van de gegevens met **Extractors.Tsv()**en maakt vervolgens een CSV-bestand met **Outputters.Csv()**. 
    
    De twee paden niet worden gewijzigd, tenzij u het bronbestand naar een andere locatie kopiëren.  Gegevens Lake Analytics maakt de uitvoermap als deze niet bestaat.
    
    Het is eenvoudiger relatieve paden gebruiken voor bestanden die zijn opgeslagen in standaardprogramma gegevens Lake accounts. U kunt ook absolute paden gebruiken.  Bijvoorbeeld 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    U moet absolute paden gebruiken voor toegang tot bestanden in gekoppelde opslag-accounts.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde opslag van Azure-account is:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of openbare containers toegangsmachtigingen worden momenteel niet ondersteund.    
    
    
**De taak indienen**

1. Open PowerShell ISE vanaf uw Windows-werkstation.
2. Voer het volgende script:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    In het script wordt de I-SQL-script-bestand opgeslagen op c:\tutorials\data-lake-analytics\copyFile.usql. Het bestandspad dienovereenkomstig bijwerken.
 
Nadat de taak is voltooid, kunt u de volgende cmdlets gebruiken voor een overzicht van het bestand en download het bestand:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Zie ook

- Klik op het tabblad-selectors boven aan de pagina overzicht van de dezelfde zelfstudie met een ander hulpprogramma.
- Een complexe query's, raadpleegt u [analyseren Website Logboeken door middel van Azure gegevens Lake analyses](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelen I--SQL-scripts met gegevens Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen ontwikkelen van toepassingen I-SQL.
- Zie [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md)voor meer I-SQL.
- Zie [Beheren Azure gegevens Lake analyses met behulp van Azure Portal](data-lake-analytics-manage-use-portal.md)voor beheertaken.
- Als u een overzicht van gegevens Lake analyses, Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md).

