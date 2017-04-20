<properties
    pageTitle="Prestatie-items voor shard kaart manager"
    description="ShardMapManager klasse- en afhankelijke routeren prestatiemeteritems"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Prestatie-items voor shard kaart manager

U kunt de prestaties van een [shard kaart manager](sql-database-elastic-scale-shard-map-management.md)kunt vastleggen, met name bij gebruik van [gegevens afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md). Methoden voor de klas Microsoft.Azure.SqlDatabase.ElasticScale.Client worden items gemaakt.  

Items worden gebruikt voor het bijhouden van de prestaties van [gegevens afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) bewerkingen. Deze items zijn toegankelijk in de Monitor prestaties, onder de categorie "Elastische Database: Shard Management".

**Voor de meest recente versie:** Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Zie ook [een app voor het gebruik van de meest recente elastische database clientbibliotheek upgraden](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Vereisten voor

* Als u wilt de prestaties categorie en de items hebt gemaakt, moet de gebruiker een deel van **de lokale beheerdersgroep op de computer aan waarop de toepassing** .  

* Als u wilt maken van een exemplaar van de teller prestaties en de items bijwerken, moet de gebruiker lid zijn van de **groep Administrators** of de **Gebruikers van prestaties bewaken** . 

## <a name="create-performance-category-and-counters"></a>Prestaties categorie en items maken 

Als u wilt de items hebt gemaakt, belt u de methode CreatePeformanceCategoryAndCounters van de [ShardMapManagmentFactory klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Alleen de beheerder kan de methode uitgevoerd: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

U kunt ook gebruiken in [deze](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-script uitvoeren van de methode. De methode maakt u de volgende items:  

* **In cache toewijzingen**: aantal toewijzingen in cache opslaan voor de toewijzing shard.
*  **DDR bewerkingen/sec**: tarief van gegevens afhankelijke routeren bewerkingen voor de map shard. Dit item wordt bijgewerkt wanneer een oproep naar [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) resulteert in een verbinding met de bestemming shard. 
*  **Toewijzing opzoeken cache treffers/sec**: tarief weer dat succesvolle cache opzoeken bewerkingen voor toewijzingen in de map shard. 
*  **Toewijzing opzoeken cache mislukte/seconde**: het aantal mislukte cache opzoeken bewerkingen voor toewijzingen in de map shard.
*  **Toewijzingen toegevoegd of bijgewerkt in de cache/sec**: snelheid welke toewijzingen worden toegevoegd of bijgewerkt in cache voor de toewijzing shard. 
*  **Toewijzingen is verwijderd uit de cache/sec**: tarief waarop de toewijzingen worden verwijderd uit de cache voor de map shard. 

Prestatie-items worden gemaakt voor elke map in de cache opgeslagen shard per proces.  


## <a name="notes"></a>Notities
De volgende gebeurtenissen activeren het maken van de items:  

* Initialisatie van de [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) met [enthousiast laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), als de ShardMapManager alle kaarten shard bevat. Hierbij de [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) en de [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) methoden.
* Succesvolle opzoeken van een shard-kaart (via [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) of [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Geslaagd het maken van shard kaart met CreateShardMap().

De items zijn bijgewerkt door alle cache bewerkingen uitgevoerd op de shard kaart en de toewijzingen. Geslaagde verwijdering van de shard-kaart met DeleteShardMap () reults in verwijdering van het exemplaar van de prestatiemeteritems.  

## <a name="best-practices"></a>Aanbevolen procedures

* Het maken van de categorie van de prestaties en items moet worden uitgevoerd in slechts één keer voordat het maken van ShardMapManager object. Elke uitvoering van de opdracht CreatePerformanceCategoryAndCounters() Hiermee wist de vorige items (gegevensverlies gerapporteerd door alle instanties) en nieuwe regels worden gemaakt.  

* Prestatiemeteritem exemplaren worden gemaakt per proces. Een toepassing is vastgelopen of verwijderen van een kaart shard uit de cache treden verwijdering van de prestatiemeteritems exemplaren.  

### <a name="see-also"></a>Zie ook

[Overzicht van de functies elastische Database](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

