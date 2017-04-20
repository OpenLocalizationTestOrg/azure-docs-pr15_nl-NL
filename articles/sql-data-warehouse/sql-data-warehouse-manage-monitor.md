<properties
   pageTitle="Bewaak uw werkzaamheden met DMVs | Microsoft Azure"
   description="Leer hoe u uw werkzaamheden met DMVs controleren."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Uw werkzaamheden met DMVs controleren

In dit artikel wordt beschreven hoe dynamische Management weergaven (DMVs) gebruiken om te controleren van uw werkbelasting en uitvoering van de query in Azure SQL Data Warehouse onderzoeken.

## <a name="permissions"></a>Machtigingen

Als u wilt de DMVs in dit artikel een query uitvoert, moet u machtiging weergave DATABASE staat of BESTURINGSELEMENT. STATUS van de DATABASE verlenen weergave is meestal de voorkeur machtiging omdat deze dus beperktere.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitor verbindingen

Alle aanmeldingen bij SQL Data Warehouse worden geregistreerd in [sys.dm_pdw_exec_sessions][].  Deze DMV bevat de laatste 10.000 aanmeldingen.  De session_id is de primaire sleutel en opeenvolging voor elke nieuwe aanmelding is toegewezen.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Monitor met een query kan worden uitgevoerd

Alle query's die zijn uitgevoerd op SQL Data Warehouse worden geregistreerd in [sys.dm_pdw_exec_requests][].  Deze DMV bevat de laatste 10.000 query's die wordt uitgevoerd.  De request_id uniek aangeeft van beide query's en is de primaire sleutel voor deze DMV.  De request_id opeenvolging voor elke nieuwe query is toegewezen en wordt voorafgegaan door QID, staat voor de query-ID.  Query's uitvoeren in dit DMV voor een bepaald session_id, ziet u alle query's voor een bepaald aanmelding.

>[AZURE.NOTE] Opgeslagen procedures meerdere aanvragen-id's gebruikt.  Aanvraag-id's zijn in de juiste volgorde toegewezen. 

Hier vindt u stappen te volgen als u wilt onderzoeken query execution-abonnementen en -tijden voor een bepaalde query.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>STAP 1: De query die u wilt onderzoeken identificeren

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Uit de voorgaande queryresultaten, **notitie de ID aanvragen** van de query die u wilt onderzoeken.

Query's in de **onderbroken** toestand wordt in de wachtrij vanwege gelijktijdigheid limieten. Deze query's wordt ook weergegeven in de query sys.dm_pdw_waits wachten op met een soort UserConcurrencyResourceType. Zie [gelijktijdigheid en werkbelasting beheren][] voor meer informatie over limieten voor gelijktijdigheid. Query's kunnen ook voor andere doeleinden, zoals voor object vergrendelingen wachten.  Als uw query voor een resource wacht, raadpleegt u [Investigating query's wachten op resources][] verderop in dit artikel.

Om te vereenvoudigen het opzoeken van een query in de tabel sys.dm_pdw_exec_requests, [LABEL][] een opmerking toewijzen aan uw query die kan worden gezocht in de weergave sys.dm_pdw_exec_requests te gebruiken.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>STAP 2: De query-abonnement onderzoeken

Gebruik de ID aanvragen voor het ophalen van de query verdeelde SQL (DSQL) abonnement uit [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wanneer u een abonnement DSQL duurt langer dan verwacht, zijn de oorzaak een complexe abonnement met veel DSQL stappen of slechts één stap duurt erg lang.  Als het abonnement dat veel stappen met verschillende bewerkingen van verplaatsen is, kunt u het optimaliseren van uw tabel onderzoeken om te verplaatsen van gegevens beperken. De [tabel verdeling][] -artikel wordt uitgelegd waarom gegevens voor het oplossen van een query moeten worden verplaatst en wordt uitgelegd sommige verdeling strategieën om te verplaatsen van gegevens beperken.

Meer informatie over één stap, de kolom *operation_type* van de querystap langdurige verder onderzoeken en houd rekening met de **Stap Index**:

- Ga verder met stap 3a voor **SQL-bewerkingen**: OnOperation, RemoteOperation, ReturnOperation.
- Ga verder met stap 3 b voor **bewerkingen van de verplaatsing van gegevens**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>STAP 3a: SQL onderzoeken op de verdeelde-databases

Gebruik de ID aanvragen en de Index stap details ophalen uit [sys.dm_pdw_sql_requests][], met gegevens van de uitvoering van de querystap op alle verdeelde databases.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wanneer de querystap wordt uitgevoerd, kan [DBCC PDW_SHOWEXECUTIONPLAN][] worden gebruikt om op te halen van het abonnement van SQL Server-geschatte uit de cache voor het plannen van SQL Server voor de stap uitvoeren op een bepaalde verdeling.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>STAP 3 b: gegevens verplaatsen op de verdeelde databases onderzoeken

Gebruik de ID aanvragen en de Index stap gegevens over een gegevens verkeer stap uitvoeren op elke verdeling van [sys.dm_pdw_dms_workers][]ophalen.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Kijk in de kolom *total_elapsed_time* om te zien als een bepaalde verdeling aanzienlijk langer duurt dan anderen voor verplaatsing van gegevens.
- Voor de langdurige verdeling gebruikt, raadpleegt u de kolom *rows_processed* om te zien als het aantal rijen die wordt verplaatst van die verdeling aanzienlijk groter is dan anderen. Als dat het geval is, is het mogelijk dat dit scheefheid van de onderliggende gegevens.

Als de query wordt uitgevoerd, kan [DBCC PDW_SHOWEXECUTIONPLAN][] worden gebruikt om op te halen van het abonnement van SQL Server-geschatte uit de cache voor SQL Server-abonnement voor de actieve SQL-stap binnen een bepaalde verdeling.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Wachten query's controleren

Als u ontdekt dat uw query geen voortgang is aanbrengt omdat er een resource wordt gewacht, volgt een query met alle bronnen wacht op een query.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Als de query actief op resources uit een andere query wachten is, klikt u vervolgens zijn de stand **AcquireResources**.  Als de query de vereiste bronnen bevat, klikt u vervolgens zijn de staat **verleend**.

## <a name="next-steps"></a>Volgende stappen
Zie [systeemweergaven][] voor meer informatie over DMVs.
Zie [Aanbevolen procedures voor SQL Data Warehouse][] voor meer informatie over aanbevolen procedures

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Systeemweergaven]: ./sql-data-warehouse-reference-tsql-system-views.md
[Tabel-verdeling]: ./sql-data-warehouse-tables-distribute.md
[Bij gelijktijdigheid en werkbelasting beheren]: ./sql-data-warehouse-develop-concurrency.md
[Wordt onderzocht query's wachten op resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
