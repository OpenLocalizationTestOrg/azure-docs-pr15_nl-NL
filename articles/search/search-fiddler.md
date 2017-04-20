<properties
    pageTitle="Het gebruik van Fiddler evalueren en testen van Azure zoeken REST API's | Microsoft Azure | De zoekservice gehoste cloud"
    description="Gebruik Fiddler voor een aanpak code-vrij te geven op beschikbaarheid van Azure zoeken bevestigt en de REST API's uitprobeert."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Fiddler evalueren en testen van Azure zoeken REST API's gebruiken
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Search Explorer](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In dit artikel wordt uitgelegd hoe u Fiddler, een [gratis download van Telerik](http://www.telerik.com/fiddler), probleem HTTP aanvragen voor en antwoorden weergeven met behulp van de Azure zoeken REST API, zonder code te schrijven. Azure zoeken is volledig beheerde, cloud-zoekservice op Microsoft Azure, eenvoudig programmeerbaar tot en met .NET en REST API's die worden gehost. De zoekservice van Azure REST API's op [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)worden beschreven.

In de volgende stappen hebt u een index maken, documenten uploaden, query de index en vervolgens het systeem voor service-informatie opvragen.

Als u wilt deze stappen hebt voltooid, moet u een Azure-zoekservice en `api-key`. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor instructies over het aan de slag.

## <a name="create-an-index"></a>Een index maken

1. Fiddler starten. Uitschakelen in het menu **bestand** **Vastleggen verkeer** overbodige HTTP-activiteit die is gerelateerd aan de huidige taak verbergen.

3. Klik op het tabblad **Composer** hebt u een verzoek om die op de volgende schermopname lijkt opstellen.

    ![][1]

2. Selecteer **plaatsen**.

3. Voer een URL waarmee de URL van de service, Aanvraagkenmerken en de api-versie. Enkele tips in gedachten moet houden:
   + HTTPS als het voorvoegsel gebruiken.
   + Aanvraagkenmerk is "/ indexen/hotels". Deze zoekopdracht tot het maken van een index met de naam 'hotels' worden vermeld.
   + API-versie is kleine letters, opgegeven als '? api-versie 2015-02-28 = ". API-versies zijn belangrijk omdat Azure zoeken updates regelmatig implementeert. Soms kan een update service een recente wijziging kennismaken met de API. Daarom moet Azure zoeken een api-versie op elk verzoek om een zodat u bezig bent met volledig beheer over welke database wordt gebruikt.

    De volledige URL ziet er ongeveer als volgt uitzien.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Geef de aanvraag koptekst, de host en api-sleutel te vervangen door de waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Plak in de velden waaruit de definitie van de index in de hoofdtekst aanvragen.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klik op **uitvoeren**.

In een paar seconden ziet u een antwoord HTTP 201 in de sessielijst, die aangeeft dat u dat de index is gemaakt.

Als u HTTP 504 krijgt, controleert u of dat HTTPS Hiermee geeft u de URL. Als u HTTP 400 of 404 ziet, controleert u het hoofdgedeelte van de aanvraag om te controleren of dat er zijn geen fouten kopiëren plakken. Een HTTP 403 duidt op een probleem met de api-sleutel (een ongeldige sleutel of een probleem is syntaxis met hoe de api-sleutel is opgegeven).

## <a name="load-documents"></a>Documenten laden

Klik op het tabblad **Composer** zullen uw verzoek om documenten te boeken eruit de volgende. De hoofdtekst van de aanvraag bevat de zoekgegevens voor 4 hotels.

   ![][2]

1. Selecteer **posten**.

2.  Voer een URL in die begint met HTTPS, gevolgd door de service-URL, gevolgd door ' /indexes/ < NaamCommunity >/documenten/index? api-versie 2015-02-28 = ". De volledige URL ziet er ongeveer als volgt uitzien.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Koptekst van de aanvraag moet hetzelfde als voor. Vergeet niet dat u de host en api-sleutel vervangen door de waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  De hoofdtekst aanvragen bevat vier documenten moet worden toegevoegd aan de index hotels.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klik op **uitvoeren**.

In een paar seconden ziet u een antwoord HTTP 200 in de sessielijst. Hiermee wordt aangeduid met het dat de documenten die zijn gemaakt. Als er een 207, is ten minste één document kan niet worden geüpload. Als er een 404, hebt u een syntaxisfout in de koptekst of de hoofdtekst van het verzoek.

## <a name="query-the-index"></a>Query de index

Nu dat een index en documenten worden geladen, kunt u query's op deze uitgeven.  Klik op het tabblad **Composer** een **GET** -opdracht query's van uw service ziet er ongeveer uit de volgende schermopname.

   ![][3]

1.  Selecteer **ophalen**.

2.  Voer een URL in die begint met HTTPS, gevolgd door de service-URL, gevolgd door ' /indexes/ < NaamCommunity > / documenten? ", gevolgd door queryparameters. In voorbeeld, gebruik de volgende URL, de naam van de steekproef host vervangen door een dat geschikt is voor uw service.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Deze query zoekt op de term "motel" en facet categorieën voor classificaties zijn opgehaald.

3.  Koptekst van de aanvraag moet hetzelfde als voor. Vergeet niet dat u de host en api-sleutel vervangen door de waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

De antwoordcode moet 200 en de reactie-uitvoer moet lijken op de volgende schermopname.

   ![][4]

De volgende voorbeeldquery afkomstig is van de [zoekindex bewerking (Azure zoeken API)](http://msdn.microsoft.com/library/dn798927.aspx) op MSDN. Veel van de voorbeeld-query's in dit onderwerp spaties, die niet zijn toegestaan in Fiddler. Elk daar ruimte voor is met een + teken voordat deze plakken in de query de tekenreeksexpressie voordat u probeert de query in Fiddler vervangen.

**Voordat u spaties worden vervangen:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Nadat de spaties zijn vervangen door +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Query het systeem

U kunt ook het systeem verkrijgen document telt en opslag verbruik zoeken. Klik op het tabblad **Composer** uw aanvraag kunt invullen ziet er ongeveer als volgt te werk en het antwoord retourneert een telling voor het aantal documenten en hoeveel ruimte wordt gebruikt.

 ![][5]

1.  Selecteer **ophalen**.

2.  Voer een URL met inbegrip van de URL van uw service, gevolgd door "/ indexen/hotels/stat? api-versie 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Geef de aanvraag koptekst, de host en api-sleutel te vervangen door de waarden die geldig voor uw service zijn.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Laat het hoofdgedeelte van de aanvraag leeg.

5.  Klik op **uitvoeren**. Hier ziet u een HTTP 200 statuscode in de sessielijst. Selecteer het item voor uw opdracht geplaatst.

6.  Klik op het tabblad **controle** , klik op het tabblad **kop** en selecteer vervolgens de indeling van JSON. Hier ziet u de grootte van document tellen en opslagruimte (in KB).

## <a name="next-steps"></a>Volgende stappen

Zie [uw zoekservice op Azure beheren](search-manage.md) voor een zonder code voor het beheren en met de zoekfunctie van Azure benaderen.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
