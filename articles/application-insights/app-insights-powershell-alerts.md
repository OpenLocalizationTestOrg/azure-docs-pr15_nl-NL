<properties
    pageTitle="Powershell gebruiken voor het instellen van waarschuwingen in toepassing inzichten"
    description="Configuratie van toepassingen inzichten om e-mailberichten over metrische wijzigingen automatiseren."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>PowerShell gebruiken voor het instellen van waarschuwingen in toepassing inzichten

U kunt de configuratie van [waarschuwingen](app-insights-alerts.md) in [Visual Studio toepassing inzichten](app-insights-overview.md)automatiseren.

Bovendien kunt u [webhooks automatiseren uw antwoord naar een melding instellen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Eenmalige instelling

Als u PowerShell met uw Azure abonnement voordat u dit nog niet hebt gebruikt:

Installeer de Azure Powershell-module op de computer waarop de scripts worden uitgevoerd.

 * Installeer [Microsoft Web Platform Installer (v5 of hoger)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Microsoft Azure Powershell installeert


## <a name="connect-to-azure"></a>Verbinding maken met Azure

Start Azure PowerShell en [verbinding maken met uw abonnement](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Meldingen ontvangen

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Een waarschuwing toevoegen


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Voorbeeld 1

Stuur me een e-mail als antwoord op HTTP-aanvragen, meer dan 5 minuten, het gemiddelde berekend van de server lager dan 1 seconde is. Mijn resources toepassing inzichten heet IceCreamWebApp en is in de resourcegroep Fabrikam. Ik ben de eigenaar van de Azure abonnement.

De GUID is de abonnements-ID (niet de sleutel instrumentation van de toepassing).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Voorbeeld 2

Ik heb een toepassing waarin ik [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) gebruiken om te rapporteren van een meting met de naam "salesPerHour." Stuur dat een e-mail naar Mijn collega's als 'salesPerHour' onder 100, gemiddeld meer dan 24 uur.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Dezelfde regel kan worden gebruikt voor de meetwaarde met behulp van de [maat-parameter](app-insights-api-custom-events-metrics.md#properties) van een andere bijhouden gesprek zoals TrackEvent of trackPageView gerapporteerd.

## <a name="metric-names"></a>Metrische namen

De naam van de metrische | De schermnaam van het | Beschrijving
---|---|---
`basicExceptionBrowser.count`|Uitzonderingen in browser|Telling van onbekende uitzonderingen in de browser.
`basicExceptionServer.count`|Serveruitzonderingen|Aantal onverwerkte uitzonderingen die door de app
`clientPerformance.clientProcess.value`|Verwerking van clienttijd|Tijd tussen het ontvangen van de laatste byte van een document totdat het DOM is geladen. Asynchrone aanvragen mogelijk nog steeds worden verwerkt.
`clientPerformance.networkConnection.value`|Pagina laden netwerk verbinding maken met tijd| Duurt de browser verbinding maken met het netwerk. Mag 0 als in de cache.
`clientPerformance.receiveRequest.value`|Antwoord tijd ontvangen| Tijd tussen browser aanvraag wordt verzonden naar starten om antwoord te ontvangen.
`clientPerformance.sendRequest.value`|Verzoek om tijd te verzenden| De tijd die u hebt gemaakt door browser verzoek verzenden.
`clientPerformance.total.value`|Laadtijd voor pagina browser|De tijd op van de gebruikersaanvraag totdat DOM, opmaakmodellen, scripts en afbeeldingen worden geladen.
`performanceCounter.available_bytes.value`|Beschikbare geheugen|Fysiek geheugen onmiddellijk beschikbaar voor een proces of systeem gebruikt.
`performanceCounter.io_data_bytes_per_sec.value`|Proces IO rente|Totaal aantal bytes per seconde gelezen en geschreven naar bestanden, netwerk en apparaten.
`performanceCounter.number_of_exceps_thrown_per_sec`|uitzondering rente|Uitzonderingen per seconde.
`performanceCounter.percentage_processor_time.value`|CPU proces|Het percentage van de verstreken tijd van alle procesthreads gebruikt door de processor instructies voor het proces toepassingen.
`performanceCounter.percentage_processor_total.value`|Processortijd|Het percentage van de tijd waarop de processor bezig is in niet-actieve threads.
`performanceCounter.process_private_bytes.value`|Proces eigen bytes|Geheugen uitsluitend die zijn toegewezen aan de gecontroleerde toepassing processen.
`performanceCounter.request_execution_time.value`|Verwerkingstijd van ASP.NET-aanvraag|De tijd van de uitvoering van de meest recente aanvraag.
`performanceCounter.requests_in_application_queue.value`|ASP.NET-aanvragen in uitvoering wachtrij|Lengte van de wachtrij voor het aanvragen van toepassing.
`performanceCounter.requests_per_sec`|Tarief van ASP.NET-aanvraag|Rentepercentage van alle aanvragen voor de toepassing per seconde van ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Afhankelijkheid fouten|Het aantal mislukte oproepen die zijn aangebracht door de servertoepassing naar externe bronnen.
`request.duration`|Server antwoord tijd|Tijd tussen de ontvangst van een HTTP-aanvraag en afronden het antwoord wordt verzonden.
`request.rate`|Tarief aanvragen|Rentepercentage van alle aanvragen voor de toepassing per seconde.
`requestFailed.count`|Mislukte aanvragen|Tellen van HTTP-aanvragen waardoor er een antwoordcode > = 400
`view.count`|Paginaweergaven|Het aantal client gebruikersaanvragen voor een pagina met webonderdelen. Synthetische verkeer worden uitgefilterd.
{uw aangepaste metrische naam}|{Uw metrische naam}|Uw metrische waarde gemeld door [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) of in de [parameter van de afmetingen van een gesprek bijhouden](app-insights-api-custom-events-metrics.md#properties).

De doelstellingen worden door verschillende telemetrielogboek modules verzonden:

Metrische groep | Verzamelen module
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>weergave | [Browser JavaScript](app-insights-javascript.md)
performanceCounter | [Prestaties](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Afhankelijkheid](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
aanvraag<br/>requestFailed|[Serveraanvraag](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

U kunt [uw antwoord naar een melding te automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure wordt een webadres van uw keuze aangeroepen wanneer een melding wordt verheven.

## <a name="see-also"></a>Zie ook


* [Script om de toepassing inzichten configureren](app-insights-powershell-script-create-resource.md)
* [Toepassing inzichten en web test resources maken van sjablonen](app-insights-powershell.md)
* [Koppeling van Microsoft Azure diagnostische gegevens inzicht krijgen in toepassing automatiseren](app-insights-powershell-azure-diagnostics.md)
* [Uw antwoord naar een waarschuwing automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
