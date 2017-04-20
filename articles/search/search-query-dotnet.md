<properties
    pageTitle="De zoekindex van Azure met de SDK .NET query | Microsoft Azure | De zoekservice gehoste cloud"
    description="Maken van een zoekopdracht in Azure zoeken en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>De zoekindex van Azure met de SDK .NET query
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In dit artikel wordt uitgelegd hoe u een index met behulp van de [Azure zoeken .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)opvragen.

Voordat u deze procedure begint, moet u al hebt [gemaakt van een Azure zoekindex](search-what-is-an-index.md) en [gevuld met gegevens](search-what-is-data-import.md).

Houd er rekening mee dat alle steekproef-code in dit artikel is geschreven in C#. U vindt de volledige bron code [op GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ik. Uw Azure zoekservice van query-api-sleutel identificeren
Nu u een Azure zoekindex hebt gemaakt, u bent bijna klaar om te actie-query's met de .NET SDK. Eerst moet u deze een van de query api-toetsen die is gegenereerd voor de zoekservice die u deze is ingericht. De .NET SDK stuurt op elk verzoek deze api-sleutel uit uw service. Met een geldige sleutel tot stand brengt vertrouwen, op basis per verzoek tussen het verzenden van de aanvraag en de service die omgaat met deze toepassing.

1. Als u wilt zoeken van uw service api-toetsen u moet zich aanmelden bij de [Portal van Azure](https://portal.azure.com/)
2. Ga naar uw Azure zoekservice van blade
3. Klik op het pictogram "Codes"

Uw service heeft *beheerder* en *query sleutels*.

  - Uw primaire en secundaire *beheerder toetsen* verlenen volledige rechten voor alle bewerkingen, waaronder de mogelijkheid te beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u de secundaire sleutel gebruiken blijven kunt als u besluit om opnieuw te genereren van de primaire sleutel en vice versa.
  - Uw *query toetsen* alleen-lezen toegang verlenen tot indexen en documenten en meestal zijn verdeeld over clienttoepassingen die zoeken verzoeken.

Voor de toepassing van query's in een index, kunt u een van de toetsen van uw query. Uw beheerder sleutels kunnen ook worden gebruikt voor query's, maar u moet de sleutel van een query gebruiken in uw toepassingscode als volgt het [principe van het laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege)beter volgt.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Een exemplaar van de klas SearchIndexClient maken
Voor het verlenen van query's met de SDK van Azure zoeken .NET, moet u maken van een exemplaar van de `SearchIndexClient` class. Deze klasse heeft verschillende constructors. De gewenste duurt uw service-Zoeknaam en index, en een `SearchCredentials` object als parameters. `SearchCredentials`uw api-sleutel terugloopt.

De onderstaande code wordt een nieuw `SearchIndexClient` voor de "hotels" index (gemaakt in [maken indexen Azure zoeken met de .NET SDK](search-create-index-dotnet.md)) met behulp van waarden voor de servicenaam zoekopdracht en api-sleutel die zijn opgeslagen in van de toepassing config-bestand (`app.config` of `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`heeft een `Documents` eigenschap. Deze eigenschap bevat alle methoden die u nodig hebt om query's zoeken Azure indexen.

## <a name="iii-query-your-index"></a>III. De Zijtabs met letter query
Zoeken met de .NET SDK is zo eenvoudig als het om te bellen kwijt de `Documents.Search` methode op uw `SearchIndexClient`. Deze methode heeft een paar parameters, met inbegrip van de zoektekst, samen met een `SearchParameters` object die kan worden gebruikt om de query verder te verfijnen.

#### <a name="types-of-queries"></a>Typen query 's
De twee belangrijkste [querytypen](search-query-overview.md#types-of-queries) gewenste zijn `search` en `filter`. A `search` query wordt gezocht naar een of meer voorwaarden in alle _doorzoekbare_ velden in de index. A `filter` query evalueert een Booleaanse expressie over alles _filterbare_ velden in een index.

Zowel zoekopdrachten als filters zijn uitgevoerd met de `Documents.Search` methode. Een zoekopdracht kan worden doorgegeven de `searchText` parameter, terwijl een filterexpressie kan worden doorgegeven de `Filter` eigenschap van het `SearchParameters` class. Als u wilt filteren zonder te zoeken, net doorgeven `"*"` voor de `searchText` parameter. Als u wilt zoeken zonder dat u filters, laat u de `Filter` eigenschap Databasewachtwoord opheffen, of niet door een `SearchParameters` helemaal exemplaar.

#### <a name="example-queries"></a>Voorbeeld van query 's

De volgende code ziet u een aantal verschillende manieren om query's in de index 'hotels' is gedefinieerd in [maken indexen Azure zoeken met de SDK .NET](search-create-index-dotnet.md#DefineIndex). U ziet dat de documenten die zijn geretourneerd met de lijst met zoekresultaten exemplaren van de `Hotel` klasse, die is gedefinieerd in [Gegevens importeren in Azure zoekprogramma's via de .NET SDK](search-import-data-dotnet.md#HotelClass). De voorbeeldcode maakt gebruik van een `WriteDocuments` methode om de lijst met zoekresultaten aan de console. Deze methode is beschreven in het volgende gedeelte.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Lijst met zoekresultaten de greep
De `Documents.Search` methode geeft als resultaat een `DocumentSearchResult` object met de resultaten van de query. Het voorbeeld in de vorige sectie gebruikt een methode genoemd `WriteDocuments` om de lijst met zoekresultaten aan de console:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Hier ziet u hoe de resultaten eruit ziet voor de query in de vorige sectie, ervan uitgaande dat de index "hotels" wordt gevuld met de voorbeeldgegevens in [Gegevens importeren in Azure zoekprogramma's via de .NET SDK's](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Het voorbeeld hierboven wordt gebruikt de console om zoekresultaten. U moet ook zoekresultaten weergeven in uw eigen toepassing. Zie [dit voorbeeld op GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) voor een voorbeeld van het genereren van zoekresultaten in een ASP.NET-MVC gebaseerde webtoepassing.
