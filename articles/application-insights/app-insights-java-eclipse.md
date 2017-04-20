<properties 
    pageTitle="Aan de slag met inzicht in de toepassing met Java in Eclips" 
    description="Gebruik van de invoegtoepassing Eclips prestaties en gebruik monitoring naar uw website Java met toepassing inzichten toevoegen" 
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
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Aan de slag met inzicht in de toepassing met Java in Eclips

De toepassing inzichten SDK wordt telemetrielogboek verzonden vanuit uw webtoepassing Java, zodat u kunt gebruik en analyseren. De invoegtoepassing voor de toepassing inzichten Eclips installeert automatisch de SDK in uw project, zodat u eraan het vak telemetrielogboek, plus een API die u kunt gebruiken om te schrijven van aangepaste telemetrielogboek.   


## <a name="prerequisites"></a>Vereisten voor

De invoegtoepassing werkt momenteel voor Maven projecten en dynamische webprojecten in Eclips. ([Toepassing inzichten toevoegen aan andere soorten Java project][java].)

U hebt nodig:

* Oracle JRE 1,6 of hoger
* Een abonnement op [Microsoft Azure](https://azure.microsoft.com/). (U kunt starten met de [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).)
* [Eclips IDE voor Java EE ontwikkelaars](http://www.eclipse.org/downloads/), Indigo of hoger.
* Windows 7 of hoger, of Windows Server 2008 of hoger

## <a name="install-the-sdk-on-eclipse-one-time"></a>De SDK installeren op Eclips (één keer)

U moet alleen dit één keer per computer uitvoeren. Deze stap een toolkit die vervolgens de SDK aan elk dynamische Web-Project toevoegen kunt is geïnstalleerd.

1. Klik op Help, nieuwe Software installeren in Eclips.

    ![Help, nieuwe Software installeren](./media/app-insights-java-eclipse/0-plugin.png)

2. De SDK is in http://dl.windowsazure.com/eclipse, onder Azure Toolkit. 
3. Schakel **Neem contact op met alle updatewebsites...**

    ![Schakel het selectievakje voor de toepassing inzichten SDK, contact opnemen met alle updatewebsites](./media/app-insights-java-eclipse/1-plugin.png)

De overige stappen voor elk project Java.

## <a name="create-an-application-insights-resource-in-azure"></a>Maken van een resource van toepassing inzichten in Azure wordt aangegeven

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Maak een nieuwe resource van toepassing inzichten.  

    ![Klik op + en kiest u inzicht krijgen in toepassing](./media/app-insights-java-eclipse/01-create.png)  
3. Stel het toepassingstype op Java-webtoepassing.  

    ![Opvulling van een naam, kies Java WebApp en klik op maken](./media/app-insights-java-eclipse/02-create.png)  
4. Zoek de instrumentation-toets voor de nieuwe bron. U moet dit binnenkort in uw CodeProject plakken.  

    ![In het nieuwe Resourceoverzicht, klik op eigenschappen en de toets Instrumentation kopiëren](./media/app-insights-java-eclipse/03-key.png)  


## <a name="add-application-insights-to-your-project"></a>Toepassing inzichten toevoegen aan uw project

1. Inzichten die toepassing in het contextmenu van uw Java-web-project toevoegen.

    ![In het nieuwe Resourceoverzicht, klik op eigenschappen en de toets Instrumentation kopiëren](./media/app-insights-java-eclipse/02-context-menu.png)

2. Plak de instrumentation opgeven die u hebt ontvangen van de Azure-portal.

    ![In het nieuwe Resourceoverzicht, klik op eigenschappen en de toets Instrumentation kopiëren](./media/app-insights-java-eclipse/03-ikey.png)


De toets wordt verzonden samen met alle items van telemetrielogboek en inzichten van de toepassing weer te geven in de bron worden vermeld.

## <a name="run-the-application-and-see-metrics"></a>Voer de toepassing en Zie aan de doelstellingen

Voer uw toepassing uit.

Ga terug naar uw bron-toepassing inzichten in Microsoft Azure.

HTTP-aanvragen gegevens wordt weergegeven op het blad Overzicht. (Als deze niet ziet, wacht een paar seconden en klik vervolgens op vernieuwen.)

![Serverreactie, verzoek telt en fouten ](./media/app-insights-java-eclipse/5-results.png)
 

Klik op in een grafiek om meer gedetailleerde afmetingen weer te geven. 

![Telt aanvragen met hun naam weergeven](./media/app-insights-java-eclipse/6-barchart.png)


[Meer informatie over de doelstellingen.][metrics]

 

En wanneer u de eigenschappen van een nieuw vergaderverzoek bekijkt, ziet u de telemetrielogboek gebeurtenissen die zijn gekoppeld aan dit zoals aanvragen en uitzonderingen.
 
![Alle sporen voor deze aanvraag](./media/app-insights-java-eclipse/7-instance.png)


## <a name="client-side-telemetry"></a>Aan de clientzijde telemetrielogboek

Klik op Get-code als u wilt controleren van mijn webpagina's van het blad Snelstartgids: 

![Klik op uw app overzicht blade, kiest u snel aan de slag, krijgen van de code wilt controleren van mijn webpagina's. Kopieer het script.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Het codefragment invoegen in de kop van uw HTML-bestanden.

#### <a name="view-client-side-data"></a>De gegevens aan de clientzijde weergeven

Open uw bijgewerkte webpagina's en deze gebruiken. Wacht een paar minuten of twee, en vervolgens terug naar de toepassing inzichten en opent u het blad gebruik. (Het blad overzicht, schuif omlaag en klik op gebruik.)

De doelstellingen van de weergave, gebruiker en sessie van de pagina wordt weergegeven op het blad gebruik:

![Sessies, gebruikers en paginaweergaven](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Meer informatie over het instellen van aan de clientzijde telemetrielogboek.][usage]

## <a name="publish-your-application"></a>Uw toepassing publiceren

Nu uw app naar de server, laat personen gebruiken, en bekijk de telemetrielogboek worden weergegeven op de portal voor publiceren.

* Controleer of dat uw firewall kan de toepassing telemetrielogboek naar deze poorten verzenden:

 * DC.Services.visualstudio.com:443
 * DC.Services.visualstudio.com:80
 * F5.Services.visualstudio.com:443
 * F5.Services.visualstudio.com:80


* Installeren op Windows-servers:

 * [Microsoft Visual C++ distribueren pakket](http://www.microsoft.com/download/details.aspx?id=40784)

    (Hiermee kunt u prestatiemeteritems.)

## <a name="exceptions-and-request-failures"></a>Uitzonderingen en aanvraag fouten

Onverwerkte uitzonderingen zijn automatisch verzameld:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Als u wilt gegevens verzamelen over andere uitzonderingen, hebt u twee opties:

* [Oproepen naar TrackException in code invoegen](app-insights-api-custom-events-metrics.md#track-exception). 
* [De Java-Agent op de server installeren](app-insights-java-agent.md). U de methoden die u wilt bekijken.


## <a name="monitor-method-calls-and-external-dependencies"></a>Monitor met een methode oproepen en externe afhankelijkheden

Interne methoden en oproepen via JDBC, met gegevens van de tijdsinstellingen van het [installeren van de Java-Agent](app-insights-java-agent.md) aan te melden worden opgegeven.


## <a name="performance-counters"></a>Prestatie-items

Klik op uw bladeserver overzicht, schuif omlaag en klik op de tegel **Servers** . Hier ziet u een bereik van prestatiemeteritems.


![Schuif omlaag naar klikt u op die de Servers tegel gemarkeerd](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Prestatiemeteritem siteverzameling aanpassen

Als u wilt uitschakelen verzameling de standaardset prestatie-items, de volgende code onder het knooppunt van de hoofdmap van het bestand ApplicationInsights.xml hebt toegevoegd:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### <a name="collect-additional-performance-counters"></a>Aanvullende prestatiemeteritems verzamelen

Aanvullende prestatie-items om te worden verzameld, kunt u opgeven.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX items (die worden aangeboden door de Java Virtual Machine)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName`– De naam die wordt weergegeven in de portal-toepassing inzichten.
*   `objectName`– De naam van het object JMX.
*   `attribute`– Het kenmerk van de naam van het object JMX om op te halen
*   `type`(optioneel) - het type van het object JMX kenmerk:
 *  Standaard: een eenvoudig type zoals int of lang.
 *  `composite`: de prestatiemetergegevens is de notatie 'Attribute.Data'
 *  `tabular`: de prestatiemetergegevens is in de indeling van een tabelrij



#### <a name="windows-performance-counters"></a>Windows prestatie-items

Elke [Windows prestatie-item](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is een lid van een categorie (op dezelfde manier dat een veld een lid van een klasse is). Categorieën kunnen zijn globale, of hebt genummerd of met de naam exemplaren.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   Weergavenaam – de naam weergegeven in de portal-toepassing inzichten.
*   Categorienaam – de prestatiemeteritemcategorie (prestatie-object) waaraan dit prestatiemeteritem gekoppeld is.
*   counterName – de naam van de teller prestaties.
*   Exemplaarnaam – de naam van de Prestatiemeteritem categorie-instantie of een lege tekenreeks (""), als de categorie één exemplaar bevat. Als de categorienaam proces is, en de prestatie-item dat u wilt verzamelen afkomstig is van het huidige JVM-proces op waarin uw app actief is, geeft `"__SELF__"`.

Uw prestatiemeteritems worden weergegeven als aangepast aan de doelstellingen in [Aan de doelstellingen Explorer][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### <a name="unix-performance-counters"></a>UNIX prestatiemeteritems

* [Collectd met de toepassing inzichten-invoegtoepassing installeren](app-insights-java-collectd.md) om een groot aantal gegevens systeem en netwerk.

## <a name="availability-web-tests"></a>Beschikbaarheid van web tests

Toepassing inzichten kunnen testen van uw website met regelmatige tussenpozen om te controleren die is en u ook reageert. [Voor het instellen van][availability], schuif omlaag om beschikbaarheid op te klikken.

![Schuif omlaag en klik op beschikbaarheid aan, klikt u vervolgens de test Web toevoegen](./media/app-insights-java-eclipse/31-config-web-test.png)

U krijgt grafieken antwoord tijden, plus e-mailmeldingen als uw site uitvalt.

![Voorbeeld van de Web-test](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Meer informatie over de beschikbaarheid van web tests.][availability] 

## <a name="diagnostic-logs"></a>Diagnostische logboeken

Als u Logback of Log4J gebruikt (versie 1.2 of v2.0) voor tracering, kunt u beschikken over de logboeken voor het traceren automatisch naar de toepassing inzichten waar u kunt verkennen en zoeken worden verzonden.

[Meer informatie over de diagnostische logboeken][javalogs]

## <a name="custom-telemetry"></a>Aangepaste telemetrielogboek 

Voeg een paar regels met code in uw webtoepassing Java om vast te stellen wat gebruikers ermee doen of om op te sporen problemen. 

U kunt code hebt ingevoegd in webpagina JavaScript zowel in de Java servers.

[Meer informatie over aangepaste telemetrielogboek][track]



## <a name="next-steps"></a>Volgende stappen

#### <a name="detect-and-diagnose-issues"></a>Problemen ontdekken en analyseren

* [Toevoegen web client telemetrielogboek] [ usage] prestaties telemetrielogboek ophalen van de webclient.
* [Web tests instellen] [ availability] te controleren of uw toepassing blijft, live en heeft gereageerd.
* [Gebeurtenissen en logboeken zoeken] [ diagnostic] om op te sporen van problemen.
* [Log4J of Logback sporen vastleggen][javalogs]

#### <a name="track-usage"></a>Gebruik bijhouden

* [Toevoegen web client telemetrielogboek] [ usage] op monitor paginaweergaven en eenvoudige gebruiker aan de doelstellingen.
* [Aangepaste gebeurtenissen en aan de doelstellingen bijhouden] [ track] voor meer informatie over hoe uw toepassing wordt gebruikt, zowel op de client en de server.


<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 