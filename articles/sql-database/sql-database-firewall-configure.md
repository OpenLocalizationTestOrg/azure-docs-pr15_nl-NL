<properties
   pageTitle="Een overzicht van SQL server firewall configureren | Microsoft Azure"
   description="Informatie over het configureren van een firewall SQL-database met de server en database-niveau firewallregels voor het beheren van access."
   keywords="database-firewall"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Azure SQL Database-firewallregels configureren \- overzicht


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database biedt een relationele database-service voor Azure en andere toepassingen op basis van Internet. Ter bescherming van uw gegevens, firewalls voorkomen dat alle access de database-server totdat u opgeven welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het oorspronkelijke IP-adres van elke aanvraag kunt invullen.

Als u wilt uw firewall configureren, moet u firewallregels die aanvaardbaar IP-adresbereiken opgeven maken. U kunt firewallregels maken op de server en database niveaus.

- **Serverniveau firewallregels:** Deze regels kunnen clients voor toegang tot uw hele Azure SQL-server, dat wil zeggen alle databases binnen dezelfde logische-server. Deze regels worden opgeslagen in **de hoofddatabase** . Serverniveau firewallregels kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies.
- **Database niveau firewallregels:** Deze regels kunnen clients voor toegang tot afzonderlijke databases binnen uw Azure SQL Database-server. U kunt deze regels voor elke database maken en ze zijn opgeslagen in de afzonderlijke databases. (U kunt de database op gebruikersniveau firewallregels voor **de hoofddatabase** kunt maken.) Deze regels is handig in het beperken van toegang aan bepaalde (veilig) databases binnen dezelfde logische-server. Database-niveau firewallregels kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies.

**Aanbeveling:** Microsoft raadt database niveau firewallregels indien mogelijk om beveiliging te verbeteren en uw database meer om geschikt te maken. Gebruik serverniveau firewallregels voor beheerders en wanneer er veel databases die dezelfde toegang eisen hebt en u niet wilt tijd hebt elke database afzonderlijk configureren.


## <a name="firewall-overview"></a>Overzicht van de firewall

Alle Transact-SQL-toegang tot uw Azure SQL-server is in eerste instantie door de firewall geblokkeerd. Als u wilt beginnen met uw Azure SQL-server, moet u gaat u naar de portal van Azure en een of meer serverniveau firewallregels die u toegang tot uw Azure SQL-server opgeven. Met de firewallregels kunt u opgeven welke IP-adresbereiken van Internet zijn toegestaan en of Azure-toepassingen kunnen probeert te verbinden met uw Azure SQL-server.

Selectief om toegang te verlenen tot slechts één van de databases in uw Azure SQL-server, moet u een database op gebruikersniveau regel voor de vereiste database maken. Geef een IP-adresbereiken voor de regel van de database-firewall die valt buiten het IP-adresbereiken die is opgegeven in de firewallregel serverniveau en zorg ervoor dat het IP-adres van de client in het bereik dat is opgegeven in de regel van de database op gebruikersniveau valt.

Verbindingspogingen met Internet en Azure eerst verloopt via de firewall voordat ze uw Azure SQL server- of SQL-Database bereikt, zoals wordt weergegeven in het volgende diagram.

   ![Diagram met een beschrijving van firewallconfiguratie.][1]

## <a name="connecting-from-the-internet"></a>Verbinding maken van Internet

Wanneer een computer probeert te verbinden met uw databaseserver van Internet, controleert de firewall eerst het oorspronkelijke IP-adres van de aanvraag ten opzichte van de volledige reeks firewallregels:

- Als het IP-adres van de aanvraag zich in een van de bereiken in de serverniveau firewallregels, wordt de verbinding met uw Azure SQL-databaseserver verleend.

- Als het IP-adres van de aanvraag zich niet in een van de bereiken in de firewallregel serverniveau, wordt de database op gebruikersniveau firewallregels worden gecontroleerd. Als het IP-adres van de aanvraag zich in een van de bereiken in de database op gebruikersniveau firewallregels, wordt de verbinding alleen voor de database die een overeenkomende regel van de database op gebruikersniveau heeft verleend.

- Als het IP-adres van de aanvraag niet binnen de bereiken in een van de server- of database-niveau firewallregels, de verbinding verzoek werkt niet is.

> [AZURE.NOTE] Controleer of dat de firewall op uw netwerk en de lokale computer maakt uitgaande communicatie op TCP-poort 1433 voor toegang tot Azure SQL-Database vanaf de lokale computer.


## <a name="connecting-from-azure"></a>Verbinding maken van Azure

Wanneer een toepassing van Azure probeert te verbinden met uw database-server, wordt de firewall gecontroleerd dat Azure verbindingen zijn toegestaan. Een firewall instellen met de begin- en einddatum gelijk is aan 0.0.0.0 adres geeft aan dat deze verbindingen zijn toegestaan. Als de poging niet is toegestaan, het verzoek niet de Azure SQL Database-server heeft bereikt.

> [AZURE.IMPORTANT] Deze optie wordt de firewall voor alle verbindingen van Azure inclusief verbindingen vanuit de abonnementen van andere klanten geconfigureerd. Wanneer u deze optie selecteert, zorg ervoor dat uw gebruikersnaam en gebruikersmachtigingen beperken tot alleen bevoegde gebruikers.

U kunt verbindingen van Azure op twee manieren inschakelen:

- Selecteer in de [portal van Microsoft Azure](https://portal.azure.com/), het selectievakje **toestaan dat azure services toegangsserver** wanneer u een nieuwe server.

- Klik in de [klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=161793)vanaf het tabblad **configureren** op een server, onder de sectie **Toegestane Services** en klik op **Ja** voor **Microsoft Azure-Services**.

## <a name="creating-the-first-server-level-firewall-rule"></a>De eerste firewallregel serverniveau maken

De eerste serverniveau firewallinstelling kan worden gemaakt met behulp van de [Azure-portal](https://portal.azure.com/) of via een programma via de REST API of Azure PowerShell. Volgende serverniveau firewallregels kan worden gemaakt en beheerd met behulp van deze methoden, ook als bij Transact-SQL. Prestaties verbeteren serverniveau firewallregels tijdelijk worden opgeslagen op het databaseniveau van de. Als u wilt de cache vernieuwen, raadpleegt u [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Zie voor meer informatie over serverniveau firewallregels, [hoe: een Azure SQL server-firewall met behulp van de Azure portal configureren](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Database-niveau firewallregels maken

Nadat u de eerste serverniveau firewall hebt geconfigureerd, wilt u mogelijk het beperken van toegang aan bepaalde databases. Als u een IP-adresbereiken in de database op gebruikersniveau firewallregel die zich buiten het bereik dat is opgegeven in de firewallregel serverniveau opgeeft, kunnen alleen deze clients die IP-adressen in het bereik van de database op gebruikersniveau toegang tot de database. U kunt maximaal 128 database niveau firewallregels voor een database hebben. Database-niveau firewallregels voor databases diamodel en de gebruiker kunnen worden gemaakt en beheerd via Transact-SQL. Zie voor meer informatie over het configureren van de database op gebruikersniveau firewallregels [sp_set_database_firewall_rule (Azure SQL-Databases)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Via programmacode beheren van firewallregels voor de

Naast de Azure-portal kunnen firewallregels worden beheerd via een programma via Transact-SQL, REST API en Azure PowerShell. De volgende tabellen wordt de set opdrachten die beschikbaar zijn voor elke methode beschreven.


### <a name="transact-sql"></a>Transact-SQL

| Catalogusweergave of de opgeslagen Procedure                                                           | Niveau     | Beschrijving                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Geeft de huidige serverniveau firewallregels     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Gemaakt of bijgewerkt serverniveau firewallregels       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Hiermee verwijdert u serverniveau firewallregels                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Database  | Geeft de huidige database niveau firewallregels   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Database  | Hiermee maakt u instellen of bijwerken van de database op gebruikersniveau firewallregels |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Databases | Hiermee verwijdert u database niveau firewallregels                |

### <a name="rest-api"></a>REST API

| API                  | Niveau  | Beschrijving                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Lijst firewallregels](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Geeft de huidige serverniveau firewallregels                 |
| [Firewallregel maken](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Gemaakt of bijgewerkt serverniveau firewallregels                   |
| [Firewallregel instellen](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | De eigenschappen van een bestaande serverniveau firewallregel wordt bijgewerkt |
| [Firewallregel verwijderen](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Hiermee verwijdert u serverniveau firewallregels                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet                                                                                              | Niveau  | Beschrijving                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Geeft als resultaat de huidige serverniveau firewallregels                  |
| [Nieuwe AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Hiermee maakt u een nieuwe firewallregel voor het niveau van de server                         |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | De eigenschappen van een bestaande serverniveau firewallregel wordt bijgewerkt |
| [Verwijderen AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Hiermee verwijdert u serverniveau firewallregels                              |

> [AZURE.NOTE] Kunnen er omhoog zo groot een vijf minuten uitstellen voor wijzigingen in de firewallinstellingen zijn doorgevoerd.

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de firewall database oplossen

Houd rekening met de volgende punten wanneer u toegang tot de Microsoft Azure SQL Database-service niet meer werkt zoals u zou verwachten:

- **Lokale firewallconfiguratie:** Voordat uw computer Azure SQL-Database openen kunt, moet u mogelijk een firewalluitzondering op uw computer voor TCP-poort 1433 maken. Als u verbindingen binnen de grens Azure cloud maakt, is het wellicht om extra poorten te openen. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** gedeelte van [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).

- **NAT (NAT):** Vanwege NAT, het IP-adres dat is gebruikt door uw computer om te verbinden met Azure SQL-Database mogelijk anders dan het IP-adres dat wordt weergegeven in uw computer IP-configuratie-instellingen. Als u wilt bekijken van het IP-adres dat uw computer wordt gebruikt om verbinding maken met Azure, meld u aan bij de portal en Ga naar het tabblad **configureren** op de server waarop uw database. Klik onder de sectie **Toegestane IP-adressen** wordt het **Huidige IP-adres van Client** weergegeven. Klik op **toevoegen** aan de **Toegestane IP-adressen** toe te staan dat deze computer toegang tot de server.

- **Wijzigingen in de lijst toestaan niet van kracht zijn nog:** Er zijn mogelijk zo groot een vijf minuten uitstellen voor wijzigingen in de configuratie van de firewall Azure SQL-Database zijn doorgevoerd.

- **De aanmelding is niet geautoriseerd of is er een onjuist wachtwoord gebruikt:** Als een aanmelding geen machtigingen op de server Azure SQL-Database of het wachtwoord onjuist is, wordt de verbinding met de Azure SQL Database-server is geweigerd. Maken van een firewallinstelling alleen recht biedt op clients de mogelijkheid om te proberen verbinding maakt met uw server. elke client moet de benodigde beveiligingsreferenties opgeven. Zie voor meer informatie over het voorbereiden van aanmeldingen Databases beheren, aanmeldingen en gebruikers in Azure SQL-Database.

- **Dynamisch IP-adres:** Als u een internetverbinding met dynamische IP-adressen hebt en u problemen ondervindt bij het via de firewall, kunt u een van de volgende oplossingen proberen:

 - Uw Internet Service Provider Internetprovider vragen om het IP-adresbereiken die zijn toegewezen aan uw clientcomputers die toegang de Azure SQL Database-server tot en het IP-adresbereiken vervolgens als een firewallregel toevoegen.

 - Bent u statische IP-adressen in plaats daarvan voor clientcomputers en klikt u vervolgens de IP-adressen als firewallregels toevoegen.

## <a name="next-steps"></a>Volgende stappen

Voor de realisatie naar artikelen over het maken van de server en database-niveau firewallregels, Zie:

- [Azure SQL-Database serverniveau firewallregels configureren met behulp van de Azure portal](sql-database-configure-firewall-settings.md)
- [Azure SQL Database-server en database-niveau firewallregels met T-SQL configureren](sql-database-configure-firewall-settings-tsql.md)
- [Azure SQL-Database serverniveau firewallregels configureren via PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL-Database serverniveau firewallregels configureren met de REST API](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database, [maken een SQL-database in minuten met behulp van de Azure portal](sql-database-get-started.md).
Als u hulp nodig hebt in verbinding maakt met een Azure SQL-database in de bron openen of toepassingen van derden, Zie [Client snel - codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u gaat aan databases, raadpleegt u [toegang en login databasebeveiliging beheren](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Aanvullende informatie

- [Beveiliging van uw database](sql-database-security.md)
- [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
