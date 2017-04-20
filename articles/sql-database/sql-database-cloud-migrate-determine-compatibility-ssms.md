<properties
   pageTitle="Gebruik van SQL Server Management Studio om te bepalen van de SQL-Database compatibiliteit voor de migratie met Azure SQL-Database | Microsoft Azure"
   description="Microsoft Azure SQL Database, databasemigratie, compatibiliteit met SQL-Database gegevens laag toepassing Wizard exporteren"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Gebruik van SQL Server Management Studio om te bepalen van de SQL-Database compatibiliteit voor de migratie met Azure SQL-Database

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade-advies](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
In dit artikel u leert om te bepalen of een SQL Server-database is compatibel met SQL-Database met de Wizard exporteren laag toepassing in SQL Server Management Studio migreren.

## <a name="using-sql-server-management-studio"></a>Gebruik van SQL Server Management Studio

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden bijgewerkt maandelijkse moet blijven synchroon met updates van de Azure-portal.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).

2. Open Management Studio en verbinding maken met de brondatabase in Object Explorer.
3. Met de rechtermuisknop op de brondatabase in de Verkenner Object, wijs **taken**aan en klik op **Exporteren gegevens laag toepassing...**

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Klik in de wizard Exporteren op **volgende**en klik vervolgens op het tabblad **Instellingen** configureren de exporteren als u wilt het Bacpac-bestand opslaan op een lokale schijflocatie of op een Azure blob. Een Bacpac-bestand is opgeslagen als er geen compatibiliteitsproblemen database. Als er compatibiliteitsproblemen zijn, worden ze worden weergegeven op de console.

    ![Instellingen van een exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Als u wilt overslaan gegevens exporteren, klikt u op het **tabblad Geavanceerd** en schakel het selectievakje **Alles selecteren** . Ons doel is op dit moment alleen om te testen op compatibiliteit.

    ![Instellingen van een exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Klik op **volgende** en klik vervolgens op **Voltooien**. Compatibiliteitsproblemen database, indien van toepassing, worden weergegeven wanneer de wizard is gevalideerd met het schema.

    ![Instellingen van een exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Als er geen fouten wordt weergegeven, de database is compatibel en u bent klaar om te migreren. Als er fouten zijn, moet u problemen oplost. Als u wilt de fouten wordt weergegeven, klikt u op **fout** voor **Validating schema**. 
    ![Instellingen van een exporteren](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Als de *. Bacpac-bestand wordt gegenereerd, en vervolgens uw database is compatibel met SQL-Database en u bent klaar om te migreren.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Databasemigratie compatibiliteitsproblemen oplossen](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Een compatibel SQL Server-database migreren met SQL-Database](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
