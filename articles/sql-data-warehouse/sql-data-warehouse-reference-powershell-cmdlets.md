<properties
   pageTitle="PowerShell-cmdlets voor Azure SQL Data Warehouse"
   description="De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse waaronder over het onderbreken en hervatten van een database te vinden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdlets en REST API's voor SQL Data Warehouse

Veel SQL Data Warehouse beheerderstaken kunnen worden beheerd via Azure PowerShell-cmdlets of REST API's.  Hieronder vindt u enkele voorbeelden van hoe u algemene taken in uw SQL Data Warehouse automatiseren met PowerShell-opdrachten.  Zie het artikel [beheren schaalbaarheid met REST][]voor enkele voorbeelden van goede REST.

> [AZURE.NOTE]  Pas Azure PowerShell met SQL Data Warehouse gebruiken, moet u Azure PowerShell versie 1.0.3 of hoger.  U kunt uw versie controleren door te voeren **Get-Module - ListAvailable-naam Azure**.  De meest recente versie kan worden geïnstalleerd van [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de meest recente versie [installeren en configureren van Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets

1. Open Windows PowerShell. 
2. Klik bij de PowerShell-prompt, moet u deze opdrachten u zich aanmeldt bij de Azure Resource Manager en selecteert u uw abonnement uitvoeren.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Voorbeeld van een pauze invoegen SQL Data Warehouse

Plaats de aanwijzer een database met de naam 'Database02' die worden gehost op een server met de naam "Server01."  De server onderdeel is in een Azure resourcegroep met de naam "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Een variatie, in dit voorbeeld buizen de opgehaalde object [Onderbreking-AzureRmSqlDatabase][].  Hierdoor wordt de database is onderbroken. De opdracht definitief geeft de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Voorbeeld van SQL Data Warehouse starten

Cv-bewerking van een database met de naam 'Database02' die worden gehost op een server met de naam "Server01." De server deel uitmaakt van een resourcegroep met de naam "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Een variatie, in dit voorbeeld wordt opgehaald een database met de naam 'Database02' vanaf een server met de naam 'Server01"die deel uitmaakt van een resourcegroep met de naam"ResourceGroup1." Deze buizen de opgehaalde object [Cv-AzureRmSqlDatabase][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Er gebeurt als de server onderdeel is foo.database.windows.net, "foo" Als de servernaam - in de PowerShell-cmdlets gebruiken.

## <a name="frequently-used-powershell-cmdlets"></a>Vaak gebruikte PowerShell-cmdlets

Deze PowerShell-cmdlets worden vaak gebruikt met Azure SQL Data Warehouse.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Nieuwe AzureRmSqlDatabase][]
- [Verwijderen AzureRmSqlDatabase][]
- [Herstellen-AzureRmSqlDatabase][] 
- [Cv-AzureRmSqlDatabase][]
- [Selecteer AzureRmSubscription][]
- [Set-AzureRmSqlDatabase][]
- [Schorten AzureRmSqlDatabase][]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer PowerShell voorbeelden:

- [Een SQL-Data Warehouse via PowerShell maken][]
- [Database terugzetten][]

Zie voor een lijst met alle taken die kunnen worden geautomatiseerd met PowerShell, [Azure SQL Database-Cmdlets][].  Zie voor een lijst met taken die kunnen worden geautomatiseerd met REST, [bewerkingen voor Azure SQL-Databases][].

<!--Image references-->

<!--Article references-->
[Het installeren en configureren van Azure PowerShell]: ./powershell-install-configure.md
[Een SQL-Data Warehouse via PowerShell maken]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database terugzetten]: ./sql-data-warehouse-restore-database-powershell.md
[Schaalbaarheid met REST beheren]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database-Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Bewerkingen voor Azure SQL-Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Nieuwe AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Verwijderen AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Herstellen-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Cv-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Selecteer AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Schorten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
