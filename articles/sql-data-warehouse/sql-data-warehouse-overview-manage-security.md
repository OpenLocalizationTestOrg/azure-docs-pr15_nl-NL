<properties
   pageTitle="Een database in SQL Data Warehouse beveiligen | Microsoft Azure"
   description="Tips voor het beveiligen van een database in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Een database in SQL Data Warehouse beveiligen

> [AZURE.SELECTOR]
- [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
- [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

In dit artikel worden de basisbeginselen van het beveiligen van uw Data Warehouse van Azure SQL-database. Met name in dit artikel kunt u aan de slag met informatiebronnen voor het beperken van toegang, gegevens beveiligen en het monitoren, activiteiten op een database.

## <a name="connection-security"></a>Beveiliging van de verbinding

Beveiliging van de verbinding verwijst naar hoe u beperken en beveiligde verbindingen met uw database via firewallregels en verbinding versleutelen.

Firewallregels worden gebruikt door de server en de database naar verbindingspogingen van IP-adressen die niet expliciet whitelisted zijn negeren. Verbindingen vanuit de toepassing of het openbare IP-adres van de clientcomputer wilt toestaan, moet u eerst een serverniveau firewallregel met de Portal van Azure, REST API of PowerShell maken. Als een goede gewoonte, moet u het IP-adresbereiken toegestaan via uw server-firewall zo veel mogelijk te beperken.  Controleer of dat de firewall op uw netwerk en de lokale computer maakt uitgaande communicatie op TCP-poort 1433 voor toegang tot Azure SQL Data Warehouse vanaf de lokale computer.  Zie [Azure SQL Database-firewall][], [sp_set_firewall_rule][]en [sp_set_database_firewall_rule][]voor meer informatie.

Verbindingen met uw SQL Data Warehouse zijn versleuteld al dan niet standaard.  Wijzigen van de verbindingsinstellingen voor het uitschakelen van versleuteling worden genegeerd.

## <a name="authentication"></a>Verificatie

Verificatie verwijst naar het hoe u uw identiteit aantonen wanneer u verbinding maakt met de database. SQL Data Warehouse ondersteunt momenteel SQL Server-verificatie met een gebruikersnaam en wachtwoord, evenals een Azure Active Directory. 

Wanneer u de logische server voor uw database hebt gemaakt, kunt u een "-Serverbeheer" aanmelding opgegeven met een gebruikersnaam en wachtwoord. Deze referenties gebruikt, kunt u verifiëren met een database op die server als de database-eigenaar of 'dbo' tot en met SQL Server-verificatie.

Echter een aanbevolen om moeten gebruikers van uw organisatie gebruiken een ander account om te verifiëren. Deze manier kunt u de machtigingen om de toepassing te beperken en de risico's van schadelijke activiteit verkleinen wanneer uw toepassingscode vatbaar voor een SQL-injectieaanvallen is. 

Een SQL Server geverifieerde gebruiker maken, verbinding maken met **de hoofddatabase op de server met uw server admin aanmelden** en maak een nieuwe aanmelding voor de server.  Bovendien is het een goed idee om een gebruiker maken in de hoofddatabase voor gebruikers van Azure SQL Data Warehouse. Maken van een gebruiker in het model, kan een gebruiker zich aanmeldt met hulpprogramma's zoals SSMS zonder op te geven van de naam van een database.  Daarnaast kunt u ze de Objectverkenner gebruiken om te bekijken van alle databases op een SQL server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Vervolgens verbinding maken met uw **SQL Data Warehouse-database** maken met uw server admin aanmelden en maak een databasegebruiker op basis van de server login die u zojuist hebt gemaakt.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Als een gebruiker als u meer bewerkingen uitvoeren doen gaat zoals aanmeldingen te maken of nieuwe databases, ook moeten deze worden toegewezen aan de `Loginmanager` en `dbmanager` rollen in de hoofddatabase. Zie voor meer informatie over deze extra rollen en verificatie met een SQL-Database, [databases beheren en aanmeldingen in Azure SQL-Database][].  Zie voor meer informatie over Azure AD voor SQL Data Warehouse, [verbinding maken met SQL Data Warehouse met behulp van Azure Active Directory-verificatie][].


## <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat u in een Data Warehouse van Azure SQL-database doen kunt en dit wordt bepaald door uw gebruikersaccount rollidmaatschappen en machtigingen. Als een goede gewoonte, moet u gebruikers de minimaal benodigde bevoegdheden verlenen. Azure SQL Data Warehouse u deze gemakkelijk kunt beheren met rollen in T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

De server-beheerdersaccount die u verbinding met maakt is een lid van db_owner, die toestemming te ondernemen binnen de database heeft. Sla dit account voor het implementeren van schema upgrades en andere beheertaken uit te voeren. Gebruik het account "ApplicationUser" met beperkte machtigingen op uw toepassing in de database verbinding maken met het minimaal benodigde bevoegdheden nodig zijn voor uw toepassing.

Er zijn andere manieren verder te beperken wat een gebruiker kan doen met Azure SQL-Database:

- Gedetailleerde [machtigingen][] kunt u bepalen welke bewerkingen die u kunt doen op afzonderlijke kolommen, tabellen, weergaven, procedures en andere objecten in de database. Gedetailleerde machtigingen gebruiken om de meeste controle en het minimaal benodigde machtigingsniveau verlenen. Het systeem gedetailleerde machtiging is iets ingewikkeld en moet worden sommige onderzoeksopzet effectief gebruiken.
- [Databaserollen][] dan db_datareader en db_datawriter kan worden gebruikt om krachtige toepassingen gebruikersaccounts of minder krachtige management accounts te maken. De ingebouwde vaste databaserollen een eenvoudige manier om machtigingen te verlenen bieden, maar kunnen resulteren in het toekennen van machtigingen voor de meer dan nodig zijn.
- [Opgeslagen procedures][] kan worden gebruikt om de acties die kunnen worden uitgevoerd op de database te beperken.

Databases en logische servers beheren vanaf de Portal van Azure-klassieke of gebruiken van de Azure resourcemanager-API wordt bepaald door uw portal gebruikersaccount roltoewijzingen. Zie voor meer informatie over dit onderwerp [Rolgebaseerd toegangsbeheer in Azure-Portal][].

## <a name="encryption"></a>Versleuteling

Azure SQL Data Warehouse transparante gegevens versleuteling (TDE) helpt beschermen tegen schadelijke activiteit door realtime coderen en decoderen van uw gegevens in rust in te voeren.  Wanneer u uw database versleutelen, worden gekoppeld back-ups en transactie-logboekbestanden versleuteld zonder wijzigingen in uw toepassingen. TDE versleutelt de opslag van een volledige database met behulp van een symmetric sleutel database versleutelingssleutel genoemd. De databasesleutel wordt in SQL-Database beschermd door een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke SQL Database-server. Microsoft automatisch Hiermee draait u deze certificaten op minstens elke 90 dagen duurt. De versleutelingsalgoritme die worden gebruikt door SQL Data Warehouse is AES-256. Zie voor een algemene beschrijving van TDE, [Transparante gegevensversleuteling][].

U kunt de database met behulp van de [Portal van Azure] coderen[ Encryption with Portal] of [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Volgende stappen

Zie [verbinding maken met SQL Data Warehouse][]voor meer informatie en voorbeelden voor het verbinden met uw SQL Data Warehouse met verschillende protocollen.

<!--Image references-->

<!--Article references-->
[Verbinding maken met SQL datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Verbinding maken met SQL datawarehouse met Azure Active Directory-verificatie]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database-firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Databaserollen]: https://msdn.microsoft.com/library/ms189121.aspx
[Databases en aanmeldingen in Azure SQL-Database beheren]: https://msdn.microsoft.com/library/ee336235.aspx
[Machtigingen]: https://msdn.microsoft.com/library/ms191291.aspx
[Opgeslagen procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparante gegevensversleuteling]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Rolgebaseerd toegangsbeheer in Azure-Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
