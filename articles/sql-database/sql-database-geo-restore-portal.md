<properties
    pageTitle="Een Azure SQL-database terugzetten uit een automatische back-up (Azure portal) | Microsoft Azure"
    description="Een Azure SQL-database terugzetten uit een automatische back-up (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Een Azure SQL-database terugzetten uit een automatische back-up met behulp van de Azure portal


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md#geo-restore)
- [Geografische herstellen: PowerShell](sql-database-geo-restore-powershell.md)

Dit artikel leest u hoe u uw database herstellen vanuit een [Automatische back-up](sql-database-automated-backups.md) in een nieuwe server met [Geografische herstellen](sql-database-recovery-using-backups/.md#geo-restore) met behulp van de Azure portal.

## <a name="select-a-database-to-restore"></a>Selecteer een database herstellen

Als u een database in de portal van Azure herstellen, volgt u de volgende stappen uit:

1.  Ga naar de [Azure-portal](https://portal.azure.com).
2.  Selecteer **+ nieuwe**aan de linkerkant van het scherm > **Databases** > **SQL-Database**:

    ![Een Azure SQL-database terugzetten](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  **Back-ups** selecteert als de bron en selecteert u de back-up die u wilt herstellen. Geef de naam van een database, een server die u wilt herstellen van de database naar, en klik op **maken**:
  
    ![Een Azure SQL-database terugzetten](./media/sql-database-geo-restore-portal/geo-restore.png)

De status van de bewerking herstellen door te klikken op het meldingspictogram in de rechterbovenhoek van de pagina controleren. 


## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
