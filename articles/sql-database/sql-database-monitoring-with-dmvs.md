<properties
   pageTitle="Cmdlets voor controle Azure SQL Database via dynamische Management weergaven | Microsoft Azure"
   description="Leer hoe u prestatieproblemen opsporen en onderzoeken algemene met behulp van dynamische management weergaven om de Microsoft Azure SQL-Database te houden."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Cmdlets voor controle Azure SQL-Database dynamische management weergaven gebruiken

Microsoft Azure SQL-Database kunt een subset van dynamische management weergaven die u wilt onderzoeken van prestatieproblemen die kunnen worden veroorzaakt door geblokkeerde of langdurige query's, resource-knelpunten, slechte query-abonnementen, enzovoort. In dit onderwerp vindt u informatie over hoe u algemene prestatieproblemen detecteren met behulp van dynamische management weergaven.

SQL-Database worden gedeeltelijk ondersteund in drie categorieën van dynamische management weergaven:

- Dynamische management database-gerelateerde weergaven.
- Dynamische management Execution-gerelateerde weergaven.
- Dynamische management transactie-gerelateerde weergaven.

Zie voor gedetailleerde informatie over het beheer van dynamische weergaven, [dynamische weergaven voor Management en functies (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.

## <a name="permissions"></a>Machtigingen

In SQL-Database, query's uitvoeren van een weergave dynamische management zijn vereist voor **Weergave DATABASE staat** . De machtiging **VIEW DATABASE STATE** geeft informatie over alle objecten binnen de huidige database.
De **Weergave DATABASE staat** als toestemming wilt verlenen aan een specifieke databasegebruiker, voert u de volgende query:

```GRANT VIEW DATABASE STATE TO database_user; ```

Dynamische management weergaven retourneren in een lokale SQL Server-sessie, informatie over de status van de server. In SQL-Database retourneren deze informatie aangaande alleen de huidige logische database.

## <a name="calculating-database-size"></a>Omvang van database berekenen

De volgende query wordt de grootte van uw database (in MB):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

De volgende query wordt de grootte van afzonderlijke objecten (in MB) in uw database:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Verbindingen voor controle

U kunt de weergave [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) gebruiken om op te halen informatie over de verbindingen naar een specifieke Azure SQL Database-server en de details van elke verbinding. De weergave [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) is ook handig bij het ophalen van informatie over alle actieve gebruikersverbindingen en interne taken.
De volgende query haalt gegevens op de huidige verbinding:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Wanneer de **sys.dm_exec_requests** en **sys.dm_exec_sessions weergaven**, als u gemachtigd **Weergave DATABASE staat** op de database wordt uitgevoerd, ziet u alle uitvoeren sessies op de database. anders ziet u alleen de huidige sessie.

## <a name="monitoring-query-performance"></a>Queryprestaties controleren

Traag of lang uitgevoerd query's kunt aanzienlijk systeembronnen in beslag nemen. In deze sectie wordt beschreven hoe dynamische management weergaven gebruiken om te bepalen van een paar algemene query prestatieproblemen. Een verwijzing oudere maar nog steeds handig voor probleemoplossing, is het artikel [Problemen oplossen van prestatieproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) op Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Bovenste N-query's zoeken

Het volgende voorbeeld retourneert informatie over de bovenste vijf query's rangschikking door gemiddelde CPU-tijd. In dit voorbeeld is de query's op basis van de query-hash, samengevoegd zodat logisch equivalente query's zijn gegroepeerd op hun resourceverbruik cumulatieve.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Cmdlets voor controle geblokkeerde query 's

Langzame netwerkverbinding of langdurige query's kunnen bijdragen aan overbelasting verbruik en het gevolg was van geblokkeerde query's. De oorzaak van het blokkeren van kan zijn slechte toepassingsontwerp, ongeldige query-abonnementen, het gebrek aan handige indexen, enzovoort. U kunt de weergave sys.dm_tran_locks gebruiken voor informatie over de huidige vergrendelen activiteit in uw Azure SQL-Database. Bijvoorbeeld code, Zie [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Controle van query-abonnementen

Een abonnement niet efficiënt query mogelijk ook processorgebruik vergroten. Het volgende voorbeeld wordt de weergave [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) om te bepalen welke query gebruikt de meest cumulatieve CPU.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Zie ook

[Inleiding tot SQL-Database](sql-database-technical-overview.md)
