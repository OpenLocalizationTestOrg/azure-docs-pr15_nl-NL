<properties
   pageTitle="SQL Server-database migreren met SQL-Database | Microsoft Azure"
   description="Meer informatie over hoe on-premises implementatie SQL Server-database migreren met Azure SQL-Database in de cloud. Database migratiehulpprogramma's gebruiken om te testen compatibiliteit vóór de databasemigratie."
   keywords="database-migratie, migratie van sql server-database, hulpmiddelen voor databases-migratie, database migreren, sql-database migreren"
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

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server-database migreren met SQL-Database in de cloud

In dit artikel leert u hoe een lokale SQL Server 2005 database of hoger met Azure SQL-Database migreren. In dit databasemigratieproces migreren u uw schema en uw gegevens van de SQL Server-database in uw huidige omgeving in SQL-Database. Als u wilt is gelukt, moet de bestaande database eerst een toets compatibiliteit doorgeven. Met [SQL-Database V12](sql-database-v12-whats-new.md), zijn er enkele resterende compatibiliteitsproblemen, gaat u dan probleem die betrekking hebben op serverniveau en meerdere databases bewerkingen. Databases en toepassingen die afhankelijk van zijn [gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md) nodig sommige herstructureren om op te lossen van deze compatibiliteitsproblemen voordat de SQL Server-database kan worden gemigreerd.

Als u wilt migreren, zijn de volgende handelingen uit de stappen u moet uitvoeren:

- **Test voor compatibiliteit**: databasecompatibiliteit met [SQL-Database V12](sql-database-v12-whats-new.md)valideren. 
- **Oplossen compatibiliteitsproblemen, indien van toepassing**: als validatie is mislukt, moet u de fouten met gegevensvalidatie oplossen.  
- **De migratie uitvoeren** Als u uw database hebt compatibele, kunt u een of meer methoden voor het uitvoeren van de migratie. 

SQL Server bevat verschillende methoden voor het uitvoeren van deze taken. Dit artikel bevat een overzicht van de beschikbare methoden voor elke taak. In het volgende diagram ziet u de stappen en de methoden.

  ![VSSSDT migratie-diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Als u wilt migreren van een niet - SQL Server-database, zoals Microsoft Access, Sybase, MySQL Oracle en DB2 met Azure SQL-Database, raadpleegt u [-Assistent migratie van SQL Server](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Hulpmiddelen voor databases-migratie testen van SQL Server-databasecompatibiliteit met SQL-Database

Als u wilt testen op compatibiliteitsproblemen SQL-Database voordat u het databasemigratieproces begint, gebruikt u een van de volgende methoden:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade-advies](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools voor Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT gebruikmaakt van de meest recente compatibiliteit regels om te bepalen van de SQL-Database V12 compatibiliteitsproblemen. Als er compatibiliteitsproblemen zijn gevonden, kunt u rechtstreeks in dit hulpmiddel kunt gevonden problemen oplossen. Dit is de aanbevolen methode om te testen en corrigeren van de SQL-Database V12 compatibiliteitsproblemen. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage is een hulpprogramma voor de opdrachtregel voor compatibiliteit problemen en genereert een rapport met gevonden compatibiliteitsproblemen te testen. Als u dit hulpprogramma gebruikt, zorg er dan voor dat u de meest recente versie gebruiken voor het gebruik van de meest recente compatibiliteit regels. Als er fouten zijn gevonden, wordt moet u een ander hulpmiddel gebruiken om op te lossen eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen.  
- [Wizard van de gegevens voor het exporteren laag toepassing in SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): met deze wizard heeft gedetecteerd en fouten naar het scherm. Als er niet fouten zijn gevonden, kunt u doorgaan en voert u de migratie met SQL-Database. Als er fouten zijn gevonden, wordt moet u een ander hulpmiddel gebruiken om op te lossen eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen.
- [De Microsoft SQL Server 2016 Upgrade Advisor Preview](http://www.microsoft.com/download/details.aspx?id=48119): dit hulpprogramma zelfstandige, die zich in de Preview-versie, detecteert en genereert een rapport met SQL-Database V12 compatibiliteitsproblemen. Dit hulpmiddel heeft nog niet de meest recente compatibiliteit regels. Als er geen fouten zijn gevonden, kunt u doorgaan en voert u de migratie met SQL-Database. Als er fouten zijn gevonden, wordt moet u een ander hulpmiddel gebruiken om op te lossen eventuele gevonden compatibiliteitsproblemen - SSDT aanbevolen. 
- [Wizard SQL Azure-migreren ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW is een codeplex-hulpprogramma waarin de regels van de compatibiliteit V11: Azure SQL-Database voor het detecteren van Azure SQL Database V12 compatibiliteitsproblemen wordt gebruikt. Als er compatibiliteitsproblemen zijn gevonden, kunnen bepaalde problemen rechtstreeks in dit hulpprogramma worden verholpen. Dit hulpmiddel vindt mogelijk compatibiliteitsproblemen die niet moeten worden opgelost. Dit is het eerste Azure SQL Database-migratie hulpprogramma hulp beschikbaar en actief wordt ondersteund door de SQL Server-community. Dit hulpmiddel kunt ook de migratie van binnen het hulpmiddel zelf te voltooien. 

## <a name="fix-database-migration-compatibility-issues"></a>Databasemigratie compatibiliteitsproblemen oplossen

Als er compatibiliteitsproblemen zijn gevonden, moet u deze oplossen voordat u verder gaat met de migratie van SQL Server-database. Er zijn tal van compatibiliteitsproblemen die u tegenkomen kunt, afhankelijk van beide op de versie van SQL Server in de brondatabase en de complexiteit van de database die u wilt migreren. Oudere versies van SQL Server hebben meer compatibiliteitsproblemen met. Gebruik de volgende bronnen, naast een gerichte Internet zoekprogramma's via het zoekprogramma van opties:

- [Onderdelen van SQL Server-database niet worden ondersteund in Azure SQL-Database](sql-database-transact-sql-information.md)
- [Stopgezette Database Engine functionaliteit in SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Stopgezette Database Engine functionaliteit in SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Stopgezette Database Engine functionaliteit in SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Stopgezette Database Engine functionaliteit in SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Stopgezette Database Engine functionaliteit in SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Daarnaast kunnen zoeken op Internet en deze bronnen gebruikt, gebruikt u de [MSDN SQL Server-community-forums](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) of [StackOverflow](http://stackoverflow.com/).

Gebruik een van de volgende database migratiehulpprogramma's naar de aangetroffen problemen oplossen:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Gebruik [SQL Server Data Tools voor Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): als u wilt gebruiken SSDT, importeren u uw databaseschema in SQL Server Data Tools voor Visual Studio "SSDT") en bouwen van het project voor een SQL-Database V12-implementatie. U vervolgens verhelpen alle gevonden compatibiliteitsproblemen in SSDT. Als alles compleet is, u de wijzigingen terug naar de brondatabase (of een kopie van de brondatabase synchroniseren. SSDT is momenteel de aanbevolen methode om te testen en SQL-Database V12 compatibiliteitsproblemen oplossen. Volg de koppeling voor een [overzicht gebruiken SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Gebruik van [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): als u wilt gebruiken SSMS, die u uitvoert Transact-SQL-opdrachten de gedetecteerd met een ander hulpmiddel fouten op te lossen. Deze methode is hoofdzakelijk voor geavanceerde gebruikers voor het wijzigen van het databaseschema rechtstreeks in de brondatabase. 
- Wizard [SQL Azure migratie](sql-database-cloud-migrate-fix-compatibility-issues.md)gebruiken ("SAMW"): als u wilt gebruiken SAMW, u een Transact-SQL-script genereren van de brondatabase. De wizard het script, indien mogelijk, zodat u het schema compatibel is met de SQL-Database V12 getransformeerd. Als alles compleet is, kunt SAMW verbinden met SQL-Database V12 het script uitvoeren. In dit hulpprogramma analyseert ook doelcellen bestanden om te bepalen compatibiliteitsproblemen. Het script kan worden gegenereerd met schema alleen of kan gegevens bevatten in BCP-indeling.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Een compatibele SQL Server-database migreren met SQL-Database

Microsoft biedt voor het migreren van een compatibele SQL Server-database, verschillende migratiemethoden voor verschillende scenario's. Welke methode die u kiest, hangt af van uw tolerantie voor downtime, de grootte en complexiteit van uw SQL Server-database en de verbinding met de cloud Microsoft Azure.  

> [AZURE.SELECTOR]
- [De Wizard migreren SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporteren naar Bacpac-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importeren uit Bacpac-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transacties herhaling](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Als u wilt uw migratiemethode kiest, is de eerste vraag als u de database uit productie tijdens de migratie uitvoeren vast. Een database migreren terwijl actieve transacties plaatsvinden kan resulteren in de database inconsistenties en database is mogelijk beschadigd. Zijn er veel methoden voor het stilleggen een database, uit het uitschakelen van clientconnectiviteit voor het maken van een [momentopname van de database](https://msdn.microsoft.com/library/ms175876.aspx).

Als u wilt migreren met minimale uitvaltijd, door [SQL Server transactie replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md) te gebruiken als uw database voldoet aan de vereisten voor transacties herhaling. Als u bepaalde downtime vast of u een testmigratie van een productiedatabase voor later migratie uitvoert, kunt u een van de volgende drie manieren:

- [De Wizard migreren SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): voor kleine en middelgrote databases, voor het migreren van een compatibele SQL Server 2005 database of hoger is zo eenvoudig als het uitvoeren van de [Database implementeren naar de Wizard van Microsoft Azure-Database](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) in SQL Server Management Studio.
- [Exporteren naar Bacpac-bestand](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) en klik vervolgens op [importeren uit Bacpac-bestand](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): als u connectivity uitdagingen (geen connectivity, lage bandbreedte of time-out problemen) en voor middelgrote tot grote databases, gebruikt u een bestand [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) . Met deze methode u een het schema van SQL Server en de gegevens exporteren naar een Bacpac-bestand. U vervolgens importeren het Bacpac-bestand naar SQL-Database met de gegevens laag toepassing Wizard exporteren in SQL Server Management Studio of het hulpprogramma van de opdrachtprompt [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) .
- Bacpac- en BCP samen gebruiken: een [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) bestands- en [BCP](https://msdn.microsoft.com/library/ms162802.aspx) voor veel groter databases gebruiken om te bereiken groter parallelization voor vergroot de prestaties, met groter complexiteit. Migreren afzonderlijk het schema en de gegevens met deze methode.
 - [Het schema alleen naar een Bacpac-bestand exporteren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Het schema alleen uit het Bacpac-bestand importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) naar SQL-Database.
 - Gebruik [BCP](https://msdn.microsoft.com/library/ms162802.aspx) om de gegevens hebt geëxtraheerd naar platte bestanden en klik vervolgens op [parallelle laden](https://technet.microsoft.com/library/dd425070.aspx) deze bestanden naar Azure SQL-Database.

     ![SQL Server database migratie - SQL-database migreren naar de cloud.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Volgende stappen

- [De Upgrade Advisor-Preview-versie van Microsoft SQL Server 2016](http://www.microsoft.com/download/details.aspx?id=48119)
- [Nieuwste versie van SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nieuwste versie van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database V12](sql-database-v12-whats-new.md)
[Transact-SQL gedeeltelijk of niet-ondersteunde functies](sql-database-transact-sql-information.md)
- [Niet - SQL Server-databases met SQL Server-migratie assistent migreren](http://blogs.msdn.com/b/ssma/)
