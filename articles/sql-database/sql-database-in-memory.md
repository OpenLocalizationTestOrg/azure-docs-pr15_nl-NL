<properties
    pageTitle="SQL In het geheugen, aan de slag | Microsoft Azure"
    description="SQL In het geheugen technologieën verbeteren aanzienlijk de prestaties van transacties en analyses werkbelasting. Leer hoe u deze mogelijkheden."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Aan de slag met In het geheugen (Preview) in SQL-Database

In het geheugen functies verbeteren aanzienlijk de prestaties van transacties en analyses werkbelasting in de juiste situaties.

In dit onderwerp wordt benadrukt twee demonstraties, één voor In het geheugen OLTP en één van In het geheugen analysegegevens. Elke demo wordt geleverd met de stappen en de code die u moet uitvoeren van de video. Kunt u:

- Gebruik van de code om te testen variaties als u wilt zien van de verschillen tussen de prestatieresultaten; of
- Lees de code voor meer informatie over het scenario en om te zien hoe te maken en de objecten In het geheugen.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Snel starten 1: In het geheugen OLTP technologieën voor sneller T-SQL prestaties](http://msdn.microsoft.com/library/mt694156.aspx) -is een ander artikel om u aan de slag te helpen.

#### <a name="in-memory-oltp"></a>In het geheugen OLTP

Zijn de functies van In het geheugen [OLTP](#install_oltp_manuallink) (online transactieverwerking):

- Geheugen geoptimaliseerde tabellen.
- Standaard gecompileerd opgeslagen procedures.


Een tabel geheugen geoptimaliseerde bevat één weergave van zelf in het actieve geheugen, naast de gewone weergave op een harde schijf. Zakelijke transacties ten opzichte van de tabel sneller worden uitgevoerd omdat ze rechtstreeks in combinatie met alleen de weergave die zich in het actieve geheugen.

Met In het geheugen OLTP, kunt u omhoog bereiken 30 tijden in transactiedoorvoer, afhankelijk van de details van de werklast krijgen.


Standaard gecompileerd opgeslagen procedures vereisen minder machine-instructies tijdens runtime dan traditionele geïnterpreteerd opgeslagen procedures. We hebben systeemeigen gecompileerd resultaat gezien in duur die zijn van 1/100th van de geïnterpreteerd duur.


#### <a name="in-memory-analytics"></a>In het geheugen Analytics 

De functie van In het geheugen [Analytics](#install_analytics_manuallink) luidt als volgt:

Columnstore indexen verbeteren de prestaties van analyses en rapportage van query's. 


#### <a name="real-time-analytics"></a>Realtime Analytics

Voor [Real-Time Analytics](http://msdn.microsoft.com/library/dn817827.aspx) combineren u In het geheugen OLTP en analyses om te gaan:

- Realtime zakelijk inzicht op basis van operationele gegevens.


#### <a name="availability"></a>Beschikbaarheid


GA, algemene beschikbaarheid:

- [Columnstore indexen](http://msdn.microsoft.com/library/dn817827.aspx) die zijn *op schijf*.


Voorbeeld:

- In het geheugen OLTP
- Realtime operationele Analytics


Overwegingen terwijl de functies In het geheugen in de Preview-versie zijn beschreven [verderop in dit onderwerp](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Deze functies zijn alleen beschikbaar voor [*Premium*](sql-database-service-tiers.md) -databases niet in elastische van toepassingen en niet beschikbaar voor alle databases Basic of standaard.  Ondersteuning voor Premium-databases in elastische toepassingen is binnenkort beschikbaar. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>A. Het voorbeeld In het geheugen OLTP installeren

U kunt de voorbeelddatabase AdventureWorksLT [V12] maken door een paar muisklikken in de [portal van Azure](https://portal.azure.com/). Vervolgens de stappen in deze sectie wordt uitgelegd hoe u de database AdventureWorksLT met kan verrijken:

- In het geheugen tabellen.
- Een ingebouwde gecompileerd opgeslagen procedure.


#### <a name="installation-steps"></a>Installatiestappen

1. Klik in de [portal van Azure](https://portal.azure.com/)door een Premium-database op een server V12 te maken. Stel de **bron** voor de voorbeelddatabase AdventureWorksLT [V12].
 - Voor gedetailleerde instructies ziet u [uw eerste Azure SQL-database maken](sql-database-get-started.md).

2. Verbinding maken met de database maken met SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Het [script In het geheugen OLTP Transact-SQL](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) naar het Klembord kopiëren.
 - De T-SQL-script Hiermee maakt u de benodigde In het geheugen-objecten in de AdventureWorksLT voorbeelddatabase die u in stap 1 hebt gemaakt.

4. De T-SQL-script in SSMS plakken en vervolgens het script uitvoeren.
 - Essentieel is de `MEMORY_OPTIMIZED = ON` component CREATE TABLE-instructies, als in:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fout 40536


Als u de fout 40536 krijgt wanneer u de T-SQL-script uitvoeren, voert u het volgende T-SQL-script om te verifiëren of de database In het geheugen ondersteunt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


**0** als resultaat betekent In het geheugen wordt niet ondersteund en 1 dat wordt ondersteund. Het probleem opsporen:

- Controleer of dat de database is gemaakt nadat de functies In het geheugen OLTP actief is geworden voor Preview.
- Controleer of de database in de Premium-servicelaag.


#### <a name="about-the-created-memory-optimized-items"></a>Over de gemaakte geheugen geoptimaliseerde items

**Tabellen**: het voorbeeld bevat de volgende geheugen geoptimaliseerde tabellen:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


U kunt tabellen geheugen geoptimaliseerde via het **Object Explorer** in SSMS door te controleren:

- Met de rechtermuisknop op **tabellen** > **Filter** > **Filterinstellingen** > **Is geheugen geoptimaliseerd** gelijk is aan 1.


Of u kunt de catalogusweergaven, zoals query:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Standaard gecompileerd opgeslagen procedure**: SalesLT.usp_InsertSalesOrder_inmem kan worden uitgevoerd via een catalogus weergave query:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>De werklast van de OLTP steekproef uitvoeren

Het enige verschil tussen de volgende twee *opgeslagen procedures* is dat de eerste procedure geheugen geoptimaliseerde versies van de tabellen wordt, terwijl de tweede procedure wordt de normale op schijf-tabellen:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


In deze sectie ziet u hoe u met het hulpprogramma handige **ostress.exe** uitvoeren van de twee opgeslagen procedures op stress niveaus. U kunt vergelijken hoe lang duurt de twee belasting wordt uitgevoerd om te voltooien.


Wanneer u ostress.exe uitvoert, wordt u aangeraden dat u parameterwaarden die zijn ontworpen voor beide doorgeven:

- Een groot aantal verbindingen, worden uitgevoerd via - n100.
- Voor elke verbinding lus honderden vaak, hebben met de parameter - r500.


Echter u mogelijk wilt laten beginnen met veel kleinere waarden zoals - n10 en - r50 om ervoor te zorgen dat het alles werkt.


### <a name="script-for-ostressexe"></a>Script voor ostress.exe


Deze sectie wordt de T-SQL-script dat is ingesloten in de opdrachtregel van onze ostress.exe weergegeven. Het script wordt gebruikt voor items die zijn gemaakt door de T-SQL-script die u eerder hebt geïnstalleerd.


Het volgende script voegt de verkoop volgorde van een steekproef met vijf lijnartikelen in de volgende geheugen geoptimaliseerde- *tabellen*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Als u de _ondisk-versie van de voorgaande T-SQL voor ostress.exe, zou u gewoon beide exemplaren van de subtekenreeks *_inmem* vervangen door *_ondisk*. Deze vervangt invloed op de namen van tabellen en opgeslagen procedures.


### <a name="install-rml-utilities-and-ostress"></a>Installeer RML hulpprogramma's en ostress


In het ideale geval zou u van plan bent ostress.exe uitvoeren op een VM Azure. U zou een [Azure virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/) maken in de dezelfde Azure geografische regio waar uw AdventureWorksLT database zich bevindt. Maar u kunt ostress.exe uitvoeren op uw laptop in plaats daarvan.


Op de VM of op wat u hosten kiest, wordt de opnieuw afspelen Markup Language (RML) hulpprogramma's, waaronder ostress.exe installeert.

- Zie het onderwerp ostress.exe in [Voorbeelddatabase voor In het geheugen OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
 - Of Lees dit [Voorbeelddatabase voor In het geheugen OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
 - Of Lees dit [Blog voor het installeren van ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>De werklast van de belasting _inmem eerst uitvoeren


U kunt een *RML Cmd* -venster onze opdrachtregel ostress.exe uitvoeren. De opdrachtregelparameters directe ostress naar:

- 100 verbindingen tegelijkertijd worden uitgevoerd (-n100).
- Elke verbinding 50 keer van de T-SQL-script uitvoeren hebt (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


De voorgaande ostress.exe-opdrachtregel uitvoeren:


1. De inhoud van de gegevens database door de volgende opdracht uit te voeren in SSMS, verwijdert u alle gegevens die is ingevoegd met een eerder uitgevoerde herstellen:
```
EXECUTE Demo.usp_DemoReset;
```

2. De tekst van de voorgaande ostress.exe-opdrachtregel naar het Klembord kopiëren.

3. Vervang de `<placeholders>` voor de parameters -S - U -P -d met de juiste reële waarden.

4. Uw bewerkte opdrachtregel worden uitgevoerd in een RML Cmd-venster.


#### <a name="result-is-a-duration"></a>Resultaat is een duur


Als ostress.exe is voltooid, worden de uitvoeren duur als de laatste regel van uitvoer in het venster RML Cmd geschreven. Bijvoorbeeld in een kortere test uitvoeren ongeveer 1,5 minuten heeft geduurd:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Opnieuw instellen, voor _ondisk bewerken en klik vervolgens opnieuw uitvoeren


Nadat u het resultaat van de _inmem uitvoeren hebt, kunt u de volgende stappen uitvoeren voor de _ondisk uitvoeren:


1. De database opnieuw instellen door de volgende opdracht uit te voeren in SSMS, verwijdert u alle gegevens die is ingevoegd met het vorige uitvoeren:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bewerk de opdrachtregel ostress.exe om alle *_inmem* vervangen door *_ondisk*.

3. Opnieuw ostress.exe uit de tweede keer, en het resultaat van de duur vastleggen.

4. Opnieuw opnieuw instellen van de database voor verantwoordelijk verwijdering van wat een grote hoeveelheid testgegevens kan zijn.


#### <a name="expected-comparison-results"></a>Verwachte vergelijkingsresultaten

Onze tests In het geheugen hebben een prestatieverbetering **9 tijden** voor deze eenvoudig werkbelasting, weergegeven met ostress uitgevoerd op een VM Azure in hetzelfde Azure gebied, als de database.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B TE DRUKKEN. Installeren van de steekproef In het geheugen Analytics


In dit gedeelte Vergelijk de resultaten IO en statistieken worden aangepast wanneer u een index columnstore versus een traditionele b-structuur index gebruikt.


Voor realtime analytics op een werkbelasting OLTP is het verstandig om te gebruiken als u een niet-gegroepeerde columnstore-index. Zie [Columnstore indexen beschreven](http://msdn.microsoft.com/library/gg492088.aspx)voor meer informatie.



### <a name="prepare-the-columnstore-analytics-test"></a>De columnstore analytics-test voorbereiden


1. Gebruik de Azure-portal naar een nieuwe AdventureWorksLT-database maken van de steekproef.
 - Gebruikt die naam.
 - Kies elke laag Premium-service.

2. De [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) naar het Klembord kopiëren.
 - De T-SQL-script Hiermee maakt u de benodigde In het geheugen-objecten in de AdventureWorksLT voorbeelddatabase die u in stap 1 hebt gemaakt.
 - Het script Hiermee maakt u de tabel dimensie en twee feitentabellen. De feitentabellen worden gevuld met 3,5 miljoen rijen.
 - Het script kan 15 minuten duren.

3. De T-SQL-script in SSMS plakken en vervolgens het script uitvoeren.
 - Essentieel is, is het trefwoord **COLUMNSTORE** op een instructie **CREATE INDEX** , zoals in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Compatibiliteitsniveau 130 AdventureWorksLT instellen:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Niveau 130 is niet rechtstreeks zijn gerelateerd aan In het geheugen functies. Maar niveau 130 geeft in het algemeen query sneller dan 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Essentieel tabellen en columnstore indexen


- dbo. FactResellerSalesXL_CCI is een tabel met een index gegroepeerd **columnstore** , die geavanceerde compressie op het niveau van de *gegevens* bevat.

- dbo. FactResellerSalesXL_PageCompressed is een tabel met een equivalente normale gegroepeerd index, die alleen op het niveau van de *pagina* is gecomprimeerd.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Essentieel query's naar de index columnstore vergelijken


[Hier](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) zijn verschillende T-SQL-query's u kunt uitvoeren om prestatieverbeteringen weer te geven. Uit stap 2 in de T-SQL-script, moet u er een paar van query's die directe belangrijke zijn is. De twee query's verschillen alleen op één regel:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Als u een index gegroepeerd columnstore ligt op de FactResellerSalesXL**_CCI** -tabel.

Het volgende fragment van de T-SQL-script afdrukken statistieken voor IO en de tijd voor de query van elke tabel


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Aandachtspunten voor de Preview-versie voor OLTP In het geheugen


De functies In het geheugen OLTP in Azure SQL-Database [voor preview op 28 oktober 2015 actief](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/)is geworden.


Klik in het huidige voorbeeld wordt In het geheugen OLTP alleen ondersteund voor:

- Databases die zich op een laag *Premium* -service.

- Databases die zijn gemaakt nadat de functies In het geheugen OLTP actief is geworden.
 - Een nieuwe database kan niet In het geheugen OLTP ondersteuning als deze is teruggezet uit een database die is gemaakt voordat de functies In het geheugen OLTP actief is geworden.


Als u twijfelt, kunt u altijd de volgende T-SQL selecteren om na te gaan of uw database In het geheugen OLTP ondersteunt uitvoeren. **1** als resultaat betekent dat de database In het geheugen OLTP worden ondersteund:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Als de query **1 retourneert**, In het geheugen OLTP wordt ondersteund in deze database en een database kopiëren en deze database herstellen die zijn gemaakt op basis van deze database.


#### <a name="objects-allowed-only-at-premium"></a>Objecten die zijn toegestaan alleen op Premium


Als een database een van de volgende typen In het geheugen OLTP objecten of typen bevat, wordt de servicelaag van de database uit Premium eenvoudige of standaard Downgrade niet ondersteund. Als u wilt de database downgraden, moet u eerst deze objecten weghalen:

- Geheugen geoptimaliseerde tabellen
- Geheugen geoptimaliseerde tabeltypen
- Standaard gecompileerde modules


#### <a name="other-relationships"></a>Andere relaties


- Functies gebruiken In het geheugen OLTP met databases in elastische toepassingen wordt niet ondersteund in voorbeeldweergave.
 - Als u wilt verplaatsen van een database die is of heeft In het geheugen OLTP objecten aan een elastische toepassingen, als volgt te werk:
  - 1. Alle tabellen geheugen geoptimaliseerde, tabeltypen en native gecompileerd T-SQL-modules in de database weghalen
  - 2. Wijzigen van de servicelaag van de database naar standaard
  - 3. De database verplaatsen naar de elastische toepassingen

- Gebruik In het geheugen OLTP met SQL Data Warehouse wordt niet ondersteund.
 - De functie van de index columnstore van In het geheugen Analytics wordt ondersteund in SQL Data Warehouse.

- Query's binnenkant gecompileerd native modules, wordt niet door de Query-Store worden vastgelegd.

- Sommige Transact-SQL-functies worden niet ondersteund met In het geheugen OLTP. Dit geldt voor zowel Microsoft SQL Server en Azure SQL-Database. Zie voor meer informatie:
 - [Transact-SQL-ondersteuning voor OLTP In het geheugen](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Transact-SQL-constructies niet wordt ondersteund door In het geheugen OLTP](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Volgende stappen


- Probeer [gebruiken In het geheugen OLTP in een bestaande Azure SQL-toepassing.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Aanvullende informatie

#### <a name="deeper-information"></a>Meer informatie

- [Meer informatie over het In het geheugen OLTP, die voor Microsoft SQL Server en Azure SQL-Database geldt](http://msdn.microsoft.com/library/dn133186.aspx)

- [Meer informatie over realtime operationele Analytics op MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Whitepaper op [algemene werkbelasting patronen en over migratie](http://msdn.microsoft.com/library/dn673538.aspx), waarin wordt beschreven werkbelasting patronen waar In het geheugen OLTP vaak aanzienlijke prestatieverbeteringen biedt.

#### <a name="application-design"></a>Het ontwerp van toepassing

- [In het geheugen OLTP (In het geheugen Optimization)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Gebruik In het geheugen OLTP in een bestaande Azure SQL-toepassing.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Hulpmiddelen voor

- [SQL Server Data Tools Preview (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), naar de meest recente versie van de maandelijkse.

- [Beschrijving van de opnieuw afspelen Markup Language (RML) hulpprogramma's voor SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Monitor geheugenopslag](sql-database-in-memory-oltp-monitoring.md) voor In het geheugen OLTP.

