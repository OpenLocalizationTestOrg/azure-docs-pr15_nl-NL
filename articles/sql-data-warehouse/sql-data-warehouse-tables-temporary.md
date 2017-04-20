<properties
   pageTitle="Tijdelijke tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met tijdelijke tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Tijdelijke tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

Tijdelijke tabellen zijn bijzonder nuttig zijn bij het verwerken van gegevens - met name tijdens transformatie waar de tussenliggende resultaten tijdelijke zijn. In SQL Data Warehouse bestaan tijdelijke tabellen op het niveau van de sessie.  Ze zijn alleen zichtbaar voor de sessie waarin ze zijn gemaakt en worden niet automatisch weergegeven wanneer deze sessie zich afmeldt.  Tijdelijke tabellen bieden een voordeel prestaties omdat veldresultaten naar lokale in plaats van externe opslag zijn geschreven.  Tijdelijke tabellen zijn iets anders uit in Azure SQL Data Warehouse dan Azure SQL-Database, zoals ze zijn toegankelijk vanaf een willekeurige plaats in de sessie, ook binnen en buiten een opgeslagen procedure.

In dit artikel bevat essentiële richtlijnen voor het gebruik van de tijdelijke tabellen en de beginselen van sessie niveau tijdelijke tabellen gemarkeerd. Met behulp van de informatie in dit artikel, kunt u uw code, verbeteren hergebruik en de toegankelijkheid van onderhoud van uw code modularize.

## <a name="create-a-temporary-table"></a>Maak een tijdelijke tabel

Tijdelijke tabellen worden gemaakt door gewoon voorvoegsel toevoegen de naam van uw tabel met een `#`.  Bijvoorbeeld:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Tijdelijke tabellen kunnen ook worden gemaakt met een `CTAS` precies dezelfde aanpak gebruiken:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`de opdracht van een zeer krachtig is en het toegevoegde voordeel van het gebruik van transactie logboekruimte ermee werken. 


## <a name="dropping-temporary-tables"></a>Tijdelijke tabellen weghalen

Wanneer een nieuwe sessie is gemaakt, is geen tijdelijke tabellen moeten bestaan.  Echter als u verbinding maakt met de opgeslagen procedure, een tijdelijk gemaakt met dezelfde naam, om ervoor te zorgen dat uw `CREATE TABLE` instructies geslaagd zijn een eenvoudige oude bestaan Neem contact op met een `DROP` kunnen worden gebruikt als in het onderstaande voorbeeld:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Voor het coderen consistentie, is dit een goede gewoonte om dit patroon gebruiken voor zowel tabellen en tijdelijke tabellen.  Het is ook verstandig om te gebruiken `DROP TABLE` tijdelijke om tabellen te verwijderen wanneer u klaar bent met hen in uw code.  Ontwikkeling is vrij gebruikelijk om te zien met de opdrachten decoratieve samengebracht aan het einde van een procedure om ervoor te zorgen deze objecten worden in de opgeslagen procedure afgewerkt.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing code

Aangezien tijdelijke tabellen u een willekeurige plaats in een sessie ziet, kan dit kan worden gebruikt om het helpen u uw toepassingscode modularize.  Bijvoorbeeld de opgeslagen procedure hieronder worden samengebracht de aanbevolen procedures van boven naar DDL update van alle statistieken in de database door de naam van de statistische genereren.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dit stadium de enige actie dat zich heeft voorgedaan is het maken van een opgeslagen procedure waardoor gewoon c en d en een tijdelijke, #stats_ddl, met DDL-instructies.  Deze opgeslagen procedure wordt #stats_ddl neerzetten als deze al bestaat om ervoor te zorgen dat wordt niet als meer dan één keer uitvoeren in een sessie.  Echter, omdat er geen `DROP TABLE` aan het einde van de opgeslagen procedure, wanneer de opgeslagen procedure is voltooid, deze blijft de gemaakte tabel zodat deze kan worden gelezen buiten de opgeslagen procedure.  In SQL Data Warehouse is in tegenstelling tot andere SQL Server-databases, het mogelijk gebruik van de tijdelijke tabel buiten de procedure waarmee het is gemaakt.  Tijdelijke tabellen SQL Data Warehouse kunnen zich gebruikte **overal** binnen de sessie. Dit kan leiden tot meer modulaire en beheerbare-code als in het onderstaande voorbeeld:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Tijdelijke tabel beperkingen

SQL Data Warehouse een aantal beperkingen opleggen bij het implementeren van tijdelijke tabellen.  Op dit moment alleen sessie bereik tijdelijke tabellen worden ondersteund.  Globale tijdelijke tabellen worden niet ondersteund.  Bovendien kunnen weergaven kunnen niet worden gemaakt op tijdelijke tabellen.

## <a name="next-steps"></a>Volgende stappen

Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [indexeren van een tabel],[Index], [partitioneren van een tabel][Partition] en [Onderhouden tabelstatistieken][Statistieken]meer informatie.  Zie voor meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].

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

<!--MSDN references-->

<!--Other Web references-->
