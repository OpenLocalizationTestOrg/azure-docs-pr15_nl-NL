<properties
   pageTitle="Beheren rekenkracht in Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="PowerShell-taken voor het beheren van rekenkracht. Schaal berekenen resources DWUs aanpassen. Of, onderbreken en hervatten resources om op te slaan kosten berekenen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Rekenkracht in Azure SQL Data Warehouse (PowerShell) beheren

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties van de schaal door schalen berekenen bronnen en het geheugen om te voldoen aan de veranderende vereisten van uw werkzaamheden. Bespaar kosten door schaal achterste resources tijdens de niet-tijden of onderbreken berekeningscluster helemaal af. 

Deze verzameling van taken wordt gebruikt voor de Azure-portal naar:

- Schaal berekenen
- Een pauze invoegen berekenen
- Cv berekenen

Zie meer informatie over dit, [beheren berekenen overzicht][].


## <a name="before-you-begin"></a>Voordat u begint

### <a name="install-the-latest-version-of-azure-powershell"></a>Installeer de meest recente versie van Azure PowerShell

> [AZURE.NOTE]  Als u Azure PowerShell met SQL Data Warehouse, moet u Azure PowerShell versie 1.0.3 of hoger.  Voor de verificatie van uw huidige versie, voert u de opdracht **Get-Module - ListAvailable-naam Azure**. U kunt de meest recente versie van [Microsoft Web Platform Installer][]installeren.  Lees [hoe u installeren en configureren van Azure PowerShell][]voor meer informatie.

### <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets

Aan de slag:

1. Open Azure PowerShell. 
2. Klik bij de PowerShell-prompt, moet u deze opdrachten u zich aanmeldt bij de Azure Resource Manager en selecteert u uw abonnement uitvoeren.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rekenkracht schaal

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Als u wilt wijzigen van de DWUs, gebruikt u de PowerShell-cmdlet [Set-AzureRmSqlDatabase][] . Het volgende voorbeeld wordt de service niveau doelstelling ingesteld op DW1000 voor de database MySQLDW die wordt gehost op server MijnServer. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Een pauze invoegen berekenen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken, gebruikt u de cmdlet [Onderbreking-AzureRmSqlDatabase][] . Het volgende voorbeeld wordt een database met de naam die worden gehost op een server met de naam Server01 Database02 onderbroken. De server is in een Azure resourcegroep met de naam ResourceGroup1. 

> [AZURE.NOTE] Er gebeurt als de server onderdeel is foo.database.windows.net, "foo" Als de servernaam - in de PowerShell-cmdlets gebruiken.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Een variatie, in dit voorbeeld van de volgende haalt de database naar het object $database. Deze vervolgens buizen het object naar [Onderbreking AzureRmSqlDatabase][]. De resultaten worden opgeslagen in de object-resultDatabase. De opdracht definitief geeft de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Als u wilt een database, gebruikt u de cmdlet [Cv-AzureRmSqlDatabase][] . Het volgende voorbeeld wordt een database met de naam die worden gehost op een server met de naam Server01 Database02 gestart. De server is in een Azure resourcegroep met de naam ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Een variatie, in dit voorbeeld van de volgende haalt de database naar het object $database. Vervolgens het object naar [Cv-AzureRmSqlDatabase][] buizen en slaat de resultaten in $resultDatabase. De opdracht definitief geeft de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [overzicht van Management][]voor andere beheertaken.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Beheer van overzicht]: ./sql-data-warehouse-overview-manage.md
[Het installeren en configureren van Azure PowerShell]: ./powershell-install-configure.md
[Overzicht van de berekeningscluster beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Cv-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Schorten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
