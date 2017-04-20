<properties 
    pageTitle="Problemen oplossen zonder gegevens - toepassing inzichten voor .NET" 
    description="Gegevens in Visual Studio-toepassing inzichten niet zien? Kunt u hier." 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Problemen oplossen zonder gegevens - toepassing inzichten voor .NET

## <a name="some-of-my-telemetry-is-missing"></a>Enkele van mijn telemetrielogboek ontbreekt

*In de toepassing inzichten Zie ik alleen een deel van de gebeurtenissen die worden gegenereerd door mijn app.*

* Als u de dezelfde breuk consistente ziet, is waarschijnlijk vanwege geavanceerde [steekproeven](app-insights-sampling.md). U kunt dit controleren open zoeken (op het blad overzicht) en kijkt u naar een exemplaar van een vergaderverzoek of andere gebeurtenis. Klik op '...' om te krijgen van de volledige details onderaan in de sectie eigenschappen. Als aantal > 1 aanvragen en klik vervolgens steekproeven is betrekking heeft. 
* Anders is het mogelijk dat u bent kunt u door een [tarief van gegevens beperken](app-insights-pricing.md#limits-summary) voor uw abonnement prijzen. Deze limieten gelden per minuut.

## <a name="no-data-from-my-server"></a>Geen gegevens van mijn server

*Ik heb mijn app op mijn webserver geïnstalleerd en nu zie ik niet alle telemetrielogboek hieruit. Het werkt OK op mijn computer ontwikkelaar.*

* Waarschijnlijk een firewall-probleem. [Stel firewalluitzonderingen voor toepassing inzichten om gegevens te verzenden](app-insights-ip-addresses.md).

*Ik [Statuscontrole geïnstalleerd](app-insights-monitor-performance-live-website-now.md) op mijn webserver om de bestaande apps te houden. Ik zie geen resultaten niet.*

* Zie [problemen met statuscontrole](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>De optie voor geen 'Toevoegen toepassing inzichten' in Visual Studio

*Wanneer ik een nieuw project maken in Visual Studio, of wanneer ik met de rechtermuisknop op een bestaand project in Solution Explorer, zie ik niet eventuele toepassing inzichten-opties.*

+ Niet alle typen .NET project worden ondersteund door de hulpmiddelen. Web en WCF-projecten worden ondersteund. Voor andere projecttypen zoals bureaublad of service-toepassingen kunt u nog steeds [een toepassing inzichten SDK aan uw project handmatig toevoegen](app-insights-windows-desktop.md).
+ Zorg ervoor dat er [Visual Studio 2013 Update 3 of hoger](http://go.microsoft.com/fwlink/?LinkId=397827). Dit zijn van toepassing inzichten hulpmiddelen voor vooraf geïnstalleerd.
+ Selecteer **Extra**, **Extensions en Updates** en controleer of **Hulpmiddelen voor inzichten die van toepassing** is geïnstalleerd en ingeschakeld. Als dat het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
+ Open het dialoogvenster Nieuw Project en kiest u ASP.NET-webtoepassing. Als u de optie toepassing inzichten er ziet, zijn klikt u vervolgens de's geïnstalleerd. Als dat niet zo is, probeert u de installatie ongedaan maken en de inzichten hulpprogramma's vervolgens opnieuw installeren.


## <a name="q02"></a>Het toevoegen van toepassing inzichten is mislukt

*Wanneer ik een nieuw webproject maken, of wanneer ik wil toepassing inzichten toevoegen aan een bestaand project, zie ik een foutbericht wordt weergegeven.*

Mogelijke oorzaken:

* Communicatie met de portal-toepassing inzichten is mislukt; of
* Er is een probleem met uw Azure-account;
* U hoeft [alleen toegang tot het abonnement of de groep waar u probeerde te maken van de nieuwe resource](app-insights-resources-roles-access-control.md).

FIX:

+ Controleer of u aanmeldingsgegevens hebt opgegeven voor het rechts Azure-account. 
+ Controleer dat u toegang tot de [portal van Azure hebt](https://portal.azure.com)in uw browser. Open instellingen en ziet als er een beperking.
+ [Toepassing inzichten toevoegen aan uw bestaande project](app-insights-asp-net.md): In Solution Explorer uw project met de rechtermuisknop op en kies "Toevoegen toepassing inzichten."
+ Als deze nog steeds niet werkt, volgt u de [Handmatige procedure](app-insights-windows-services.md) kunt u een resource in de portal en voeg de SDK toe aan uw project. 

## <a name="emptykey"></a>Ik krijg een foutmelding 'Instrumentation toets kan niet leeg zijn'

Lijkt erop dat is een fout opgetreden tijdens de installatie-toepassing inzichten of wellicht een adapter logboekregistratie.

Klik in Solution Explorer met de rechtermuisknop op `ApplicationInsights.config` en kiest u **Inzicht krijgen in toepassing configureren**. Krijgt u een dialoogvenster met een uitnodiging te melden bij een Azure en maak een bron-toepassing inzichten, of een bestaande opnieuw gebruiken.


##<a name="NuGetBuild"></a>"NuGet pakket(ten) ontbreken" op mijn server opbouwen

*Alles genereert OK wanneer ik ben foutopsporing op mijn computer ontwikkeling, maar wordt er een fout NuGet op de server opbouwen.*

Zie [Herstellen van NuGet-pakket](http://docs.nuget.org/Consume/Package-Restore) en [Automatische pakket herstellen](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Ontbrekende menuopdracht toepassing inzichten openen vanuit Visual Studio

*Ik zie geen opdrachten toepassing inzichten wanneer ik mijn project Solution Explorer met de rechtermuisknop, of ik een opdracht inzicht krijgen in de geopende toepassing niet wordt weergegeven.*

Mogelijke oorzaken:

* Als u de resource van toepassing inzichten handmatig hebt gemaakt of als het project is een type dat wordt niet ondersteund door de toepassing inzichten-hulpmiddelen.
* De toepassing inzichten hulpprogramma's zijn uitgeschakeld in uw Visual Studio.
* Uw Visual Studio dat ouder is dan 2013 Update 3.

FIX:

* Controleer of dat uw Visual Studio-versie niet wordt update 2013 3 of hoger.
* Selecteer **Extra**, **Extensions en Updates** en controleer of **Hulpmiddelen voor inzichten die van toepassing** is geïnstalleerd en ingeschakeld. Als dat het geval is, klikt u op **Updates** om te zien of er een update beschikbaar is.
* Met de rechtermuisknop op uw project in Solution Explorer. Als u de opdracht **Configureren toepassing inzichten**ziet, kunt u deze om uw project aan de resource in de service-toepassing inzichten verbinding te gebruiken.


Anders dat uw projecttype rechtstreeks wordt niet ondersteund door de toepassing inzichten-hulpmiddelen. Als u wilt zien van uw telemetrielogboek, meld u aan bij de [portal van Azure](https://portal.azure.com), kiest u inzichten van toepassing op de linkernavigatiebalk en selecteer uw toepassing.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Toegang geweigerd' op de toepassing inzichten openen vanuit Visual Studio

*De opdracht 'Openen toepassing inzichten', ga ik naar de Azure-portal, maar ik krijg een foutmelding 'toegang geweigerd'.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

De Microsoft-aanmelding die u voor het laatst op uw standaardbrowser gebruikt heeft geen toegang tot [de resource die is gemaakt wanneer de toepassing inzichten voor deze app is toegevoegd](app-insights-asp-net.md). Er zijn twee waarschijnlijke redenen: 

* U hebt meer dan één Microsoft-account - wellicht een werk- en een persoonlijk Microsoft-account? De aanmeldingsproblemen die u voor het laatst op uw standaardbrowser gebruikt is voor een ander account dan de tabel die toegang tot het [toevoegen van toepassing inzichten aan het project heeft](app-insights-asp-net.md). 

 * FIX: Klik op uw naam op bovenste rechts van het browservenster en meld u af. Meld u aan met het account dat toegang heeft. Klik op de linkernavigatiebalk toepassing inzichten en selecteer uw app.

* Iemand anders toepassing inzichten toegevoegd aan het project en deze bent vergeten zodat u [toegang tot de resourcegroep](app-insights-resources-roles-access-control.md) waarin deze is gemaakt. 

 * FIX: Als diegene een organisatie-account gebruikt, ze kunnen u toevoegen aan het team; of ze kunnen u afzonderlijke toegang verlenen aan de resourcegroep.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Activa niet gevonden' op de toepassing inzichten openen vanuit Visual Studio

*De opdracht 'Openen toepassing inzichten', ga ik naar de Azure-portal, maar ik krijg een foutmelding 'activa niet gevonden'.*

Mogelijke oorzaken:

* De resource van toepassing inzichten voor uw toepassing is verwijderd; of
* De toets instrumentation is ingesteld of gewijzigd in ApplicationInsights.config door deze te bewerken rechtstreeks, zonder het projectbestand bijwerken. 

De sleutel instrumentation in ApplicationInsights.config besturingselementen waarbij het telemetrielogboek wordt verzonden. Een regel in het projectbestand bepaalt welke resource wordt geopend wanneer u de opdracht in Visual Studio gebruiken. 

FIX:

* In Solution Explorer met de rechtermuisknop op het project en kies toepassing inzichten, toepassing inzichten configureren. U kunt in het dialoogvenster ofwel naar telemetrielogboek verzenden naar een bestaande resource of een nieuw account te maken. Of:
* De resource rechtstreeks openen. Meld u aan bij [de portal van Azure](https://portal.azure.com)inzichten van toepassing op de linkernavigatiebalk op en selecteer uw app.



## <a name="where-do-i-find-my-telemetry"></a>Waar vind ik mijn telemetrielogboek?

*Ik aangemeld bij de [portal van Microsoft Azure](https://portal.azure.com)en ik zoek op het dashboard van Azure thuis. Dus waar vind ik mijn gegevens toepassing inzichten?*

* Klik op de linkernavigatiebalk op inzichten-toepassing en klik op de appnaam van uw. Als u geen projecten er, die u wilt [toevoegen of toepassing inzichten in uw webproject configureren](app-insights-asp-net.md).

    U ziet er enkele samenvatting grafieken. U kunt klikken op de zelfstudies het beste om meer details weer te geven.

* Klik op de knop toepassing inzichten in Visual Studio, terwijl u uw app foutopsporing.


## <a name="q03"></a>Geen server-gegevens (of geen gegevens op alle)

*Ik heb mijn app wordt uitgevoerd en vervolgens opent u de service-toepassing inzichten in Microsoft Azure, maar alle grafieken weergeven 'Leren voor het verzamelen van...' of 'Niet geconfigureerd.'* Of, *alleen in de weergave pagina-en gebruikersgegevens, maar geen servergegevens.*

+ Voer uw toepassing uit in de foutopsporingsmodus voor in Visual Studio (F5). Gebruikt u de toepassing kunnen sommige telemetrielogboek genereren. Controleer of u gebeurtenissen die zijn vastgelegd in het uitvoervenster Visual Studio kunt zien. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Klik in de portal-toepassing inzichten [Diagnostische zoekactie](app-insights-diagnostic-search.md)te openen. Gegevens weergegeven meestal hier eerst.
+ Klik op de knop vernieuwen. Het blad wordt vernieuwd regelmatig, maar kunt u het ook handmatig doen. Het vernieuwingsinterval is langer voor grotere tijdsbereik.
+ Controleer dat de toetsen instrumentation overeenkomen. Klik op het belangrijkste blad voor de app in de portal-toepassing inzicht krijgen in de vervolgkeuzelijst **Essentials** kijkt u naar **Instrumentation-toets**. In uw project in Visual Studio, open vervolgens ApplicationInsights.config en zoek de `<instrumentationkey>`. Controleer of de twee toetsen gelijk zijn. Als dat niet:
 + Klik in de portal op toepassing inzichten en zoek de app-resource met de juiste sleutel uit. of
 + In Visual Studio Solution Explorer, met de rechtermuisknop op het project en kies toepassing inzichten, configureren. Opnieuw instellen van de app om te telemetrielogboek verzenden naar de juiste resource.
 + Als u niet over de overeenkomende toetsen beschikt, controleert u dat u de dezelfde aanmeldingsreferenties in Visual Studio als in bij de portal gebruikt.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ In het [Microsoft Azure thuis dashboard](https://portal.azure.com), kijkt u naar de map van de servicestatus. Als er enkele waarschuwing indicaties zijn, wacht u totdat ze zijn geretourneerd naar OK en vervolgens sluiten en opnieuw uw toepassing inzichten toepassing blade openen.
+ Ook [onze blog status](http://blogs.msdn.com/b/applicationinsights-status/)controleren.
+ Hebt u een code te schrijven voor de [servers SDK](app-insights-api-custom-events-metrics.md) die kunnen worden gewijzigd de sleutel instrumentation in `TelemetryClient` exemplaren of in `TelemetryContext`? Of hebt u een [filter of steekproeven configuratie](app-insights-api-filtering-sampling.md) die mogelijk worden gefilterd schrijven te veel?
+ Als u ApplicationInsights.config hebt bewerkt, Controleer zorgvuldig de configuratie van [TelemetryInitializers en TelemetryProcessors](app-insights-api-filtering-sampling.md). Een verkeerde naam typen of een parameter kan leiden tot de SDK geen gegevens te verzenden.

## <a name="q04"></a>Er zijn geen gegevens over het gebruik van de paginaweergaven, Browsers

*Ik zie de gegevens in grafieken Server antwoord tijd en aanvragen van de Server, maar geen gegevens in de laadtijd voor pagina-weergave of in de Browser of gebruik bladen.*

De gegevens afkomstig van scripts in de webpagina's. 

+ Als u een toepassing inzichten toegevoegd aan een bestaand webproject, [moet u de scripts handmatig toevoegen](app-insights-javascript.md).
+ Controleer of dat Internet Explorer uw site niet wordt weergegeven in de compatibiliteitsmodus.
+ De functie foutopsporing van de browser (F12 in sommige browsers, kiest u netwerk) gebruiken om te bevestigen dat gegevens wordt verzonden naar `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Er zijn geen gegevens afhankelijkheid of uitzondering

Zie [afhankelijkheid telemetrielogboek](app-insights-asp-net-dependencies.md) en [uitzondering telemetrielogboek](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Geen prestatiegegevens

Prestatiegegevens (CPU, IO rente, enzovoort) is beschikbaar voor [Java-webservices](app-insights-java-collectd.md), [Windows-bureaublad-apps](app-insights-windows-desktop.md) [IIS web apps en services als u statuscontrole installeert](app-insights-monitor-performance-live-website-now.md)en [Azure-Cloudservices](app-insights-azure.md). Klik onder instellingen voor Servers vindt u deze.

Het is niet beschikbaar voor Azure websites.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Er zijn geen gegevens (server) omdat ik de app gepubliceerd naar Mijn server

+ Controleer dat u alle Microsoft daadwerkelijk hebt gekopieerd. ApplicationInsights dll-bestanden op de server, samen met Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ In uw firewall moet u mogelijk [enkele TCP-poorten openen](app-insights-ip-addresses.md#data-access-api).
+ Als u een proxyserver gebruiken om te verzenden afmelden bij uw bedrijfsnetwerk, stelt u [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) in Web.config
+ Windows Server 2008: Zorg ervoor dat u de volgende updates hebt geïnstalleerd: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt

* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u de quota voor uw maandelijkse gegevenspunten raken? Open het instellingen/Quota en prijzen om vast te stellen. Als dat het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen kleurenschema](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik ben verwacht

Als uw toepassing een groot aantal gegevens stuurt en u de toepassing inzichten SDK voor ASP.NET versie 2.0.0-beta3 of hoger gebruikt, kan de functie [Geavanceerde steekproeven](app-insights-sampling.md) werken en slechts een percentage van uw telemetrielogboek verzenden. 

U kunt deze uitschakelen, maar dit wordt niet aanbevolen. Steekproeven is zo ontworpen dat gerelateerde telemetrielogboek juist wordt verzonden, klikt u voor diagnose. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Verkeerde geografische gegevens in de gebruiker telemetrielogboek

De plaats, regio en land dimensies zijn afgeleid van IP-adressen en niet altijd juist.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering "methode niet gevonden" over het uitvoeren van Azure Cloud Services

U voor .NET 4.6 maken? 4.6 wordt niet automatisch ondersteund in Azure Cloud Services-rollen. [4.6 op elke rol installeren](../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat u uw app.

## <a name="still-not-working"></a>Nog werkt niet...

* [Toepassing inzichten-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

