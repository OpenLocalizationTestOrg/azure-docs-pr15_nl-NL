<properties
    pageTitle="Onderbreken en hervatten gegevensmigratie (uitrekken-Database) | Microsoft Azure"
    description="Informatie over het onderbreken of gegevensmigratie naar Azure hervatten."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Onderbreken en hervatten gegevensmigratie (uitrekken-Database)

Als u wilt onderbreken of gegevensmigratie naar Azure hervatten, **Uitrekken** selecteren voor een tabel in SQL Server Management Studio en selecteert u **onderbreken** gegevensmigratie onderbreken of **hervatten** gegevensmigratie hervatten. U kunt ook Transact\-SQL onderbreken of gegevensmigratie hervatten.

Een pauze invoegen migratie van de gegevens in afzonderlijke tabellen wanneer u wilt oplossen op de lokale server of u kunt de beschikbare netwerkbandbreedte optimaliseren.

## <a name="pause-data-migration"></a>Een pauze invoegen gegevensmigratie

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Gebruik van SQL Server Management Studio om te onderbreken gegevensmigratie

1.  Selecteer de uitrekken in SQL Server Management Studio in Object Explorer\-tabel waarvoor u wilt onderbreken gegevensmigratie ingeschakeld.

2.  Rechts\-klikt u op en selecteer **Uitrekken**en selecteer vervolgens **een pauze invoegen**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Gebruik Transact\-SQL gegevensmigratie onderbreken
Voer de volgende opdracht.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Migratie van cv-gegevens

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>SQL Server Management Studio gegevensmigratie weer wilt gebruiken

1.  Selecteer de uitrekken in SQL Server Management Studio in Object Explorer\-tabel waarvoor u wilt hervatten gegevensmigratie ingeschakeld.

2.  Rechts\-klikt u op en selecteer **Uitrekken**en selecteer vervolgens **cv**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Gebruik Transact\-SQL gegevensmigratie hervatten
Voer de volgende opdracht.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Controleren of de migratie actief of onderbroken is

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Gebruik van SQL Server Management Studio om te controleren of de migratie actief of onderbroken is
SQL Server Management Studio, open **Uitrekken Database beeldscherm** en de waarde van de kolom **Status van de migratie** controleren. Zie voor meer informatie [controleren en problemen met gegevensmigratie](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Gebruik van Transact-SQL om te controleren of de migratie actief of onderbroken is
De catalogus weergave **sys.remote_data_archive_tables** query en controleert u de waarde van de kolom **is_migration_paused** . Zie [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx)voor meer informatie.

## <a name="see-also"></a>Zie ook

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[controleren en problemen met gegevensmigratie](sql-server-stretch-database-monitor.md)
