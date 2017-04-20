<properties
   pageTitle="Azure-Service REST API-versie 2015-02-28-voorbeeld Search | Microsoft Azure | Voorbeeld van Azure Search API"
   description="Azure voorbeeldweergave zoeken Service REST API-versie 2015-02-28-experiment functies zoals natuurlijke taal Analyzers en moreLikeThis zoekopdrachten."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure zoeken Service REST API: Versie 2015-02-28-voorbeeld

In dit artikel is bedoeld voor de documentatie bij `api-version=2015-02-28-Preview`. In dit voorbeeld wordt de huidige versie van de algemeen beschikbaar uitgebreid [api-versie 2015-02-28 =](https://msdn.microsoft.com/library/dn798935.aspx), door op te geven van de volgende experiment functies:

- `moreLikeThis`queryparameter in de [Zoeken-documenten](#SearchDocs) API. Andere documenten die relevant voor een ander specifiek document zijn vindt.

Een paar extra onderdelen van de `2015-02-28-Preview` REST API afzonderlijk worden beschreven. Hierbij:

- [Score profielen](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexeerfuncties](search-api-indexers-2015-02-28-preview.md)

Azure Search-service is beschikbaar in meerdere versies. Raadpleeg [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie.

## <a name="apis-in-this-document"></a>API's in dit document

Azure zoeken service API ondersteunt twee URL-syntaxis voor API bewerkingen: eenvoudig en OData (Zie [ondersteuning voor OData (Azure zoeken API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) voor meer informatie). De volgende lijst bevat de syntaxis van de eenvoudige.

[Indexen maken](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index bijwerken](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index ophalen](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Vermelding indexen](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Indexstatistieken opvragen](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analyzer testen](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Indexen verwijderen](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Toevoegen, verwijderen, en gegevens binnen een Index bijwerken](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Documenten zoeken](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Lookup-Document](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Aantal documenten](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Suggesties](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Indexbewerkingen

U kunt maken en beheren van indexen in Azure Search-service via een eenvoudige HTTP aanvragen (POST, ophalen, wordt opgeslagen, DELETE) ten opzichte van een resource opgegeven index. Als u wilt een index maken, moet u eerst een JSON-document waarin wordt beschreven van het schema index posten. Het schema Hiermee definieert u de velden van de index, hun gegevenstypen en hoe ze kunnen worden gebruikt (bijvoorbeeld in volledige tekst zoekopdrachten, filters, sortering of faceting). Ook wordt gedefinieerd score profielen, suggesters en andere kenmerken om te configureren van het gedrag van de index.

In het volgende voorbeeld ziet u een schema voor het zoeken op Hotelinformatie met het veld Beschrijving is gedefinieerd in twee talen gebruikt. Zoals u ziet hoe kenmerken bepalen hoe het veld wordt gebruikt. Bijvoorbeeld de `hotelId` als het document wilt gebruiken (`"key": true`) en wordt uitgesloten van volledige tekst zoekopdrachten (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nadat de index is gemaakt, uploadt u documenten die de index te vullen. Zie [toevoegen of bijwerken documenten](#AddOrUpdateDocuments) voor deze stap.

Voor een video-Inleiding tot het indexeren in Azure zoeken, raadpleegt u het [kanaal 9 Cloud begeleidende aflevering van Azure zoeken](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Indexen maken

Een index is het belangrijkste middel ordenen en zoeken van documenten in Azure zoeken, vergelijkbaar met hoe een tabel records in een database organiseert. Elke index heeft een verzameling documenten dat alle aan het schema index (veldnamen, gegevenstypen en eigenschappen voldoen), maar indexen ook opgeven extra constructies (suggesters, score profielen en CORS opties) die andere gedrag zoeken definiëren.

U kunt een nieuwe index maken in een Azure-zoekservice met een verzoek om HTTP POST of opslag. De hoofdtekst van de aanvraag is een schema JSON waarin de gegevens index en configuratie.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook opslag gebruiken en geef de indexnaam van de op de URI. Als u de index niet bestaat, kunt u het wordt gemaakt.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Een index maken, bepaalt de structuur van de documenten die zijn opgeslagen en gebruikt in zoekopdrachten. Vullen met de index is een afzonderlijke bewerking. Voor deze stap kunt u een [indexering](https://msdn.microsoft.com/library/azure/mt183328.aspx) (beschikbaar voor ondersteunde gegevensbronnen) of een bewerking [toevoegen, bijwerken of documenten verwijderen](https://msdn.microsoft.com/library/azure/dn798930.aspx) . De omgekeerde index wordt gegenereerd wanneer de documenten worden geplaatst.

**Opmerking**: het maximale aantal toegestane indexen is afhankelijk van de prijzen van laag. De gratis service kunnen maximaal 3 indexen. Standaard service kunt 50 indexen per Search-service. Zie [beperkingen en limieten](http://msdn.microsoft.com/library/azure/dn798934.aspx) voor meer informatie.

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Create Index** kan worden samengesteld met behulp van een bericht of de opslag methode. Wanneer u een bericht gebruikt, moet u de indexnaam van een in het hoofdgedeelte van de aanvraag samen met de schemadefinitie index opgeven. Met opgeslagen is de indexnaam van de onderdeel van de URL. Als de index niet bestaat, wordt deze is gemaakt. Als dit al bestaat, wordt deze bijgewerkt naar de nieuwe definitie.

De indexnaam van de moet worden kleine letters, beginnen met een letter of een getal, hebben geen streepjes of stippellijnen en minder dan 128 tekens bevatten. De rest van de naam kunt nadat de indexnaam van de met een letter of een getal is gestart, opnemen een letter, getal en streepjes, zolang de streepjes geen opeenvolgende zijn.

De `api-version` is vereist. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor een lijst met beschikbare versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor
- het verzoek voor de zoekservice wordt geverifieerd. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Create Index** moet bevatten een `api-key` koptekst ingesteld op uw sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U kunt zowel de servicenaam verkrijgen en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

<a name="RequestData"></a>
**De syntaxis van de hoofdtekst aanvragen**

De hoofdtekst van de aanvraag bevat de schemadefinitie van een, waaronder de lijst met gegevensvelden in documenten die worden geladen deze index, gegevenstypen, kenmerken, evenals een optionele lijst scores voor profielen die worden gebruikt voor het verkrijgen van overeenkomende documenten tegelijk query.

Houd er rekening mee dat een POST-aanvraag, moet u de indexnaam van de in de hoofdtekst van het verzoek.

Er kan niet meer dan één sleutelveld in de index. Er moet een tekenreeksveld. Dit veld vertegenwoordigt de unieke id voor elk document dat is opgeslagen in de index.

De belangrijkste onderdelen van een index onder andere het volgende:

- `name`
- `fields`die wordt in deze index, zoals naam, het gegevenstype en eigenschappen die toegestane acties op dat veld definiëren worden ingevoerd.
- `suggesters`wordt gebruikt voor automatisch aanvullen of aanvullen tijdens typen query's.
- `scoringProfiles`wordt gebruikt voor aangepaste zoekresultaten score classificeren. Zie [score profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie.
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` gebruikt om te bepalen hoe uw documenten/query's worden onderverdeeld in worden geïndexeerd/doorzoekbare tokens. Gaat u naar [analyse in Azure zoeken](https://aka.ms//azsanalysis) voor meer informatie.
- `defaultScoringProfile`wordt gebruikt voor de standaardinstelling scoren gedrag overschrijven.
- `corsOptions`toe te staan dat cross-origin query's op de Zijtabs met letter.

De syntaxis voor het structureren van de aanvraag nettolading is als volgt. Een voorbeeld van een aanvraag is opgegeven op verder in dit onderwerp.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexkenmerken**

De volgende kenmerken kunnen worden ingesteld wanneer u een index maakt. Zie [score profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx)voor meer informatie over profielen score en score.

`name`-Hiermee stelt u de naam van het veld.

`type`-Hiermee stelt u het gegevenstype voor het veld. Zie [Gegevenstypen ondersteund](#DataTypes) voor een lijst met ondersteunde typen.

`searchable`-Hiermee markeert u het veld als volledige tekst zoeken kunnen. Dit betekent dat deze worden onderworpen aan analyse zoals woordafbreking tijdens het indexeren. Als u een `searchable` veld toe aan een waarde is zoals "zonnig dag', intern deze wordt gesplitst in de afzonderlijke tokens 'zonnig' en 'dag'. Hiermee worden de volledige tekst wordt gezocht naar deze termen. Velden van het type `Edm.String` of `Collection(Edm.String)` zijn `searchable` al dan niet standaard. Velden van andere typen kunnen niet worden `searchable`.

  - **Opmerking**: `searchable` velden extra ruimte in uw index gebruiken omdat Azure zoeken een extra tokens versie van de waarde van het veld voor volledige tekst zoekacties wordt opgeslagen. Als u wilt Bespaar ruimte in uw index en u een veld hoeft moet worden opgenomen in zoekopdrachten, stelt u `searchable` naar `false`.

`filterable`-Hiermee kunt het veld waarnaar u wilt verwijzen `$filter` query's. `filterable`verschilt van `searchable` in hoe tekenreeksen worden afgehandeld. Velden van het type `Edm.String` of `Collection(Edm.String)` die zijn `filterable` niet worden bewerkt woordafbreking, zodat vergelijkingen zijn voor exact overeenkomt met alleen. Als u een dergelijk veld instellen bijvoorbeeld `f` naar 'zonnig dag', `$filter=f eq 'sunny'` vindt geen overeenkomsten, maar `$filter=f eq 'sunny day'` wordt. Alle velden zijn `filterable` al dan niet standaard.

`sortable`-Al dan niet standaard het systeem resultaten sorteren op score, maar in veel ervaringen gebruikers wilt sorteren op velden in de documenten. Velden van het type `Collection(Edm.String)` kunnen niet worden `sortable`. Alle andere velden zijn `sortable` al dan niet standaard.

`facetable`-Meestal gebruikt in een presentatie van zoekresultaten met treffers per categorie (voor bijvoorbeeld, zoeken naar digitale camera's en Zie treffers op merk, door megapixels, door prijs, enz.). Deze optie kan niet worden gebruikt met velden van het type `Edm.GeographyPoint`. Alle andere velden zijn `facetable` al dan niet standaard.

  - **Opmerking**: velden van het type `Edm.String` die zijn `filterable`, `sortable`, of `facetable` mag niet meer dan 32 KB lengte. Dit is omdat deze velden worden behandeld als een eenmalige zoekterm en de maximale lengte van een term in Azure zoeken 32KB is. Als u meer tekst dan deze opslaan in een veld één tekenreeks wilt, moet u expliciet instellen `filterable`, `sortable`, en `facetable` naar `false` in de indexdefinitie van uw.

  - **Opmerking**: als een veld geen van de bovenstaande kenmerken die is ingesteld heeft op `true` (`searchable`, `filterable`, `sortable`, of`facetable`) het veld effectief wordt uitgesloten van de omgekeerde index. Deze optie is handig voor velden die niet in query's worden gebruikt, maar in de zoekresultaten u nodig hebt. Met uitzondering van dergelijke velden uit de index verbeterde prestaties.

`key`-Hiermee markeert u het veld unieke id's voor documenten binnen de index bevatten. Exact één veld moet worden gekozen als de `key` velden en deze moeten van het type `Edm.String`. Sleutelvelden kunnen opzoeken documenten rechtstreeks via de [API voor opzoeken](#LookupAPI)worden gebruikt.

`retrievable`-Instellen of het veld in een zoekresultaat kan worden geretourneerd.  Dit is handig wanneer u een veld (bijvoorbeeld, marge) gebruiken als filter, wilt sortering of om scoren, maar niet dat het veld wilt alleen zichtbaar voor de gebruiker. Dit kenmerk moet `true` voor `key` velden.

`analyzer`-Hiermee stelt u de naam van de analyzer wilt gebruiken voor het veld op zoeken en indexeren. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane set waarden. Deze optie kan alleen worden gebruikt met `searchable` velden en deze kunnen niet worden ingesteld samen met een `searchAnalyzer` of `indexAnalyzer`.  Als de analyzer is gekozen, kan deze niet worden gewijzigd voor het veld.

`searchAnalyzer`-Hiermee stelt u de naam van de analyzer tegelijk zoeken voor het veld gebruikt. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane set waarden. Deze optie kan alleen worden gebruikt met `searchable` velden. Moet worden ingesteld samen met `indexAnalyzer` en deze kan niet worden ingesteld samen met de `analyzer` optie. Deze analyzer kan worden bijgewerkt op een bestaand veld.

`indexAnalyzer`-Hiermee stelt u de naam van de analyzer indexing tegelijk voor het veld gebruikt. Zie [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx)voor de toegestane set waarden. Deze optie kan alleen worden gebruikt met `searchable` velden. Moet worden ingesteld samen met `searchAnalyzer` en deze kan niet worden ingesteld samen met de `analyzer` optie. Als de analyzer is gekozen, kan deze niet worden gewijzigd voor het veld.

`suggesters`-Hiermee stelt u de modus en de velden die de bron van de inhoud voor suggesties. Zie [Suggesters](#Suggesters) voor meer informatie.

`scoringProfiles`-Hiermee definieert u aangepaste score gedrag waarmee u van invloed zijn op welke items worden weergegeven op een hoger in de lijst met zoekresultaten. Score profielen bestaan uit het veld lijndikten en functies. Zie [scoren profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie over de kenmerken die worden gebruikt in een score profiel.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Ondersteuning voor talen**

Doorzoekbare velden worden analyse dat het grootste deel vaak houdt woordafbreking, tekst normaliseren en het wegfilteren van termen. Doorzoekbare velden in Azure zoeken worden standaard geanalyseerd met de [Apache Lucene standaard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) waarin tekst opgesplitst in elementen volgens de regels["segmentatie Unicode-tekst"](http://unicode.org/reports/tr29/) . Bovendien hierop de standaard analyzer alle tekens aan hun formulier kleine letters. Geïndexeerde documenten zowel zoektermen leest u over de analyse tijdens indexeren en queryverwerking.

Azure zoeken ondersteunt verschillende talen. Elke taal vereist een niet-standaard tekst analyzer die kenmerken van een bepaalde taal. Azure zoeken biedt twee soorten analyzers:

- 35 analyzers ondersteund door Lucene.
- 50 analyzers ondersteund door, bedrijfseigen Microsoft natuurlijke taal storende gebruikt in Office en Bing.

Sommige ontwikkelaars liever de oplossing meer vertrouwde, eenvoudige, open source van Lucene. Lucene analyzers sneller zijn, maar de Microsoft-analyzers hebt geavanceerde functies, zoals Lemmata, word decompounding (in talen zoals Duits, Deens, Nederlands, Zweeds, Noors, Estlands, voltooien, Hongaars, Slowaaks) en entiteit herkenning (URL's, e-mailberichten, datums, getallen). Indien mogelijk moet u vergelijkingen van Microsoft zowel Lucene analyzers te bepalen welke een beter geschikt is uitgevoerd.

***Hoe ze vergelijken***

De Lucene analyzer voor Engels breidt de standaard analyzer. Deze verwijdert bezittelijke naamwoorden (volgspaties van) uit woorden, is van toepassing als gevolg aan de hand van [Porter afleiding algoritme](http://tartarus.org/~martin/PorterStemmer/), en Engelse [woorden stoppen](http://en.wikipedia.org/wiki/Stop_words).

De Microsoft-analyzer uitvoeren in vergelijking Lemmata in plaats van afleiding. Het betekent dit dat deze kan omgaan met verbogen en onregelmatig word formulieren veel beter welke resultaten in meer relevante zoekresultaten (controle module 7 van [Azure zoeken MVA presentatie](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) voor meer informatie).

Indexering met Microsoft analyzers is op twee tot drie keer langzamer dan hun equivalenten Lucene, afhankelijk van de taal van het gemiddelde. Zoekprestaties geen aanzienlijk invloed voor gemiddelde grootte query's.

***Configuratie***

Voor elk veld in de definitie van de index, kunt u instellen de `analyzer` eigenschap waarmee de naam van een analyzer waarmee wordt bepaald welke taal en leverancier. De dezelfde analyzer worden toegepast wanneer indexeren en het zoeken van dat veld.
U kunt bijvoorbeeld afzonderlijke velden voor Engels, Frans en Spaans hotel beschrijvingen die aanwezig naast elkaar in dezelfde index hebben. De [queryparameter 'searchFields'](#SearchQueryParameters) gebruiken om op te geven welk veld taalspecifieke wilt zoeken tegen in uw query's. U kunt bekijken query voorbeelden waarin de `analyzer` eigenschap in [Documenten zoeken](#SearchDocs). 

***Analyzer lijst***

Hieronder vindt u de lijst met ondersteunde talen samen met de namen van Lucene en Microsoft analyzer.

<table style="font-size:12">
    <tr>
        <th>Taal</th>
        <th>De naam van de Microsoft-analyzer</th>
        <th>Lucene analyzer naam</th>
    </tr>
    <tr>
        <td>Arabisch</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>Armeens</td>
        <td></td>
        <td>Hy.lucene</td>
    </tr>
    <tr>
        <td>Bengaals</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskisch</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>Bulgaars</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>Catalaans</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>Vereenvoudigd Chinees</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>Traditioneel Chinees</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>Kroatisch</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tsjechisch</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>Deens</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>Nederlands</td>
        <td>nl.Microsoft</td>
        <td>nl.lucene</td>  
    </tr>    
    <tr>
        <td>Engels</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>Estisch</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Fins</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>Frans</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>Galicisch</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>Duits</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>Grieks</td>
        <td>El.Microsoft</td>
        <td>El.lucene</td>      
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebreeuws</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>Hongaars</td>      
        <td>Hu.Microsoft</td>
        <td>Hu.lucene</td>
    </tr>
    <tr>
        <td>IJslands</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesisch (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>Iers</td>
        <td></td>
        <td>Ga.lucene</td>
    </tr>
    <tr>
        <td>Italiaans</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>Japans</td>
        <td>Ja.Microsoft</td>
        <td>Ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>Ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreaans</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lets</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>Litouws</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maleis (Latijns)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Noors</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>Perzisch</td>
        <td></td>
        <td>FA.lucene</td>      
    </tr>
    <tr>
        <td>Pools</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugees (Brazilië)</td>
        <td>pt-Br.microsoft</td>
        <td>pt-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugees (Portugal)</td>
        <td>pt-Pt.microsoft</td>        
        <td>pt-Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Roemeens</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Russisch</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>Servisch (Cyrillisch)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Servisch (Latijns)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowaaks</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Sloveens</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spaans</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Zweeds</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thais</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turks</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>Oekraïens</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamees</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Bovendien biedt Azure zoeken taal-agnostic analyzer configuraties</td>
    <tr>
        <td>Standaard ASCII vouwen</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode-tekst segmentatie (standaard Basisvormherkenning)</li>
            <li>ASCII-opvouwbaar filter - converteert Unicode-tekens die geen deel uitmaakt van naar de reeks eerst 127 ASCII-tekens in de ASCII-equivalenten. Dit is handig voor het verwijderen van diakritische tekens.</li>
        </ul>
        </td>
    </tr>
</table>

Alle analyzers met namen die zijn gemarkeerd met <i>lucene</i> zijn mogelijk gemaakt door [de Apache Lucene taal analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Meer informatie over de ASCII vouwen filter vindt u [hier](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` wordt gedefinieerd welke velden in een index worden gebruikt voor het ondersteunen van automatisch aanvullen in zoekopdrachten. Automatische zoekfunctie tekenreeksen zijn meestal verzonden naar de [Suggesties API](#Suggestions) terwijl de gebruiker een zoekopdracht typt, en de API geeft als een reeks voorgestelde woordgroepen resultaat. Een suggester die u in de index definieert wordt bepaald welke velden worden gebruikt voor het maken van de tekst aanvullen zoektermen. Zie [Suggesters](#Suggesters) voor configuratiegegevens.

**Score profielen**

A `scoringProfile` aangepast score gedrag waarmee u van invloed zijn op welke items worden weergegeven in de lijst met zoekresultaten hoger definieert. Score profielen bestaan uit het veld lijndikten en functies. Als u wilt gebruiken, kunt u een profiel opgeven met de naam van de queryreeks.

Een profiel scoren standaard werkt achter de schermen om een score zoeken voor elk item in een resultatenset te berekenen. U kunt de interne, niet-benoemde score profiel gebruiken. U kunt ook instellen `defaultScoringProfile` gebruik van een aangepast profiel als het standaardapparaat, aangeroepen wanneer een aangepast profiel niet is opgegeven in de query-tekenreeks.

Zie [score profielen toevoegen aan een zoekindex (Azure zoeken Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) voor meer informatie.

**Opties voor CORS**

Aan de clientzijde Javascript kan een API's niet al dan niet standaard aanroepen sinds de browser voorkomen alle cross-origin aanvragen dat wordt. CORS (Cross-Origin resources delen) inschakelen door in te stellen de `corsOptions` kenmerk toe te staan dat cross-origin query's naar de Zijtabs met letter. Houd er rekening mee dat alleen query API's ondersteuning CORS vanwege de beveiliging. De volgende opties kunnen voor CORS worden ingesteld:

- `allowedOrigins`(vereist): dit is een lijst met oorspronkelijke bestanden dat toegang tot de index wordt verleend. Dit betekent dat eventuele Javascript-code van deze oorsprong bediend kunnen worden om query's in de index (ervan uitgaande dat dit de juiste API-sleutel biedt). Elke oorsprong is meestal van het formulier `protocol://fully-qualified-domain-name:port` Hoewel de poort vaak wordt weggelaten. Zie [dit artikel](http://go.microsoft.com/fwlink/?LinkId=330822) voor meer informatie.
 - Als u toestaan van toegang tot alle oorspronkelijke bestanden wilt, bevatten `*` als één item in de `allowedOrigins` matrix. Houd er rekening mee dat **Dit wordt niet aanbevolen procedure voor het zoeken van boorputten.** Dit kan echter nuttig zijn voor de ontwikkeling of foutopsporing zijn.
- `maxAgeInSeconds`(optioneel): Browsers gebruikt deze waarde om te bepalen de duur (in seconden) om de cache CORS Preflight-antwoorden. Dit moet een positief geheel getal. Des te groter is van deze waarde kan de betere prestaties zijn, maar het langer duurt voor CORS beleidswijzigingen zijn doorgevoerd. Als dit niet is ingesteld, wordt de standaardduur van een van 5 minuten worden gebruikt.

<a name="CreateUpdateIndexExample"></a>
**Voorbeeld van de hoofdtekst aanvragen**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Antwoord**

Voor een succesvolle aanvraag: '201 gemaakt'.

Standaard bevat de hoofdtekst van het antwoord de JSON voor de definitie van de index die is gemaakt. Als de `Prefer` koptekst van de aanvraag is ingesteld op `return=minimal`, de hoofdtekst van het antwoord is leeg en wordt de statuscode success "204 geen inhoud" in plaats van '201 gemaakt'. Dit geldt ook opslag of POST is gebruikt om de index te maken.

**Opmerkingen**

Er is momenteel beperkte ondersteuning voor index schema-updates. Schema updates waarvoor het opnieuw indexeren zoals het wijzigen van veldtypen worden momenteel niet ondersteund. Hoewel bestaande velden kunnen niet worden gewijzigd of verwijderd, nieuwe velden kunnen worden toegevoegd aan een bestaande index op elk gewenst moment. Wanneer een nieuw veld wordt toegevoegd, hebben alle bestaande documenten in de index wordt automatisch een null-waarde voor dat veld. Geen extra opslagruimte wordt verbruikt totdat nieuwe documenten zijn toegevoegd aan de index.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

De functie van de suggesties in Azure zoeken is de mogelijkheid van een query voor tekst aanvullen of automatisch aanvullen, met een lijst van mogelijke zoektermen in antwoord op gedeeltelijke tekenreeks invoer ingevoerd in een zoekvak. U waarschijnlijk opgevallen Querysuggesties bij gebruik van de commerciële web zoekprogramma's: een lijst met voorwaarden voor het typen van ".NET" in Bing oplevert ".NET 4.5 uitvoeren ', '.NET Framework 3.5", enzovoort. Wanneer u met de zoekservice REST API, suggesties implementeren in een aangepaste Azure-zoektoepassing is het volgende nodig:

- Suggesties inschakelen door toe te voegen een constructie **suggester** in de index, geeft de naam van de modus en een lijst met velden waarvoor aanvullen wordt aangeroepen. Bijvoorbeeld als u "stad" als een bronveld typen een gedeeltelijke zoekopdracht van "Zee" resulteert in "Seattle", "Strand" en "Seatac" (alle drie zijn de plaatsnamen van de werkelijke) aangeboden als Querysuggesties aan de gebruiker.

- Suggesties door te bellen van de [Suggesties API](#Suggestions) in uw toepassingscode roepen. Automatische zoekfunctie tekenreeksen zijn meestal verzonden naar de service terwijl de gebruiker een zoekopdracht typt, en deze API geeft als een reeks voorgestelde woordgroepen resultaat.

In dit artikel wordt uitgelegd hoe u een **suggester**configureert. Controleer ook de [Suggesties API](#Suggestions) voor meer informatie over hoe een suggester worden gebruikt.

**Gebruik**

`Suggesters`zijn gemaakt in de index en werk beste wanneer wijzen op specifieke documenten in plaats van losse termen of woordgroepen. De beste candidate-velden zijn titels, namen en andere relatief woordgroepen die een item kunnen identificeren. Minder effectieve zijn terugkerende velden, zoals categorieën en tags of erg lang velden zoals beschrijvingen of opmerkingen velden.

Als onderdeel van de definitie van de index, kunt u een enkel suggester naar toevoegen de `suggesters` siteverzameling. Eigenschappen die een suggester definiëren onder andere het volgende:

- `name`: De naam van de suggester. U de naam van de suggester gebruiken bij het aanroepen van de `suggest` API.
- `searchMode`: De strategie die wordt gebruikt om te zoeken naar woordgroepen candidate. De modus alleen momenteel ondersteund is `analyzingInfixMatching`, flexibele overeenkomende van woordgroepen aan het begin of in het midden van zinnen uitvoert.
- `sourceFields`: Een lijst met een of meer velden die de bron van de inhoud voor suggesties. Alleen velden van het type `Edm.String` en `Collection(Edm.String)` mogelijk bronnen voor suggesties. Alleen velden met een aangepaste taal analyzer instellen niet kunnen worden gebruikt.

**Voorbeeld van suggester**

Een suggester maakt deel uit van de index. Slechts één suggester kan bestaan het `suggesters` verzamelen in de huidige versie, samen met de Veldenverzameling en `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Als u de openbare preview-versie van Azure zoeken, gebruikt `suggesters` Hiermee vervangt u een oudere Booleaanse eigenschap (`"suggestions": false`) die alleen voorvoegsel suggesties voor korte tekenreeksen (3-25 tekens) worden ondersteund. De vervangende, `suggesters`, ondersteunt infix overeenkomende waarmee wordt gezocht naar overeenkomende termen aan het begin of midden op de veldinhoud, met betere tolerantie voor fouten in tekenreeksen met zoeken. Beginnen met het algemeen beschikbare versie, is dit nu de enige implementatie van de suggesties API. De oudere `suggestions` eigenschap die is geïntroduceerd in `api-version=2014-07-31-Preview` nog steeds in deze versie werkt, maar werkt niet in de `2015-02-28` of hogere versies van Azure zoekopdracht.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Index bijwerken

U kunt een bestaande index binnen Azure zoekprogramma's via een aanvraag HTTP plaatsen bijwerken. Updates kunnen opnemen nieuwe velden toevoegen aan het bestaande schema, CORS opties wijzigen en wijzigen, score profielen. Zie [scoren profielen toevoegen](https://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie. U opgeven de naam van de index wilt bijwerken op de aanvraag URI:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Belangrijke:** Ondersteuning voor index schema-updates is beperkt tot de bewerkingen die geen nodig bij het opnieuw opbouwen de zoekindex. Schema updates waarvoor het opnieuw indexeren, zoals het wijzigen van veldtypen, worden momenteel niet ondersteund. Nieuwe velden kunnen worden toegevoegd op elk gewenst moment, hoewel bestaande velden kunnen niet worden gewijzigd of verwijderd. Geldt ook voor `suggesters`. Nieuwe velden kunnen worden toegevoegd aan een suggester op het moment de velden worden toegevoegd, maar velden kunnen niet worden verwijderd uit `suggesters` en bestaande velden kunnen niet worden toegevoegd aan `suggesters`.

Wanneer u een nieuw veld toevoegt aan een index, hebben alle bestaande documenten in de index wordt automatisch een null-waarde voor dat veld. Geen extra opslagruimte wordt verbruikt totdat nieuwe documenten zijn toegevoegd aan de index.

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Index bijwerken** gemaakt met HTTP plaatsen. Met opgeslagen is de indexnaam van de onderdeel van de URL. Als de index niet bestaat, wordt deze is gemaakt. Als de index al bestaat, wordt deze bijgewerkt naar de nieuwe definitie.

De indexnaam van de moet worden kleine letters, beginnen met een letter of een getal, hebben geen streepjes of stippellijnen en minder dan 128 tekens bevatten. De rest van de naam kunt nadat de indexnaam van de met een letter of een getal is gestart, opnemen een letter, getal en streepjes, zolang de streepjes geen opeenvolgende zijn.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Index bijwerken** moet bevatten een `api-key` koptekst ingesteld op uw sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**De syntaxis van de hoofdtekst aanvragen**

Bij het bijwerken van een bestaande index, moet de hoofdtekst bevatten de oorspronkelijke schemadefinitie, plus de nieuwe velden die u wilt toevoegen, evenals de gewijzigde score profielen, suggesters en CORS opties, indien van toepassing. Als u niet de score profielen en CORS opties wijzigt, moet u de oorspronkelijke wanneer de index is gemaakt opnemen. In het algemeen het beste patroon voor updates is om te halen de definitie van de index met een ophalen, wijzigen en klik vervolgens bijwerken met opslag.

De syntaxis van de schema kon u een index maken is hier gereproduceerd uitkomt. Zie [Index maken](#CreateIndex) voor meer informatie.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Antwoord**

Voor een succesvolle aanvraag: "204 geen inhoud".

Al dan niet standaard wordt de hoofdtekst van het antwoord niet leeg zijn. Echter als de `Prefer` koptekst van de aanvraag is ingesteld op `return=representation`, de hoofdtekst van het antwoord bevat de JSON voor de indexdefinitie die werd bijgewerkt. In dit geval de statuscode success worden ' 200 OK ".

**Definitie van de index bijwerken met aangepaste analyzers**

Als een analyzer, een Basisvormherkenning, een token filter of een teken filter is gedefinieerd, kan niet worden gewijzigd. Nieuwe bestanden kunnen worden toegevoegd aan een bestaande index alleen als de `allowIndexDowntime` vlag is ingesteld op waar in het verzoek van de update index: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Houd er rekening mee dat deze bewerking uw index offline voor ten minste een paar seconden, veroorzaakt door uw indexeren en queryaanvragen mislukt wordt geplaatst. En wegschrijven beschikbaarheid van de index kan zijn gehandicapten voor enkele minuten nadat de index wordt bijgewerkt, of langer voor zeer grote indexen.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Lijst met indexen

De bewerking **Lijst indexen** wordt een lijst met de indexen in uw Azure Search-service.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Lijst indexen** worden gemaakt volgens de GET-methode.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. De **Lijst indexen** aanvraag moet bevatten een `api-key` ingesteld op een sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

Hier volgt een voorbeeld antwoord hoofdtekst:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Houd er rekening mee dat u het antwoord naar beneden af op alleen de eigenschappen waarin u geïnteresseerd bent kunt filteren. Als u alleen een lijst met indexnamen wilt, gebruik bijvoorbeeld de OData `$select` optie query:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

In dit geval er de reactie van het bovenstaande voorbeeld als volgt:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Dit is een handige techniek om op te slaan bandbreedte als er een groot aantal indexen in uw Search-service.

<a name="GetIndex"></a>
## <a name="get-index"></a>Index ophalen

De bewerking **Ophalen Index** krijgt de indexdefinitie van Azure zoekresultaten.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. Het verzoek **Krijgen Index** kan worden samengesteld met de methode GET.

[Naam van de index] in het verzoek URI geeft aan welke index om terug te keren uit de verzameling indexen.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Krijgen Index** moet bevatten een `api-key` ingesteld op een sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

Zie het voorbeeld JSON in [maken en bijwerken van een Index](#CreateUpdateIndexExample) voor een voorbeeld van de nettolading antwoord.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Index verwijderen

De bewerking **Index verwijderen** Hiermee verwijdert u een index en de bijbehorende documenten vanuit uw Azure Search-service. U kunt de indexnaam van de kunt u vanuit het servicedashboard in de portal van Azure of vanuit de API. Zie [Lijst indexen](#ListIndexes) voor meer informatie.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. Het verzoek **Index verwijderen** worden gemaakt volgens de methode verwijderen.

[Naam van de index] in het verzoek URI geeft aan welke index verwijderen uit de verzameling indexen.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor de URL van uw service. Het verzoek **Index verwijderen** moet bevatten een `api-key` koptekst ingesteld op uw sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 204 geen inhoud geretourneerd voor een succesvolle reactie.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Indexstatistieken opvragen

De bewerking **Ophalen indexstatistieken** geeft als resultaat van Azure zoekresultaten een telling van het document voor de huidige index, plus het gebruik van opslagruimte.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistieken van grootte voor het aantal en opslag van het document zijn om de paar minuten, niet in realtime verzameld. De statistieken die het resultaat van deze API kunnen daarom niet overeen met wijzigingen die worden veroorzaakt door recente indexing bewerkingen.

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Krijgen indexstatistieken** worden gemaakt volgens de GET-methode.

[Naam van de index] in het verzoek URI Hiermee wordt aan de service om terug te keren indexstatistieken voor de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.


**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Krijgen indexstatistieken** moet bevatten een `api-key` ingesteld op een sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

De hoofdtekst van het antwoord heeft de volgende indeling:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Analyzer testen

De **API analyseren** ziet u hoe een analyzer tekst opgesplitst in tokens.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Analyseren API** kan worden samengesteld met de methode POST.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.


**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Analyseren API** moet bevatten een `api-key` ingesteld op een sleutel beheerder (in plaats van een sleutel query).

U moet ook de indexnaam van de en de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

of

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

De `analyzer_name`, `tokenizer_name`, `token_filter_name` en `char_filter_name` moet geldige namen van vooraf gedefinieerd of aangepast analyzers, tokenizers, token filters en teken filters voor de index. Voor meer dat informatie over het proces van het lexicale analyse gaat u naar [analyse in Azure zoeken](https://aka.ms/azsanalysis).

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

De hoofdtekst van het antwoord heeft de volgende indeling:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Voorbeeld van de API analyseren**

**Aanvragen**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Antwoord**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Document-bewerkingen

In Azure zoeken, een index is opgeslagen in de cloud en gevuld met JSON-documenten die u naar de service uploaden. Alle documenten die u uploadt bestaan uit de corpus van uw zoekgegevens. Documenten bevatten velden, waarvan sommige zijn ge? exeerd in zoektermen terwijl ze worden geüpload. De `/docs` segment van de URL in de API voor het zoeken van Azure de collectie van documenten in een index. Alle bewerkingen uitgevoerd op de verzameling zoals uploaden, samenvoegen, te verwijderen of query's uitvoeren documenten nemen plaats in de context van een enkele index, dus de URL's voor deze bewerkingen wordt altijd beginnen met `/indexes/[index name]/docs` voor de naam van een opgegeven index.

Uw toepassingscode moet ofwel genereren JSON documenten uploaden naar Azure zoeken of u kunt een [indexering](https://msdn.microsoft.com/library/dn946891.aspx) gebruiken om het laden van documenten als de gegevensbron Azure SQL-Database of DocumentDB. Meestal worden indexen ingevuld vanuit een één gegevensset die u opgeeft.

Het is raadzaam op beschikt over een document voor elk item dat u wilt zoeken. Een toepassing voor de huur film mogelijk één document per film, een toepassing voor de via één document per SKU mogelijk is, een toepassing online cursusmateriaal mogelijk één document per cursus, een bedrijf onderzoek mogelijk hebt u één document voor elke academische papier in hun opslagplaats, enzovoort.

Documenten bestaan uit een of meer velden. Velden kan de tekst die is ge? exeerd met Azure zoeken in zoektermen, evenals niet ge? exeerd of niet-tekstwaarden die kunnen worden gebruikt in filters of score profielen bevatten. De namen, gegevenstypen en ondersteunde zoekfuncties voor elk veld worden bepaald door het schema index. Een van de velden in elk schema index moet worden aangewezen als een ID en elk document moet een waarde voor het veld-ID die uniek dat document in de index hebben. Alle andere documentvelden zijn optioneel en een null-waarde wordt standaard als niet opgegeven. Houd er rekening mee dat null-waarden pas van vrijmaken in de zoekindex kracht worden.

Voordat u documenten uploaden kunt, moet hebt u al de index gemaakt op de service. Zie [Index maken](#CreateIndex) voor meer informatie over deze eerste stap.

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Toevoegen, bijwerken of verwijderen van documenten

U kunt uploaden, samenvoegen, samenvoegen of uploaden of verwijderen documenten van een opgegeven index met HTTP POST. Groot aantal updates, wordt batchen van documenten maximaal (1000 documenten per batch) of ongeveer 16 MB per batch aanbevolen.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. U kunt uploaden, samenvoegen, samenvoegen of uploaden of verwijderen documenten van een opgegeven index met HTTP POST.

Het verzoek URI bevat [indexnaam van de], opgeven welke index documenten posten. U kunt alleen documenten die u kunt één index boeken tegelijk.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Toevoegen documenten** moet bevatten een `api-key` koptekst ingesteld op uw sleutel beheerder (in plaats van een sleutel query).

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

De hoofdtekst van de aanvraag bevat een of meer documenten moeten worden geïndexeerd. Documenten worden aangeduid met een unieke sleutel. Elk document dat is gekoppeld aan een actie: upload, samenvoegen, mergeOrUpload of verwijderen. Upload-aanvragen, moeten de gegevens van het document bevatten als een set sleutel/waardeparen.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Document toetsen mogen alleen letters, cijfers, streepjes ("-"), onderstrepingstekens (_") en gelijk-tekens ("="). Zie [Naming regels](https://msdn.microsoft.com/library/azure/dn857353.aspx)voor meer informatie.

**Documentacties**

- `upload`: Een actie uploaden is vergelijkbaar met een "upsert" waar het document wordt ingevoegd als deze nieuwe en bijgewerkt/vervangen als deze bestaat. Houd er rekening mee dat alle velden in de zaak update worden vervangen.
- `merge`: Een bestaand document samenvoegen bijgewerkt met de opgegeven velden. Als het document niet bestaat, wordt de samenvoeging, mislukt. Elk veld dat u in een samenvoegbewerking opgeeft wordt vervangen door het bestaande veld in het document. Dit geldt ook voor velden van het type `Collection(Edm.String)`. Als het document bevat een veld "codes" met de waarde bijvoorbeeld `["budget"]` en u een samenvoegbewerking met waarde uitvoeren `["economy", "pool"]` voor "codes", de laatste waarde van het veld "codes" is `["economy", "pool"]`. Deze wordt **niet** worden `["budget", "economy", "pool"]`.
- `mergeOrUpload`: lijkt veel op `merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document nog niet bestaat lijkt veel op `upload` met een nieuw document.
- `delete`: Verwijderen Hiermee verwijdert u het opgegeven document in de index. Houd er rekening mee dat alle velden u opgeeft in een `delete` bewerking dan het veld sleutel worden genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `merge` in plaats daarvan en gewoon stelt u het veld expliciet tot `null`.

**Antwoord**

Statuscode 200 (OK) geretourneerd voor een succesvolle antwoord, wat betekent dat alle items is geïndexeerd. Hiermee wordt aangegeven door de `status` eigenschap wordt ingesteld op waar voor alle items, ook als het `statusCode` eigenschap wordt ingesteld op 201 (voor zojuist geüploade documenten) of 200 (voor samengevoegde of verwijderde documenten):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Statuscode 207 (meerdere Status) wordt geretourneerd als ten minste één item is niet lukt om via geïndexeerd. Items die niet zijn geïndexeerd hebben de `status` veld ingesteld op onwaar. De `errorMessage` en `statusCode` eigenschappen de reden voor de indexing fout wordt aangegeven:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

De volgende tabel beschrijft de verschillende statuscodes voor per document die kunnen worden geretourneerd in de reactie. Houd er rekening mee dat sommige duiden op problemen met het verzoek zelf, terwijl de anderen tijdelijke fouten aangeven. De laatste die u na een vertraging moet opnieuw.

<table style="font-size:12">
    <tr>
        <th>Statuscode</th>
        <th>Betekenis</th>
        <th>Herstelbare</th>
        <th>Notities</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Document is gewijzigd of verwijderd.</td>
        <td>n/b</td>
        <td>Verwijderingen zijn <a href="https://en.wikipedia.org/wiki/Idempotence">idempotency is ingeschakeld</a>. Dat wil zeggen, zelfs als de documentsleutel van een niet in de index bestaat, er de verwijderbewerking met die toets treedt een 200 statuscode.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Document is gemaakt.</td>
        <td>n/b</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Er is een fout in het document dat deze niet worden geïndexeerd.</td>
        <td>Nee</td>
        <td>Het foutbericht wordt weergegeven in het antwoord wordt aangegeven wat is er mis met het document.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Het document kan niet worden samengevoegd omdat de opgegeven sleutel niet in de index bestaat.</td>
        <td>Nee</td>
        <td>Deze fout treedt niet voor uploads, aangezien ze nieuwe documenten maken en niet voor verwijderen voorkomt omdat deze <a href="https://en.wikipedia.org/wiki/Idempotence">idempotency is ingeschakeld</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Een versieconflict is gedetecteerd tijdens een poging om het indexeren van een document.</td>
        <td>Ja</td>
        <td>Dit kan gebeuren wanneer u probeert te hetzelfde document meer dan één keer gelijktijdig indexeren.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>De index is tijdelijk niet beschikbaar omdat deze is bijgewerkt met de vlag 'allowIndexDowntime' is ingesteld op 'waar'.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>De zoekservice is tijdelijk niet beschikbaar is, mogelijk vanwege dik laden.</td>
        <td>Ja</td>
        <td>Uw code moet er wachten voordat de in dit geval opnieuw of u risico verlenging van de service niet beschikbaar.</td>
    </tr>
</table> 

**Opmerking**: als uw clientcode vaak een 207 antwoord aantreft, een mogelijke reden hiervoor is dat het systeem onder laden. U kunt dit controleren door te schakelen de `statusCode` eigenschap voor 503. Als dit het geval is, wordt u aangeraden ***indexing aanvragen beperken***. Anders als verkeer indexeren niet subside, het systeem kan worden gestart afkeuren van alle verzoeken met 503 fouten.

Statuscode 429 geeft aan dat u hebt uw quotum van het aantal documenten per index overschreden. U moet een nieuwe index maken of voor hoger Capaciteitslimieten bijwerken.

**Voorbeeld:**

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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Documenten zoeken

**Een zoekbewerking** is uitgegeven als een GET of POST-aanvraag en parameters die de criteria voor het selecteren van overeenkomende documenten geven.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer gebruikt u posten in plaats van ophalen**

Wanneer u de **zoekopdracht** API bellen HTTP GET hebt gebruikt, moet u weten dat niet langer zijn de lengte van de aanvraag-URL dan 8 KB. Dit is meestal genoeg voor de meeste toepassingen. Sommige toepassingen produceren echter zeer grote query's of OData-filterexpressies. Voor deze toepassingen is gebruiken HTTP POST een betere keus omdat hiermee groter filters en query's dan ophalen. Met het bericht is het aantal termen of componenten in een query de beperken factor, niet de grootte van de onbewerkte query sinds de limiet van de aanvraag voor het bericht 16 MB is.

> [AZURE.NOTE] Hoewel de limiet van de bericht-aanvraag zeer grote is, zoekquery's en filterexpressies kunnen niet willekeurig complexe. Zie [de syntaxis van de query Lucene](https://msdn.microsoft.com/library/mt589323.aspx) en [syntaxis van de OData-expressie](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over beperkingen complexiteit van zoeken query en filteren.

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. **De zoekopdracht** worden gemaakt volgens de methoden GET of POST.

Het verzoek URI geeft aan welke index op query voor alle documenten die overeenkomen met de parameters. Parameters zijn opgegeven in de query-tekenreeks in het geval van aanvragen voor het ophalen en in het verzoek hoofdtekst in het geval van een bericht vraagt.

Als een goede gewoonte bij het maken van GET-aanvragen, moet u [-URL-codering](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifieke queryparameters bij het aanroepen van de REST API rechtstreeks. Voor **zoekopdrachten** bevat dit:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-codering wordt alleen aanbevolen voor de bovenstaande queryparameters. Als u per ongeluk URL-coderen de hele queryreeks (alles na de?), aanvragen worden afgebroken.

Zo is de URL-codering ook alleen nodig bij het aanroepen van de REST API rechtstreeks met ophalen. Geen URL-codering is nodig bij het aanroepen van **Zoeken** op basis van bericht of bij gebruik van de [bibliotheek voor .NET-client](https://msdn.microsoft.com/library/dn951165.aspx)zoals omgaat met URL-codering voor u.

<a name="SearchQueryParameters"></a>
**Queryparameters**

**Zoeken** accepteert verschillende parameters die query's met criteria opgeven en de werking van de zoekopdracht ook opgeven. U vindt dat deze parameters in de URL-queryreeks bij het aanroepen van **Zoeken** via Ontvang en als JSON eigenschappen in het hoofdgedeelte van de aanvraag bij het aanroepen van **Zoeken** via de POST. De syntaxis voor sommige parameters verschilt enigszins tussen GET en POST. Deze verschillen worden aangegeven met toepasselijke hieronder:

`search=[string]`(optioneel) - de tekst wilt zoeken. Alle `searchable` velden worden doorzocht al dan niet standaard tenzij `searchFields` is opgegeven. Bij het zoeken naar `searchable` velden, de zoektekst zelf is ge? exeerd, zodat meerdere voorwaarden kunnen worden gescheiden door een spatie (bijvoorbeeld: `search=hello world`). Gebruik zodat deze overeenkomen met een term, `*` (dit is handig voor Booleaanse filter query's). Deze parameter weglaten heeft hetzelfde resultaat als de eigenschap instelt op `*`. Zie [Eenvoudige Query-syntaxis](https://msdn.microsoft.com/library/dn798920.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht.

  - **Opmerking**: de resultaten zijn soms verrassend wanneer de query's uitvoeren via `searchable` velden. De Basisvormherkenning bevat logica voor de afhandeling van zaken die beschikbaar is in de Engelse tekst zoals apostrofs, komma's getallen, enzovoort. Bijvoorbeeld `search=123,456` komen overeen met één term 123,456 in plaats van de afzonderlijke termen 123 en 456, aangezien komma's worden gebruikt als duizend-scheidingstekens voor grote aantallen in het Engels. Daarom wordt aangeraden met een leeg gedeelte in plaats van leestekens te scheiden van termen in de `search` parameter.

`searchMode=any|all`(optioneel, standaard `any`) - of een of meer van de zoektermen moet worden overeengestemd om te tellen van het document als een van de zoekwaarde.

`searchFields=[string]`(optioneel) - de lijst met door komma's gescheiden veldnamen om te zoeken naar de opgegeven tekst. Doelvelden moeten worden gemarkeerd als `searchable`.

`queryType=simple|full`(optioneel, standaard `simple`) - wanneer ingesteld op "eenvoudige" zoektekst geïnterpreteerd met gebruikmaking van een eenvoudige querytaal waarmee symbolen, zoals +, * en "". Query's worden geëvalueerd voor alle doorzoekbare velden (of velden, zoals aangegeven in `searchFields`) in elk document al dan niet standaard. Als het querytype is ingesteld op `full` zoeken in tekst met de querytaal Lucene waarmee zoekopdrachten in het veld / regiospecifieke en gewogen beschouwd. Zie [Eenvoudige syntaxis van de Query](https://msdn.microsoft.com/library/dn798920.aspx) en [Syntaxis van de Query Lucene](https://msdn.microsoft.com/library/mt589323.aspx) voor specifieke informatie over de syntaxis van de zoekopdracht. 
 
> [AZURE.NOTE] Zoeken in de querytaal wordt niet ondersteund door $filter die vergelijkbare functionaliteit biedt Lucene variëren.

`moreLikeThis=[key]`(optioneel) **Belangrijke:** Deze functie is alleen beschikbaar in `2015-02-28-Preview`. Deze optie kan niet worden gebruikt in een query met de parameter text zoeken `search=[string]`. De `moreLikeThis` parameter vindt u documenten die vergelijkbaar met het document dat is opgegeven door de document-toets zijn. Wanneer een zoekopdracht wordt aangebracht met `moreLikeThis`, een lijst met zoektermen wordt gegenereerd op basis van de frequentie en zeldzaamheid van termen in het brondocument. Deze voorwaarden worden gebruikt voor het maken van het verzoek. Standaard is de inhoud van alle `searchable` velden worden beschouwd als tenzij `searchFields` wordt gebruikt om te beperken welke velden worden doorzocht.  

`$skip=#`(optioneel) - het aantal zoekresultaten overgeslagen; Mogen niet groter is dan 100.000. Als u nodig hebt om documenten te scannen in volgorde, maar niet gebruiken `$skip` vanwege deze beperking, kunt u overwegen `$orderby` op een toets volledig besteld en `$filter` query in plaats daarvan met een bereik.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `skip` in plaats van `$skip`.

`$top=#`(optioneel) - het aantal zoekresultaten om op te halen. Dit kan worden gebruikt in combinatie met `$skip` willen implementeren aan de clientzijde paginering van lijst met zoekresultaten.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `top` in plaats van `$top`.

`$count=true|false`(optioneel, standaard `false`)-Hiermee bepaalt u of het totale aantal resultaten ophalen. Dit is de telling van alle documenten die overeenkomen met de `search` en `$filter` parameters, worden genegeerd `$top` en `$skip`. Als deze waarde op `true` mogelijk van invloed op de prestaties. Houd er rekening mee dat het aantal geretourneerde een schatting is.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `count` in plaats van `$count`.

`$orderby=[string]`(optioneel) - een lijst met door komma's gescheiden expressies om te sorteren van de resultaten op. Elke expressie kunt werken met de naam van een veld of een oproep door naar de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` naar aangegeven oplopend, en `desc` om aan te geven aflopend. De standaardinstelling is oplopende volgorde. Ties wordt de scores vergelijken van documenten worden opgesplitst. Als er geen `$orderby` is opgegeven, wordt de standaardsorteervolgorde is Aflopend op document vergelijken score. Er is een limiet van 32 componenten voor `$orderby`.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `orderby` in plaats van `$orderby`.

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden om op te halen. Als u niets opgeeft, wordt alle velden die zijn gemarkeerd als opvraagbare in het schema zijn opgenomen. U kunt alle velden ook expliciet aanvragen als deze parameter `*`.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `select` in plaats van `$select`.

`facet=[string]`(nul of meer) - een veld toe aan facet door. U kunt ook de tekenreeks parameters om aan te passen de faceting uitgedrukt als door komma's gescheiden kan bevatten `name:value` paren. Ongeldige parameters zijn:

- `count`(maximaal aantal facet termen; standaard is 10). Er is geen maximum, maar hogere waarden oplopen een bijbehorende op de prestaties, vooral als het beperkte veld een groot aantal unieke termen bevat.
  - Bijvoorbeeld: `facet=category,count:5` ontvangt van de vijf belangrijkste categorieën in facet resultaten.  
  - **Opmerking**: als de `count` parameter kleiner is dan het aantal unieke termen, de resultaten mogelijk niet nauwkeurig. Dit is vanwege de manier waarop faceting query's zijn verdeeld over shards. Met groter wordende `count` doorgaans verbetert de nauwkeurigheid van de term-tellingen komen, maar met kosten berekend prestaties.
- `sort`(een van `count` om te sorteren *Aflopend* op aantal, `-count` om te sorteren *Oplopend* op aantal, `value` om te sorteren *Oplopend* op waarde, of `-value` om te sorteren *Aflopend* op waarde)
  - Bijvoorbeeld: `facet=category,count:3,sort:count` ontvangt van de drie belangrijkste categorieën in facet resultaten in aflopende volgorde door het aantal documenten met de naam van elke stad. Bijvoorbeeld als de bovenste drie categorieën Budget, Motel en sportauto zijn en Budget 5 treffers heeft, Motel 6 heeft en luxe 4 heeft, zijn klikt u vervolgens de gerangschikte verzamelingen in de volgorde Motel, Budget, luxe.
  - Bijvoorbeeld: `facet=rating,sort:-value` gerangschikte verzamelingen voor alle mogelijke classificaties, in aflopende volgorde door waarde oplevert. Bijvoorbeeld als de classificaties van 1 tot en met 5, worden de gerangschikte verzamelingen besteld 5, 4, 3, 2, 1, ongeacht het aantal documenten overeenkomen met elke beoordeling.
- `values`(numerieke recht streepje scheidingsteken of `Edm.DateTimeOffset` waarden die aangeven dat een dynamische verzameling facet vermelding waarden)
  - Bijvoorbeeld: `facet=baseRate,values:10|20` drie gerangschikte verzamelingen oplevert: één voor grondtal tarief van 0 tot, maar niet maximaal inclusief 10, één voor 10, maar niet met inbegrip van 20, en één voor 20 of hoger.
  - Bijvoorbeeld: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` twee gerangschikte verzamelingen oplevert: één voor hotels renovated vóór februari 2010 en één voor hotels renovated februari 1e, 2010 of hoger.
- `interval`(geheel getal interval groter dan 0 voor getallen, of `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` voor datumwaarden tijd)
  - Bijvoorbeeld: `facet=baseRate,interval:100` gerangschikte verzamelingen op basis van bereiken rente op basis van grootte 100 oplevert. Bijvoorbeeld als basis tarieven alle tussen $60 en $600 zijn, wordt er gerangschikte verzamelingen voor 0-100, 100-200, 200-300 300-400, 400-500 en 500-600.
  - Bijvoorbeeld: `facet=lastRenovationDate,interval:year` één Emmertje voor elk jaar wanneer hotels zijn renovated oplevert.
- `timeoffset`([+-] uu: mm, [+-] hhmm, of [+-] [hh) `timeoffset` is optioneel. Kan alleen worden gecombineerd met de `interval` optie, waarbij alleen toegepast op een veld van het type `Edm.DateTimeOffset`. De waarde geeft de UTC-tijd in tijd in account voor bij het instellen van de grenzen van de tijd.
  - Bijvoorbeeld: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` gebruikmaakt van de rand van de dag die begint op UTC 01:00:00 (middernacht in de doel-tijdzone)
- **Opmerking**: `count` en `sort` kunnen worden gecombineerd in de dezelfde facet-specificatie, maar ze kunnen niet worden gecombineerd met `interval` of `values`, en `interval` en `values` samen kan niet worden gecombineerd.
- **Opmerking**: Interval aspecten op datum tijd worden berekend op basis van UTC-tijd als `timeoffset` niet is opgegeven. Bijvoorbeeld: voor `facet=lastRenovationDate,interval:day`, de grens op 00:00:00 UTC begint dag. 

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `facets` in plaats van `facet`. Ook, geeft u deze als een matrix JSON van tekenreeksen waar elke tekenreeks een afzonderlijke facet-expressie is.

`$filter=[string]`(optioneel) - een gestructureerde zoekexpressie in standaardsyntaxis OData. Zie [De syntaxis van de OData-expressie](#ODataExpressionSyntax) voor meer informatie over de subset van de OData-expressie grammatica die ondersteuning biedt voor Azure zoeken.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `filter` in plaats van `$filter`.

`highlight=[string]`(optioneel) - een set met door komma's gescheiden veldnamen die wordt gebruikt voor treffer worden gemarkeerd. Alleen `searchable` velden kunnen worden gebruikt voor treffers markeren.

`highlightPreTag=[string]`(optioneel, standaard `<em>`) - A-code die voegt toe om aan te raken belangrijkste punten van de tekenreeks. Moeten worden ingesteld met `highlightPostTag`.

> [AZURE.NOTE] Wanneer bellen **Zoeken** met de ophalen, gereserveerde tekens in de URL moet worden percentage-codering (bijvoorbeeld % 23 in plaats van #).

`highlightPostTag=[string]`(optioneel, standaard `</em>`)-een tag tekenreeks die wordt toegevoegd om uitschieters raken. Moeten worden ingesteld met `highlightPreTag`.

> [AZURE.NOTE] Wanneer bellen **Zoeken** met de ophalen, gereserveerde tekens in de URL moet worden percentage-codering (bijvoorbeeld % 23 in plaats van #).

`scoringProfile=[string]`(optioneel) - de naam van een score profiel moet worden geëvalueerd overeenkomen met scores voor overeenkomende documenten om te kunnen de resultaten sorteren.

`scoringParameter=[string]`(nul of meer) - geeft aan dat de waarden voor elke parameter die is gedefinieerd in een functie score (bijvoorbeeld `referencePointParameter`) met de opmaak `name-value1,value2,...`.

- Bijvoorbeeld, als het score profiel een functie met een parameter met de naam 'mylocation definieert' de optie query tekenreeks zou `&scoringParameter=mylocation--122.2,44.8`. Het eerste streepje scheidt u de naam van de lijst met waarden, terwijl het tweede streepje deel uit van de eerste waarde (lengtegraad in dit voorbeeld maakt).
- Score parameters kunt u geen deze waarden in de lijst met enkele aanhalingstekens escape zoals voor de tag die verhogen kunt bevatten komma's. Als de waarden zelf enkele aanhalingstekens bevatten, kunt u ze escape verdubbeld.
  - Bijvoorbeeld als u een tag verhogen parameter met de naam 'mytag' hebt en u wilt vergroten op de tag waarden 'Hallo, O'Brien' en 'Smit', de query de optie tekenreeks zou `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Offertes zijn alleen vereist voor waarden met komma's.

> [AZURE.NOTE] Wanneer u belt **Zoeken** met de posten, deze parameter heet `scoringParameters` in plaats van `scoringParameter`. U ook als een matrix JSON van tekenreeksen waar elke tekenreeks een afzonderlijke is `name-values` paar gegevenspunten.

`minimumCoverage`(optioneel is, wordt standaard ingesteld op 100)-een getal tussen 0 en 100 waarin wordt aangegeven dat het percentage van de index die moet worden bedekt door een zoekopdracht in volgorde voor de query moet worden gerapporteerd als een gunstige uitkomst. Standaard de volledige index beschikbaar moet zijn of `Search` HTTP-statuscode 503 zullen retourneren. Als u `minimumCoverage` en `Search` is geslaagd, deze retourneert HTTP 200 en bevatten een `@search.coverage` waarde in het antwoord waarin wordt aangegeven dat het percentage van de index die is opgenomen in de query.

> [AZURE.NOTE] Als deze parameter een waarde lager dan 100 zijn handig voor zorgen dat de beschikbaarheid van de zoekopdracht zelfs voor services met slechts één replica. Niet alle overeenkomstige documenten zijn echter gegarandeerd aanwezig zijn in de lijst met zoekresultaten. Als zoeken intrekken belangrijker in uw toepassing dan beschikbaarheid is en laat u het beste `minimumCoverage` bij de standaardwaarde van 100.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter in de URL ongeacht of u **Zoeken** met GET of POST belt.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor de URL van uw service. **De zoekopdracht** kunt opgeven voor een beheerder-toets of de query-sleutel voor `api-key`.

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Voor ophalen: geen.

Voor bericht:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Vervolg van automatische zoekfunctie antwoorden**

Azure zoeken kan soms de gevraagde resultaten retourneren in één zoeken antwoord. Dit kan gebeuren om verschillende redenen, zoals wanneer de query te veel documenten aanvraagt door het opgeven van niet `$top` of een waarde opgeeft voor `$top` die te groot is. In dat geval Azure zoeken bevat de `@odata.nextLink` aantekeningen in de hoofdtekst van het antwoord en ook `@search.nextPageParameters` als het een POST-aanvraag is. U kunt de waarden van deze aantekeningen aan het opstellen van een andere zoekopdracht om het volgende gedeelte van het antwoord zoeken. Heet dit ***voortgezet*** van de oorspronkelijke zoeken-aanvraag en de aantekeningen worden ***voortgezet tokens***genoemd. Zie [het volgende voorbeeld](#SearchResponse) voor meer informatie over de syntaxis van deze aantekeningen en waar deze worden weergegeven in de hoofdtekst van het antwoord. 

De redenen waarom Azure zoeken voortgezet tokens mogelijk retourneren zijn implementatiespecifieke en kunnen worden gewijzigd. Robuuste clients moet altijd klaar worden afgehandeld wanneer minder documenten dan verwacht worden geretourneerd en een token voortgezet is opgenomen kunt doorgaan met het ophalen van documenten. Houd er ook rekening mee dat u dezelfde HTTP methode als de oorspronkelijke aanvraag gebruiken moet om door te gaan. Bijvoorbeeld als u een GET-aanvraag hebt verzonden, die u verzendt voortgezet verzoeken moeten ook gebruiken GET (en hetzelfde geldt voor bericht).

<a name="SearchResponse"></a>
**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Voorbeelden:**

Klik op de pagina voor de [Syntaxis van de OData-expressie voor zoekprogramma's Azure](https://msdn.microsoft.com/library/azure/dn798921.aspx) vindt u meer voorbeelden.

1)  Zoeken de Index gesorteerd in aflopende volgorde op datum.


    GET-/indexes/hotels/docs? zoeken = * & $orderby = lastRenovationDate desc & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "*", "orderby": "lastRenovationDate desc"}

2)  In een beperkte zoeken, zoekt u de index en aspecten voor categorieën, de classificatie, labels, evenals items met baseRate in specifieke bereiken ophalen:


    GET-/indexes/hotels/docs? zoeken = test & facet = categorie & facet = classificatie & facet = labels en facet = baseRate, waarden: 80 | 150 | 220 & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "testen", "aspecten": ["categorie", "classificatie", "codes", "baseRate, waarden: 80 | 150 | 220"]}

3)  De queryresultaten van de vorige beperkte met behulp van een filter beperken nadat de gebruiker klikt op beoordeling 3 en categorie "Motel":


    GET-/indexes/hotels/docs? zoeken = test & facet = labels en facet = baseRate, waarden: 80 | 150 | 220 & $filter = classificatie n, 3 en categorie eq 'Motel' & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie = 2015-02-28-voorbeeld {'search': "testen", "aspecten": ["codes", "baseRate, waarden: 80 | 150 | 220"], "filter": "classificatie eq 3 en categorie eq 'Motel'"}

4) Stel in een beperkte zoekopdracht, een bovengrens van unieke termen in een query geretourneerd. De standaardwaarde is 10, maar u kunt vergroot of verkleint deze waarde nu de `count` parameter op de `facet` kenmerk:


    GET-/indexes/hotels/docs? zoeken = test & facet = city, tellen: 5 & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "testen", "aspecten": ["city, tellen: 5"]}

5)  Zoek in de Index in bepaalde velden; Bijvoorbeeld: een veld taalspecifieke:


    GET-/indexes/hotels/docs? zoeken = hôtel & searchFields = description_fr & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "hôtel", "searchFields": "description_fr"}

6) De Index doorzoeken op meerdere velden. U kunt bijvoorbeeld opslaan en doorzoekbare queryvelden in meerdere talen, allemaal in dezelfde index.  Als Engels en Frans beschrijvingen in hetzelfde document naast elkaar bestaan, kunt u een of meer in de queryresultaten terugkeren:


    GET-/indexes/hotels/docs? zoeken = hotel & searchFields = beschrijving, description_fr & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "hotel", "searchFields": "beschrijving, description_fr"}

Houd er rekening mee dat u kunt alleen een query één index tegelijk. Maak geen meerdere indexen voor elke taal tenzij u van plan bent om query's in één voor één.

7)  Paginering - ophalen van de 1e pagina van items (paginaformaat is 10):


    GET-/indexes/hotels/docs? zoeken = * & $skip = 0 & $top = 10 & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "*", "overslaan": 0, "boven": 10}

8)  Paginering - ophalen van de 2e pagina van items (paginaformaat is 10):


    GET-/indexes/hotels/docs? zoeken = * & $skip = 10 & $top = 10 & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "*", "overslaan": 10, "boven": 10}

9)  Een specifieke reeks velden ophalen:


    GET-/indexes/hotels/docs? zoeken = * & $select = hotelName, beschrijving en api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "*", "select": "hotelName, beschrijving"}

10)  Documenten die overeenkomen met een specifieke filterexpressie ophalen:


    GET-/indexes/hotels/docs? $filter = (baseRate ge 60 en baseRate lt 300) of de hotelName-n "spannende" blijven- & de api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {"filter": "(baseRate ge 60 en baseRate lt 300) of hotelName eq '" spannende "blijven'"}

11) De index en return fragmenten met belangrijkste punten van treffers zoeken


    GET-/indexes/hotels/docs? zoeken = iets & markeren = beschrijving & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "iets", "markeren": "Beschrijving"}

12) Zoek in de index en documenten van dichter bij verder weg van een verwijzing locatie gesorteerd terug te keren


    GET-/indexes/hotels/docs? zoeken = iets & $orderby=geo.distance (locatie, geography'POINT(-122.12315 47.88121)') & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "iets", "orderby": "geo.distance (locatie, geography'POINT(-122.12315 47.88121)')"}

13) Zoek in de index ervan uitgaande dat er een score profiel 'geografische' genoemd met twee afstand score functies, één definiëren als u een parameter genoemd 'currentLocation' en een definiëren van een parameter met de naam "lastLocation"


    GET-/indexes/hotels/docs? zoeken = iets & scoringProfile = geografische & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "iets", "scoringProfile": "geografische", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Documenten zoeken in de index met [de syntaxis van de eenvoudige query](https://msdn.microsoft.com/library/dn798920.aspx). Deze query retourneert hotels waar doorzoekbare velden de termen "comfort" en "locatie" maar niet 'motel bevatten':


    GET-/indexes/hotels/docs? zoeken = comfort + locatie-motel & searchMode = all & api-versie = 2015-02-28-voorbeeld

    BERICHT /indexes/hotels/docs/search? api-versie 2015-02-28-Preview = {'search': "comfort + locatie-motel", "searchMode": "alle"}

Houd rekening met het gebruik van `searchMode=all` hierboven. Deze parameter inclusief overschrijft de standaard van `searchMode=any`, ervoor zorgen dat die `-motel` betekent "AND NOT" in plaats van "Of NOT". Zonder `searchMode=all`, krijgt u "Of NOT" waarin wordt uitgebreid met die in plaats van zoekresultaten beperkt en dit is erg intuïtief aan enkele gebruikers.

15) Documenten zoeken in de index [lucene query-syntaxis](https://msdn.microsoft.com/library/mt589323.aspx)gebruiken. Deze query retourneert hotels waarvan het categorieveld de term "budget" bevat en alle doorzoekbare velden met de woordgroep "onlangs renovated". Documenten met de woordgroep "onlangs renovated" krijgen een hogere rang grond van de term Microfoonversterking waarde (3)

    GET-/indexes/hotels/docs?search = categorie: budget en \"onlangs renovated\"^ 3 & searchMode = all & api-versie 2015-02-28-Preview = & querytype = volledige

    BERICHT /indexes/hotels/docs/search?api-version 2015-02-28-Preview = {'search': "categorie: budget en \"onlangs renovated\"^ 3", "queryType": "volledige", "searchMode": "alle"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Lookup-Document

Een document met de bewerking **Opzoeken Document** opgehaald uit Azure zoeken. Dit is handig wanneer een gebruiker op een specifieke zoekresultaat klikt, en u wilt opzoeken specifieke details over dat document.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. Het verzoek **Opzoeken Document** kan als volgt worden samengesteld.

    GET /indexes/[index name]/docs/key?[query parameters]

U kunt ook de syntaxis van de OData-traditioneel gebruiken voor het opzoeken van belangrijke:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Het verzoek URI bevat een [indexnaam van] en [key], die aangeeft welk document om op te halen uit welke index. U kunt slechts één document tegelijkertijd krijgen. Gebruik de **zoekfunctie** om meerdere documenten in een enkele aanvraag.

**Queryparameters**

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden om op te halen. Als niet-opgegeven of ingesteld op `*`, alle velden die zijn gemarkeerd als opvraagbare in het schema zijn opgenomen in de raming.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor de URL van uw service. Het verzoek **Opzoeken Document** kunt opgeven een beheerder-toets of de query-sleutel voor `api-key`.

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Voorbeeld**

Zoek het document dat sleutel '2' heeft

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Zoek het document dat sleutel '3' met de syntaxis van de OData-heeft:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Aantal documenten

Het **Aantal documenten** betrekking heeft een telling van het aantal documenten in een zoekindex opgehaald. De `$count` syntaxis maakt deel uit van de OData-protocol.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. Het **Aantal documenten** -verzoek kan worden samengesteld met de methode GET.

[Naam van de index] in het verzoek URI Hiermee wordt aan de service om terug te keren een telling van alle items in de verzameling documenten van de opgegeven index.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen.

- `Accept`: Met deze waarde moet zijn ingesteld op `text/plain`.
- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor de URL van uw service. Het verzoek **Aantal documenten** kunt opgeven een beheerder-toets of de query-sleutel voor `api-key`.

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Geen.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

De hoofdtekst van het antwoord bevat de waarde tellen als geheel opgemaakt als tekst zonder opmaak.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Suggesties

De bewerking **suggesties** opgehaald suggesties op basis van gedeeltelijke zoeken invoer. Dit wordt meestal gebruikt in de zoekvakken naar tekst aanvullen suggesties bieden, zoals gebruikers zoektermen invoeren.

Suggestie aanvragen gericht op de suggesties voor documenten van de doelsite, zodat de voorgestelde tekst kan worden herhaald als meerdere candidate documenten overeenkomen met dezelfde zoekopdracht invoer. U kunt `$select` om op te halen andere documentvelden (inclusief de toets document) zodat u weet welke document fungeert als bron voor elke suggestie.

Een bewerking **suggesties** wordt uitgegeven als een aanvraag GET of POST.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Wanneer gebruikt u posten in plaats van ophalen**

Wanneer u een HTTP GET om te bellen van de **suggesties** API gebruikt, moet u weten dat niet langer zijn de lengte van de aanvraag-URL dan 8 KB. Dit is meestal genoeg voor de meeste toepassingen. Sommige toepassingen produceren echter zeer grote query's, specifiek OData filterexpressies. Voor deze toepassingen is gebruiken HTTP POST een betere keus omdat hiermee groter filters dan ophalen. Met het bericht is het aantal clausules van een filter de beperken factor, niet de grootte van de filtertekenreeks onbewerkte sinds de limiet van de aanvraag voor het bericht 16 MB is.

> [AZURE.NOTE] Hoewel de maximale grootte van POST-verzoek erg groot is, mogen filterexpressies willekeurig complexe niet. Zie [de syntaxis van de OData-expressie](https://msdn.microsoft.com/library/dn798921.aspx) voor meer informatie over filteren complexiteit beperkingen.

**Aanvragen**

HTTPS is vereist voor serviceaanvragen. Het verzoek **suggesties** worden gemaakt volgens de methoden GET of POST.

Het verzoek URI bevat de naam van de index op query. Parameters, zoals gedeeltelijk invoer zoekterm, zijn opgegeven in de query-tekenreeks in het geval van aanvragen voor het ophalen en in het verzoek hoofdtekst in het geval van een bericht vraagt.

Als een goede gewoonte bij het maken van GET-aanvragen, moet u [-URL-codering](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifieke queryparameters bij het aanroepen van de REST API rechtstreeks. Dit omvat voor **suggesties** bewerkingen uitvoeren:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-codering wordt alleen aanbevolen voor de bovenstaande queryparameters. Als u per ongeluk URL-coderen de hele queryreeks (alles na de?), aanvragen worden afgebroken.

Zo is de URL-codering ook alleen nodig bij het aanroepen van de REST API rechtstreeks met ophalen. Geen URL-codering is nodig bij het aanroepen van **suggesties** met bericht of bij gebruik van de [bibliotheek voor .NET-client](https://msdn.microsoft.com/library/dn951165.aspx)zoals omgaat met URL-codering voor u.

**Queryparameters**

**Suggesties** accepteert verschillende parameters die query's met criteria opgeven en de werking van de zoekopdracht ook opgeven. U vindt dat deze parameters in de URL-queryreeks bij het aanroepen van **suggesties** via Ontvang en als JSON eigenschappen in het hoofdgedeelte van de aanvraag wanneer u **suggesties** via de POST belt. De syntaxis voor sommige parameters verschilt enigszins tussen GET en POST. Deze verschillen worden aangegeven met toepasselijke hieronder:

`search=[string]`-de zoektekst met suggestie voor query's. Moet ten minste 1 teken, en niet meer dan 100 tekens.

`highlightPreTag=[string]`(optioneel) - een tekenreeks markeren die voegt toe als u wilt zoeken treffers. Moeten worden ingesteld met `highlightPostTag`.

> [AZURE.NOTE] Wanneer bellen **suggesties** met ophalen, gereserveerde tekens in de URL moet worden percentage-codering (bijvoorbeeld % 23 in plaats van #).

`highlightPostTag=[string]`(optioneel) - een tekenreeks markeren die worden toegevoegd als u wilt zoeken treffers. Moeten worden ingesteld met `highlightPreTag`.

> [AZURE.NOTE] Wanneer bellen **suggesties** met ophalen, gereserveerde tekens in de URL moet worden percentage-codering (bijvoorbeeld % 23 in plaats van #).

`suggesterName=[string]`-de naam van de suggester zoals opgegeven in de `suggesters` siteverzameling die deel uitmaakt van de definitie van de index. A `suggester` bepaalt welke velden worden gescand op voorgestelde querytermen. Zie [Suggesters](#Suggesters) voor meer informatie.

`fuzzy=[boolean]`(optioneel, standaard = ONWAAR)-toen ingesteld op waar deze API vindt u suggesties zelfs als er een teken vervangen of ontbrekende in het Zoektekstvak. Het wordt geleverd bij een prestaties omdat bij benadering suggestie zoekopdrachten trager en meer informatiebronnen gebruiken terwijl u Hiermee beschikt u over een betere ervaring in sommige gevallen.

`searchFields=[string]`(optioneel) - de lijst met door komma's gescheiden veldnamen om te zoeken naar de opgegeven zoektekst. Doelvelden moeten zijn ingeschakeld voor suggesties.

`$top=#`(optioneel, standaard = 5)-het aantal suggesties om op te halen. Moet u een getal tussen 1 en 100.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** bericht met deze parameter heet `top` in plaats van `$top`.

`$filter=[string]`(optioneel) - een expressie die de documenten worden gefilterd in aanmerking voor suggesties.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** bericht met deze parameter heet `filter` in plaats van `$filter`.

`$orderby=[string]`(optioneel) - een lijst met door komma's gescheiden expressies om te sorteren van de resultaten op. Elke expressie kunt werken met de naam van een veld of een oproep door naar de `geo.distance()` functie. Elke expressie kan worden gevolgd door `asc` naar aangegeven oplopend, en `desc` om aan te geven aflopend. De standaardinstelling is oplopende volgorde. Er is een limiet van 32 componenten voor `$orderby`.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** bericht met deze parameter heet `orderby` in plaats van `$orderby`.

`$select=[string]`(optioneel) - een lijst met door komma's gescheiden velden om op te halen. Als u niets opgeeft, wordt alleen de sleutel en suggestie documenttekst geretourneerd. U kunt alle velden expliciet aanvragen als deze parameter `*`.

> [AZURE.NOTE] Bij het aanroepen van **suggesties** bericht met deze parameter heet `select` in plaats van `$select`.

`minimumCoverage`(optioneel is, wordt standaard ingesteld op 80)-een getal tussen 0 en 100 waarin wordt aangegeven dat het percentage van de index die moet worden bedekt door een query suggesties om de query moet worden gerapporteerd als een gunstige uitkomst. Standaard moet ten minste 80% van de index beschikbaar zijn of `Suggest` HTTP-statuscode 503 zullen retourneren. Als u `minimumCoverage` en `Suggest` is geslaagd, deze retourneert HTTP 200 en bevatten een `@search.coverage` waarde in het antwoord waarin wordt aangegeven dat het percentage van de index die is opgenomen in de query.

> [AZURE.NOTE] Als deze parameter een waarde lager dan 100 zijn handig voor zorgen dat de beschikbaarheid van de zoekopdracht zelfs voor services met slechts één replica. Niet alle overeenkomstige suggesties zijn echter gegarandeerd aanwezig zijn in de resultaten. Als intrekken belangrijker in uw toepassing dan beschikbaarheid is, zal het beste niet Verlaag `minimumCoverage` onder de standaardwaarde van 80.

`api-version=[string]`(vereist). De preview-versie is `api-version=2015-02-28-Preview`. Zie [Zoeken Service versiebeheer](http://msdn.microsoft.com/library/azure/dn864560.aspx) voor meer informatie en alternatieve versies.

Opmerking: voor deze bewerking, de `api-version` is opgegeven als een queryparameter in de URL ongeacht of u **suggesties** met GET of POST belt.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen

- `api-key`: De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor de URL van uw service. Het verzoek **suggesties** kunt opgeven een beheerder-toets of de query-toets als de `api-key`.

U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U krijgt de servicenaam van de en `api-key` vanuit uw servicedashboard in de Portal Azure. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

**Hoofdtekst aanvragen**

Voor ophalen: geen.

Voor bericht:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Als de optie raming wordt gebruikt om op te halen velden die ze van elk element van de matrix uitmaken deel:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Voorbeeld**

5 suggesties waar de automatische zoekfunctie invoer 'lux' is ophalen

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
