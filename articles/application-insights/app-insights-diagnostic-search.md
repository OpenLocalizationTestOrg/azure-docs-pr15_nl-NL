<properties 
    pageTitle="Met diagnostische zoeken | Microsoft Azure" 
    description="Zoeken en filteren van afzonderlijke gebeurtenissen, aanvragen, en meld u sporen." 
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
    ms.date="06/09/2016" 
    ms.author="awills"/>
 
# <a name="using-diagnostic-search-in-application-insights"></a>Diagnostische gegevens zoeken in de toepassing inzichten gebruiken

Diagnostische gegevens tijdens het zoeken is een functie van de [Toepassing inzichten] [ start] die u gebruikt om te zoeken en verkennen van afzonderlijke telemetrielogboek items, zoals paginaweergaven, uitzonderingen, of aanvragen met webonderdelen. En u kunt log sporen en gebeurtenissen die u hebt gecodeerd weergeven.

## <a name="where-do-you-see-diagnostic-search"></a>Waar ziet u diagnostische gegevens zoeken?


### <a name="in-the-azure-portal"></a>In de portal van Azure

Diagnostische gegevens zoeken kunt u expliciet openen:

![Open diagnostische gegevens zoeken](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)


Dit wordt ook geopend wanneer u klikt op via sommige diagrammen en raster items. In dit geval zijn de filters vooraf ingesteld kunt richten op het type item dat u hebt geselecteerd. 

Als uw toepassing een webservice is, bevat het blad overzicht bijvoorbeeld een grafiek van het volume van aanvragen. Klikt u erop en u toegang hebt tot een meer gedetailleerde grafiek met een lijst met hoeveel aanvragen voor elke URL zijn aangebracht. Klik op een rij en krijgt u een lijst met de afzonderlijke aanvragen voor die URL:

![Open diagnostische gegevens zoeken](./media/app-insights-diagnostic-search/07-open-from-filters.png)


De hoofdtekst van diagnostische gegevens zoeken is een lijst met telemetrielogboek items - serveraanvragen, pagina weergaven, aangepaste gebeurtenissen die u hebt gecodeerd, enzovoort. Boven aan de lijst wordt een samenvatting grafiek met aantallen gebeurtenissen na verloop van tijd.

Gebeurtenissen meestal weergegeven in zoekresultaten voor diagnostische voordat deze worden weergegeven in metrische explorer. Hoewel het blad met tussenpozen wordt vernieuwd, klik u op vernieuwen als u voor een bepaalde gebeurtenis aan het wachten bent.


### <a name="in-visual-studio"></a>In Visual Studio

Open het venster Zoeken in Visual Studio:

![](./media/app-insights-diagnostic-search/32.png)

Het venster zoeken heeft dezelfde functies als de webportal:

![](./media/app-insights-diagnostic-search/34.png)


## <a name="sampling"></a>Steekproeven

Als uw app een groot aantal telemetrielogboek genereert (en de ASP.NET SDK versie 2.0.0-beta3 dat u gebruikt of hoger), de module Geavanceerde steekproeven automatisch afgetrokken van het volume dat wordt verzonden naar de portal door te sturen van alleen een vertegenwoordiger fractie van gebeurtenissen. Gebeurtenissen die betrekking op één verzoek hebben worden echter worden geselecteerd of uitgeschakeld als een groep, zodat u tussen gerelateerde gebeurtenissen navigeren kunt. 

[Algemene informatie over de steekproeven](app-insights-sampling.md).


## <a name="inspect-individual-items"></a>Afzonderlijke items controleren

Selecteer een item telemetrielogboek om sleutelvelden weer te geven en verwante items. Als u zien van de volledige reeks velden wilt, klikt u op '...'. 


![Klik op Nieuw Item van het werk, bewerkt u de velden en klik vervolgens op OK.](./media/app-insights-diagnostic-search/10-detail.png)

Als u de volledige reeks velden zoekt, gebruikt u gewone tekenreeksen (zonder jokertekens). De beschikbare velden, hangt af van het type telemetrielogboek.

## <a name="create-work-item"></a>Werkitem maken

U kunt een fout in Visual Studio Team Services maken met de gegevens van een item telemetrielogboek. 

![Klik op Nieuw Item van het werk, bewerkt u de velden en klik vervolgens op OK.](./media/app-insights-diagnostic-search/42.png)

De eerste keer dat u dit doet, wordt u gevraagd om een koppeling naar uw Team Services-account en project te configureren.

![De URL van uw Team Services-server en de naam van het Project invullen en klikt u op machtigen](./media/app-insights-diagnostic-search/41.png)

(U kunt ook naar het blad configuratie openen Klik op Instellingen > werkitems.)

## <a name="filter-event-types"></a>Typen gebeurtenissen filteren

Open het blad Filter en kiest u de gebeurtenistypen die u wilt zien. (Als u later herstellen van de filters waarmee u het blad hebt geopend wilt, klikt u op beginwaarden.)


![Kies Filter en selecteer telemetrielogboek typen](./media/app-insights-diagnostic-search/02-filter-req.png)


De gebeurtenistypen zijn:

* **Doelcellen** - diagnostische logboeken, met inbegrip van TrackTrace, log4Net NLog en System.Diagnostic.Trace gesprekken.
* **Aanvragen** - HTTP-aanvragen ontvangen door de server-toepassing, inclusief pagina's, scripts, afbeeldingen, bestanden in de stijl en gegevens. Deze gebeurtenissen worden gebruikt voor de vergaderverzoeken en antwoorden overzicht grafieken maken.
* **Paginaweergave** - Telemetrielogboek verzonden door de webclient gebruikt om pagina weergaverapporten te maken. 
* **Aangepaste gebeurtenis** : als u oproepen naar TrackEvent() ingevoegd in volgorde naar [gebruik controleren][track], kunt u deze hier zoeken.
* **Uitzondering** - onbekende uitzonderingen in de server en die u zich aanmeldt met behulp van TrackException().

## <a name="filter-on-property-values"></a>Filteren op eigenschapswaarden

U kunt gebeurtenissen op de waarden van hun eigenschappen filteren. De beschikbare eigenschappen, hangt af van de typen gebeurtenissen die u hebt geselecteerd. 

Kies bijvoorbeeld aanvragen met een specifieke antwoordcode.

![Een eigenschap uitvouwen en kies een waarde](./media/app-insights-diagnostic-search/03-response500.png)

Kiezen geen waarden van een bepaalde eigenschap heeft hetzelfde resultaat als het kiezen van alle waarden; deze activeert uitschakelen filteren op die eigenschap.


### <a name="narrow-your-search"></a>Zoekopdrachten verfijnen

Zoals u ziet dat de aantallen aan de rechterkant van de filterwaarden hoeveel exemplaren er zijn in de huidige gefilterde groep weergeven. 

In dit voorbeeld is dit wissen die de `Reports/Employees` aanvragen resultaten in de meeste van de 500 fouten ziet:

![Een eigenschap uitvouwen en kies een waarde](./media/app-insights-diagnostic-search/04-failingReq.png)

Als u ook Zie welke andere gebeurtenissen zijn er tijdens deze periode wilt, kunt u bovendien **opnemen gebeurtenissen met ongedefinieerd eigenschappen**controleren.

## <a name="remove-bot-and-web-test-traffic"></a>Bots en web test-verkeer verwijderen

Gebruik het filter **reële of synthetische verkeer** en **reële**controleren.

U kunt ook filteren op **bron van synthetische-verkeer is toegestaan**.

## <a name="inspect-individual-occurrences"></a>Afzonderlijke vermeldingen controleren

De naam van die aanvraag toevoegen aan de set filter en kunt u afzonderlijke exemplaren van die gebeurtenis vervolgens controleren.

![Selecteer een waarde](./media/app-insights-diagnostic-search/05-reqDetails.png)

De details weergeven voor verzoek gebeurtenissen, uitzonderingen die zijn aangebracht terwijl de aanvraag is verwerkt.

Klik op een uitzondering om de detailgegevens, inclusief de tracering stapel weer te geven.

![Klik op een uitzondering](./media/app-insights-diagnostic-search/06-callStack.png)

## <a name="find-events-with-the-same-property"></a>Gebeurtenissen met dezelfde eigenschap zoeken

Alle items met dezelfde eigenschapswaarde vinden:

![Met de rechtermuisknop op een eigenschap](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-by-metric-value"></a>Zoeken op metrische waarde

Aanvragen antwoord altijd > 5s krijgen.  Tijden worden aangegeven in tikken: 10 000 maatstreepjes = 1ms.

!["Antwoord time":(threshold TO *)](./media/app-insights-diagnostic-search/11-responsetime.png)



## <a name="search-the-data"></a>De gegevens zoeken

U kunt zoeken naar termen in een van de eigenschapswaarden. Dit is vooral handig als u [aangepaste gebeurtenissen] hebt geschreven[ track] met eigenschapswaarden. 

Het is raadzaam een bereik, als zoekopdrachten voor een kortere datumbereik sneller worden tijd in te stellen. 

![Open diagnostische gegevens zoeken](./media/app-insights-diagnostic-search/appinsights-311search.png)

Zoeken naar termen, niet subreeksen. Termen zijn alfanumerieke tekenreeksen met enkele interpunctie, zoals '.' en '_'. Bijvoorbeeld:

term|*niet* is overeenkomt met de|maar deze komen overeen met
---|---|---
HomeController.About|over<br/>Start|h\*over<br/>Start\*
IsLocal|lokale<br/>is<br/>\*lokale|ISL\*<br/>islocal<br/>i\*l\*
Nieuwe vertraging|w d|Nieuw<br/>vertraging<br/>n\* en d\*


Hier volgen de zoekexpressies die u kunt gebruiken:

Voorbeeldquery | Effect 
---|---
vertragen|Zoeken naar alle gebeurtenissen in het datumbereik waarvan de velden de term bevatten "vertraagd"
database??|Komt overeen met database01, databaseAB...<br/>? is niet toegestaan aan het begin van een zoekterm.
database * |Komt overeen met de database, database01, databaseNNNN<br/> * is niet toegestaan aan het begin van een zoekterm
Apple en banaan|Zoek de gebeurtenissen die beide termen bevatten. Gebruik kapitaal 'en' niet 'en'.
Apple banaan of-bewerking<br/>Apple banaan|Zoek de gebeurtenissen die een van beide termen bevatten. Gebruik 'Of' niet 'of'. < /br/ > korte formulier.
Apple niet banaan<br/>Apple-banaan|Zoek de gebeurtenissen die één term, maar niet op de andere bevatten.<br/>Korte formulier.
App * en banaan-(grape pear)|Logische operators en haakjes.
"Metrisch": 0 tot en met 500<br/>"Metrisch": 500 aan * | Zoek de gebeurtenissen die de benoemde maateenheden in het waardebereik bevatten.


## <a name="save-your-search"></a>Uw zoekopdracht opslaan als

Als u de gewenste filters hebt ingesteld, kunt u de zoekopdracht opslaan als favoriet. Als u in een organisatie-account werkt, kunt u kiezen of u wilt delen met andere teamleden.

![Klik op favoriet, stel de naam en klik op Opslaan](./media/app-insights-diagnostic-search/08-favorite-save.png)


Zie de zoekopdracht opnieuw, **gaat u naar het blad Overzicht** en Favorieten openen:

![Favorieten-tegel](./media/app-insights-diagnostic-search/09-favorite-get.png)

Als u met relatieve tijdsbereik opgeslagen, heeft het opnieuw geopend blad de meest recente gegevens. Als u met Absolute tijdsbereik hebt opgeslagen, ziet u dezelfde gegevens elke keer.


## <a name="send-more-telemetry-to-application-insights"></a>Meer telemetrielogboek inzicht krijgen in toepassing verzenden

Naast de out-van-het-box-telemetrielogboek verzonden door de toepassing inzichten SDK, kunt u het volgende doen:

* Log traces van uw favoriete logboekregistratie framework in [.NET] vastleggen[ netlogs] of [Java][javalogs]. Dit betekent dat u kunt traces log doorzoeken en deze afstemmen met paginaweergaven, uitzonderingen en andere gebeurtenissen. 
* [Programmacode schrijven] [ track] aangepaste gebeurtenissen, paginaweergaven en uitzonderingen verzenden. 

[Meer informatie over het verzenden van Logboeken en aangepaste telemetrielogboek inzicht krijgen in toepassing][trace].


## <a name="questions"></a>Q & A

### <a name="limits"></a>Hoeveel gegevens blijven behouden?

Maximaal 500 gebeurtenissen per seconde van elke toepassing. Gebeurtenissen worden gedurende zeven dagen bewaard.

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Hoe kan ik POST-gegevens in mijn serveraanvragen zien?

We niet Meld u aan de POST-gegevens automatisch, maar u kunt [TrackTrace of logboek oproepen][trace]. Plaats de bericht-gegevens in de bericht-parameter. U kunt niet filteren op het bericht de manier waarop die u eigenschappen kunt, maar de groottelimiet overschrijden langer is.

## <a name="add"></a>Volgende stappen

* [Logboeken en aangepaste telemetrielogboek inzicht krijgen in toepassing verzenden][trace]
* [Beschikbaarheid en serverreactie tests instellen][availability]
* [Problemen oplossen][qna]



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[javalogs]: app-insights-java-trace-logs.md
[netlogs]: app-insights-asp-net-trace-logs.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
[trace]: app-insights-search-diagnostic-logs.md
[track]: app-insights-api-custom-events-metrics.md

 