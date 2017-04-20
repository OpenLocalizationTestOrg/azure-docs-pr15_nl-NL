<properties
   pageTitle="Gelijktijdigheid en werkbelasting management in SQL Data Warehouse | Microsoft Azure"
   description="Inzicht in Azure SQL Data Warehouse gelijktijdigheid en werkbelasting beheer voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Gelijktijdigheid en werkbelasting management in SQL Data Warehouse

Overzichtelijk om prestaties te bieden bij het op schaal, Microsoft Azure SQL Data Warehouse Hiermee kunt bepalen u bij gelijktijdigheid niveaus en resourcetoewijzingen zoals geheugen en Processorsnelheid prioriteit. In dit artikel maakt u kennis met de concepten van gelijktijdigheid en werkbelasting management, waarin wordt uitgelegd hoe beide voorzieningen zijn geïmplementeerd en hoe u ze in uw BTW-records voor de gegevens kan bepalen. SQL Data Warehouse werkbelasting management is bedoeld om u te helpen u ondersteuning voor meerdere gebruikers omgevingen. Het is niet bedoeld voor meerdere tenant werkbelasting.

## <a name="concurrency-limits"></a>Limieten voor gelijktijdigheid

SQL Data Warehouse kunnen maximaal 1024 verbindingen. Alle 1.024 verbindingen kunnen gelijktijdig query's verzenden. SQL Data Warehouse kan echter optimale doorvoersnelheid, wachtrij sommige query's om ervoor te zorgen dat elke query een minimale geheugen verlenen ontvangt. Queuing plaatsvindt tijdens het uitvoeren van een query. Door de wachtrij plaatsen query's wanneer gelijktijdigheid limieten worden bereikt, kunt SQL Data Warehouse vergroten totale doorvoer door ervoor te zorgen dat actieve query's toegang tot ernstige benodigde geheugen resources krijgen.  

Bij gelijktijdigheid limieten worden bepaald door twee concepten: *gelijktijdige query's* en *gelijktijdigheid sleuven*. Voor een query uitvoeren, moet deze uit te voeren in zowel de limiet van de query bij gelijktijdigheid en de toewijzing van gelijktijdigheid slot.

- Gelijktijdige query's zijn de query's uitvoeren op hetzelfde moment. SQL Data Warehouse ondersteunt maximaal 32 gelijktijdige query's op de grotere DWU.
- Bij gelijktijdigheid sleuven zijn toegewezen op basis van DWU. Elke 100 DWU biedt 4 gelijktijdigheid sleuven. Bijvoorbeeld een DW100 toegewezen 4 gelijktijdigheid sleuven en DW1000 toegewezen 40. Beide query's gebruikt een of meer gelijktijdigheid sleuven, afhankelijk van de [resource class](#resource-classes) van de query. Query uitvoeren in de klas van de resource smallrc verbruikt één gelijktijdigheid boek. Query uitvoeren in een hogere resource klasse verbruikt meer gelijktijdigheid slots.

De volgende tabel beschrijft de limieten voor beide gelijktijdige query's en gelijktijdigheid sleuven op de verschillende DWU grootte.

### <a name="concurrency-limits"></a>Limieten voor gelijktijdigheid

|  DWU   | Max gelijktijdige query 's  | Bij gelijktijdigheid sleuven toegewezen |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Als een van deze drempels wordt voldaan, zijn de nieuwe query's in de wachtrij en de worden uitgevoerd op basis formule eerst hebt aangemeld.  Als een query's is voltooid en het aantal query's en sleuven onder de limieten valt, wordt in de wachtrij query's worden uitgebracht. 

> [AZURE.NOTE]  *Selecteer* query's uitvoeren uitsluitend op dynamische management (DMVs) catalogus taakweergaven of niet worden geregeld door een van de beperkingen bij gelijktijdigheid. Het systeem ongeacht het aantal query's uitvoeren op is geïnstalleerd, kunt u controleren.

## <a name="resource-classes"></a>Resource-klassen

Resource klassen help die u bepalen geheugentoewijzing en CPU-bewerkingen gegeven aan een query. U kunt vier resource klassen toewijzen aan een gebruiker in de vorm van *databaserollen*. De vier resource-klassen zijn **smallrc**, **mediumrc** **largerc**en **xlargerc**. Gebruikers in smallrc krijgen een kleinere hoeveelheid geheugen en kunnen profiteren van hoger gelijktijdigheid. In tegenstelling krijgen gebruikers die zijn toegewezen aan xlargerc grote hoeveelheden geheugen en daarom zijn minder van hun query's kunnen tegelijkertijd worden uitgevoerd.

Standaard wordt elke gebruiker lid zijn van de klas kleine resource, smallrc. De procedure `sp_addrolemember` wordt gebruikt om uit te breiden de klas resource en `sp_droprolemember` wordt gebruikt om te verkleinen van de resource-klasse. Deze opdracht zou bijvoorbeeld de klasse resource van loaduser naar largerc vergroten:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Er is een goede gewoonte permanent gebruikers toewijzen aan een resource-klasse in plaats van hun klassen resource te wijzigen. Bijvoorbeeld maken laadtijd aan gegroepeerd columnstore tabellen voor hogere kwaliteit indexen wanneer meer geheugen toegewezen. Om ervoor te zorgen dat laadtijd toegang tot hogere geheugen hebben, een gebruiker specifiek voor het laden van gegevens maken en permanent deze gebruiker toewijzen aan een hogere resource class.

Er zijn een paar soorten query's die niet van een grotere geheugentoewijzing gebruikmaken kunnen. Het systeem wel hun class resourcetoewijzing negeren en uitgevoerd altijd in plaats daarvan deze query's in de klas kleine resource. Als deze query's altijd in de klas kleine resource uitvoeren, kunnen ze uitgevoerd wanneer bij gelijktijdigheid sleuven onder druk zijn en ze meer slots dan nodig won't gebruiken. Zie [Resource class uitzonderingen](#query-exceptions-to-concurrency-limits) voor meer informatie.

Een paar meer informatie over resource class:

- Machtigingen *wijzigen van de rol* is vereist voor het wijzigen van de resource klas van een gebruiker.  
- Hoewel u een gebruiker aan een of meer van de hoger resource klassen toevoegen kunt, duurt het gebruikers op de kenmerken van de hoogste resource klas waaraan ze zijn toegewezen. Als een gebruiker is toegewezen aan zowel mediumrc als largerc, is de hoger resource klas (largerc) dat wil zeggen de resource-klasse die wordt van kracht.  
- De resource klasse van de systeem-beheerder kan niet worden gewijzigd.

Zie voor een gedetailleerd voorbeeld [wijzigen gebruiker resource class voorbeeld](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Geheugentoewijzing

Zijn er voor- en nadelen van een gebruiker resource class vergroten. Een klasse resource voor een gebruiker met groter wordende geeft de toegang van hun query's naar het meer geheugen kan betekenen query's sneller worden uitgevoerd.  Hogere resource klassen verkleinen echter ook het aantal gelijktijdige query's die kan worden uitgevoerd. Dit is de verhouding tussen de toewijzing van grote hoeveelheden geheugen aan één query of andere query's, die ook geheugentoewijzingen moeten, gelijktijdig toestaan. Als u één gebruiker krijgt hoog toewijzingen geheugen voor een query, worden voor andere gebruikers geen toegang tot die hetzelfde geheugen een query uit te voeren.

De volgende tabel uit kaarten het geheugen toegewezen aan elke verdeling door DWU- en resourcekalenders klasse.

### <a name="memory-allocations-per-distribution-mb"></a>Geheugentoewijzingen per verdeling (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1600   |
| DW500  |   100   |    400   |    800  |  1600   |
| DW600  |   100   |    400   |    800  |  1600   |
| DW1000 |   100   |    800   |  1600  |  3200   |
| DW1200 |   100   |    800   |  1600  |  3200   |
| DW1500 |   100   |    800   |  1600  |  3200   |
| DW2000 |   100   |  1600   |  3200  |  6,400   |
| DW3000 |   100   |  1600   |  3200  |  6,400   |
| DW6000 |   100   |  3200   |  6,400  |  12,800  |

In de bovenstaande tabel ziet u dat een query uitvoeren op een DW2000 in de klas van de resource xlargerc toegang tot 6400 MB geheugen binnen elk van de 60 verdeelde databases zou hebben.  In SQL Data Warehouse, zijn er 60 onderzoeken. Als u wilt berekenen van de totale geheugentoewijzing voor een query in een bepaalde bronklasse, moeten de bovenstaande waarden daarom zijn vermenigvuldigd met 60.

### <a name="memory-allocations-system-wide-gb"></a>Geheugen toewijzingen systeeminstellingen (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

In deze tabel met systeemproblemen geheugentoewijzingen, kunt u zien dat een query uitvoeren op een DW2000 in de klas van xlargerc resource is toegewezen aan een totaal van 375 GB geheugen (6400 MB * 60 onderzoeken / 1.024 converteren naar GB) via het geheel van uw SQL Data Warehouse.

## <a name="concurrency-slot-consumption"></a>Bij gelijktijdigheid slot verbruik

SQL Data Warehouse verleent meer geheugen tot query's met hogere resource lessen. Geheugen is een vaste bron.  Daarom de meer geheugen toegewezen per query, de minder gelijktijdige query's kunnen uitvoeren. De volgende tabel herhaalt alle van de vorige concepten in één weergave ziet u het aantal bij gelijktijdigheid beschikbaar door DWU en de sleuven verbruikt door elke categorie resource.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Toewijzing en verbruik van gelijktijdigheid sleuven

|  DWU   | Maximumaantal gelijktijdige query 's  | Bij gelijktijdigheid sleuven toegewezen | Sleuven die worden gebruikt door smallrc |  Sleuven die worden gebruikt door mediumrc |  Sleuven die worden gebruikt door largerc |  Sleuven die worden gebruikt door xlargerc |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


In deze tabel ziet u de die voorlopig SQL Data Warehouse zoals DW1000 maximaal 32 gelijktijdige query's en een totaal van 40 gelijktijdigheid sleuven worden toegewezen. Als u alle gebruikers worden uitgevoerd in smallrc, zou 32 gelijktijdige query's worden toegestaan, omdat elke query 1 gelijktijdigheid slot wilt gebruiken. Als alle gebruikers op een DW1000 zijn uitgevoerd in mediumrc en gelijktijdigheid beperkt tot 5 gebruikers is elke query zou 800 MB worden toegewezen per verdeling voor een totaal geheugentoewijzing van 47 GB per query (40 gelijktijdigheid sleuven / 8 sleuven per gebruiker mediumrc).

## <a name="query-importance"></a>Query urgentie

SQL Data Warehouse implementeert resource klassen met behulp van de werklast groepen. Er zijn acht werkbelasting groepen die het gedrag van de resource klassen over de verschillende DWU grootte controleren in totaal. Voor elke DWU, SQL Data Warehouse gebruikt alleen vier van de groepen acht werkbelasting. Dit een goed idee omdat elke groep werkbelasting is toegewezen aan een van de vier resource klassen: smallrc, mediumrc, largerc, of xlargerc. De urgentie van een inzicht krijgen in de groepen werkbelasting is dat sommige van deze groepen werkbelasting zijn ingesteld op hogere *prioriteit heeft*. Urgentie wordt gebruikt voor CPU plannen. Query's uitvoeren met een hoge prioriteit krijgt drie keer meer CPU-bewerkingen dan bij urgentie Normaal. Bij gelijktijdigheid slot toewijzingen bepalen daarom ook CPU prioriteit. Wanneer een query 16 of meer sleuven verbruikt, wordt deze uitgevoerd als belangrijk.

De volgende tabel ziet u de urgentie toewijzingen voor elke groep werkbelasting.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Toewijzingen van de werklast groep aan bij gelijktijdigheid sleuven en urgentie

| Werkbelasting groepen | Bij gelijktijdigheid slot toewijzing | MB / verdeling | Urgentie toewijzing |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Gemiddeld       |
| SloDWGroupC01   |            2             |         200       |       Gemiddeld       |
| SloDWGroupC02   |            4             |         400       |       Gemiddeld       |
| SloDWGroupC03   |            8             |         800       |       Gemiddeld       |
| SloDWGroupC04   |           16             |       1600       |       Hoog         |
| SloDWGroupC05   |           32             |       3200       |       Hoog         |
| SloDWGroupC06   |           64             |       6,400       |       Hoog         |
| SloDWGroupC07   |          128             |      12,800       |       Hoog         |

U kunt zien die een DW500 worden gebruikt 1, 4, 8 of 16 gelijktijdigheid sleuven voor smallrc, mediumrc, largerc en xlargerc, respectievelijk uit de grafiek **toewijzing en verbruik van gelijktijdigheid sleuven** . U kunt deze waarden opzoeken in de voorgaande grafiek om de urgentie voor elke categorie resource.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500 toewijzing van resources klassen urgentie

| Resource-klasse | Werkbelasting groep | Bij gelijktijdigheid sleuven gebruikt | MB / verdeling | Urgentie |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Gemiddeld   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Gemiddeld   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Gemiddeld   |
| xlargerc       | SloDWGroupC04  |          16            |        1600      |   Hoog     |


U kunt de volgende DMV-query gebruiken om de verschillen tussen de resource geheugentoewijzing in detail vanuit het perspectief van de resource gouverneur te bekijken of te analyseren actieve en historische gebruik van de groepen werkbelasting bij het oplossen van.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Query's die voldoen aan de beperkingen bij gelijktijdigheid

De meeste query's vallen resource klassen. Deze query's moeten binnen de beide de gelijktijdige query en gelijktijdigheid slot drempelwaarden. Een gebruiker kiezen niet uitsluiten van een query uit het model van gelijktijdigheid slot.

Als u wilt wijzen er nogmaals, ingaan op de volgende beweringen resource klassen:

- INVOEGEN-P
- UPDATE
- VERWIJDEREN
- SELECTEREN (wanneer de query's uitvoeren gebruikerstabellen)
- ALTER INDEX OPNIEUW OPBOUWEN
- ALTER INDEX HERORGANISEREN
- ALTER TABEL OPNIEUW OPBOUWEN
- INDEXEN MAKEN
- GEGROEPEERD COLUMNSTORE INDEX MAKEN
- TABEL ALS SELECTEREN (CTAS) MAKEN
- Gegevens laden
- Gegevens verkeer bewerkingen uitgevoerd door de gegevens verkeer Service (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Query uitzonderingen op bij gelijktijdigheid limieten

Sommige query's kunnen niet voldoen aan de resource klasse waarvoor de gebruiker is toegewezen. Deze uitzonderingen op de limieten gelijktijdigheid zijn doorgevoerd wanneer het geheugen resources u nodig hebt voor een bepaalde opdracht laag, vaak zijn omdat de opdracht één metagegevens wordt. Het doel van deze uitzonderingen is groter geheugentoewijzingen voor query's die worden nooit hoeft te vermijden. In dat geval wordt altijd de standaard kleine resource klasse (smallrc) gebruikt ongeacht de klas werkelijke resource is toegewezen aan de gebruiker. Bijvoorbeeld `CREATE LOGIN` altijd wordt uitgevoerd in de smallrc. De resources die nodig zijn om te voldoen aan deze bewerking zijn erg laag, zodat deze niet logisch zodat de query in het model van gelijktijdigheid slot.  Deze query's worden ook niet beperkt door de 32-gebruiker gelijktijdigheid limiet, een onbeperkt aantal deze query's op de sessielimiet voor de van 1.024 sessies omhoog kunt uitvoeren.

De volgende beweringen doen resource klassen niet van toepassing:

- MAKEN of de tabel neerzetten
- ALTER TABLE... SWITCH, splitsen of PARTITION samenvoegen
- ALTER INDEX UITSCHAKELEN
- DECORATIEVE INDEX
- MAKEN, bijwerken of DROP STATISTICS
- TABEL AFKAPPEN
- ALTER AUTORISATIE
- AANMELDING MAKEN
- MAKEN, wijzigen of DROP USER
- MAKEN, wijzigen of DROP PROCEDURE
- MAKEN of weergave neerzetten
- WAARDEN INVOEGEN
- Selecteer in het systeemweergaven en DMVs
- UITLEG
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Een voorbeeld van een gebruiker resource klassen wijzigen

1. **Maken login:** Een verbinding met **de hoofddatabase op de SQL-server waarop uw SQL Data Warehouse-database** openen en voer de volgende opdrachten uit.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Dit is een goed idee om een gebruiker maken in de hoofddatabase voor gebruikers van Azure SQL Data Warehouse. Maken van een gebruiker in het model, kan een gebruiker zich aanmeldt met hulpprogramma's zoals SSMS zonder op te geven van de naam van een database.  Daarnaast kunt u ze de Objectverkenner gebruiken om te bekijken van alle databases op een SQL server.  Zie voor meer informatie over het maken en beheren van gebruikers, [Secure een database in SQL Data Warehouse][].

2. **Maken SQL Data Warehouse gebruiker:** Een verbinding met de **SQL Data Warehouse** -database openen en voer de volgende opdracht uit.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Machtigingen verlenen:** Het volgende voorbeeld worden toegewezen `CONTROL` voor de database van **SQL Data Warehouse** . `CONTROL`aan de database is niveau het equivalent van db_owner in SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Resource class vergroten:** Gebruik de volgende query naar een gebruiker toevoegen aan een hogere werklast management rol.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Verkleinen resource class:** Gebruik de volgende query een gebruiker verwijderen uit een werkbelasting management rol.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Het is niet mogelijk is een gebruiker verwijderen uit smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>In de wachtrij query detectie en andere DMVs

U kunt de `sys.dm_pdw_exec_requests` DMV om aan te geven van query's die in een wachtrij gelijktijdigheid wachten. Query's wachten op een slot gelijktijdigheid heeft de status **geschorst**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Werkbelasting beheerrollen kunnen worden bekeken met `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

De volgende query ziet u welke de rol van elke gebruiker is toegewezen.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse heeft de volgende typen wachten:

- **LocalQueriesConcurrencyResourceType**: query's die gaan buiten het kader van gelijktijdigheid slot zitten. Functies zoals DMV query's en systeem `SELECT @@VERSION` ziet u voorbeelden van lokale query's.
- **UserConcurrencyResourceType**: query's die zich binnen het kader van gelijktijdigheid slot bevinden. Query's voor eindgebruikers tabellen vertegenwoordigen voorbeelden die in dit resourcetype wilt gebruiken.
- **DmsConcurrencyResourceType**: wacht ten gevolge van verkeer gegevensbewerkingen.
- **BackupConcurrencyResourceType**: deze wachten geeft aan dat een database is back-up gemaakt. De maximale waarde voor dit resourcetype is 1. Als meerdere back-ups hebt aangevraagd op hetzelfde moment, wordt de anderen wachtrij.

De `sys.dm_pdw_waits` DMV kan worden gebruikt om te zien welke resources op een nieuw vergaderverzoek wacht.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

De `sys.dm_pdw_resource_waits` DMV ziet u alleen de resource wacht dat door een opgegeven query. Resource-wachttijd metingen alleen de tijd wachten op resources te verstrekken, in tegenstelling tot de wachttijd signaal, dat wil zeggen de tijd die het duurt voordat de onderliggende SQL-servers om te plannen van de query naar de CPU.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

De `sys.dm_pdw_wait_stats` DMV kan worden gebruikt voor historische trendanalyse van wachten op.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [Secure een database in SQL Data Warehouse][]. Voor meer informatie over hoe groter resource klassen gegroepeerd columnstore index kwaliteitsverbetering, Zie [Rebuilding indexen segment kwaliteitsverbetering].

<!--Image references-->

<!--Article references-->
[Een database in SQL Data Warehouse beveiligen]: ./sql-data-warehouse-overview-manage-security.md
[Opnieuw samenstellen van indexen segment kwaliteitsverbetering]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Een database in SQL Data Warehouse beveiligen]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
