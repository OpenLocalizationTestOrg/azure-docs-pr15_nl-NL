<properties
   pageTitle="Beheren rekenkracht in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="(T-SQL) Transact-SQL-taken aan schalen prestaties door DWUs aan te passen. Bespaar kosten door schaalbaarheid terug tijdens de niet-tijden."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Beheren rekenkracht in Azure SQL Data Warehouse (T-SQL)

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties van de schaal door schalen berekenen bronnen en het geheugen om te voldoen aan de veranderende vereisten van uw werkzaamheden. Bespaar kosten door schaal achterste resources tijdens de niet-tijden of onderbreken berekeningscluster helemaal af. 

Deze verzameling taken gebruikt de T-SQL naar:

- Weergave-instellingen voor huidige DWU
- Wijziging berekeningscluster resources door DWUs aan te passen

Als u wilt onderbreken of hervatten van een database, kiest u een van de andere platform opties boven aan dit artikel.

Zie meer informatie over dit, [beheren berekenen power overzicht][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Weergave-instellingen voor huidige DWU

De huidige DWU-instellingen voor uw databases weergeven:

1. SQL Server-Object Explorer openen in Visual Studio-2015.
2. Verbinding maken met de hoofddatabase die is gekoppeld aan de logische SQL Database-server.
2. Selecteer in de weergave van de dynamische beheer sys.database_service_objectives. Hier volgt een voorbeeld: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Schaal berekenen

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

De DWUs wijzigen:


1. Verbinding maken met de hoofddatabase die is gekoppeld aan uw logische SQL Database-server.
2. Gebruik de instructie [ALTER DATABASE][] TSQL. Het volgende voorbeeld wordt de service niveau doelstelling ingesteld op DW1000 voor de database MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [overzicht van Management][]voor andere beheertaken.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Beheer van overzicht]: ./sql-data-warehouse-overview-manage.md
[Berekeningscluster power overzicht beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[DATABASE WIJZIGEN]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
