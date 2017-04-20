<properties 
    pageTitle="Cmdlets voor controle gebruik en de prestaties voor Windows-bureaublad-apps" 
    description="Gebruik en prestaties van uw Windows-bureaublad-app met HockeyApp en toepassing inzichten analyseren." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Cmdlets voor controle gebruik en de prestaties in Windows-bureaublad-apps

*Er is een toepassing inzichten in de proefversie.*

[Visual Studio toepassing inzichten](app-insights-overview.md) en [HockeyApp](https://hockeyapp.net) kunt u uw gedistribueerde toepassing voor gebruik en de prestaties te controleren.

> [AZURE.IMPORTANT] U wordt aangeraden [HockeyApp](https://hockeyapp.net) distribueren en het bureaublad en apparaat apps controleren. Met HockeyApp, kunt u verdeling, live testen en feedback van gebruikers beheren, evenals gebruik en vastlopen rapporten controleren. U kunt ook [exporteren en uw telemetrielogboek met Analytics query](app-insights-hockeyapp-bridge-app.md).

> Hoewel telemetrielogboek kan worden verzonden naar de toepassing inzichten vanuit een bureaubladtoepassing, is dit het vooral handig voor foutopsporing en experiment.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Telemetrielogboek inzicht krijgen in toepassing verzenden vanuit een Windows-toepassing

1. Klik in de [portal van Azure](https://portal.azure.com) [maken van een toepassing inzichten resource](app-insights-create-new-resource.md). Kies ASP.NET-app voor toepassingstype.
2. Een kopie van de sleutel Instrumentation duren. Zoek de sleutel in de Essentials vervolgkeuzelijst van de nieuwe resource die u zojuist hebt gemaakt. 
3. Bewerk de NuGet-pakketten van uw app-project in Visual Studio, en Microsoft.ApplicationInsights.WindowsServer toevoegen. (Of kies Microsoft.ApplicationInsights als u alleen de absoluut API, zonder de standaard telemetrielogboek siteverzameling modules.)
4. Stel de sleutel instrumentation in uw code:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*uw sleutel*`";` 

    of in ApplicationInsights.config (als u een van de standaard telemetrielogboek-pakketten hebt geïnstalleerd):
 
    `<InstrumentationKey>`*uw sleutel*`</InstrumentationKey>` 

    Als u ApplicationInsights.config gebruikt, Controleer of de eigenschappen in Solution Explorer zijn ingesteld op **Build Action = inhoud, kopiëren naar uitvoer Directory = kopiëren**.
5. [Gebruik de API](app-insights-api-custom-events-metrics.md) telemetrielogboek verzenden.
6. Uw app uitvoeren en Zie de telemetrielogboek in de bron die u hebt gemaakt in de Portal Azure.

## <a name="telemetry"></a>Voorbeeldcode

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Volgende stappen

* [Een dashboard maken](app-insights-dashboards.md)
* [Diagnostische gegevens zoeken](app-insights-diagnostic-search.md)
* [Aan de doelstellingen verkennen](app-insights-metrics-explorer.md)
* [Gebruiksanalyses query's schrijven](app-insights-analytics.md)
 
