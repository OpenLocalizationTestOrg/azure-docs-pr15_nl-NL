<properties
    pageTitle="Azure CDN geavanceerde HTTP rapporten | Microsoft Azure"
    description="Geavanceerde HTTP-rapporten in Microsoft Azure CDN. Deze rapporten vindt gedetailleerde informatie over CDN activiteit."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Geavanceerde HTTP-rapporten in Microsoft Azure CDN

## <a name="overview"></a>Overzicht

In dit document wordt uitgelegd geavanceerde HTTP rapportage in Microsoft Azure CDN. Deze rapporten vindt gedetailleerde informatie over CDN activiteit.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Geavanceerde HTTP-rapporten openen

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Plaats de muisaanwijzer boven de tab **Analytics** en klik vervolgens muis boven de **Geavanceerde HTTP-rapporten** : flyout.  Klik op **grote HTTP-Platform**.

    ![CDN-beheerportal - menu Geavanceerde rapporten](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Rapportopties worden weergegeven.

## <a name="geography-reports-map-based"></a>Geografie-rapporten (kaart-basis)

Er zijn vijf rapporten die gebruikmaken van een kaart om aan te geven van de regio's waarvan de inhoud wordt aangevraagd. Deze rapporten zijn wereldkaart, kaart van de Verenigde Staten, Canada kaart, kaart van Europa en Azië en stille kaart.

Elk rapport op kaarten gebaseerde wordt de rang van geografische entiteiten (dat wil zeggen landen, provincies en provincies) op basis van het percentage treffers die afkomstig zijn uit die regio. Daarnaast wordt een kaart weergegeven om te helpen u de locaties waaruit de inhoud wordt aangevraagd visualiseren. Dit kan doen door elke regio op basis van de hoeveelheid aanvraag ervaren in dat gebied kleurcodering. Lichtere arcering regio's geven lagere aanvraag voor uw inhoud, terwijl donkerder regio's hogere niveaus van de aanvraag voor uw inhoud geven.

Gedetailleerde verkeer en bandbreedte informatie voor elke regio is opgegeven direct onder de kaart. Hiermee kunt u het totale aantal treffers, het percentage treffers en de totale hoeveelheid gegevens worden overgedragen (in GB) en het percentage van de gegevens worden overgedragen voor elke regio. Een beschrijving voor elk van deze gegevens bekijken. Ten slotte, als u de muisaanwijzer op een gebied (dat wil zeggen land, staat of provincie), de naam en het percentage treffers die zijn aangebracht in de regio verschijnt als knopinfo.

Hieronder vindt u een korte beschrijving voor elk type op kaarten gebaseerde Geografie rapport.

Rapportnaam | Beschrijving
------------|------------
Plattegrond van de wereld | Dit rapport kunt u de overal ter wereld vraag naar uw CDN-inhoud weergeven. Elk land is voorzien van kleurcoderingen op de wereldkaart om aan te geven het percentage treffers die afkomstig uit die regio zijn.
Verenigde Staten kaart | Dit rapport kunt u de aanvraag voor uw CDN-inhoud weergeven in de Verenigde Staten. Elke status is met kleurcodering op deze kaart om aan te geven het percentage treffers die afkomstig uit die regio zijn.
Canada kaart | Dit rapport kunt u de aanvraag voor uw CDN-inhoud weergeven in Canada. Elke provincie is met kleurcodering op deze kaart om aan te geven het percentage treffers die afkomstig uit die regio zijn.
Kaart van Europa | Dit rapport kunt u de aanvraag voor uw CDN-inhoud weergeven in Europa. Elk land is met kleurcodering op deze kaart om aan te geven het percentage treffers die afkomstig uit die regio zijn.
Azië Pacific kaart | Dit rapport kunt u de aanvraag voor uw CDN-inhoud weergeven in Azië. Elk land is met kleurcodering op deze kaart om aan te geven het percentage treffers die afkomstig uit die regio zijn.

## <a name="geography-reports-bar-charts"></a>Geografie rapporten (staafdiagrammen)

Er zijn twee aanvullende rapporten met statistische informatie op basis van de Geografie boven steden en Top landen. Deze rapporten rangschikken steden en landen, respectievelijk op basis van het aantal treffers die afkomstig van deze regio's zijn. Na het genereren van dit type rapport, geven een staafdiagram aan de bovenste 10 steden of landen die inhoud via een specifieke platform gevraagd. Deze staafdiagram worden geplaatst, kunt u snel inzicht in de regio's die de hoogste aantal aanvragen voor uw inhoud te genereren.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden in de opgegeven regio. Direct onder de grafiek (x-as) vindt u een label voor elk van de bovenste 10 regio's.

### <a name="using-the-bar-charts"></a>Gebruik van de staafdiagrammen

* Als u de muisaanwijzer op een balk, wordt de naam en het totale aantal treffers die zijn aangebracht in de regio worden weergegeven als knopinfo.
* De knopinfo voor het rapport boven steden kunt u een stad door de naam, provincie en Landafkorting identificeren.
* Als het plaats of de regio (dat wil zeggen provincie) waarvan een nieuw vergaderverzoek afkomstig kan niet worden bepaald, klikt u vervolgens deze geeft aan dat ze afkomstig onbekend zijn. Als het land is onbekend en klik vervolgens twee vraagteken (dat wil zeggen??), worden weergegeven.
* Een rapport eventueel aan de doelstellingen voor "Europa" of "Azië/Pacific." Deze items zijn niet bedoeld statistische informatie te verstrekken over alle IP-adressen in de regio's. In plaats daarvan deze alleen van toepassing op aanvragen die afkomstig zijn van IP-adressen die zijn verdeeld over de Europa of Azië in plaats van naar een specifieke plaats of land.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totale aantal treffers, het percentage treffers, de hoeveelheid gegevens worden overgedragen (in GB) en het percentage van de gegevens worden overgedragen voor de bovenste 250 regio's. Een beschrijving voor elk van deze gegevens bekijken.

Een korte beschrijving, is opgegeven voor beide soorten onderstaande rapporten.

Rapportnaam | Beschrijving
------------|------------
Bovenste steden | Dit rapport wordt de rang van steden op basis van het aantal treffers die afkomstig zijn uit die regio.
Bovenste landen | Dit rapport wordt de rang van landen op basis van het aantal treffers die afkomstig zijn uit die regio.

## <a name="daily-summary"></a>Dagelijkse samenvatting

Het rapport dagelijks overzicht kunt u het totale aantal treffers en verzonden over een bepaald platform dagelijks weergeven. Deze informatie kan worden gebruikt voor het snel onderscheiden CDN activiteit patronen. Bijvoorbeeld: dit rapport kunt u bepalen welke dagen ervaren hoger of lager is dan de verwachte verkeer.

Na het genereren van dit type rapport, een staafdiagram, vindt u een visueel weergegeven over de hoeveelheid platform / regiospecifieke aanvraag ervaren dagelijks gedurende de periode waarvoor de lijst. Deze uitgevoerd door een balk voor elke dag in het rapport weer te geven. De periode selecteren genoemd, bijvoorbeeld 'Laatste Week' genereert een staafdiagram met zeven balken. Elke staaf wordt het totale aantal treffers ervaren op die dag aangegeven.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op de opgegeven datum. Direct onder de grafiek (x-as), vindt u een label de datum geeft (opmaak: jjjj-MM-DD) voor elke dag in de lijst opgenomen.

> [AZURE.TIP] Als u de muisaanwijzer op een balk, wordt het totale aantal treffers die zijn aangebracht op die datum als knopinfo worden weergegeven.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elke dag waarvoor de lijst.

## <a name="by-hour"></a>Per uur

Het rapport per uur kunt u het totale aantal treffers en verzonden over een bepaald platform per uur worden berekend. Deze informatie kan worden gebruikt voor het snel onderscheiden CDN activiteit patronen. Bijvoorbeeld: dit rapport kunt u de perioden tijdens de dag die hoger of lager dan verwachte verkeer ondervinden detecteren.

Na het genereren van dit type rapport, wordt een staafdiagram wordt visueel weergegeven over de hoeveelheid platform / regiospecifieke aanvraag ervaring per uur worden berekend over de periode waarvoor de lijst. Deze uitgevoerd door een balk voor elk uur waarvoor de lijst weer te geven. Bijvoorbeeld selecteren van een 24-uurs periode genereert een staafdiagram met 24-balken. Elke staaf wordt het totale aantal treffers ervaren tijdens dat uur aangegeven.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op het opgegeven uur. Direct onder de grafiek (x-as), vindt u een label de datum/tijd geeft (opmaak: jjjj-MM-DD uu: mm) voor elk uur opnemen in de lijst. Tijd wordt aangegeven met 24-uursnotatie en dit is opgegeven met de UTC/GMT-tijdzone.

> [AZURE.TIP] Als u de muisaanwijzer op een balk, het totale aantal treffers die zijn aangebracht tijdens dat uur weergegeven als knopinfo.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elk uur waarvoor de lijst.

## <a name="by-file"></a>Door bestand

Het rapport per bestand kunt u het bedrag van de aanvraag en het verkeer weergegeven die zijn gemaakt op een bepaald platform voor de meest gevraagde activa weergeven. Een staafdiagram wordt na het genereren van dit type rapport gegenereerd op de bovenste 10 meest gevraagde activa gedurende de opgegeven periode.

> [AZURE.NOTE] Voor de toepassing van dit rapport, worden edge CNAME-URL's geconverteerd naar hun equivalente CDN-URL's. Hierdoor wordt een nauwkeurige overeenstemming voor het totale aantal treffers dat is gekoppeld aan een actief ongeacht het CDN of de rand CNAME-URL die wordt gebruikt deze moeten aanvragen.

De linkerkant van de grafiek (y-as) geeft het aantal aanvragen voor alle activa in de opgegeven periode. Direct onder de grafiek (x-as) vindt u een label die de bestandsnaam voor elk van de bovenste 10 gevraagde activa aangeeft.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Hier vindt u de volgende informatie voor elk van de bovenste 250 gevraagde activa: relatieve pad, het totale aantal treffers, het percentage treffers, de hoeveelheid gegevens worden overgedragen (in GB) en het percentage van de gegevens worden overgedragen.

## <a name="by-file-detail"></a>Door bestand Detail

Het rapport per bestand Detail kunt u het bedrag van de aanvraag en het verkeer weergegeven die zijn gemaakt op een bepaald platform voor een bepaald activum weergeven. Op de menubalk van dit rapport wordt met de optie voor meer informatie in het bestand. Deze optie biedt een lijst met uw meest gevraagde activa op de geselecteerde platform. Als u wilt een rapport door bestand Detail genereren, moet u om de gewenste activa van de optie voor meer informatie in het bestand te selecteren. Waarna, een staafdiagram wordt gemeld dat de hoeveelheid dagelijkse vraag die deze gegenereerd over de opgegeven periode.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat activa ervaring op een bepaalde dag. Direct onder de grafiek (x-as), vindt u een label de datum geeft (opmaak: jjjj-MM-DD) voor welke CDN aanvraag voor het activum is gerapporteerd.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totale aantal treffers en de hoeveelheid gegevens die worden overgebracht (in GB) voor elke dag waarvoor de lijst.

## <a name="by-file-type"></a>Per bestandstype

Het rapport op Type kunt u het bedrag van de aanvraag en het verkeer door bestandstype gemaakte weergeven. Na het genereren van dit type rapport, wordt een grafiek donut geven het percentage treffers gegenereerd door de bovenste 10 bestandstypen.

> [AZURE.TIP] Als u de muisaanwijzer op een segment in de grafiek donut, mediatype het Internet van dat bestandstype wordt weergegeven als knopinfo.

De gegevens die is gebruikt voor het genereren van het diagram donut kunnen worden bekeken eronder. Er vindt u het bestandstype naam extensie/Internet media, het totale aantal treffers, het percentage treffers, de hoeveelheid gegevens worden overgedragen (in GB) en het percentage van de gegevens worden overgedragen voor elk van de belangrijkste 250 bestandstypen.

## <a name="by-directory"></a>Door adreslijst

Het rapport door Directory kunt u het bedrag van de aanvraag en het verkeer weergegeven die zijn gemaakt op een bepaald platform voor inhoud van een bepaalde map weergeven. Na het genereren van dit type rapport, wordt het totale aantal treffers gegenereerd door inhoud in de bovenste 10 mappen worden aangegeven door een staafdiagram.

### <a name="using-the-bar-chart"></a>Het staafdiagram gebruiken

* Plaats de muisaanwijzer op een balk om weer te geven van de relatieve pad naar de bijbehorende map.
* Inhoud die is opgeslagen in een submap van een map worden niet meegeteld bij het berekenen van de aanvraag door directory. Deze berekening uitsluitend gebruikmaakt van het aantal aanvragen voor inhoud die is opgeslagen in de werkelijke adreslijst gegenereerd.
* Voor de toepassing van dit rapport, worden edge CNAME-URL's geconverteerd naar hun equivalente CDN-URL's. Hierdoor wordt een nauwkeurige overeenstemming voor alle statistieken die is gekoppeld aan een actief ongeacht het CDN of de rand CNAME-URL die wordt gebruikt deze moeten aanvragen.

De linkerkant van de grafiek (y-as) geeft het totale aantal verzoeken om de inhoud die zijn opgeslagen in uw mappen top 10. Elke staaf in het diagram vertegenwoordigt een map. Gebruik de kleurcodering schema kiezen op basis van een balk naar een map die wordt weergegeven in de sectie boven 250 volledige mappen.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Hier vindt u de volgende informatie voor elk van de mappen boven 250: relatieve pad, het totale aantal treffers, het percentage treffers, de hoeveelheid gegevens worden overgedragen (in GB) en het percentage van de gegevens worden overgedragen.

## <a name="by-browser"></a>Door de Browser

Het rapport per Browser kunt u bekijken welke browsers zijn gebruikt voor het aanvragen van inhoud. Na het genereren van dit type rapport, wordt een cirkeldiagram geven het percentage van aanvragen die zijn afgehandeld door de bovenste 10 browsers.

### <a name="using-the-pie-chart"></a>Het cirkeldiagram gebruiken

* Plaats de muisaanwijzer op een segment van het cirkeldiagram om de naam en de versie van een browser te geven.
* Voor de toepassing van dit rapport, kan elke combinatie van unieke browserversie wordt beschouwd als een andere browser.
* Het segment "Overig" genoemd, bevat het percentage van aanvragen die zijn afgehandeld door alle browsers en andere versies.

De gegevens die is gebruikt voor het genereren van het cirkeldiagram kunnen worden bekeken eronder. Er vindt u het type/versienummer van de browser, het totale aantal treffers en het percentage treffers voor elk van de belangrijkste 250-browsers.

## <a name="by-referrer"></a>Door Referrer

Het rapport door Referrer kunt u de belangrijkste verwijzingen tot inhoud weergeven op het geselecteerde platform. Een referrer wordt aangegeven dat de hostnaam van waaruit u een aanvraag is gegenereerd. Na het genereren van dit type rapport, wordt een staafdiagram geven aan het bedrag van de aanvraag (dat wil zeggen treffers) gegenereerd door de bovenste 10 verwijzingen.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat activa ervaring voor elke referrer aan. Elke staaf in het diagram vertegenwoordigt een referrer. Gebruik de kleurcodering schema kiezen op basis van een balk naar een referrer weergegeven in de sectie boven 250 Referrer.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Daar leert u de URL, het totale aantal treffers en het percentage treffers gegenereerd op basis van elk van de belangrijkste 250 verwijzingen.

## <a name="by-download"></a>Via downloaden

Het rapport door downloaden kunt u patronen downloaden voor de meest gevraagde inhoud analyseren. Boven aan het rapport bevat een staafdiagram dat vergelijkt downloads met voltooide downloads voor de bovenste 10 gevraagde activa geprobeerd. Elke staaf is voorzien van kleurcoderingen op basis van een poging tot download (blauw) of het downloaden van een voltooide (groen).

> [AZURE.NOTE] Voor de toepassing van dit rapport, worden edge CNAME-URL's geconverteerd naar hun equivalente CDN-URL's. Hierdoor wordt een nauwkeurige overeenstemming voor alle statistieken die is gekoppeld aan een actief ongeacht het CDN of de rand CNAME-URL die wordt gebruikt deze moeten aanvragen.

De linkerkant van de grafiek (y-as) wordt de bestandsnaam voor elk van de bovenste 10 gevraagde activa aangegeven. Direct onder de grafiek (x-as) vindt u labels die aangeven van het totale aantal downloads geprobeerd/voltooid.

Direct onder het staafdiagram worden geplaatst, de volgende informatie wordt weergegeven voor de bovenste 250 gevraagde activa: relatieve pad (inclusief bestandsnaam), het aantal keren dat deze volledig is gedownload, het aantal keren dat deze is aangevraagd en het percentage van aanvragen waardoor het downloaden van een voltooid.

> [AZURE.TIP] Onze CDN niet door een HTTP-client (dat wil zeggen browser) geïnformeerd wanneer activa volledig is gedownload. Hierdoor wat u hoeft te berekenen of activa volledig is gedownload op basis van de statuscodes en byte-bereik aanvragen. Het eerste wat dat we kijken wanneer deze berekening is of het verzoek resulteert in een 200 OK statuscode. Als dat het geval is, klikt u vervolgens we byte-bereik verzoeken om ervoor te zorgen dat ze de gehele activa bedekt. Tot slot vergelijken we de hoeveelheid gegevens die worden overgebracht naar de grootte van de gevraagde activa. Als de gegevens worden overgedragen gelijk aan of groter is dan de bestandsgrootte is en de aanvragen byte-bereik geschikt voor dat actief zijn, worden klikt u vervolgens de treffer geteld als het downloaden van een voltooid.
>
>Vanwege de interpretatie aard van dit rapport, moet u rekening mee de volgende punten die de consistentie en de nauwkeurigheid van dit rapport veranderen kunnen behouden.
>
>* Verkeerspatronen kunnen niet nauwkeurig worden voorspeld wanneer gebruikersagenten zich anders gedragen. Dit kan leiden tot resultaten van de voltooide downloaden die ouder dan 100 zijn %.
>* Activa die van HTTP geleidelijke downloaden gebruikmaken mogelijk niet nauwkeurig worden weergegeven door dit rapport. Dit is vanwege gebruikers willen verschillende plaatsen in een video.

## <a name="by-404-errors"></a>Door een 404-fouten

Het rapport door 404 fouten kunt u het type inhoud dat de meeste 404 niet gevonden statuscodes genereert identificeren. Boven aan het rapport bevat een staafdiagram voor de top 10-activa waarvoor een 404 niet gevonden statuscode is geretourneerd. Deze staafdiagram worden vergeleken het totale aantal aanvragen met aanvragen waardoor er een 404 niet gevonden statuscode voor deze activa. Elke staaf is voorzien van kleurcoderingen. Een gele balk wordt gebruikt om aan te geven dat het verzoek geleid tot een statuscode 404 niet gevonden. Een rode balk wordt gebruikt om aan te geven het totale aantal aanvragen voor het activum.

> [AZURE.NOTE] Voor de toepassing van dit rapport, moet u het volgende:
>
>* Een treffer vertegenwoordigt een verzoek voor activa ongeacht statuscode.
>* Rand CNAME-URL's worden geconverteerd naar hun equivalente CDN-URL's. Hierdoor wordt een nauwkeurige overeenstemming voor alle statistieken die is gekoppeld aan een actief ongeacht het CDN of de rand CNAME-URL die wordt gebruikt deze moeten aanvragen.

De linkerkant van de grafiek (y-as) wordt de bestandsnaam voor elk van de bovenste 10 gevraagde activa waardoor er een 404 niet gevonden statuscode aangegeven. Direct onder de grafiek (x-as) vindt u labels die geven het totale aantal aanvragen en het aantal aanvragen waardoor er een 404 niet gevonden statuscode.

Direct onder het staafdiagram worden geplaatst, de volgende informatie wordt weergegeven voor de bovenste 250 gevraagde activa: relatieve pad (inclusief bestandsnaam), het aantal aanvragen waardoor er een 404 niet gevonden statuscode het totale aantal keren dat de activa is aangevraagd en het percentage van aanvragen waardoor er een 404 niet gevonden statuscode.

## <a name="see-also"></a>Zie ook
* [Azure CDN-overzicht](cdn-overview.md)
* [Realtime stat in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met de regels-engine](cdn-rules-engine.md)
* [Rand prestaties analyseren](cdn-edge-performance.md)
