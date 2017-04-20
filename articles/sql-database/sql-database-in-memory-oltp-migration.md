<properties
    pageTitle="In het geheugen OLTP verbetert SQL txn perf | Microsoft Azure"
    description="Hanteer In het geheugen OLTP om te verbeteren de prestaties voor transacties in een bestaande SQL-database."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Gebruik In het geheugen OLTP (preview) om te verbeteren de toepassingsprestaties van uw in SQL-Database

[In het geheugen OLTP](sql-database-in-memory.md) kan worden gebruikt voor het verbeteren van de werkzaamheden OLTP in [Premium](sql-database-service-tiers.md) Azure SQL-Databases zonder het prestatieniveau van de te vergroten.

Volg deze stappen om vast In het geheugen OLTP in uw bestaande database.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Stap 1: Controleer dat de Premium-database In het geheugen OLTP ondersteunt

De functie In het geheugen bieden ondersteuning voor Premium-databases die zijn gemaakt in November 2015 of hoger. U kunt gaan of de Premium-database de functie In het geheugen wordt ondersteund door de volgende Transact-SQL-instructie uit te voeren. In het geheugen wordt ondersteund als het geretourneerde resultaat 1 is (niet 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* staat voor het *Verwerken van Extreme transactie*

Als uw bestaande database moet worden verplaatst naar een nieuwe V12 Premium-database, kunt u de volgende technieken wilt exporteren en vervolgens uw gegevens te importeren.

#### <a name="export-steps"></a>Exportstappen

Uw productiedatabase exporteren naar een bacpac met behulp van:

- De functionaliteit voor [exporteren](sql-database-export.md) in de [portal](https://portal.azure.com/).

- De functionaliteit van de **gegevens exporteren laag toepassing** in een [bijgewerkt SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. Vouw in het **Object Explorer**de **Databases** uit.
 2. Met de rechtermuisknop op het knooppunt van uw database.
 3. Klik op **taken** > **gegevens laag toepassing exporteren**.
 4. Werken in het venster van de wizard die wordt weergegeven.


#### <a name="import-steps"></a>Importstappen

De bacpac importeren in een nieuwe Premium-database.

1. Klik in de [portal van](https://portal.azure.com/)Azure
 - Navigeer naar de server.
 - Selecteer de optie [Database importeren](sql-database-import.md) .
 - Selecteer een Premium prijzen van laag.

2. Gebruik SSMS de bacpac importeren:
 - Klik in het **Object Explorer**met de rechtermuisknop op het knooppunt **Databases** uit.
 - Klik op **importeren gegevens laag toepassing**.
 - Werken in het venster van de wizard die wordt weergegeven.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Stap 2: Objecten om te migreren naar In het geheugen OLTP identificeren

SSMS bevat een **Overzicht van transactie prestaties Analysis** -rapport die u op een database maken met een actieve werkbelasting uitvoeren kunt. Het rapport kunt u tabellen en opgeslagen procedures die kandidaten voor migratie naar In het geheugen OLTP zijn identificeren.

In SSMS, om het rapport te genereren:
- Klik in het **Object Explorer**met de rechtermuisknop op het knooppunt van uw database.
- Klik op **rapporten** > **Standard Reports** > **transactie prestaties analyse-overzicht**.

Zie [vaststellen als een tabel of de opgeslagen Procedure moet worden overgezet naar In het geheugen OLTP](http://msdn.microsoft.com/library/dn205133.aspx)voor meer informatie.


## <a name="step-3-create-a-comparable-test-database"></a>Stap 3: Een vergelijkbare testdatabase maken

Stel dat de lijst wordt aangegeven dat uw database een tabel die u profiteren wilt van wordt geconverteerd naar een tabel geheugen geoptimaliseerde heeft. Het is raadzaam dat u eerst testen om te bevestigen van de vermelding door te testen.

Moet u een testexemplaar van uw productiedatabase. De testdatabase moet hetzelfde service laag niveau als uw productiedatabase.

Om te vereenvoudigen testen, moet u uw testdatabase als volgt aanpassen:

1. Verbinding maken met de testdatabase met behulp van SSMS.

2. Als u wilt voorkomen dat u de optie met (MOMENTOPNAME) in query's nodig hebt, kunt u de optie van de database zoals wordt weergegeven in de volgende T-SQL-instructie instellen:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Stap 4: Tabellen migreren

U moet maken en vullen een geheugen geoptimaliseerde kopie van de tabel die u wilt testen. U kunt deze hebt gemaakt met behulp van:

- De handige geheugen optimalisatie Wizard in SSMS.
- Handmatige T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Geheugen optimaliseren Wizard in SSMS

Gebruik deze migratieoptie:

1. Verbinding maken met de testdatabase met SSMS.

2. In het **Object Explorer**met de rechtermuisknop op de tabel en klik vervolgens op **Geheugen optimalisatie Advisor**.
 - De wizard **Tabel geheugen optimaliseren Advisor** wordt weergegeven.

3. Klik op de **migratie validatie** (of de knop **volgende** ) of de tabel heeft een niet-ondersteunde functies die niet worden ondersteund in geheugen geoptimaliseerde tabellen in de wizard. Zie voor meer informatie:
 - De *geheugen optimalisatie controlelijst* in [Het geheugen optimalisatie Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Transact-SQL-constructies niet wordt ondersteund door In het geheugen OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migreren naar In het geheugen OLTP](http://msdn.microsoft.com/library/dn247639.aspx).

4. Als de tabel geen niet-ondersteunde functies heeft, kan de advisor het werkelijke schema en de gegevensmigratie uitvoeren voor u.


#### <a name="manual-t-sql"></a>Handmatige T-SQL

Gebruik deze migratieoptie:

1. Verbinding maken met uw testdatabase met behulp van SSMS (of een vergelijkbare hulpprogramma).

2. Het volledige T-SQL-script voor de tabel en de bijbehorende indexen aanvragen.
 - SSMS, met de rechtermuisknop op het knooppunt van uw tabel.
 - Klik op **de Scripttabel als** > **maken om te** > **nieuwe Query-venster**.

3. Klik in het scriptvenster toevoegen door (MEMORY_OPTIMIZED = ON) naar de instructie CREATE TABLE.

4. Als er een index GEGROEPEERD is, kunt u deze naar NONCLUSTERED wijzigen.

5. Wijzig de bestaande tabel met behulp van SP_RENAME.

6. De nieuwe geheugen geoptimaliseerde kopie van de tabel maken door uw bewerkte CREATE TABLE-script uit te voeren.

7. Kopieer de gegevens aan de tabel geheugen geoptimaliseerde met behulp van invoegen... SELECTEER * IN:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Stap 5 (optioneel): migreren opgeslagen procedures

De functie In het geheugen kunt ook een opgeslagen procedure voor verbeterde prestaties wijzigen.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Overwegingen met native gecompileerd opgeslagen procedures

Een ingebouwde gecompileerd opgeslagen procedure moet hebben op de component T-SQL met de volgende opties:

- NATIVE_COMPILATION

- TOEPASSING: waarbij de tabellen die de opgeslagen procedure kan geen de kolomdefinities gewijzigd op geen enkele manier invloed heeft op de opgeslagen procedure bevatten, tenzij u de opgeslagen procedure plaatst.


Een systeemeigen module moet een grote [ATOMAIRE blokken](http://msdn.microsoft.com/library/dn452281.aspx) gebruiken voor beheer van transacties. Er is geen functie voor een expliciete BEGIN TRANSACTION of voor transactie TERUGDRAAIEN. Als uw code worden gedetecteerd een overtreding van bedrijfsregels, kan deze de atomaire blok met een instructie [genereren](http://msdn.microsoft.com/library/ee677615.aspx) eindigen.


### <a name="typical-create-procedure-for-natively-compiled"></a>Normale maken PROCEDURE voor het oorspronkelijke programma gecompileerd

De T-SQL maken van een ingebouwde gecompileerd opgeslagen procedure is meestal vergelijkbaar met de volgende sjabloon:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- MOMENTOPNAME is voor de TRANSACTION_ISOLATION_LEVEL, de meest voorkomende waarde voor de standaard gecompileerd opgeslagen procedure. Een subset van de andere waarden worden echter ook ondersteund:
 - HERHAALD LEZEN
 - SERIALISEERBAAR


- De taal-waarde moet aanwezig zijn in de weergave sys.languages.


### <a name="how-to-migrate-a-stored-procedure"></a>Het migreren van een opgeslagen procedure

De migratiestappen zijn:


1. Het script CREATE PROCEDURE naar de normale geïnterpreteerd opgeslagen procedure aanvragen.

2. Herschrijf de header zodat deze overeenkomen met de vorige sjabloon.

3. Controleren of alle functies die niet worden ondersteund voor native gecompileerd opgeslagen procedures worden gebruikt door de opgeslagen procedure T-SQL-code. Het toepassen van de oplossingen indien nodig.
 - Zie [Problemen migratie voor native gecompileerd opgeslagen Procedures](http://msdn.microsoft.com/library/dn296678.aspx)voor meer informatie.

4. De naam van de oude opgeslagen procedure wijzigen met behulp van SP_RENAME. Of u gewoon.

5. Uw bewerkte CREATE PROCEDURE T-SQL-script uitvoeren.


## <a name="step-6-run-your-workload-in-test"></a>Stap 6: Uw werkzaamheden uitvoeren in testen

Een werkbelasting worden uitgevoerd in uw testdatabase die vergelijkbaar is met de werklast die wordt uitgevoerd in uw productiedatabase. Dit moet de prestatieverbetering bereikt door het gebruik van de functie In het geheugen voor tabellen en opgeslagen procedures op te geven.

Belangrijke kenmerken van de werklast zijn:

- Het aantal verbindingen.

- Verhouding tussen de alleen-lezen/schrijven.


Als u wilt aanpassen en de werklast test uitvoeren, kunt u met de functie handige ostress.exe, die geïllustreerd in [hier](sql-database-in-memory.md).


Als u wilt minimaliseren netwerklatentie, Voer uw test in dezelfde Azure geografische regio waar de database zich bevindt.


## <a name="step-7-post-implementation-monitoring"></a>Stap 7: Post-implementatie bewaken

U kunt de effecten van de prestaties van uw In het geheugen implementaties in productie cmdlets voor controle:

- [Monitor geheugenopslag](sql-database-in-memory-oltp-monitoring.md).

- [Cmdlets voor controle Azure SQL-Database dynamische management weergaven gebruiken](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Verwante koppelingen

- [In het geheugen OLTP (In het geheugen Optimization)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Inleiding tot het oorspronkelijke programma gecompileerd opgeslagen Procedures](http://msdn.microsoft.com/library/dn133184.aspx)

- [Geheugen optimaliseren Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

