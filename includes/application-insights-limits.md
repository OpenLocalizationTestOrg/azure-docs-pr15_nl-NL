Er zijn enkele beperkingen ten aanzien van het nummer van de doelstellingen en gebeurtenissen per toepassing (dat wil zeggen per instrumentation toets). 

Limieten hangt af van de [prijzen van laag](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

**Resource** | **Standaardlimiet** | **Maximumlimiet**
-------- | ------------- | -------------
Gegevenspunten sessie<sup>1, 2</sup> per maand | onbeperkt | 
Totaal aantal gegevenspunten per maand voor de aanvraag, gebeurtenis afhankelijkheid, doelcellen, uitzondering en paginaweergave | 5 miljoen | 50 miljoen<sup>3</sup>
[Tracering en Log](../articles/application-insights/app-insights-search-diagnostic-logs.md) tarief van gegevens | 200 dp/s | 500 dp/s
[Uitzondering](../articles/application-insights/app-insights-asp-net-exceptions.md) gegevens rente | 50 dp/s | 50 dp/s
Totale rente voor de aanvraag, gebeurtenis, afhankelijkheid en pagina weergave telemetrielogboek | 200 dp/s | 500 dp/s
Onbewerkte Gegevensretentie voor [Zoeken](../articles/application-insights/app-insights-diagnostic-search.md) en [analyses](../articles/application-insights/app-insights-analytics.md) | 7 dagen
Geaggregeerde gegevens bewaarbeleid voor [de doelstellingen explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 dagen
[Eigenschap](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) naam tellen | 100 |
Lengte van naam van eigenschap | 150 | 
Lengte van de eigenschap waarde | 8192 | 
Tracering en uitzondering lengte van bericht | 10000 |
[Metrisch](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) naam tellen | 100 |
Lengte van metrische naam |  150 | 
[Beschikbaarheid van de tests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> een gegevenspunt is een afzonderlijke metrische waarde of een gebeurtenis, met het bijgevoegde eigenschappen en afmetingen.

<sup>2</sup> A-sessie gegevenspunt logboeken van het begin of einde van een sessie en logboeken gebruikers-id.

<sup>3</sup> kunt u extra capaciteit dan 50 miljoen kopen.
 
[Over prijzen en quota's in de toepassing inzichten](../articles/application-insights/app-insights-pricing.md)
