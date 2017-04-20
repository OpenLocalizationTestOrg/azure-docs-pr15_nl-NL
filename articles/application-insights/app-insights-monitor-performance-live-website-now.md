<properties
    pageTitle="Een diagnose stellen bij prestatieproblemen op een actieve IIS-website | Microsoft Azure"
    description="Van een website prestaties controleren zonder deze opnieuw te implementeren. Zelfstandige gebruiken of met de toepassing inzichten SDK om afhankelijkheid telemetrielogboek."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="awills"/>


# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Instrument WebApps tijdens runtime met de toepassing inzichten

*Er is een toepassing inzichten in de proefversie.*

U kunt een live WebApp met inzicht in de Visual Studio-toepassing instrument zonder te wijzigen of implementeer deze opnieuw uw code. In uw apps worden gehost door een on-premises implementatie IIS-server, installeert u statuscontrole; of als ze Azure-WebApps bent of die worden uitgevoerd in een VM Azure, kunt u de toepassing inzichten extensie installeren. (Er zijn ook afzonderlijk artikelen over het implementeren van [live J2EE WebApps](app-insights-java-live.md) en [Azure-Cloudservices](app-insights-cloudservices.md).)

![voorbeeld-grafieken](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Hebt u een keuze van drie routes toepassing inzichten toepassen op uw .NET-webtoepassingen:

* **Tijd gemaakt:** [De toepassing inzichten SDK toevoegen] [greenbrown] aan uw web app-code. 
* **Runtime:** Instrument uw web-app op de server, zoals hieronder, zonder PowerPoint opnieuw moet maken en opnieuw te distribueren de code beschreven.
* **Beide:** De SDK maken in uw web app-code en de runtime-extensies ook van toepassing. Profiteer van het beste van beide opties. 

Hier volgt een overzicht van wat u door elke route openen:

||Tijd gemaakt|Runtime|
|---|---|---|
|Aanvragen en uitzonderingen|Ja|Ja|
|[Meer gedetailleerde uitzonderingen](app-insights-asp-net-exceptions.md)||Ja|
|[Afhankelijkheid diagnostische gegevens](app-insights-asp-net-dependencies.md)|Klik op .NET 4.6 +|Ja|
|[Systeem prestatie-items](app-insights-performance-counters.md)||IIS of Azure cloudservice binnenkomt, niet Azure WebApp|
|[API voor aangepaste telemetrielogboek][api]|Ja||
|[Doelcellen log-integratie](app-insights-asp-net-trace-logs.md)|Ja||
|[Paginagegevens gebruiker & weergeven](app-insights-javascript.md)|Ja||
|Niet nodig om code opnieuw te maken|Nee||


## <a name="instrument-your-web-app-at-run-time"></a>Uw web-app instrument tijdens runtime

Moet u een [Microsoft Azure](http://azure.com) -abonnement.

### <a name="if-your-app-is-an-azure-web-app-or-cloud-service"></a>Als uw app een Azure WebApp of Service van de Cloud is

* Selecteer toepassing inzichten in het Configuratiescherm van de app in Azure wordt aangegeven. 

    [Meer informatie](app-insights-azure.md).

### <a name="if-your-app-is-hosted-on-your-iis-server"></a>Als uw app wordt gehost op uw IIS-server

1. Klik op de IIS-web-server, meld u aan met beheerdersreferenties.
2. Download en voert u het [installatieprogramma van de statuscontrole](http://go.microsoft.com/fwlink/?LinkId=506648).
4. In de installatiewizard, moet u zich aanmelden bij Microsoft Azure.

    ![Meld u aan bij Azure met de referenties van uw Microsoft-account](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Fouten? Zie [problemen met](#troubleshooting).*

5. Kies de geïnstalleerde webtoepassing of website die u wilt controleren, moet u de resource die u wilt de resultaten bekijken in de portal-toepassing inzichten configureren.

    ![Kies een app en een resource.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    Normaal gesproken vergt u kiest voor het configureren van een nieuwe resource en [resourcegroep][roles].

    Een bestaande bron anders gebruiken als u al ingesteld [web getest] [ availability] voor uw site of [WebClient monitoring][client].

6. IIS opnieuw starten.

    ![Kies opnieuw beginnen bij het begin van het dialoogvenster.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Uw webservice wordt even worden onderbroken.

6. Zoals u ziet dat ApplicationInsights.config is ingevoegd in de web-apps die u wilt controleren.

    ![Zoek het bestand .config samen met de codebestanden van de web-app.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   Er zijn ook enkele wijzigingen in het web.config.

#### <a name="want-to-reconfigure-later"></a>Wilt u configureren later (opnieuw)?

Nadat u de wizard hebt voltooid, kunt u de agent opnieuw configureren wanneer u maar wilt. U kunt dit ook gebruiken als u de agent hebt geïnstalleerd, maar er sommige problemen met de eerste configuratie is.

![Klik op het pictogram inzichten van toepassing op de taakbalk](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)


## <a name="view-performance-telemetry"></a>Weergave prestaties telemetrielogboek

Meld u aan bij [de portal van Azure](https://portal.azure.com), blader toepassing inzichten en opent u de resource die u hebt gemaakt.

![Kies Bladeren, toepassing inzichten, en selecteert u uw app](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Open het blad prestaties als u wilt zien van de aanvraag, antwoord tijd, afhankelijkheid en andere gegevens.

![Prestaties](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Klik op een grafiek om een gedetailleerde weergave te openen.

U kunt [bewerken, rangschikken, opslaan](app-insights-metrics-explorer.md), en dit vastmaken grafieken of het hele blad aan een [dashboard](app-insights-dashboards.md).

## <a name="dependencies"></a>Afhankelijkheden

De grafiek afhankelijkheid duur wordt de tijd weergegeven die u hebt gemaakt door het aanroepen van uw app externe onderdelen zoals databases, REST API's of Azure-blobopslag.

De grafiek door oproepen naar verschillende afhankelijkheden segmenten: de grafiek bewerken, inschakelen groeperen en vervolgens groeperen op afhankelijkheid, afhankelijkheidstype of afhankelijkheid prestaties.

![Afhankelijkheid](./media/app-insights-monitor-performance-live-website-now/23-dep.png)

## <a name="performance-counters"></a>Prestatie-items 

(Niet voor Azure Webapps.) Klik op Servers op het blad Overzicht om grafieken van server prestatiemeteritems zoals CPU-gebruik voor welke en geheugen weer te geven.

Als u meerdere exemplaren van de server hebt, wilt u mogelijk de grafieken om te groeperen op rol exemplaar bewerken.

![Servers](./media/app-insights-monitor-performance-live-website-now/22-servers.png)

U kunt ook [de set prestatiemeteritems die door de SDK worden gemeld wijzigen](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3). 


## <a name="exceptions"></a>Uitzonderingen

![Klik op de server uitzonderingen grafiek](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

U kunt inzoomen op specifieke uitzonderingen (vanuit de afgelopen zeven dagen) en u stapel sporen en contextgegevens.

## <a name="sampling"></a>Steekproeven

Als uw toepassing een groot aantal gegevens stuurt en u de toepassing inzichten SDK voor ASP.NET versie 2.0.0-beta3 of hoger gebruikt, kan de functie Geavanceerde steekproeven werken en slechts een percentage van uw telemetrielogboek verzenden. [Meer informatie over steekproeven.](app-insights-sampling.md)


## <a name="troubleshooting"></a>Problemen oplossen

### <a name="connection-errors"></a>Fouten

U moet [enkele uitgaande poorten](app-insights-ip-addresses.md#outgoing-ports) openen in de firewall van de server toe te staan dat statuscontrole om te werken.

### <a name="no-telemetry"></a>Geen telemetrielogboek?

  * Gebruik uw site om bepaalde gegevens te genereren.
  * Wacht enkele minuten als u wilt dat de gegevens binnenkomen en klik vervolgens op **vernieuwen**.
  * Open diagnostische gegevens zoeken (de zoeken-tegel) om afzonderlijke gebeurtenissen weer te geven. Gebeurtenissen zijn vaak zichtbaar in de diagnostische zoeken voordat statistische gegevens worden weergegeven in de grafieken.
  * Statuscontrole openen en selecteer uw toepassing in het linkerdeelvenster. Controleer of er nog een diagnostisch hulpprogramma berichten van deze toepassing in de sectie 'Configuratie meldingen':

  ![Open het blad prestaties als u wilt zien van de aanvraag, antwoord tijd, afhankelijkheid en andere gegevens](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Controleer of dat uw server-firewall uitgaand verkeer op de bovenstaande poorten toestaat.
  * Klik op de server als u een bericht over 'onvoldoende machtigingen' ziet, probeert u het volgende:
    * Selecteer uw groep van toepassingen in IIS-beheer, open **Geavanceerde instellingen**en onder **Process Model** ziet u de identiteit.
    * In het Configuratiescherm van de Computer management toevoegen deze identiteit aan de groep gebruikers van prestaties bewaken.
  * Als er MMA/SCOM geïnstalleerd op uw server, kunnen sommige versies conflicteren. Verwijder zowel SCOM als statuscontrole en opnieuw installeren van de meest recente versies.
  * Zie [problemen met de][qna].

## <a name="system-requirements"></a>Systeemvereisten

Ondersteuning voor OS voor toepassing inzichten statuscontrole op Server:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows server 2012 R2

met de meest recente SP en .NET Framework 4.0 en 4.5

Aan de clientzijde Windows 7, 8 en 8.1, opnieuw met .NET Framework 4.0 en 4.5

IIS-ondersteuning is: IIS 7: 7.5 hebt, 8, 8,5 (IIS is vereist)

## <a name="automation-with-powershell"></a>Automatisering met PowerShell

U kunt starten en stoppen met het bewaken via PowerShell op uw IIS-server.

Eerst de module toepassing inzichten te importeren:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Ontdek welke apps zijn wordt gecontroleerd:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name`(Optioneel) De naam van een web-app.
* Toont de toepassing inzichten controle status voor elke web-app (of de benoemde app) in deze IIS-server.

* Geeft als resultaat `ApplicationInsightsApplication` voor elke app:
 * `SdkState==EnabledAfterDeployment`:-App moet worden gecontroleerd en is geïmplementeerd tijdens runtime, door het hulpprogramma statuscontrole, of door `Start-ApplicationInsightsMonitoring`.
 * `SdkState==Disabled`: De app is niet geïmplementeerd voor inzichten van toepassing. Dit is nooit geïmplementeerd of runtime-cmdlets voor controle is uitgeschakeld met het hulpmiddel statuscontrole of met `Stop-ApplicationInsightsMonitoring`.
 * `SdkState==EnabledByCodeInstrumentation`: De app is geïmplementeerd door de SDK toe te voegen aan de broncode. De SDK kan niet worden bijgewerkt of gestopt.
 * `SdkVersion`ziet u de versie wordt gebruikt voor het controleren van deze app.
 * `LatestAvailableSdkVersion`ziet u de versie die momenteel beschikbaar in de galerie NuGet. Voor informatie over het upgraden van de app in deze versie gebruiken `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name`De naam van de app in IIS
* `-InstrumentationKey`De ikey van de toepassing inzichten resource waar u de resultaten moet worden weergegeven.

* Deze cmdlet geldt alleen voor apps die worden niet al geïmplementeerd - dat wil zeggen SdkState == NotInstrumented.

    De cmdlet heeft geen invloed op een app die al is geïmplementeerd, tegelijk opbouwen door de SDK toe te voegen aan de code of tijdens het uitvoeren van een vorige gebruik van deze cmdlet.

    De SDK-versie waarmee de app instrument is de versie die onlangs met deze server is gedownload.

    Als u wilt downloaden van de meest recente versie, gebruikt u de Update-ApplicationInsightsVersion.

* Geeft als resultaat `ApplicationInsightsApplication` op succes. Als dit mislukt, aanmelding een spoor stderr.

    
          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name`De naam van een IIS-app
* `-All`Kleurovergangsbeëindigingen alle apps in deze IIS-server waarvoor bewaken`SdkState==EnabledAfterDeployment`

* Controle van de opgegeven apps stopt en instrumentation verwijderd. Deze functie werkt alleen voor apps die hebt is geïmplementeerd tijdens runtime met het gereedschap Status controleren of de begin-ApplicationInsightsApplication. (`SdkState==EnabledAfterDeployment`)

* Geeft als resultaat ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: De naam van een web-app in IIS.
* `-InstrumentationKey`(Optioneel). Gebruik deze opdracht om te wijzigen van de resource waaraan van de app-telemetrielogboek wordt verzonden.
* Deze cmdlet:
 * Upgrades de benoemde app naar de versie van de SDK gedownload meest recent op deze computer. (Werkt alleen als `SdkState==EnabledAfterDeployment`)
 * Als u een sleutel instrumentation opgeeft, wordt de benoemde app geconfigureerd om het telemetrielogboek verzenden aan de resource met die toets. (Werkt als `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* De meest recente toepassing inzichten SDK is gedownload naar de server.


## <a name="next"></a>Volgende stappen

* [Maken van web tests] [ availability] om ervoor te zorgen dat uw site live blijft.
* [Gebeurtenissen en logboeken zoeken] [ diagnostic] om op te sporen van problemen.
* [Toevoegen web client telemetrielogboek] [ usage] uitzonderingen van webpagina-code en kunt u doelcellen oproepen invoegen.
* [Toepassing inzichten SDK toevoegen aan uw web-servicecode] [ greenbrown] zodat u de doelcellen invoegen kunt en log u in de code belt.



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md
