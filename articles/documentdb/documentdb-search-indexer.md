<properties
    pageTitle="DocumentDB verbinding met Azure zoekprogramma's via indexeerfuncties | Microsoft Azure"
    description="In dit artikel leest u hoe u met de indexering Azure zoeken met DocumentDB als gegevensbron."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Verbindende DocumentDB met Azure zoekprogramma's via indexeerfuncties

Als u op zoek bent implementatie van geweldige zoeken ervaringen DocumentDB gegevens, kunt u de zoekopdracht Azure indexering gebruiken voor DocumentDB! In dit artikel wordt we uitgelegd hoe u Azure DocumentDB integreren met Azure zoeken zonder code voor het behoud van indexing infrastructuur te schrijven!

Dit als u wilt instellen, kunt u moet [een zoekopdracht Azure-account setup](../search/search-create-service-portal.md) (u hoeft niet te upgraden naar standaard zoeken), en vervolgens bellen de [Azure zoeken REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) als een DocumentDB **gegevensbron** en een **indexering** voor de gegevensbron wilt maken.

U kunt in serviceaanvragen verzenden om te communiceren met de REST API's, [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)of elk hulpmiddel van uw voorkeur gebruiken.


##<a id="Concepts"></a>Azure zoeken indexering concepten

Azure zoeken ondersteunt het maken en beheren van gegevens gegevensbronnen (inclusief DocumentDB) en indexeerfuncties die ten opzichte van die gegevensbronnen werken.

Een **gegevensbron** geeft aan welke gegevens moeten worden geïndexeerd, referenties voor toegang tot de gegevens en beleid voor het inschakelen van Azure zoeken efficiënt identificeren wijzigingen in de gegevens (zoals gewijzigd of verwijderd documenten binnen de verzameling). De gegevensbron is gedefinieerd als een onafhankelijke bron, zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexering** wordt beschreven hoe de gegevens van uw gegevensbron loopt in de zoekindex van een doel. Het is raadzaam over het maken van één indexering voor elke combinatie van target index en gegevens bron. Terwijl u meerdere indexeerfuncties in dezelfde index schrijft hebt kunt, kunt alleen een indexering schrijven in een enkele index. Een indexering wordt gebruikt om:

- Een momentopname van de gegevens voor het vullen van een index uitvoeren.
- Een index met wijzigingen in de gegevensbron volgens een schema synchroniseren. De planning maakt deel uit van de definitie van indexering.
- Aanroepen op aanvraag-updates voor een index naar wens.

##<a id="CreateDataSource"></a>Stap 1: Een gegevensbron maken

Een HTTP POST-verzoek om een nieuwe gegevensbron maken in uw Azure Search-service, inclusief de kop van de volgende aanvraag actie.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

De `api-version` is vereist. Geldige waarden zijn `2015-02-28` of een latere versie. Ga naar [API-versies in Azure zoeken](../search/search-api-versions.md) als u wilt zien van alle ondersteunde zoeken API-versies.

De hoofdtekst van de aanvraag bevat de definitie van de gegevensbron, waaronder de volgende velden moet:

- **naam**: Kies een naam voor uw database DocumentDB.

- **type**: Gebruik `documentdb`.

- **referenties**:

    - **connectionString**: vereist. Geef de verbindingsgegevens aan uw DocumentDB Azure-database in de volgende indeling:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **container**:

    - **naam**: vereist. Geef de id van de verzameling DocumentDB moeten worden geïndexeerd.

    - **query**: optioneel. U kunt een query als u wilt samenvoegen van een willekeurige JSON-document in een platte schema die Azure Search indexeren kunt opgeven.

- **dataChangeDetectionPolicy**: optioneel. Zie onderstaande worden [gegevens detectie van beleid wijzigen](#DataChangeDetectionPolicy) .

- **dataDeletionDetectionPolicy**: optioneel. Zie [gegevens verwijdering detectie beleid](#DataDeletionDetectionPolicy) hieronder.

Zie hieronder voor een [voorbeeld van de aanvraag hoofdtekst](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Gewijzigde documenten vastleggen

Het doel van een beleid voor detectie wijzigen is efficiënt gewijzigde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `High Water Mark` beleid gebruiken de `_ts` laatste wijziging tijdstempel eigenschap verstrekt door DocumentDB - die is opgegeven als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

U moet ook om toe te voegen `_ts` in de raming en `WHERE` component voor uw query. Bijvoorbeeld:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Verwijderde documenten vastleggen

Wanneer de rijen zijn verwijderd uit de brontabel, moet u de rijen verwijderen uit als u ook de zoekindex. Het doel van een beleid verwijdering detectie is efficiënt verwijderde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `Soft Delete` beleid (verwijderen is gemarkeerd met een vlag van enkele sorteren), wordt bepaald welke als volgt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] U moet de eigenschap softDeleteColumnName opnemen in de SELECT-component als u een aangepaste raming gebruikt.

###<a id="CreateDataSourceExample"></a>Voorbeeld van de hoofdtekst aanvragen

Het volgende voorbeeld wordt een gegevensbron met een aangepaste hints voor de query en beleid:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Antwoord

U ontvangt een antwoord HTTP 201 gemaakt als de gegevensbron is gemaakt.

##<a id="CreateIndex"></a>Stap 2: Een index maken

Maak een doellijst Azure zoekindex als u nog niet hebt. Vanaf de [Azure Portal UI](../search/search-create-index-portal.md) of met behulp van de [Index-API maken](https://msdn.microsoft.com/library/azure/dn798941.aspx), kunt u dit doen.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Zorg ervoor dat het schema van de doel-index compatibel met het schema van de bron JSON-documenten of de uitvoer van uw aangepaste query raming is.

>[AZURE.NOTE] Voor gepartitioneerde verzamelingen, is het document standaardsleutel van DocumentDB `_rid` eigenschap, die wordt gewijzigd in `rid` in Azure zoeken. Ook DocumentDB van `_rid` waarden bevatten, tekens die ongeldig in Azure zoeken toetsen zijn; daarom de `_rid` waarden zijn Base64 codering.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Afbeelding A: toewijzing tussen JSON-gegevenstypen en gegevenstypen Azure zoeken

| JSON-GEGEVENSTYPE|   COMPATIBELE DOEL INDEX VELDTYPEN|
|---|---|
|BOOL|Edm.Boolean, Edm.String|
|Getallen die zijn opgemaakt als gehele getallen|Edm.Int32, Edm.Int64, Edm.String|
|Getallen die zijn opgemaakt als zwevende punten|Edm.Double, Edm.String|
|Tekenreeks|Edm.String|
|Matrices van primitief typt bijvoorbeeld "a", "b", "c" |Collection(EDM.String)|
|Tekenreeksen die zijn opgemaakt als datums| Edm.DateTimeOffset, Edm.String|
|GeoJSON objecten bijvoorbeeld {"type": "Punt", "coördinaten": [lang, lat]} | Edm.GeographyPoint |
|Andere JSON-objecten|N/B|

###<a id="CreateIndexExample"></a>Voorbeeld van de hoofdtekst aanvragen

Het volgende voorbeeld wordt een index met een veld-id en beschrijving:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Antwoord

U ontvangt een antwoord HTTP 201 gemaakt als de index is gemaakt.

##<a id="CreateIndexer"></a>Stap 3: Een indexering maken

U kunt een nieuwe indexering binnen een Azure-zoekservice maken met behulp van een HTTP POST-aanvraag met de volgende koppen.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

De hoofdtekst van de aanvraag bevat de definitie indexering, waaronder de volgende velden moet:

- **naam**: vereist. De naam van de indexering.

- **gegevensbronnaam**: vereist. De naam van een bestaande gegevensbron.

- **targetIndexName**: vereist. De naam van een bestaande index.

- **planning**: optioneel. Zie onderstaande worden [planning indexeren](#IndexingSchedule) .

###<a id="IndexingSchedule"></a>Actieve indexeerfuncties volgens een schema

Een indexering kunt u desgewenst een schema opgeven. Als een planning aanwezig is, wordt de indexering regelmatig aan de hand van planning uitvoeren. Planning heeft de volgende kenmerken:

- **interval**: vereist. Een duurwaarde waarmee een interval of punt voor indexering wordt uitgevoerd. Het kleinste toegestane interval is 5 minuten; de langste is één dag. Deze moet worden opgemaakt als een XSD "dayTimeDuration" waarde (een beperkte subset van de waarde van een [ISO-8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

- **Starttijd**: vereist. Een UTC-datetime waarmee wordt bepaald wanneer de indexering moet beginnen uitgevoerd.

###<a id="CreateIndexerExample"></a>Voorbeeld van de hoofdtekst aanvragen

Het volgende voorbeeld wordt een indexering die gegevens opgehaald uit de verzameling waarnaar wordt verwezen door de `myDocDbDataSource` gegevensbron aan de `mySearchIndex` index volgens een schema dat begint op 1 januari 2015 UTC en per uur is uitgevoerd.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Antwoord

U ontvangt een antwoord HTTP 201 gemaakt als de indexering is gemaakt.

##<a id="RunIndexer"></a>Stap 4: Een indexering uitvoeren

Naast het uitgevoerd regelmatig volgens een schema, kan een indexering ook worden opgeroepen op aanvraag de volgende HTTP POST-verzoek verzenden:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwoord

U ontvangt een antwoord HTTP 202 geaccepteerd als de indexering is geactiveerd.

##<a name="GetIndexerStatus"></a>Stap 5: Get indexering status

U kunt een HTTP GET-aanvraag om op te halen van de huidige status en de uitvoering van de geschiedenis van een indexering probleem:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwoord

Hier ziet u een HTTP 200 OK antwoord samen met de hoofdtekst van een antwoord met informatie over algemene indexering systeemstatus status, de laatste indexering aanroep, evenals de geschiedenis van recente indexering aanroepen (indien aanwezig).

Het antwoord ziet er ongeveer als volgt uit:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Uitvoering geschiedenis bevat snel aan de 50 meest recente voltooide executions, die zijn gesorteerd in omgekeerde chronologische volgorde (dus de meest recente uitvoering boven aan het antwoord komt).

##<a name="NextSteps"></a>Volgende stappen

Gefeliciteerd! U hebt geleerd hoe u kunt Azure DocumentDB integreren met Azure zoeken met behulp van de indexering voor DocumentDB.

 - Als u wilt weten hoe over Azure DocumentDB, moet u de [pagina van de service DocumentDB](https://azure.microsoft.com/services/documentdb/)weergegeven.

 - Meer informatie over hoe meer informatie over Azure zoeken, moet u de [service zoekpagina](https://azure.microsoft.com/services/search/)weergegeven.
