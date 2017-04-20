<properties
    pageTitle="Een Azure SQL-database naar een bestand Bacpac-archiveren via PowerShell"
    description="Een Azure SQL-database naar een bestand Bacpac-archiveren via PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Een Azure SQL-database naar een bestand Bacpac-archiveren via PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


In dit artikel vindt u aanwijzingen voor het archiveren van uw Azure SQL-database naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand (opgeslagen in Azure-blobopslag) via PowerShell.

Wanneer u een archief van een Azure SQL-database maken moet, kunt u de databaseschema en gegevens exporteren naar een Bacpac-bestand. Een bestand Bacpac-is gewoon een ZIP-bestand met de extensie .bacpac. Een Bacpac-bestand kunt later worden opgeslagen in Azure-blobopslag of in de lokale opslag in een on-premises locatie. Dit kan ook worden geïmporteerd terug naar Azure SQL-Database of installatie van SQL Server on-premises implementatie.

**Overwegingen**

- Een archief transactioneel consistent, moet u ervoor zorgen dat er geen schrijven activiteit optreedt tijdens het exporteren of die u wilt exporteren uit een [Transactioneel consistente kopie](sql-database-copy.md) van uw Azure SQL-database.
- De maximale grootte van een Bacpac-bestand met Azure-blobopslag gearchiveerd is 200 GB. Als u wilt archiveren een groter Bacpac-bestand naar de lokale opslag, moet u het hulpprogramma van de opdrachtprompt [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) gebruiken. Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. U kunt ook [download](https://msdn.microsoft.com/library/mt204009.aspx) de nieuwste versie van SQL Server Data Tools om dit hulpprogramma.
- Archiveren met Azure premium storage met behulp van een Bacpac-bestand wordt niet ondersteund.
- Als de exportbewerking groter is dan 20 uur, kan deze worden geannuleerd. Als u wilt vergroten prestaties tijdens het exporteren, kunt u het volgende doen:
 - Tijdelijk uw serviceniveau verhogen.
 - Niet meer alle lezen en schrijven activiteit tijdens het exporteren.
 - Gebruik een [geclusterde index](https://msdn.microsoft.com/library/ms190457.aspx) met niet-null-waarden in alle grote tabellen. Zonder gegroepeerde indexen, kan een exporteren als het duurt langer dan 6-12 uur mislukken. Dit komt omdat de service exporteren scannen van de tabel om te proberen moet te exporteren van de hele tabel uitvoeren. Een goede manier om te bepalen als uw tabellen zijn geoptimaliseerd voor exporteren is **DBCC SHOW_STATISTICS** uitvoeren en zorg ervoor dat de *RANGE_HI_KEY* niet null is en de waarde goede verdeling. Zie [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx)voor meer informatie.

> [AZURE.NOTE] BACPACs niet moeten worden gebruikt voor back-up en herstellen van bewerkingen. Azure SQL-Database automatisch back-ups voor elke gebruikersdatabase gemaakt. Zie de [SQL-Database automatische back-ups](sql-database-automated-backups.md)voor meer informatie.

Als u wilt voltooien in dit artikel, moet u het volgende:

- Een Azure-abonnement.
- Een Azure SQL-database.
- Een [standaardopslag van Azure-account](../storage/storage-create-storage-account.md), met een container blob voor de opslag van de BACPAC in standard opslag.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>De database exporteren

De [nieuw AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) cmdlet dient een aanvraag van de database exporteren naar de service. Afhankelijk van de grootte van uw database duurt de exportbewerking enige tijd om te voltooien.

> [AZURE.IMPORTANT] Om te garanderen een transactioneel consistente Bacpac-bestand, kunt u moet eerst [een kopie van uw database maken](sql-database-copy-powershell.md)en exporteer het bestand de databasekopie.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>De voortgang van de exportbewerking

Na het uitvoeren van [nieuw-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), u kunt de status controleren van de aanvraag kunt invullen door te voeren [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Met dit direct na het verzoek meestal geeft als resultaat **Status: InProgress**. Wanneer er **Status: is geslaagd** het bestand is geëxporteerd.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Voorbeeld van de SQL-database exporteren

Het volgende voorbeeld een bestaande SQL-database exporteert naar een BACPAC en vervolgens ziet u hoe u de status van de exportbewerking controleren.

Als u wilt uitvoeren in het voorbeeld, zijn er enkele variabelen die u moet vervangen door de specifieke waarden voor uw database en opslag-account. Blader naar uw opslag-account om toegang te krijgen van de opslagaccountnaam, de naam van de container blob en sleutelwaarde in de [portal van Azure](https://portal.azure.com). U kunt de toets vinden door te klikken op **toegangstoetsen** op uw opslagruimte account blade.

Vervang de volgende `VARIABLE-VALUES` met waarden voor uw specifieke Azure resources. De naam van de database is de bestaande database die u wilt exporteren.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Volgende stappen

- Als u wilt weten hoe u een Azure SQL-database importeren via Powershell, raadpleegt u [een BACPAC via PowerShell importeren](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Aanvullende informatie

- [Nieuwe AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)