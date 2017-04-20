<properties 
    pageTitle="Azure SQL-elastische schaal Veelgestelde vragen over | Microsoft Azure" 
    description="Veelgestelde vragen over elastische schaal van Azure SQL-Database." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Elastische Databasehulpmiddelen Veelgestelde vragen 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Als ik heb een enkel-tenant per shard en geen sharding-toets, hoe ik vullen de sharding-toets voor de info schema?

Het schema info-object wordt alleen gebruikt voor het samenvoegen van scenario's splitsen. Als een toepassing inherent één-tenant is, hoeft niet het hulpmiddel gesplitste samenvoegen en er is dus niet nodig om het schema info-object te vullen.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Ik heb een database ingericht en ik heb al een Manager van de Map Shard, hoe kan ik deze nieuwe database registreren als een shard?

Zie **[een shard aan een toepassing via de bibliotheek van de client elastische database toevoegen](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Hoeveel kost het elastische Databasehulpmiddelen?

Met de bibliotheek van de client elastische database wordt niet in rekening gebracht de kosten. Kosten toenemen alleen voor de Azure SQL-databases die u gebruikt voor shards en de Manager van de Map Shard, evenals de web/werknemer rollen die u voor het hulpmiddel voor het samenvoegen van gesplitste inrichten.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Waarom worden mijn aanmeldingsreferenties niet werkt wanneer ik een shard vanuit een andere server toevoegen?
Gebruik geen referenties in de vorm van "gebruiker ID=username@servername”, gebruiken in plaats daarvan ' gebruikers-ID = gebruikersnaam".  Zorg ook dat de aanmelding "gebruikersnaam" machtigingen voor de shard heeft.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Heb ik nodig om te maken van een Manager van de Map Shard en shards vullen telkens wanneer ik mijn toepassingen starten?

Nee, het maken van de Manager in Shard Map (bijvoorbeeld **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) wordt één keer.  Uw toepassing moet de oproep **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** gebruiken tijdens het starten van een toepassing.  Er moet slechts één van deze oproep per toepassingsdomein.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Ik heb vragen over het gebruik van elastische databasehulpmiddelen, hoe kom ik aan deze beantwoord? 

Neem contact maken met ons op het [forum van Azure SQL-Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Wanneer u wordt er een database-verbinding met een sleutel sharding, maar ik kan nog steeds querygegevens voor andere sharding toetsen op de dezelfde shard.  Is dit standaard zo?

De elastische schaal-API's geeft u een verbinding met de juiste database voor uw sleutel sharding, maar geen sharding belangrijke filteren.  Toevoegen aan uw query toevoegen aan de omvang beperken tot de meegeleverde sharding-toets, **waar** componenten indien nodig.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kan ik een andere versie van Azure-Database voor elke shard gebruiken in mijn shard instellen?

Ja, een shard is een afzonderlijke database en kan dus één shard mogelijk een Premium-editie terwijl een andere Standard edition worden. Bovendien kan de editie van een shard omhoog of omlaag schalen meerdere keren tijdens de levensduur van de shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Het inrichten van gesplitste samenvoegen hulpmiddel bevat (of verwijderen) op een database tijdens een bewerking splitsen of samenvoegen? 

Nee. De doeldatabase met het juiste schema moet bestaan en worden geregistreerd bij de Manager van de Map Shard voor bewerkingen **splitsen** .  Voor het **samenvoegen** van gegevens, moet u de shard verwijderen uit de manager van de map shard en verwijder vervolgens de database.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 