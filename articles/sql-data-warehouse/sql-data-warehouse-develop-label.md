<properties
   pageTitle="Labels tot instrument query's gebruiken in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het gebruik van de etiketten tot instrument query's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Labels tot instrument query's in de SQL Data Warehouse gebruiken
SQL Data Warehouse ondersteunt zogenoemde query etiketten. Voordat u overschakelt naar een diepte laten we eens kijken naar een voorbeeld van een:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Deze regel codes de tekenreeks 'Mijn Query Label' aan de query. Dit is met name handig tijdens het label query kunt via de DMVs is. Dit beschikken wij over een om om te achterhalen probleem query's en ook ter identificatie van de voortgang van via een ETL klaar.

Een goede naamgevingsconventie echt helpt hier. Bijvoorbeeld iets zoals ' PROJECT: PROCEDURE: instructie: Opmerking ' zou help ter identificatie van de query in onder de code in een besturingselement voor gegevensbronnen.

U kunt de volgende query waarin de dynamische management-weergaven gebruiken om te zoeken op label:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Het is essentieel dat u vierkante haken of dubbele aanhalingstekens rond het word-label teruglopen wanneer de query's uitvoeren. Label is een gereserveerd woord en wordt een fout veroorzaakt als deze zonder scheidingstekens.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
