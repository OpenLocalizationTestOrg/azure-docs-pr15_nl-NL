<properties
   pageTitle="Migreren naar SQL-Database replicatietransacties | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie, database, importeren transacties herhaling"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>SQL Server-database migreren met Azure SQL-Database replicatietransacties

> [AZURE.SELECTOR]
- [De Wizard migreren SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar Bacpac-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit Bacpac-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transacties herhaling](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

In dit artikel leert u een compatibele SQL Server-database migreren met Azure SQL-Database met minimale uitvaltijd replicatietransacties voor SQL Server.

## <a name="understanding-the-transactional-replication-architecture"></a>Inzicht in de architectuur transacties herhaling

Wanneer u geen enkele uw SQL Server-database verwijderen uit terwijl de migratie is uitgevoerd, kunt u SQL Server-transacties replicatie als uw migratieoplossing. Als u wilt deze oplossing wordt gebruikt, kunt u uw Azure SQL-Database configureren als abonnee van naar de on-premises implementatie SQL Server-instantie die u wilt migreren. De on-premises implementatie transacties herhaling distributeur gegevens uit de on-premises implementatie de database synchroniseert moet worden gesynchroniseerd (de uitgever) terwijl nieuwe transacties verder optreden. 

U kunt ook transacties herhaling gebruiken om te migreren van een subset van uw lokale database. De publicatie waaraan u met Azure SQL-Database repliceren kan worden beperkt tot een subset van de tabellen in de database worden gerepliceerd. Voor elke tabel worden gerepliceerd, kunt u de gegevens naar een subset van de rijen en/of een subset van de kolommen beperken.

Met transacties herhaling weergegeven alle wijzigingen in uw gegevens of het schema in uw Azure SQL-Database. Zodra de synchronisatie voltooid is en u bent klaar om te migreren, wijzig de verbindingsreeks van uw toepassingen ze verwijzen naar uw Azure SQL-Database. Wanneer de transacties herhaling kan wijzigingen links op de lokale database en al uw toepassingen wijs Azure DB, kunt u transacties herhaling verwijderen. Uw Azure SQL-Database is nu uw productiesysteem.

 ![SeedCloudTR-diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Transacties herhaling vereisten

Transacties replicatie is een technologie ingebouwde en geïntegreerde met SQL Server sinds SQL Server 6.5. Het is een volwaardige en beproefde technologie dat meestal DBAs weten waarmee ze ervaring hebben. Met de [SQL Server-2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)is het nu mogelijk uw Azure SQL-Database configureren als [transacties herhaling abonnee](https://msdn.microsoft.com/library/mt589530.aspx) aan de publicatie on-premises implementatie. De functionaliteit voor u dit van Management Studio stellen krijgen is hetzelfde als wanneer u een abonnee transacties herhaling op een on-premises implementatie-server hebt ingesteld. Ondersteuning voor dit scenario wordt ondersteund wanneer de uitgever en de distributeur ten minste één van de volgende versies van SQL Server zijn:

 - SQL Server 2016 en hoger 
 - SQL Server 2014 SP1 CU3 en hoger
 - SQL Server 2014 RTM CU10 en hoger
 - SQL Server 2012 SP2 CU8 en hoger
 - SQL Server 2012 SP3 en hoger


> [AZURE.IMPORTANT] De meest recente versie van SQL Server Management Studio moet blijven gebruiken gesynchroniseerd met updates voor Microsoft Azure en SQL-Database. Oudere versies van SQL Server Management Studio kunnen SQL-Database ingesteld als abonnee. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Aanvullende informatie

- [Transacties herhaling](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
