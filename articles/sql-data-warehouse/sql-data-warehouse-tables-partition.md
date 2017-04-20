<properties
   pageTitle="Tabellen in SQL Data Warehouse partitioneren | Microsoft Azure"
   description="Aan de slag met de tabel partitioneren in Azure SQL Data Warehouse."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse partitioneren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

Partitioneren wordt ondersteund op alle SQL Data Warehouse tabeltypen; inclusief gegroepeerde columnstore, geclusterde index en opslagruimte.  Partitioneren wordt ook ondersteund op alle typen van de verdeling, inclusief zowel hash of round robin verdeeld.  Partitioneren kunt is u uw gegevens splitsen in kleinere groepen van gegevens en in de meeste gevallen partitioneren voor een datumkolom ophalen uitgevoerd.

## <a name="benefits-of-partitioning"></a>Voordelen van het partitioneren

Partitioneren profiteert prestaties voor het onderhoud en de query van gegevens.  Opgeven of beide of slechts één kunt profiteren is afhankelijk van hoe gegevens worden geladen en of dezelfde kolom kan worden gebruikt voor beide doeleinden, aangezien partitioneren kan alleen worden uitgevoerd op één kolom.

### <a name="benefits-to-loads"></a>Voordelen van aan de laadtijd voor

Het belangrijkste voordeel van partitioneren in SQL Data Warehouse is verbeteren de efficiency en prestaties van gegevens door gebruik van partition verwijdering laden, overschakelen en samenvoegen.  Gegevens in de meeste gevallen is partities op een datumkolom ophalen die is nauw verbonden met de volgorde waarin de gegevens in de database is geladen.  Een van de grootste voordelen van het gebruik van partities voor het behoud van gegevens deze het vermijden van transactielogboek.  Terwijl u gewoon invoegen, bijwerken of verwijderen van gegevens de eenvoudigste manier, met wat creativiteit en inspanning is, kunt met partitioneren tijdens het laden aanzienlijk verbeteren prestaties.

Partition overstappen kan worden gebruikt om snel te verwijderen of vervangen van een sectie van een tabel.  Een verkoop feitentabel kan bijvoorbeeld alleen door de gegevens bevatten voor de afgelopen 36 maanden.  Aan het einde van elke maand, wordt de oudste maand met verkoopgegevens verwijderd uit de tabel.  Deze gegevens kan worden verwijderd met behulp van een verwijderinstructie om te verwijderen van de gegevens voor de oudste maand.  Echter kunt verwijderen van een grote hoeveelheid gegevens rij-voor-rij met een delete-instructie erg lang duren, evenals de kans op pesterijen grote transacties die lang te draaien duren kan als er iets mis gaat maken.  Een meer optimale aanpak is u gewoon het oudste partition van gegevens.  Waar de afzonderlijke rijen verwijderen uur kan duren duurt verwijderen van een hele partition seconden.

### <a name="benefits-to-queries"></a>Voordelen tot query 's

Partitioneren kan ook worden gebruikt om queryprestaties te verbeteren.  Als een query wordt een filter op een gepartitioneerde kolom toegepast, kan dit de gescande afbeelding aan alleen de in aanmerking komende partities die mogelijk een veel subset van gegevens voorkomen scannen van de volledige tabel kunt beperken.  Door de introductie van gegroepeerd columnstore indexen zijn de voordelen van de prestaties predikaatfunctie schrappen zijn minder handig, maar in sommige gevallen kan er een voordeel voor query's.  Bijvoorbeeld als de verkoop feitentabel is verdeeld in 36 maanden met het veld Verkoopdatum, vervolgens een query naar dat filter op de verkoopdatum kunt overslaan zoeken in partities die niet overeenkomen met het filter.

## <a name="partition-sizing-guidance"></a>Partition hoekformaatgreep richtlijnen

Terwijl partitioneren kan worden gebruikt om de prestaties verbeteren bepaalde scenario's, kan het maken van een tabel met **te veel** partities prestaties onder bepaalde omstandigheden schade.  Deze problemen zijn met name waar voor gegroepeerd columnstore tabellen.  Voor het partitioneren om het handig zijn, is het is belangrijk om te begrijpen hoe gebruikt u partitioneren en het aantal partities die u wilt maken.  Er is geen vaste snel regel over hoeveel partities te veel worden, dit is afhankelijk van hoeveel partities u en uw gegevens worden geladen naar tegelijk.  Maar ware 10s naar 100s van partities, niet 1000s toevoegen als een algemene vuistregel.

Bij het maken van partities op **gegroepeerde columnstore** tabellen, is het belangrijk om rekening met het aantal rijen in elke partition wordt terechtkomt te.  Voor optimale compressie en prestaties van gegroepeerd columnstore tabellen, is een minimum aan 1 miljoen rijen per distributie en partition nodig.  Voordat de partities worden gemaakt, wordt elke tabel al verdeeld met SQL Data Warehouse in 60 verdeelde databases.  Een partitioneren toegevoegd aan een tabel is naast de onderzoeken achter de schermen gemaakt.  Dit voorbeeld gebruiken als de verkoop feitentabel 36 maandelijkse partities opgenomen en gezien het feit dat SQL Data Warehouse 60 onderzoeken heeft, klikt u vervolgens de verkoop feitentabel bevatten moet 60 miljoen rijen per maand of 2.1 miljard rijen wanneer alle maanden zijn ingevuld.  Als een tabel minder rijen dan de aanbevolen minimum aantal rijen per partition bevat, kunt u overwegen minder partities zodat u het aantal rijen per partition vergroten.  Zie ook het [indexeren][Index] artikel waarin query's die kunnen worden uitgevoerd voor SQL Data Warehouse de kwaliteit van cluster columnstore indexen.

## <a name="syntax-difference-from-sql-server"></a>Syntaxis van de verschil met SQL Server

SQL Data Warehouse maakt u kennis met een eenvoudigere definitie van de partities die enigszins van SQL Server verschilt.  Partities functies en schema's worden niet gebruikt in SQL Data Warehouse als ze in SQL Server worden.  U hoeft te is in plaats daarvan welke gepartitioneerde kolom en de rand wordt verwezen.  Terwijl de syntaxis van de partitioneren enigszins van SQL Server afwijken, wordt de basisbegrippen zijn identiek.  SQL Server en SQL Data Warehouse ondersteuning voor één partitiekolom per tabel die varieerden partition kan zijn.  Zie meer informatie over het partitioneren [partitioneren tabellen en indexen][].

Het onderstaande voorbeeld van een instructie SQL Data Warehouse partitioneren [tabel maken][] partities de tabel FactInternetSales aan de kolom OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migreren partitioneren uit SQL Server

Om te migreren definities van SQL Server-partition gewoon naar SQL Data Warehouse:

- De SQL Server- [partition kleurenschema][]verwijderen.
- De definitie van de [partitiefunctie][] toevoegen aan uw tabel maken.

Als u een gepartitioneerde tabel vanuit een SQL Server-instantie migreert de onder SQL kunnen u helpen bij het aantal rijen in elke partition status.  Houd er rekening mee dat als de dezelfde partities granulatie wordt gebruikt voor SQL Data Warehouse, het aantal rijen per partition wordt verlagen met een factor van 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Beheer van de werklast

Een definitief stuk overweging te houden de tabel partition beschikking is [werkbelasting management][].  Beheer van de werklast in SQL Data Warehouse is hoofdzakelijk het beheer van geheugen en gelijktijdigheid.  Het maximale geheugen toegewezen aan elke verdeling tijdens het uitvoeren van query is in SQL Data Warehouse geregeld resource klassen.  Ideale geval wordt de partities worden verkleind tegen andere factoren zoals de behoeften geheugen van het bouwen van een gegroepeerd columnstore indexen.  Gegroepeerde columnstore indexen voordeel enorm wanneer ze meer geheugen worden toegewezen.  Daarom zult u om ervoor te zorgen dat een partition index opnieuw opbouwen niet geheugen wordt gezet. De hoeveelheid geheugen die beschikbaar zijn voor uw query met groter wordende kan worden bereikt door het overschakelen van de standaardrol, smallrc, naar een van de andere rollen zoals largerc.

Informatie over de toewijzing van geheugen per verdeling is beschikbaar via query's in de resource gouverneur dynamische management weergaven. In feite is uw verlenen geheugen kleiner dan de onderstaande cijfers. Dit biedt echter een niveau van richtlijnen die u gebruiken kunt wanneer uw partities voor management gegevensbewerkingen formaat te wijzigen.  Proberen te voorkomen dat uw partities groter dan het geheugen verlenen verstrekt door de klas extra groot resource formaat te wijzigen. Als uw partities groter dan in deze afbeelding kunt u de kans op pesterijen geheugendruk die op zijn beurt naar minder optimale compressie leidt uitvoeren.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partition overschakelen

SQL Data Warehouse ondersteunt partition splitsen, samenvoegen en overschakelen. Elk van deze functies is excuted met de instructie [ALTER TABLE][] .

Als u wilt schakelen van de partities tussen twee tabellen moet u ervoor zorgen dat de partities op de desbetreffende grenzen uitlijnen en dat de tabeldefinities overeenkomen. Als het selectievakje beperkingen zijn niet beschikbaar voor het afdwingen van het bereik van waarden in een tabel moet de brontabel de dezelfde partition grenzen als de doeltabel bevatten. Als dit niet het geval is, klikt u vervolgens de schakeloptie partition, mislukt tijdens de metagegevens partition niet worden gesynchroniseerd.

### <a name="how-to-split-a-partition-that-contains-data"></a>Het splitsen van een partition met gegevens

De efficiëntste methode voor het splitsen van een partition die al gegevens bevat is het gebruik van een `CTAS` instructie. Als de gepartitioneerde tabel een gegroepeerd columnstore is moet zich de tabel partition lege voordat deze kan worden gesplitst.

Hierna ziet u een voorbeeld-gepartitioneerde columnstore-tabel met één rij in elke partition is:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Als u het object statistische maakt, zorgen we dat tabelmetagegevens nauwkeuriger zijn. Als we statistieken maken weglaat, klikt u vervolgens SQL Data Warehouse gebruiken standaardwaarden. Raadpleeg [Statistieken][]voor meer informatie over statistieken.

We vervolgens kunt zoeken voor de rij tellen met de `sys.partitions` weergave catalogus:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Als we splitsen in deze tabel wilt, krijgt we een foutmelding:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, niveau 15, staat 1, regel 44 GESPLITST component van PARTITION wijzigen-instructie is mislukt omdat het niet leeg is.  Alleen lege partities kunnen worden gesplitst wanneer u een index columnstore op de tabel is. Houd rekening met uitschakelen van de index columnstore voordat u de instructie PARTITION wijzigen en klik vervolgens opnieuw columnstore wanneer PARTITION wijzigen voltooid is.

We kunnen echter gebruiken `CTAS` om een nieuwe tabel waarin onze gegevens te maken.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Een schakeloptie is toegestaan als de grenzen partition worden uitgelijnd. Dit betekent dat de brontabel met een lege partition die we later kunt splitsen.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Alle die er nog moet gebeuren is om uit te lijnen onze gegevens naar de nieuwe partition grenzen met `CTAS` en schakeloptie onze gegevens opnieuw in aan de primaire tabel

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Als u klaar bent met het verplaatsen van de gegevens is een goed idee om de statistieken over de doeltabel om ervoor te zorgen dat nauwkeurig weerspiegelen van de nieuwe verdeling van de gegevens in de desbetreffende partities vernieuwen:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabel partitioneren besturingselement voor gegevensbronnen

Om te voorkomen dat de tabeldefinitie van uw van **roesten** in het brone-mailsysteem besturingselement wilt u mogelijk Houd rekening met de volgende methode:

1. De tabel te maken als een gepartitioneerde tabel maar zonder partition-waarden

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`de tabel als onderdeel van het implementatieproces:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Met deze methode de code in een besturingselement voor gegevensbronnen statische blijft en de partities grens waarden zijn toegestaan dynamisch; met de warehouse ontwikkeling na verloop van tijd.

## <a name="next-steps"></a>Volgende stappen

Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [indexeren van een tabel][Index], [Statistische tabelgegevens onderhouden][Statistieken] en[tijdelijke] [Tijdelijke tabellen]meer informatie.  Zie voor meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[beheer van de werklast]: ./sql-data-warehouse-develop-concurrency.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Gepartitioneerde tabellen en indexen]: https://msdn.microsoft.com/library/ms190787.aspx
[TABEL WIJZIGEN]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TABEL MAKEN]: https://msdn.microsoft.com/library/mt203953.aspx
[Partition, functie]: https://msdn.microsoft.com/library/ms187802.aspx
[Partition kleurenschema]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
