<properties
   pageTitle="Uw SQL-code migreren naar SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Uw SQL-code migreren naar SQL Data Warehouse

Wanneer uw code uit een andere database migreren naar SQL Data Warehouse, moet u waarschijnlijk wijzigingen aanbrengen in uw code gebaseerd. Sommige functies SQL Data Warehouse kunnen aanzienlijk prestaties verbeteren, zoals ze zijn bedoeld om te werken in een gedistribueerde manier. Als u wilt behouden prestaties en schaal, zijn sommige functies echter ook niet beschikbaar.

## <a name="common-t-sql-limitations"></a>Algemene T-SQL-beperkingen

De volgende lijst bevat een overzicht van de meest gebruikte functie die niet worden ondersteund in Azure SQL Data Warehouse. De koppelingen gaat u naar tijdelijke oplossingen voor de niet-ondersteunde functie:

- [ANSI-joins op updates][]
- [ANSI-joins op verwijderen][]
- [samenvoegen-instructie][]
- meerdere databases joins
- [cursors][]
- [SELECTEER... IN][]
- [INVOEGEN... RAAD][]
- uitvoer-component
- in line door de gebruiker gedefinieerde functies
- met meerdere instructies functies
- [algemene tabelexpressies](#Common-table-expressions)
- [recursieve algemene tabelexpressies (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-functies en procedures
- $partition-functie
- tabelvariabelen
- tabel waardeparameters
- gedistribueerde transacties
- doorvoeren / terugdraaien werk
- transactie opslaan
- uitvoering contexten (EXECUTE AS)
- [Group by-component met rollup / kubus / sets Groepeeropties][]
- [geneste niveaus voorbij 8][]
- [bijwerken door weergaven][]
- [gebruik van selecteren voor de toewijzing van variabele][]
- [geen MAX-gegevenstype voor dynamische SQL-tekenreeksen][]

Gelukkig kunt meeste van deze beperkingen worden gewerkt rond. Uitleg vindt u in de relevante ontwikkeling artikelen hierboven vermelde.

## <a name="supported-cte-features"></a>Ondersteunde CTE-functies

Algemene tabelexpressies (CTE's) worden gedeeltelijk ondersteund in SQL Data Warehouse.  De volgende CTE-functies worden momenteel ondersteund:

- Een CTE kan worden opgegeven in een SELECT-instructie.
- Een CTE kan worden opgegeven in een weergave maken-instructie.
- Een CTE kan worden opgegeven in een instructie maken tabel als selecteren (CTAS).
- Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CRTAS).
- Een CTE kan worden opgegeven in een instructie maken externe tabel als selecteren (CETAS).
- Een externe tabel kunt u vanuit een CTE verwijzen.
- Een externe tabel kunt u vanuit een CTE verwijzen.
- Meerdere definities van CTE query kunnen worden gedefinieerd in een CTE.

## <a name="cte-limitations"></a>CTE beperkingen

Algemene tabelexpressies hebt enkele beperkingen in SQL Data Warehouse waaronder:

- Een CTE moet worden gevolgd door één SELECT-instructie. INVOEGEN, bijwerken, verwijderen, en samenvoegen-instructies worden niet ondersteund.
- Een algemene tabelexpressie die verwijzingen naar zichzelf (een recursieve algemene tabelexpressie bevat) wordt niet ondersteund (Zie onder sectie).
- Meer dan één met component geven in een CTE is niet toegestaan. Bijvoorbeeld als een CTE_query_definition een subquery bevat, die subquery mogen geen bevatten een geneste met component die een andere CTE definieert.
- Een ORDER BY-component kan niet worden gebruikt in de CTE_query_definition, behalve wanneer een TOP-component is opgegeven.
- Wanneer een CTE wordt gebruikt in een instructie die deel uitmaakt van een batch, moet de instructie voordat deze worden gevolgd door een puntkomma.
- Wanneer gebruikt in instructies voorbereid door sp_prepare, werken CTE's wordt dezelfde wijze als andere SELECT-instructies in PDW. Echter als CTE's worden gebruikt als onderdeel van CETAS voorbereid door sp_prepare, kunt het gedrag uitstellen uit SQL Server en andere instructies PDW vanwege de manier waarop binding is geïmplementeerd voor sp_prepare. Als een selecteren dat verwijzingen CTE gebruikt een verkeerde kolom die niet in CTE bestaat, wordt de sp_prepare inactiviteit detectie van de fout, maar de fout wordt in plaats daarvan tijdens sp_execute worden gegenereerd.

## <a name="recursive-ctes"></a>Recursieve CTE 's

Recursieve CTE's worden niet ondersteund in SQL Data Warehouse.  De migraion van recursieve CTE kan enigszins voltooid en het beste proces om op te splitsen is het in meerdere stappen. U kunt meestal een lus gebruiken en een tijdelijke tabel vullen terwijl u de tussentijdse query's voor een recursieve doorlopen. Zodra de tijdelijke tabel wordt gevuld kunt u de gegevens als een set één resultaat terugkeren. Een soortgelijke benadering is gebruikt voor het oplossen van `GROUP BY WITH CUBE` in het artikel [group by-component met rollup / kubus / sets Groepeeropties][] .

## <a name="unsupported-system-functions"></a>Voor niet-ondersteunde systeemfuncties

Er zijn ook enkele systeemfuncties die niet worden ondersteund. Enkele van de belangrijkste meestal kunt u vinden in zijn gebruikt gegevensopslag zijn:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Sommige van deze problemen kan worden gewerkt rond.

## <a name="rowcount-workaround"></a>@@ROWCOUNTtijdelijke oplossing

Omgaan met onvoldoende ondersteuning voor @@ROWCOUNT, maken van een opgeslagen procedure die het laatste aantal rijen ophalen uit sys.dm_pdw_request_steps en vervolgens uitvoeren `EXEC LastRowCount` na een DML-instructie.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Volgende stappen
Zie [Transact-SQL-onderwerpen][]voor een volledige lijst van alle ondersteunde T-SQL-instructies.

<!--Image references-->

<!--Article references-->
[ANSI-joins op updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-joins op verwijderen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[samenvoegen-instructie]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INVOEGEN... RAAD]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL-onderwerpen]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[SELECTEER... IN]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by-component met rollup / kubus / sets Groepeeropties]: ./sql-data-warehouse-develop-group-by-options.md
[geneste niveaus voorbij 8]: ./sql-data-warehouse-develop-transactions.md
[bijwerken door weergaven]: ./sql-data-warehouse-develop-views.md
[gebruik van selecteren voor de toewijzing van variabele]: ./sql-data-warehouse-develop-variable-assignment.md
[geen MAX-gegevenstype voor dynamische SQL-tekenreeksen]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
