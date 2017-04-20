<properties
   pageTitle="Migreren van uw schema naar SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het migreren van uw schema naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migreren van uw schema naar SQL Data Warehouse#

De volgende samenvattingen kunnen u meer informatie over de verschillen tussen SQL Server en SQL Data Warehouse voor het migreren van uw database.

## <a name="table-migration"></a>Tabel-migratie

Wanneer u migreert tabellen, wilt u vertrouwd te raken met de tabelfuncties van SQL Data Warehouse tabellen.  Het [overzicht van de tabel][] is een prima plek om te beginnen.  In dit artikel maakt u kennis met de belangrijkste overwegingen bij het maken van een tabel zoals tabel statistiek-verdeling gebruikt, partitioneren en indexeren.  Dit omvat tevens sommige [niet-ondersteunde tabelfuncties][] en tijdelijke oplossingen.

SQL Data Warehouse ondersteunt de algemene business-gegevenstypen.  Zie het artikel [gegevenstypen][] voor een lijst met ondersteunde en [niet-ondersteunde gegevenstypen][].  De [gegevenstypen][] artikel bevat ook een query voor [niet-ondersteunde gegevenstypen][].  Als door uw gegevenstypen te converteren, moet u kijkt u naar de [Aanbevolen procedures voor het gegevenstype][].

## <a name="next-steps"></a>Volgende stappen
Nadat u uw databaseschema is gemigreerd naar SQL Data Warehouse, Ga naar een van de volgende artikelen:

- [Migreren van uw gegevens][]
- [Migreren van uw code][]

Zie het artikel [Aanbevolen procedures][] voor meer informatie over aanbevolen werkwijzen voor SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Migreren van uw code]: ./sql-data-warehouse-migrate-code.md
[Migreren van uw gegevens]: ./sql-data-warehouse-migrate-data.md
[aanbevolen procedures]: ./sql-data-warehouse-best-practices.md
[overzicht van de tabel]: ./sql-data-warehouse-tables-overview.md
[niet-ondersteunde tabelfuncties]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[niet-ondersteunde gegevenstypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[aanbevolen procedures voor het gegevenstype]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
