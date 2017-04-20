<properties
   pageTitle="Migreren van bestaande databases naar schalen | Microsoft Azure"
   description="Een laptopgeheugen databases als u wilt gebruiken elastische Databasehulpmiddelen door te maken van een shard kaart manager converteren"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Bestaande databases naar schalen migreren

Uw bestaande schaal bij een laptopgeheugen databases die met hulpmiddelen voor databases Azure SQL-Database (zoals de [elastische Database clientbibliotheek](sql-database-elastic-database-client-library.md)) in een handomdraai. Eerst moet u een bestaande groep van databases gebruik van de [shard toewijzen manager](sql-database-elastic-scale-shard-map-management.md)converteren. 

## <a name="overview"></a>Overzicht
Een bestaande een laptopgeheugen database migreren: 

1. Bereid de [shard kaart manager-database](sql-database-elastic-scale-shard-map-management.md).
2. Maak de map shard.
3. De afzonderlijke shards voorbereiden.  
2. Toewijzingen toevoegen aan de map shard.

Deze technieken kunnen worden geïmplementeerd via de [bibliotheek van .NET Framework-client](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)of de PowerShell-scripts op [DB in Azure SQL - Database elastische extra scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). De voorbeelden hier gebruikt de PowerShell-scripts.

Zie voor meer informatie over de ShardMapManager, [Shard management toewijzen](sql-database-elastic-scale-shard-map-management.md). Zie voor een overzicht van de elastische databasehulpmiddelen, [elastische Database functies overzicht](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>De shard kaart manager-database voorbereiden

De beheerder van de map shard is een speciale database met de gegevens als u wilt schaal-out databases beheren. U kunt een bestaande database gebruiken of een nieuwe database maken. Houd er rekening mee dat een database die fungeert als shard kaart manager niet dezelfde database als een shard moet worden. Houd er ook rekening mee dat de PowerShell-script niet de database voor u maken. 

## <a name="step-1-create-a-shard-map-manager"></a>Stap 1: Maak een shard kaart manager

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>De manager van de map shard ophalen

Na het maken, kunt u de manager van de map shard met deze cmdlet ophalen. Deze stap is nodig telkens wanneer u moet het object ShardMapManager gebruiken.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Stap 2: de shard-toewijzing maken

Moet u het type shard gegevensinstellingen wilt maken. De keuze, is afhankelijk van de databasearchitectuur: 

1. Eén tenant per database (Zie de [woordenlijst](sql-database-elastic-scale-glossary.md)voor termen,.) 
2. Meerdere tenants per database (twee typen):
    3. Lijst toewijzing
    4. Bereik toewijzing
 

Maak een map van de shard **lijst toewijzing** voor een model één-tenant. Het model één-tenant wordt toegewezen één database per pachter. Dit is een effectieve model voor SaaS ontwikkelaars zoals deze beheer eenvoudiger.

![Lijst toewijzing][1]

Het model met meerdere tenant verschillende tenants toegewezen aan één database (en kunt u groepen van tenants verdelen over meerdere databases). Gebruik dit model wanneer u elke tenant heeft kleine Gegevensbehoeften verwachten. In dit model kunt we een bereik van tenants toewijzen aan een database met behulp van **de toewijzing bereik**. 
 

![Bereik toewijzing][2]

Of u een databasemodel van meerdere tenant meerdere tenants toewijzen aan één database met behulp van een *lijst met toewijzing* kunt implementeren. Bijvoorbeeld BW1 wordt gebruikt voor het opslaan van informatie over de tenant-id 1 en 5 en DB2 gegevens voor de tenant 7 en 10-tenant worden opgeslagen. 

![Muliple tenants op één DB][3] 

**Op basis van uw keuze, kies een van de volgende opties:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Optie 1: een kaart shard voor de toewijzing van een lijst maken
Maak een shard-map met het object dat ShardMapManager. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Optie 2: een kaart shard voor een toewijzing bereik maken

Houd er rekening mee dat voor het gebruik van deze toewijzing patroon, de id van de tenant waarden moet doorlopend bereiken, en is het aanvaardbaar tussenruimte in de bereiken door het bereik gewoon overslaan bij het maken van de databases hebt.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Optie 3: De toewijzingen van de lijst op één database
Instellen van deze patroon ook, is het maken van een kaart lijst zoals weergegeven in stap 2, optie 1 vereist.

## <a name="step-3-prepare-individual-shards"></a>Stap 3: Afzonderlijke shards voorbereiden

Elke shard (database) toevoegen aan de manager van de map shard. Dit zorgt ervoor dat de afzonderlijke databases voor het opslaan van toewijzingsgegevens. Deze methode wordt uitgevoerd op elke shard.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Stap 4: Toewijzingen toevoegen

De toevoeging van toewijzingen, is afhankelijk van het soort shard-map die u hebt gemaakt. Als u een lijst-kaart hebt gemaakt, kunt u de toewijzingen van de lijst toevoegen. Als u een bereik-kaart hebt gemaakt, kunt u bereik toewijzingen toevoegen.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Optie 1: de gegevens voor een toewijzing lijst toewijzen

Wijs de gegevens door een lijst-toewijzing toe te voegen voor elke tenant.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Optie 2: de gegevens voor een toewijzing bereik toewijzen

De toewijzingen bereik voor de tenant-id bereik met alle – database koppelingen toevoegen:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Stap 4 optie 3: de gegevens voor meerdere tenants op één database toewijzen

Voor elke tenant, voert u de toevoegen-ListMapping (optie 1, hierboven). 


## <a name="checking-the-mappings"></a>De toewijzingen controleren

Informatie over de bestaande shards en de toewijzingen die is gekoppeld aan deze kan worden doorzocht volgende opdrachten gebruiken:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Overzicht

Nadat u de instellingen hebt voltooid, kunt u beginnen met het gebruik van de bibliotheek van de client elastische Database. U kunt ook [gegevens afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [meerdere shard query](sql-database-elastic-scale-multishard-querying.md)gebruiken.

## <a name="next-steps"></a>Volgende stappen


Krijgen de PowerShell-scripts van [DB elastische Azure SQL-Database hulpmiddelen voor sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

De hulpmiddelen voor zijn ook op GitHub: [Azure/elastische-db-hulpmiddelen](https://github.com/Azure/elastic-db-tools).

De gesplitste samenvoegen gebruiken om gegevens te verplaatsen naar of vanuit een model met meerdere tenant aan een model één tenant. Zie [gesplitste samenvoegen hulpmiddel](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Aanvullende informatie

Zie voor informatie over algemene gegevens architectuur patronen van meerdere tenant software-als-een-service (SaaS) databasetoepassingen, [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Vragen en functieverzoeken verzenden

Neem contact maken met ons op het [forum van de SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieverzoeken verzenden, voeg ze naar de [SQL-Database feedback-forum](https://feedback.azure.com/forums/217321-sql-database/)voor vragen.

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
