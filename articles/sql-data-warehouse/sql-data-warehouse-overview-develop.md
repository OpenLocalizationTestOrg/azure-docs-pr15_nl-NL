<properties
   pageTitle="Ontwerpen beslissingen en code technieken voor SQL Data Warehouse ontwikkeling | Microsoft Azure"
   description="Ontwikkeling concepten, ontwerp, aanbevelingen en code technieken voor SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Beslissingen en code technieken voor SQL Data Warehouse ontwerpen

Verdiep u via deze artikelen ontwikkeling voor een beter begrip van belangrijke ontwerpbesluiten, aanbevelingen en code technieken voor SQL Data Warehouse.

## <a name="key-design-decisions"></a>Belangrijke ontwerpbeslissingen
De volgende artikelen markeren enkele belangrijke concepten en ontwerpbeslissingen die u moet begrijpen voor de ontwikkeling van uw gedistribueerde gegevenswarehouse SQL Data Warehouse gebruiken:

- [verbindingen][]
- [bij gelijktijdigheid][]
- [transacties][]
- [door gebruiker gedefinieerd schema's maken][]
- [tabel-verdeling][]
- [indexen van tabel][]
- [Tabelpartities][]
- [CTAS][]
- [statistieken][]

## <a name="development-recommendations-and-coding-techniques"></a>Aanbevelingen bij de ontwikkeling en code technieken
Deze artikelen markeren specifieke coding technieken, tips en aanbevelingen voor het ontwikkelen van uw SQL Data Warehouse:

- [opgeslagen procedures][]
- [labels][]
- [Weergaven][]
- [tijdelijke tabellen][]
- [dynamische SQL][]
- [herhalen][]
- [groeperen op Opties][]
- [variabele toewijzing][]

## <a name="next-steps"></a>Volgende stappen
Zodra u via de artikelen ontwikkeling zijn verdiep u via de pagina [Transact-SQL-verwijzing][] voor meer informatie over de syntaxis van de ondersteunde voor SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[bij gelijktijdigheid]: ./sql-data-warehouse-develop-concurrency.md
[verbindingen]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamische SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[groeperen op Opties]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[herhalen]: ./sql-data-warehouse-develop-loops.md
[statistieken]: ./sql-data-warehouse-tables-statistics.md
[opgeslagen procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[tabel-verdeling]: ./sql-data-warehouse-tables-distribute.md
[indexen van tabel]: ./sql-data-warehouse-tables-index.md
[Tabelpartities]: ./sql-data-warehouse-tables-partition.md
[tijdelijke tabellen]: ./sql-data-warehouse-tables-temporary.md
[transacties]: ./sql-data-warehouse-develop-transactions.md
[door gebruiker gedefinieerd schema's maken]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variabele toewijzing]: ./sql-data-warehouse-develop-variable-assignment.md
[Weergaven]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-verwijzing]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
