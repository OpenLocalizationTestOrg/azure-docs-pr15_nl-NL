<properties
    pageTitle="Scoren profielen (Azure zoeken REST API-versie 2015-02-28-Preview) | Microsoft Azure | Voorbeeld van Azure Search API"
    description="Azure tijdens het zoeken is een gehoste cloud-zoekservice die ondersteuning biedt voor optimaliseren van gerangschikte resultaten op basis van de gebruiker gedefinieerde score profielen."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Score profielen (Azure zoeken REST API-versie 2015-02-28-Preview)

> [AZURE.NOTE] In dit artikel worden de score profielen in de [2015-02-28-Preview](search-api-2015-02-28-preview.md). Er is momenteel geen verschil is tussen de `2015-02-28` die op [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) en de `2015-02-28-Preview` versie die hier worden beschreven, maar we dit document toch kan alleen worden aangeboden documentdekking over de hele API bieden.

## <a name="overview"></a>Overzicht

Scoren verwijst naar de berekening van een score zoeken voor elk item in lijst met zoekresultaten geretourneerd. De score is een aanduiding van belang zijn van een item in de context van de huidige zoekbewerking. Hoe hoger score, de nog relevanter het item. Items zijn in de zoekresultaten rang gerangschikt van hoog naar laag, op basis van de zoeken-score berekend voor elk item.

Azure zoeken standaard score moet berekenen van een eerste score gebruikt, maar u kunt de berekening door een score profiel aanpassen. Score profielen hebt u meer controle over de classificatie van items in de zoekresultaten. U wilt bijvoorbeeld items op basis van hun potentiële omzet verhogen, nieuwere objecten promoten of misschien verhogen items die in de voorraad te lang zijn.

Een score profiel maakt deel uit van de definitie van de index, op basis van velden, functies en parameters.

Zodat u een idee van hoe een score profiel eruitziet, ziet in het volgende voorbeeld u een eenvoudige profiel met de naam 'geografische'. Dit item verhoogt de items die u hebt de zoekterm in het `hotelName` veld. Wordt ook gebruikt de `distance` functie voorkeur krijgen voor items die zich bevindt in tien kilometer van de huidige locatie. Als iemand op de term 'inn zoekt' en 'inn' gebeurt er wilt toevoegen aan de naam van de hotel, weergegeven documenten die hotels met 'inn' bevatten hoger in de lijst met zoekresultaten.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Als u wilt gebruiken dit score profiel, wordt uw query geformuleerd als u wilt opgeven van het profiel op de query-tekenreeks. In de onderstaande query, ziet u de queryparameter `scoringProfile=geo` in het verzoek.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Deze query zoekt op de term 'inn' en geeft in de huidige locatie. Opmerking dat deze query andere parameters, zoals bevat `scoringParameter`. Queryparameters worden beschreven in de [Zoeken-documenten (Azure zoeken API)](search-api-2015-02-28-preview.md#SearchDocs).

Klik op [voorbeeld](#example) als u wilt bekijken van een uitgebreider voorbeeld van een score profiel.

## <a name="what-is-default-scoring"></a>Wat is standaard scoren?

Scoren berekent een score zoeken voor elk item in een rang geordende resultatenset. Elk item in een resultatenset zoeken is toegewezen een score zoeken en klik vervolgens rangschikking hoog naar laag. Items met de hogere scores worden geretourneerd met de toepassing. Standaard de bovenste 50 worden geretourneerd, maar u kunt de `$top` parameter om terug te keren een kleiner of groter aantal items (maximaal 1000 in één antwoord).

Standaard is een score zoeken berekend op basis van statistische eigenschappen van de gegevens en de query. Azure zoeken vindt u documenten die de zoektermen in de query-tekenreeks opnemen (sommige of alle, afhankelijk van `searchMode`), voorkeur documenten die veel exemplaren van de zoekterm bevatten. De zoeken-score bovenliggende zelfs hoger als de term niet vaak voorkomen in de corpus gegevens, maar algemene binnen het document is. De basis voor deze methode ter computing relevantie heet TF-IDF of (term frequentie-inverse document frequentie).

Ervan uitgaande dat er geen aangepast sorteren, worden resultaten vervolgens geklasseerd op Zoeken score voordat ze worden geretourneerd naar de bellen-toepassing. Als `$top` niet is opgegeven, 50 items met de hoogste zoekopdracht score worden geretourneerd.

Score zoekwaarden kunnen overal in een resultatenset worden herhaald. Stel, u hebt 10 items met een score van 1.2, 20 items met een score van 1.0 en 20 items met een score van 0,5. Wanneer meerdere treffers dezelfde zoeken score hebt, de volgorde van dezelfde behaald items is niet gedefinieerd en is niet stabiele. De query opnieuw, en u ziet mogelijk uitvoeren items shift positie. Twee items met een identieke score gegeven, is geen zeker welke monitor wordt als eerste weergegeven.

## <a name="when-to-use-custom-scoring"></a>Wanneer gebruikt u aangepaste scoren

Wanneer de standaard classificeren gedrag niet Ga ver in vergadering uw zakelijke doelstellingen, moet u een of meer score profielen maken. U kunt bijvoorbeeld misschien dat zoekrelevantie voorrang toegevoegde items geven moet. U hebt ook een veld dat winstmarge bevat, of een ander veld waarin wordt aangegeven dat de potentiële opbrengst. Verhogen treffers dat voordelen voor uw bedrijf brengen, kan dit een belangrijke factor bij het bepalen van de score profielen gebruiken zijn.

Relevantie gebaseerde bestellen wordt ook geïmplementeerd via scoren profielen. Houd rekening met pagina's met zoekresultaten u hebt gebruikt in het verleden waarmee u sorteren op prijs, datum, classificatie of relevantie te verkrijgen. Score profielen station in Azure zoeken, de optie 'interessant'. De definitie van relevantie te verkrijgen wordt bepaald door u echter afhankelijk van de zakelijke doelstellingen en het type zoekfunctie moet worden geleverd.

<a name="example"></a>
## <a name="example"></a>Voorbeeld

Zoals aangegeven, wordt aangepaste scoren geïmplementeerd via de profielen die zijn gedefinieerd in een schema index scoren.

In dit voorbeeld ziet u het schema van een index met twee score profielen (`boostGenre`, `newAndHighlyRated`). Een query op deze index dat beide profiel bevat terwijl een queryparameter het profiel wordt gebruikt voor het verkrijgen van de resultatenset.

[In dit voorbeeld proberen](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Werkstroom

Implementatie van aangepaste score gedrag, moet u een score profiel toevoegen aan het schema waarin de index. U kunt meerdere score profielen binnen een index hebben, maar u kunt slechts één standaardprofiel opgeven voor een opgegeven query.

Beginnen met de [sjabloon](#bkmk_template) die beschikbaar zijn in dit onderwerp.

Geef een naam. Score profielen zijn optioneel, maar als u een toevoegt, de naam is vereist. Zorg ervoor dat u de naamconventies voor velden (begint met een letter, voorkomt u speciale tekens en gereserveerde woorden). Zie [Regels voor naamgeving](http://msdn.microsoft.com/library/azure/dn857353.aspx) voor meer informatie.

De hoofdtekst van het score profiel is gebaseerd op gewogen velden en functies.

### <a name="weights"></a>Lijndikten ###

De `weights` eigenschap van een score profiel geeft naam-waardeparen die een relatieve dikte aan een veld toewijzen. In het [voorbeeld](#example)wordt de albumTitle, nieuwe en artistName velden zijn gestimuleerd 1,5, 5 en 2, respectievelijk. Waarom is nieuwe verhoogd veel hoger dan de andere? Als de zoekopdracht wordt uitgevoerd via de gegevens die enigszins homogeen is (zoals het geval met 'nieuwe' in de `musicstoreindex`), moet u wellicht een grotere variantie in de relatieve lijndikten. Bijvoorbeeld, in de `musicstoreindex`, 'uit' weergegeven als beide een nieuwe en in dezelfde geformuleerd nieuwe beschrijvingen. Als u wilt dat nieuwe naar wegen tegen nieuwe beschrijving, moet het nieuwe veld een veel hoger relatieve dikte.

### <a name="functions"></a>Functies ###

Functies worden gebruikt wanneer aanvullende berekeningen vereist voor specifieke contexten zijn. Geldige functietypen zijn `freshness`, `magnitude`, `distance` en `tag`. Elke functie heeft parameters die uniek voor deze zijn.

  - `freshness`Als u wilt vergroten moet worden gebruikt door hoe nieuwe of oude een item is. Deze functie kan alleen worden gebruikt met datetime-velden (`Edm.DataTimeOffset`). Opmerking de `boostingDuration` kenmerk wordt alleen gebruikt met de functie fris.
  - `magnitude`moet worden gebruikt als u vergroten op basis wilt van hoe hoog of laag een numerieke waarde is. Scenario's die voor deze functie bellen nemen waarbij de winstmarge, prijs hoogste, laagste prijs of een telling van downloads. Als u wilt dat het inverse patroon (bijvoorbeeld om lagere prijs voor Microfoonversterking items meer dan duur items), kunt u het bereik, hoog naar laag, omkeren. Een bereik van prijzen van €100 gegeven in valutawaarde 1, stelt u `boostingRangeStart` 100 en `boostingRangeEnd` bij 1 de items lagere prijs vergroten. Deze functie kan alleen worden gebruikt met velden dubbel en geheel getal.
  - `distance`moet worden gebruikt als u wilt vergroten door nabijheid of geografische locatie. Deze functie kan alleen worden gebruikt met `Edm.GeographyPoint` velden.
  - `tag`moet worden gebruikt als u wilt vergroten door codes gemeenschappelijke tussen documenten en zoekopdrachten gaat uitvoeren. Deze functie kan alleen worden gebruikt met `Edm.String` en `Collection(Edm.String)` velden.
  
#### <a name="rules-for-using-functions"></a>Regels voor het gebruik van functies ####

  - Functietype (fris, grootte, afstand, markering) moet kleine letters.
  - Functies kunnen geen null of lege waarden bevatten. Als u een veldnaam opneemt, hebt u specifiek, stel deze in op een ander nummer.
  - Functies kunnen alleen worden toegepast op filterbare velden. Zie [Index maken](search-api-2015-02-28-preview.md#CreateIndex) voor meer informatie over filterbare velden.
  - Functies kunnen alleen worden toegepast op de velden die zijn gedefinieerd in de velden verzameling indexen.

Nadat u de index is gedefinieerd, stelt u de index samen door het schema index, gevolgd door documenten te uploaden. Zie [Index maken](search-api-2015-02-28-preview.md#CreateIndex) en [toevoegen of bijwerken documenten](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) voor instructies voor deze bewerkingen. Zodra de index is gemaakt, kunt u een functionele score profiel die met uw zoekgegevens werkt nodig hebt.

<a name="bkmk_template"></a>
## <a name="template"></a>Sjabloon
Dit gedeelte worden de syntaxis en het sjabloon voor het scoren profielen. Raadpleeg [Index kenmerkverwijzing](#bkmk_indexref) in de volgende sectie voor beschrijvingen van de kenmerken.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Overzicht van de eigenschap score profiel

**Opmerking** Een score functie kan alleen worden toegepast op de velden die filterbare zijn.

| Eigenschap | Beschrijving |
|----------|-------------|
| `name`   | Vereist. Dit is de naam van het score profiel. De dezelfde naamgevingsconventies voor een veld volgt. Dit moet beginnen met een letter, mogen geen punten, dubbele punten bevatten of @ symbolen en kan niet starten met de uitdrukking 'azureSearch"(hoofdlettergevoelig). |
| `text` | De eigenschap lijndikten bevat. |
| `weights` | Optioneel. Een paar naam-waarde die, een veldnaam en relatieve dikte aangeeft. Relatieve dikte moet een positief geheel getal of Drijvendekommagetal. U kunt de naam van het veld zonder een bijbehorende gewicht opgeven. Lijndikten worden gebruikt om het belang van één veld ten opzichte van elkaar te geven. |
| `functions` | Optioneel. Houd er rekening mee dat een score functie kan alleen worden toegepast op velden die filterbare zijn. |
| `type` | Vereist voor het scoren van functies. Geeft het type van de functie om te gebruiken. Geldige waarden zijn `magnitude`, `freshness`, `distance` en `tag`. U kunt meer dan één functie opnemen in elk score profiel. De functienaam moet kleine letters. |
| `boost` | Vereist voor het scoren van functies. Een positief getal dat wordt gebruikt als de vermenigvuldiger voor onbewerkte score. Deze mogen niet gelijk is aan 1. |
| `fieldName` | Vereist voor het scoren van functies. Een score functie kan alleen worden toegepast op de velden die deel uitmaken van de Veldenverzameling van de index en die zijn filterbare. Elk functietype introduceert bovendien aanvullende beperkingen (fris wordt gebruikt met datetime velden, grootte met gehele getal of dubbele velden, afstand met locatievelden en tag met tekenreeks of tekenreeks siteverzameling velden). U kunt slechts één veld per functiedefinitie opgeven. Bijvoorbeeld als u wilt gebruiken grootte tweemaal in hetzelfde profiel, moet u twee definities grootte, één telefoonnummer voor elk veld opnemen. |
| `interpolation` | Vereist voor het scoren van functies. Hiermee definieert u de richtingscoëfficiënt waarvoor de score verhogen toeneemt vanaf het begin van het bereik naar het einde van het bereik. Geldige waarden zijn `linear` (standaard), `constant`, `quadratic`, en `logarithmic`. Zie [interpolations instellen](#bkmk_interpolation) voor meer informatie. |
| `magnitude` | De omvang scoren functie wordt gebruikt voor classificatie op basis van het bereik van waarden voor een numeriek veld wijzigen. Enkele van de meest voorkomende voorbeelden van het gebruik van deze zijn:<ul><li>Sterwaardering: wijzigen het scoren op basis van de waarde in het veld "Sterretje beoordeling". Wanneer u twee items relevant zijn, wordt het item met de hogere waardering eerst worden weergegeven.</li><li>Marge: Wanneer twee documenten relevant zijn, winkel mogelijk wilt verhogen van documenten met hogere marges eerst.</li><li>Klik op telt: voor toepassingen die de voortgang bijhouden, klikt u op tot en met acties producten of pagina's, kunt u grootte voor Microfoonversterking items die meestal om optimaal verkeer.</li><li>Telt downloaden: voor toepassingen dat bijhouden downloads, de grootte-functie kunt u verhogen items waarnaar u hebt de meest downloads.</li></ul> |
| `magnitude:boostingRangeStart` | Hiermee stelt u de beginwaarde van het bereik waarover grootte score voorzien. De waarde moet een geheel getal of Drijvendekommagetal. Voor classificaties van 1 tot en met 4 zou dit 1. Voor marges meer dan 50% zou dit 50. |
| `magnitude:boostingRangeEnd` | Hiermee stelt u de waarde van het bereik waarover grootte score voorzien. De waarde moet een geheel getal of Drijvendekommagetal. Voor classificaties van 1 tot en met 4 zou dit 4. |
| `magnitude:constantBoostBeyondRange` | Geldige waarden zijn true of false (standaard). Als de waarde true, de volledige Microfoonversterking blijft toe te passen op documenten met een waarde voor het doelveld die hoger zijn dan de bovenkant van het bereik is. Als de waarde false, niet voor documenten met een waarde voor het doelveld die buiten het bereik valt voor de Microfoonversterking van deze functie toegepast. |
| `freshness` | De functie scoren fris wordt gebruikt voor classificatie scores voor items op basis van waarden in DateTimeOffset velden wijzigen. Een item met een recentere datum kan bijvoorbeeld hoger dan oudere items worden gerangschikt. (Houd er rekening mee dat het is ook mogelijk voor rang onderdelen zoals agendagebeurtenissen met toekomstige datums zodanig dat items dichter bij de huidige kunnen worden rangschikking hoger dan items verder in de toekomst.) In de huidige versie van de service, wordt één uiteinde van het bereik opgelost aan de huidige tijd. Het andere uiteinde is een tijd in het verleden op basis van de `boostingDuration`. Te verhogen een bereik van momenten in de toekomst gebruikt u een negatief `boostingDuration`. De snelheid waarmee de verhogen wijzigingen uit maximaal en minimumbereik wordt bepaald door de interpolatie toegepast op het score profiel (Zie de onderstaande afbeelding). Als u wilt omkeren de prestatieverbetering factor die wordt toegepast, kiest u Microfoonversterking bevolking van minder dan 1. |
| `freshness:boostingDuration` | Sets een verloopdatum na verhoogd worden niet meer voor een bepaald document. Zie [Set boostingDuration] [#bkmk_boostdur] in de volgende sectie voor de syntaxis en voorbeelden. |
| `distance` | Sluit u de afstand score functie wordt gebruikt om te beïnvloeden score van documenten op basis van hoe of uiterst ze zijn ten opzichte van de geografische locatie van een verwijzing. De locatie van de verwijzing is opgegeven als onderdeel van de query in een parameter (met de `scoringParameter` queryparameter) als een ion, lat argument. |
| `distance:referencePointParameter` | Een parameter worden doorgegeven in een query wilt gebruiken als verwijzing locatie. scoringParameter is een queryparameter. Zie [Documenten zoeken](search-api-2015-02-28-preview.md#SearchDocs) voor beschrijvingen van queryparameters. |
| `distance:boostingDistance` | Een getal dat geeft de afstand in kilometer vanaf de verwijzing locatie waar het prestatieverbetering bereik eindigt. |
| `tag` | De tag scoren functie wordt gebruikt om te beïnvloeden score van documenten op basis van labels in documenten en zoekopdrachten gaat uitvoeren. Documenten met tags net als bij de zoekopdracht wordt worden verhoogd. De labels voor de query wordt weergegeven als een score parameter in elke zoekopdracht (met de `scoringParameter` queryparameter). |
| `tag:tagsParameter` | Een parameter worden doorgegeven in query's kunt u labels voor een bepaald verzoek opgeven. `scoringParameter`een queryparameter is. Zie [Documenten zoeken](search-api-2015-02-28-preview.md#SearchDocs) voor beschrijvingen van queryparameters. |
| `functionAggregation` | Optioneel. Dit geldt alleen wanneer functies zijn opgegeven. Geldige waarden zijn: `sum` (standaard), `average`, `minimum`, `maximum`, en `firstMatching`. Een score zoeken is één waarde die is berekend uit meerdere variabelen, met inbegrip van meerdere functies. Deze kenmerken geeft aan hoe de verhoogt van alle functies worden gecombineerd tot een enkel statistische Microfoonversterking die vervolgens wordt toegepast op de score basisdocument. De basis score is gebaseerd op de tf-idf waarde berekend uit het document en de query. |
| `defaultScoringProfile` | Wanneer een zoekopdracht wordt uitgevoerd als er geen score profiel is opgegeven, wordt standaard scoren gebruikt (tf-idf alleen). Een standaard scoren profielnaam kan hier worden ingesteld de oorzaak van Azure zoeken wilt dit profiel gebruiken wanneer u geen specifieke profiel wordt uitgedrukt in het verzoek zoeken. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Interpolations instellen

Interpolations kunt u de richtingscoëfficiënt definiëren waarvoor de score verhogen toeneemt vanaf het begin van het bereik naar het einde van het bereik. De volgende interpolations kunnen worden gebruikt:

  - `Linear`: Voor items die in het bereik max en min zijn, worden de Microfoonversterking toegepast op het item in een voortdurend aflopende bedrag uitgevoerd. Lineaire is de standaard-interpolatie score profiel.
  - `Constant`: Voor items die zich bevindt in de begin- en dat bereik eindigt, wordt een constante Microfoonversterking worden toegepast op de rang resultaten.
  - `Quadratic`: Kwadratische wordt in eerste instantie verkleinen kleinere tempo In vergelijking met een lineaire interpolatie waarop een voortdurend aflopende Microfoonversterking, en vervolgens als dit het einde van het bereik nadert, deze afneemt met een veel hoger tijdsinterval. Deze optie interpolatie is niet toegestaan in de tag scoren functies.
  - `Logarithmic`: Logaritmisch wordt in eerste instantie verkleinen hoger tempo In vergelijking met een lineaire interpolatie waarop een voortdurend aflopende Microfoonversterking, en vervolgens als dit het einde van het bereik nadert, deze afneemt met een kleinere veel tijdsinterval. Deze optie interpolatie is niet toegestaan in de tag scoren functies.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>BoostingDuration instellen

`boostingDuration`is een kenmerk van de functie fris. U deze gebruikt voor het instellen van een verloopbeleid punt achter verhoogd worden niet meer voor een bepaald document. Als u wilt vergroten een productlijn of huisstijl gedurende een periode 10 dagen, geeft u bijvoorbeeld de 10 dagen als "P10D" voor deze documenten. Of vergroten aanstaande gebeurtenissen in de volgende week opgeven "-P7D".

`boostingDuration`moet worden opgemaakt als een XSD "dayTimeDuration" waarde (een beperkte subset van de waarde van de duur van een ISO-8601). Het patroon hiervoor is: `[-]P[nD][T[nH][nM][nS]]`.

De volgende tabel vindt enkele voorbeelden.

| Duur | boostingDuration |
|----------|------------------|
| 1 dag | "P1D" |
| 2 dagen en 12 uur | "P2DT12H" |
| 15 minuten | "PT15M" |
| 30 dagen, 5 uur 10 minuten, en 6.334 seconden | "P30DT5H10M6.334S" |

Zie voor meer voorbeelden [XML-Schema: gegevenstypen (website W3.org)](http://www.w3.org/TR/xmlschema11-2/).

**Zie ook**
[Azure zoeken Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) op MSDN <br/>
[Index (Azure zoeken API) maken](http://msdn.microsoft.com/library/azure/dn798941.aspx) op MSDN<br/>
[Toevoegen aan een zoekindex score profiel](http://msdn.microsoft.com/library/azure/dn798928.aspx) op MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
