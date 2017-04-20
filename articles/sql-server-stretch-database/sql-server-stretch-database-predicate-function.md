<properties
    pageTitle="Selecteer rijen om te migreren met behulp van een filterfunctie (uitrekken-Database) | Microsoft Azure"
    description="Leer hoe u rijen wilt migreren met behulp van een filterfunctie selecteren."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Selecteer rijen om te migreren met behulp van een filterfunctie (uitrekken-Database)

Als u koudwatersystemen gegevens in een aparte tabel opslaat, kunt u de Database uitrekken voor het migreren van de hele tabel. Als uw tabel warm- en koudwatersystemen gegevens bevat, aan de andere kant, kunt u een filterfunctie om de rijen om te migreren te selecteren. Het filter predikaat is een inline-tabel\-waarde van de functie. Dit onderwerp wordt beschreven hoe u schrijft een inline-tabel\-waarde van de functie voor het selecteren van rijen wilt migreren.

>   [AZURE.NOTE] Als u een filterfunctie die niet goed werkt opgeeft, werkt gegevensmigratie ook niet goed. Uitrekken Database is van toepassing de filterfunctie op de tabel met de operator CROSS toepassen.

Als u een filterfunctie niet opgeeft, wordt de hele tabel gemigreerd.

Wanneer u de Database inschakelen voor uitrekken Wizard uitvoert, kunt u een hele tabel migreren of kunt u een functie eenvoudig filter in de wizard. Als u een ander type van de filterfunctie gebruiken om te selecteren van rijen om te migreren wilt, de volgende dingen doen.

-   De wizard afsluiten en de instructie ALTER TABLE uitrekken inschakelen voor de tabel en geef een filterfunctie uitvoeren.

-   Voer de instructie ALTER TABLE om op te geven van een filterfunctie nadat u de wizard afsluit.

De syntaxis van de ALTER TABLE voor het toevoegen van een functie wordt verderop in dit onderwerp beschreven.

## <a name="basic-requirements-for-the-filter-function"></a>Basisvereisten voor de filterfunctie
De tabel inline\-waarden functie vereist voor een Database uitrekken filter predikaat eruitziet in het volgende voorbeeld.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
De parameters voor de functie moeten-id's voor de kolommen in de tabel.

Schemabinding is vereist om te voorkomen dat de kolommen die worden gebruikt door de filterfunctie worden verwijderd of gewijzigd.

### <a name="return-value"></a>Retourwaarde
Als de functie een niet retourneert\-leeg resultaat, de rij is in aanmerking om te worden gemigreerd. Anders \- dat wil zeggen als de functie niet resulteren \- de rij is niet in aanmerking om te worden gemigreerd.

### <a name="conditions"></a>Voorwaarden
De &lt; *predikaat* &gt; van één voorwaarde of meerdere voorwaarden samengevoegd met de logische operator en kunnen bestaan.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Elke voorwaarde kan beurtelings bestaan van één primitieve voorwaarde of meerdere primitieve voorwaarden die met de logische operator OR zijn gekoppeld.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Primitieve voorwaarden
Een primitieve voorwaarde kan een van de volgende vergelijkingen.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Een parameter van de functie om een constante expressie te vergelijken. Bijvoorbeeld `@column1 < 1000`.

    Hier volgt een voorbeeld waarin wordt gecontroleerd of de waarde van een *datumkolom* ophalen &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   De operator IS NULL of IS niet null-waarden toepassen op een parameter van de functie.

-   De operator in gebruiken om te vergelijken van een parameter van de functie aan een lijst met een constante waarden.

    Hier volgt een voorbeeld die Hiermee wordt gecontroleerd of de waarde van een *verzending\_status* kolom is `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Vergelijkingsoperatoren
De volgende vergelijkingsoperators worden ondersteund.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Constante expressies
De constanten die u in een filterfunctie gebruiken is een deterministic-expressie die kan worden geëvalueerd als u de functie definieert. Constante expressies kunnen de volgende onderdelen bevatten.

-   Letterlijke waarden. Bijvoorbeeld `N’abc’, 123`.

-   Algebraïsche expressies. Bijvoorbeeld `123 + 456`.

-   Deterministic functies. Bijvoorbeeld `SQRT(900)`.

-   Deterministic conversies met CAST of converteren. Bijvoorbeeld `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Andere expressies
U kunt de BETWEEN en niet BETWEEN operatoren gebruiken als de resulterende functie aan de regels die hier worden beschreven voldoet nadat u de BETWEEN vervangen en niet tussen gebruiken met de overeenkomstige en en of expressies.

U kunt geen subquery's gebruikt of niet\-deterministic functies zoals ASELECT() of GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Een filterfunctie toevoegen aan een tabel
Een filterfunctie toevoegen aan een tabel met de instructie **ALTER TABLE** voeren en geven van een bestaande tabel met inline\-functie waarde als de waarde van de **FILTER\_PREDIKAATFUNCTIE** parameter. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nadat u de functie aan de tabel als een predicaat koppelt, wordt de volgende dingen waar zijn.

-   De volgende keer gegevensmigratie plaatsvindt, alleen de rijen waarvoor de functie retourneert een niet\-lege waarde worden gemigreerd.

-   De kolommen die worden gebruikt door de functie zijn schema afhankelijk. U kunt deze kolommen niet wijzigen, zo lang maken als een tabel de functie als de predicaat filter gebruikt wordt.

U kunt de inline-tabel niet neerzetten\-functie waarde zo lang maken als een tabel de functie als de predicaat filter gebruikt wordt.

>   [AZURE.NOTE] Ter verbetering van de prestaties van de filterfunctie, moet u een index maken voor de kolommen die worden gebruikt door de functie.

### <a name="passing-column-names-to-the-filter-function"></a>Kolomnamen doorgeven aan de filterfunctie
Wanneer u een filterfunctie aan een tabel toewijst, geef de kolomnamen doorgegeven aan de filterfunctie met een naam voor de één-onderdeel. Als u hebt opgegeven Luc Bartels van Vugt wanneer u de kolom doorgeeft werkbladnamen, volgende query's ten opzichte van de uitrekken\-ingeschakeld tabel, mislukt.

Bijvoorbeeld als u een kolomnaam uit drie delen opgeeft zoals wordt weergegeven in het volgende voorbeeld, de instructie wordt uitgevoerd, maar volgende query's op de tabel, mislukt.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

In plaats daarvan opgeven met de filterfunctie met een deel van een kolomnaam in het volgende voorbeeld wordt weergegeven.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Voeg een filterfunctie na het uitvoeren van de Wizard  

Als u een functie gebruiken waarmee u maken in de Wizard **Database inschakelen voor uitrekken wilt kunt** , kunt u de instructie ALTER TABLE om op te geven van een functie nadat u de wizard afsluit kunt uitvoeren. Als u een functie wilt toepassen, wel u moet de gegevensmigratie die al bezig stoppen en weer terughalen gemigreerde gegevens. (Zie voor meer informatie over waarom dit nodig is, [een bestaande filterfunctie vervangen](#replacePredicate).  

1. De richting van de migratie omkeren en weer terug die al door de gegevens worden gemigreerd. U kunt deze bewerking niet annuleren nadat deze is gestart. U ook kosten op Azure voor uitgaande gegevens overdrachten \(egress\). Zie [hoe Azure prijzen werkt](https://azure.microsoft.com/pricing/details/data-transfers/)voor meer informatie.  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Wacht totdat de migratie om te voltooien. U kunt de status in **Uitrekken Database Monitor** uit SQL Server Management Studio controleren of u kunt de weergave **sys.dm_db_rda_migration_status** query. Zie voor meer informatie [controleren en problemen met gegevensmigratie](sql-server-stretch-database-monitor.md) of [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. De filterfunctie dat u wilt toepassen op de tabel maken.  

4. De functie toevoegen aan de tabel en opnieuw beginnen gegevensmigratie naar Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Rijen filteren op datum
Het volgende voorbeeld worden gemigreerd rijen waar in de kolom **datum** een waarde lager dan 1 januari 2016 bevat.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Rijen filteren op de waarde in de statuskolom
Het volgende voorbeeld worden gemigreerd rijen waarvan de kolom **status** een van de opgegeven waarden bevat.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Rijen filteren met behulp van een schuifregelaar venster
Als u wilt filteren rijen met behulp van een schuifregelaar venster, houd rekening met de volgende vereisten voor de filterfunctie.

-   De functie heeft deterministisch. U kunt een functie die automatisch wordt herberekend de schuifregelaar venster verloop van tijd dus geen maken.

-   De functie gebruikt schemabinding. Daarom bijwerken u niet gewoon de functie 'ter plaatse"elke dag door te bellen van de **Functie wijzigen** als de schuifregelaar venster wilt verplaatsen.

Begin met een filterfunctie zoals in het volgende voorbeeld, die overzet rijen waar de kolom **systemEndTime** een waarde lager dan 1 januari 2016 bevat.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

De filterfunctie toepassen op de tabel.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Als u bijwerken van de schuifregelaar venster wilt, kunt u het volgende doen:

1.  Maak een nieuwe functie waarmee u het nieuwe schuifregelaar venster. Het volgende voorbeeld wordt de datums die eerder zijn dan 2 januari 2016, in plaats van 1 januari 2016.

2.  Vervang de vorige filterfunctie door het nieuwe bereik door te bellen **ALTER TABLE**, zoals wordt weergegeven in het volgende voorbeeld.

3. (Optioneel) zet u de vorige filterfunctie dat u niet langer gebruikt door bellen **Functie neerzetten**. (Deze stap wordt niet weergegeven in het voorbeeld.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Als u meer voorbeelden van geldige filterfuncties

-   Het volgende voorbeeld worden twee primitieve voorwaarden gecombineerd met behulp van de logische operator en.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Het volgende voorbeeld maakt gebruik van meerdere voorwaarden en een deterministic conversie met converteren.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Het volgende voorbeeld wordt een wiskundige operatoren en functies.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Het volgende voorbeeld wordt de BETWEEN en niet BETWEEN operatoren. In dit gebruik is geldig omdat de resulterende functie aan de regels die hier worden beschreven voldoet nadat u de BETWEEN vervangen en niet tussen gebruiken met de overeenkomstige en en of expressies.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    De voorgaande functie is gelijk aan de volgende functie nadat u de BETWEEN en niet BETWEEN operatoren door de overeenkomstige en en of expressies vervangen.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Voorbeelden van filterfuncties die niet ongeldig zijn

-   De volgende functie is niet geldig omdat deze een niet bevat\-deterministic conversie.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   De volgende functie is niet geldig omdat deze een niet bevat\-deterministic functie summarize().

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   De volgende functie is niet geldig omdat deze een subquery bevat.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   De volgende functies zijn niet geldig omdat expressies die algebraïsche operatoren of ingebouwde\-in functies moeten als resultaat een constante wanneer u de functie definieert. U opnemen geen kolomverwijzingen in algebraïsche expressies of functie oproepen.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   De volgende functie is niet geldig omdat deze niet voldoet aan de regels die hier worden beschreven nadat u de operator BETWEEN door de equivalente en-expressie vervangen.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    De voorgaande functie is gelijk aan de volgende functie nadat u de operator BETWEEN door de equivalente en-expressie vervangen. Deze functie is niet geldig omdat primitieve voorwaarden kunnen alleen de logische operator OR gebruiken.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Hoe uitrekken Database is van toepassing de filterfunctie
Uitrekken Database met de filterfunctie wordt toegepast op de tabel en bepaalt in aanmerking komend rijen met de operator CROSS toepassen. Bijvoorbeeld:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Als de functie een niet retourneert\-leeg resultaat voor de rij de rij is in aanmerking om te worden gemigreerd.

## <a name="replacePredicate"></a>Een bestaande filterfunctie vervangen
U kunt een eerder opgegeven filterfunctie vervangen door de instructie **ALTER TABLE** opnieuw gebruikt en een nieuwe waarde voor de **FILTER\_PREDIKAATFUNCTIE** parameter. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
De nieuwe tabel in de inline\-waarden functie heeft de volgende vereisten.

-   De nieuwe functie heeft tot het minder beperkend zijn dan de vorige functie.

-   De operators uit de oude functie moeten bestaan in de nieuwe functie.

-   De nieuwe functie kan geen operators die niet in de oude functie bevatten.

-   De volgorde van operatoren argumenten wijzigen niet.

-   Alleen constante waarden die deel uitmaken van een `<, <=, >, >=` vergelijking kan worden gewijzigd in een manier die de functie minder beperkend maakt.

### <a name="example-of-a-valid-replacement"></a>Voorbeeld van een geldige vervangende
Wordt ervan uitgegaan dat de volgende functie het huidige filter predikaat.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
De volgende functie is een geldige vervangende omdat de nieuwe datumconstante (waarmee een later bereikt datum) zorgt ervoor dat de functie minder beperkend.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Voorbeelden van vervanging die niet ongeldig zijn
De volgende functie is dus geen geldige vervanging omdat de nieuwe datumconstante (waarmee een eerder bereikt datum) niet wordt de functie minder beperkend.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
De volgende functie is dus geen geldige vervanging omdat een van de vergelijkingsoperatoren is verwijderd.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
De volgende functie is dus geen geldige vervanging omdat een nieuwe voorwaarde is toegevoegd met de logische operator en.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Een filterfunctie van een tabel verwijderen
Als u wilt migreren van de hele tabel in plaats van de geselecteerde rijen, verwijdert u de bestaande functie door in te stellen **FILTER\_PREDIKAATFUNCTIE** op null. Bijvoorbeeld:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nadat u de filterfunctie hebt verwijderd, worden alle rijen in de tabel in aanmerking komen voor de migratie. U kunt geen daardoor een filterfunctie voor dezelfde tabel later opgeven tenzij u weer terughalen alle externe gegevens voor de tabel van Azure eerst. Deze beperking bestaat om te voorkomen dat de situatie waar rijen die niet in aanmerking komen voor de migratie wanneer u een nieuwe filterfunctie opgeeft al zijn gemigreerd naar Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>De filterfunctie is toegepast op een tabel controleren
Als u wilt controleren of er met de filterfunctie is toegepast op een tabel, opent u de catalogusweergave **sys.remote\_gegevens\_archief\_tabellen** en controleert u de waarde van de **filter\_predikaat** kolom. Als de waarde null is, is de hele tabel in aanmerking voor het archiveren. Zie [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx)voor meer informatie.

## <a name="security-notes-for-filter-functions"></a>Beveiligingsinformatie voor filterfuncties  
Een beschadigde account met bevoegdheden voor db_owner kan de volgende dingen doen.  

-   Maken en toepassen van een tabelwaardefunctie die grote hoeveelheden serverbronnen verbruikt of een langere periode resulteert in een DOS wacht.  

-   Maken en toepassen van een tabelwaardefunctie die het mogelijk af te leiden van de inhoud van een tabel waarvoor de gebruiker is expliciet geweigerd leestoegang maakt.  

## <a name="see-also"></a>Zie ook

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
