<properties
   pageTitle="Lussen in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het Transact-SQL lussen en vervangt cursors in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="loops-in-sql-data-warehouse"></a>Lussen in SQL datawarehouse
SQL Data Warehouse ondersteunt de lus [terwijl][] voor herhaaldelijk uitvoeren instructie blokken. Deze blijven voor zo lang maken als de opgegeven voorwaarden voldaan wordt of totdat de code specifiek wordt beëindigd de lus gebruiken de `BREAK` trefwoord. Lussen zijn met name handig voor het vervangen van cursors die zijn gedefinieerd in de SQL-code. Gelukkig lezen bijna alle cursors die zijn geschreven in SQL-code van de vooruitspoelen, zijn alleen diverse. [Terwijl] lussen zijn een uitstekende alternatief dus als u dat u hoeft merkt te vervangen.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Gebruikmaken van lussen en vervangen van cursors in SQL Data Warehouse
Echter voordat het hoofd eerst Dubbelklik vraagt u zelf de volgende vraag: "Kan deze cursor opnieuw naar worden geschreven instellen op basis van bewerkingen gebruiken?". In veel gevallen het antwoord Ja en is het meestal de beste manier. Een set op basis van de bewerking vaak veel sneller dan een iteratieve, per rij manier kunt uitvoeren.

Vooruitspoelen alleen-lezen cursors kunnen eenvoudig worden vervangen door een herhaling constructie. Hieronder volgt een eenvoudig voorbeeld. Voorbeeld bijgewerkt de statistieken voor elke tabel in de database. Door iteratie van de tabellen in de lus kunnen we elke opdracht uitvoeren in volgorde.

Maak eerst een tijdelijke tabel die een unieke rij getal gebruikt om te identificeren van de afzonderlijke beweringen bevat:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Tweede, geïnitialiseerd de variabelen die nodig is om te missen uitvoeren:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nu herhalen op instructies deze een uitvoert op een moment:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Ten slotte de tijdelijke tabel die is gemaakt in de eerste stap verwijderen

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[TIJD]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
