<properties
   pageTitle="Een SQL Azure datawarehouse (PowerShell) herstellen | Microsoft Azure"
   description="PowerShell taken voor het herstellen van een Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Een SQL Azure datawarehouse (PowerShell) herstellen

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In dit artikel leert u hoe u herstelt een Azure SQL Data Warehouse via PowerShell.

## <a name="before-you-begin"></a>Voordat u begint

**Controleer of de capaciteit van de DTU.** Elke SQL Data Warehouse wordt gehost door een SQL-server (bijvoorbeeld myserver.database.windows.net) waarvoor een standaard DTU quotum.  Voordat u een SQL-Data Warehouse terugzetten kunt, Controleer de uw SQL-server heeft onvoldoende resterende DTU quotum voor de database wordt teruggezet. Informatie over het berekenen van DTU nodig of dat er meer DTU, raadpleegt u [een DTU quotum wijziging aanvragen][].

### <a name="install-powershell"></a>PowerShell installeren

Pas Azure PowerShell met SQL Data Warehouse gebruiken, moet u voor het installeren van Azure PowerShell versie 1.0 of groter.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-naam AzureRM**.  De meest recente versie kan worden geïnstalleerd van [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de meest recente versie [installeren en configureren van Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Een database actief of onderbroken terugzetten

Een database van een momentopname gebruik de PowerShell-cmdlet [Herstellen-AzureRmSqlDatabase][] om te zetten.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en vermelden van alle abonnementen die is gekoppeld aan uw account.
3. Selecteer het abonnement met de database wilt terugzetten.
4. Lijst met de punten herstellen voor de database.
5. Kies het gewenste punt in het gebruik van de RestorePointCreationDate.
6. De database herstellen naar de gewenste herstellen komma.
7. Controleer of de herstelde database online is.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Nadat de herstellen is voltooid, kunt u de herstelde database door de volgende [configureren van uw database na herstel][]configureren.


## <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Als u wilt een verwijderde database terugzet, gebruikt u de cmdlet [Herstellen-AzureRmSqlDatabase][] .

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en vermelden van alle abonnementen die is gekoppeld aan uw account.
3. Selecteer het abonnement met de verwijderde database wilt terugzetten.
4. De specifieke verwijderde database krijgen.
5. De verwijderde database herstellen.
6. Controleer of de herstelde database online is.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Nadat de herstellen is voltooid, kunt u de herstelde database door de volgende [configureren van uw database na herstel][]configureren.


## <a name="restore-from-an-azure-geographical-region"></a>Terugzetten uit een Azure geografisch gebied

Als u wilt herstellen van een database, gebruikt u de cmdlet [Herstellen-AzureRmSqlDatabase][] .

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en vermelden van alle abonnementen die is gekoppeld aan uw account.
3. Selecteer het abonnement met de database wilt terugzetten.
4. Zorgen dat de database die u wilt herstellen.
5. Het verzoek herstel voor de database maken.
6. Controleer of de status van de database geografische hersteld.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Zie het [configureren van uw database na herstel][]uw database configureren nadat de herstellen is voltooid. 


De herstelde database wordt worden ingeschakeld voor TDE als de brondatabase TDE is ingeschakeld.


## <a name="next-steps"></a>Volgende stappen
Lees meer informatie over de functies van de bedrijfscontinuïteit bedrijven van Azure SQL Database-edities, het [Azure SQL-Database business bedrijfscontinuïteit overzicht][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Database business bedrijfscontinuïteit-overzicht]: sql-database-business-continuity.md
[Een DTU quotum wijziging aanvragen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[De database na herstel configureren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Het installeren en configureren van Azure PowerShell]: powershell-install-configure.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[De database na herstel configureren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Herstellen-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
