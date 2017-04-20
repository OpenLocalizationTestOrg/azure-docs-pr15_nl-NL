<properties
   pageTitle="Een SQL Server-database migreren met Azure SQL-Database | Microsoft Azure"
   description="Microsoft Azure SQL-Database, database implementeert, databasemigratie, database importeren, exporteren database, wizard migreren"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Importeren uit BACPAC met behulp van SSMS SQL-Database

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In dit artikel leest hoe u importeren vanuit een bestand [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) met SQL-Database met de Wizard exporteren laag toepassing in SQL Server Management Studio.

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u hebt al deze is ingericht uw exemplaar van Azure SQL-logische en de verbindingsgegevens bij de hand hebt.

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden bijgewerkt maandelijkse moet blijven synchroon met updates van de Azure-portal.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).

2. Verbinding maken met uw Azure SQL Database-server, met de rechtermuisknop op de map **Databases** en klik op **importeren gegevens laag toepassing...**

    ![Importeren van gegevens laag toepassing menu-item](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  U maakt de database in Azure SQL-Database, een Bacpac-bestand importeren uit uw lokale schijf of Selecteer de opslag van Azure-account en de container waarnaar u uw Bacpac-bestand hebt geüpload.

    ![Importinstellingen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Wanneer een BACPAC van Azure-blobopslag importeren, gebruikt u standaard opslag. Een BACPAC importeren uit de premium-opslag wordt niet ondersteund.

4.  Geef de **nieuwe naam** voor de database op Azure SQL-DB, stelt u de **Editie van Microsoft Azure SQL-Database** (servicelaag), **maximale grootte van de database**en **Service doelstelling** (prestatieniveau).

    ![Database-instellingen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Klik op **volgende** en klik vervolgens op **Voltooien** om het Bacpac-bestand importeren in een nieuwe database in de Azure SQL Database-server.

6. Object Verkenner verbinding maken met uw gemigreerde database in uw Azure SQL Database-server.

6.  De database en de eigenschappen van de Azure-portal, worden weergeven.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
