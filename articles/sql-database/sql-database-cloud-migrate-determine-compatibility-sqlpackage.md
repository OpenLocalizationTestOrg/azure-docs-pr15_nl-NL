<properties
   pageTitle="SQL-Database compatibiliteit met SqlPackage.exe bepalen | Microsoft Azure"
   description="Microsoft Azure SQL Database, databasemigratie, compatibiliteit van de SQL-Database, SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>SQL-Database compatibiliteit met SqlPackage.exe bepalen

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade-advies](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In dit artikel leert u om te bepalen of een database van SQL Server compatibele om te migreren naar SQL-Database met het hulpprogramma voor [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) opdrachtprompt.

## <a name="using-sqlpackageexe"></a>SqlPackage.exe gebruiken

1. Open een opdrachtprompt en een map met de nieuwste versie van sqlpackage.exe wijzigen. Dit hulpprogramma wordt geleverd met de meest recente versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)of kunt u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) downloaden rechtstreeks vanuit het Microsoft Downloadcentrum.
2. Voer de volgende SqlPackage-opdracht met de volgende argumenten voor uw omgeving:

    ' sqlpackage.exe /Action:Export /ssn: < servernaam > /sdn: < databasenaam > /tf: < target_file > /p:TableData < schema_name.table_name >>< = uitvoerbestand > 2 > & 1

  	| De argumenten  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | de naam van de bron-server  |
  	| < databasenaam >  | de naam van de bron-database  |
  	| < target_file >  | naam en locatie voor Bacpac-bestand  |
  	| < schema_name.table_name >  | de tabellen waarvoor gegevens uitvoer naar het doelbestand zijn  |
  	| < uitvoerbestand >  | de bestandsnaam en locatie voor het uitvoerbestand met fouten, indien van toepassing  |

    De reden voor het argument /p:TableName is dat we alleen wilt testen op databasecompatibiliteit voor exporteren naar Azure SQL DB V12 in plaats van de gegevens exporteren uit alle tabellen. Als het argument exporteren van sqlpackage.exe ondersteunt helaas niet nul tabellen ophalen. U moet ten minste één tabel, zoals een enkele kleine tabel opgeven. De < uitvoerbestand > bevat het rapport van eventuele fouten. De "> 2 > & 1" tekenreeks buizen zowel de standaard uitvoer als de standaardfout die voortvloeien uit het uitvoeren van opdrachten aan opgegeven uitvoerbestand.

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Het uitvoerbestand openen en controleer de compatibiliteitsfouten, indien van toepassing. 

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Databasemigratie compatibiliteitsproblemen oplossen](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Een compatibel SQL Server-database migreren met SQL-Database](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
