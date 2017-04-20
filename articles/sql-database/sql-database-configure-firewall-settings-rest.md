<properties
    pageTitle="Azure SQL-Database serverniveau firewallregels met de REST API | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang de SQL Azure-databases tot."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/09/2016"
    ms.author="sstein"/>


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Azure SQL-Database serverniveau firewallregels configureren met de REST API


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database gebruikt firewallregels toe te staan dat verbindingen met de servers en databases. U kunt de server en database-niveau firewallinstellingen voor het diamodel of de gebruikersdatabase van een in uw Azure SQL-databaseserver op selectief toegang geeft tot de database definiëren.

> [AZURE.IMPORTANT] Als u wilt toestaan dat toepassingen van Azure verbinding maken met uw database-server, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over firewallregels en inschakelen verbindingen van Azure [Azure SQL Database-Firewall](sql-database-firewall-configure.md). Als u verbindingen binnen de grens Azure cloud maakt, moet u mogelijk enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** gedeelte van [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Serverniveau firewallregels tot en met REST API beheren
1. Beheer van firewallregels tot en met REST API moet worden geverifieerd. Zie de [handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](../resource-manager-api-authentication.md)voor informatie.
2. Serverniveau regels kunnen worden gemaakt, bijgewerkt of verwijderd REST API gebruiken

    Om te maken of bijwerken van een firewallregel serverniveau, voert u de opslag-methode voor het gebruik van de volgende handelingen uit:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Hoofdtekst aanvragen

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Als u wilt verwijderen van een bestaande serverniveau firewallregel, uitvoeren met de methode DELETE met het volgende:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Gebruik van de REST API firewallregels beheren

* [Maken of bijwerken van de firewallregel](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Firewallregel verwijderen](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Firewallregel ophalen](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Lijst van alle firewallregels voor de](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Volgende stappen

Zie voor een het artikel over het gebruik van Transact-SQL server en database-niveau firewallregels maken, [Azure SQL-Database configureren server en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Voor hoe naar artikelen over het maken van serverniveau firewallregels via andere methoden, leest u de: 

- [Azure SQL-Database serverniveau firewallregels configureren met behulp van de Azure portal](sql-database-configure-firewall-settings.md)
- [Azure SQL-Database serverniveau firewallregels configureren via PowerShell](sql-database-configure-firewall-settings-powershell.md)

Zie voor een zelfstudie over het maken van een database, [maken een SQL-database in minuten met behulp van de Azure portal](sql-database-get-started.md).
Als u hulp nodig hebt in verbinding maakt met een Azure SQL-database in de bron openen of toepassingen van derden, Zie [Client snel - codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u gaat aan databases, raadpleegt u [toegang en login databasebeveiliging beheren](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende informatie

- [Beveiliging van uw database](sql-database-security.md)
- [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 
