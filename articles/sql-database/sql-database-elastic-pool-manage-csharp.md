<properties
    pageTitle="Bewaken en beheren van een toepassingen elastische database met C# | Microsoft Azure"
    description="Gebruik C#-database-technieken voor het beheren van een Azure SQL-Database elastische database-toepassingen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Bewaken en beheren van een toepassingen elastische database met C & #x 23; 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Informatie over het beheren van een [groep elastische database](sql-database-elastic-pool.md) met C & #x 23;. 

>[AZURE.NOTE] Veel nieuwe functies van de SQL-Database worden alleen ondersteund wanneer u het [Azure resourcemanager implementatiemodel](../azure-resource-manager/resource-group-overview.md), gebruikt zodat u altijd de meest recente moet gebruiken **Azure SQL Database Management bibliotheek voor .NET ([documenten](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. De oudere [klassieke implementatie model - bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden ondersteund voor achterwaartse compatibiliteit alleen, zodat we raden u aan om de nieuwere resourcemanager op basis van bibliotheken.

U voltooit de stappen in dit artikel, moet u de volgende items:

- Een elastische toepassingen (de groep die u wilt beheren). Een groep, Zie [maken een toepassingen elastische database met C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Zie voor een gratis exemplaar van Visual Studio, de pagina [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen

U kunt een zelfstandige database verplaatsen in- of uitzoomen op een groep.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Lijst met databases in een elastische toepassingen

Als u wilt ophalen alle databases in een groep, belt u de methode [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) .

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Instellingen van de prestaties van een groep wijzigen

De eigenschappen van toepassingen bestaande worden opgehaald. Wijzig de waarden en de methode CreateOrUpdate uitgevoerd.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latentie van elastische groep bewerkingen

- Het min-eDTUs per database of max eDTUs per database meestal wijzigen is voltooid in vijf minuten of minder.
- Tijd om te wijzigen van de grootte van de groep (eDTUs), is afhankelijk van de totale grootte van alle databases in de groep. Wijzigingen gemiddelde 90 minuten of minder per 100 GB. Als bijvoorbeeld de totale ruimte van alle databases in de groep 200 GB, klikt u vervolgens de verwachte latentie is voor het wijzigen van de groep eDTU per toepassingen is 3 uur of minder.




## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen](sql-database-develop-error-messages.md).
- [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management-API 's](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Een nieuwe elastische database-toepassingen maken met C#](sql-database-elastic-pool-create-csharp.md)
- [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md)
- Zie [schaal af met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen gebruiken als u wilt schalen, verplaats gegevens, query, of transacties maken.

