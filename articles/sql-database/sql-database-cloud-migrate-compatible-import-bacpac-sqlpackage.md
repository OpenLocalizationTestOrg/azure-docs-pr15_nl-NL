<properties
   pageTitle="Met SQL-Database importeren uit een Bacpac-bestand met SqlPackage"
   description="Microsoft Azure SQL-Database, databasemigratie-database importeren, Bacpac-bestand, sqlpackage importeren"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Met SQL-Database importeren uit een Bacpac-bestand met SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In dit artikel leest hoe u met SQL-database importeren uit een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met het hulpprogramma [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dit hulpprogramma wordt geleverd met de meest recente versies van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools voor Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)of kunt u de nieuwste versie van [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) downloaden rechtstreeks vanuit het Microsoft Downloadcentrum.


> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u hebt al deze is ingericht een SQL-databaseserver, de verbindingsgegevens bij de hand hebt en hebt gecontroleerd of de brondatabase compatibel is.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Importeren vanuit een bestand Bacpac-in Azure SQL-Database met SqlPackage

Gebruik de volgende stappen uit het hulpprogramma [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) gebruiken om te importeren van een compatibele SQL Server-database (of Azure SQL-database) uit een Bacpac-bestand.

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u hebt al deze is ingericht een Azure SQL Database-server en de verbindingsgegevens bij de hand hebt.

1. Open een opdrachtprompt en wijzigen van een map met het hulpprogramma voor de opdrachtregel sqlpackage.exe - Dit hulpprogramma wordt geleverd met zowel Visual Studio en SQL Server.
2. Voer de volgende sqlpackage.exe-opdracht met de volgende argumenten voor uw omgeving:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| De argumenten  | Beschrijving  |
  	|---|---|
  	| < servernaam >  | de naam van de doel-server  |
  	| < databasenaam >  | de naam van de doel-database  |
  	| < gebruikersnaam >  | de naam van de gebruiker in de doelserver |
  	| < wachtwoord >  | wachtwoord van de gebruiker  |
  	| < source_file >  | de bestandsnaam en locatie voor het geïmporteerde Bacpac-bestand  |

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
