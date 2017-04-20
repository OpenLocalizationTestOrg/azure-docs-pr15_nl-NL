<properties
    pageTitle="Een verwijderde Azure SQL-database (Azure portal) terugzetten | Microsoft Azure"
    description="Herstel een verwijderde Azure SQL-database (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Een verwijderde Azure SQL-database met behulp van de Portal Azure terugzetten

> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [**Verwijderde DB terugzetten: Portal**](sql-database-restore-deleted-database-portal.md)
- [Verwijderde DB terugzetten: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Selecteer de database herstellen 

Een verwijderde database in de portal van Azure herstellen:

1.  Klik op **meer services**in de [portal van Azure](https://portal.azure.com) > **SQL-servers**.
3.  Selecteer de server die deel uitmaakt van de database die u wilt herstellen.
4.  Schuif omlaag naar de sectie **bewerkingen** van uw server-blade en selecteer **Verwijderde databases**: ![een Azure SQL-database terugzetten](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Selecteer de database die u wilt herstellen.
6.  Geef een databasenaam en klik op **OK**:

    ![Een Azure SQL-database terugzetten](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
