<properties 
    pageTitle="DocumentDB gegevens sorteren met sorteren | Microsoft Azure" 
    description="Leer hoe u ORDER BY gebruiken in query's DocumentDB in LINQ en SQL en hoe een indexing beleid voor ORDER BY query's opgeven." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Gegevens sorteren DocumentDB Order By gebruiken
Microsoft Azure DocumentDB documenten opvragen met SQL via JSON-documenten. Queryresultaten kunnen de ORDER BY-component gebruiken in query-SQL-instructies worden gerangschikt.

Lees dit artikel en kunt u wel de volgende vragen beantwoorden: 

- Hoe ik met Order By query?
- Hoe configureer ik een indexing beleid voor Order By?
- Wat u te wachten volgende?

[Voorbeelden](#samples) en een [Veelgestelde vragen over](#faq) zijn ook beschikbaar.

Zie de [DocumentDB Query zelfstudie](documentdb-sql-query.md)voor volledige informatie op SQL query's uitvoeren.

## <a name="how-to-query-with-order-by"></a>Hoe u de Query met Order By
Zoals in ANSI-SQL, kunt u nu ook een optioneel Order By-component in SQL-instructies bij het DocumentDB opvragen. De component kan bevatten een optioneel ASC/DESC argument de volgorde waarin de resultaten moeten worden opgehaald. 

### <a name="ordering-using-sql"></a>Ordening met SQL
Hier is bijvoorbeeld een query voor het ophalen van de top 10-boeken in aflopende volgorde van de titel. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Ordening SQL gebruikt met filteren
U kunt ordenen met een geneste eigenschap in documenten zoals Books.ShippingDetails.Weight en kunt u extra filters in de WHERE-component in combinatie met Order By, zoals in dit voorbeeld:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Met de LINQ-Provider voor .NET ordening
Met de .NET SDK versie 1.2.0 en hoger, u kunt ook de component OrderBy() of OrderByDescending() binnen LINQ-query's, zoals in dit voorbeeld:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB ondersteunt ordening met één numerieke, tekenreeks of Booleaanse eigenschap per query, met aanvullende querytypen binnenkort beschikbaar. Zie [wat er nieuw volgende](#Whats_coming_next) voor meer informatie.

## <a name="configure-an-indexing-policy-for-order-by"></a>Een indexing beleid voor Order By configureren

Intrekken of DocumentDB twee soorten indexen (Hash en bereik), die kunnen worden ingesteld voor specifieke paden/eigenschappen, gegevenstypen (tekenreeksen/getallen) en klik in de verschillende precisie waarden (maximale precisie van formules of de waarde van een vaste precisie) ondersteunt. Aangezien DocumentDB Hash indexeren als standaard gebruikt, moet u een nieuwe siteverzameling maken met een aangepast indexing beleid met bereik van getallen, tekenreeksen of beide, om te kunnen gebruiken Order By. 

>[AZURE.NOTE] Tekenreeks bereik indexen zijn op 7 juli 2015 met REST API versie 2015-06-03 ingevoerd. Om te maken van beleidsregels voor Order By ten opzichte van tekenreeksen, moet u SDK versie 1.2.0 van .NET SDK of versie 1.1.0 Python, Node.js of Java SDK gebruiken.
>
>Voordat u REST API versie 2015-06-03 is het standaardbeleid voor siteverzameling indexing Hash voor zowel tekenreeksen en getallen. Dit is gewijzigd in Hash voor tekenreeksen en bereik voor getallen. 

Zie [DocumentDB indexing beleidsregels](documentdb-indexing-policies.md)voor meer informatie.

### <a name="indexing-for-order-by-against-all-properties"></a>Indexering in voor de Order By ten opzichte van alle eigenschappen
Hier volgt hoe u kunt maken van een siteverzameling met "Alle bereik" indexering in voor de Order By ten opzichte van elk/alle numerieke of eigenschappen die worden weergegeven binnen JSON documenten binnen deze tekenreeks. Hier wordt overschreven door de standaardeenheid voor de index voor tekenreekswaarden naar bereik en klik in de maximale nauwkeurigheid (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Houd er rekening mee dat Order By alleen retourneert resultaten van de gegevenstypen (tekenreeks en getal) die zijn geïndexeerd met een RangeIndex. Bijvoorbeeld, hebt u de standaard indexeren beleid alleen met RangeIndex op getallen, retourneert een Order By ten opzichte van een pad met tekenreekswaarden geen documenten.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indexering in voor de Order By voor één eigenschap
Hier ziet u hoe u een siteverzameling kunt maken met het indexeren voor Order By ten opzichte van alleen de eigenschap titel, dat wil een tekenreeks zeggen. Er zijn twee paden, één telefoonnummer voor de eigenschap titel ('/ titel /?') met bereik indexeren en de andere voor elke andere eigenschap met de standaard indexing kleurenschema, dat wil Hash voor tekenreeksen en bereik voor getallen zeggen.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Voorbeelden
Bekijk dit [Github voorbeelden project](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) die laat hoe u Order By zien, inclusief maken beleidsregels indexeren en paginering met Order By. In de voorbeelden zijn bron openen en is het raadzaam om in te dienen halen aanvragen met bijdragen aan goede doelen die andere ontwikkelaars DocumentDB kunnen profiteren. Raadpleeg de [bijdrage richtlijnen](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) voor instructies over het bijdragen.  

## <a name="faq"></a>FAQ

**Wat is het verwachte aanvragen per eenheid Britanniche| verbruik van Order By query's?**

Aangezien maakt gebruik van de index DocumentDB voor zoekacties Order By, is het aantal verzoek eenheden dat door Order By query's is vergelijkbaar met de overeenkomstige query's zonder Order By. Zoals elke andere bewerking op DocumentDB, is het aantal eenheden van de aanvraag is afhankelijk van de grootte/shapes van documenten, evenals de complexiteit van de query. 


**Wat is de verwachte indexeren belasting voor Order By?**

De indexing opslag realiseren wordt in verhouding tot het aantal eigenschappen zijn. In de scenario voor het ongunstigste geval is de index realiseren 100% van de gegevens. Er is geen verschil in doorvoer (aanvragen eenheden) realiseren tussen bereik/Order By indexeren en de standaard Hash indexeren.

**Hoe ik mijn bestaande gegevens in DocumentDB met Order By query?**

Als u wilt sorteren queryresultaten met Order By, moet u het indexeren beleid van de siteverzameling naar een bereik index gebruikstype ten opzichte van de eigenschap gebruikt om te sorteren wijzigen. Zie [indexering van beleid wijzigen](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Wat zijn de huidige beperkingen van Order By?**

Order By kan worden opgegeven alleen ten opzichte van een eigenschap, een van beide numerieke of tekenreeks wanneer het bereik is geïndexeerd met de maximale nauwkeurigheid (-1).

U kunt geen als volgt te werk:
 
- Order By met interne tekenreekseigenschappen zoals-id, _rid en _self (binnenkort).
- Order By met eigenschappen afgeleid van het resultaat van een binnen-document join (binnenkort).
- Sorteren op meerdere eigenschappen (binnenkort).
- Order By met query's op databases, verzamelingen, gebruikers, machtigingen of bijlagen (binnenkort).
- Bestel u door met de berekende eigenschappen bijvoorbeeld het resultaat van een expressie of een UDF/ingebouwde-in functie.

Order By is momenteel niet ondersteund voor query's cross-partition wanneer u een Query Explorer gebruikt in de portal van Azure.

## <a name="troubleshooting"></a>Problemen oplossen

Als er een foutbericht dat Order By niet wordt ondersteund, controleert u om ervoor te zorgen dat u gebruikt een versie van de [SDK](documentdb-sdk-dotnet.md) die ondersteuning biedt voor sorteren. 

## <a name="next-steps"></a>Volgende stappen

Zich het [Github voorbeelden project](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) splitsen en begint u uw gegevens ordening! 

## <a name="references"></a>Verwijzingen
* [Referentie voor DocumentDB Query](documentdb-sql-query.md)
* [DocumentDB indexeren beleidsverwijzing](documentdb-indexing-policies.md)
* [DocumentDB SQL-verwijzing](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By voorbeelden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

