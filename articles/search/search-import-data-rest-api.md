<properties
    pageTitle="Gegevens uploaden in Azure zoekprogramma's via de REST API | Microsoft Azure | De zoekservice gehoste cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoekprogramma's via de REST API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Gegevens uploaden naar Azure zoekprogramma's via de REST API
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In dit artikel wordt uitgelegd hoe u de [Azure zoeken REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) gebruiken om gegevens te importeren in een Azure-zoekindex.

Voordat u deze procedure begint, moet u al hebt [gemaakt van een Azure-zoekindex](search-what-is-an-index.md).

Om te kunnen push documenten in uw index met de REST API, verleent u een HTTP POST-aanvraag naar van uw index URL eindpunt. De hoofdtekst van het hoofdgedeelte van de aanvraag is een JSON-object met de documenten moeten worden toegevoegd, gewijzigd of verwijderd.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ik. Uw Azure zoekservice van beheerder api-sleutel identificeren
Bij het verlenen van HTTP-aanvragen ten opzichte van uw service door middel van de REST API, moet *elke* API verzoek om de api-sleutel die is gegenereerd voor de zoekservice die u deze is ingericht bevatten. Met een geldige sleutel tot stand brengt vertrouwen, op basis per verzoek tussen het verzenden van de aanvraag en de service die omgaat met deze toepassing.

1. Als u wilt zoeken van uw service api-toetsen u moet zich aanmelden bij de [Portal van Azure](https://portal.azure.com/)
2. Ga naar uw Azure zoekservice van blade
3. Klik op het pictogram "Codes"

Uw service heeft *beheerder* en *query sleutels*.

  - Uw primaire en secundaire *beheerder toetsen* verlenen volledige rechten voor alle bewerkingen, waaronder de mogelijkheid te beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u de secundaire sleutel gebruiken blijven kunt als u besluit om opnieuw te genereren van de primaire sleutel en vice versa.
  - Uw *query toetsen* alleen-lezen toegang verlenen tot indexen en documenten en meestal zijn verdeeld over clienttoepassingen die zoeken verzoeken.

Voor de toepassing van het importeren van gegevens in een index, gebruikt u een uw sleutel primaire of secundaire beheerder.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Bepalen welke indexing actie moet worden gebruikt
Wanneer u met de REST API, verleent u HTTP POST-aanvragen met JSON verzoek instanties naar uw Azure zoekindex van eindpunt URL. De JSON-object in de hoofdtekst van uw HTTP-aanvraag bevat een enkel JSON-matrix met de naam 'waarde' met JSON-objecten dat staat voor documenten die u wilt toevoegen aan uw index, bijwerken of verwijderen.

Elk object JSON in de matrix 'waarde' vertegenwoordigt een document moeten worden geïndexeerd. Elk van deze objecten van het document sleutel bevat en Hiermee geeft u de gewenste indexing actie (uploaden, samenvoegen, verwijderen, enzovoort). Afhankelijk van welke van de onder acties u kiest, alleen bepaalde velden opgenomen voor elk document worden moeten:

@search.action | Beschrijving | Vereiste velden voor elk document | Notities
--- | --- | --- | ---
`upload` | Een `upload` actie is vergelijkbaar met een "upsert" waar het document worden ingevoegd als deze nieuwe en bijgewerkt/vervangen indien aanwezig. | toets, plus eventuele andere velden die u wilt definiëren | Bij een bestaand document bijwerken/vervanging, een veld dat niet is opgegeven in de uitnodiging heeft het veld is ingesteld op `null`. Dit gebeurt zelfs wanneer het veld eerder is ingesteld op een niet-null-waarde.
`merge` | Een bestaand document wordt bijgewerkt met de opgegeven velden. Als het document niet in de index bestaat, mislukt de samenvoeging. | toets, plus eventuele andere velden die u wilt definiëren | Elk veld dat u in een samenvoegbewerking opgeeft wordt vervangen door het bestaande veld in het document. Dit geldt ook voor velden van het type `Collection(Edm.String)`. Als het document bevat een veld bijvoorbeeld `tags` met waarde `["budget"]` en u een samenvoegbewerking met waarde uitvoeren `["economy", "pool"]` voor `tags`, de laatste waarde van de `tags` veld `["economy", "pool"]`. Worden niet `["budget", "economy", "pool"]`.
`mergeOrUpload` | Deze actie lijkt veel op `merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat nog, lijkt veel op `upload` met een nieuw document. | toets, plus eventuele andere velden die u wilt definiëren | -
`delete` | Hiermee verwijdert het opgegeven document uit de index. | alleen sleutel | Velden die u opgeeft andere dan het veld sleutel worden genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `merge` in plaats daarvan en gewoon het veld expliciet ingesteld op null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Maken van uw HTTP-aanvraag en hoofdtekst aanvragen
Nu u hebt de benodigde veldwaarden verzameld voor uw Indexacties, bent u gereed om de werkelijke HTTP-aanvraag en het hoofdgedeelte van de JSON-aanvraag om uw gegevens te importeren te maken.

#### <a name="request-and-request-headers"></a>Vergaderverzoeken en verzoek kopteksten
In de URL, moet u de servicenaam van uw, naam van de index ("hotels' in dit geval), evenals de juiste API-versie bieden (de huidige versie van de API `2015-02-28` op het moment van dit document wordt gepubliceerd). U moet definiëren de `Content-Type` en `api-key` kopteksten aanvragen. Voor de laatste, een van de van uw service-beheerder toetsen te gebruiken.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Hoofdtekst aanvragen

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

In dit geval gebruiken we `upload`, `mergeOrUpload`, en `delete` als onze acties zoeken.

Wordt ervan uitgegaan dat in dit voorbeeld "hotels" index al wordt gevuld met een aantal documenten. Houd er rekening mee hoe we geen om op te geven van alle mogelijke documentvelden bij gebruik van `mergeOrUpload` en hoe we de document-toets alleen opgegeven (`hotelId`) bij gebruik van `delete`.

Ook rekening mee dat u kunt alleen tot 1000 documenten (of 16 MB) in een enkele indexing aanvraag.

## <a name="iv-understand-your-http-response-code"></a>IV. Meer informatie over uw code HTTP-antwoord
#### <a name="200"></a>200
Wanneer u een verzoek voor een succesvolle indexing hebt verzonden, ontvangt u een HTTP-antwoord met statuscode van `200 OK`. De JSON-hoofdtekst van het HTTP-antwoord worden als volgt:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
De statuscode `207` wordt geretourneerd als ten minste één item is niet lukt om via geïndexeerd. De JSON-hoofdtekst van het HTTP-antwoord bevat informatie over de mislukte documenten.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Dit vaak betekent dat het selectievakje laden van uw zoekservice is kunnen bereiken een punt waar aanvragen indexeren begint om terug te keren `503` antwoorden. In dit geval wordt ten zeerste aanbevolen dat uw clientcode weer uitschakelen en wachten voordat deze opnieuw. Hiermee krijgt het systeem enige tijd om te herstellen, de kans dat toekomstige aanvragen slagen toeneemt. Snel opnieuw proberen uw aanvragen, wordt alleen de situatie verlengen.

#### <a name="429"></a>429
De statuscode `429` wordt geretourneerd als u hebt uw quotum van het aantal documenten per index overschreden.

#### <a name="503"></a>503
De statuscode `503` wordt geretourneerd als geen van de items in het verzoek zijn geïndexeerd. Deze fout betekent dat het systeem druk bezet is en uw aanvraag kunt invullen op dit moment kan niet worden verwerkt.

> [AZURE.NOTE] In dit geval wordt ten zeerste aanbevolen dat uw clientcode weer uitschakelen en wachten voordat deze opnieuw. Hiermee krijgt het systeem enige tijd om te herstellen, de kans dat toekomstige aanvragen slagen toeneemt. Snel opnieuw proberen uw aanvragen, wordt alleen de situatie verlengen.

Voor meer informatie over Documentacties en success/fout antwoorden, raadpleegt u de [toevoegen, bijwerken of documenten verwijderen](https://msdn.microsoft.com/library/azure/dn798930.aspx). Zie voor meer informatie over andere HTTP statuscodes die kan worden geretourneerd voor het geval is mislukt, [HTTP-statuscodes (Azure zoeken)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Volgende
Na het vullen van de zoekindex van Azure, bent u klaar om te beginnen met het verlenen van query's om te zoeken naar documenten. Zie [Uw Azure-zoekindex Query](search-query-overview.md) voor meer informatie.
