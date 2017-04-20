<properties 
    pageTitle="Filteren en voorverwerking in de toepassing inzichten SDK | Microsoft Azure" 
    description="Schrijf Telemetrielogboek Processors en Telemetrielogboek begin voor de SDK om te filteren of eigenschappen toevoegen aan de gegevens voordat het telemetrielogboek wordt verzonden naar de toepassing inzichten-portal." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filteren en telemetrielogboek in de toepassing inzichten SDK voorverwerking

*Er is een toepassing inzichten in de proefversie.*

U kunt schrijven en -invoegtoepassingen voor toepassingen inzichten SDK om aan te passen hoe telemetrielogboek is vastgelegd en verwerkt voordat deze wordt verzonden naar de toepassing inzichten-service configureren. 

Deze functies zijn momenteel beschikbaar voor de ASP.NET-SDK.

* [Meting](app-insights-sampling.md) , wordt het volume van telemetrielogboek verminderd handhaven uw statistieken. Deze samen blijft gerelateerd gegevenspunten zodat u hiertussen bij het oplossen van een probleem kunt navigeren. Klik in de portal worden de totale aantallen vermenigvuldigd om dit voor de steekproef.
* [Filteren met Telemetrielogboek Processors](#filtering) kunt u selecteren of telemetrielogboek in de SDK aanpassen voordat deze wordt verzonden naar de server. U kunt bijvoorbeeld het volume van telemetrielogboek verminderen door aanvragen van robots uit te sluiten. Maar het filteren van een eenvoudige methode die u wilt verkleinen verkeer dan steekproeven is. Hiermee kunt u meer controle over wat wordt verzonden, maar moet u rekening moet houden dat dit gevolgen heeft voor uw statistieken - bijvoorbeeld als u alle succesvolle aanvragen uitfilteren.
* [Begin van de telemetrielogboek eigenschappen toevoegen](#add-properties) aan een telemetrielogboek verzonden vanuit uw app, inclusief telemetrielogboek van de standaard modules. Bijvoorbeeld, zou u berekende waarden; of versienummers waarmee de gegevens in de portal te filteren.
* [De SDK-API](app-insights-api-custom-events-metrics.md) wordt gebruikt om aangepaste gebeurtenissen en aan de doelstellingen te verzenden.


Voordat u begint:

* Installeer de [toepassing inzichten SDK voor ASP.NET v2](app-insights-asp-net.md) in uw app. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filteren: ITelemetryProcessor

Deze methode hebt u meer directe controle over wat is opgenomen of uitgesloten van de stream telemetrielogboek. U kunt deze gebruiken in combinatie met steekproeven, of afzonderlijk.

Als u wilt filteren telemetrielogboek, moet u een processor telemetrielogboek schrijven en met de SDK registeren. Alle telemetrielogboek doorloopt uw processor en kunt u kiezen om zet deze neer op van de stream of eigenschappen toevoegen. Dit geldt ook voor telemetrielogboek van de standaard modules zoals de HTTP-verzoek verzamelen en de afhankelijkheid verzamelen, maar ook telemetrielogboek die u zelf hebt geschreven. U kunt bijvoorbeeld telemetrielogboek over aanmeldingsaanvragen van robots of succesvolle afhankelijkheid gesprekken uitfilteren. 

> [AZURE.WARNING] Het telemetrielogboek verzonden vanuit de SDK filteren kunt met processors laten de statistieken die u in de portal ziet en moeilijk te volgen verwante items.
> 
> In plaats daarvan kunt u overwegen [steekproeven](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Een processor telemetrielogboek maken

1. Controleer of de toepassing inzichten SDK in uw project is versie 2.0.0 of hoger. Met de rechtermuisknop op uw project in Visual Studio Solution Explorer en kies NuGet-pakketten beheren. Schakel Microsoft.ApplicationInsights.Web in NuGet pakket manager.

1. U kunt een filter maken implementeren ITelemetryProcessor. Dit is een andere uitbreiden zoals telemetrielogboek-module, telemetrielogboek initialisatiefunctie en telemetrielogboek kanaal. 

    Zoals u ziet dat Telemetrielogboek Processors een reeks verwerking maken. Wanneer u een processor telemetrielogboek, moet u een koppeling naar de volgende processor in de keten doorgeven. Als een gegevenspunt telemetrielogboek wordt doorgegeven aan de methode proces, zijn werk doet en vervolgens de volgende Telemetrielogboek Processor u belt in de keten.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Hiermee voegt u in ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dit is dezelfde sectie waar u een filter steekproeven geïnitialiseerd.)

U kunt tekenreekswaarden uit het bestand .config doordat openbare benoemde eigenschappen in uw klas doorgeven. 

> [AZURE.WARNING] Handelingen kunt verrichten zodat deze overeenkomt met de typenaam van de en de namen van alle eigenschappen in het aan de klasse en de eigenschap namen in de code. Als het bestand .config verwijst naar een niet-bestaande type of de eigenschap, mislukken de SDK stilte eventuele telemetrielogboek verzenden.

 
**U kunt ook** kunt u het filter in code geïnitialiseerd. Voeg een geschikte initialisatieklasse - bijvoorbeeld AppStart in Global.asax.cs - uw processor op de keten:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients die zijn gemaakt nadat dit punt uw processors wordt gebruikt.

### <a name="example-filters"></a>Voorbeeld filters

#### <a name="synthetic-requests"></a>Synthetische aanvragen

Bots en web tests uitfilteren. Hoewel de doelstellingen Explorer u de optie kunt uitfilteren synthetische bronnen, minder verkeer door ze te filteren op de SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Mislukte verificatie

Uitfilteren aanvragen met een "401" reactie. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Snel externe afhankelijkheid oproepen uitfilteren

Als u alleen diagnose stellen bij oproepen die traag wilt zijn, snel weg te filteren. 

> [AZURE.NOTE] Hiermee wordt een verkeerd de statistieken die u op de portal ziet. De grafiek afhankelijkheid ziet er als wanneer de afhankelijkheid oproepen alle fouten worden.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Een diagnose stellen bij afhankelijkheidsproblemen

Een project om op te sporen afhankelijkheidsproblemen automatisch normale ping-opdrachten door naar te verzenden afhankelijkheden wordt beschreven in [Dit blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) .


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Eigenschappen toevoegen: ITelemetryInitializer

Gebruik telemetrielogboek begin algemene eigenschappen die worden verzonden met alle telemetrielogboek; definiëren en overschrijven van geselecteerde gedrag van de standaard telemetrielogboek modules. 

De toepassing inzichten voor webpakket verzamelt bijvoorbeeld telemetrielogboek over HTTP-aanvragen. Standaard worden gemarkeerd als een aanvraag met een reactiecode mislukt > = 400. Maar als u 400 behandelen als een gunstige uitkomst wilt, kunt u een telemetrielogboek initialisatiefunctie die de eigenschap Success opgeeft.

Als u een initialisatiefunctie telemetrielogboek opgeeft, wordt aangeroepen wanneer een van de methoden Track*() wordt genoemd. Dit geldt ook voor methoden aangeroepen door de standaard telemetrielogboek modules. Deze modules Stel bij afspraak niet alle eigenschappen die al is ingesteld door een initialisatiefunctie. 

**Uw initialisatiefunctie definiëren**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Uw initialisatiefunctie laden**

In ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*U kunt ook* kunt u de initialisatiefunctie in code, bijvoorbeeld in Global.aspx.cs maken:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Zie meer informatie in dit voorbeeld.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript telemetrielogboek begin

*JavaScript*

Voeg een initialisatiefunctie telemetrielogboek direct na de initialisatiecode die u hebt ontvangen van de portal: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Zie voor een overzicht van de niet-aangepaste eigenschappen beschikbaar op de telemetryItem, het [gegevensmodel](app-insights-export-data-model.md#lttelemetrytypegt).

U kunt zo veel begin als u wilt toevoegen. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor en ITelemetryInitializer

Wat is het verschil tussen telemetrielogboek processors en telemetrielogboek begin?

* Er zijn enkele overlapt in wat u met hen doen kunt: beide eigenschappen toevoegen aan telemetrielogboek kunnen worden gebruikt.
* TelemetryInitializers kan altijd worden uitgevoerd voordat u TelemetryProcessors.
* TelemetryProcessors kunt u volledig vervangen of verwijderen van een item telemetrielogboek.
* TelemetryProcessors verwerken niet prestatiemeteritem telemetrielogboek.



## <a name="persistence-channel"></a>Permanente kanaal 

Als uw app wordt uitgevoerd waarin de internetverbinding is niet altijd beschikbaar of traag, kunt u overwegen het kanaal permanente in plaats van het standaard in het geheugen kanaal. 

Het standaard in het geheugen kanaal verliest eventuele telemetrielogboek die niet zijn verzonden door de tijd die de app wordt gesloten. Hoewel u de beschikking over `Flush()` wilt verzenden in de buffer resterende gegevens, deze nog steeds verliest gegevens als er geen internetverbinding, of als de app wordt afgesloten voordat de overdracht voltooid is.

Het kanaal permanente buffers daarentegen telemetrielogboek in een bestand, voordat u deze verzendt bij de portal. `Flush()`zorgt ervoor dat de gegevens in het bestand is opgeslagen. Als gegevens niet door de tijd die de app wordt gesloten verzonden worden, blijft dit in het bestand. Wanneer de app opnieuw opstart, wordt de gegevens vervolgens verzonden als er een internetverbinding. Gegevens worden bij elkaar opgeteld in het bestand zo lang als nodig is een verbinding beschikbaar is. 

### <a name="to-use-the-persistence-channel"></a>Gebruik van het kanaal permanente

1. Het pakket NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3)importeren.
2. Deze code opnemen in uw app op een locatie geschikt initialisatie:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Gebruik `telemetryClient.Flush()` voordat u uw app wordt gesloten, om te controleren of gegevens wordt verzonden naar de portal of naar het bestand hebt opgeslagen.

    Houd er rekening mee dat Flush() synchroon voor het kanaal permanente, maar asynchroon voor andere kanalen is.

 
Het kanaal permanente is geoptimaliseerd voor apparaten scenario's, waarbij het aantal gebeurtenissen die zijn gemaakt met de toepassing relatief klein is en de verbinding is vaak onbetrouwbare. Dit kanaal wordt gebeurtenissen naar de schijf schrijven naar betrouwbaar opslag eerst en vervolgens probeert om het te verzenden. 

#### <a name="example"></a>Voorbeeld

Stel dat u wilt controleren onverwerkte uitzonderingen. U zich hebt geabonneerd de `UnhandledException` gebeurtenis. In de terugbellen, moet u een oproep naar uitlijnen om ervoor te zorgen dat de telemetrielogboek behouden is gebleven opnemen.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Wanneer de app wordt afgesloten, ziet u een bestand in `%LocalAppData%\Microsoft\ApplicationInsights\`, die de gecomprimeerde gebeurtenissen bevat. 
 
Volgende keer dat u deze toepassing, begint wordt het kanaal dit bestand verdergaan en telemetrielogboek geven inzicht krijgen in de toepassing als dat zo.

#### <a name="test-example"></a>Test-voorbeeld

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


De code van het kanaal permanente ligt op [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Verwijzing-documenten

* [API-overzicht](app-insights-api-custom-events-metrics.md)

* [ASP.NET-verwijzing](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK-Code

* [ASP.NET-Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript-SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Volgende stappen


* [Zoeken gebeurtenissen en Logboeken][diagnostic]
* [Steekproeven](app-insights-sampling.md)
* [Problemen oplossen][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
