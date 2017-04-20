<properties
    pageTitle="Een Azure SQL-Database herstellen vanuit een geografische-redundante back-up (PowerShell) | Microsoft Azure"
    description="Een Azure SQL-Database naar een nieuwe server uit een geografische-redundante back-up herstellen"
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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Een Azure SQL-Database herstellen vanuit een geografische-redundante back-up via PowerShell


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [Geografische-herstellen: Azure Portal](sql-database-geo-restore-portal.md)

In dit artikel leest u hoe u uw database terugzetten naar een nieuwe server met behulp van geografische herstellen. Dit kunt doen via PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>De database in een database zelfstandige geografische-herstellen

1. Ophalen van de geografische-redundante back-up van uw database die u herstellen wilt met behulp van de [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Beginnen met terugzetten uit de geografische-redundante back-up met behulp van de [herstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>De database naar een groep elastische database geografische-herstellen

1. Ophalen van de geografische-redundante back-up van uw database die u herstellen wilt met behulp van de [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Beginnen met terugzetten uit de geografische-redundante back-up met behulp van de [herstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-cmdlet. Geef de naam van de toepassingen die u wilt herstellen van uw database in.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Volgende stappen

- Zie [Business bedrijfscontinuïteit overzicht](sql-database-business-continuity.md)voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's.
- Zie meer informatie over back-ups van Azure SQL-Database wordt automatisch, [automatisch back-ups in SQL-Database](sql-database-automated-backups.md).
- Voor meer informatie over het gebruik van automatische back-ups voor herstel, Zie [herstellen van een database van back-ups van de service is gestart](sql-database-recovery-using-backups.md).
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md).  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md).
