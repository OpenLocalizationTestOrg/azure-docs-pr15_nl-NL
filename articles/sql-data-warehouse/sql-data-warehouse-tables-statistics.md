<properties
   pageTitle="Beheer van statistieken over tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met statistieken over tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Statistieken over tabellen in SQL Data Warehouse beheren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

Hoe meer SQL Data Warehouse op de hoogte van uw gegevens, des te sneller query's op uw gegevens kunnen worden uitgevoerd.  De manier waarop dat u SQL Data Warehouse over uw gegevens gebruikt informeren, is door het verzamelen van statistieken over uw gegevens.  Statistieken op uw gegevens hebt, is een van de belangrijkste dingen die u doen kunt om het optimaliseren van uw query's.  Gegevens kunnen SQL Data Warehouse het meest optimale plan voor query's maken.  Dit is omdat het optimaliseren van de query SQL Data Warehouse een optimizer kosten die zijn gebaseerd is.  Dat wil zeggen verschilt van de kosten van verschillende query-abonnementen en kiest vervolgens het abonnement met de laagste kosten, die het abonnement die wordt uitgevoerd de snelste ook moet worden.

Statistieken kan worden gemaakt van één kolom, meerdere kolommen of in een index van een tabel.  Statistieken worden opgeslagen in een histogram waarin wordt vastgelegd van het bereik en selectiviteit van waarden.  Dit is met name van belang wanneer de optimizer, moet worden geëvalueerd JOINs, GROUP BY, HAVING en WHERE-componenten in een query.  Bijvoorbeeld als de optimizer maakt een schatting van dat de datum die u in uw query filtert retourneert 1 rij, kies een sterk afwijkt van plan bent dan als deze optie maakt een schatting dat ze datum u hebt geselecteerd is 1 miljoen rijen worden geretourneerd.  Tijdens het maken van statistieken erg belangrijk is, is het belangrijk dat statistieken *nauwkeurig* weergeven de huidige status van de tabel.  Met actuele statistieken zorgt ervoor dat het een goed plan is geselecteerd door de optimizer.  De abonnementen die zijn gemaakt door de optimizer zijn alleen nuttig als de statistieken op uw gegevens.

Het proces van het maken en bijwerken van statistieken is momenteel een handmatig proces, maar is heel eenvoudig te doen.  Dit is anders dan bij SQL Server die automatisch wordt gemaakt en bijgewerkt statistieken over één kolommen en indexen.  Met behulp van de onderstaande informatie, kunt u het beheer van de statistieken enorm automatiseren van uw gegevens. 

## <a name="getting-started-with-statistics"></a>Aan de slag met statistieken

 Maken van de steekproef statistieken voor elke kolom is een eenvoudige manier om aan de slag met statistieken.  Aangezien het net zo belangrijk statistieken om up-to-date te houden, mogelijk voorzichtig bij uw statistieken dagelijks of na elke laden. Er zijn altijd voor-en nadelen tussen prestaties en de kosten maken en bijwerken van statistieken.  Als u dat het duurt te lang voor het behoud van al uw statistieken vindt, wilt u mogelijk wil nauwkeuriger welke kolommen hebben statistieken of welke kolommen regelmatig hoeft te worden bijgewerkt.  U wilt bijvoorbeeld datumkolommen bijwerken dagelijks nieuwe waarden kunnen worden toegevoegd in plaats van na elke laden. Nogmaals, krijgt u optimaal wilt profiteren doordat statistieken van kolommen die nodig zijn voor het in JOINs, GROUP BY, HAVING en WHERE-componenten.  Hebt u een tabel met een groot aantal kolommen die alleen worden gebruikt in de SELECT-component, statistieken over deze kolommen kunt niet en iets besteden, kan meer inspanning alleen de kolommen waar statistieken helpt, identificeren beperken de tijd aan uw statistieken onderhouden.

## <a name="multi-column-statistics"></a>Meerdere kolommen statistieken

Naast het statistieken op één kolommen maakt, kan het gebeuren dat uw query's van meerdere kolommen statistieken profiteren.  Meerdere kolommen statistieken worden gemaakt in een lijst met kolommen statistieken.  De opties omvatten één kolom statistieken over de eerste kolom in de lijst, plus meer informatie over meerdere kolommen correlatie densiteit genoemd.  Bijvoorbeeld als u een tabel die met elkaar op twee kolommen verbindt hebt, vindt u mogelijk dat SQL Data Warehouse beter het abonnement optimaliseren kunt wordt de relatie tussen twee kolommen begrepen.   Meerdere kolommen statistieken kunt verbeteren de prestaties van query's voor bepaalde bewerkingen zoals samengestelde joins en groeperen op.

## <a name="updating-statistics"></a>Statistieken bijwerken

Statistieken bij te werken is een belangrijk onderdeel van je database management opgeslagen.  Wijzigingen in de verdeling van gegevens in de database moeten statistieken worden bijgewerkt.  Verouderde statistieken leidt tot sub optimale queryprestaties.

Een aanbevolen procedure is statistieken moeten worden bijgewerkt op datumkolommen elke dag wanneer nieuwe datums worden toegevoegd.  Elke keer nieuwe rijen in het datawarehouse zijn geladen, worden nieuwe laden datums of transactiedatums toegevoegd. Deze wijzigen van de verdeling van gegevens en breng de statistieken verouderde. Daarentegen statistieken over een kolom land in een klantentabel nooit moet mogelijk worden bijgewerkt, zoals de distributie van de waarden in het algemeen niet wijzigen. Ervan uitgaande dat de verdeling constante tussen klanten, is niet nieuwe rijen toevoegen aan de variatie in tabel wilt wijzigen van de verdeling van gegevens. Echter als uw BTW-records voor de gegevens alleen een land bevat en u van gegevens uit een nieuw land gebruikmaken, moet waardoor de gegevens uit meerdere landen wordt opgeslagen, klikt u vervolgens groeiende u statistieken op de kolom land.

Een van de eerste vragen om te vragen bij het oplossen van een query is, "Komen de statistieken bijgewerkt?"

Deze vraag is niet een waarde die door de leeftijd van de gegevens kunnen worden beantwoord. Een object up-to-date statistieken mogelijk zeer oude als er geen materiaalresource wijziging van de onderliggende gegevens is. Wanneer het aantal rijen ingrijpend gewijzigd of er is een materiaalresource wijziging in de distributie van de waarden voor een bepaald kolom *vervolgens* dan wordt het tijd statistieken moeten worden bijgewerkt.  

Voor een verwijzing naar **SQL Server** (niet SQL Data Warehouse) automatisch wordt bijgewerkt statistieken in deze situaties:

- Als u nul rijen in de tabel hebt wanneer u rijen toevoegt, krijgt u een automatische update van statistieken
- Wanneer u meer dan 500 rijen toevoegt aan een tabel die beginnen met minder dan 500 rijen (bijvoorbeeld aan het begin u 499 en 500 rijen toevoegen aan een totaal van 999 rijen), krijgt u een automatische update 
- Als u meer dan 500 rijen die u 500 extra rijen + 20% van de grootte van de tabel toevoegen moet voordat u een automatische update op de Stat ziet

Aangezien er geen DMV om te bepalen is of de gegevens in de tabel heeft gewijzigd sinds de laatste keer statistieken zijn bijgewerkt, kunt weten de leeftijd van uw statistieken bieden u waarin een gedeelte van de afbeelding.  U kunt de volgende query gebruiken om te bepalen de laatste keer dat uw statistieken waar bijgewerkt op elke tabel.  

> [AZURE.NOTE] Onthoud dat als er een materiaalresource wijziging in de distributie van de waarden voor een bepaalde kolom, moet u statistieken ongeacht de laatste keer dat ze zijn bijgewerkt bijwerken.  

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datumkolommen in een data-warehouse, bijvoorbeeld meestal regelmatig hoeft te worden statistieken updates. Elke keer nieuwe rijen in het datawarehouse zijn geladen, worden nieuwe laden datums of transactiedatums toegevoegd. Deze wijzigen van de verdeling van gegevens en breng de statistieken verouderde.  Daarentegen statistieken over geslacht kolom in een klantentabel nooit moet mogelijk worden bijgewerkt. Ervan uitgaande dat de verdeling constant tussen klanten, is niet nieuwe rijen toevoegen aan de variatie in tabel wilt wijzigen van de verdeling van gegevens. Echter als uw BTW-records voor de gegevens alleen één geslacht en een nieuwe vereiste resultaten in meerdere geslachten bevat moet groeiende u statistieken op de kolom geslacht moeten worden bijgewerkt.

Zie [Statistieken][] op MSDN voor nadere uitleg.

## <a name="implementing-statistics-management"></a>Statistieken management implementeren

Dit is het meestal een goed idee om uw gegevens laden om ervoor te zorgen dat statistieken worden bijgewerkt aan het einde van het selectievakje laden uitbreiden. De gegevens zijn geladen is wanneer u tabellen meest hun grootte en/of de distributie van de waarden wijzigen. Daarom is dit een logische plaats willen implementeren sommige processen.

Sommige principes hieronder vermeld voor het bijwerken van uw statistieken tijdens het laden:

- Ervoor zorgen dat elke tabel geladen ten minste één statistieken object bijgewerkt. Hiermee wordt de informatie over de grootte (aantal rijen en aantal pagina's) van tabellen als onderdeel van de update stat bijgewerkt.
- Kolommen die deel uitmaken JOIN, GROUP BY, sorteren en DISTINCT componenten te belichten
- Houd rekening met kolommen "oplopend toets" zoals transactie datums vaker terwijl deze waarden niet in het histogram statistieken opgenomen worden bijwerken.
- Houd rekening met statische verdeling kolommen, minder regelmatig bijwerken.
- Onthoud dat elk object statistische wordt bijgewerkt in de reeks. Gewoon implementeren `UPDATE STATISTICS <TABLE_NAME>` mogelijk niet ideaal - met name voor breed tabellen met een groot aantal statistieken objecten.

> [AZURE.NOTE] Raadpleeg de SQL Server-2014 verhouding schatting model whitepaper voor meer informatie over [oplopend toets].

Zie [Verhouding raming][] op MSDN voor nadere uitleg.

## <a name="examples-create-statistics"></a>Voorbeelden: Statistieken maken

Deze voorbeelden wordt getoond hoe u verschillende opties voor het maken van statistieken gebruiken. De opties die u voor elke kolom gebruikt, hangt af van de kenmerken van uw gegevens en hoe de kolom in query's worden gebruikt.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Statistieken van één kolom met standaardopties maken

Als u wilt statistieken maken voor een kolom, bieden u gewoon een naam voor het object statistieken en de naam van de kolom.

Deze syntaxis gebruikt alle de standaardopties. Standaard voorbeelden SQL Data Warehouse 20 procent van de tabel bij het maken van statistieken.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B TE DRUKKEN. Statistieken van één kolom maken aan de hand van elke rij

Het standaardtarief van de steekproeven van 20 procent is voldoende voor de meeste gevallen. U kunt echter het tarief weer dat steekproeven aanpassen.

Als u wilt een voorbeeld van de volledige tabel, gebruik de volgende syntaxis:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Maken van één kolom statistieken door het opgeven van de grootte van de steekproef

U kunt ook de grootte van de steekproef opgeven als een percentage:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D TE DRUKKEN. Één kolom statistieken maken voor slechts enkele van de rijen

Een andere optie, kunt u statistieken op een gedeelte van de rijen in de tabel. Dit is een gefilterde toetsingsgrootheid genoemd.

U kunt bijvoorbeeld gefilterde statistieken gebruiken als u van plan bent om query's in een specifieke partition van een grote gepartitioneerde tabel. Statistieken op alleen de waarden partition maakt, wordt de nauwkeurigheid van de statistieken verbeteren en daarom verbeteren de queryprestaties van de.

In dit voorbeeld wordt statistieken gemaakt op een bereik van waarden. De waarden kunnen eenvoudig worden gedefinieerd zodat deze overeenkomen met het bereik van waarden in een partition.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] Voor het optimaliseren van de query overwegen gefilterde statistieken wanneer deze het abonnement gedistribueerde query kiest, moet de query in de definitie van het object statistieken passend maken. Volgens het vorige voorbeeld, van de query waar-component moet Kol1 waarden tussen 2000101 en 20001231 opgeven.

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Maken van één kolom statistieken met alle opties

U kunt de opties natuurlijk samen combineren. Het volgende voorbeeld wordt een gefilterde statistieken-object gemaakt met de grootte van een aangepaste steekproef:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Zie [Statistieken maken][] op MSDN voor de volledige verwijzing.

### <a name="f-create-multi-column-statistics"></a>F. Meerdere kolommen statistieken maken

Als u wilt maken van een statistieken met meerdere kolommen, gewoon gebruiken de voorgaande Column, maar meer kolommen opgeven.

> [AZURE.NOTE] Het histogram, die wordt gebruikt voor het aantal rijen in het queryresultaat schatten, is alleen beschikbaar voor de eerste kolom weergegeven in de definitie van de object statistieken.

In dit voorbeeld wordt het histogram is ingeschakeld *product\_categorie*. Meerdere kolommen statistieken worden berekend op *product\_categorie* en *product\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Omdat er een relatie tussen *product\_categorie* en *product\_sub\_categorie*, een stat met meerdere kolommen is handig als deze kolommen tegelijkertijd worden geraadpleegd.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G Statistieken maken voor alle kolommen in een tabel

Een manier om statistieken te maken is problemen CREATE STATISTICS opdrachten na het maken van de tabel.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Een opgeslagen procedure gebruiken om u te statistieken maken voor alle kolommen in een database

SQL Data Warehouse beschikt niet over van een systeem opgeslagen procedure gelijk aan [sp_create_stats]-[] in SQL Server. Deze opgeslagen procedure Hiermee maakt u een object met één kolom statistieken aan elke kolom van de database die statistieken nog geen.

Hierdoor kunt u aan de slag met het databaseontwerp van uw. Je mag rustig aan uw wensen aanpassen.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Als u wilt statistieken maken voor alle kolommen in de tabel met deze procedure, belt u gewoon de procedure.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Voorbeelden: update statistieken

Als u wilt bijwerken statistieken, kunt u het volgende doen:

1. Object met één statistieken worden bijgewerkt. Geef de naam van het statistieken object dat u wilt bijwerken.
2. Alle statistieken objecten op een tabel bijwerken. Geef de naam van de tabel in plaats van één specifieke statistieken object.


### <a name="a-update-one-specific-statistics-object"></a>A. Een specifieke statistieken object bijwerken ###
Gebruik de volgende syntaxis een specifieke statistieken-object wilt bijwerken:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Specifieke statistieken objecten bijwerkt, kunt u de tijd en de resources die zijn vereist voor het beheren van statistieken minimaliseren. U moet hiervoor voorzichtig, echter om de beste statistieken objecten bijwerken te kiezen.


### <a name="b-update-all-statistics-on-a-table"></a>B TE DRUKKEN. Alle statistieken op een tabel ###
Hierdoor wordt een eenvoudige methode voor het bijwerken van alle statistieken objecten op een tabel.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS dbo.table1;
```

Deze instructie is eenvoudig te gebruiken. Onthoud dat dit alle statistieken op de tabel wordt bijgewerkt en daarom mogelijk inzetten meer dan nodig is. Als de prestaties een probleem is, maar dit is groeiende de eenvoudigste en meest volledige manier om te garanderen statistieken zijn bijgewerkt.

> [AZURE.NOTE] Wanneer alle statistieken op een tabel wordt bijgewerkt, gebeurt SQL Data Warehouse een scan steekproef in de tabel voor elke statistieken. Als de tabel te groot is, heeft vele kolommen en meer statistische gegevens, kan het zijn efficiënter afzonderlijke statistieken op basis van nodig moeten worden bijgewerkt.

Voor een implementatie van een `UPDATE STATISTICS` procedure raadpleegt u het[tijdelijke] artikel [Tijdelijke tabellen]. De implementatie-methode verschilt enigszins naar de `CREATE STATISTICS` bovenstaande procedure, maar het uiteindelijke resultaat is hetzelfde.

Zie [Update Statistics][] op MSDN voor de volledige syntaxis.

## <a name="statistics-metadata"></a>Statistieken metagegevens
Er zijn verschillende systeemweergave en functies die u gebruiken kunt om informatie over statistische gegevens te vinden. U kunt bijvoorbeeld zien als een object statistieken mogelijk verouderd met behulp van de functie stat-datum moeten zien wanneer statistieken laatst zijn gemaakt of bijgewerkt.

### <a name="catalog-views-for-statistics"></a>Weergaven voor statistieken catalogus
Deze systeemweergaven bevatten informatie over statistieken:

| Catalogusweergave | Beschrijving |
| :----------- | :---------- |
| [sys.Columns][]  | Een rij voor elke kolom. |
| [sys.Objects][]  | Een rij voor elk object in de database. |  |
| [sys.schemas][]  | Een rij voor elke schema in de database. |  |
| [sys.Stats][] | Een rij voor elk object statistieken. |
| [sys.stats_columns][] | Een rij voor elke kolom in het object statistieken. Een koppeling terug naar sys.columns. |
| [systeemtabellen][] | Een rij voor elke tabel (inclusief externe tabellen). |
| [sys.table_types][] | Een rij voor elk gegevenstype. |


### <a name="system-functions-for-statistics"></a>Systeemfuncties voor statistieken
Deze systeemfuncties zijn handig voor het werken met statistieken:

| Systeemfunctie | Beschrijving |
| :-------------- | :---------- |
| [STATS_DATE][]    | Datum die het object statistieken voor het laatst werd bijgewerkt. |
| [DBCC SHOW_STATISTICS][] | Biedt samenvatting niveau en gedetailleerde informatie over de distributie van de waarden, zoals begrepen door het object statistieken. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Functies en statistieken kolommen combineren in één weergave

Deze weergave kunt u kolommen die betrekking op statistieken hebben en resultaat is van de functie van [STATS_DATE()] [] samen.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Voorbeelden van DBCC SHOW_STATISTICS()

DBCC SHOW_STATISTICS() ziet u de gegevens in een object statistieken gehouden. Deze gegevens bestaat uit drie delen.

1. Koptekst
2. Dichtheid Vector
3. Histogram

De metagegevens over de statistieken voor de koptekst. Het histogram geeft de distributie van de waarden in de eerste kolom van de sleutel van het object statistieken. De dichtheid vector maatregelen cross-kolom correlatie. SQLDW berekent verhouding maakt een schatting met een van de gegevens in het object statistieken.

### <a name="show-header-density-and-histogram"></a>Koptekst, dichtheid en histogram weergeven

Dit eenvoudige voorbeeld ziet alle drie onderdelen van een object statistieken.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Laten zien hoe een of meer delen van DBCC SHOW_STATISTICS();

Als u alleen geïnteresseerd bent in bepaalde delen weergeven, gebruikt u de `WITH` component en opgeven welke onderdelen die u wilt zien:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() verschillen
DBCC SHOW_STATISTICS() is meer strikt geïmplementeerd in SQL Data Warehouse vergeleken met SQL Server.

1. Niet-gedocumenteerde functies worden niet ondersteund
- Stats_stream niet gebruiken
- Resultaten voor specifieke deelverzamelingen van statistiekgegevens bijvoorbeeld kan niet deelnemen (DENSITY_VECTOR STAT_HEADER JOIN)
2. NO_INFOMSGS kunnen niet worden ingesteld voor bericht onderdrukken
3. Vierkante haken rond statistieken namen kan niet worden gebruikt
4. Kolomnamen niet gebruiken om te identificeren statistieken objecten
5. Aangepaste fout 2767 wordt niet ondersteund

## <a name="next-steps"></a>Volgende stappen

Zie [DBCC SHOW_STATISTICS][] op MSDN voor meer informatie.  Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [indexeren van een tabel],[Index], [partitioneren van een tabel][Partition] en[tijdelijke] [Tijdelijke tabellen]meer informatie.  Zie voor meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].  

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
[Verhouding schatting]: https://msdn.microsoft.com/library/dn600374.aspx
[STATISTIEKEN MAKEN]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistieken]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.Columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.Objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.Stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[systeemtabellen]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[STATISTIEKEN BIJWERKEN]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
