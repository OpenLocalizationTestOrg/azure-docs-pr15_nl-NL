<properties
    pageTitle="Indexen Azure zoeken met de REST API maken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Een index maken in code met behulp van de Azure zoeken HTTP REST API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Indexen Azure zoeken met de REST API maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


In dit artikel wordt u begeleid bij het proces van het maken van een Azure zoeken [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) de Azure zoeken REST API gebruiken.

Voordat u deze handleiding volgen en een index maken, hebt u al [gemaakt van een Azure-zoekservice](search-create-service-portal.md).

Als u wilt maken van een Azure zoekindex de REST API gebruiken, verleent u slechts één HTTP POST-aanvraag naar uw Azure zoekservice van URL-eindpunt. De indexdefinitie van uw worden, opgeslagen in het hoofdgedeelte van de aanvraag als juist opgemaakte JSON-inhoud.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ik. Uw Azure zoekservice van beheerder api-sleutel identificeren
Nu dat u een Azure-zoekservice ingericht hebt, kunt u HTTP-aanvragen ten opzichte van uw service URL eindpunt met de REST API uitgeven. *Alle* API aanvragen moeten echter de api-sleutel die is gegenereerd voor de zoekservice die u deze is ingericht bevatten. Met een geldige sleutel tot stand brengt vertrouwen, op basis per verzoek tussen het verzenden van de aanvraag en de service die omgaat met deze toepassing.

1. Als u wilt zoeken van uw service api-toetsen u moet zich aanmelden bij de [Portal van Azure](https://portal.azure.com/)
2. Ga naar uw Azure zoekservice van blade
3. Klik op het pictogram "Toetsen"

Uw service heeft *beheerder* en *query sleutels*.

 - Uw primaire en secundaire *beheerder toetsen* verlenen volledige rechten voor alle bewerkingen, waaronder de mogelijkheid te beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u de secundaire sleutel gebruiken blijven kunt als u besluit om opnieuw te genereren van de primaire sleutel en vice versa.
 - Uw *query toetsen* alleen-lezen toegang verlenen tot indexen en documenten en meestal zijn verdeeld over clienttoepassingen die zoeken verzoeken.

Voor de toepassing van een index maken, gebruikt u een uw sleutel primaire of secundaire beheerder.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. De zoekindex van Azure met juist opgemaakte JSON definiëren
Een enkele HTTP POST-aanvraag met uw service maakt de Zijtabs met letter. De hoofdtekst van uw HTTP POST-aanvraag bevat één JSON object dat de zoekindex van Azure definieert.

1. De eerste eigenschap van dit JSON-object is de naam van de index.
2. De tweede eigenschap van dit JSON-object is een JSON-matrix met de naam `fields` die voor elk veld in de index een afzonderlijke JSON-object bevat. Elk van deze JSON-objecten bevatten meerdere naam/waardeparen voor elk van de veldkenmerken zoals "naam", "type", enzovoort.

Het is belangrijk dat u uw zoekopdracht gebruiker ervaring en zakelijke behoeften in gedachten behouden bij het ontwerpen van de Zijtabs met letter, zoals de [kenmerken juist zijn ingesteld](https://msdn.microsoft.com/library/azure/dn798941.aspx)door elk veld moet worden toegewezen. Deze kenmerken bepalen welke functies (filteren, faceting, sorteren zoeken in volledige tekst, enzovoort) zoeken van toepassing op welke velden. Voor elk kenmerk dat u geen opgeeft, is de standaardwaarde om in te schakelen van de corresponderende zoekfunctie, tenzij u deze speciaal uitschakelt.

In ons voorbeeld hebt we onze index "hotels" benoemde en onze velden als volgt gedefinieerd:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

We hebt zorgvuldig de indexkenmerken voor elk veld op basis van hoe we dat ze worden gebruikt in een toepassing. Bijvoorbeeld `hotelId` is een unieke sleutel dat personen zoeken naar hotels waarschijnlijk weet niet, zodat we zoeken in volledige tekst voor dat veld door in te stellen uitschakelen `searchable` naar `false`, waarin ruimte wordt opgeslagen in de index.

Houd er rekening mee dat precies één veld in de index van het type `Edm.String` moet de aangewezen als het sleutelveld.

De definitie van de bovenstaande index gebruikt een analyzer aangepaste taal voor de `description_fr` veld omdat deze is bedoeld voor de opslag van Franse tekst. Zie [het onderwerp op MSDN ondersteunen taal](https://msdn.microsoft.com/library/azure/dn879793.aspx) , evenals de bijbehorende [weblog posten](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taal analyzers.

## <a name="iii-issue-the-http-request"></a>III. De HTTP-verzoek verzenden
1. De indexdefinitie van uw gebruikt als het hoofdgedeelte van de aanvraag, moet u een HTTP POST-aanvraag naar de URL van uw Azure zoeken service-eindpunt. Zorg ervoor dat u de servicenaam van uw gebruiken als de hostnaam en de juiste plaats in de URL, `api-version` als tekenreeks queryparameter (de huidige versie van de API `2015-02-28` op het moment van dit document wordt gepubliceerd).
2. Geef in de kop van de aanvraag, de `Content-Type` als `application/json`. U moet ook op te geven van uw service-beheerder opgeven die u die zijn opgegeven in stap I in de `api-key` koptekst.


U hebt uw eigen service naam en api-sleutel voor het verlenen van de onderstaande aanvraag opgeven:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Voor een succesvolle aanvraag ziet u statuscode 201 (gemaakt). Ga de API reference op [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)voor meer informatie over het maken van een index via de REST API's. Zie voor meer informatie over andere HTTP statuscodes die kan worden geretourneerd voor het geval is mislukt, [HTTP-statuscodes (Azure zoeken)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Wanneer u klaar bent met een index en u wilt verwijderen, moet u een verzoek HTTP verwijderen. Dit is bijvoorbeeld hoe we de index 'hotels' wilt verwijderen:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Volgende
Na het maken van een Azure zoekindex, bent u klaar uw inhoud in de index te [uploaden](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
