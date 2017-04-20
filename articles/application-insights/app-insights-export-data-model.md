<properties 
    pageTitle="Toepassing inzichten-gegevensmodel" 
    description="Eigenschappen van continue export in JSON geëxporteerd en gebruikt als filters wordt beschreven." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Toepassing inzichten exporteren-gegevensmodel

In deze tabel worden de eigenschappen van telemetrielogboek verzonden vanuit de [Toepassing inzichten](app-insights-overview.md) SDK's bij de portal. Hier ziet u deze eigenschappen in gegevensuitvoer van [Continue exporteren](app-insights-export-telemetry.md).
Ze ook weergegeven in eigenschapsfilters in [Metrisch Explorer](app-insights-metrics-explorer.md) en [Diagnostische zoeken](app-insights-diagnostic-search.md).

Houd er rekening mee wordt verwezen:

* `[0]`in deze tabellen aangegeven een punt in het pad waar u moet een index; invoegen maar het is niet altijd 0.
* De tijdsduur van de zijn in tiende van een microseconde, dus 10000000 == 1 seconde.
* Datums en tijden UTC zijn en krijgen in de ISO-indeling`yyyy-MM-DDThh:mm:ss.sssZ`

Er zijn verschillende [voorbeelden](app-insights-export-telemetry.md#code-samples) die aangeven hoe ze worden gebruikt.



## <a name="example"></a>Voorbeeld

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Context

Alle typen telemetrielogboek vergezeld een sectie context. Niet meer van deze velden worden verzonden met elk gegevenspunt.



|Pad|Type|Notities|
|---|---|---|
| context.Custom.Dimensions [0]  | object]  | Combinaties van sleutelwaarde tekenreeks om door de aangepaste eigenschappen parameter zijn ingesteld. Belangrijke maximale lengte 100, de maximale lengte 1024 waarden. Meer dan 100 unieke waarden, de eigenschap kan worden gezocht, maar kan niet worden gebruikt voor segmentatie. Max 200 toetsen per ikey.  |
| context.Custom.metrics [0]  | object]  | Sleutel-waardeparen instellen door aangepaste afmetingen parameter en TrackMetrics. Belangrijke maximale lengte 100, de waarden mogelijk numerieke. |
| context.data.eventTime | tekenreeks | UTC |
| context.data.isSynthetic | Booleaanse waarde | Aanvraag is zichtbaar voor afkomstig zijn uit een bots of web-toets. |
| context.data.samplingRate | getal | Percentage van telemetrielogboek gegenereerd door de SDK die wordt verzonden naar de portal. Bereik 0,0-100,0.|
| context.Device | object | Clientapparaat worden gebruikt |
| context.Device.browser | tekenreeks | IE, Chrome... |
| context.device.browserVersion | tekenreeks | Chrome 48,0... |
| context.device.deviceModel | tekenreeks | |
| context.device.deviceName | tekenreeks | |
| context.Device.id | tekenreeks | |
| context.Device.locale | tekenreeks | en-GB, nl-nl... |
| context.Device.Network | tekenreeks | |
| context.device.oemName | tekenreeks | |
| context.device.osVersion | tekenreeks | Host OS |
| context.device.roleInstance | tekenreeks | ID van serverhost |
| context.device.roleName | tekenreeks | |
| context.Device.type | tekenreeks | PC, Browser... |
| context.Location | object | Afgeleid van client-IP. |
| context.Location.City | tekenreeks | Afgeleid van client-IP, indien bekend  |
| context.Location.clientip | tekenreeks | Laatste achthoek is anoniem 0. |
| context.Location.continent | tekenreeks | |
| context.Location.Country | tekenreeks | |
| context.Location.Province | tekenreeks | Provincie |
| context.Operation.id | tekenreeks | Items met dezelfde bewerking-id worden weergegeven als verwante Items in de portal. Meestal de aanvraag-id. |
| context.Operation.name | tekenreeks | URL of het verzoek naam |
| context.operation.parentId | tekenreeks | Hiermee kunnen geneste verwante items. |
| context.Session.id | tekenreeks | Id van een groep bewerkingen uit dezelfde bron. Een periode van 30 minuten zonder een bewerking geeft het einde van een sessie. |
| context.session.isFirst | Booleaanse waarde | |
| context.user.accountAcquisitionDate | tekenreeks | |
| context.user.anonAcquisitionDate | tekenreeks | |
| context.user.anonId | tekenreeks | |
| context.user.authAcquisitionDate | tekenreeks | [Geverifieerde gebruiker](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Booleaanse waarde | |
| internal.data.documentVersion | tekenreeks | |
| Internal.Data.id | tekenreeks | |



## <a name="events"></a>Gebeurtenissen

Aangepaste gebeurtenissen gegenereerd door [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Pad|Type|Notities|
|---|---|---|
| aantal gebeurtenissen [0] | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld 4 =&gt; 25%. |
| de gebeurtenisnaam van de [0] | tekenreeks | De naam van de gebeurtenis.  Maximale lengte 250. |
| url van de gebeurtenis [0] | tekenreeks | |
| gebeurtenis [0] urlData.base | tekenreeks | |
| gebeurtenis [0] urlData.host | tekenreeks | |

## <a name="exceptions"></a>Uitzonderingen

Rapporten [uitzonderingen](app-insights-asp-net-exceptions.md) op de server en in de browser. 


|Pad|Type|Notities|
|---|---|---|
| constructie basicException [0] | tekenreeks | |
| basicException [0] tellen | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld 4 =&gt; 25%. |
| basicException [0] exceptionGroup | tekenreeks | |
| basicException [0] exceptionType | tekenreeks | |tekenreeks | |
| basicException [0] failedUserCodeMethod | tekenreeks | |
| basicException [0] failedUserCodeAssembly | tekenreeks | |
| basicException [0] handledAt | tekenreeks | |
| basicException [0] hasFullStack | Booleaanse waarde | |
| basicException [0]-id | tekenreeks | |
| methode basicException [0] | tekenreeks | |
| bericht basicException [0] | tekenreeks | Uitzonderingsbericht. Maximale lengte 10k.|
| basicException [0] outerExceptionMessage | tekenreeks | |
| basicException [0] outerExceptionThrownAtAssembly | tekenreeks | |
| basicException [0] outerExceptionThrownAtMethod | tekenreeks | |
| basicException [0] outerExceptionType | tekenreeks | |
| basicException [0] outerId | tekenreeks | |
| basicException [0] parsedStack [0] constructie | tekenreeks | |
| basicException [0] parsedStack [0] bestandsnaam | tekenreeks | |
| basicException [0] parsedStack [0] niveau | geheel getal | |
| basicException [0] parsedStack [0] lijn | geheel getal | |
| basicException [0] parsedStack [0] methode | tekenreeks | |
| stapel basicException [0] | tekenreeks | Maximale lengte 10k|
| typeName basicException [0] | tekenreeks | |



## <a name="trace-messages"></a>Berichten traceren

Verzonden door [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)en door de [logboekregistratie adapters](app-insights-asp-net-trace-logs.md).


|Pad|Type|Notities|
|---|---|---|
| bericht [0] Loggernaam | tekenreeks ||
| [0] berichtparameters | tekenreeks ||
| bericht [0] onbewerkte | tekenreeks | Het logboekbericht maximale lengte 10k. |
| Foutcode bericht [0] | tekenreeks | |



## <a name="remote-dependency"></a>Externe afhankelijkheid

Door TrackDependency verzonden. Gebruikt voor het rapportprestaties en het gebruik van [oproepen naar afhankelijkheden](app-insights-asp-net-dependencies.md) op de server en AJAX u belt in de browser.

|Pad|Type|Notities|
|---|---|---|
| asynchrone remoteDependency [0] | Booleaanse waarde | |
| remoteDependency [0] baseName | tekenreeks |  |
| remoteDependency [0] commandName | tekenreeks | Bijvoorbeeld "Start/index" |
| remoteDependency [0] tellen | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld 4 =&gt; 25%. |
| remoteDependency [0] dependencyTypeName | tekenreeks | HTTP, SQL... |
| remoteDependency [0] durationMetric.value | getal | De tijd op van het gesprek tot voltooiing van antwoord van afhankelijkheid |
| remoteDependency [0]-id | tekenreeks | |
| de naam van de remoteDependency [0] | tekenreeks | URL. Maximale lengte 250.|
| remoteDependency [0] resultCode | tekenreeks | van HTTP afhankelijkheid |
| remoteDependency [0] success | Booleaanse waarde | |
| type remoteDependency [0] | tekenreeks | HTTP, Sql... |
| remoteDependency [0]-url | tekenreeks |  Maximale lengte 2000 |
| remoteDependency [0] urlData.base | tekenreeks | Maximale lengte 2000 |
| remoteDependency [0] urlData.hashTag | tekenreeks | |
| remoteDependency [0] urlData.host | tekenreeks | Maximale lengte 200 |


## <a name="requests"></a>Aanvragen

Door [TrackRequest](app-insights-api-custom-events-metrics.md#track-request)verzonden. De standaard modules Hiermee rapporten server antwoord tijd, gemeten op de server. 


|Pad|Type|Notities|
|---|---|---|
| aanvraag [0] tellen | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld: 4 =&gt; 25%. |
| aanvraag [0] durationMetric.value | getal | Tijd aangevraagde binnengekomen aan antwoord. 1e7 == 1s |
| aanvraag [0]-id | tekenreeks | Bewerkings-id |
| de naam van de aanvraag [0] | tekenreeks | GET-bericht + url grondtal.  Maximale lengte 250 |
| aanvraag [0] responseCode | geheel getal | HTTP-antwoord verzonden naar klant |
| [0] success aanvragen | Booleaanse waarde | Standaard == (responseCode &lt; 400) |
| aanvraag [0]-url | tekenreeks | Inclusief niet host |
| aanvraag [0] urlData.base | tekenreeks | |
| aanvraag [0] urlData.hashTag | tekenreeks |  |
| aanvraag [0] urlData.host | tekenreeks | |


## <a name="page-view-performance"></a>Paginaweergave prestaties

Verzonden door de browser. Meet de tijd om een pagina van de gebruiker het verzoek om weer te geven (exclusief asynchrone AJAX oproepen) voltooid wordt gestart.

Context-waarden bevatten client OS en versie van de browser. 


|Pad|Type|Notities|
|---|---|---|
| clientPerformance [0] clientProcess.value | geheel getal | De tijd op van het einde van het ontvangen van de HTML-code voor het weergeven van de pagina. |
| de naam van de clientPerformance [0] | tekenreeks | |
| clientPerformance [0] networkConnection.value | geheel getal | Tijd tot stand brengen van een netwerkverbinding. |
| clientPerformance [0] receiveRequest.value | geheel getal | De tijd op van het einde van de aanvraag wordt verzonden naar de HTML-code bij beantwoorden ontvangen. |
| clientPerformance [0] sendRequest.value | geheel getal | Tijd uit die u wilt verzenden de HTTP-aanvraag hebt gemaakt. |
| clientPerformance [0] total.value | geheel getal | Tijd verzend het verzoek tot de weergave van de pagina gaan. |
| clientPerformance [0]-url | tekenreeks | URL van deze aanvraag |
| clientPerformance [0] urlData.base | tekenreeks | |
| clientPerformance [0] urlData.hashTag | tekenreeks | |
| clientPerformance [0] urlData.host | tekenreeks | |
| clientPerformance [0] urlData.protocol | tekenreeks | |

## <a name="page-views"></a>Paginaweergaven

Door trackPageView() of [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view) verzonden

|Pad|Type|Notities|
|---|---|---|
| aantal weergaven [0] | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld 4 =&gt; 25%. |
| weergave [0] durationMetric.value | geheel getal | (Optioneel) instellen in trackPageView() of startTrackPage() - waarde stopTrackPage(). Niet hetzelfde als clientPerformance waarden. |
| Weergavenaam [0] | tekenreeks | De titel van de pagina.  Maximale lengte 250 |
| url van de weergave [0] | tekenreeks | |
| weergave [0] urlData.base | tekenreeks | |
| weergave [0] urlData.hashTag | tekenreeks | |
| weergave [0] urlData.host | tekenreeks | |



## <a name="availability"></a>Beschikbaarheid

[Beschikbaarheid van web tests](app-insights-monitor-web-app-availability.md)-rapporten.

|Pad|Type|Notities|
|---|---|---|
| beschikbaarheid [0] availabilityMetric.name | tekenreeks | beschikbaarheid |
| beschikbaarheid [0] availabilityMetric.value | getal |1.0 of 0,0 |
| beschikbaarheid [0] tellen | geheel getal | 100 / ([steekproeven](app-insights-sampling.md) snelheid). Bijvoorbeeld 4 =&gt; 25%. |
| beschikbaarheid [0] dataSizeMetric.name | tekenreeks | |
| beschikbaarheid [0] dataSizeMetric.value | geheel getal | |
| beschikbaarheid [0] durationMetric.name | tekenreeks | |
| beschikbaarheid [0] durationMetric.value | getal | De duur van de toets. 1e7 == 1s |
| beschikbaarheidsbericht [0] | tekenreeks | Diagnostische gegevens is mislukt |
| resultaat van de beschikbaarheid van [0] | tekenreeks | Keer/Fail |
| beschikbaarheid [0] runLocation | tekenreeks | Geografische bron van http vereist |
| beschikbaarheid [0] testName | tekenreeks | |
| beschikbaarheid [0] testRunId | tekenreeks | |
| beschikbaarheid [0] testTimestamp | tekenreeks | |




## <a name="metrics"></a>Aan de doelstellingen

Gegenereerd door TrackMetric().

De metrische waarde vindt u in context.custom.metrics[0]

Bijvoorbeeld:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Over metrieke waarden

Metrische waarden, zowel in metrische rapporten en andere luchthavens, worden vermeld met een standaard-objectstructuur. Bijvoorbeeld:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Momenteel - Hoewel dit mogelijk wijzigen in de toekomst - in alle waarden gerapporteerd van de standaard SDK modules, `count==1` en alleen de `name` en `value` velden zijn handig. De enige zaak waar zij verschillende worden zou zou als u uw eigen oproepen TrackMetric schrijven in waarin u de andere parameters instellen. 

Er is het doel van de andere velden toe te staan dat aan de doelstellingen worden samengevoegd in de SDK verkeer verkleinen bij de portal. Bijvoorbeeld kan u meerdere opeenvolgende metingen gemiddelde voor elke metrieke rapport verzenden. U zou vervolgens berekenen van de min, max, de standaarddeviatie en statistische waarde (som of gemiddelde) en aantal instellen voor het aantal metingen voorgesteld door het rapport. 

In de bovenstaande tabellen hebben we de velden zelden gebruikte tellen, min, max, STDEV en sampledValue weggelaten.

U kunt in plaats van de doelstellingen van de vooraf verzamelen, [steekproeven](app-insights-sampling.md) gebruiken als u moet het volume van telemetrielogboek verkleinen.


### <a name="durations"></a>Duur

Duur worden aangegeven in tiende van een microseconde, behalve anders wordt aangegeven, zodat 10000000.0 1 seconde betekent.



## <a name="see-also"></a>Zie ook

* [Toepassing inzichten](app-insights-overview.md) 
* [Continue exporteren](app-insights-export-telemetry.md)
* [Voorbeelden van programmacode](app-insights-export-telemetry.md#code-samples)


