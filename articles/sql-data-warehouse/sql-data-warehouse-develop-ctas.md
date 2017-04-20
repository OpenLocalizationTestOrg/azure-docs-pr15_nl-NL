<properties
   pageTitle="Tabel (CTAS) geselecteerd in SQL Data Warehouse maken | Microsoft Azure"
   description="Tips voor het coderen van de tabel maken als de select-instructie (CTAS) in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Tabel maken als selecteren (CTAS) in SQL datawarehouse
Selecteer tabel maken of `CTAS` is een van de belangrijkste T-SQL-functies beschikbaar. Dit is een volledig parallelized bewerking die u een nieuwe tabel op basis van de uitvoer van een SELECT-instructie maakt. `CTAS`is de eenvoudigste en snelste manier om een kopie van een tabel te maken. U kunt deze drukvulling versie van `SELECT..INTO` als u wilt. Dit document bevat zowel voorbeelden en aanbevolen procedures voor het `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>CTAS gebruiken om te kopiëren van een tabel

Misschien een van de meest gebruikte gebruikmaakt van `CTAS` wordt een kopie van een tabel maken zodat u de DDL kunt wijzigen. Als u bijvoorbeeld de tabel als oorspronkelijk gemaakt `ROUND_ROBIN` en nu wilt wijzigen in een tabel op een kolom, distributed `CTAS` ziet u hoe u de kolom van de verdeling wilt wijzigen. `CTAS`kan ook worden gebruikt om te typen partitioneren, indexering of kolom wijzigen.

Stel dat u hebt gemaakt in deze tabel met het standaardtype voor de verdeling van `ROUND_ROBIN` distributed aangezien er geen kolom verdeling is opgegeven in de `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nu wilt u een nieuwe kopie van deze tabel met een Columnstore gegroepeerde Index maken zodat u van de prestaties van gegroepeerde Columnstore tabellen profiteren kunt. U ook in deze tabel op ProductKey distributie sinds u zijn anticiperen op verbindingen in deze kolom en u wilt verplaatsen van gegevens voorkomen tijdens joins op ProductKey. U wilt reageren ook de partities op OrderDateKey, zodat u snel oude gegevens verwijderen kunt door neer te zetten oude partities toevoegen. Hier ziet u de instructie CTAS waarin uw oude tabel naar een nieuwe tabel kopiëren wilt.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Ten slotte kunt u de tabellen om te wisselen in de nieuwe tabel en zet uw oude tabel namen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL Data Warehouse biedt nog geen ondersteuning auto maken of bijwerken statistieken jaarlijks automatisch.  Om te krijgen de beste prestaties van uw query's, het is belangrijk dat statistieken worden gemaakt voor alle kolommen van alle tabellen na de eerste laden of belangrijke wijzigingen optreden in de gegevens.  Zie het onderwerp [Statistieken][] in de groep ontwikkelen van onderwerpen voor een gedetailleerde uitleg van statistieken.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Gebruik van CTAS omgaan met niet-ondersteunde functies

`CTAS`kan ook worden gebruikt om te omzeilen van een getal van de onderstaande niet-ondersteunde functies. Dit kan vaak bewijzen moeten een situatie win/win zoals niet alleen zijn uw code voldoen, maar deze wordt vaak sneller uitgevoerd op SQL Data Warehouse. Dit is grond van het ontwerp van de volledig parallelized. Scenario's die rond kunnen worden gewerkt met CTAS opnemen:

- SELECTEER... IN
- ANSI-JOINS op UPDATEs
- ANSI-JOINs op verwijderen
- Instructie samenvoegen

> [AZURE.NOTE] Probeer om na te denken "CTAS eerste". Als u denkt dat u kunt oplossen een probleem met `CTAS` die is gewoonlijk de beste benadering deze - zelfs als u meer gegevens als gevolg schrijft.
>

## <a name="selectinto"></a>SELECTEER... IN
Kan `SELECT..INTO` wordt weergegeven in een aantal locaties in uw oplossing.

Hieronder volgt een voorbeeld van een `SELECT..INTO` instructie:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Converteren naar de bovenstaande `CTAS` helemaal uitgestreken doorsturen is:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS is momenteel vereist dat een kolom verdeling worden opgegeven.  Als u niet met opzet hebt wilt wijzigen van de kolom verdeling uw `CTAS` de snelste wordt uitgevoerd als u de kolom van een verdeling die is hetzelfde als de onderliggende tabel als deze strategie vermijdt verplaatsing van gegevens selecteren.  Als u maakt een kleine tabel waar prestaties is niet een factor en klik vervolgens kunt u opgeven `ROUND_ROBIN` om te voorkomen moet bepalen van de kolom van een verdeling.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI join vervangende voor update-instructies

U vindt u mogelijk dat u hebt een complexe update die aan elkaar koppelt meer dan twee tabellen samenwerken met ANSI deelnemen aan de syntaxis van de bijwerken of verwijderen uit te voeren.

Stel dat u had om bij te werken in deze tabel:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Mogelijk de oorspronkelijke query hebt gezien ongeveer zo uitziet:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Aangezien SQL Data Warehouse biedt geen ondersteuning voor ANSI deelneemt aan de vergadering de `FROM` -component van een `UPDATE` instructie, niet kunt u deze code via kopiëren zonder deze te wijzigen enigszins.

U kunt een combinatie van een `CTAS` en een impliciete join voor het vervangen van deze code:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI join vervangende voor instructies verwijderen
Soms de beste manier om het verwijderen van gegevens is via `CTAS`. Selecteer de gegevens die u wilt behouden in plaats van de gegevens gewoon te verwijderen. Deze met name voor het geval `DELETE` overzichten die gebruikmaken van ansi-syntaxis van de gekoppelde aangezien SQL Data Warehouse biedt geen ondersteuning voor ANSI deelneemt aan de vergadering in de `FROM` -component van een `DELETE` instructie.

Een voorbeeld van een geconverteerde verwijderinstructie is beschikbaar onder:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Samenvoegen instructies vervangen
Samenvoegen-instructies kunnen worden vervangen, ten minste in deel, met behulp van `CTAS`. U kunt samenvoegen de `INSERT` en de `UPDATE` in één-instructie. Verwijderde records moeten worden afgesloten in een tweede instructie.

Een voorbeeld van een `UPSERT` is beschikbaar onder:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS aanbeveling: expliciet provincie gegevenstype en de optie voor nulwaarden in uitvoer

Wanneer u migreert code kunt u vinden dat u over dit type coding patroon uitvoeren:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctief mogelijk Bedenk eerst moet u deze code migreren naar een CTAS en u zou juiste. Er is echter een verborgen probleem hier.

De volgende code is niet hetzelfde resultaat rendement:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Zoals u ziet dat de kolom 'resultaat' doorsturen de gegevenswaarden voor type en de optie voor nulwaarden van de expressie uitvoert. Dit kan leiden tot subtiele varianties in waarden als u niet dat u daarbij.

Probeer de volgende handelingen uit als voorbeeld:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

De waarde die is opgeslagen voor resultaat wordt weergegeven. Zoals de permanente waarde in de resultaatkolom wordt gebruikt in andere expressies wordt de fout nog aanzienlijk.

![][1]

Dit is vooral voor gegevens-migraties. Hoewel de tweede query Hoewel nauwkeuriger is is er een probleem. De gegevens niet meer verschillende vergeleken met het bronsysteem en die leidt naar vragen integriteit bij de migratie. Dit is een van die niet vaak voorkomen gevallen, waarin het antwoord 'fout gegaan' dat is wel het juiste account!

De reden zien we deze verschillen tussen de twee resultaten is naar beneden af op impliciete type toewijzing. De tabel definieert in het eerste voorbeeld definitie van de kolom. Er treedt een impliciete typeconversie op de rij wordt ingevoegd. In het tweede voorbeeld is het geen impliciete typeconversie als het gegevenstype van de kolom wordt gedefinieerd voor de expressie. Zoals u ziet ook dat de kolom in het tweede voorbeeld is gedefinieerd als een lege kolom dat in het eerste voorbeeld deze niet heeft. Wanneer de tabel is gemaakt met de optie eerste voorbeeld voor kolom nulwaarden is expliciet gedefinieerd. In de tweede resulteert deze is net links op de expressie en al dan niet standaard dit voorbeeld in de definitie van een null-waarden.  

Deze problemen op te lossen moet u expliciet instellen de conversie van en de optie voor nulwaarden in de `SELECT` deel van de `CTAS` instructie. U kunt deze eigenschappen niet instellen in het tabelgedeelte maken.

Het volgende voorbeeld wordt getoond hoe de code vast:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Houd rekening met het volgende:
- CAST of CONVERT kunnen zijn gemaakt
- ISNULL wordt gebruikt voor de optie voor nulwaarden niet COALESCE afdwingen
- ISNULL is de buitenste functie
- Het tweede deel van de ISNULL is een constante dat wil zeggen 0

> [AZURE.NOTE] Voor de optie voor nulwaarden worden correct ingesteld is essentieel gebruiken `ISNULL` en niet `COALESCE`. `COALESCE`is niet een deterministic functie en het resultaat van de expressie wordt daarom altijd moet null-waarden bevatten. `ISNULL`is niet hetzelfde. Het is deterministic. Daarom wanneer het tweede deel van de `ISNULL` functie is een constante of een letterlijke waarde en vervolgens de resulterende waarde niet NULL.

Dit is niet alleen een handig voor zorgen dat de integriteit van uw berekeningen. Het is ook belangrijk is voor het overschakelen van tabel partition. Stel dat u in deze tabel die is gedefinieerd als uw faculteit hebt:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Het waardeveld is echter een berekende expressie die geen deel uitmaakt van de brongegevens.

Als u wilt maken van uw gepartitioneerde gegevensset u mogelijk wilt u dit wilt doen:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

De query wordt uitgevoerd geen probleem. Het probleem wordt geleverd bij het uitvoeren van de schakeloptie partition. De tabeldefinities komen niet overeen. Als u wilt maken van de tabeldefinities van de die overeenkomen met de CTAS moeten worden gewijzigd.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

U kunt daarom zien dat type consistentie en onderhouden van optie voor nulwaarden eigenschappen op een CTAS is een goede technische aanbevolen procedure. Deze helpt bij het voor het behoud van integriteit in uw berekeningen en ervoor zorgen dat het overschakelen van partition mogelijk is.

Raadpleeg MSDN voor meer informatie over het gebruik van [CTAS][]. Het is een van de belangrijkste beweringen in Azure SQL Data Warehouse. Zorg ervoor dat u deze zodat u weet.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
