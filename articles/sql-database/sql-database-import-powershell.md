<properties
    pageTitle="Een Bacpac-bestand importeren om te maken van een Azure SQL-database met behulp van PowerShell | Microsoft Azure"
    description="Een bestand Bacpac-een Azure SQL-database maken met behulp van PowerShell importeren"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Een bestand Bacpac-een Azure SQL-database maken met behulp van PowerShell importeren

**Één database**

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

In dit artikel vindt u aanwijzingen voor het maken van een Azure SQL-database door een bestand [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) met PowerShell te importeren.

De database is gemaakt vanuit een Bacpac-bestand (.bacpac) geïmporteerd uit een opslag van Azure blob container. Als u een bestand Bacpac-in Azure Storage niet hebt, raadpleegt u [archief een Azure SQL-database naar een bestand Bacpac-via PowerShell](sql-database-export-powershell.md). Als u al een Bacpac-bestand dat zich niet in Azure opslag, [Gebruik AzCopy te eenvoudig uploaden naar uw opslagruimte van Azure-account](../storage/storage-use-azcopy.md#blob-upload)hebt.

> [AZURE.NOTE] Azure SQL-Database wordt automatisch gemaakt en onderhouden van back-ups voor elke gebruikersdatabase die u kunt terugzetten. Zie de [SQL-Database automatische back-ups](sql-database-automated-backups.md)voor meer informatie.


Als u wilt importeren in een SQL-database, hebt u het volgende nodig:

- Een Azure-abonnement. Klik op **Gratis proefversie** boven aan deze pagina en keert u terug naar het voltooien van dit artikel als u een abonnement op Azure gewoon nodig hebt.
- Een Bacpac-bestand van de database die u wilt importeren. De BACPAC moet in een container van de blob [opslag van Azure-account](../storage/storage-create-storage-account.md) .



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>De variabelen voor uw omgeving instellen

Er zijn een paar variabelen waar moet u de voorbeeldwaarden vervangen door de specifieke waarden voor de database en uw account opslag.

Naam van de server moet een server die al in het abonnement dat is geselecteerd in de vorige stap bestaat. Moet de server die u wilt dat de database moet worden gemaakt in. Het importeren van een database rechtstreeks in een elastische toepassingen wordt niet ondersteund. Maar u kunt eerst importeren in één database en de database naar een groep verplaatsen.

De naam van de database is de naam die u wilt gebruiken voor de nieuwe database.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


De volgende variabelen zijn van het account opslag waarin uw BACPAC zich bevindt. Blader naar uw opslag-account om toegang te krijgen van deze waarden in de [portal van Azure](https://portal.azure.com). U kunt de primaire toegangstoets vinden door te klikken op **alle instellingen** en klik vervolgens op **toetsen** van uw opslag-account blade.

De naam van de blob is de naam van een bestaand Bacpac-bestand dat u wilt maken van de database uit. U moet de extensie .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Uitgevoerd de [Get-referentie] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet opent u een venster waarin wordt gevraagd om uw gebruikersnaam en wachtwoord. Voer de admin aanmelden en het wachtwoord voor de SQL-Database-server ($ServerName dan hierboven), en niet de gebruikersnaam en wachtwoord voor uw Azure-account.

    $credential = Get-Credential


## <a name="import-the-database"></a>De database importeren

Deze opdracht dient een aanvraag van de database importeren met de service. Afhankelijk van de grootte van uw database duurt de importbewerking enige tijd om te voltooien.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>De voortgang van de bewerking

Na het uitvoeren van [nieuw-AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), u kunt de status controleren van de aanvraag kunt invullen door te voeren de [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-Database PowerShell-script voor importeren


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Volgende stappen

- Zie voor meer verbinding maken met en een geïmporteerde SQL-Database query [verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)
