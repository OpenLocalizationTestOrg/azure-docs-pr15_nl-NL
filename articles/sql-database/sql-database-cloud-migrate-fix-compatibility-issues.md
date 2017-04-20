<properties
   pageTitle="Compatibiliteitsproblemen van SQL Server-database voor de migratie met SQL-Database oplossen | Microsoft Azure"
   description="Microsoft Azure SQL Database, databasemigratie, compatibiliteit, de Wizard SQL Azure-migratie"
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Wizard SQL Azure-migratie om oplossen SQL Server-database compatibiliteitsproblemen vóór de migratie met Azure SQL-Database te gebruiken

> [AZURE.SELECTOR]
- [SQL Azure-migratiewizard](sql-database-cloud-migrate-fix-compatibility-issues.md) gebruiken
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) gebruiken
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) gebruiken

In dit artikel leert u detecteren en oplossen van SQL Server-database compatibiliteitsproblemen met behulp van de Wizard SQL Azure migratie voor de migratie met Azure SQL-Database.

## <a name="using-sql-azure-migration-wizard"></a>Gebruik van de wizard SQL Azure migreren

Gebruik het hulpmiddel van de CodePlex [wizard SQL Azure migreren](http://sqlazuremw.codeplex.com/) naar een T-SQL-script uit een database niet-compatibele gegevensbron te genereren. Dit script worden vervolgens getransformeerd door de wizard zodat u deze compatibel is met de SQL-Database. Vervolgens verbinding met Azure SQL-Database het script uitvoeren. In dit hulpprogramma analyseert ook doelcellen bestanden om te bepalen compatibiliteitsproblemen. Het script kan worden gegenereerd met schema alleen of kan gegevens bevatten in BCP-indeling. Aanvullende documentatie, inclusief Stapsgewijze instructies is beschikbaar op CodePlex op [wizard SQL Azure migreren](http://sqlazuremw.codeplex.com/).  

 ![SAMW migratie-diagram](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Niet alle niet-compatibele schema die zijn gevonden door de wizard kan worden opgelost met de ingebouwde transformaties. Incompatibele script dat niet kan worden opgelost worden gerapporteerd als fouten met opmerkingen die zijn toegevoegd in de gegenereerde script. Als er veel fouten zijn gevonden, kunt u Visual Studio of SQL Server Management Studio stapsgewijs en elke fout die kan niet worden opgelost met de Wizard migreren van SQL Server op te lossen.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Een compatibele SQL Server-database migreren met SQL-Database](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
