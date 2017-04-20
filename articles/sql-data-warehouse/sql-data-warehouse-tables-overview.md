<properties
   pageTitle="Overzicht van tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met Azure SQL-gegevenstabellen BTW-records."
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
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Overzicht van tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

Aan de slag met het maken van tabellen in SQL Data Warehouse is eenvoudig.  De syntaxis van de eenvoudige [Tabel maken][] volgt de syntaxis van de algemene u bent waarschijnlijk al kent van het werken met andere databases.  Als u wilt een tabel maakt, moet u gewoon uw tabel een naam geven, naam van de kolommen en gegevenstypen voor elke kolom definiëren.  Als u hebt tabellen maken in andere databases, ziet dit er erg bekend voorkomen voor u.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Het bovenstaande voorbeeld wordt een tabel Klanten met twee kolommen, voornaam en achternaam.  Elke kolom wordt gedefinieerd met het gegevenstype van VARCHAR(25), waardoor de gegevens die moeten 25 tekens.  Deze fundamentele kenmerken van een tabel, evenals anderen, zijn hoofdzakelijk hetzelfde als de andere databases.  Gegevenstypen zijn gedefinieerd voor elke kolom en de integriteit van uw gegevens te garanderen.  Indexen kunnen worden toegevoegd aan de prestaties verbeteren door I/O.  Partitioneren kan worden toegevoegd aan de prestaties verbeteren als u nodig hebt om gegevens te wijzigen.

[De naam wijzigen] [ RENAME] een tabel SQL Data Warehouse ziet er zo uit:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Verdeelde tabellen

Een nieuw fundamentele kenmerk geïntroduceerd door gedistribueerde systemen zoals SQL Data Warehouse is de **kolom van de verdeling**.  De kolom verdeling is zeer veel wat het klinkt zoals.  Dit is de kolom die hoe distribueren of delen bepaalt, uw gegevens achter de schermen.  Wanneer u een tabel zonder op te geven van de kolom verdeling maakt, is de tabel automatisch verdeeld volgens **round robin**.  Hoewel round robin tabellen voldoende in sommige gevallen zijn kunnen, kunt definiëren verdeling kolommen enorm verkleinen verplaatsing van gegevens tijdens query's, dus prestaties optimaliseren.  Zie [een tabel distribueren][verdelen] voor meer informatie over het selecteren van een verdeling kolom.

## <a name="indexing-and-partitioning-tables"></a>Indexering en tabellen partitioneren

Als u aan met SQL Data Warehouse worden geavanceerdere en prestaties optimaliseren wilt, wilt u meer informatie over tabellen-ontwerpen.  Zie de artikelen op [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [indexeren van een tabel][Index] en [een tabel partitioneren][Partition]meer informatie.

## <a name="table-statistics"></a>Tabelstatistieken

Statistiek zijn een erg belangrijk zijn voor het ophalen van de beste prestaties afmelden bij uw SQL Data Warehouse.  Aangezien SQL Data Warehouse nog automatisch maakt en statistieken van de update voor u, alsof u mogelijk hebt zou verwachten in Azure SQL-Database, het lezen van onze-artikel over [Statistieken][] mogelijk een van de belangrijkste artikelen wordt u om ervoor te zorgen dat u de beste prestaties uit uw query's ophalen lezen.

## <a name="temporary-tables"></a>Tijdelijke tabellen

Tijdelijke tabellen zijn tabellen die alleen bestaan voor de duur van uw aanmelding en niet zichtbaar voor andere gebruikers.  Tijdelijke tabellen is een goede manier om te voorkomen dat anderen zien tijdelijke resultaten en ook dat u een opruimen te verkleinen.  Aangezien tijdelijke tabellen ook gebruikmaken van lokale opslag, kunnen ze sneller voor bepaalde bewerkingen bieden.  Zie de [Tijdelijke tabel][tijdelijke] artikelen voor meer informatie over tijdelijke tabellen.

## <a name="external-tables"></a>Externe tabellen

Externe tabellen, ook bekend als Polybase tabellen, zijn tabellen vanuit SQL Data Warehouse, maar punt kunnen worden opgezocht met externe gegevens uit SQL Data Warehouse.  Bijvoorbeeld, kunt u een externe tabel die verwijst naar bestanden op Azure-blobopslag.  Zie voor meer informatie over het maken en een externe tabel query [laden gegevens met Polybase][].  

## <a name="unsupported-table-features"></a>Niet-ondersteunde tabelfuncties

SQL Data Warehouse bevat veel van dezelfde tabelfuncties aangeboden door andere databases, maar er zijn enkele functies die nog niet worden ondersteund.  Hieronder volgt een lijst met enkele van de tabelfuncties die nog niet worden ondersteund.

| Niet-ondersteunde functies |
| --- |
|[De identiteit][] (Zie [Tijdelijke oplossing voor het toewijzen van vervangende sleutel][])|
|Primaire sleutel, refererende sleutels, unieke- en Check- [Beperkingen van de tabel][]|
|[Unieke indexen][]|
|[Berekende kolommen][]|
|[Verspreid kolommen][]|
|[De gebruiker gedefinieerde typen][]|
|[Reeks][]|
|[Triggers][]|
|[Geïndexeerde weergaven][]|
|[Synoniemen][]|

## <a name="table-size-queries"></a>Tabel grootte-query 's

Een eenvoudige manier om te identificeren spaties en rijen dat door een tabel in elk van de 60 onderzoeken, is het gebruik van [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Echter kan met DBCC-opdrachten worden helemaal beperken.  Dynamische management weergaven (DMVs) kunt u veel meer details, evenals hebt u veel meer controle over de queryresultaten.  Beginnen met het maken van deze weergave, waarin wordt verwezen bij veel van onze voorbeelden in dit en andere artikelen.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tabelruimte samenvatting

Deze query retourneert de rijen en ruimte op tabel.  Dit is een uitstekende query om te zien welke tabellen worden uw grootste tabellen en of ze round robin of hash verdeeld.  U ziet voor distributed hashtabellen ook de kolom verdeling.  In de meeste gevallen moet uw grootste tabellen hash wordt verdeeld met een index gegroepeerd columnstore.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabelruimte per Verdelingstype

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabelruimte per indextype

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Verdeling ruimte samenvatting

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Volgende stappen

Zie de artikelen op [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [Statistische tabelgegevens onderhouden][Statistieken] en[tijdelijke] [Tijdelijke tabellen]voor meer informatie.  Zie voor meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: ./sql-data-warehouse-best-practices.md
[Gegevens met Polybase laden]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TABEL MAKEN]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[De identiteit]: https://msdn.microsoft.com/library/ms186775.aspx
[Tijdelijke oplossing vervangend toewijzen]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Tabelbeperkingen]: https://msdn.microsoft.com/library/ms188066.aspx
[Berekende kolommen]: https://msdn.microsoft.com/library/ms186241.aspx
[Verspreid kolommen]: https://msdn.microsoft.com/library/cc280604.aspx
[De gebruiker gedefinieerde typen]: https://msdn.microsoft.com/library/ms131694.aspx
[Reeks]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Geïndexeerde weergaven]: https://msdn.microsoft.com/library/ms191432.aspx
[Synoniemen]: https://msdn.microsoft.com/library/ms177544.aspx
[Unieke indexen]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
