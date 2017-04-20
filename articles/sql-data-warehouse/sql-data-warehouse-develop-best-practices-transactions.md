<properties
   pageTitle="Transacties voor SQL Data Warehouse optimaliseren | Microsoft Azure"
   description="Aanbevolen oefening richtlijnen voor het schrijven van efficiënt transactie-updates in Azure SQL Data Warehouse"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Transacties voor SQL Data Warehouse optimaliseren

In dit artikel wordt uitgelegd hoe de prestaties van uw transacties code minimaal risico voor lange terugdraaien van acties optimaliseren.

## <a name="transactions-and-logging"></a>Transacties en gegevens vastleggen

Transacties zijn een belangrijk onderdeel van een relationele database-engine. Transacties SQL Data Warehouse gebruikt tijdens de wijziging van gegevens. Deze transacties zijn expliciete of impliciete. Eén `INSERT`, `UPDATE` en `DELETE` instructies zijn alle voorbeelden van impliciete transacties. Expliciete transacties expliciet worden geschreven door een ontwikkelaar met `BEGIN TRAN`, `COMMIT TRAN` of `ROLLBACK TRAN` en worden meestal gebruikt wanneer meerdere wijziging instructies moeten samen in één atomaire eenheid worden gekoppeld. 

Azure SQL Data Warehouse wijzigingen worden doorgevoerd in de database met transactielogboeken. Elke verdeling heeft een eigen transactielogboek. Transactie log schrijft zijn automatische. Er is geen configuratie vereist. Terwijl dit proces zorgt ervoor dat het schrijven deze een realiseren tot in het systeem. U kunt dit gevolgen minimaliseren door transactioneel efficiënt code te schrijven. Transactioneel efficiënt code worden ruim zijn onderverdeeld in twee categorieën.

- Gebruikmaken van minimale logboekregistratie constructies, indien mogelijk
- Procesgegevens met behulp van beperkt batches enkelvoud langdurige transacties vermijden
- Een patroon voor grote wijzigingen aan een bepaald partition overstappen partition vast

## <a name="minimal-vs-full-logging"></a>Minimale versus volledige logboekregistratie

In tegenstelling tot volledig geregistreerde bewerkingen, waarin het transactielogboek gebruiken om elke rijwijziging bij te houden, minimaal geregistreerde bewerkingen van bijhouden zover toewijzingen en alleen de metagegevens wijzigingen. Daarom minimale logboekregistratie heeft betrekking op logboekregistratie alleen de informatie die is vereist te draaien de transactie in het geval van een fout of een expliciete aanvraag (`ROLLBACK TRAN`). Als u veel minder informatie wordt bijgehouden in het transactielogboek, wordt een minimaal geregistreerde bewerking beter dan een vergelijkbaar formaat volledig geregistreerde bewerking kunt uitvoeren. Bovendien omdat minder schrijft gaan het transactielogboek, veel minder logboekgegevens wordt gegenereerd en dus meer I/O efficiënt.

De limieten van de beveiliging transactie zijn alleen van toepassing op volledig geregistreerde bewerkingen.

>[AZURE.NOTE] Minimaal geregistreerde bewerkingen kunnen deelnemen aan expliciete transacties. Als u alle wijzigingen in toewijzing structuren worden bijgehouden, is het mogelijk minimaal geregistreerde bewerkingen terugkeren. Het is belangrijk om te begrijpen dat de wijziging "minimaal" wordt geregistreerd niet is niet aangemeld.

## <a name="minimally-logged-operations"></a>Minimaal geregistreerde bewerkingen

De volgende bewerkingen zijn kunnen minimaal aan te melden:

- TABEL als selecteren ([CTAS][]) maken
- INVOEGEN... SELECTEER
- INDEXEN MAKEN
- ALTER INDEX OPNIEUW OPBOUWEN
- DECORATIEVE INDEX
- TABEL AFKAPPEN
- TABEL VERWIJDEREN
- ALTER TABEL VERANDEREN PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Verplaatsing van interne gegevensbewerkingen (zoals `BROADCAST` en `SHUFFLE`) worden niet beïnvloed door de limiet van de beveiliging transactie.

## <a name="minimal-logging-with-bulk-load"></a>Minimale logboekregistratie met bulksgewijs laden

`CTAS`en `INSERT...SELECT` worden beide bulksgewijs te laden bewerkingen. Echter beide worden beïnvloed door de definitie van de doel-tabel en zijn afhankelijk van het scenario laden. Hierna volgt een tabel waarin wordt uitgelegd als uw bulkbewerking wordt volledig of minimaal geregistreerd:  

| Primaire Index               | Scenario voor laden                                            | Logboekregistratie-modus |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Opslagruimte                        | Een                                                      | **Minimale**  |
| Gegroepeerde Index             | Lege doeltabel                                       | **Minimale**  |
| Gegroepeerde Index             | Geladen rijen elkaar niet overlappen met bestaande pagina's in de doeltoepassing | **Minimale**  |
| Gegroepeerde Index             | Geladen rijen laat deze overlappen met bestaande pagina's in de doeltoepassing        | Volledige         |
| Gegroepeerde Columnstore Index | Grootte batch > = 102,400 per partition uitgelijnd verdeling | **Minimale**  |
| Gegroepeerde Columnstore Index | Batch grootte < 102,400 per partition uitgelijnd verdeling  | Volledige         |

Het is handig om te weten dat alle schrijft secundaire of niet-gegroepeerde indexen bijwerken altijd volledig geregistreerde bewerkingen zijn.

> [AZURE.IMPORTANT] SQL Data Warehouse heeft 60 onderzoeken. Daarom, ervan uitgaande dat er alle rijen gelijkmatig zijn verdeeld en lossen in een enkel partition, uw batch moet 6,144,000 rijen bevatten of groter minimaal zijn aangemeld bij het schrijven naar een Columnstore gegroepeerde Index. Als de tabel is partities en de rijen die wordt ingevoegd tijdspanne partition grenzen, moet u 6,144,000 rijen per partition begrenzing ervan uitgaande dat er zelfs gegevens verdeling. Elke partition in elke verdeling moet onafhankelijk groter is dan de drempelwaarde voor 102.400 rij voor het invoegen minimaal aangemeld zijn de verdeling.

Gegevens laden in een niet-lege tabel met een gegroepeerde index, kan er vaak een combinatie van volledig geregistreerde en minimaal geregistreerde rijen bevatten. Een gegroepeerde index is een gebalanceerde structuur (b-structuur) van pagina's. Als de pagina naar al bevat rijen uit een andere transactie worden geschreven, klikt u vervolgens deze schrijft volledig geregistreerd. Echter als de pagina leeg is vervolgens het schrijven naar die pagina minimaal geregistreerd.

## <a name="optimizing-deletes"></a>Hiermee verwijdert u optimaliseren

`DELETE`een volledig geregistreerde bewerking is.  Als u verwijderen van een grote hoeveelheid gegevens in een tabel of een partition wilt, dat vaak relevant meer naar `SELECT` de gegevens die u behouden wilt, en die kan worden uitgevoerd als een minimaal geregistreerde bewerking.  Hiervoor moet u een nieuwe tabel maken met [CTAS][].  Zodra gemaakt, gebruikt u de [naam][] u verwisselt uw oude tabel met de nieuwe tabel.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Updates optimaliseren

`UPDATE`een volledig geregistreerde bewerking is.  Als u wilt bijwerken van een groot aantal rijen in een tabel of een partition dit kan vaak uiterst efficiënter om te gebruiken van een minimaal geregistreerde bewerking zoals [CTAS][] kunt doen.

In het voorbeeld hieronder een volledige tabel update is geconverteerd naar een `CTAS` zodat minimale logboekregistratie mogelijk is.

In dit geval zijn we een kortingsbedrag achteraf toevoegen aan de verkoop in de tabel:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Grote tabellen opnieuw te maken kunt profiteren van het gebruik van functies voor het beheren van werkbelasting SQL Data Warehouse. Raadpleeg de sectie voor het beheren van werkbelasting in het artikel [gelijktijdigheid][] voor meer informatie.

## <a name="optimizing-with-partition-switching"></a>Optimaliseren met partition overschakelen

Wanneer geconfronteerd met grote schaal wijzigingen in een [tabel partition][], maakt een patroon overstappen partition vervolgens een groot aantal komen. Als de wijziging van gegevens aanzienlijk is en zich uitstrekt over meerdere partities, klikt u vervolgens bij gewoon iteratie van de partities hetzelfde resultaat bereikt.

De stappen voor het uitvoeren van een schakeloptie partition zijn als volgt:
1. Een leeg out partition maken
2. De 'update' als een CTAS uitvoeren
3. De bestaande gegevens weggegooid overschakelen naar de out tabel
4. Schakelen tussen in de nieuwe gegevens
5. De gegevens opschonen

Echter ter identificatie van de partities die u wilt schakelen we er eerst voor het maken van een procedure helper zoals de onderstaande. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Deze procedure gemaximaliseerd code opnieuw gebruiken en blijft de partition compactere voorbeeld veranderen.

De onderstaande code ziet u de vijf stappen hierboven genoemde om te bereiken van een volledige partition routine overschakelen.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Logboekregistratie met kleine batches minimaliseren

Voor bewerkingen voor het wijzigen van grote gegevens, kan het zinvol zijn worden verdeeld stukken of batches om te bepalen de hoeveelheid werk, de bewerking.

Hieronder vindt u een voorbeeld werken. De batchgrootte is ingesteld op een eenvoudig getal om te markeren van de methode. In feite is de batchgrootte veel meer. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Een pauze invoegen en de schaal van richtlijnen

Azure SQL Data Warehouse kunt u onderbreken, hervatten en schaal van uw gegevenswarehouse op aanvraag. Wanneer u onderbreken of uw SQL Data Warehouse schaal is het belangrijk om te begrijpen dat tijdens een vlucht transacties direct; worden beëindigd Als gevolg van openstaande transacties ongedaan. Als uw werkzaamheden hebt u er een wijziging van de gegevens lang actief zijn en onvoltooide voordat u de bewerking een pauze invoegen of schaal verleend, moet dit te activeren dat ongedaan moet worden. Dit kan van invloed zijn op hoe lang die het duurt om te onderbreken of schaal van de Data Warehouse van Azure SQL-database. 

> [AZURE.IMPORTANT] Beide `UPDATE` en `DELETE` zijn volledig geregistreerde bewerkingen en zodat deze ongedaan maken/opnieuw bewerkingen kunnen aanzienlijk langer duren dan equivalent aangemeld minimaal bewerkingen. 

Het beste scenario is om te laten in flight gegevens wijziging transacties voltooid voordat u onderbreken of SQL Data Warehouse schaalbaarheid. Echter niet altijd mogelijk praktische. Als u wilt Verklein het risico van een lang terugdraaien, kunt u een van de volgende opties:

- Langdurige bewerkingen opnieuw schrijven [CTAS][] gebruiken
- De bewerking uitsplitsen in stukken; Klik op een subset van de rijen besturingsomgeving

## <a name="next-steps"></a>Volgende stappen

Zie [transacties in SQL Data Warehouse][] voor meer informatie over moeten worden geïsoleerd niveaus en transacties limieten.  Zie [Aanbevolen procedures voor Warehouse SQL-gegevens][]voor een overzicht van andere aanbevolen procedures.

<!--Image references-->

<!--Article references-->
[Transacties in SQL datawarehouse]: ./sql-data-warehouse-develop-transactions.md
[tabel partition]: ./sql-data-warehouse-tables-partition.md
[Bij gelijktijdigheid]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[NAAM WIJZIGEN]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

