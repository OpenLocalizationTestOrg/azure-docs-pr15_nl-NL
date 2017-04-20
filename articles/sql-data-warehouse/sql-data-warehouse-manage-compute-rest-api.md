<properties
   pageTitle="Beheren rekenkracht in Azure SQL Data Warehouse (REST) | Microsoft Azure"
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Rekenkracht in Azure SQL Data Warehouse (REST) beheren

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

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rekenkracht schaal

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

U wijzigt de DWUs met het [maken of bijwerken Database][] REST API. Het volgende voorbeeld wordt de service niveau doelstelling ingesteld op DW1000 voor de database MySQLDW die wordt gehost op server MijnServer. De server is in een Azure resourcegroep met de naam ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Een pauze invoegen berekenen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken, gebruikt u de [Database onderbreken][] REST API. Het volgende voorbeeld wordt een database met de naam die worden gehost op een server met de naam Server01 Database02 onderbroken. De server is in een Azure resourcegroep met de naam ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Als u wilt een database, gebruikt u de REST API voor [Cv-Database][] . Het volgende voorbeeld wordt een database met de naam die worden gehost op een server met de naam Server01 Database02 gestart. De server is in een Azure resourcegroep met de naam ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [overzicht van Management][]voor andere beheertaken.

<!--Image references-->

<!--Article references-->
[Beheer van overzicht]: ./sql-data-warehouse-overview-manage.md
[Overzicht van de berekeningscluster beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Een pauze invoegen-Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Cv-Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Maak of -Database bijwerken]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
