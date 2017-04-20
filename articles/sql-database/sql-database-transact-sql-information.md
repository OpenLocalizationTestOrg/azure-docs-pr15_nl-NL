<properties
   pageTitle="Niet worden ondersteund in Azure SQL Database T-SQL | Microsoft Azure"
   description="Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL-Database"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL Database Transact-SQL-verschillen


De Transact-SQL-functies die toepassingen, hangt af van de meeste worden ondersteund in zowel Microsoft SQL Server en Azure SQL-Database. Voert u een gedeeltelijke lijst met ondersteunde functies voor toepassingen:

- Gegevenstypen.
- Operatoren.
- String, de rekenkundige, logische, en de cursor functies.

Azure SQL-Database is echter ontworpen te isoleren functies uit alle afhankelijkheden van **de hoofddatabase** . Als gevolg hiervan veel serverniveau activiteiten niet geschikt zijn voor SQL-Database en worden niet ondersteund. Functies die zijn afgeschaft in SQL Server worden gewoonlijk niet ondersteund in SQL-Database.

> [AZURE.NOTE]
> In dit onderwerp wordt beschreven hoe de functies die beschikbaar in SQL-Database zijn wanneer bijgewerkt naar de huidige versie; SQL-Database V12. Zie voor meer informatie over V12, [SQL-Database V12 wat van nieuw](sql-database-v12-whats-new.md).

De volgende secties bevatten functies die worden gedeeltelijk ondersteund en de functies die niet worden volledig ondersteund.


## <a name="features-partially-supported-in-sql-database-v12"></a>Functies die gedeeltelijk worden ondersteund in SQL-Database V12

SQL-Database V12 ondersteunt sommige maar niet alle de argumenten die bestaat in de bijbehorende SQL Server 2016 Transact-SQL-instructies. De instructie CREATE PROCEDURE is bijvoorbeeld beschikbaar maar de opties van CREATE PROCEDURE niet beschikbaar zijn. Raadpleeg de syntaxis van de gekoppelde-onderwerpen voor meer informatie over de ondersteunde gebieden van elke-instructie.

- Databases: [maken](https://msdn.microsoft.com/library/dn268335.aspx )/[wijzigen](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs zijn in het algemeen beschikbaar zijn voor functies die beschikbaar zijn.
- Functies: [maken](https://msdn.microsoft.com/library/ms186755.aspx)/[functie wijzigen](https://msdn.microsoft.com/library/ms186967.aspx)
- [BEËINDIGEN](https://msdn.microsoft.com/library/ms173730.aspx) 
- Aanmeldingen: [maken](https://msdn.microsoft.com/library/ms189751.aspx)/[LOGIN wijzigen](https://msdn.microsoft.com/library/ms189828.aspx)
- Opgeslagen procedures: [maken](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabellen: [maken](https://msdn.microsoft.com/library/dn305849.aspx)/[wijzigen](https://msdn.microsoft.com/library/ms190273.aspx)
- Typen (aangepast): [TYPE maken](https://msdn.microsoft.com/library/ms175007.aspx)
- Gebruikers: [maken](https://msdn.microsoft.com/library/ms173463.aspx)/[gebruiker wijzigen](https://msdn.microsoft.com/library/ms176060.aspx)
- Weergaven: [maken](https://msdn.microsoft.com/library/ms187956.aspx)/[weergave wijzigen](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Functies die niet worden ondersteund in SQL-Database

- Sortering van systeemobjecten
- Verbinding gerelateerd: ORIGINAL_DB_NAME-eindpunt instructies. SQL-Database biedt geen ondersteuning voor Windows-verificatie, maar biedt ondersteuning voor de vergelijkbare Azure Active Directory-verificatie. Bepaalde verificatietypen is vereist voor de meest recente versie van SSMS. Zie [verbinding maken met SQL-Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)voor meer informatie.
- Cross databasequery's drie of vier deel namen gebruiken. (Alleen-lezenquery's van meerdere databases worden ondersteund met behulp van [elastische databasequery](sql-database-elastic-query-overview.md).)
- Cross-database koppelen van eigendom, betrouwbare instelling
- Gegevens verzamelen
- Databasediagrammen
- Database Mail
- DATABASEPROPERTY (gebruiken in plaats daarvan DATABASEPROPERTYEX)
- EXECUTE AS aanmeldingen
- Versleuteling: extensible key management
- Eventing: gebeurtenissen, meldingen, querymeldingen
- Functies die zijn gerelateerd aan de plaatsing van de database-bestanden, de grootte en databasebestanden die automatisch worden beheerd door Microsoft Azure.
- Functies die betrekking hebben op beschikbaarheid, dat wordt beheerd via uw Microsoft Azure-account: back-up maken, te herstellen, AlwaysOn, spiegelen, logboekbestanden, herstelmodi. Voor meer informatie, Zie Azure SQL Database back-up en herstellen.
- Functies die afhankelijk zijn van de lezer log is uitgevoerd op SQL-Database: Push-replicatie, wijzigingsgegevens vastleggen.
- Functies die afhankelijk van de SQL Server-Agent of de database MSDB zijn: taken, meldingen, operatoren, op basis van beleid Management, database mail, servers voor het centrale beheer.
- FILESTREAM
- Functies: fn_get_sql, fn_virtualfilestats, fn_virtualservernodes
- Globale tijdelijke tabellen
- Serverinstellingen hardware zijn gerelateerd: geheugen, werk-threads, Processorsnelheid affiniteit, doelcellen vlaggen, enzovoort. Gebruik in plaats hiervan de serviceniveaus.
- HAS_DBACCESS
- STAT TAAK BEËINDIGEN
- Gekoppelde servers, OPENQUERY OPENROWSET, OPENDATASOURCE, BULKSGEWIJS invoegen en vierdelige namen
- Basispagina/doel-servers
- .NET framework [CLR-integratie met SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Resource-gouverneur
- Semantische zoeken
- Referenties voor de server. Database gebruiken in plaats daarvan beperkt van referenties.
- Items op een server niveau: Server rollen, IS_SRVROLEMEMBER, sys.login_token. Machtigingen voor het siteverzamelingsniveau van server zijn niet beschikbaar, hoewel enkele worden vervangen door de database op gebruikersniveau machtigingen. Sommige DMVs serverniveau zijn niet beschikbaar hoewel enkele worden vervangen door de database op gebruikersniveau DMVs.
- Als u kiest express: localdb, gebruikersexemplaren
- Service makelaar
- REMOTE_PROC_TRANSACTIONS INSTELLEN
- AFSLUITEN
- sp_addmessage
- Opties voor sp_configure en opnieuw te configureren. Sommige opties zijn beschikbaar met [ALTER DATABASE beperkt configuratie](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- Controle van SQL Server. Gebruik de SQL-Database in plaats daarvan controle.
- SQL Server Profiler
- SQL Server-trace
- Vlaggen aanwijzen. Sommige items van de vlag doelcellen zijn verplaatst naar de compatibiliteitsmodus.
- Voor foutopsporing in Transact-SQL
- Triggers: Binnen bereik van de Server of aanmelding triggers
- USE-instructie: als u wilt wijzigen van de databasecontext naar een andere database, moet u een nieuwe verbinding met de nieuwe database maken.


## <a name="full-transact-sql-reference"></a>Volledige Transact-SQL-verwijzing

Zie [Transact-SQL-verwijzing (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) in SQL Server Books Online voor meer informatie over de grammaticacontrole, gebruik en voorbeelden Transact-SQL. 

### <a name="about-the-applies-to-tags"></a>Over de labels "Is van toepassing op"

De Transact-SQL-verwijzing bevat onderwerpen die betrekking hebben op versies van SQL Server 2008 bij de huidige. Klik onder de titel van het onderwerp is een pictogram vermelding staaf-, de vier SQL Server-platforms en die aangeeft dat de toepassing. Van de beschikbaarheidsgroepen zijn bijvoorbeeld geïntroduceerd in SQL Server 2012. Het onderwerp [AVAILABILTY-groep maken](https://msdn.microsoft.com/library/ff878399.aspx) wordt aangegeven dat de instructie is van toepassing op ** SQL Server (beginnend met 2012). De instructie geldt niet voor SQL Server 2008, SQL Server 2008 R2, Azure SQL-Database, Azure SQL Data Warehouse of Parallel Data Warehouse.

In sommige gevallen het algemene onderwerp van een onderwerp in een product kan worden gebruikt, maar er zijn kleine verschillen tussen producten. De verschillen zijn aangegeven bij middelpunten in het onderwerp desgewenst.

