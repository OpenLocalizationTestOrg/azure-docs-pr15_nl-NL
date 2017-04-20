<properties 
    pageTitle="Toevoegen van een shard met hulpmiddelen voor databases elastische | Microsoft Azure" 
    description="Het gebruik van elastische schaal-API's nieuwe shards toevoegen aan een shard instellen." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Een shard met hulpmiddelen voor databases elastische toevoegen

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Een shard voor een nieuw bereik of de toets toevoegen  

Toepassingen moeten vaak gewoon toevoegen nieuwe shards voor het verwerken van gegevens die wordt verwacht van nieuwe sleutels of belangrijke bereiken, voor een shard-map die al bestaat. Bijvoorbeeld een toepassing een laptopgeheugen per Tenant-ID mogelijk moet u een nieuwe shard inrichten voor een nieuwe tenant of een laptopgeheugen maandelijks van gegevens mogelijk moet u een nieuwe shard deze is ingericht vóór het begin van elke nieuwe maand. 

Als het nieuwe bereik sleutelwaarden niet al deel van een bestaande toewijzing uitmaakt, is heel eenvoudig naar de nieuwe shard toevoegen en de nieuwe sleutel of een bereik aan die shard koppelen. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Voorbeeld: een shard en bijbehorende bereik toevoegen aan een bestaande shard-kaart
In dit voorbeeld wordt de [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) de [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) methoden, en Hiermee maakt u een exemplaar van de klasse [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . Klik in het onderstaande voorbeeld heeft een database met de naam **sample_shard_2** en alle benodigde schemaobjecten erin zijn gemaakt waarin bereik [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Als alternatief, kunt u Powershell gebruiken om te maken van een nieuwe Shard kaart Manager. Een voorbeeld vindt u [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Een shard voor een leeg gedeelte van een bestaand bereik toevoegen  

In bepaalde omstandigheden kan u mogelijk hebt al een bereik aan een shard toegewezen en deze gedeeltelijk worden gevuld met gegevens, maar u nu wilt komende gegevens doorgestuurd naar een andere shard. Zoals u shard op het dagenbereik en al toegewezen 50 dagen moet u een shard, maar op dag 24, die u wilt toekomstige gegevens in een ander shard terechtkomt. Het elastische database [splitsen samenvoegen hulpmiddel](sql-database-elastic-scale-overview-split-and-merge.md) deze bewerking kan uitvoeren, maar als verplaatsing van gegevens niet nodig is (bijvoorbeeld gegevens voor het bereik van dagen, [25, 50), dat wil zeggen, dag 25 op 50 exclusief, inclusief nog niet bestaat) u kunt dit uitvoeren geheel rechtstreeks met de Shard kaart Management API's.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Voorbeeld: een bereik splitsen en het toewijzen van het lege deel aan een shard toegevoegde

Een database met de naam "sample_shard_2" en alle benodigde schemaobjecten erin zijn gemaakt.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Belangrijk**: Gebruik deze techniek alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is.  De bovenstaande methoden controleren niet gegevens voor het bereik worden verplaatst, zodat het beste controles opnemen in uw code.  Als rijen in het bereik dat wordt verplaatst bestaat, wordt de verdeling van de werkelijke gegevens niet overeenkomen met de bijgewerkte shard-kaart. Gebruik het [hulpmiddel gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) om de bewerking in plaats daarvan in dat geval.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
