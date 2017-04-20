<properties
   pageTitle="SQL Server-database migreren met SQL-Database Database implementeren naar Microsoft Azure-Database Wizard gebruikt | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie Wizard van Microsoft Azure-Database"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>SQL Server-database migreren met SQL-Database met Database implementeren naar de Wizard van Microsoft Azure-Database


> [AZURE.SELECTOR]
- [De Wizard migreren SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar Bacpac-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit Bacpac-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transacties herhaling](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

De Database implementeren naar de wizard Microsoft Azure-Database in SQL Server Management Studio migreert een [compatibel SQL Server-database](sql-database-cloud-migrate.md) rechtstreeks aan op uw Azure SQL Database-server.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Gebruik de Database distribueren naar de Wizard van Microsoft Azure-Database

> [AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u hebt een [deze is ingericht SQL Database-server](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden bijgewerkt maandelijkse moet blijven synchroon met updates van de Azure-portal.

    > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).

2. Open Management Studio en verbinding maken met uw SQL Server-database moeten worden gemigreerd in Object Explorer.
3. Met de rechtermuisknop op de database in de Verkenner Object, wijs **taken**aan en klik op **Database implementeren met Microsoft Azure SQL-Database...**

    ![Implementeren naar Azure van menu taken](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  In de wizard uitvoeren, klikt u op **volgende**en klik vervolgens op **verbinding maken met** de verbinding met uw SQL-databaseserver configureren.

    ![Implementeren naar Azure van menu taken](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Voer in het verbinding maken met in het dialoogvenster Server de verbindingsgegevens verbinding maken met uw SQL-Database-server.

    ![Implementeren naar Azure van menu taken](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Geef de volgende handelingen uit voor het [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand dat deze wizard tijdens het migratieproces maakt:

 - De **naam van de nieuwe database** 
 - De **Editie van Microsoft Azure SQL-Database** ([servicelaag](sql-database-service-tiers.md))
 - De **maximale grootte van de database**
 - De **Service doelstelling** (prestatieniveau).
 - De **tijdelijke bestandsnaam**  

    ![Instellingen van een exporteren](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Voltooi de wizard. Afhankelijk van de grootte en complexiteit van de database duurt implementatie van een paar minuten meerdere uren. Als deze wizard compatibiliteitsproblemen detecteert, fouten naar het scherm worden weergegeven en de migratie niet doorgaat. Ga naar de [database compatibiliteitsproblemen oplossen](sql-database-cloud-migrate-fix-compatibility-issues.md)voor hulp bij het oplossen van database compatibiliteitsproblemen met.

7.  Object Verkenner verbinding maken met uw gemigreerde database in uw Azure SQL Database-server.
8.  De database en de eigenschappen van de Azure-portal, worden weergeven.

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
