<properties 
    pageTitle="Meerdere shard query's uitvoeren | Microsoft Azure" 
    description="Query's uitvoeren in shards met de bibliotheek van de client elastische database." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Meerdere shard query's uitvoeren

## <a name="overview"></a>Overzicht

Met de [Hulpmiddelen voor databases elastische](sql-database-elastic-scale-introduction.md), kunt u een laptopgeheugen databaseoplossingen maken. **Meerdere shard query's uitvoeren** wordt voor taken zoals gegevens verzamelen/rapportage die is vereist dat u een query die zich uitstrekt over meerdere shards gebruikt. (Contrast dit naar [gegevens-afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), waarmee al uw werk op een enkele shard.) 

## <a name="overview"></a>Overzicht

1. Een [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) of [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) met de [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), de [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)of de methode [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) verkrijgen. Zie [**het bouwen van een ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) en [**u een RangeShardMap of ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Een object **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** maken.
2. Maak een **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Stel de **[eigenschap CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** op een opdracht T-SQL.
3. De opdracht uitvoeren door te bellen van de **[ExecuteReader methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Bekijk de resultaten met de **[MultiShardDataReader class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Voorbeeld

De volgende code ziet u het gebruik van meerdere shard query's uitvoeren met een bepaald **ShardMap** *myShardMap*met de naam. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Een belangrijke verschil is het maken van meerdere shard verbindingen. Waar **SqlConnection** op één database werkt, gaat de **MultiShardConnection** een ***verzameling shards*** als invoer. De verzameling shards van een kaart shard vullen. De query wordt vervolgens op de verzameling shards **UNION ALL** semantiek gebruiken voor het samenstellen van een enkel resultaat van de algehele uitgevoerd. (Optioneel) kan de naam van de shard waar de rij afkomstig van zijn worden toegevoegd aan de uitvoer op basis van de eigenschap **ExecutionOptions** weer te geven. 

Opmerking de oproep door naar **myShardMap.GetShards()**. Deze methode alle shards opgehaald uit de map shard en een eenvoudige manier om een query uitvoeren in alle relevante databases. De verzameling shards voor een query met meerdere shard verfijnd worden kan verder door een query LINQ in te voeren via de verzameling geretourneerd uit de oproep door naar **myShardMap.GetShards()**. In combinatie met het beleid gedeeltelijke resultaten, heeft de huidige capaciteit bij het zoeken van meerdere shard zijn ontworpen voor gebruik voor tientallen maximaal honderden shards.

Een beperking voor het uitvoeren van query's met meerdere shard is momenteel het gebrek aan validatie voor shards en shardlets die worden doorzocht. Terwijl gegevens-afhankelijke routering wordt gecontroleerd of een bepaald shard deel uit van de kaart shard op het moment van query's uitvoeren, voer query's met meerdere shard dit selectievakje niet. Dit kan leiden tot meerdere shard-query uitvoeren op databases die zijn verwijderd uit de map shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Meerdere shard query's en gesplitste samenvoegen bewerkingen

Query's met meerdere shard controleren niet of shardlets op de aangevraagde database doorlopende gesplitste samenvoegen bewerkingen deelnemen. (Zie [schaal hulpprogramma voor het elastische Database splitsen samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).) Dit kan leiden tot inconsistenties waar rijen uit de dezelfde shardlet voor meerdere databases in dezelfde query met meerdere shard weergeven. Houd rekening met deze beperkingen en houd rekening met draining doorlopende gesplitste samenvoegen bewerkingen en wijzigingen in de map shard tijdens het uitvoeren van query's met meerdere shard.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Zie ook
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** klassen en methoden.


Het beheren van shards met de [elastische Database client-bibliotheek](sql-database-elastic-database-client-library.md). Een naamruimte [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) vindt u de mogelijkheid om query's in meerdere shards met een enkele query en het resultaat bevat. Deze biedt een opvragen abstraction via een verzameling shards. Het biedt ook alternatieve execution beleidsregels, met name gedeeltelijke resultaten te handelen fouten bij het opvragen over veel shards.  

 