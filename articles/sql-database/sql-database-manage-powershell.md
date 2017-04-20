<properties
    pageTitle="Azure SQL-Database met PowerShell beheren | Microsoft Azure"
    description="Beheer van de Azure SQL-Database met PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Azure SQL-Database met PowerShell beheren


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

In dit onderwerp ziet u de PowerShell-cmdlets die worden gebruikt om u te veel Azure SQL Database-taken uitvoeren. Zie voor een volledige lijst, [Azure SQL Database-Cmdlets] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken voor onze SQL-Database en verwante Azure resources met de [nieuw-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx)-cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.
Zie [een SQL-database PowerShell-script maken](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)voor een voorbeeldscript.

## <a name="create-a-sql-database-server"></a>Een SQL-databaseserver maken

Een SQL-databaseserver maken met de [nieuw-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx)-cmdlet. *Server1* vervangen door de naam van uw mailserver. Namen van servers voor moet uniek zijn voor alle Azure SQL Database-servers. Als de servernaam al gebruikt wordt, krijgt u een fout. Deze opdracht kan enkele minuten duren om te voltooien. De resourcegroep moet al bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Zie [Wat is SQL-Database](sql-database-technical-overview.md)voor meer informatie. Zie [een SQL-database PowerShell-script maken](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)voor een voorbeeldscript.


## <a name="create-a-sql-database-server-firewall-rule"></a>Een Database van SQL server-firewallregel maken

Maak een firewallregel voor toegang tot de server met de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-cmdlet. Voer de volgende opdracht, worden vervangen door de begin- en IP-adressen met geldige waarden voor de klant. De resourcegroep en server moet al bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Voor andere Azure services toegang tot uw server, maakt u een firewallregel en stelt u beide de `-StartIpAddress` en `-EndIpAddress` op **0.0.0.0**. Deze firewallregel speciale staat alle Azure verkeer voor toegang tot de server.

Zie [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx)voor meer informatie. Zie [een SQL-database PowerShell-script maken](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)voor een voorbeeldscript.


## <a name="create-a-sql-database-blank"></a>Maken van een SQL-database (leeg)

Een database maken met de [nieuw-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx)-cmdlet. De resourcegroep en server moet al bestaan in uw abonnement. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie [Wat is SQL-Database](sql-database-technical-overview.md)voor meer informatie. Zie [een SQL-database PowerShell-script maken](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)voor een voorbeeldscript.


## <a name="change-the-performance-level-of-a-sql-database"></a>Wijzigen van het prestatieniveau van een SQL-database

Schaal van de database omhoog of omlaag met de [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx)-cmdlet. De resourcegroep, server en database moeten al bestaan in uw abonnement. Stel de `-RequestedServiceObjectiveName` in een spatie (zoals de volgende fragment) voor eenvoudige laag. Stel deze in op *S0*, *S1*, *P1*, *P6*, enzovoort, zoals het voorgaande voorbeeld voor andere lagen.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie voor meer informatie [SQL-Database-opties en prestaties: begrijpen wat is beschikbaar in elke servicelaag](sql-database-service-tiers.md). Zie voor een voorbeeldscript [steekproef PowerShell-script de service laag en prestaties het niveau van uw SQL-database wilt wijzigen](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database naar dezelfde server kopiëren

Een SQL-database kopiëren naar dezelfde server met de [nieuw-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx)-cmdlet. Stel de `-CopyServerName` en `-CopyResourceGroupName` op dezelfde waarden als uw bron-database-server- en resourcekalenders-groep.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Zie [een Azure SQL-Database kopiëren](sql-database-copy.md)voor meer informatie. Voor een voorbeeldscript, raadpleegt u [een SQL-database PowerShell-script kopiëren](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Een SQL-database verwijderen

Verwijderen van een SQL-database met de [verwijderen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx)-cmdlet. De resourcegroep, server en database moeten al bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Een SQL-databaseserver verwijderen

Verwijderen van een server met de [verwijderen-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx)-cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Maken en beheren van elastische database van toepassingen via PowerShell

Zie [een nieuwe elastische database-toepassingen met PowerShell maken](sql-database-elastic-pool-create-powershell.md)voor meer informatie over het maken van elastische database van toepassingen via PowerShell.

Zie voor meer informatie over het beheren van elastische database van toepassingen via PowerShell [beeldscherm en beheren van een toepassingen elastische database met PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Gerelateerde informatie

- [Azure SQL Database-Cmdlets] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure-Cmdlet verwijzing] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)
