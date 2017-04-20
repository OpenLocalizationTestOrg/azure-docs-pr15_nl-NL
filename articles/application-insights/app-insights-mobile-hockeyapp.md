<properties
    pageTitle="Prestatiecontroles voor mobiele WebApps gebruiken met ontwikkelaars Analytics | Microsoft Azure"
    description="Prestaties van toepassingen en het gebruik monitoren voor ontwikkelaars van de mobiele app. , bureaublad, webservice en backend-apps gebruiken met HockeyApp en inzichten van toepassing."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobiele analyses met HockeyApp en inzichten van toepassing

De prestaties en het gebruik van uw beta-toets en geïmplementeerd mobiele en bureaublad-apps gebruiken met [HockeyApp](https://hockeyapp.net/)bewaken. De ondersteunende web service en backend-apps gebruiken met [Visual Studio toepassing inzichten](app-insights-overview.md)bewaken. Gegevens van de client en server apps in Analytics analyseren en de grafieken naast elkaar weergeven in een Azure-dashboard.

Microsoft Developer Analytics biedt twee oplossingen voor prestatiecontroles uit toepassing:

* **HockeyApp voor mobiele apparaten** en de bureaublad-apps.
 * Testversies aan geselecteerde gebruikers distribueren.
 * Loopt vast analyse.
 * Aangepaste telemetrielogboek voor gebruiksanalyse.
* **Toepassing inzichten voor websites** en -services en back-end-toepassingen.
 * Prestaties en het gebruik van de doelstellingen en waarschuwingen.
 * Uitzondering rapportage en diagnostische traceren.
 * Diagnostische gegevens zoeken met filteren en de gerelateerde telemetrielogboek koppelingen.

Beide oplossingen bieden:

 * Krachtige **[analytische querytaal](app-insights-analytics.md)** voor hulpprogramma's voor diagnose en analyse.
 * **[Gegevens exporteren](app-insights-export-telemetry.md)** naar uw eigen opslag.
 * De weergave van de **geïntegreerde dashboard** van analytische grafieken en tabellen.

## <a name="monitor-your-app-components"></a>Bewaak uw app-onderdelen

Volg de instructies in deze pagina's voor het installeren van de SDK in code en uw app bijhoudt.

### <a name="web-apps---application-insights"></a>WebApps - toepassing inzichten

* [ASP.NET-web-app](app-insights-asp-net.md) 
* [Java WebApp](app-insights-java-get-started.md)
* [Node.js WebApp](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure Cloudservices](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobiele apps - HockeyApp

* [iOS-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-app](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universele Windows-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 en 8.1-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Presentatie van Windows Foundation-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Voor Windows-bureaublad-apps raden HockeyApp. Maar u kunt ook [telemetrielogboek vanuit een Windows-app inzicht krijgen in toepassing verzenden](app-insights-windows-desktop.md). U kunt doen om te experimenteren met de toepassing inzichten.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analyses en exporteren voor HockeyApp telemetrielogboek

U kunt aangepaste HockeyApp onderzoeken en meld u via de analyses en continue exporteren functies van toepassing inzichten door [het instellen van een brug](app-insights-hockeyapp-bridge-app.md)telemetrielogboek.




