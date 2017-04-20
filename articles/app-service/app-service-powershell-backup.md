<properties
    pageTitle="PowerShell gebruiken om te back-up en herstellen van App Service-apps"
    description="Informatie over het gebruik van PowerShell back-up en herstellen van een app in Azure App-Service"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>PowerShell gebruiken om te back-up en herstellen van App Service-apps

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST API](../app-service-web/websites-csm-backup.md)

Informatie over het gebruik van Azure PowerShell back-up en herstellen van [App Service-apps](https://azure.microsoft.com/services/app-service/web/). Zie voor meer informatie over web app back-ups, inclusief vereisten en beperkingen, [Back-up van een WebApp in Azure App-Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Vereisten voor
Als u wilt PowerShell gebruiken voor het beheren van uw app back-ups, hebt u het volgende nodig:

- **Een SA's URL** waarmee lees- en schrijftoegang tot een container Azure Storage. Zie [informatie over het model SA's](../storage/storage-dotnet-shared-access-signature-part-1.md) voor een uitleg van de URL's SA's. Zie [Azure-PowerShell gebruiken met Azure-opslag](../storage/storage-powershell-guide-full.md) voor voorbeelden van het beheren van Azure Storage via PowerShell.
- **De verbindingsreeks A** als u wilt een back-up van een database samen met uw web-app.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Hoe u een URL SA's voor gebruik met de back-cmdlets uit de web-app genereren
De URL van een SA's kunnen worden gegenereerd met PowerShell. Hier volgt een voorbeeld van het genereren van dat kan worden gebruikt met de cmdlets in dit artikel beschreven.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Azure PowerShell 1.3.2 installeren of groter

Zie [Azure PowerShell gebruiken met Azure resourcemanager](../powershell-install-configure.md) voor instructies over het installeren en gebruiken van Azure PowerShell.

## <a name="create-a-backup"></a>Een back-up maken

Gebruik de cmdlet New-AzureRmWebAppBackup om het maken van een back-up van een web-app.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Hiermee wordt een back-up gemaakt met de naam van een automatisch gegenereerde. Als u Geef een naam voor de back-up wilt, gebruikt u de optionele parameter naam_back-up.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Als u wilt een database in de back-up opnemen, maakt eerst een back-instelling van database met de cmdlet New-AzureRmWebAppDatabaseBackupSetting en deze instelling in de Databases-parameter van de cmdlet New-AzureRmWebAppBackup opgeven. De parameter Databases accepteert een matrix van database-instellingen, zodat u kunt een back-up van meer dan één database.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Back-ups krijgen

De cmdlet Get-AzureRmWebAppBackupList geeft als resultaat een matrix met alle back-ups voor een web-app. U kunt de naam van de web-app en de resourcegroep moet opgeven.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Als u een specifieke back-up, gebruikt u de cmdlet Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

U kunt ook een object web app in een van de back-up management cmdlets uitkomt aanvoeren.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Automatische back-ups plannen

U kunt de back-ups veroorzaken, automatisch met een opgegeven interval plannen. Als u wilt configureren een back-planning, gebruikt u de cmdlet bewerken-AzureRmWebAppBackupConfiguration. Deze cmdlet kost verschillende parameters:

- **De naam van** -de naam van de web-app.
- **ResourceGroupName** - de naam van het resourceveld groep met de web-app.
- **Slot** - optioneel. De naam van de web-app slot.
- **StorageAccountUrl** - de SA's-URL voor de opslag van Azure container gebruikt voor het opslaan van de back-ups.
- **FrequencyInterval** - numerieke waarde voor hoe vaak de back-ups moeten worden gemaakt. Moet u een positief geheel getal.
- **FrequencyUnit** - tijdseenheid voor hoe vaak de back-ups moeten worden gemaakt. Opties zijn uur en dag.
- **RetentionPeriodInDays** - hoeveel dagen de automatische back-ups moeten worden opgeslagen voordat ze automatisch worden verwijderd.
- **Starttijd** - optioneel. De tijd waarop de automatische back-ups moeten beginnen. Back-ups eerder onmiddellijk beginnen als dit null is. Moet u een datum/tijd.
- **Databases** - optioneel. Een matrix van DatabaseBackupSettings voor de back-up-databases.
- **KeepAtLeastOneBackup** - optioneel veranderd parameter. Leveren dit als een back-up moet altijd worden bewaard in de opslagruimte-account, ongeacht hoe oude is.

Hier volgt een voorbeeld van het gebruik van deze cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Als u de huidige back-planning, gebruikt u de cmdlet Get-AzureRmWebAppBackupConfiguration. Dit is handig voor het wijzigen van een planning die al is geconfigureerd.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Een web-app herstellen uit een back-up

Als u een web-app herstellen uit een back-up, gebruikt u de cmdlet herstellen-AzureRmWebAppBackup. De eenvoudigste manier om gebruik deze cmdlet is naar recht streepje in een back-object opgehaald uit de cmdlet Get-AzureRmWebAppBackup of de cmdlet Get-AzureRmWebAppBackupList.

Nadat u een back-object hebt, kunt u deze naar de cmdlet herstellen-AzureRmWebAppBackup aanvoeren. Geeft de parameter van de schakeloptie overschrijven om aan te geven dat u van plan bent om het te overschrijven van uw web-app met de inhoud van de back-up. Als de back-up databases bevat, worden deze databases ook hersteld.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Hier volgt een voorbeeld van het gebruik van de herstellen-AzureRmWebAppBackup door het opgeven van alle parameters.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Een back-up verwijderen

Als u wilt een back-up verwijderen, gebruikt u de cmdlet verwijderen-AzureRmWebAppBackup. Hiermee verwijdert u de back-up van uw account opslag. Geef de appnaam van de, de resourcegroep en de ID van de back-up die u wilt verwijderen.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

U kunt ook een back-object aanvoeren naar de cmdlet verwijderen-AzureRmWebAppBackup om deze te verwijderen.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
