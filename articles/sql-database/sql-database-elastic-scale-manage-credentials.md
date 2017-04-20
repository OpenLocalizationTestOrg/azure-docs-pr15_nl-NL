<properties 
    pageTitle="Beheer van referenties in de bibliotheek van de client elastische database | Microsoft Azure" 
    description="Het instellen van het juiste niveau van de referenties van beheerder om alleen-lezen, voor elastische database-apps" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Referenties gebruikt voor toegang tot de bibliotheek van de client elastische Database

De [elastische Database clientbibliotheek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) wordt drie verschillende soorten referenties gebruikt voor toegang tot de [shard kaart manager](sql-database-elastic-scale-shard-map-management.md). Afhankelijk van de nodig, gebruikt u de referentie met het laagste niveau van access die worden mogelijke.

* **Beheer van referenties**: voor het maken of bewerken van een manager van de map shard. (Zie de [woordenlijst](sql-database-elastic-scale-glossary.md).) 
* **Access-referenties**: voor toegang tot een bestaande shard kaart manager voor informatie over shards.
* **Verbindingsreferenties**: verbinding maken met shards. 

Zie ook [databases beheren en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Over het beheer van referenties

Management referenties worden gebruikt om een object [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) voor toepassingen die werken met shard kaarten te maken. (Bijvoorbeeld [een shard met hulpmiddelen voor databases elastische toevoegen](sql-database-elastic-scale-add-a-shard.md) en [gegevens afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)Zie) De gebruiker van de bibliotheek van de client elastische schaal wordt gemaakt van de SQL-gebruikers en SQL-aanmeldingen en zorgt ervoor dat elk de alleen-lezen/schrijven-machtigingen voor de database voor globale shard en ook alle shard-databases wordt verleend. Deze referenties worden gebruikt voor het behoud van de globale shard-kaart en de toewijzingen lokale shard wanneer wijzigingen in de map shard worden uitgevoerd. Gebruik bijvoorbeeld de referenties management om het object shard kaart manager te maken (met [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

De variabele **smmAdminConnectionString** is een verbindingsreeks met de referenties beheren. De gebruikersnaam en wachtwoord biedt alleen-lezen/schrijven toegang tot zowel shard kaart database als afzonderlijke shards. De verbindingsreeks management bevat ook de servernaam en de databasenaam voor de database voor globale shard. Hier volgt een typische verbindingsreeks daartoe:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Gebruik geen waarden in de vorm van "username@server"—instead alleen gebruik de waarde "gebruikersnaam".  Dit komt doordat referenties moeten werken met zowel de shard kaart manager-database en de afzonderlijke shards, die mogelijk op verschillende servers.

## <a name="access-credentials"></a>Access-referenties
  
Gebruik de referenties met alleen-lezen machtigingen voor de toewijzing globale shard bij het maken van een shard kaart manager in een toepassing die wordt shard kaarten niet beheren. De informatie opgehaald uit de globale shard kaart onder deze referenties worden gebruikt voor [gegevens-afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en om te vullen de cache van de map shard op de client. De referenties zijn beschikbaar via het patroon dat dezelfde oproep naar **GetSqlShardMapManager** , zoals hierboven: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Houd rekening met het gebruik van de **smmReadOnlyConnectionString** om aan het gebruik van verschillende referenties voor deze toegang namens **niet-beheerders** weer te geven: deze referenties moeten schrijfmachtigingen niet op de kaart globale shard geven. 

## <a name="connection-credentials"></a>Verbindingsreferenties 

Aanvullende referenties nodig zijn bij het gebruik van de methode [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) voor toegang tot een shard die is gekoppeld aan een sleutel sharding. Deze referenties moeten machtigingen voor alleen-lezen toegang tot de lokale shard kaart tabellen die zich op de shard. Dit is nodig om uit te voeren verbinding validatie van gegevens-afhankelijke routering voor de shard. Dit codefragment kan de toegang tot gegevens in de context van de afhankelijke omleiding van gegevens: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

In dit voorbeeld bevat **smmUserConnectionString** de verbindingsreeks voor de gebruikersreferenties. Hier volgt een typische verbindingsreeks gebruikersreferenties voor Azure SQL-DB: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Net als met de beheerdersreferenties kan niet worden waarden in de vorm van "username@server". Alleen gebruiken in plaats daarvan "gebruikersnaam".  Houd er ook rekening mee dat de verbindingsreeks bevat geen een servernaam en de naam van de database. Dat komt doordat de **OpenConnectionForKey** -oproep wordt automatisch de verbinding met de juiste shard op basis van de sleutel doorverwezen. De naam van de database en de servernaam worden dus niet gegeven. 

## <a name="see-also"></a>Zie ook
[Databases en aanmeldingen in Azure SQL-Database beheren](sql-database-manage-logins.md)

[Beveiliging van uw SQL-Database](sql-database-security.md)

[Aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 