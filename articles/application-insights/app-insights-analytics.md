<properties 
    pageTitle="Analytics - de krachtige zoekfunctie van toepassing inzichten | Microsoft Azure" 
    description="Overzicht van analyses, de krachtige diagnostische zoekfunctie van toepassing inzichten. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analytics in toepassing inzichten


[Analytics](app-insights-analytics.md) is de krachtige zoekfunctie van [Toepassing inzichten](app-insights-overview.md). Deze pagina's worden de Analytics query lanquage beschreven. 

* **[Bekijk de inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Uitproberen analyses van onze gesimuleerd gegevens](https://analytics.applicationinsights.io/demo)** als uw app is niet gegevens inzicht krijgen in toepassing nog verzendt.

## <a name="queries-in-analytics"></a>Query's in Analytics
 
Een typische query is *een brontabel gevolgd door een reeks *operatoren* , gescheiden door een* `|`. 

Bijvoorbeeld, laten we Ontdek welke tijd van de burgers van Hyderabad onze web-app uitproberen dag. En terwijl we er, laten we eens kijken welke resultaatcodes worden geretourneerd naar hun HTTP-aanvragen. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

We tellen distinct client IP-adressen, dat deze de afgelopen 7 dagen per uur van de dag groeperen. 

Laten we het resultaat met de presentatie staafdiagram kiezen de resultaten van verschillende antwoord codes stapelen weer:

![Kies staafdiagram, x en y-as en vervolgens segmentatie](./media/app-insights-analytics/020.png)

Lijkt erop dat onze app populairste wordt uitgevoerd op lunchpauze en pot-tijd in Hyderabad. (En gaan we deze 500 codes.)


Er zijn ook krachtige statistische bewerkingen:

![](./media/app-insights-analytics/025.png)


De taal heeft vele aantrekkelijke functies:

* [Filter](app-insights-analytics-reference.md#where-operator) uw telemetrielogboek onbewerkte app door velden, met inbegrip van uw aangepaste eigenschappen en aan de doelstellingen.
* [Deelnemen aan](app-insights-analytics-reference.md#join-operator) een meerdere tabellen – correlate aanvragen met paginaweergaven, afhankelijkheid oproepen, uitzonderingen en log wordt getraceerd.
* Krachtige statistische [aggregaties](app-insights-analytics-reference.md#aggregations).
* Net zo krachtig zijn als SQL, maar veel gemakkelijker voor complexe query's: in plaats van nesten overzichten, u de gegevens van de ene elementaire bewerking naar de volgende pipe.
* Direct en krachtige visualisaties.







## <a name="connect-to-your-application-insights-data"></a>Verbinding maken met uw gegevens toepassing inzichten


Open Analytics uit van uw app [Overzicht blade](app-insights-dashboards.md) in inzichten van toepassing: 

![Portal.azure.com openen, opent u de toepassing inzichten resource en klik op analyse.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Limieten

Queryresultaten zijn op dit moment, beperkt tot het zojuist hebt gedurende een week afgelopen gegevens.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Volgende stappen


* Het is raadzaam begint u met de [taal rondleiding](app-insights-analytics-tour.md).