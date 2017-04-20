<properties
   pageTitle="Opgeslagen procedures in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor de uitvoering van opgeslagen procedures in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="stored-procedures-in-sql-data-warehouse"></a>Opgeslagen procedures in SQL Data Warehouse

SQL Data Warehouse ondersteunt veel van de Transact-SQL-functies zijn gevonden in SQL Server. Er zijn belangrijker schaal specifieke functies die we wordt willen gebruiken om de prestaties van uw oplossing uit.

Behoud zijn de omvang en de prestaties van SQL Data Warehouse er echter ook enkele functies en functionaliteit gebruiken die zijn doorgevoerd verschillen en anderen die niet worden ondersteund.

In dit artikel wordt uitgelegd hoe u opgeslagen procedures binnen SQL Data Warehouse implementeert.

## <a name="introducing-stored-procedures"></a>Kennismaking met opgeslagen procedures
Opgeslagen procedures zijn een uitstekende manier voor die uw SQL-code; het opslaan van deze dicht bij uw gegevens in het datawarehouse. Door de code die in eenheden helpen opgeslagen procedures ontwikkelaars modularize hun oplossingen; vergemakkelijken groter herbruikbaarheid met code. Elke opgeslagen procedure kunt ook parameters zodat ze nog meer flexibele accepteren.

SQL Data Warehouse biedt een vereenvoudigd en gestroomlijnde opgeslagen procedure-implementatie. Het grootste verschil vergeleken met SQL Server is dat de opgeslagen procedure niet vooraf gecompileerde code. In datawarehouses we over het algemeen minder betrokken zijn bij de compilatietijd. Het is belangrijk dat de opgeslagen procedurecode juist is geoptimaliseerd wanneer deze ten opzichte van grote hoeveelheden gegevens. Het doel is om op te slaan uren, minuten en seconden niet milliseconden. Daarom handiger om na te denken van opgeslagen procedures als containers voor SQL-logica.     

Wanneer SQL Data Warehouse uw opgeslagen procedure wordt uitgevoerd worden de SQL-instructies geparseerd, vertaald en geoptimaliseerde tijdens runtime. Tijdens dit proces wordt elke instructie geconverteerd naar gedistribueerde query's. De SQL-code die daadwerkelijk is uitgevoerd voor de gegevens verschilt aan de query die is verzonden.

## <a name="nesting-stored-procedures"></a>Geneste opgeslagen procedures
Wanneer opgeslagen procedures bellen andere opgeslagen procedures of dynamische SQL-instructies uitvoeren en vervolgens de binnenste procedure of de code aanroep opgeslagen genoemd worden genest.

SQL Data Warehouse ondersteuning maximaal 8 geneste niveaus. Dit is iets anders met SQL Server. Het niveau nesten in SQL Server is 32.

Het bovenste niveau opgeslagen procedure gesprek gelijk is om het nesten van niveau 1

```sql
EXEC prc_nesting
```
Als u de opgeslagen procedure is ook een ander Raad bellen vervolgens vergroten dit door het niveau nesten met 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Als u de tweede procedure voert sommige dynamische sql vervolgens vergroten dit door het niveau nesten met 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Opmerking SQL Data Warehouse biedt momenteel geen ondersteuning voor @@NESTLEVEL. U moet een om bij te houden van uw niveau nesten zelf. Het is waarschijnlijk u de limiet voor het niveau van 8 nesten wordt er maar doet u dit moet u opnieuw werken uw code en 'samenvoegen' deze zodat deze in deze limiet past.

## <a name="insertexecute"></a>INVOEGEN... UITVOEREN
SQL Data Warehouse niet toestaan dat u de resultatenset van een opgeslagen procedure met een verklaring omtrent het invoegen in beslag neemt. Er is echter een alternatieve methode die u kunt gebruiken.

Raadpleeg het volgende artikel in [tijdelijke tabellen] voor een voorbeeld over hoe u dit wilt doen.

## <a name="limitations"></a>Beperkingen

Er zijn enkele aspecten van opgeslagen Transact-SQL-procedures die niet zijn geïmplementeerd in SQL Data Warehouse.

Ze zijn:

- tijdelijke opgeslagen procedures
- genummerde opgeslagen procedures
- uitgebreide opgeslagen procedures
- CLR opgeslagen procedures
- optie codering
- replicatie-optie
- tabelwaarden parameters
- alleen-lezen parameters
- standaardparameters
- uitvoering contexten
- Return, instructie

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[tijdelijke tabellen]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
