<properties 
    pageTitle="Aan de slag met elastische Databasehulpmiddelen" 
    description="Eenvoudige uitleg van de functie in hulpmiddelen voor elastische webdatabase van Azure SQL-Database, inclusief eenvoudig voorbeeld app uitvoeren." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Aan de slag met elastische Databasehulpmiddelen

In dit document kennismaakt u met de functionaliteit voor ontwikkelaars door de voorbeeld-app uit te voeren. De steekproef Hiermee maakt u een eenvoudige een laptopgeheugen toepassing en belangrijke functies van hulpmiddelen voor databases elastische verkent. Het voorbeeld wordt gedemonstreerd functies van de [elastische database client-bibliotheek](sql-database-elastic-database-client-library.md)

Als u wilt de bibliotheek hebt geïnstalleerd, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Houd er rekening mee dat de bibliotheek met de hieronder beschreven voorbeeld-app is geïnstalleerd.

## <a name="prerequisites"></a>Vereisten voor

1. Visual Studio 2012 of hoger met C# is vereist. Download een gratis versie op [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2,7 of hoger. Als u de meest recente versie, raadpleegt u [NuGet installeren](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Downloaden en uitvoeren van de steekproef-app

De **elastische met Azure SQL-Database, aan de slag** voorbeeldtoepassing ziet u de belangrijkste aspecten van de ontwikkeling-ervaring voor een laptopgeheugen toepassingen met Azure SQL database elastische tools. De nadruk op belangrijke gebruik dozen voor [shard toewijzen management](sql-database-elastic-scale-shard-map-management.md), [gegevens afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [meerdere shard query's uitvoeren](sql-database-elastic-scale-multishard-querying.md). Als u wilt downloaden en uitvoeren van de steekproef, als volgt te werk: 

1. Open Visual Studio en selecteer **Bestand -> Nieuw Project ->**.
2. Klik in het dialoogvenster, klikt u op **Online**.

    ![Nieuw Project > Online][2]
3. Klik vervolgens op **Visual C#** onder **voorbeelden**.

    ![Klik op visuele C#][3]
4. Typ in het zoekvak **elastische db** om te zoeken naar de steekproef. De titel **Elastische DB-hulpprogramma's voor Azure SQL - aan de slag** wordt weergegeven.

    ![Zoekvak][1]
 
5. Selecteer in het voorbeeld, kies een naam en een locatie voor het nieuwe project en drukt u op **OK** om het project te maken.
6. Open het bestand **app.config** in de oplossing voor het project steekproef en volg de instructies in het bestand om de naam van de Azure SQL database-server en uw aanmeldingsgegevens (gebruikersnaam en wachtwoord) toevoegen.
7. Maken en uitvoeren van de toepassing. Wanneer u wordt gevraagd, wacht u Visual Studio om de NuGet-pakketten van de oplossing herstellen. Hiermee wordt de nieuwste versie van de bibliotheek van de client elastische database downloaden van NuGet.
8. Spelen met de verschillende opties voor meer informatie over de mogelijkheden van de client-bibliotheek. Opmerking de stappen die de toepassing in de beheerconsole wordt uitvoer en je mag rustig verkennen van de code achter de schermen.

    ![voortgang][4]

Gefeliciteerd, u hebt gemaakt en uitvoeren van uw eerste een laptopgeheugen toepassing met hulpmiddelen voor databases elastische op Azure SQL-Database. Een kort overzicht van de shards die de steekproef gemaakt door te verbinden met Visual Studio of SQL Server Management Studio toe aan uw Server Azure-DB duren. Ziet u nieuwe steekproef shard databases en een manager-database voor shard map in het voorbeeld heeft gemaakt.

> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Belangrijkste onderdelen van de steekproef code

1. **Shards beheren en Shard kaarten**: de code laat zien hoe u werkt met shards, bereiken, en toewijzingen in bestand **ShardMapManagerSample.cs**. U vindt meer informatie over dit onderwerp hier: [Shard kaart Management](http://go.microsoft.com/?linkid=9862595).  
2. **Afhankelijke omleiding van gegevens**: de routering van transacties naar de juiste shard weer te geven in **DataDependentRoutingSample.cs**. Zie de [Afhankelijke routering van gegevens](http://go.microsoft.com/?linkid=9862596)voor meer informatie. 
3. **Query uitvoeren op meerdere Shards**: query's via shards wordt geïllustreerd in het bestand **MultiShardQuerySample.cs**. Zie [Meerdere Shard query's uitvoeren](http://go.microsoft.com/?linkid=9862597)voor meer informatie.
4. **Lege shards toevoegen**: het iteratieve toevoegen van nieuwe lege shards wordt uitgevoerd door de code in bestand **AddNewShardsSample.cs**. Details over dit onderwerp vindt u hier: [Shard kaart Management](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andere bewerkingen elastische schaal

1. **Een bestaande shard splitsen**: de functie voor het splitsen van shards is beschikbaar via het **samenvoegen van gesplitste hulpmiddel**. U vindt meer informatie over dit hulpprogramma hier: [overzicht van de gesplitste samenvoegen hulpmiddel](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Bestaande shards samenvoegen**: Shard wordt samengevoegd ook worden uitgevoerd met het **hulpmiddel gesplitste samenvoegen**. Raadpleeg voor meer informatie: [overzicht van de gesplitste samenvoegen hulpmiddel](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Kosten

De elastische Databasehulpmiddelen zijn gratis. Hulpmiddelen voor databases elastische bevat geen extra kosten boven aan de kosten voor uw Azure gebruik opleggen. 

Bijvoorbeeld de voorbeeldtoepassing Hiermee maakt u nieuwe databases. De kosten, is afhankelijk van de DB van Azure SQL database-editie die u kiezen en de Azure gebruik van de toepassing.

Zie [Meer informatie het prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)voor de prijsinformatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de elastische Databasehulpmiddelen:

* [Documentatieoverzicht voor hulpmiddelen voor elastische database](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Voorbeelden van de code: 
    -    [Elastische DB met Azure SQL - aan de slag](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastische DB met Azure SQL - integratie met entiteit Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard elasticiteit op Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [aankondiging elastische schaal](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanaal 9: [elastische schaal overzicht Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Discussieforum: [Azure SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Prestaties meten: [prestatie-items voor shard kaart manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
