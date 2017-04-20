<properties
   pageTitle="Transacties in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het implementeren van transacties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transacties in SQL datawarehouse

Zoals u verwachten zou, ondersteunt SQL Data Warehouse transacties als onderdeel van de werklast van de BTW-records gegevens. Om ervoor te zorgen dat de prestaties van SQL Data Warehouse op schaal blijft zijn sommige functies echter beperkt in vergelijking met SQL Server. In dit artikel worden de verschillen gemarkeerd en de andere lijsten. 

## <a name="transaction-isolation-levels"></a>Transactie moeten worden geïsoleerd niveaus
SQL Data Warehouse implementeert ZURE transacties. De moeten worden geïsoleerd van de ondersteuning van transacties is echter beperkt tot `READ UNCOMMITTED` en dit kan niet worden gewijzigd. U kunt een aantal manieren om te voorkomen dat dirty lezen van gegevens als dit een probleem voor u is kleurcodering implementeren. De populairste methoden gebruikmaken van zowel CTAS als tabel partition overstappen (vaak de schuifregelaar venster patroon genoemd) als u wilt voorkomen dat gebruikers gegevens dat is nog steeds wordt voorbereid opvragen. Weergaven die vooraf de gegevens filteren die is ook een populaire manier.  

## <a name="transaction-size"></a>Transactie-grootte
Een enkele gegevens wijziging transactie is grootte beperkt. Vandaag, de limiet "per verdeling" wordt toegepast. De totale toewijzing kan dus worden berekend door te vermenigvuldigen met de limiet van de verdeling tellen. Naar niet-geheel exacte het maximum aantal rijen in de transactie deelt u de verdeling initiaal door de totale grootte van elke rij. Houd rekening met de kolomlengte van een gemiddelde duurt in plaats van het gebruik van de maximale grootte voor kolommen met variabele lengte.

In de tabel hieronder de volgende veronderstellingen zijn aangebracht:

* Een even verdeling van gegevens is opgetreden 
* De rijlengte van de gemiddelde is 250 bytes

| [DWU][]    | Hoofdletters per verdeling (toevoegen) | Aantal onderzoeken | MAX transactie grootte (toevoegen) | # Rijen per verdeling | Max rijen per transactie |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60.000.000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2700                     | 180,000,000             | 10,800,000,000           |

De transactie groottelimiet overschrijden, wordt toegepast per transactie of -bewerking. Deze wordt niet toegepast op alle gelijktijdige transacties. Elke transactie wordt daarom toegestaan deze hoeveelheid gegevens naar het logboek schrijven. 

Te optimaliseren en de hoeveelheid gegevens die in het logboek opgenomen minimaliseren Raadpleeg het artikel [transacties aanbevolen procedures][] .

> [AZURE.WARNING] De grootte van de maximale transactie kan alleen worden bereikt voor HASHBEWERKINGEN of ROUND_ROBIN distributed tabellen waarbij de verspreiding van de gegevens is zelfs. Als de transactie gegevens in een schuine manier naar de onderzoeken schrijft zijn de limiet is waarschijnlijk voordat u de grootte van de maximale transactie worden bereikt.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Transactie provincie
SQL Data Warehouse wordt de functie XACT_STATE() rapporteren van een mislukte transactie met de waarde -2. Dit betekent dat de transactie is mislukt en is gemarkeerd voor alleen ongedaan maken

> [AZURE.NOTE] Het gebruik van-2 door de functie XACT_STATE ter aanduiding van een mislukte transactie vertegenwoordigt verschillende gedrag in SQL Server. SQL Server gebruikt de waarde -1 om aan te geven van een niet-committable transactie. SQL Server mogelijk enkele fouten binnen een transactie zonder zonder wordt gemarkeerd als niet committable. Bijvoorbeeld `SELECT 1/0` wordt een fout veroorzaken, maar niet automatisch een transactie in een niet-committable staat. SQL Server ook toestemming geeft dat lezen in de niet committable transactie. Echter laat SQL Data Warehouse niet u dit kunt doen. Als er een fout optreedt binnen een transactie SQL Data Warehouse deze wordt automatisch ingevuld met de-2-staat en is niet mogelijk om te maken van een select-instructies verder totdat de instructie is hersteld. Daarom belangrijk om te controleren dat uw toepassingscode om te zien als deze XACT_STATE() als u gebruikt mogelijk moet code wijzigingen aanbrengen.

Bijvoorbeeld in SQL Server ziet u mogelijk een transactie die er zo uitziet:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Als u uw code verlaten, terwijl deze zich boven krijgt u het volgende foutbericht weergegeven:

Msg 111233, niveau 16, staat 1, regel 1 111233; De huidige transactie is afgebroken, waarbij de wachtende wijzigingen hebt is hersteld. Oorzaak: Een transactie terugdraaien alleen-lezen status is niet expliciet ongedaan voordat u een DDL, DML of SELECT-instructie.

U krijgt geen ook de uitvoer van de functies ERROR_ *.

De code moet iets worden gewijzigd in SQL Data Warehouse:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Normaal is nu waargenomen. De fout in de transactie wordt beheerd en de functies ERROR_ * Verstrek waarden zoals verwacht.

Alle die is gewijzigd, is dat de `ROLLBACK` van de transactie moest gebeuren voor het lezen van de gegevens van de fout in de `CATCH` blokkeren.

## <a name="errorline-function"></a>Error_Line()-functie
Het is ook handig om te weten dat SQL Data Warehouse niet implementeren of de functie ERROR_LINE() ondersteunen. Als u dit in uw code die u moet verwijderen om te voldoen aan SQL Data Warehouse hebt. Labels van de query in code gebruiken voor het implementeren equivalente functionaliteit. Raadpleeg het artikel [LABEL][] voor meer informatie over deze functie.

## <a name="using-throw-and-raiserror"></a>WEGGOOIEN en RAISERROR gebruiken
WEGGOOIEN is de moderne uitvoering voor uitzonderingen in SQL Data Warehouse verhogen maar RAISERROR wordt ook ondersteund. Er zijn enkele verschillen waarde met aandacht voor echter.

- Door gebruiker gedefinieerde foutberichten getallen kan niet in het bereik 100.000 150.000 voor WEGGOOIEN
- RAISERROR foutberichten worden opgelost aan 50.000
- Gebruik van sys.messages wordt niet ondersteund.

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse beschikt over een paar andere beperkingen die betrekking op transacties hebben.

Dit zijn de volgende:

- Geen gedistribueerde transacties
- Geen geneste transacties toegestaan
- Geen opslaan punten toegestaan
- Geen benoemde transacties
- Geen gemarkeerde transacties
- Biedt geen ondersteuning voor DDL zoals `CREATE TABLE` binnen een gebruiker gedefinieerd transactie

## <a name="next-steps"></a>Volgende stappen
Zie meer informatie over het optimaliseren van transacties, [transacties aanbevolen procedures][].  Zie voor meer informatie over aanbevolen werkwijzen voor andere SQL Data Warehouse, [SQL Data Warehouse aanbevolen procedures][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Aanbevolen procedures voor transacties]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
