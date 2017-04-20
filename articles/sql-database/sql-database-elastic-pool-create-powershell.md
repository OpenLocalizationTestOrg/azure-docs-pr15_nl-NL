<properties
    pageTitle="Maak een nieuwe elastische database-toepassingen met PowerShell | Microsoft Azure"
    description="Leer hoe u PowerShell gebruiken om te schalen Azure SQL-Database resources door te maken van een resourcegroep die scalable elastische database voor het beheren van meerdere databases."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Een nieuwe elastische database-toepassingen maken met PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Informatie over het maken van een [groep elastische database](sql-database-elastic-pool.md) met PowerShell-cmdlets. 

Zie voor algemene foutcodes, [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastische toepassingen zijn algemeen beschikbaar (GA) in alle Azure regio's behalve Noord centraal ons en West India waar deze momenteel in de proefversie is.  GA van elastische toepassingen in deze regio's krijgt zo snel mogelijk. Bovendien ondersteunen elastische van toepassingen niet momenteel databases met [in het geheugen OLTP of in het geheugen analytics](sql-database-in-memory.md).


U moet werken Azure PowerShell 1,0 of hoger. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.

## <a name="create-a-new-pool"></a>Een nieuwe groep maken

De cmdlet [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) Hiermee maakt u een nieuwe groep. De waarden voor eDTU per groep, min en max Dtus worden beperkt door de waarde van de laag service (basic, standard of premium). Zie [eDTU en opslagruimte bestandsgrootten voor elastische pools en elastische databases](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Een nieuwe elastische database in een groep maken

Gebruik de cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) en stel de parameter **ElasticPoolName** op de doel-toepassingen. Als u wilt een bestaande database verplaatsen naar een groep, raadpleegt u [een database naar een elastische groep verplaatsen](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Een groep maken en deze te vullen met meerdere nieuwe databases 

Het maken van een groot aantal databases in een groep kan duren als klaar met via de portal of de PowerShell-cmdlets die slechts één database tegelijk maken. Als u wilt maken naar een nieuwe groep automatiseren, raadpleegt u [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Voorbeeld: Maak een groep via PowerShell 

Dit script Hiermee maakt u een nieuwe Azure resourcegroep en een nieuwe server. Wanneer u wordt gevraagd, een beheerdersgebruikersnaam en wachtwoord voor de nieuwe server (niet uw Azure referenties) opgeven.

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Volgende stappen

- [Uw groep beheren](sql-database-elastic-pool-manage-powershell.md)
- [Elastische taken maken](sql-database-elastic-jobs-overview.md) Elastische taken kunnen u T-SQL-scripts uitvoeren op een willekeurig aantal databases in de groep.
- [Schalen af met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen naar schalen gebruiken.

