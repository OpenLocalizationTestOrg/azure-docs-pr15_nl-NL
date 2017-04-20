<properties
   pageTitle="Dynamische SQL in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamische SQL in SQL datawarehouse
Bij het ontwikkelen van toepassingscode voor SQL Data Warehouse moet u mogelijk een dynamische sql gebruiken om u te helpen flexibele, algemene en modulaire oplossingen. SQL Data Warehouse biedt geen ondersteuning voor de gegevenstypen blob op dit moment. Hiermee kan de grootte van uw tekenreeksen beperken zoals blob typen zowel varchar(max) en nvarchar(max) typen opnemen. Als u dit soort in uw toepassingscode gebruikt hebt bij het maken van zeer grote tekenreeksen, moet u de code in stukken verbreken en de Raad-instructie in plaats daarvan gebruiken.

Een eenvoudig voorbeeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de tekenreeks kort is kunt u [sp_executesql][] als normale.

> [AZURE.NOTE] Instructies die worden uitgevoerd als dynamische SQL nog steeds niet onderhevig aan alle regels voor gegevensvalidatie van TSQL.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
