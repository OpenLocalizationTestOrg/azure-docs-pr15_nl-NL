<properties
   pageTitle="Tabellen in SQL Data Warehouse distribueren | Microsoft Azure"
   description="Aan de slag met tabellen in Azure SQL Data Warehouse distribueren."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse distribueren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

SQL Data Warehouse is dat een massively parallelle verwerking (MPP) distributed databasesysteem.  Door gegevens delen en de mogelijkheid op verschillende knooppunten verwerkt, kan SQL Data Warehouse enorme schaalbaarheid - uiterst buiten een enkel systeem bieden.  Bepalen hoe uw gegevens in uw SQL Data Warehouse distribueren, is een van de belangrijkste factoren bij het bereiken van optimale prestaties.   De toets om optimale prestaties is minimaliseren verplaatsing van gegevens en de toets om te minimaliseren verplaatsing van gegevens selecteren op zijn beurt de juiste verdeling strategie.

## <a name="understanding-data-movement"></a>Informatie over de verplaatsing van gegevens

De gegevens van elke tabel is in een MPP-systeem verdeeld over de verschillende onderliggende databases.  De meest geoptimaliseerde query's op een MPP-systeem kunnen alleen worden doorgegeven via uitvoeren op de afzonderlijke verdeelde databases zonder interactie tussen de andere database.  Stel dat u hebt een database maken met verkoopgegevens bevat twee tabellen, verkoop en klanten.  Als u een query die vereist zijn voor uw omzettabel te koppelen aan uw klantentabel hebt en u uw verkoop- en de klantentabellen omhoog door klantnummer deelt, elke klant plaatsen in een afzonderlijke database, kunnen alle query's die deelnemen aan de verkoop- en klant worden opgelost in elke database met geen kennis van de andere databases.  In tegenstelling als u de verkoopgegevens gedeeld door het nummer en uw gegevens van de klant op klantnummer, klikt u vervolgens elke opgegeven database geen de bijbehorende gegevens voor elke klant en dus als u deelnemen aan de verkoopgegevens met de klantgegevens van de wilt, moet u de gegevens voor elke klant ophalen uit de andere databases.  In dit voorbeeld tweede moet verplaatsing van gegevens optreden als u wilt de gegevens van de klant verplaatsen naar de verkoopgegevens, zodat de twee tabellen kunnen worden gekoppeld.  

Verplaatsing van gegevens is niet altijd slecht, soms is het nodig zijn voor het oplossen van een query.  Maar wanneer deze extra stap kan worden voorkomen, natuurlijk uw query wordt uitgevoerd sneller.  Gegevens verplaatsen ontstaat meestal als tabellen zijn gekoppeld- of aggregaties worden uitgevoerd.  Vaak u moet doen beide worden weergegeven, dus terwijl u mogelijk optimaliseren voor één scenario, zoals een join, u nog steeds verplaatsing van de gegevens moet kunt oplossen voor de andere scenario, zoals een aggregatie.  De slag uitzoeken welke minder werk is.  In de meeste gevallen, voor het distribueren van grote feitentabellen op een kolom, meestal gekoppelde de meeste effectieve methode voor het beperken van de verplaatsing van de meeste gegevens is.  Gegevens op de join-kolommen distribueren is een veel meer gebruikte methode verkleinen van de verplaatsing van de gegevens dan het distribueren van gegevens in kolommen betrokken bij een aggregatie.

## <a name="select-distribution-method"></a>Verdelingsmethode selecteren

Uw gegevens worden in SQL Data Warehouse achter de schermen opgedeeld in 60 databases.  Elke afzonderlijke-database is een **verdeling**genoemd.  Wanneer u gegevens in elke tabel is geladen, heeft SQL Data Warehouse te weten hoe u het delen van uw gegevens over deze 60 onderzoeken.  

De verdeling wordt gedefinieerd op het tabelniveau van de en er zijn momenteel twee keuzes:

1. **Round robin** waarin gegevens distribueren gelijkmatig maar willekeurig.
2. **Hash Distributed** waarin gegevens op basis verdeelt van de waarden uit één kolom

Standaard wanneer u een methode verdeling van gegevens geen definieert de tabel verdeeld met de methode **round robin** -verdeling.  Echter naarmate u meer geavanceerde in uw implementatie, zult u **hash distributed** tabellen met de verplaatsing van de gegevens die op zijn beurt queryprestaties optimaliseert minimaliseren.

### <a name="round-robin-tables"></a>Round Robin tabellen

Met de methode Round Robin van het distribueren van gegevens is zeer veel hoe het klinkt.  Als uw gegevens worden geladen, wordt elke rij gewoon verzonden naar de volgende verdeling.  Deze methode van het distribueren van de gegevens worden altijd willekeurig de gegevens zeer gelijkmatig verdelen over alle de onderzoeken.  Er is geen sorteren klaar tijdens het round robin waarin uw gegevens geplaatst.  Een round robin distributie wordt soms een willekeurig hash daarom genoemd.  Is er geen nodig voor meer informatie over de gegevens met een round robin verdeelde tabel.  Daarom zorg Round Robin tabellen vaak voor goede laden doelen.

Standaard als geen enkele manier verdeling is gekozen, de methode round robin-verdeling gebruikt.  Round robin tabellen zijn eenvoudig te gebruiken, omdat de gegevens worden willekeurig gedistribueerd in het systeem betekent dat het systeem welke verdeling kan niet garanderen moet elke rij is echter op.  Hierdoor moet het systeem soms roepen een verplaatsing van de gegevens waarmee uw gegevens beter kunt organiseren voordat deze een query kunt oplossen.  Deze extra stap kan vertragen aan uw query's.

U kunt Round Robin distributie gebruiken voor uw tabel in de volgende scenario's:

- Wanneer aan de slag als eenvoudige uitgangspunt
- Als er geen duidelijke gekoppelde toets
- Als er is geen goede kolom voor hashbewerkingen distribueren van de tabel
- Als de tabel een gemeenschappelijke join-sleutel niet met andere tabellen deelt
- Als de join minder aanzienlijk dan andere joins in de query is
- Als de tabel is een tijdelijke tabel voor het tijdelijk opslaan

In deze voorbeelden wordt een tabel Round Robin maken:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Hoewel round robin tabel standaardtype expliciete in uw DDL wordt een aanbevolen is dat de bedoelingen van de tabelindeling van uw wissen aan anderen zijn.

### <a name="hash-distributed-tables"></a>Verdeelde tabellen hash

Met behulp van algoritme **Hash distributed** distributie van uw tabellen, kan de prestaties voor veel scenario's verbeteren door verplaatsing van gegevens op query moment.  Distributed hashtabellen zijn tabellen zijn verdeeld over de verdeelde databases met een hashing algoritme op een enkele kolom die u hebt geselecteerd.  De kolom verdeling, wordt bepaald hoe de gegevens over uw verdeelde databases wordt verdeeld.  De functie hash gebruikt de kolom verdeling rijen toewijzen aan onderzoeken.  De hashing algoritme en de resulterende verdeling is deterministic.  Dit is dezelfde waarde met hetzelfde gegevenstype wordt altijd heeft voor de dezelfde verdeling.    

In dit voorbeeld wordt een tabel verdeeld over id maken:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Selecteer kolom van de verdeling

Als u om **hash distribueren** een tabel besluit, moet u om een kolom Eén verdeling te selecteren.  Als u een kolom verdeling selecteert, zijn er drie belangrijke factoren u rekening moet houden.  

Selecteer een enkele kolom die wordt:

1. Niet bijgewerkt
2. Verdelen gegevens, gegevens scheefheid voorkomen
3. Minimaliseren verplaatsing van gegevens

### <a name="select-distribution-column-which-will-not-be-updated"></a>Selecteer verdeling kolom die niet worden bijgewerkt

Verdeling kolommen kunnen daarom niet worden bijgewerkt, selecteer een kolom met statische waarden.  Als een kolom worden bijgewerkt moeten, is het meestal niet een goede verdeling candidate.  Als er een zaak waarin u de kolom van een verdeling bijwerken is moet, kunt u dit doen door eerst de rij verwijderen en klikt u vervolgens een nieuwe rij invoegen.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Selecteer verdeling kolom waarin gegevens worden gelijkmatig verdelen

Aangezien een gedistribueerd systeem alleen zo snel als de laagst mogelijke verdeling uitvoert, is het belangrijk dat het verdeelt u het werk gelijkmatig over de onderzoeken om te kunnen bereiken gebalanceerde worden uitgevoerd in het systeem.  De manier waarop die het werk dat wordt gedeeld op een gedistribueerd systeem is gebaseerd op waar de gegevens voor elke verdeling zich bevindt.  Hierdoor is het belangrijk dat u selecteert u de kolom rechts verdeling om de gegevens te distribueren zodat elke verdeling gelijk werk heeft en is de dezelfde tijd om het deel van het werk te voltooien.  Wanneer werk wordt ook gedeeld in het systeem, worden de gegevens wordt verdeeld over de onderzoeken.  Wanneer u gegevens, wordt niet gelijkmatig verdeeld, verwijst naar deze **gegevens laten**.  

Houd rekening met het volgende als u gegevens gelijkmatig verdelen en voorkomen dat gegevens Scheefheid, bij het selecteren van de kolom van de verdeling:

1. Selecteer een kolom die een groot aantal unieke waarden bevat.
2. Vermijd het distribueren van gegevens voor kolommen met een paar unieke waarden. 
3. Voorkomen dat gegevens op kolommen met een hoge frequentie van null-waarden distribueren.
4. Voorkomen dat gegevens op datumkolommen distribueren.

Aangezien elke waarde is aan 1 van 60 onderzoeken hashing, als u wilt bereiken even verdeling zult u om een kolom die is ten zeerste uniek en bevat meer dan 60 unieke waarden te selecteren.  Om te illustreren, kunt u een zaak die een kolom waar slechts 40 unieke waarden heeft.  Als deze kolom is geselecteerd als de sleutel verdeling, zou de gegevens voor die tabel terechtkomt op 40 onderzoeken, maximaal 20 onderzoeken met zonder gegevens en geen verwerking moet verlaten.  De andere 40 onderzoeken moet daarentegen meer werk dat te doen als de gegevens is meer dan 60 onderzoeken gelijkmatig verdeeld.  Dit scenario is een voorbeeld van gegevens scheefheid.

Elke querystap wacht in MPP-systeem wordt voor alle onderzoeken hun delen van het werk voltooid.  Als u één verdeling is meer werk dan de anderen doen, klikt u vervolgens de resource van de andere onderzoeken zijn in principe verspild NET wachten op de bezet verdeling.  Wanneer het werk is niet gelijkmatig verdeeld over alle onderzoeken, verwijst naar deze **scheefheid verwerken**.  Verwerking scheefheid treedt query's worden uitgevoerd langer duren dan als de werkbelasting kunt worden gelijkmatig verdeeld over de onderzoeken.  Gegevens scheefheid leidt tot verwerking scheefheid.

Voorkom distribueren uiterst kolom, zoals het null-waarden worden terechtkomt op de dezelfde verdeling. Distribueren op een datumkolom ophalen kunt ook de oorzaak verwerking scheefheid omdat alle gegevens voor een bepaalde datum wordt terechtkomt op de dezelfde verdeling. Als u meerdere gebruikers zijn query's uitvoeren alle filteren op dezelfde datum, en vervolgens alleen 1 van de 60 onderzoeken gaan doen al het werk sinds een bepaalde datum wordt alleen deel uitmaken van een verdeling. In dit scenario wordt de query's waarschijnlijk 60 keer langzamer dan als de gegevens gelijkmatig zijn verdeeld over alle de onderzoeken uitgevoerd. 

Wanneer u geen goede kolommen bestaat, klikt u vervolgens kunt u overwegen round robin als de verdelingsmethode.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Selecteer verdeling kolom te minimaliseren verplaatsing van gegevens

Gegevens verplaatsen door te selecteren van de kolom rechts verdeling minimaliseren, is een van de belangrijkste strategieën voor het optimaliseren van de prestaties van uw SQL Data Warehouse.  Gegevens verplaatsen ontstaat meestal als tabellen zijn gekoppeld- of aggregaties worden uitgevoerd.  Kolommen die worden gebruikt `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` en `HAVING` componenten alle zorg voor **goede** hash-verdeling vreemde valuta. 

Op de andere hand, kolommen in de `WHERE` component Doe **niet** zorg voor goede hash-kolom kandidaten omdat ze beperken welke onderzoeken deelnemen aan de query, de oorzaak van het verwerking laten.  Een goed voorbeeld van een kolom die mogelijk verleidelijk om te distribueren naar, maar vaak kan leiden tot deze scheefheid verwerking is een datumkolom ophalen.

In het algemeen, hebt u twee grote feitentabellen vaak betrokken in een join, krijgt u de meeste prestaties door beide tabellen in een van de join-kolommen verdelen.  Als u een tabel die deel nooit naar een andere grote feitentabel uitmaakt hebt, kijkt u naar de kolommen die vaak in de `GROUP BY` component.

Er zijn enkele belangrijke criteria die moeten worden voldaan om te voorkomen dat gegevens verplaatsen tijdens een join:

1. De tabellen verbindt waarop in de join moet hash distributed op **één** van de kolommen die deelneemt aan de join definieert.
2. De gegevenstypen van de join-kolommen moeten overeenkomen met tussen beide tabellen.
3. De kolommen moeten worden gekoppeld met een operator is gelijk aan.
4. Het jointype mogelijk niet een `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Analytische gegevens scheefheid

Wanneer gegevens in een tabel is verdeeld met de methode van de verdeling hash bestaat de kans dat sommige onderzoeken worden vervormd als u niet goed meer gegevens dan andere. Overtollige gegevens scheefheid kan invloed hebben op prestaties van query's omdat het uiteindelijke resultaat van een gedistribueerde query moet wachten tot de langste actieve verdeling om te voltooien. Afhankelijk van de mate van de scheefheid van de gegevens moet u mogelijk dit adres.

### <a name="identifying-skew"></a>Scheefheid identificeren

Een eenvoudige manier om aan te geven van een tabel, zoals vervormd is via `DBCC PDW_SHOWSPACEUSED`.  Dit is een zeer snelle en eenvoudige manier om het aantal rijen in de tabel die zijn opgeslagen in elk van de 60 onderzoeken van uw database weer te geven.  Vergeet niet dat voor de meest gebalanceerde prestaties, de rijen in een gedistribueerde moeten gelijkmatig worden verdeeld over alle onderzoeken.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Als u een query uitvoeren op de Azure SQL Data Warehouse dynamische management weergaven (DMV) kunt u echter een meer gedetailleerde analyse uitvoeren.  Als u wilt starten, de weergave [dbo.vTableSizes][] weergave maken met behulp de SQL van artikel over [Tabel overzicht][Overzicht] .  Nadat de weergave is gemaakt, kunt u deze query om te bepalen welke tabellen hebt meer dan 10% gegevens scheefheid uitvoeren.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Gegevens scheefheid oplossen

Niet alle scheefheid nog een oplossing garandeert.  In sommige gevallen kan de prestaties van een tabel in sommige query's wegen tegen de schade scheefheid gegevens.  Om te bepalen als u gegevens moet oplossen scheefheid in een tabel, moet u weten zoveel mogelijk informatie over de gegevensvolumes en query's in uw werkzaamheden.   Eén manier nagaan wat de invloed van scheefheid is via de stappen in het artikel [Query controleren][] om de wat de invloed van scheefheid op queryprestaties en specifiek de invloed op hoe lang query's uitvoeren om uit te voeren op de afzonderlijke onderzoeken te houden.

Distribueren van gegevens is een kwestie van de juiste verhouding tussen gegevens scheefheid minimaliseren en minimaliseren verplaatsing van gegevens te zoeken. Deze tegengestelde kunnen doelstellingen en soms wilt u gegevens scheefheid houden om te kunnen verplaatsing van gegevens beperken. Bijvoorbeeld wanneer de kolom verdeling vaak de gedeelde kolom in joins en aggregaties is, wordt u worden minimaliseren verplaatsing van gegevens. Het voordeel van de verplaatsing van de minimale gegevens die mogelijk wegen tegen de invloed van gegevens laten. 

De normale manier voor het oplossen van gegevens scheefheid is opnieuw maken in de tabel met de kolom van een andere verdeling. Aangezien er is geen manier om te wijzigen van de verdeling kolom aan een bestaande tabel, de manier om te wijzigen van de verdeling van een tabel deze opnieuw te maken met een [CTAS]-[].  Hier volgen twee voorbeelden van de manier waarop gegevens scheefheid oplossen:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Voorbeeld 1: Maak de tabel met een nieuwe kolom van de verdeling

Dit voorbeeld wordt [CTAS]-[] om een tabel met een ander hash-verdeling kolom opnieuw te maken. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Voorbeeld 2: De tabel met round robin distributie opnieuw maken

In dit voorbeeld wordt [CTAS]-[] gebruikt om een tabel met round robin in plaats van een verdeling hash opnieuw te maken. Deze wijziging zullen produceren zelfs gegevens verdeling maar oplevert verplaatsing van betere gegevens. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Volgende stappen

Zie meer informatie over tabellen-ontwerpen, [verdelen][], [Index][], [Partition][], [Gegevenstypen][], [Statistieken][] en [Tijdelijke tabellen][tijdelijke] artikelen.

Zie [Aanbevolen procedures voor Warehouse SQL-gegevens][]voor een overzicht van de aanbevolen procedures.


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
[Query controleren]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
