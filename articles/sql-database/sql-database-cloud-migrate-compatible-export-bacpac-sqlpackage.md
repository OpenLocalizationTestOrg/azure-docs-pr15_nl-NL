<properties
   pageTitle="Een SQL Server-database exporteren naar een Bacpac-bestand met SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie database exporteren, Bacpac-bestand, sqlpackage exporteren"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Een SQL Server-database exporteren naar een Bacpac-bestand met SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

In dit artikel leest hoe u het exporteren van een SQL Server-database naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met het hulpprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met de meest recente versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)of kunt u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) downloaden rechtstreeks vanuit het Microsoft Downloadcentrum.

1. Open een opdrachtprompt en wijzigen van een map met het hulpprogramma voor de opdrachtregel sqlpackage.exe - Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server. Gebruik zoeken op uw computer om het pad in uw omgeving.
2. Voer de volgende sqlpackage.exe-opdracht met de volgende argumenten voor uw omgeving:

    ' sqlpackage.exe /Action:Export /ssn: < servernaam > /sdn: < databasenaam > /tf: < target_file >

  	| De argumenten  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | de naam van de bron-server  |
  	| < databasenaam >  | de naam van de bron-database  |
  	| < target_file >  | naam en locatie voor Bacpac-bestand  |

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Een BACPAC met Azure SQL-Database met SSMS importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importeren in een BACPAC Azure SQL Database SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importeren in een BACPAC Azure SQL Database Azure-portal](sql-database-import.md)
- [Een BACPAC met PowerShell van Azure SQL-Database importeren](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
