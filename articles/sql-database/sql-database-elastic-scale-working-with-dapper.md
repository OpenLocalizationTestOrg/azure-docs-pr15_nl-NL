<properties 
    pageTitle="Gebruik van elastische database client-bibliotheek met Dapper | Microsoft Azure" 
    description="Gebruik elastische database client-bibliotheek met Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Gebruik van elastische database client-bibliotheek met Dapper 

Dit document is voor ontwikkelaars dat aanmelding Dapper u toepassingen kunt maken, maar ook wilt Profiteer [elastische database gereedschap](sql-database-elastic-scale-introduction.md) als u wilt maken van sharding als u wilt schalen hun gegevenslaag geïmplementeerd.  In dit document ziet u de wijzigingen in Dapper-toepassingen die nodig zijn voor integratie met hulpmiddelen voor databases elastische. Klik op de elastische database shard management en afhankelijk van de gegevens voor het bericht routering onze focus is met Dapper. 

**Voorbeeld van een Code**: [elastische Databasehulpmiddelen voor Azure SQL Database - Dapper integratie](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integratie van **Dapper** en **DapperExtensions** met de bibliotheek van de client elastische database voor Azure SQL-Database gaat heel eenvoudig. Uw toepassingen kunnen gegevens afhankelijke routering door te wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) -objecten gebruiken om de oproep [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de [clientbibliotheek](http://msdn.microsoft.com/library/azure/dn765902.aspx)gebruiken. Hiermee wordt de wijzigingen in uw toepassing alleen beperkt wanneer nieuwe verbindingen worden gemaakt en hebt geopend. 

## <a name="dapper-overview"></a>Dapper overzicht
**Dapper** is een object-relationele-toewijzing. Deze kaarten .NET-objecten uit uw toepassing met een relationele database (en vice versa). Het eerste deel van de voorbeeldcode ziet u hoe u de bibliotheek van de client elastische database kunt integreren met Dapper-toepassingen. Het tweede deel van de steekproef-code wordt geïllustreerd hoe integreren als u zowel Dapper en DapperExtensions gebruikt.  

De functionaliteit van de toewijzing in Dapper biedt extensie methoden op databaseverbindingen die verzendende T-SQL-instructies voor uitvoering en het opvragen van de database te vereenvoudigen. Bijvoorbeeld kunt Dapper u gemakkelijk toewijzen tussen uw .NET-objecten en de parameters van SQL-instructies voor oproepen **uitvoeren** of de resultaten van uw SQL-query's gebruiken in **Query** -aanroepen van Dapper .NET-objecten. 

Wanneer u een DapperExtensions gebruikt, moet u niet langer bieden de SQL-instructies. De SQL-instructies achter de schermen maken extensies methoden zoals **GetList** of **Invoegen** via de databaseverbinding
 
Een ander voordeel van Dapper en ook DapperExtensions is dat de toepassing besturingselementen voor het maken van de database-verbinding. Hiermee kunt werken met de elastische database client-bibliotheek waarin makelaars database verbindingen op basis van de toewijzing van shardlets aan databases.

Als u de Dapper stroombaan, raadpleegt u [Dapper stip netto](http://www.nuget.org/packages/Dapper/). Zie [DapperExtensions](http://www.nuget.org/packages/DapperExtensions)voor de Dapper extensies.

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Een kort overzicht van de bibliotheek van de client elastische database

Met de bibliotheek van de client elastische database u definieert partities van de toepassingsgegevens *shardlets* genoemd, koppel deze aan databases, en ze op *sharding toetsen*te identificeren. U kunt zo veel databases als u nodig hebt en uw shardlets over deze databases verdelen hebben. De toewijzing van sharding waarden van de database is opgeslagen door een kaart shard is verstrekt door API's van de bibliotheek. Deze functie heet **shard management toewijzen**. De kaart shard oefent ook de makelaar van databaseverbindingen voor aanvragen die een sleutel sharding uitvoeren. Deze functie wordt genoemd **afhankelijke omleiding van gegevens**.

![Shard plattegronden en afhankelijke omleiding van gegevens][1]

De manager van de map shard voorkomt gebruikers inconsistente weergaven op shardlet-gegevens die optreden mogelijk bij gelijktijdige shardlet management bewerkingen zijn er op de databases. Klik hiervoor broker de toewijzingen shard de databaseverbindingen voor een toepassing die zijn gemaakt met de bibliotheek. Wanneer shard management bewerkingen van invloed op de shardlet zijn kunnen, hierdoor de functionaliteit van de map shard om een database-verbinding automatisch af te sluiten. 

In plaats van de gebruikelijke manier om te maken van verbindingen voor Dapper gebruikt, moeten we de [OpenConnectionForKey methode](http://msdn.microsoft.com/library/azure/dn824099.aspx)te gebruiken. Dit zorgt ervoor dat alle validatieregels plaatsvindt en verbindingen correct worden beheerd wanneer alle gegevens worden verplaatst tussen shards.

### <a name="requirements-for-dapper-integration"></a>Vereisten voor Dapper-integratie

Als u werkt met zowel de bibliotheek van de client elastische database als de Dapper API's, die we wilt bewaren van de volgende eigenschappen:

* **Scaleout**: We wilt toevoegen of verwijderen van databases uit de gegevenslaag van de een laptopgeheugen toepassing zo nodig voor de eisen van de capaciteit van de toepassing. 

-    **Consistentie**: aangezien de schaal van de toepassing wordt aangepast via sharding, moeten we afhankelijke omleiding van gegevens uitvoeren. We wilt gebruiken van de gegevens afhankelijke routeren mogelijkheden van de bibliotheek kunt doen. Met name we wilt behouden de validatie en consistentie garandeert verstrekt door verbindingen die zijn brokered tot en met de manager van de map shard om beschadigde bestanden of verkeerde queryresultaten te voorkomen. Dit zorgt ervoor dat verbindingen met een opgegeven shardlet zijn geweigerd of gestopt als (bijvoorbeeld) de shardlet momenteel wordt verplaatst naar een andere shard met gesplitste/samenvoegen API's.

-    **Object toewijzing**: We wilt bewaren van het gemak van de toewijzingen die is verstrekt door Dapper tussen klassen in de toepassing en de onderliggende databasestructuren vertalen. 

De volgende sectie bevat richtlijnen voor deze vereisten voor toepassingen op basis van **Dapper** en **DapperExtensions**.

## <a name="technical-guidance"></a>Technische informatie vinden
### <a name="data-dependent-routing-with-dapper"></a>Gegevens afhankelijke routering met Dapper 

De toepassing is met Dapper, meestal verantwoordelijk voor het maken en de verbindingen met de onderliggende database te openen. Een type T gegeven door de toepassing, retourneert Dapper queryresultaten zoals .NET verzamelingen van het type T. Dapper voert de toewijzing van de T-SQL Resultatenrijen op de objecten van het type T. Daarnaast kaarten Dapper .NET-objecten in SQL-waarden of parameters voor instructies voor data voor language (DML). Dapper biedt deze functionaliteit via extensie methoden op de normale [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) object uit de ADO .NET SQL-Client-bibliotheken. De SQL-verbinding geretourneerd door de elastische schaal-API's voor DDR zijn ook normale [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten. In deze sectie geeft ons rechtstreeks via Dapper extensies het type die het resultaat van de clientbibliotheek DDR API, zoals het is ook een eenvoudige SQL-Client verbinding.

Deze opmerkingen kunnen u eenvoudig verbindingen brokered door de bibliotheek van de client elastische database voor Dapper te gebruiken.

In dit codevoorbeeld (uit de bijbehorende voorbeeld) ziet u de manier waarop de sleutel sharding is opgegeven door de toepassing naar de bibliotheek naar de verbinding met de juiste shard broker.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

De oproep door naar de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API vervangt de standaard maken en openen van een SQL-Client verbinding. De oproep [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kost de argumenten die vereist voor afhankelijke omleiding van gegevens zijn: 

-    De kaart shard voor toegang tot de gegevens afhankelijke routeren-interfaces
-    De toets sharding de shardlet identificeren
-    De referenties (gebruikersnaam en wachtwoord) verbinding maken met de shard

Het object shard-kaart Hiermee maakt u een verbinding met de shard waarin de shardlet voor de opgegeven sharding-sleutel. De databaseclient elastische API's ook de verbinding met het implementeren van de consistentie garanties labelen. Aangezien de oproep door naar [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) een normale SQL-Client connection-object retourneert, volgt de volgende oproep door naar de methode **Execute** extensie uit Dapper de standaard Dapper praktijk.

Query's werken zeer grotendeels net zoals – u de verbinding met [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) vanaf de client API voor het eerst opent. U de normale Dapper extensie methoden gebruiken om te wijzen de resultaten van uw SQL-query in .NET-objecten:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Opmerking die het **gebruik van** blok met de verbinding DDR bereiken alle databasebewerkingen binnen de blokkeren als u wilt de één shard waar tenantId1 wordt bewaard. De query retourneert alleen blogs die zijn opgeslagen op de huidige shard, maar niet de bestanden die zijn opgeslagen op een andere shards. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Afhankelijke routering met Dapper en DapperExtensions gegevens

Dapper wordt geleverd voor een selectie aan extra extensies dat verdere gemak en abstractielaag ten opzichte van de database leveren kan bij het ontwikkelen van databasetoepassingen. DapperExtensions is een voorbeeld. 

DapperExtensions gebruiken in uw toepassing wordt niet gewijzigd hoe databaseverbindingen worden gemaakt en beheerd. Het is nog steeds van de toepassing verantwoordelijkheid verbindingen openen en normale SQL-Client verbindingsobjecten worden geacht door de extensie methoden. We kunnen zijn afhankelijk van de [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) als hierboven beschreven. Als de volgende voorbeelden van de code weergeven, is de enige wijziging dat we niet langer moet de T-SQL-instructies schrijven:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

En hier ziet u in het voorbeeld voor de query: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Tijdelijke fouten voor de verwerking

Het team van Microsoft Patterns en procedures die zijn gepubliceerd de [Tijdelijke foutenstructuuranalyse afhandelen toepassing blokkeren](http://msdn.microsoft.com/library/hh680934.aspx) zodat softwareontwikkelaars Verklein veelgebruikte tijdelijke foutenstructuuranalyse voorwaarden die zijn opgetreden als actief in de cloud. Zie voor meer informatie [Perseverance, geheim van alle successen: met de tijdelijke foutenstructuuranalyse afhandelen van toepassing-blok](http://msdn.microsoft.com/library/dn440719.aspx).

In het voorbeeld is afhankelijk van de bibliotheek tijdelijke foutenstructuuranalyse ter bescherming tegen tijdelijke fouten. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** in de bovenstaande code wordt gedefinieerd als een **SqlDatabaseTransientErrorDetectionStrategy** met een aantal nieuwe pogingen van 10 en 5 seconden wachttijd tussen nieuwe pogingen. Als u transacties gebruikt, zorg dat de reikwijdte van uw opnieuw terug naar het begin van de transactie in het geval van een tijdelijke veroorzaakt.

## <a name="limitations"></a>Beperkingen

De methoden beschreven in dit document leiden tot een aantal beperkingen:

* De voorbeeldcode voor dit document bevat niet laten zien hoe schema beheren binnen shards.
* Een verzoek voor een gegeven, wordt ervan uitgegaan dat alle verwerking van de database zich in een enkel shard zoals die wordt aangeduid met de toets sharding is verstrekt door het verzoek. Echter wordt deze verondersteld niet altijd houdt, bijvoorbeeld wanneer het is niet mogelijk een sharding-toets om beschikbaar te maken. Om dit op te lossen, bevat de bibliotheek van de client elastische database de [MultiShardQuery class](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). De klasse implementeert een abstraction verbinding voor query's uitvoeren via verschillende shards. MultiShardQuery gebruiken in combinatie met Dapper valt buiten het bereik van dit document.

## <a name="conclusion"></a>Sluiten

Met behulp van Dapper en DapperExtensions kunnen eenvoudig profiteren van hulpmiddelen voor databases elastische voor Azure SQL-Database. Via de stappen in dit document, kunnen deze toepassingen gebruiken de mogelijkheid van het hulpprogramma voor gegevens afhankelijke routering door te wijzigen van het maken en openen van nieuwe [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objecten gebruik van het gesprek [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) van de bibliotheek van de client elastische database. Hiermee wordt de wijzigingen in de toepassing vereist naar deze locaties waar nieuwe verbindingen worden gemaakt en geopend beperkt. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 