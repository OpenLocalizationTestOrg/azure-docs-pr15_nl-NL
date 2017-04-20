<properties 
    pageTitle="Releaseopmerkingen voor toepassing inzichten voor Windows" 
    description="De meest recente updates voor Windows Store SDK." 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Releaseopmerkingen voor toepassing inzichten SDK voor Windows Phone en op te slaan

De toepassing inzichten SDK verzendt telemetrielogboek over uw live-app naar [Toepassing inzichten](https://azure.microsoft.com/services/application-insights/), waar kunt u de gebruik en analyseren.


## <a name="version-111"></a>Versie 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Verhelp tijdens vastlopen een loopt vast bij gebruik van de Windows Phone-Silverlight SDK. Na deze wijziging eventuele vastlopen die later dan ~ 2 seconden na de oproep door naar WindowsAppInitialier.InitializeAsync(...) gebeurt gehandhaafd voor schijf en het volgende wordt verzonden wanneer de app wordt gestart. Als er vastlopen voordat ~ 2 seconden nadat het gesprek gebeurt, wordt deze genegeerd.  
- Instellen van de NuGet afhankelijkheden naar een specifieke versie van de kern en Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Core SDK

- Core wordt beheerd in github. Toekomstige releaseopmerkingen van de SDK Core vindt u [in github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Versie 1.1

### <a name="core-sdk"></a>Core SDK

- SDK wordt nu kort beschreven nieuwe telemetrielogboek type ```DependencyTelemetry``` die bevat informatie over afhankelijkheid gesprek van toepassing
- Nieuwe methode ```TelemetryClient.TrackDependency``` kunt kunt u informatie over afhankelijkheid oproepen verzenden van toepassing

## <a name="version-100"></a>Versie 1.0.0

### <a name="windows-app-sdk"></a>Windows-App SDK

- Nieuwe initialisatie voor Windows-Apps. Nieuwe `WindowsAppInitializer` klasse met `InitializeAsync()` methode kunt voor bootstraps initialisatie van de siteverzameling SDK. Deze wijziging toestaan meer controle en prestatieverbeteringen voor aanzienlijk app initialisatie via vorige ApplicationInsights.config-methode.
- DeveloperMode is niet langer automatisch ingesteld. U moet in code opgeven als DeveloperMode gedrag wilt wijzigen.
- NuGet pakket invoegt niet langer ApplicationInsights.config. Aanbevolen gebruik van de nieuwe WindowsAppInitializer bij het toevoegen van handmatig NuGet-pakket.
- ApplicationInsights.config alleen leest `<InstrumentationKey>`, alle andere instellingen in de voorkeuren voor WindowsAppInitializer instellingen worden genegeerd.
- Store Market worden automatisch die worden verzameld door SDK.
- Een groot aantal correcties, verbeteringen in stabiliteit en prestatieverbeteringen.

### <a name="core-sdk"></a>Core SDK

- ApplicationInsights.config bestand is niet langer requiered. En worden niet toegevoegd door het pakket NuGet. Configuratie kunt volledig in code worden opgegeven.
- NuGet pakket wordt niet langer een doelbestand toevoegen aan uw oplossing. Hiermee verwijdert u de automatische instelling van DeveloperMode tijdens een foutopsporing opbouwen. DeveloperMode moet handmatig worden ingesteld in de code.

## <a name="version-017"></a>Versie 0.17

### <a name="windows-app-sdk"></a>Windows-App SDK

- Windows-App SDK ondersteunt nu universele Windows Apps gemaakt ten opzichte van Windows 10 technical preview en met tegenover 2015 RC.

### <a name="core-sdk"></a>Core SDK

- TelemetryClient standaardwaarden geïnitialiseerd de InMemoryChannel.
- Nieuwe API toegevoegd, `TelemetryClient.Flush()`. Deze methode uitlijnen wordt geactiveerd of van alle telemetrielogboek aangemeld op die client onmiddellijke blokkeren uploaden. Hiermee worden handmatige activeert is geüpload voordat de proces wordt afgesloten.
- NuGet pakket toegevoegd een doel .net 4.5. Dit doel heeft geen externe afhankelijkheden (verwijderde BCL en EventSource afhankelijkheden).

## <a name="version-016"></a>Versie 0,16 

2015-04-28 preview

- SDK ondersteunt nu DNX doelplatform om in te schakelen voor controle van [Core .NET framework](http://www.dotnetfoundation.org/NETCore5) -toepassingen.
- Exemplaren van ```TelemetryClient``` Instrumentation sleutel niet meer in cache. Nu als instrumentation sleutel is niet ingesteld in ```TelemetryClient``` expliciet ```InstrumentationKey``` waarde null. Deze corrigeert een probleem wanneer u instelt ```TelemetryConfiguration.Active.InstrumentationKey``` nadat sommige telemetrielogboek is al die worden verzameld, telemetrielogboek modules zoals afhankelijkheid verzamelen, web aanvragen gegevens verzamelen en prestaties items verzamelen nieuwe instrumentation sleutel wordt gebruikt.

## <a name="version-015"></a>Versie 0,15

- Nieuwe eigenschap ```Operation.SyntheticSource``` nu beschikbaar op ```TelemetryContext```. U kunt nu uw telemetrielogboek-items markeren als "niet een echte gebruikersverkeer" en opgeven hoe dit verkeer is gegenereerd. U kunt als voorbeeld met deze eigenschap verkeer onderscheiden van uw automatisering test uit laden test-verkeer is toegestaan.
- Kanaal logica is verplaatst naar de afzonderlijke NuGet Microsoft.ApplicationInsights.PersistenceChannel genoemd. Standaardkanaal heet nu InMemoryChannel
- Nieuwe methode ```TelemetryClient.Flush``` kunt telemetrielogboek items synchroon uit de buffer leegmaken

## <a name="version-013"></a>Versie 0,13

Geen releaseopmerkingen voor oudere versies beschikbaar. 
