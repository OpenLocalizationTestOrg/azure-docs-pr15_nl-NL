<properties
    pageTitle="Prestaties van de rand in Azure CDN analyseren | Microsoft Azure"
    description="Prestaties van de rand knooppunt in Microsoft Azure CDN analyseren. Rand prestaties Analytics vindt u gedetailleerde informatie verkeer en bandbreedte gebruik voor de CDN."
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

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Prestaties van de rand knooppunt in Microsoft Azure CDN analyseren

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Rand prestaties analytics vindt u gedetailleerde informatie verkeer en bandbreedte gebruik voor de CDN. Deze informatie kan vervolgens worden gebruikt om te genereren trending statistieken, waarmee u kunt inzicht op hoe uw activa worden opgeslagen in de cache en afgeleverd in uw klanten. Op zijn beurt Hiermee kunt u een strategie formulier voor het optimaliseren van de bezorging van uw inhoud en om te bepalen welke problemen moeten worden aangepakt op betere middelen de CDN. Hierdoor niet alleen u kunnen gegevens bezorging prestaties te verbeteren, maar u wordt ook wel uw CDN verlagen.

> [AZURE.NOTE] Alle rapporten met de UTC/GMT notatie wanneer u een datum/tijd opgeeft.

## <a name="reports-and-log-collection"></a>Rapporten en verzamelen

CDN activiteitsgegevens worden verzameld door de rand prestaties Analytics-module voordat deze rapporten kunt genereren. Dit siteverzameling gebeurt wanneer een dag en deze behandelt de activiteit die hebben plaatsgevonden tijdens de vorige dag. Dit betekent dat van een rapport statistieken uitmaken van een steekproef van statistieken van de dag op het moment dat deze is verwerkt en handelingen niet per se bevatten de volledige reeks gegevens voor de huidige dag. De primaire functie van deze rapporten is om te beoordelen prestaties. Ze moeten niet worden gebruikt voor facturering doeleinden of exacte numerieke statistieken.

> [AZURE.NOTE] De onbewerkte gegevens waaruit rand prestaties analytische rapporten worden gegenereerd is beschikbaar voor ten minste 90 dagen duurt.

## <a name="dashboard"></a>Dashboard

Het dashboard voor gebruiksanalyses van rand-prestaties houdt de huidige en historische CDN-verkeer via een grafiek en statistieken. Gebruik dit dashboard om te bepalen de recente en de lange termijn trends op de prestaties van CDN-verkeer is toegestaan voor uw account.

Dit dashboard bestaat uit:

* Een interactieve grafiek waarmee de visualisatie van belangrijke statistieken en trends.
* Een tijdlijn vindt u een idee van langdurig patronen voor belangrijke statistieken en trends.
* Belangrijke maatstaven en statistische informatie over het verbetert onze CDN-netwerk siteverkeer door de algehele prestaties, gebruik en efficiëntie gemeten.

### <a name="accessing-the-edge-performance-dashboard"></a>Toegang krijgen tot het dashboard van de prestaties van rand

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-edge-performance/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Plaats de muisaanwijzer boven de tab **Analytics** en vervolgens de muis boven de **Rand leiden Analytics** : flyout.  Klik op **Dashboard**.

    Het dashboard voor gebruiksanalyses knooppunt rand wordt weergegeven.

### <a name="chart"></a>Grafiek

Het dashboard bevat een grafiek die wordt een meting bijgehouden over de periode die is geselecteerd in de tijdlijn die wordt weergegeven direct daaronder.  Een tijdlijn grafieken omhoog naar de laatste twee jaar van CDN activiteit wordt direct onder de grafiek weergegeven.

#### <a name="using-the-chart"></a>Gebruik van de grafiek

* Standaard wordt het tarief weer dat efficiency cache voor de afgelopen 30 dagen worden uitgezet.
* Deze grafiek wordt gegenereerd met gegevens die zijn gesorteerd op dagelijks.
* Aanwijzer boven een dag op het lijndiagram wordt aangegeven een datum en de waarde van de meetwaarde op die datum.
* Klik op Weekends markeren om te schakelen een overlay met lichtgrijze verticale balken die weekends naar de grafiek vertegenwoordigen. Dit soort overlay is handig om verkeerspatronen te identificeren via weekends.
* Klik op weergave één jaar geleden als u wilt schakelen tussen een overlay met de activiteit van het vorige jaar over de dezelfde periode naar de grafiek. Dit type vergelijking biedt inzicht in de lange termijn CDN gebruikspatronen. De hoek van de hand van de rechterbovenhoek van de grafiek bevat een legenda waarmee wordt aangegeven de kleurcode voor elke lijndiagram.

#### <a name="updating-the-chart"></a>Bijwerken van de grafiek

* Tijdsbereik: Voer een van de volgende opties:
    * Selecteer het gewenste gebied in de tijdlijn. De grafiek wordt bijgewerkt met gegevens die met de selecteerde tijdsperiode overeenkomt.
    * Dubbelklik op de grafiek om alle beschikbare historische gegevens met een maximum van twee jaar weer te geven.
* Metrisch: Klik op het pictogram dat wordt weergegeven naast de gewenste meetwaarde. De grafiek en de tijdlijn worden vernieuwd met de gegevens voor de bijbehorende meetwaarde.


### <a name="key-metrics-and-statistics"></a>Belangrijke aan de doelstellingen en statistieken worden aangepast

#### <a name="efficiency-metrics"></a>Aan de doelstellingen efficiency

Het doel van deze gegevens is om te zien of de cache efficiency kan worden verbeterd. De belangrijkste voordelen kleurencombinaties uit de cache efficiency zijn:

* Verminderde belasting op de server origin wat tot leiden kan:
    * Betere prestaties van web-server.
    * Lagere operationele kosten.
* Verbeterde gegevens bezorging hardwareversnelling aangezien meer aanvragen rechtstreeks vanuit de CDN moeten worden verzonden.

Veld | Beschrijving
------|------------
Cache Efficiency | Bevat het percentage van de gegevens die worden overgebracht die is dienden uit cache. Deze metrische maatregelen wanneer een in de cache opgeslagen versie van de gevraagde inhoud is served rechtstreeks vanuit de CDN (edge-servers) voor aanvragers (bijvoorbeeld webbrowser)
Frequentie van treffers | Bevat het percentage van aanvragen die zijn opgehaald uit de cache. Deze metrische maatregelen wanneer een in de cache opgeslagen versie van de gevraagde inhoud is served rechtstreeks vanuit de CDN (edge-servers) voor aanvragers (bijvoorbeeld webbrowser).
% van externe Bytes - geen Config Cache | Bevat het percentage van verkeer die vanaf servers origin is bereikbaar naar de CDN (edge-servers) die wordt niet worden opgeslagen in de cache als gevolg van de functie overslaan Cache (HTTP regels Engine).
% van externe Bytes - Cache verlopen | Bevat het percentage van verkeer die naar de CDN (edge-servers) vanaf servers origin is bereikbaar grond verouderde inhoud gevalideerd.

#### <a name="usage-metrics"></a>Gebruik de doelstellingen

Het doel van deze gegevens is te leveren inzicht in de volgende kosten-knippen maatregelen:

* Operationele kosten tot en met de CDN minimaliseren.
* CDN uitgaven via cache efficiency en compressie wordt afgetrokken.

> [AZURE.NOTE] Verkeer volume getallen geven verkeer dat is gebruikt in berekeningen van verhoudingen en percentages en mogelijk alleen een deel van de totale verkeer weergeven voor grote hoeveelheden klanten.

Veld | Beschrijving
------|------------
Opslaan Bytes uit | Geeft het gemiddelde aantal bytes dat is overgedragen voor elk verzoek om een van de CDN (edge-servers) bediend aan de aanvrager (bijvoorbeeld webbrowser).
Geen Cache Config Byte rente | Bevat het percentage van verkeer served uit de CDN (edge-servers) aan de aanvrager (bijvoorbeeld webbrowser) die wordt niet worden opgeslagen in de cache vanwege de functie overslaan Cache.
Gecomprimeerde Byte rente | Bevat het percentage van de CDN (edge-servers) naar aanvragers (bijvoorbeeld webbrowser) verzonden verkeer in een gecomprimeerde indeling.
Uitgaande bytes | Geeft de hoeveelheid gegevens, in bytes, die uit de CDN (edge-servers) zijn bezorgd bij de aanvrager (bijvoorbeeld webbrowser).  
Bytes In | Geeft aan de hoeveelheid gegevens, in bytes vanuit aanvragers (bijvoorbeeld webbrowser) verzonden naar de CDN (edge-servers).
Bytes Remote | Geeft de hoeveelheid gegevens, in bytes van CDN en de klantenservice origin-servers naar de CDN (edge-servers) verzonden.

#### <a name="performance-metrics"></a>Prestatiegegevens

Het doel van deze gegevens is voor het bijhouden van de algehele prestaties van CDN voor uw verkeer.

Veld | Beschrijving
------|------------
Tarief overbrengen | Geeft de gemiddelde snelheid waarmee inhoud van de CDN is overgedragen naar een aanvrager.
Duur | Geeft de gemiddelde tijd (in milliseconden), die nodig is voor het leveren van activa aan een aanvrager (bijvoorbeeld webbrowser).
Gecomprimeerde verzoek rente | Bevat het percentage treffers die uit de CDN (edge-servers) zijn bezorgd bij de aanvrager (bijvoorbeeld webbrowser) in een gecomprimeerde indeling.
4XX fout rente | Bevat het percentage treffers die een statuscode 4xx gegenereerd.
5xx fout rente | Bevat het percentage treffers die een statuscode 5xx gegenereerd.
Treffers | Geeft het aantal aanvragen voor CDN-inhoud.

#### <a name="secure-traffic-metrics"></a>Beveiligd verkeer aan de doelstellingen

Het doel van deze gegevens is voor het bijhouden van CDN prestaties voor HTTPS-verkeer is toegestaan.

Veld | Beschrijving
------|------------
Cache beveiligen Efficiency | Bevat het percentage van de gegevens die worden overgebracht voor HTTPS-verzoeken die zijn dienden uit cache. Deze metrische maatregelen wanneer een in de cache opgeslagen versie van de gevraagde inhoud is served rechtstreeks vanuit de CDN (edge-servers) voor aanvragers (bijvoorbeeld webbrowser) via HTTPS.
Veilige overdracht rente | Geeft de gemiddelde snelheid waarmee inhoud van de CDN (edge-servers) is overgedragen naar aanvragers (bijvoorbeeld endwebservers) via HTTPS.
Gemiddelde Secure duur | Geeft de gemiddelde tijd (in milliseconden), die nodig is voor het leveren van activa aan een aanvrager (bijvoorbeeld webbrowser) via HTTPS.
Secure treffers | Geeft het aantal HTTPS aanvragen voor CDN-inhoud.
Secure Bytes uit | Geeft de hoeveelheid HTTPS-verkeer, in bytes die uit de CDN (edge-servers) zijn bezorgd bij de aanvrager (bijvoorbeeld webbrowser).

## <a name="reports"></a>Rapporten

Elk rapport in deze module bevat een grafiek en statistieken over het gebruik van de bandbreedte en -verkeer is toegestaan voor verschillende soorten aan de doelstellingen (bijvoorbeeld HTTP-statuscodes, cache statuscodes, aanvragen URL, enzovoort). Deze informatie kan worden gebruikt voor dieper grondigere hoe inhoud wordt wordt served naar uw klanten en geworden CDN gedrag om gegevens bezorging prestaties te verbeteren.

### <a name="accessing-the-edge-performance-reports"></a>De rand prestatierapporten openen

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-edge-performance/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Plaats de muisaanwijzer boven de tab **Analytics** en vervolgens de muis boven de **Rand leiden Analytics** : flyout.  Klik op **de grote Object HTTP**.

    De rand knooppunt analytics-Rapporten scherm wordt weergegeven.

Rapport | Beschrijving
-------|------------
Dagelijkse samenvatting | Kunt u dagelijkse verkeer trends weergeven over een opgegeven periode. Elke staaf in deze grafiek voorstelt een bepaalde datum. De grootte van de balk geeft het totale aantal treffers die zijn aangebracht op die datum.
Overzicht van per uur | Kunt u weergeven per uur verkeer trends over een opgegeven periode. Elke staaf in deze grafiek voorstelt één uur op een bepaalde datum. De grootte van de balk geeft het totale aantal treffers die zijn aangebracht tijdens dat uur.
Protocollen | Geeft de uitsplitsing van verkeer tussen het HTTP en HTTPS-protocol. Een grafiek ring bevat het percentage treffers die zijn aangebracht voor elk type protocol.
HTTP-methoden | Kunt u een snelle beeld van waarmee HTTP-methoden worden gebruikt voor het aanvragen van uw gegevens te krijgen. De meest voorkomende HTTP-verzoek methoden zijn meestal ophalen, hoofd en POST. Een grafiek donut bevat het percentage treffers die zijn aangebracht voor elk type HTTP-verzoek methode.
URL 's | Een grafiek waarin u kunt de bovenste 10 gevraagde URL's bevat. Een balk wordt weergegeven voor elke URL. De hoogte van de balk geeft aan hoeveel treffers dat bepaalde URL is gegenereerd via de tijdspanne waarvoor de lijst. Statistieken voor de bovenste 100 aangevraagd dat URL 's direct onder deze grafiek worden weergegeven.
CNAME-records | Een grafiek waarin u kunt de top 10 CNAME-records gebruikt voor het aanvragen van activa over de tijd tijdspanne van een rapport bevat. Statistieken voor de bovenste 100 aangevraagd dat CNAME-records direct onder deze grafiek worden weergegeven.
Oorspronkelijke bestanden | Een grafiek waarin u kunt de bovenste 10 CDN of klant origin servers waaruit activa over een opgegeven tijdsperiode zijn aangevraagd bevat. Statistieken voor de bovenste 100 aangevraagd CDN- of klantgegevens origin-servers direct onder deze grafiek worden weergegeven. Klant origin servers worden geïdentificeerd door de naam die is gedefinieerd in de optie mapnaam.
Geografische POP 's | Ziet u hoeveel van uw verkeer wordt gerouteerd via een bepaald punt-van-aanwezigheid (POP). De afkorting van drie letters vertegenwoordigt een POP in onze CDN-netwerk.
Clients | Een grafiek waarin u kunt de bovenste 10 clients die aangevraagd van activa over een opgegeven tijdsperiode bevat. Voor de toepassing van dit rapport, alle aanvragen die afkomstig van hetzelfde IP-adres zijn worden beschouwd als van dezelfde client. Statistieken voor de bovenste 100 clients worden direct onder deze grafiek weergegeven. Dit rapport is handig voor het downloaden activiteit patronen vaststellen voor uw belangrijkste klanten.
Statussen die cache | Biedt een gedetailleerd overzicht van cache gedrag, die mogelijk methoden voor het verbeteren van de algehele eindgebruikers-ervaring. Aangezien de snelste prestaties afkomstig zijn uit de cachetreffers, kunt u gegevens bezorging snelheden door Minimalisatie van het aantal Cachemissers en verlopen cachetreffers kunt optimaliseren.
GEEN Details | Bevat een grafiek waarin u kunt de bovenste 10 URL's voor activa, waarvan de inhoud fris cache niet was ingeschakeld gedurende een opgegeven periode. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
CONFIG_NOCACHE Details | Een grafiek waarin u kunt de top 10-URL's voor activa die zijn niet in cache vanwege van de klant CDN configuratie bevat. Deze typen elementen zijn served rechtstreeks vanaf de oorspronkelijke server. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
UNCACHEABLE Details | Een grafiek waarin u kunt de bovenste 10 URL's voor activa die niet in de cache vanwege verzoek koptekstgegevens bevat. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
TCP_HIT Details | Een grafiek waarin u kunt de bovenste 10 URL's voor activa die direct worden verwerkt uit cache bevat. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
TCP_MISS Details | Een grafiek waarin u kunt de bovenste 10 URL's voor activa met een status van de cache van TCP_MISS bevat. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
TCP_EXPIRED_HIT Details | Een grafiek waarin u kunt de bovenste 10 URL's voor verouderde activa die zijn served rechtstreeks vanuit de POP bevat. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
TCP_EXPIRED_MISS Details | Een grafiek waarin u kunt de bovenste 10 URL's voor verouderde activa waarvoor een nieuwe versie had moeten worden opgehaald van de originele server bevat. Statistieken voor de bovenste 100 URL's voor deze soorten activa worden direct onder deze grafiek weergegeven.
TCP_CLIENT_REFRESH_MISS Details | Een staafdiagram waarin de top 10-URL's voor activa zijn opgehaald uit een bronserver vanwege een aanvraag geen-cache van de client bevat. Statistieken voor de bovenste 100 URL's voor deze soorten aanvragen worden direct onder deze grafiek weergegeven.
Clienttypen voor aanvraag | Geeft het type van aanvragen die zijn aangebracht door HTTP-clients (bijvoorbeeld browsers). Dit rapport bevat een ring-grafiek met een indruk dat hoe aanvragen nog worden verwerkt. Bandbreedte en verkeer informatie voor elk aanvraagtype wordt weergegeven onder de grafiek.
Gebruikersagent | Een staafdiagram weergeven van de bovenste 10 gebruikersagenten om aan te vragen van uw inhoud tot en met onze CDN bevat. Een gebruikersagent is meestal een webbrowser, Mediaspeler of de browser van een mobiele telefoon. Statistieken voor de bovenste 100 gebruikersagenten worden direct onder deze grafiek weergegeven.
Verwijzingen | Een staafdiagram weergeven van de belangrijkste 10 verwijzingen tot inhoud via onze CDN bevat. Een referrer is meestal de URL van de webpagina of resource die is gekoppeld aan uw inhoud. Hieronder de grafiek vindt u gedetailleerde informatie voor de bovenste 100 verwijzingen.
Compressietypen | Bevat een ring-grafiek die uitsplitsing van gevraagde activa of ze zijn gecomprimeerd met onze edge-servers. Het percentage van de gecomprimeerde activa is opgesplitst op het type compressie gebruikt. Hieronder de grafiek vindt u gedetailleerde informatie voor elk compressietype en status.
Bestandstypen | Bevat een staafdiagram waarin u kunt de bovenste 10 bestandstypen die zijn aangevraagd door onze CDN voor uw account. Voor de toepassing van dit rapport, een bestandstype is gedefinieerd door de bestandsextensie van het actief en Internet mediatype (bijvoorbeeld .html \[tekst/html\], .htm \[tekst/html\], .aspx \[tekst/html\], enz.). Hieronder de grafiek vindt u gedetailleerde informatie voor de bovenste 100 bestandstypen.
Unieke bestanden | Een grafiek die wordt getekend het totale aantal unieke activa die op een bepaalde dag zijn aangevraagd over een opgegeven tijdsperiode bevat.
Token Auth-overzicht | Een cirkeldiagram die een beknopt overzicht biedt op of de gevraagde activa zijn beveiligd met Token gebaseerde verificatie bevat. Beveiligde activa worden weergegeven in de grafiek op basis van de resultaten van hun poging tot verificatie.
Token Auth weigeren Details | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen die zijn geweigerd vanwege Token gebaseerde verificatie weergeven.
HTTP-antwoord Codes | Biedt een overzicht van de codes HTTP-status (bijvoorbeeld 200 OK, 403 verboden, 404 niet gevonden, enzovoort) die zijn afgeleverd in uw klanten HTTP door onze edge-servers. Een cirkeldiagram kunt u snel nagaan hoe uw activa zijn opgehaald. Gedetailleerde statistische gegevens worden weergegeven voor elke antwoordcode onder de grafiek.
404-fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een 404 niet gevonden antwoordcode weergeven.
403 fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een 403 antwoordcode weergeven. Een 403 code van de-antwoord wordt weergegeven wanneer een aanvraag is geweigerd door de bronserver van een klant of een randserver op onze POP.
4XX fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een antwoordcode in het bereik 400 weergeven. Dit rapport wordt gehouden 403 zijn niet gevonden en 404 antwoord codes. Meestal een 4xx antwoordcode wordt weergegeven wanneer een aanvraag is geweigerd grond van een Clientfout.
504 fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een 504 Gateway time-out antwoordcode weergeven. Een 504 Gateway time-out antwoordcode wordt weergegeven wanneer er een time-out optreedt wanneer een HTTP-proxy probeert te communiceren met een andere server. In het geval van onze CDN, een 504 Gateway time-out antwoordcode meestal doet zich voor als een edge-server niet kan worden tot stand brengen van de communicatie met de bronserver van een klant.
502 fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een 502 Ongeldige Gateway antwoordcode weergeven. Een 502 Ongeldige Gateway antwoordcode wordt weergegeven wanneer een HTTP-protocolfout tussen een server en een HTTP-proxy optreedt. In het geval van onze CDN, een 502 Ongeldige Gateway antwoordcode meestal vindt plaats wanneer een bronserver klant geeft als een ongeldige antwoord wordt verzonden naar een edgeserver resultaat. Een reactie is ongeldig als deze kan niet worden geparseerd of als het onvolledig is.
5XX fouten | Bevat een staafdiagram waarmee u kunt de bovenste 10 aanvragen waardoor er een antwoordcode in het bereik 500 weergeven.  Dit rapport wordt gehouden zijn 502 Ongeldige Gateway en 504 Gateway time-out antwoord codes.

## <a name="see-also"></a>Zie ook
* [Azure CDN-overzicht](cdn-overview.md)
* [Realtime stat in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met de regels-engine](cdn-rules-engine.md)
* [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
