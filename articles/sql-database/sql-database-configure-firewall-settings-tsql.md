<properties
    pageTitle="Azure SQL Database-server en database-niveau firewallregels met T-SQL | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang de SQL Azure-databases tot."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Azure SQL Database-server en database-niveau firewallregels met T-SQL configureren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database gebruikt firewallregels toe te staan dat verbindingen met de servers en databases. U kunt de server en database-niveau firewallinstellingen voor het diamodel of de gebruikersdatabase van een in uw Azure SQL-databaseserver op selectief toegang geeft tot de database definiëren.

> [AZURE.IMPORTANT] Als u wilt toestaan dat toepassingen van Azure verbinding maken met uw database-server, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over firewallregels en inschakelen verbindingen van Azure [Azure SQL Database-Firewall](sql-database-firewall-configure.md). Als u verbindingen binnen de grens Azure cloud maakt, moet u mogelijk enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** gedeelte van [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Serverniveau firewallregels

Alleen de hoofdsom login serverniveau of Azure Active Directory-beheerder kunt u een firewallregel serverniveau maken met behulp van Transact-SQL.

1. Een queryvenster starten en verbinding maken met de virtuele hoofddatabase met behulp van SQL Server Management Studio.
2. Serverniveau firewallregels kunnen worden geselecteerd, die zijn gemaakt, bijgewerkt of verwijderd uit in het queryvenster.
3. Als u wilt maken of bijwerken serverniveau firewallregels, uitvoeren de `sp_set_firewall_rule` opgeslagen procedure. Het volgende voorbeeld wordt een bereik van IP-adressen op de server Contoso.<br/>Begin door te kijken welke regels al aanwezig.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Voeg nu een firewallregel.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Een firewallregel serverniveau verwijderen, voert u de sp_delete_firewall_rule opgeslagen procedure. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Zie voor meer informatie over deze opgeslagen procedures, [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) en [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Database-niveau firewallregels

Alleen een databasegebruiker met de machtiging **beheren** in de database (zoals de eigenaar van de database) kunt een database op gebruikersniveau firewallregel maken.

1. Na het maken van een firewall serverniveau voor uw IP-adres, start u een queryvenster via de portal klassieke of SQL Server Management Studio.
2. Verbinding maken met de database waarvan u wilt een database op gebruikersniveau firewallregel maken.

    Uitvoeren als u wilt een nieuwe maken of bijwerken van een bestaande database op gebruikersniveau firewallregel, de `sp_set_database_firewall_rule` opgeslagen procedure. Het volgende voorbeeld wordt een nieuwe firewallregel met de naam ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Uitvoeren als u wilt verwijderen van een bestaande database op gebruikersniveau firewallregel, de `sp_delete_database_firewall_rule` opgeslagen procedure. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.
`
   Raad sp_delete_database_firewall_rule @name N'ContosoFirewallRule ='

Zie voor meer informatie over deze opgeslagen procedures, [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) en [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Volgende stappen

Voor hoe naar artikelen over het maken van serverniveau firewallregels via andere methoden, leest u de: 

- [Azure SQL-Database serverniveau firewallregels configureren met behulp van de Azure-Portal](sql-database-configure-firewall-settings.md)
- [Azure SQL-Database serverniveau firewallregels configureren via PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL-Database serverniveau firewallregels configureren met de REST API](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database, [maken een SQL-database in minuten met behulp van de Azure portal](sql-database-get-started.md).
Als u hulp nodig hebt in verbinding maakt met een Azure SQL-database in de bron openen of toepassingen van derden, Zie [Client snel - codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u gaat aan databases, raadpleegt u [toegang en login databasebeveiliging beheren](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende informatie

- [Beveiliging van uw database](sql-database-security.md)
- [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)
