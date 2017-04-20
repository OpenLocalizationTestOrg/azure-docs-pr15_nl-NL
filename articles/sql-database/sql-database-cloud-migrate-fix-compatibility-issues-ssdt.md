<properties
   pageTitle="Compatibiliteitsproblemen van SQL Server-database voor de migratie met SQL-Database oplossen | Microsoft Azure"
   description="Microsoft Azure SQL Database, databasemigratie, compatibiliteit, de Wizard in de SQL Azure-migratie, SSDT"
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

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Een SQL Server-Database migreren met Azure SQL-Database met behulp van SQL Server Data Tools voor Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade-advies](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In dit artikel leert u detecteren en oplossen van SQL Server-database-compatibiliteitsproblemen met behulp van de SQL Server Data Tools voor Visual Studio vóór de migratie met Azure SQL-Database.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Met behulp van SQL Server Data Tools voor Visual Studio

Gebruik SQL Server Data Tools voor Visual Studio ("SSDT") het databaseschema importeren in een project Visual Studio-database voor analyse. Als u wilt analyseren, die u kunt opgeven van het doelplatform voor het project als SQL-Database V12 en vervolgens het project te maken. Als het bouwen geslaagd is, is de database compatibel. Als het bouwen mislukt, kunt u de fouten in SSDT (of een van de andere hulpprogramma's die worden beschreven in dit onderwerp) kunt oplossen. Zodra het project met succes wordt gemaakt, kunt u deze kunt publiceren weer als een kopie van de brondatabase. Vervolgens kunt u de vergelijkingsfunctie in SSDT kopiëren van de gegevens van de brondatabase naar de compatibele V12 van Azure SQL-database. Vervolgens kunt u deze bijgewerkte database migreren. Als u deze optie gebruikt, moet u de [nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)downloaden.

  ![VSSSDT migratie-diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Als alleen-schema-migratie moet ondernomen worden, kan het schema worden gepubliceerd rechtstreeks vanuit de Visual Studio rechtstreeks naar Azure SQL-Database. Gebruik deze methode als u meer wijzigingen dan door de wizard migreren alleen kunnen worden verwerkt door het databaseschema is vereist.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Compatibiliteitsproblemen met behulp van SQL Server Data Tools voor Visual Studio detecteren
   
1.  Open de **SQL Server-Object Explorer** in Visual Studio. Met **SQL Server toevoegen** verbinding maken met de SQL Server-instantie met de database wordt gemigreerd. Zoek de database in Object Explorer, met de rechtermuisknop op de database en selecteer **Nieuw Project maken...**     
    
    ![Nieuw Project](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Configureer de importinstellingen **objecten alleen**importeren. Schakel de opties voor het importeren van de volgende handelingen uit: waarnaar wordt verwezen aanmeldingen, machtigingen en instellingen van de database.    

    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Klik op **Start** als u wilt importeren van de database en het project met een T-SQL-scriptbestand voor elk object in de database maken. De scriptbestanden worden genest in mappen binnen het project.    

    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  Klik in de Visual Studio Solution Explorer met de rechtermuisknop op het databaseproject en selecteer Eigenschappen. Klik op de pagina **Project-instellingen** configureren de doel-Platform naar Microsoft Azure SQL Database V12.    
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Met de rechtermuisknop op het project en selecteer **maken** voor het maken van het project.    
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  Elke incompatibiliteit tussen in de **Lijst met fouten** weergegeven. In dit geval is de gebruikersnaam NT Authority\Netwerkservice niet compatibel. Omdat dit niet compatibel is, kunt u dit opmerkingen te plaatsen of verwijdert u deze (en adres van de gevolgen van het verwijderen van deze aanmelding en de rol van de databaseoplossing).     
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Compatibiliteitsproblemen met behulp van SQL Server Data Tools voor Visual Studio op te lossen

1.  Dubbelklik op het eerste script om te openen van het script in een query-venster en het script commentaar en klikt u vervolgens het script uitvoeren.     
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Herhaal deze procedure voor elk script compatibiliteitsproblemen met totdat er geen fout zijn.    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Wanneer de database geen fouten bevat is, wordt met de rechtermuisknop op het project en selecteer **publiceren**. Een kopie van de brondatabase is gebouwd en gepubliceerd (het is raadzaam een kopie, ten minste in eerste instantie gebruiken).     
 - Voordat u publiceert, afhankelijk van de gegevensbron SQL Server-versie (eerder dan SQL Server-2014), mogelijk moet u opnieuw instellen van het project doelplatform om in te schakelen implementatie.     
 - Als u een oudere SQL Server-database migreert, kan niet beter geen functionaliteit opnemen in het project die niet worden ondersteund in de bron SQL Server tot de database te migreren naar een nieuwere versie van SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  In SQL Server-Objectverkenner met de rechtermuisknop op de brondatabase en klik op **Gegevensvergelijking**. Vergelijking van het project met de oorspronkelijke database, kunt u meer informatie over welke wijzigingen zijn aangebracht door de wizard. Selecteer uw V12 van Azure SQL-versie van de database en klik vervolgens op **Voltooien**.    
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Controleer de verschillen gedetecteerd en klik op **Update doel** voor het migreren van gegevens uit de brondatabase in de V12 van Azure SQL-database.     
    
    ![alternatieve tekst](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Kies een implementatiemethode. Zie [een compatibele SQL Server-database migreren met SQL-Database.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Volgende stappen

- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
- [Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
