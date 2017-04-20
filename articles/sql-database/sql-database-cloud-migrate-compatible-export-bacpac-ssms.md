
<properties
   pageTitle="Een SQL Server-database exporteren naar een Bacpac-bestand met SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL-Database, databasemigratie database exporteren, Bacpac-bestand, exporteren gegevens laag toepassing wizard Exporteren"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Een SQL Server-database exporteren naar een Bacpac-bestand met SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
In dit artikel leest hoe u het exporteren van een SQL Server-database naar een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -bestand met de Wizard exporteren laag toepassing in SQL Server Management Studio. 

1. Controleer of u de nieuwste versie van SQL Server Management Studio. Nieuwe versies van Management Studio worden bijgewerkt maandelijkse moet blijven synchroon met updates van de Azure-portal.

     > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).

2. Open Management Studio en verbinding maken met de brondatabase in Object Explorer.

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Met de rechtermuisknop op de brondatabase in de Verkenner Object, wijs **taken**aan en klik op **Exporteren gegevens laag toepassing...**

    ![Een toepassing voor de laag gegevens exporteren vanuit het menu taken](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Configureer de exporteren als u wilt het Bacpac-bestand opslaat naar een met een lokale schijflocatie of naar een Azure blob in de wizard exporteren. De geëxporteerde BACPAC bevat altijd het volledige databaseschema en, al dan niet standaard, gegevens uit alle tabellen. Gebruik het tabblad Geavanceerd als u wilt uitsluiten van de gegevens van sommige of alle tabellen. U kunt, bijvoorbeeld kiezen om alleen de gegevens voor verwijzingstabellen, in plaats van alle tabellen te exporteren.

***Belangrijke*** Wanneer een BACPAC naar Azure-blobopslag exporteert, gebruikt u standaard opslag. Een BACPAC importeren uit de premium-opslag wordt niet ondersteund.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
