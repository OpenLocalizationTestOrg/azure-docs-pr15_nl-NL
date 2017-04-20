<properties 
    pageTitle="Problemen oplossen en vragen over de toepassing inzichten" 
    description="Iets in Visual Studio-toepassing inzichten duidelijk zijn en werkt niet? Kunt u hier." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Vragen - toepassing inzichten voor ASP.NET

## <a name="configuration-problems"></a>Configuratieproblemen

*Ik heb problemen met het instellen van mijn:*

* [.NET-app](app-insights-asp-net-troubleshoot-no-data.md)
* [Een app al actief bewaken](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure diagnostische gegevens](app-insights-azure-diagnostics.md)
* [Java WebApp](app-insights-java-troubleshoot.md)
* [Andere platforms](app-insights-platforms.md)

*Ik ophalen geen gegevens uit mijn server*

* [Uitzonderingen van de firewall instellen](app-insights-ip-addresses.md)
* [Stel een ASP.NET-server](app-insights-monitor-performance-live-website-now.md)
* [Een Java-server instellen](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Kan ik inzicht in de toepassing met... gebruiken?

[Zie Platforms][platforms]


## <a name="is-it-free"></a>Is dit gratis?

* Ja, als u ervoor de gratis [prijzen van laag kiest](app-insights-pricing.md). U krijgt een vriendelijke quota van gegevens en de meeste functies. 
* U moet uw creditcardgegevens registreren bij Microsoft Azure opgeven, maar geen kosten komen tenzij u een andere dat is betaald voor Azure service gebruiken, of u expliciet naar een betaling laag bijwerkt.
* Als uw app meer gegevens dan de maandelijkse quota voor de gratis laag stuurt, wordt het stopgezet aan te melden. Als dat gebeurt, kunt u kiest om te beginnen betaalt of wacht totdat het quotum is opnieuw aan het einde van de maand.
* Eenvoudige gegevens voor gebruik en sessie is niet onderhevig aan een quotum.
* Er is ook een gratis proefabonnement van 30 dagen, waarin u de gratis betaalde functies krijgt.
* Elke resource van toepassing een afzonderlijke quotum heeft en u de prijzen laag onafhankelijk van andere instellen.

#### <a name="what-do-i-get-if-i-pay"></a>Wat vind ik als ik betalen?

* Een grotere [maandelijkse quotum van gegevens](https://azure.microsoft.com/pricing/details/application-insights/).
* Optie om te betalen gebruikskosten kunt doorgaan met gegevens verzamelen via het quotum voor maandelijkse. Als uw gegevens is gewijd aan quotum, bent u btw geheven per Mb.
* [Doorlopend exporteren](app-insights-export-telemetry.md).


## <a name="q14"></a>Wat wordt toepassing inzichten gewijzigd in mijn project?

De details, hangt af van het type project. Voor een webtoepassing:


+ Deze bestanden toegevoegd aan uw project:

 + ApplicationInsights.config. 
 + AI.js


+ Deze NuGet-pakketten is geïnstalleerd:

 -  *Toepassing inzichten API* - de core-API

 -  *Toepassing inzichten API voor webtoepassingen* - gebruikt voor het verzenden van telemetrielogboek van de server

 -  *Toepassing inzichten API voor JavaScript-toepassingen* - gebruikt voor het verzenden van telemetrielogboek van de client

    De pakketten bevatten deze samenstellen:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Hiermee voegt u items in:

 - Web.config

 - Packages.config

+ (Nieuwe projecten alleen - als u met het [toevoegen van toepassing inzichten aan een bestaand project][start], moet u dit handmatig doen.) Hiermee voegt u fragmenten in de code clients en servers geïnitialiseerd ze de toepassing inzichten resource-ID. Bijvoorbeeld wordt code in een MVC-app ingevoegd in de basispagina Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hoe kan ik upgraden vanuit oudere versies van SDK?

Zie de [release-informatie](app-insights-release-notes.md) voor de SDK die geschikt zijn voor het type van toepassing. 



## <a name="update"></a>Hoe kan ik welke Azure resource mijn project gegevens worden verzonden wijzigen?

Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kies **Update toepassing inzichten**. U kunt de gegevens verzenden naar een bestaand of nieuw bron in Azure wordt aangegeven. De wizard update verandert de sleutel instrumentation in ApplicationInsights.config, waarmee wordt bepaald waar uw gegevens in de server SDK wordt verzonden. Tenzij u "Alles bijwerken" uitschakelt, worden deze ook de sleutel die waar deze wordt weergegeven in uw webpagina's aanpassen.


#### <a name="data"></a>Hoe lang wordt gegevens opgeslagen in de portal? Is het veilig?

Eens kijken [Gegevensretentie en Privacy][data].

## <a name="logging"></a>Logboekregistratie

#### <a name="post"></a>Hoe kan ik POST-gegevens in diagnostische zoeken zien?

We niet POST-gegevens automatisch melden, maar u kunt een oproep TrackTrace: de gegevens in de parameter bericht plaatsen. Dit heeft een langere bestandslimiet dan de beperkingen voor de tekenreekseigenschappen van de, hoewel u kunt niet op is geïnstalleerd filteren. 

## <a name="security"></a>Beveiliging

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Zijn mijn gegevens veilig in de portal? Hoe lang is deze bewaard?

Zie [gegevens bewaren en Privacy][data].


## <a name="q17"></a>Heb ik alles in de toepassing inzichten ingeschakeld?

<table border="1">
<tr><th>Wat u ziet</th><th>Hoe kom ik aan dit</th><th>Waarom u deze wilt</th></tr>
<tr><td>Beschikbaarheid van grafieken</td><td><a href="../app-insights-monitor-web-app-availability/">Web tests</a></td><td>Weet dat uw web-app is omhoog</td></tr>
<tr><td>Server app perf: antwoord tijden,...
</td><td><a href="../app-insights-asp-net/">Toepassing inzichten toevoegen aan uw project</a><br/>of <br/><a href="../app-insights-monitor-performance-live-website-now/">Li statuscontrole installeren op server</a> (of uw eigen programmacode schrijven voor het <a href="../app-insights-api-custom-events-metrics/#track-dependency">bijhouden van afhankelijkheden</a>)</td><td>Perf problemen opsporen</td></tr>
<tr><td>Afhankelijkheid telemetrielogboek</td><td><a href="../app-insights-monitor-performance-live-website-now/">Li statuscontrole installeren op server</a></td><td>Een diagnose stellen bij problemen met databases of andere externe onderdelen</td></tr>
<tr><td>Stapel sporen ophalen uit uitzonderingen</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">TrackException oproepen invoegen in uw code</a> (maar enkele automatisch worden gemeld)</td><td>Ontdekken en uitzonderingen analyseren</td></tr>
<tr><td>Zoeken log sporen</td><td><a href="../app-insights-search-diagnostic-logs/">Een adapter logboekregistratie toevoegen</a></td><td>Een diagnose stellen bij uitzonderingen, perf problemen</td></tr>
<tr><td>Basisbeginselen van client-gebruik: paginaweergaven, sessies,...</td><td><a href="../app-insights-javascript/">JavaScript-initialisatiefunctie in webpagina 's</a></td><td>Gebruiksanalyses</td></tr>
<tr><td>De aangepaste doelstellingen client</td><td><a href="../app-insights-api-custom-events-metrics/">Bijhouden u belt in webpagina 's</a></td><td>Gebruikerservaring verbeteren</td></tr>
<tr><td>Aangepaste Maatstelsel van server</td><td><a href="../app-insights-api-custom-events-metrics/">Gesprekken bijhouden in servercode</a></td><td>Bedrijfsinformatie</td></tr>
</table>


## <a name="automation"></a>Automatisering

U kunt de [PowerShell-scripts schrijven](app-insights-powershell.md) als u wilt maken en bijwerken van toepassing inzichten resources.

## <a name="more-answers"></a>Meer informatie

* [Toepassing inzichten-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 