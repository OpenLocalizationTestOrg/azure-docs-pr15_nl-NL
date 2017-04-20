<properties 
    pageTitle="Werken met georuimtelijke gegevens in Azure DocumentDB | Microsoft Azure" 
    description="Meer informatie over het maken, index en ruimte objecten met Azure DocumentDB query." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Werken met georuimtelijke gegevens in Azure DocumentDB

In dit artikel is een inleiding tot de functie georuimtelijke in [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Lees dit en kunnen u de volgende vragen beantwoorden:

- Hoe kan ik ruimte gegevens opslaan in Azure DocumentDB?
- Hoe kan ik georuimtelijke gegevens in Azure DocumentDB in SQL- en LINQ query?
- Hoe ik in- of uitschakelen ruimte indexering in DocumentDB?

Zie dit [Github project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) voor voorbeelden van programmacode.

## <a name="introduction-to-spatial-data"></a>Inleiding tot ruimte gegevens

Ruimte gegevens worden de positie en de vorm van objecten in de ruimte. In de meeste toepassingen deze komen overeen met objecten op de aarde, dat wil zeggen georuimtelijke gegevens. Ruimte gegevens kunnen worden gebruikt om aan te geven van de locatie van een persoon, locatie van belang of de rand van een bepaalde plaats of een meer. Veelvoorkomende gebruik gevallen vaak gebruikmaakt van nabijheid query's voor het verplaatsen, bijvoorbeeld "zoeken alle koffie winkels dicht bij mijn huidige locatie'. 

### <a name="geojson"></a>GeoJSON
DocumentDB ondersteunt indexering en query's uitvoeren van georuimtelijke punt gegevens die worden weergegeven met de [GeoJSON specificatie](http://geojson.org/geojson-spec.html). GeoJSON gegevensstructuren zijn altijd geldig JSON-objecten, zodat zij kunnen worden opgeslagen en een query wordt uitgevoerd DocumentDB gebruiken zonder speciale hulpprogramma's en -bibliotheken. De DocumentDB SDK's bieden helper klassen en methoden die u eenvoudig kunt werken met ruimte gegevens. 

### <a name="points-linestrings-and-polygons"></a>Punten, linestrings en Veelhoek
Een **punt** wordt bepaald door een één positie in de ruimte. Een punt voorstelt in georuimtelijke gegevens, de exacte locatie, die een adres van een supermarkt, kiosk, auto of een stad kan zijn.  Een punt wordt weergegeven in GeoJSON (en DocumentDB) met de coördinaten koppelen of lengte en breedte. Hier volgt een voorbeeld JSON voor een punt.

**Punten in DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] Hiermee geeft u de specificatie GeoJSON lengtegegevens voornaam en breedtegraad tweede. Net als in andere toepassingen toewijzing, lengte en breedte worden hoeken en graden in het diagram vertegenwoordigd. Lengtegegevens waarden worden gemeten vanaf de eerste meridiaan behoren en tussen 180 en 180.0 graden en breedtegraad waarden worden gemeten vanaf de evenaar en tussen-90.0 en 90.0 graden. 
>
> DocumentDB interpreteert coördinaten zoals weergegeven per de WGS-84 verwijzing systeem. Zie hieronder voor meer informatie over coördinaten verwijzing systemen.

Dit kan zijn ingesloten in een document DocumentDB zoals in dit voorbeeld van een gebruikersprofiel die locatiegegevens bevatten:

**Profiel gebruiken met locatie opgeslagen in DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Naast het punten ondersteunt GeoJSON ook LineStrings en veelhoek. **LineStrings** vertegenwoordigen een reeks van twee of meer punten in de ruimte en de lijnsegmenten die ze verbinding te maken. In georuimtelijke gegevens, linestrings vaak gebruikt om aan te geven hoofdwegen of rivieren. Een **Veelhoek** is een rand aan van verbonden punten die een gesloten LineString vormt. Veelhoek worden meestal gebruikt voor natuurlijke formaties zoals meren of de jurisdicties zoals steden en Staten. Hier volgt een voorbeeld van een veelhoek in DocumentDB. 

**Veelhoek in DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] De specificatie GeoJSON moet voor geldige veelhoek, het laatste coördinaten paar opgegeven moet hetzelfde als de eerste, een gesloten shape maken.
>
>Punten in een veelhoek moeten worden opgegeven in linksom volgorde. Een veelhoek die is opgegeven in rechtsom volgorde staat voor de inverse van de regio erin.

Naast de punt, LineString en veelhoek geeft GeoJSON ook de weergave voor het groeperen van meerdere georuimtelijke locaties, alsmede het willekeurige eigenschappen koppelen aan geolocatie als een van de **functie**. Aangezien deze objecten geldig JSON zijn, kunnen alle worden opgeslagen en verwerkt in DocumentDB. Echter DocumentDB biedt alleen ondersteuning voor automatische indexering van punten.

### <a name="coordinate-reference-systems"></a>Verwijzing systemen coördineren

Aangezien de vorm van de aarde onregelmatig is, wordt veel coördinaten verwijzing systemen (CRS), elk voorzien van hun eigen frames van verwijzing en maateenheden coördinaten van georuimtelijke gegevens weergegeven. Het 'nationale raster van-Brittannië"is bijvoorbeeld een verwijzing-systeem is heel nauwkeurig voor het Verenigd Koninkrijk, maar niet buiten. 

Meest populaire in gebruik vandaag nog niet is de wereld geodetisch systeem [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). Gebruik WGS 84 GPS-apparaten en veel toewijzing services met inbegrip van Google-plattegronden en Bing Maps-API's. DocumentDB ondersteunt indexering en query's uitvoeren met georuimtelijke gegevens met het WGS-84 CRS alleen. 

## <a name="creating-documents-with-spatial-data"></a>Documenten maken met ruimte gegevens
Wanneer u documenten die GeoJSON waarden bevatten maakt, worden ze automatisch geïndexeerd met een ruimte index overeenkomstig de indexing beleid van de siteverzameling. Als u met een DocumentDB SDK in een dynamisch getypte taal zoals Python of Node.js werkt, moet u geldige GeoJSON maken.

**Document met georuimtelijke gegevens in Node.js maken**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Als u met het .NET (of Java) SDK's werkt, kunt u de nieuwe klassen voor de punt en Veelhoek binnen de naamruimte Microsoft.Azure.Documents.Spatial locatiegegevens insluiten in uw toepassingsobjecten. Deze klassen vereenvoudigen de serialisatie en deserialisatie ruimte gegevens in GeoJSON.

**Document met georuimtelijke gegevens in .NET maken**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Als u niet beschikt over de breedte- en lengtegegevens informatie, maar de fysieke adressen of de naam van de locatie zoals plaats of land hebt, kunt u de werkelijke coördinaten opzoeken met behulp van een service geografische codes zoals Bing Maps REST Services. Meer informatie over Bing Maps geografische codes [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Query's uitvoeren ruimte typen

Nu dat we eens kijken hoe u het invoegen van georuimtelijke gegevens hebt gemaakt, laten we eens kijken hoe u deze gegevens gebruiken met SQL- en LINQ DocumentDB query te nemen.

### <a name="spatial-sql-built-in-functions"></a>Ruimte ingebouwde SQL-functies
DocumentDB ondersteunt de volgende geopende georuimtelijke Consortium (OGC) ingebouwde functies voor georuimtelijke query's uitvoeren. Raadpleeg voor meer informatie over de complete set ingebouwde functies in de SQL-taal, [Query DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Retourneert de afstand tussen de twee GeoJSON punt expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Geeft als resultaat een Booleaanse expressie die aangeeft of het GeoJSON punt in het eerste argument opgegeven binnen de veelhoek GeoJSON in het tweede argument is.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Geeft als resultaat een Booleaanse waarde die aangeeft of de opgegeven GeoJSON punt of veelhoek expressie geldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Geeft als resultaat de waarde van een JSON met een Booleaanse waarde als de opgegeven GeoJSON punt of veelhoek expressie geldig is en ongeldige, bovendien de reden als een tekenreekswaarde.</td>
</tr>
</table>

Ruimte functies kunnen worden gebruikt om uit te voeren nabijheid query's op ruimte gegevens. Hier is bijvoorbeeld een query die alle family documenten die zijn binnen 30 kilometer van de opgegeven locatie met de ingebouwde functie ST_DISTANCE retourneert. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Als u ruimte indexering in uw indexing beleid, moeten klikt u vervolgens 'afstand query's ' worden verzonden efficiënt tot en met de index. Voor meer informatie over de ruimte indexeren, raadpleegt u de sectie hieronder. Als u een ruimte index voor de opgegeven paden niet hebt, kunt u nog steeds ruimte query's uitvoeren door op te geven `x-ms-documentdb-query-enable-scan` verzoek koptekst met de waarde ingesteld op "true". In .NET, kan dit doen door het optionele argument van de **FeedOptions** tot query's met [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ingesteld op waar. 

ST_WITHIN kan worden gebruikt om te controleren als een punt binnen een veelhoek veroorzaakt. Veelhoek worden meestal gebruikt om aan te geven grenzen zoals postcodes verwijderd, staat grenzen of natuurlijke formaties. Opnieuw als u ruimte indexering in uw indexing beleid, klikt u vervolgens 'haaienvin' query's moeten worden verzonden efficiënt tot en met de index. 

Veelhoek argumenten in ST_WITHIN kunnen slechts één keer bevatten, dat wil zeggen de veelhoek mag geen gaten in deze. 

**Query**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Resultaten**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Vergelijkbaar met hoe niet-overeenkomende typen werkt in DocumentDB query, als de locatiewaarde die is opgegeven in een van de argumenten is onjuist of ongeldige en klik vervolgens deze **ongedefinieerd** en het geëvalueerde document van de queryresultaten worden overgeslagen wordt geëvalueerd. Als uw query geen resultaten worden geretourneerd, voert u ST_ISVALIDDETAILED naar foutopsporing waarom het type spatail ongeldig is.     

DocumentDB ondersteunen ook inverse query's uitvoeren, dat wil zeggen kunt u veelhoek of lijnen in DocumentDB indexeren en vervolgens naar de gebieden met een opgegeven punt zoeken. Dit patroon wordt gebruikt in logistiek identificeren, bijvoorbeeld wanneer een die binnenkomt of verlaat een aangewezen gebied. 

**Query**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Resultaten**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID en ST_ISVALIDDETAILED kan worden gebruikt om te controleren of een ruimte object geldig is. De volgende query wordt bijvoorbeeld de geldigheid van een komma met een automatisch niet bereik Breedtegraadwaarde (-132.8) gecontroleerd. ST_ISVALID alleen een Booleaanse waarde retourneert en ST_ISVALIDDETAILED geeft als resultaat de Booleaanse waarde en een tekenreeks met de reden waarom het volgende ongeldig wordt.

**Query**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Resultaten**

    [{
      "$1": false
    }]

Deze functies kunnen ook worden gebruikt voor het valideren van veelhoek. Bijvoorbeeld hier we gebruiken ST_ISVALIDDETAILED voor het valideren van een veelhoek die niet is gesloten. 

**Query**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Resultaten**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>Query's uitvoeren in de .NET SDK LINQ

De DocumentDB .NET SDK ook providers methoden stub `Distance()` en `Within()` voor gebruik in LINQ expressies. De provider DocumentDB LINQ equivalent deze methode oproepen naar de overeenkomstige SQL ingebouwde functie oproepen (ST_DISTANCE en ST_WITHIN respectievelijk). 

Hier volgt een voorbeeld van een query LINQ waarmee wordt gezocht naar alle documenten in de DocumentDB verzameling waarvan u de waarde "locatie" is binnen een straal van 30 kilometer van het opgegeven wijst LINQ gebruiken.

**LINQ-query voor afstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Op dezelfde manier als volgt een query voor het zoeken van alle documenten waarvan "locatie" binnen de opgegeven vak/veelhoek is. 

**LINQ naar binnen zoeken.**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nu dat we eens kijken hoe u documenten met LINQ en SQL query hebt gemaakt, laten we eens kijken hoe u DocumentDB configureert voor ruimte indexeren te nemen.

## <a name="indexing"></a>Indexeren

Zoals wordt beschreven in het [Schema Agnostic indexering met Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papier, we DocumentDB van database-engine worden ontworpen behoorlijk schema agnostic en geef de eerste-klas ondersteuning voor JSON. De database-engine schrijven geoptimaliseerd van DocumentDB begrijpt native ruimte gegevens (punten, veelhoek en regels) dat wordt aangegeven in de standaard GeoJSON.

Kortom, de geometrie geprojecteerd van geodetisch coördinaten op een 2D vlak en dan geleidelijk in cellen met een **quadtree**verdeeld. Deze cellen zijn toegewezen aan 1D op basis van de locatie van de cel in een **curve benaderen Hilbert ruimte doorvoeren**, waarin gewoon overgenomen in plaats van punten. Bovendien wanneer locatiegegevens wordt geïndexeerd, wordt er een zogeheten **mozaïekpatroon**, dat wil zeggen alle cellen die elkaar een locatie overlappen worden geïdentificeerd en opgeslagen als toetsen in de index DocumentDB. Query keer, argumenten, zoals punten en Veelhoek zijn ook representatie mozaïekpatroon als u wilt extraheren van de relevante cellenbereiken-ID en klik vervolgens gebruikt voor het ophalen van gegevens uit de index.

Als u een indexing beleid met ruimte index voor / * (alle paden), en vervolgens de overal binnen de verzameling gevonden zijn geïndexeerd voor efficiënte ruimte query's (ST_WITHIN en ST_DISTANCE). Ruimte indexen niet hebben een waarde precisie van formules, en gebruik altijd een standaardwaarde precisie van formules.

>[AZURE.NOTE] DocumentDB ondersteunt automatische indexeren van punten, veelhoek en LineStrings

Het codefragment van de volgende JSON ziet u een indexing beleid met ruimte indexeren is ingeschakeld, dat wil zeggen indexeren nergens GeoJSON gevonden in documenten voor ruimte query's uitvoeren. Als u het indexeren beleid met behulp van de Portal Azure wijzigt, kunt u de volgende JSON voor indexing beleid ruimte op de verzameling indexering inschakelen.

**Siteverzameling indexeren beleid JSON met spatiaal ingeschakeld voor punten en Veelhoek**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier ziet u een codefragment in .NET die laat hoe een verzameling maken zien met ruimte indexeren is ingeschakeld voor alle paden met wordt verwezen. 

**Maken van een siteverzameling met ruimte indexeren**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

En hier ziet u hoe u een bestaande siteverzameling om te profiteren van de ruimte indexering via de punten die zijn opgeslagen in documenten kunt wijzigen.

**Een bestaande siteverzameling met ruimte indexeren wijzigen**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Als de locatie GeoJSON waarde in het document onjuist of ongeldige is, wordt klikt u vervolgens deze niet geïndexeerd voor ruimte query's uitvoeren. U kunt locatie waarden met ST_ISVALID en ST_ISVALIDDETAILED valideren.
>
> Als de definitie van uw siteverzameling een partitiesleutel bevat, indexeren transformatie voortgang niet wordt vermeld. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt uit het verleden over het aan de slag met ondersteuning voor georuimtelijke in DocumentDB, kunt u het volgende doen:

- Beginnen met de [voorbeelden van georuimtelijke .NET-code op Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs) kleurcodering
- Aan de handen met georuimtelijke query's uitvoeren op de [DocumentDB Query Speelplaats](http://www.documentdb.com/sql/demo#geospatial)
- Meer informatie over [DocumentDB Query](documentdb-sql-query.md)
- Meer informatie over het [Beleid voor het DocumentDB indexeren](documentdb-indexing-policies.md)
