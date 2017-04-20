<properties
    pageTitle="Een Azure SQL-database herstellen naar een vorig punt in tijd (Azure portal) | Microsoft Azure"
    description="Herstel een Azure SQL-database naar een vorig punt in tijd."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Een Azure SQL-database herstellen naar een vorig punt in tijd in de portal van Azure


> [AZURE.SELECTOR]
- [Overzicht](sql-database-recovery-using-backups.md)
- [Point-In-Time herstellen: PowerShell](sql-database-point-in-time-restore-powershell.md)

In dit artikel leest u het herstellen van uw database op een eerder in tijd van de [dat SQL-Database automatische back-ups](sql-database-automated-backups.md) met behulp van de Azure portal.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Een SQL-database naar een vorig punt in tijd herstellen

Selecteer een database terugzetten in de portal van Azure:

1.  Open de [portal van Azure](https://portal.azure.com).
2.  Aan de linkerkant van het scherm, selecteert u **meer services** > **SQL-databases**.
3.  Klik op de database die u wilt herstellen.
4.  Aan de bovenkant van de pagina van uw database, selecteer **herstellen**:

    ![Een Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Klik op de pagina **herstellen** selecteert u de datum en tijd (in UTC-tijd) de database te herstellen en klik vervolgens op **OK**:

    ![Een Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>De bewerking voor terugzetten controleren

1. Wanneer u op **OK** in de vorige stap, klik op het meldingspictogram in de rechterbovenhoek van de pagina en klik op de melding **herstellen SQL-database** voor meer informatie.

    ![Een Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. De pagina herstellen SQL-database wordt geopend met informatie over de status van de terugzetten. U kunt klikken op de regel-artikel voor meer informatie:

    ![Een Azure SQL-database terugzetten](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
