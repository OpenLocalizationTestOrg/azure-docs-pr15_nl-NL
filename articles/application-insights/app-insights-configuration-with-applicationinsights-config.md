<properties 
    pageTitle="Toepassing inzichten SDK configureren met ApplicationInsights.config of .xml" 
    description="In- of uitschakelen van gegevens verzamelen modules en prestatiemeteritems en andere parameters toevoegen" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>De toepassing inzichten SDK configureren met ApplicationInsights.config of .xml

De toepassing inzichten .NET SDK bestaat uit een aantal NuGet-pakketten. Het [pakket core](http://www.nuget.org/packages/Microsoft.ApplicationInsights) biedt de API voor het verzenden van telemetrielogboek inzicht krijgen in de toepassing. [Extra pakketten](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieden telemetrielogboek _modules_ en _begin_ voor het bijhouden van automatisch telemetrielogboek vanuit uw toepassing en de context. Door het configuratiebestand aanpassen, kunt u inschakelen of uitschakelen van telemetrielogboek modules en begin en parameters instellen voor een deel van deze.

Het configuratiebestand heet `ApplicationInsights.config` of `ApplicationInsights.xml`, afhankelijk van het type van de toepassing. Wordt deze automatisch toegevoegd aan uw project wanneer u [de meeste versies van de SDK installeren][start]. Wordt ook toegevoegd aan een web-app door [Statuscontrole op een IIS-server][redfield], of wanneer u de Appplication inzichten [-extensie voor een Azure-website of VM](app-insights-azure-web-apps.md)hebt geselecteerd.

Er is niet een equivalente-bestand om te bepalen de [SDK in een webpagina][client].

In dit document wordt beschreven in de secties die u in de configuratie, ziet bestand, hoe ze de onderdelen van de SDK, bepalen en welke NuGet-pakketten laden die onderdelen.

## <a name="telemetry-modules-aspnet"></a>Telemetrielogboek Modules (ASP.NET)

Elke module telemetrielogboek verzamelt van een bepaalde soort gegevens en de core API gebruikt om de gegevens te verzenden. De modules worden geïnstalleerd door verschillende NuGet-pakketten, die ook de vereiste regels aan het .config-bestand toevoegen.

Er is een knooppunt in het configuratiebestand voor elke module. Als u wilt uitschakelen in een module, het knooppunt verwijderen of dit opmerking.



### <a name="dependency-tracking"></a>Afhankelijkheid bijhouden

[Afhankelijkheid bijhouden](app-insights-dependencies.md) verzamelt telemetrielogboek over oproepen die uw app aanbrengt in databases en externe services en -databases. Als u wilt toestaan dat deze module om te werken in een IIS-server, moet u [Statuscontrole installeren][redfield]. Gebruik deze in Azure-WebApps of VMs, [selecteert u de toepassing inzichten extensie](app-insights-azure-web-apps.md).

U kunt ook uw eigen code met de [API TrackDependency](app-insights-api-custom-events-metrics.md#track-dependency)bijhouden afhankelijkheid schrijven.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-pakket.

### <a name="performance-collector"></a>Prestaties verzamelen

[Verzamelt systeem prestatiemeteritems](app-insights-performance-counters.md) zoals belasting van processor, geheugen en het netwerk van IIS-installaties. U kunt opgeven welke items te verzamelen, met inbegrip van prestatiemeteritems die u hebt ingesteld zelf.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-pakket.


### <a name="application-insights-diagnostics-telemetry"></a>Toepassing inzichten diagnostisch hulpprogramma Telemetrielogboek

De `DiagnosticsTelemetryModule` fouten in de toepassing inzichten instrumentation code zelf rapporten. Bijvoorbeeld als de code geen toegang de van prestatiemeteritems tot of als een `ITelemetryInitializer` een uitzondering. Doelcellen telemetrielogboek bijgehouden door deze module wordt weergegeven in de [Diagnostische zoeken][diagnostic]. Diagnostische gegevens verzendt naar dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen dit pakket hebt geïnstalleerd, wordt het bestand ApplicationInsights.config niet automatisch gemaakt. 

### <a name="developer-mode"></a>Modus voor ontwikkelaars

`DeveloperModeWithDebuggerAttachedTelemetryModule`forceert u inzicht krijgen in de toepassing `TelemetryChannel` gegevens onmiddellijk telemetrielogboek één item per keer, wanneer een foutopsporing is gekoppeld aan het toepassingsproces te verzenden. Hierdoor wordt de tijdsduur tussen het moment dat wanneer uw toepassing telemetrielogboek wordt bijgehouden en wanneer deze wordt weergegeven op de toepassing inzichten-portal. Hierdoor aanzienlijk realiseren in CPU en netwerk bandbreedte.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Toepassingsserver inzicht krijgen in Windows](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet pakket

### <a name="web-request-tracking"></a>Webaanvraag bijhouden

De [code voor het tijdstip en het resultaat van antwoord](app-insights-asp-net.md) van HTTP-aanvragen-rapporten. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet pakket

### <a name="exception-tracking"></a>Uitzondering bijhouden

`ExceptionTrackingTelemetryModule`nummers onverwerkte uitzonderingen in uw web-app. Zie [fouten en uitzonderingen][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet pakket


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-wordt bijgehouden [niet in acht genomen taak uitzonderingen](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-wordt bijgehouden onverwerkte uitzonderingen voor werknemer rollen, windows-services en consoletoepassingen.
* [Toepassingsserver inzicht krijgen in Windows](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-pakket.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Het pakket Microsoft.ApplicationInsights biedt de [core API](https://msdn.microsoft.com/library/mt420197.aspx) van de SDK. Gebruik deze opdracht de andere telemetrielogboek modules en u kunt ook [gebruiken om uw eigen telemetrielogboek definiëren](app-insights-api-custom-events-metrics.md).

* Geen vermelding in ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-pakket. Als u alleen dit NuGet hebt geïnstalleerd, wordt geen .config-bestand wordt gegenereerd.

## <a name="telemetry-channel"></a>Telemetrielogboek kanaal

Het kanaal telemetrielogboek beheert buffer en de overdracht van telemetrielogboek bij de toepassing inzichten-service. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`is het standaardkanaal voor services. Deze buffers gegevens in het geheugen.
* `Microsoft.ApplicationInsights.PersistenceChannel`is een alternatief voor consoletoepassingen. Dit kunt unflushed gegevens opslaan op permanente opslagruimte wanneer uw app afgesloten en deze verzendt wanneer de app opnieuw wordt gestart.


## <a name="telemetry-initializers-aspnet"></a>Telemetrielogboek begin (ASP.NET)

Telemetrielogboek begin instellen contexteigenschappen die zijn verzonden, samen met alle items van telemetrielogboek. 

U kunt [uw eigen begin schrijven](app-insights-api-filtering-sampling.md#add-properties) als contexteigenschappen wilt instellen.

De standaard begin is ingesteld op basis van het Web of in een WindowsServer NuGet-pakketten:


* `AccountIdTelemetryInitializer`Hiermee stelt u de eigenschap AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Hiermee stelt u de eigenschap AuthenticatedUserId als instellen door de JavaScript-SDK.
* `AzureRoleEnvironmentTelemetryInitializer`updates voor de `RoleName` en `RoleInstance` eigenschappen van de `Device` context voor alle telemetrielogboek artikelen met informatie opgehaald uit de Azure runtime-omgeving.
* `BuildInfoConfigComponentVersionTelemetryInitializer`updates voor de `Version` eigenschap van het `Component` context voor alle telemetrielogboek items met de waarde opgehaald uit de `BuildInfo.config` dat is gemaakt door MS Build.
* `ClientIpHeaderTelemetryInitializer`updates `Ip` eigenschap van het `Location` context van alle telemetrielogboek items op basis van de `X-Forwarded-For` HTTP koptekst van de aanvraag kunt invullen.
* `DeviceTelemetryInitializer`updates van de volgende eigenschappen van de `Device` context voor alle items met telemetrielogboek.
 - `Type`is ingesteld op "PC"
 - `Id`is ingesteld op de domeinnaam van de computer waarop de webtoepassing wordt uitgevoerd.
 - `OemName`is ingesteld op de waarde opgehaald uit de `Win32_ComputerSystem.Manufacturer` veld met WMI.
 - `Model`is ingesteld op de waarde opgehaald uit de `Win32_ComputerSystem.Model` veld met WMI.
 - `NetworkType`is ingesteld op de waarde opgehaald uit de `NetworkInterface`.
 - `Language`is ingesteld op de naam van de `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`updates voor de `RoleInstance` eigenschap van het `Device` context voor alle telemetrielogboek artikelen met de naam van het domein van de computer waarop de webtoepassing wordt uitgevoerd.
* `OperationNameTelemetryInitializer`updates de `Name` eigenschap van het `RequestTelemetry` en de `Name` eigenschap van het `Operation` context van alle telemetrielogboek items op basis van de HTTP-methode, evenals de namen van ASP.NET MVC controller en actie die wordt gestart te verwerken van het verzoek.
* `OperationIdTelemetryInitializer`of `OperationCorrelationTelemetryInitializer` updates de `Operation.Id` contexteigenschap van alle telemetrielogboek items bijgehouden tijdens het verwerken van een nieuw vergaderverzoek met de automatisch gegenereerde `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`updates voor de `Id` eigenschap van het `Session` context voor alle telemetrielogboek items met de waarde opgehaald uit de `ai_session` cookie gegenereerd door het ApplicationInsights JavaScript-instrumentation-code die wordt uitgevoerd in de browser van de gebruiker. 
* `SyntheticTelemetryInitializer`of `SyntheticUserAgentTelemetryInitializer` updates de `User`, `Session` en `Operation` contexten eigenschappen van alle telemetrielogboek items bij het verwerken van een verzoek om van een synthetische bron, zoals een beschikbaarheid testen of zoek engine bots bijgehouden. Standaard worden [Aan de doelstellingen Explorer](app-insights-metrics-explorer.md) geen synthetische telemetrielogboek weergegeven. 

    De `<Filters>` instellen eigenschappen van de aanvragen identificeren.
* `UserAgentTelemetryInitializer`updates voor de `UserAgent` eigenschap van het `User` context van alle telemetrielogboek items op basis van de `User-Agent` HTTP koptekst van de aanvraag kunt invullen.
* `UserTelemetryInitializer`updates voor de `Id` en `AcquisitionDate` eigenschappen van `User` context voor alle items met telemetrielogboek met waarden opgehaald uit de `ai_user` cookie gegenereerd door de toepassing inzichten JavaScript-instrumentation-code in de browser van de gebruiker uitgevoerd.
* `WebTestTelemetryInitializer`Hiermee stelt u de gebruikers-id, de sessie-id en de eigenschappen van synthetische gegevensbron voor HTTP-aanvragen die afkomstig van de [beschikbaarheid van de tests zijn](app-insights-monitor-web-app-availability.md).
De `<Filters>` instellen eigenschappen van de aanvragen identificeren.

## <a name="telemetry-processors-aspnet"></a>Telemetrielogboek Processors (ASP.NET)

Telemetrielogboek processors kunnen filteren en aanpassen van elk item telemetrielogboek net voordat deze is verzonden vanuit de SDK bij de portal.

U kunt [uw eigen processors telemetrielogboek schrijven](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Geavanceerde steekproeven telemetrielogboek processor (van 2.0.0-beta3)

Dit is standaard ingeschakeld. Als uw app een groot aantal telemetrielogboek stuurt, deze processor Hiermee verwijdert u delen van de afbeelding.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

De parameter biedt het doel dat het algoritme probeert te bereiken. Elk exemplaar van de SDK werkt belooft zodat als uw server een cluster van verschillende computers is, de werkelijke hoeveelheid telemetrielogboek dienovereenkomstig gewijzigd moet worden vermenigvuldigd.

[Meer informatie over steekproeven](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Vaste rente steekproeven telemetrielogboek processor (van 2.0.0-beta1)

Er is ook een standaard [steekproeven telemetrielogboek processor](app-insights-api-filtering-sampling.md#sampling) (2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanaalparameters (Java)

Deze parameters invloed op hoe de SDK Java moet opslaan en de telemetrielogboek-gegevens die worden verzameld leegmaken.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Het aantal telemetrielogboek items die kunnen worden opgeslagen in een van de SDK in het geheugen opslag. Wanneer dit nummer is bereikt, wordt deze telemetrielogboek wordt geleegd - dat wil zeggen de telemetrielogboek items worden verzonden naar de toepassing inzichten-server.

-   Min: 1
-   Max: 1000
-   Standaard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Bepaalt hoe vaak de gegevens die zijn opgeslagen in de opslagruimte in het geheugen moet worden leeggemaakt (verzonden inzicht krijgen in toepassingen).

-   Min: 1
-   Max: 300
-   Standaard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Bepaalt de maximale grootte in MB die is toegewezen aan de permanente opslag op de lokale schijf. Deze opslag wordt gebruikt voor persistent maken telemetrielogboek items die niet kon worden verzonden naar het eindpunt van de toepassing inzichten. Wanneer de opslaggrootte is voldaan, worden nieuwe telemetrielogboek items genegeerd.

-   Min: 1
-   Max: 100
-   Standaard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Hiermee bepaalt u de toepassing inzichten resource waarin uw gegevens worden weergegeven. Gewoonlijk maakt u een afzonderlijke resource, met een afzonderlijke sleutel, voor elk van uw toepassingen.

Als u de sleutel dynamisch - bijvoorbeeld instellen wilt als u resultaten wilt verzenden vanuit uw toepassing naar verschillende bronnen - kunt u de sleutel uit het configuratiebestand weglaat en stel deze in de code in plaats daarvan.

Als u wilt de sleutel instellen voor alle exemplaren van TelemetryClient, instellen standaard telemetrielogboek modules, inclusief de sleutel in TelemetryConfiguration.Active. Ga als volgt in een initialisatiemethode, zoals global.aspx.cs in een ASP.NET-service:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Als u alleen een specifieke verzameling gebeurtenissen naar een andere resource stuurt wilt, kunt u de toets instellen voor een specifieke TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Meer informatie over de API][api].

Om een nieuwe sleutel, [maakt u een nieuwe bron in de portal toepassing inzichten][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

