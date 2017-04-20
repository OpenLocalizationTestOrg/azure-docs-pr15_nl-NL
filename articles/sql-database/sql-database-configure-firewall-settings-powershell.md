<properties
    pageTitle="Azure SQL-Database serverniveau firewallregels configureren via PowerShell | Microsoft Azure"
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


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Azure SQL-Database serverniveau firewallregels configureren via PowerShell


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Azure SQL-Database gebruikt firewallregels toe te staan dat verbindingen met de servers en databases. U kunt de server en database-niveau firewallinstellingen voor de hoofddatabase of de gebruikersdatabase van een in uw SQL-databaseserver op selectief toegang geeft tot de database definiëren.

> [AZURE.IMPORTANT] Als u wilt toestaan dat toepassingen van Azure verbinding maken met uw database-server, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over firewallregels en inschakelen verbindingen van Azure [Azure SQL Database-Firewall](sql-database-firewall-configure.md). Als u verbindingen binnen de grens Azure cloud maakt, moet u mogelijk enkele extra TCP-poorten. Zie voor meer informatie de "V12 van SQL-Database: buiten de vs binnen" sectie van [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Server firewallregels maken

Serverniveau firewall regels kunnen worden gemaakt, bijgewerkt en verwijderd met behulp van Azure PowerShell.

Een nieuwe serverniveau als firewallregel wilt maken, uitvoeren de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt een bereik van IP-adressen op de server Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Als u wilt wijzigen van een bestaande serverniveau firewallregel, uitvoeren de [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt het bereik van aanvaardbaar IP-adressen voor de regel met de naam ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Als u wilt verwijderen van een bestaande serverniveau firewallregel, uitvoeren de [verwijderen-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Firewallregels beheren met behulp van PowerShell

U kunt ook PowerShell gebruiken voor het beheren van firewallregels. Zie de volgende onderwerpen voor meer informatie:

* [Nieuwe AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Verwijderen-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Volgende stappen

Transact-SQL server en database-niveau firewallregels, maken Zie voor informatie over het gebruik van [Azure SQL-Database configureren server en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md).

Zie voor informatie over het maken van serverniveau firewallregels andere methoden gebruiken:

- [Azure SQL-Database serverniveau firewallregels configureren met behulp van de Azure portal](sql-database-configure-firewall-settings.md)
- [Azure SQL-Database serverniveau firewallregels configureren met de REST API](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database, [maken een SQL-database in minuten met behulp van de Azure portal](sql-database-get-started.md).
Voor hulp bij het verbinding maakt met een Azure SQL-database in de bron openen of toepassingen van derden, Zie [Client snel - codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u gaat aan databases, raadpleegt u [toegang en login databasebeveiliging beheren](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende informatie

- [Beveiliging van uw database](sql-database-security.md)
- [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
