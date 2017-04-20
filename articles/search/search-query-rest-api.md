<properties
    pageTitle="De zoekindex van Azure met de REST API query | Microsoft Azure | De zoekservice gehoste cloud"
    description="Maken van een zoekopdracht in Azure zoeken en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Query de zoekindex van Azure de REST API gebruiken
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In dit artikel wordt uitgelegd hoe u een index met behulp van de [Azure zoeken REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)opvragen.

Voordat u deze procedure begint, moet u al hebt [gemaakt van een Azure zoekindex](search-what-is-an-index.md) en [gevuld met gegevens](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ik. Uw Azure zoekservice van query-api-sleutel identificeren
Een belangrijk onderdeel van elke zoekbewerking ten opzichte van de Azure zoeken REST API is de *api-sleutel* die is gegenereerd voor de service die u deze is ingericht. Met een geldige sleutel tot stand brengt vertrouwen, op basis per verzoek tussen het verzenden van de aanvraag en de service die omgaat met deze toepassing.

1. Als u wilt zoeken van uw service api-toetsen u moet zich aanmelden bij de [Portal van Azure](https://portal.azure.com/)
2. Ga naar uw Azure zoekservice van blade
3. Klik op het pictogram "Codes"

Uw service heeft *beheerder* en *query sleutels*.

 - Uw primaire en secundaire *beheerder toetsen* verlenen volledige rechten voor alle bewerkingen, waaronder de mogelijkheid te beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u de secundaire sleutel gebruiken blijven kunt als u besluit om opnieuw te genereren van de primaire sleutel en vice versa.
 - Uw *query toetsen* alleen-lezen toegang verlenen tot indexen en documenten en meestal zijn verdeeld over clienttoepassingen die zoeken verzoeken.

Voor de toepassing van query's in een index, kunt u een van de toetsen van uw query. Uw beheerder sleutels kunnen ook worden gebruikt voor query's, maar u moet de sleutel van een query gebruiken in uw toepassingscode als volgt het [principe van het laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege)beter volgt.

## <a name="ii-formulate-your-query"></a>II. Uw query opstellen
Er zijn twee manieren om uw gebruik van de REST API index te [doorzoeken](https://msdn.microsoft.com/library/azure/dn798927.aspx). Eén manier is om een HTTP POST-aanvraag waar uw queryparameters worden gedefinieerd in een JSON-object in de hoofdtekst van het verzoek. De andere manier is om een HTTP GET-aanvraag waar uw queryparameters worden gedefinieerd in de URL van de aanvraag. Houd er rekening mee dat bericht meer [versoepeld limieten](https://msdn.microsoft.com/library/azure/dn798927.aspx) van de grootte van queryparameters dan ophalen heeft. Om die reden is het raadzaam te posten met, tenzij er speciale omstandigheden waarin gebruiken GET zou handiger.

Voor zowel POST en ophalen, moet u uw *naam*, *de indexnaam van de*en de juiste *API-versie* (de huidige versie van de API `2015-02-28` op het moment van dit document wordt gepubliceerd) in de aanvraag-URL. Voor ophalen worden de *queryreeks* aan het einde van de URL waar u de queryparameters opgeeft. Zie hieronder voor de URL-notatie:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

De opmaak van het bericht is de dezelfde, maar met alleen api-versie in de queryreeks-parameters.



#### <a name="example-queries"></a>Voorbeeld van query 's

Hier volgen een paar voorbeeld-query's op een index met de naam "hotels". Deze query's worden weergegeven in zowel GET en POST-indeling.

De volledige index voor de term 'budget' zoeken en alleen terug te keren de `hotelName` veld:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Een filter toepassen op de index hotels goedkoper dan $150 per's nachts zoeken en de `hotelId` en `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Zoek in de volledige index, volgorde op basis van een bepaald veld (`lastRenovationDate`) in aflopende volgorde, waarbij de resultaten van de twee belangrijkste genomen en ziet u alleen `hotelName` en `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Stuur uw HTTP-aanvraag
Nu dat u hebt uw query geformuleerd als onderdeel van uw aanvraag-URL HTTP (voor GET) of de hoofdtekst (voor POST), kunt u uw aanvraag kopteksten definiëren en dien uw query.

#### <a name="request-and-request-headers"></a>Vergaderverzoeken en verzoek kopteksten
U moet twee verzoek kopteksten definiëren voor ophalen of drie voor bericht:
1. De `api-key` koptekst moet zijn ingesteld op de query-sleutel die u in stap ik hierboven gevonden. Houd er rekening mee dat ook u een beheerder sleutel als kunt de `api-key` koptekst, maar het wordt aanbevolen dat u de sleutel van een query gebruiken als alleen-lezen toegang tot indexen en documenten uitsluitend wordt toegewezen.
2. De `Accept` koptekst moet zijn ingesteld op `application/json`.
3. Voor het bericht alleen de `Content-Type` koptekst moet ook worden ingesteld op `application/json`.

Zie hieronder voor een HTTP GET-aanvraag om het gebruik van de Azure zoeken REST API, met een eenvoudige query die wordt gezocht naar de term "motel" "hotels"-index te doorzoeken:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Dit is dezelfde voorbeeldquery met behulp van HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Een queryverzoek voor een succesvolle resulteert in de statuscode `200 OK` en de lijst met zoekresultaten worden geretourneerd als JSON in de hoofdtekst van het antwoord. Hier ziet u welke de resultaten voor de bovenstaande query zien er zo, ervan uitgaande dat de index "hotels" wordt gevuld met de voorbeeldgegevens in [Gegevens importeren in Azure zoekprogramma's via de REST API](search-import-data-rest-api.md) (Let erop dat de JSON is opgemaakt voor duidelijkheid).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Ga naar het gedeelte 'Antwoord' van [Documenten zoeken](https://msdn.microsoft.com/library/azure/dn798927.aspx)voor meer informatie. Zie voor meer informatie over andere HTTP statuscodes die kan worden geretourneerd voor het geval is mislukt, [HTTP-statuscodes (Azure zoeken)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
