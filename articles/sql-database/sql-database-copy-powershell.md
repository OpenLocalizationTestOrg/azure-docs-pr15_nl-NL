<properties 
    pageTitle="Kopieer een Azure SQL-database via PowerShell | Microsoft Azure" 
    description="Kopie van een Azure SQL-database via PowerShell maken" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopieer een Azure SQL-database via PowerShell


> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

In dit artikel leest hoe u een SQL-database met PowerShell kopiëren naar dezelfde server op een andere server, of een database kopiëren naar een [groep elastische database](sql-database-elastic-pool.md). De kopieeropdracht database wordt de cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Als u wilt voltooien in dit artikel, moet u het volgende:

- Een Azure SQL-database (een database kopiëren). Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).
- De meest recente versie van Azure PowerShell. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.


Veel nieuwe functies van de SQL-Database worden alleen ondersteund wanneer u het [Azure resourcemanager implementatiemodel](../azure-resource-manager/resource-group-overview.md), gebruikt zodat voorbeelden de [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) voor de Resource-Manager gebruiken. Het bestaande klassieke implementatiemodel [(klassieke) Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) voor compatibiliteit met eerdere versies worden ondersteund, maar we raden u aan om de cmdlets resourcemanager.


>[AZURE.NOTE] Afhankelijk van de grootte van uw database duurt de kopie enige tijd om te voltooien.


## <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database naar dezelfde server kopiëren

Als u wilt de kopie maken op dezelfde server, weglaat de `-CopyServerName` parameter (of stel deze in op dezelfde server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database naar een andere server kopiëren

Als u wilt de kopie maken op een andere server, bevatten de `-CopyServerName` parameter en stelt u deze naar een andere server. De *kopie* -server moet al bestaan. Als deze in een andere resource-groep, dan moet u ook de `-CopyResourceGroupName` parameter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopieer een SQL-database naar een elastische database-toepassingen

Als u een kopie van een SQL-database in een groep, stelt u de `-ElasticPoolName` -parameter voor een bestaande toepassingen.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Aanmeldingen oplossen

Om op te lossen aanmeldingen nadat de kopieerbewerking is voltooid, raadpleegt u [aanmeldingen oplossen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Voorbeeld van de PowerShell-script

Het volgende script wordt ervan uitgegaan alle resourcegroepen-servers en de groep reeds bestaande (de waarden van variabelen vervangen door uw bestaande resources). Alles moet bestaan, behalve voor de databasekopie.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Volgende stappen

- Zie [kopiëren van een Azure SQL-database](sql-database-copy.md) voor een overzicht van het kopiëren van een Azure SQL-Database.
- Zie [een Azure SQL-database met behulp van de Azure portal kopiëren](sql-database-copy-portal.md) naar het kopiëren van een database met behulp van de Azure-portal.
- Zie [kopiëren een Azure SQL-database T-SQL gebruiken](sql-database-copy-transact-sql.md) om te kopiëren van een database met behulp van Transact-SQL.
- Lees [hoe u de beveiliging van de Azure SQL-database na herstel beheren](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.


## <a name="additional-resources"></a>Aanvullende informatie

- [Nieuwe AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL Database PowerShell-Cmdlet-verwijzing](https://msdn.microsoft.com/library/mt574084.aspx)
