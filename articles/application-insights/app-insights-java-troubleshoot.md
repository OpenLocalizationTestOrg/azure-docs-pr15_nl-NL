<properties 
    pageTitle="Problemen met toepassing inzichten in een Java-web-project" 
    description="De gids voor probleemoplossing: live Java-apps gebruiken met de toepassing inzichten cmdlets voor controle." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Problemen oplossen en Q & A voor toepassing inzichten voor Java

Vragen of problemen met [Visual Studio toepassing inzichten in Java][java]? Hier volgen enkele tips.


## <a name="build-errors"></a>Maken van fouten

*Ik krijg opbouwen of controlesom fouten met gegevensvalidatie in Eclips, bij het toevoegen van de toepassing inzichten SDK via Maven of Gradle.*

* Als de afhankelijkheid <version> element een patroon met jokertekens gebruikt (bijvoorbeeld (Maven) `<version>[1.0,)</version>` of (Gradle) `version:'1.0.+'`), kunt u een specifieke versie in plaats daarvan opgeven zoals `1.0.2`. Zie de [release-informatie](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) voor de meest recente versie.

## <a name="no-data"></a>Er zijn geen gegevens 

*Ik heb toegevoegd toepassing inzichten en Mijn app uitgevoerd, maar ik ken nooit gegevens in de portal.*

* Wacht een paar minuten en klikt u op vernieuwen. De grafieken regelmatig zelf vernieuwen, maar u kunt ook handmatig vernieuwen. Het vernieuwingsinterval, is afhankelijk van het tijdsbereik van de grafiek.
* Controleer of u een instrumentation sleutel gedefinieerd in het bestand ApplicationInsights.xml (in de map bronnen in uw project hebt)
* Controleer of er geen `<DisableTelemetry>true</DisableTelemetry>` knooppunt in het XML-bestand.
* In uw firewall moet u mogelijk TCP-poorten 80 en 443 voor uitgaand verkeer naar dc.services.visualstudio.com openen. Bekijk hier de [volledige lijst met firewalluitzonderingen](app-insights-ip-addresses.md)
* Begin in het Microsoft Azure bord, kijkt u naar de plattegrond van de status van service. Als er enkele waarschuwing indicaties zijn, wacht u totdat ze zijn geretourneerd naar OK en vervolgens sluiten en opnieuw uw toepassing inzichten toepassing blade openen.
* Logboekregistratie aan het venster IDE console inschakelen door toe te voegen een `<SDKLogger />` element onder het knooppunt hoofdmap in het ApplicationInsights.xml-bestand (in de map bronnen in uw project) en het selectievakje voor items die worden voorafgegaan door [fout].
* Zorg ervoor dat het juiste ApplicationInsights.xml-bestand is geladen door de SDK Java door te kijken van de console Uitvoerberichten voor een overzicht 'configuratiebestand is is gevonden'.
* Als het configuratiebestand niet wordt gevonden, controleer dan de Uitvoerberichten om te zien waar het configuratiebestand wordt gezocht en zorg ervoor dat de ApplicationInsights.xml bevindt zich in een van deze zoeklocaties. Als een vuistregel, kunt u het configuratiebestand in de buurt van de toepassing inzichten SDK potten plaatsen. Bijvoorbeeld: in Tomcat, betekent dit dat de WEB-INF/bibliotheek-map.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt

* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u de quota voor uw maandelijkse gegevenspunten raken? Open instellingen/Quota en prijzen om vast te stellen. Als dat het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen kleurenschema](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik ben verwacht

* Open de quota en prijzen blade en controleer of [steekproeven](app-insights-sampling.md) betrekking heeft. (de overdracht van 100% betekent dat steekproeven niet betrekking heeft.) De toepassing inzichten-service kan worden ingesteld op alleen een deel van het telemetrielogboek die uit uw app binnenkomt accepteren. Hiermee kunt u binnen de quota voor uw maandelijkse van telemetrielogboek houden. 

## <a name="no-usage-data"></a>Geen gebruiksgegevens

*Ik zie gegevens over het aanvragen en tijden antwoord, maar geen paginaweergave, browser of gebruikersgegevens.*

U instellen kunt dat uw app telemetrielogboek vanaf de server verzendt. Nu uw volgende stap voor het [instellen van uw webpagina's om te verzenden telemetrielogboek vanuit uw browser is][usage].

U kunt ook als de klant een app in een [telefoon of ander apparaat is][platforms], kunt u telemetrielogboek daarvandaan verzenden. 

Gebruik dezelfde instrumentation sleutel voor het instellen van de client en de server telemetrielogboek. De gegevens worden weergegeven in dezelfde toepassing inzichten resource en u kunt wel relateren gebeurtenissen van clients en servers.



## <a name="disabling-telemetry"></a>Telemetrielogboek uitschakelen

*Hoe kan ik telemetrielogboek siteverzameling uitschakelen?*

In code:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Of** 

Werk ApplicationInsights.xml (in de map bronnen in uw project). De volgende handelingen uit onder het knooppunt hoofdmap toevoegen:

    <DisableTelemetry>true</DisableTelemetry>

De XML-methode gebruikt, moet u de toepassing opnieuw starten wanneer u de waarde wijzigt.

## <a name="changing-the-target"></a>Het doel wijzigen

*Hoe kan ik welke Azure resource mijn project gegevens worden verzonden wijzigen?*

* [De toets instrumentation van de nieuwe resource krijgen.][java]
* Als u inzicht krijgen in toepassing toegevoegd aan uw project met behulp van de Azure-Toolkit voor Eclips, klik met de rechtermuisknop uw webproject, selecteer **Azure**, **Toepassing inzichten configureren**, en de sleutel wijzigen.
* Anders de sleutel in ApplicationInsights.xml in de map resources in uw project bijwerken.


## <a name="the-azure-start-screen"></a>Het startscherm van Azure

*Ik zoek bij [de portal van Azure](https://portal.azure.com). De plattegrond zegt iets over mijn app?*

Nee, deze ziet u de status van Azure servers overal ter wereld.

*Van het bord Azure starten (beginscherm), hoe vind ik gegevens over mijn app?*

Voorwaarde dat u [uw app voor de toepassing inzichten instellen][java], klikt u op Bladeren, selecteer toepassing inzichten en selecteer de app-resource die u hebt gemaakt voor de app. Als u er sneller in toekomstige, u kunt uw app vastmaken aan het bord starten.

## <a name="intranet-servers"></a>Intranetservers

*Kan ik een server controleren op mijn intranet?*

Ja, aangeboden dat uw server telemetrielogboek bij de portal-toepassing inzichten via de openbare internet kunt verzenden. 

In uw firewall moet u mogelijk TCP-poorten 80 en 443 voor uitgaand verkeer naar dc.services.visualstudio.com en f5.services.visualstudio.com openen.

## <a name="data-retention"></a>Gegevensretentie 

*Hoe lang wordt gegevens opgeslagen in de portal? Is het veilig?*

Zie [Gegevensretentie en privacy][data].

## <a name="next-steps"></a>Volgende stappen

*Ik heb toepassing inzichten ingesteld voor mijn app Java-server. Op welke andere manieren kan ik doen?*

* [Beschikbaarheid van uw webpagina's controleren][availability]
* [Gebruik van de pagina met webonderdelen controleren][usage]
* [Gebruik bijhouden en een diagnose stellen bij problemen in uw apparaat-apps][platforms]
* [Programmacode om bij te houden van gebruik van uw app schrijven][track]
* [Vastleggen van diagnostische logboeken][javalogs]


## <a name="get-help"></a>U kunt hulp krijgen

* [Stapel overloop](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 