<properties
    pageTitle="Een regel voor het niveau van de server firewall SQL-Database configureren | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang Azure SQL server tot."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Portal Azure configureren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)

Azure SQL-server gebruikt firewallregels toe te staan dat verbindingen met de servers en databases. U kunt de server en database-niveau firewallinstellingen voor het diamodel of de gebruikersdatabase van een in de logische Azure SQL server-server selectief toestaan tot de database definiëren. In dit onderwerp wordt beschreven hoe serverniveau firewallregels.

> [AZURE.IMPORTANT] Als u wilt toestaan dat toepassingen van Azure verbinding maken met uw Azure SQL-server, moeten Azure verbindingen zijn ingeschakeld. Zie informatie over hoe de firewallregels werk [het configureren van een Azure SQL server-firewall \- overzicht](sql-database-firewall-configure.md). Als u verbindingen binnen de grens Azure cloud maakt, moet u mogelijk enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** gedeelte van [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)

**Aanbeveling:** Gebruik serverniveau firewallregels voor beheerders en wanneer u veel databases die dezelfde toegang eisen hebt hebt en u niet wilt dat elke database afzonderlijk configureren tijd. Microsoft raadt database niveau firewallregels indien mogelijk, ter verbetering van beveiliging en kunt u de database meer portable.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Bestaande serverniveau firewallregels tot en met de Azure-portal beheren

Herhaal de stappen voor het beheren van de serverniveau firewallregels.

- Als u wilt toevoegen op de huidige computer, klikt u op client-IP-toevoegen.
- Als u wilt toevoegen van extra IP-adressen, typt u in de regelnaam, IP-adres begin en einde IP-adres.
- Een bestaande regel wijzigen, klikt u op een van de velden in de regel en wijzigen.
- Als u wilt verwijderen van een bestaande regel, plaats de muisaanwijzer op de regel totdat de X aan het einde van de rij wordt weergegeven. Klik op de X om de regel te verwijderen.

Klik op **Opslaan** als de wijzigingen wilt opslaan.

## <a name="next-steps"></a>Volgende stappen

Zie voor een het artikel over het gebruik van Transact-SQL server en database-niveau firewallregels maken, [Azure SQL-Database configureren server en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Voor hoe naar artikelen over het maken van serverniveau firewallregels via andere methoden, leest u de: 

- [Azure SQL-Database serverniveau firewallregels configureren via PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL-Database serverniveau firewallregels configureren met de REST API](sql-database-configure-firewall-settings-rest.md)

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

 
