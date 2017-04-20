<properties
   pageTitle="Variabelen in SQL Data Warehouse toewijzen | Microsoft Azure"
   description="Tips voor het toewijzen van Transact-SQL-variabelen in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Variabelen in SQL Data Warehouse toewijzen
Variabelen in SQL Data Warehouse worden ingesteld met de `DECLARE` instructie of de `SET` instructie.

Meer van de volgende zijn perfect geldige manieren om in te stellen van de waarde van een variabele:

## <a name="setting-variables-with-declare"></a>Variabelen met DECLARE instellen

Initialisatie van de variabelen met DECLARE is een van de meest flexibele manieren om de waarde van een variabele instellen in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

U kunt ook DECLARE gebruiken voor het instellen van meer dan één variabele tegelijk. U kunt niet gebruiken `SELECT` of `UPDATE` u dit wilt doen:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

U kunt niet geïnitialiseerd en een variabele gebruiken in de instructie DECLARE. Om te laten zien van het punt in het onderstaande voorbeeld is **niet** toegestaan als @p1 is geïnitialiseerd en gebruikt in de dezelfde DECLARE-instructie. Hierdoor wordt een fout.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Waarden instellen met instellen
Set is een zeer gebruikte methode voor het instellen van één variabele.

De onderstaande voorbeelden zijn ongeldig manieren van het instellen van een variabele met instellen:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

U kunt alleen één variabele tegelijk instellen met SET. Zoals hierboven is zichtbaar zijn samengestelde operatoren echter toegestane.

## <a name="limitations"></a>Beperkingen
U kunt selecteren of een UPDATE niet gebruiken voor variabele toewijzing.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
