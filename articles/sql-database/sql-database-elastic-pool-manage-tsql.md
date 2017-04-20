<properties 
    pageTitle="Maken of een Azure SQL-database verplaatsen naar een elastische toepassingen met T-SQL | Microsoft Azure" 
    description="T-SQL Azure SQL-database maken in een elastische van toepassingen gebruiken. Of de datbase verplaatsen en afmelden bij de groepen met T-SQL." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Bewaken en beheren van een toepassingen elastische database met Transact-SQL  

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Gebruik de opdrachten [Create Database (Azure SQL-Database)](https://msdn.microsoft.com/library/dn268335.aspx) en [Database(Azure SQL Database) wijzigen](https://msdn.microsoft.com/library/mt574871.aspx) om te maken en databases te verplaatsen in- en afmelden bij elastische toepassingen. De elastische toepassingen moet bestaan voordat u deze opdrachten kunt gebruiken. Deze opdrachten van invloed zijn op alleen databases. Het maken van nieuwe groepen en de instelling van de eigenschappen van toepassingen (zoals min en max eDTUs) kunnen niet worden gewijzigd met T-SQL-opdrachten.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Een nieuwe database maken in een elastische toepassingen
Gebruik de opdracht DATABASE maken met de optie SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Alle databases in een elastische toepassingen overnemen de servicelaag van de elastische toepassingen (Basic, standaard, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Een database verplaatsen tussen elastische van toepassingen
Gebruik de opdracht DATABASE wijzigen met de wijzigen en stel SERVICE\_doelstelling optie als ELASTISCHE\_RESOURCEGROEP; Stel de naam op de naam van de doel-toepassingen.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen 
Gebruik de opdracht DATABASE wijzigen met de wijzigen en stel SERVICE\_doelstelling optie ELASTIC_POOL; Stel de naam op de naam van de doel-toepassingen.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Een database verplaatsen uit een elastische toepassingen
Gebruik de opdracht ALTER DATABASE en de SERVICE_OBJECTIVE instellen op een van de prestaties (S0, S1, enzovoort).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lijst met databases in een elastische toepassingen
Gebruik de [sys.database\_service \_doelstellingen weergave](https://msdn.microsoft.com/library/mt712619) voor een overzicht van alle databases in een elastische toepassingen. Meld u aan bij de hoofdlijst database om query's in de weergave.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Resource gebruiksgegevens ophalen voor een groep

Gebruik de [sys.elastic\_groep \_resource \_stat weergave](https://msdn.microsoft.com/library/mt280062.aspx) om te bekijken van de resource gebruiksstatistieken van een elastische toepassingen op een logische server. Meld u aan bij de hoofdlijst database om query's in de weergave.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Resourcegebruik krijgen voor een elastische-database

Gebruik de [sys.dm\_ db\_ resource\_stat weergave](https://msdn.microsoft.com/library/dn800981.aspx) of [sys.resource \_stat weergave](https://msdn.microsoft.com/library/dn269979.aspx) om te bekijken van de resource gebruiksstatistieken van een database in een elastische toepassingen. Dit proces is vergelijkbaar met query's uitvoeren Resourcegebruik voor elke één database.

## <a name="next-steps"></a>Volgende stappen

Na het maken van een elastische database-toepassingen, kunt u Elastische databases in de groep beheren door elastische taken te maken. Elastische taken vergemakkelijken actieve T-SQL-script uitvoert op een willekeurig aantal databases in de groep. Zie [overzicht van elastische database](sql-database-elastic-jobs-overview.md)voor meer informatie. 

Zie [schaal af met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen gebruiken als u wilt schalen, verplaats gegevens, query, of transacties maken.
