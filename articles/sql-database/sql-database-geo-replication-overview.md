<properties
    pageTitle="Actieve geografische-replicatie voor Azure SQL-Database"
    description="Actieve geografische-herhaling kunt u setup-4 kopieën van uw database in een van de Azure datacenters."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Overzicht: SQL actieve geografische-databasereplicatie

Actieve geografische-herhaling kunt u configureren om maximaal vier leesbare secundaire databases in de locaties in dezelfde of een andere data center (regio's). Secundaire databases zijn beschikbaar voor query's en voor failover in het geval van een storing gegevens-beheercentrum of problemen met de verbinding maken met de hoofddatabase.

>[AZURE.NOTE] Actieve geografische-herhaling (leesbare ontvangen) is nu beschikbaar voor alle databases in alle Servicelagen. Klik in April 2017, wordt het niet-leesbare secundaire type ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar leesbare secundaire servers.

 Kunt u actieve geografische-replicatie gebruik van de [Azure-portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md) [Transact-SQL](sql-database-geo-replication-transact-sql.md), configureren of de [REST API - maken of bijwerken Database](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Configureren: Azure portal](sql-database-geo-replication-portal.md)
- [Configureren: PowerShell](sql-database-geo-replication-powershell.md)
- [Configureren: T-SQL](sql-database-geo-replication-transact-sql.md)

Als voor een reden uw primaire database mislukt, of gewoon moet worden ondernomen offline, *kunt u naar een van de secundaire databases* . Wanneer failover is geactiveerd op een van de secundaire-databases, worden alle andere secundaire servers worden automatisch gekoppeld aan de nieuwe primaire.

Kunt u naar een secundair met de [portal van Azure](sql-database-geo-replication-failover-portal.md) [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), de [REST API - Failover geplande](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)of [REST API - niet-geplande Failover](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Failover: Azure portal](sql-database-geo-replication-failover-portal.md)
- [Failover: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Failover: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Controleer dat de verificatievereisten voor de server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging van de SQL-Database na herstel](sql-database-geo-replication-security-config.md)voor meer informatie.


De functie actieve geografische-herhaling implementeert een om bieden van database redundantie binnen dezelfde Microsoft Azure regio of in verschillende gebieden (geografische-redundantie). Actieve geografische-replicatie is asynchroon wordt overgenomen door vastgelegde transacties uit een database op maximaal vier kopieën van de database op andere servers bevinden, met vastgelegde overgeslagen (RCSI) voor moeten worden geïsoleerd lezen. Wanneer actieve geografische-replicatie is geconfigureerd, wordt een secundaire database gemaakt op de opgegeven server. De oorspronkelijke database wordt de hoofddatabase. De hoofddatabase asynchroon wordt overgenomen door vastgelegde transacties elk van de secundaire-databases. Alleen volledige transacties worden gerepliceerd. Terwijl u op elk gewenst moment de secundaire database wijkt mogelijk iets achter de hoofddatabase, de secundaire gegevens gegarandeerd nooit zelf hoeft gedeeltelijke transacties. De specifieke gegevens van de vrijgegeven Productieorder kan worden gevonden op [Overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

Een van de belangrijkste voordelen van actieve geografische-replicatie is dat u een database op gebruikersniveau noodgevallen herstel-oplossing met lage hersteltijd. Als u de secundaire database op een server in een ander gebied plaatst, kunt u maximale flexibiliteit toevoegen aan uw toepassing. Cross-regio redundantie kan toepassingen terughalen uit een permanente verlies van een hele datacenter of delen van een datacenter die worden veroorzaakt door natuurrampen, kritieke menselijke fouten of schadelijke acties. De volgende afbeelding ziet dat een voorbeeld van actieve geografische-replicatie is geconfigureerd met een primair in de regio Noord centraal ons en secundaire in de regio Zuid centraal ons.

![Geografische herhaling relatie](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Een ander belangrijk voordeel is dat de secundaire databases kunnen gelezen worden en laten alleen-lezen-werkbelastingen zoals rapportage taken kunnen worden gebruikt. Als u alleen de secundaire database voor taakverdeling gebruiken wilt, kunt u deze hebt gemaakt in de regio hetzelfde als de primaire. U maakt een secundair in dezelfde regio, vergroot van de toepassing flexibiliteit naar kritieke fouten optreden.  

Er zijn andere scenario's waarin actieve geografische-replicatie kan worden gebruikt:

- **Database-migratie**: U kunt actieve geografische-herhaling gebruiken om een database migreren van de ene server naar een andere online met minimale uitvaltijd.
- **Toepassingsupgrades**: U kunt een extra secundair als een fail back-up maken tijdens toepassingsupgrades.

Als u wilt bereiken reële bedrijfscontinuïteit, met de database redundantie tussen datacenters toevoegen is slechts een deel van de oplossing. Een toepassing (service) end-to-end herstellen nadat een volledige uitval herstel van alle onderdelen waaruit de service en afhankelijke services vereist. Voorbeelden van deze onderdelen zijn de clientsoftware (bijvoorbeeld een browser met een aangepaste JavaScript), web voorste uiteinden, opslag en DNS. Het is belangrijk dat alle onderdelen tegen de dezelfde fouten zijn en beschikbaar is binnen het herstelproces is tijd doel (RTO) van uw toepassing. Daarom moet u alle afhankelijke services identificeren en begrijpt de garanties en de mogelijkheden die ze bieden. Vervolgens moet u de nodige maatregelen om ervoor te zorgen dat uw servicefuncties tijdens de overname van de services waarvan deze afhankelijk is uitvoeren. Zie voor meer informatie over ontwerpen oplossingen voor herstel, [Cloud-oplossingen ontwerpen voor noodgevallen herstel met actieve geografische-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Actieve geografische herhaling mogelijkheden
De functie actieve geografische-herhaling biedt de volgende essentiële mogelijkheden:

- **Automatische asynchroon replicatie**: U kunt alleen een secundaire database maken door toe te voegen aan een bestaande database. De secundaire kan alleen worden gemaakt in een andere Azure SQL Database-server. Zodra u hebt gemaakt, worden de secundaire database wordt gevuld met de gegevens hebt gekopieerd uit de hoofddatabase. Dit proces wordt seeding genoemd. Nadat de secundaire database is gemaakt en overgeënt, worden updates voor de hoofddatabase asynchroon gerepliceerd naar de secundaire database automatisch. Asynchroon replicatie betekent dat transacties worden vastgelegd voor de primaire database voordat ze worden gerepliceerd naar de secundaire database. 

- **Meerdere secundaire databases**: twee of meer secundaire databases vergroten redundantie en beschermingsniveau voor de primaire database en de toepassing. Als meerdere secundaire databases bestaat, blijft de toepassing beveiligd, zelfs als een van de secundaire databases mislukt. Als er slechts één secundaire database, en dit mislukt, wordt de toepassing blootgesteld aan hoger risico totdat er een nieuwe secundaire database is gemaakt.

- **Leesbare secundaire databases**: een toepassing toegang heeft tot een secundaire database voor alleen-lezen-bewerkingen voor het gebruik van de beveiliging van dezelfde of een andere principes gebruikt voor toegang tot de hoofddatabase. De secundaire databases werken in de modus van momentopname geïsoleerd om ervoor te zorgen herhaling van de updates van de primaire (log opnieuw afspelen) is niet vertraagd door query's die zijn uitgevoerd op de secundaire.

>[AZURE.NOTE] Het logboek opnieuw afspelen is vertraagd op de secundaire database als er op schema-updates die deze ontvangt van de primaire waarvoor de schemavergrendeling van een van de secundaire database.

- **Actieve geografische-herhaling van elastische groep databases**: actieve geografische-replicatie kan worden geconfigureerd voor elke database in een groep elastische database. De secundaire database kan zich in een andere elastische database-toepassingen. Voor gewone databases, kan de secundaire een elastische database-toepassingen en vice versa zijn zo lang maken als de Servicelagen hetzelfde zijn. 

- **Configureerbare prestatieniveau van de secundaire database**: een secundaire database kan worden gemaakt met lagere prestatieniveau dan de primaire. Primaire en secundaire databases moeten zijn hetzelfde niveau service hebt. Deze optie wordt niet aanbevolen voor toepassingen met hoge database schrijven activiteit omdat de vertraging meer replicatie wordt de kans op pesterijen aanzienlijke gegevensverlies verhoogd na een failover. Prestaties van de toepassing wordt bovendien na failover totdat de nieuwe primaire wordt bijgewerkt naar een hoger prestatieniveau beïnvloed. De grafiek log IO percentage op Azure-portal vindt u een goede manier om een schatting van de minimale prestatieniveau van de secundaire dat is vereist voor het laden herhaling vangen te. Als uw primaire database P6 bijvoorbeeld (1000 DTU) en het logboek IO procent is 50% is de secundaire moet ten minste P4 (500 DTU). U kunt ook de logboekgegevens IO [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) of [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) database-weergaven gebruiken ophalen.  Zie voor meer informatie over de prestaties van de SQL-Database, [Opties voor de SQL-Database en prestaties](sql-database-service-tiers.md). 

- **Door de gebruiker ingestelde failover en foutherstel**: een secundaire database kan expliciet worden veranderd aan de primaire rol op elk gewenst moment door de toepassing of de gebruiker. Tijdens een reële storing moet de optie 'ongeplande' worden gebruikt, die direct verhoogt het niveau van een secundair moeten de primaire. Wanneer de mislukte primaire hersteld en weer beschikbaar is, wordt het systeem automatisch Hiermee markeert u de herstelde primaire als een secundaire en zet het bijgewerkt met de nieuwe primaire. Vanwege de asynchroon aard van herhaling, een kleine hoeveelheid gegevens mogelijk verloren tijdens ongeplande failovers als een primaire mislukt voordat deze wordt overgenomen door de meest recente wijzigingen aan de secundaire. Wanneer een primaire met meerdere ontvangen overgenomen wordt, wordt het systeem automatisch de relaties herhaling geconfigureerd en de resterende secundaire servers voor koppelingen naar de zojuist gepromoveerde primaire zonder tussenkomst van de gebruiker. Nadat de storing waardoor de overname beperkt, is het mogelijk wenselijk om terug te keren van de toepassing de primaire regio. Om dat te doen, moet de opdracht failover worden aangeroepen met de optie "geplande". 

- **Referenties en firewallregels gesynchroniseerd houden**: het is raadzaam [database firewallregels](sql-database-firewall-configure.md) gebruiken voor geografische gerepliceerd databases zodat deze regels kan worden gerepliceerd met de database, zodat alle secundaire databases hebben dezelfde firewallregels als de primaire. Deze methode hoeven klanten handmatig configureren en onderhouden firewallregels op servers hostingprovider zowel de primaire en secundaire-databases. Daarnaast dat [deel uit van de databasegebruikers](sql-database-manage-logins.md) te gebruiken voor toegang tot gegevens zorgt ervoor dat zowel primaire als secundaire databases hebt altijd dezelfde gebruiker referenties tijdens een een overname, er is geen onderbrekingen vanwege problemen met aanmeldingen en wachtwoorden opnieuw instellen. Met het toevoegen van [Azure Active Directory](../active-directory/active-directory-whatis.md), kunnen klanten gebruikerstoegang tot zowel primaire en secundaire databases en hoeft u voor het beheren van referenties in databases helemaal beheren.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgraden of een downgrade uitvoeren van een primaire-database
U kunt upgraden of een primaire-database naar een ander prestatieniveau (binnen de hetzelfde niveau van de service) downgraden zonder te verbreken een secundaire databases. Wanneer u een upgrade uitvoert, wordt aangeraden die u eerst de secundaire database upgraden en vervolgens de primaire bij te werken. Wanneer een downgrade uitvoeren, de volgorde: de primaire eerst downgraden en klikt u vervolgens de secundaire downgraden. 

De secundaire database moet zich in hetzelfde niveau als de primaire, service, zodat u de koppeling van de geografische herhaling beëindigen en de naam van de secundaire database wijzigen of u gewoon uw primaire database migreren naar een andere servicelaag is vereist. Vervolgens de primaire migreren naar de nieuwe servicelaag en configureren, geografische-herhaling. Uw nieuwe secundair wordt automatisch gemaakt met hetzelfde prestatieniveau als de primaire al dan niet standaard.

## <a name="preventing-the-loss-of-critical-data"></a>Het kritieke gegevensverlies voorkomen
Continue kopiëren gebruikt vanwege de hoge latentie van WAN-netwerken te gebruiken, een om asynchroon herhaling. Asynchroon replicatie wordt gegevensverlies onvermijdelijke als er een fout optreedt. Sommige toepassingen mogelijk echter geen gegevens verloren gaan. Als u wilt beveiligen deze essentiële updates, kunt een ontwikkelaar van toepassingen zodra u de transactie doorvoert de [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) systeem procedure bellen. Bellen **sp_wait_for_database_copy_sync** blokken de thread totdat de transactie die het laatst is vastgelegd is gerepliceerd naar de secundaire database. De procedure wacht totdat alle in de wachtrij transacties hebben bevestigd door de secundaire database. **sp_wait_for_database_copy_sync** is beperkt tot de koppeling van een specifieke continue kopiëren. Elke gebruiker met de rechten van de verbinding met de primaire-database kunt deze procedure bellen.

>[AZURE.NOTE] De vertraging die worden veroorzaakt door een procedureaanroep **sp_wait_for_database_copy_sync** mag aanzienlijk zijn. De vertraging is afhankelijk van de grootte van de lengte van de log transactie op het moment dat en dit gesprek niet weer totdat de hele logboek worden gerepliceerd. Vermijd het bellen van deze procedure tenzij absoluut nodig.

## <a name="programmatically-managing-active-geo-replication"></a>Actieve geografische-replicatie beheren via programmacode

Zoals eerder besproken, kan actieve geografische-herhaling ook worden beheerd via een programma via Azure PowerShell en de REST API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

- **Azure resourcemanager API en rollen gebaseerde beveiliging**: actieve geografische-replicatie omvat een reeks [Azure resourcemanager-API's]( https://msdn.microsoft.com/library/azure/mt163571.aspx) voor beheer, met inbegrip van [Azure resourcemanager gebaseerde PowerShell-cmdlets](sql-database-geo-replication-powershell.md). Deze API's vereisen van het gebruik van resourcegroepen en ondersteuning voor rollen gebaseerde beveiliging (RBAC). Rollen Zie [Azure Role-Based toegangsbeheer](../active-directory/role-based-access-control-configure.md)voor meer informatie over het implementeren van access.

>[AZURE.NOTE] Veel nieuwe functies van actieve geografische-replicatie worden alleen ondersteund voor het gebruik van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) op basis van [Azure SQL REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) en [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx). De REST API (klassieke)] (https://msdn.microsoft.com/library/azure/dn505719.aspx) en [Azure SQL-Database (klassieke) cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) worden ondersteund voor compatibiliteit met eerdere versies, zodat met de API resourcemanager Azure-programma's worden aanbevolen. 


### <a name="transact-sql"></a>Transact-SQL

|Opdracht|Beschrijving|
|-------|-----------|
|[ALTER DATABASE (Azure SQL-Database)](https://msdn.microsoft.com/library/mt574871.aspx)|SECUNDAIRE op SERVER toevoegen argument gebruiken om een secundaire database voor een bestaande database en begint gegevensreplicatie te maken|
|[ALTER DATABASE (Azure SQL-Database)](https://msdn.microsoft.com/library/mt574871.aspx)|FAILOVER of FORCE_FAILOVER_ALLOW_DATA_LOSS gebruiken om te schakelen van een secundaire database om te worden primaire failover starten
|[ALTER DATABASE (Azure SQL-Database)](https://msdn.microsoft.com/library/mt574871.aspx)|Gebruik SERVER secundaire op verwijderen om te beëindigen een gegevensreplicatie tussen een SQL-Database en de opgegeven secundaire database.|
|[sys.geo_replication_links (Azure SQL-Database)](https://msdn.microsoft.com/library/mt575501.aspx)|Geeft als resultaat informatie over alle bestaande herhaling koppelingen voor elke database op de logische Azure SQL Database-server.|
|[sys.dm_geo_replication_link_status (Azure SQL-Database)](https://msdn.microsoft.com/library/mt575504.aspx)|Hiermee haalt de laatste keer dat replicatie-, laatste herhaling vertragings- en andere informatie over de replicatiekoppeling voor een bepaald SQL-database.|
|[sys.dm_operation_status (Azure SQL-Database)](https://msdn.microsoft.com/library/dn270022.aspx)|Ziet u de status van alle databasebewerkingen, waaronder de status van de koppelingen herhaling.|
|[sp_wait_for_database_copy_sync (Azure SQL-Database)](https://msdn.microsoft.com/library/dn467644.aspx)|wordt de toepassing te wachten totdat alle vastgelegde transacties worden gerepliceerd en bevestigd door de actieve secundaire database.|
||||

### <a name="powershell"></a>PowerShell

|Cmdlet|Beschrijving|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Krijgt een of meer databases.|
|[Nieuwe AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Hiermee maakt u een secundaire database voor een bestaande database en wordt begonnen repliceren.|
|[Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Hiermee schakelt u een secundaire database om te worden primaire failover starten.|
|[Verwijderen AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Beëindigt repliceren tussen een SQL-Database en de opgegeven secundaire database.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Krijgt de geografische herhaling koppelingen tussen een Azure SQL-Database en een resourcegroep of SQL Server.|
||||

### <a name="rest-api"></a>REST API

|API|Beschrijving|
|---|-----------|
|[Maken of bijwerken-Database (createMode = terugzetten)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Hiermee maakt u, bijwerken of herstelt u een primaire of een secundaire database.|
|[Get maken of de databasestatus van de bijwerken](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Geeft als resultaat de status tijdens een bewerking maken.|
|[Secundaire Database instellen als primair (geplande uitvalbeveiliging)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Een secundaire database in een samenwerkingsverband geografische herhaling om de hoofddatabase van nieuwe vertrouwd te verhogen.|
|[Secundaire Database instellen als primair (niet-geplande Failover)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Een failover omzetten in de secundaire database en de secundaire instellen als de primaire.|
|[Replicatiekoppelingen ophalen](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Hiermee haalt alle herhaling koppelingen voor een opgegeven SQL-database in een samenwerkingsverband geografische-herhaling. Hiermee haalt u de gegevens in de weergave van de catalogus sys.geo_replication_links zichtbaar.|
|[Replicatiekoppeling ophalen](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Hiermee haalt een specifieke replicatiekoppeling voor een opgegeven SQL-database in een samenwerkingsverband geografische-herhaling. Hiermee haalt u de gegevens in de weergave van de catalogus sys.geo_replication_links zichtbaar.|
||||



## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Zie meer informatie over back-ups van Azure SQL-Database wordt automatisch, [automatisch back-ups in SQL-Database](sql-database-automated-backups.md).
- Voor meer informatie over het gebruik van automatische back-ups voor herstel, Zie [herstellen van een database van back-ups van de service is gestart](sql-database-recovery-using-backups.md).
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md).
- Zie voor meer informatie over verificatievereisten voor voor een nieuwe primaire server en database, [beveiliging van de SQL-Database na herstel](sql-database-geo-replication-security-config.md).
