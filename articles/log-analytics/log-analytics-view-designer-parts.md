<properties
    pageTitle="Meld u Analytics-ontwerpfunctie voor weergaven | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de instellingen voor elk van de visualisatie onderdelen beschikbaar voor gebruik in uw aangepaste weergaven."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Log Analytics-ontwerpfunctie voor weergaven visualisatie deel verwijzing
De ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties met gegevens uit de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de instellingen voor elk van de visualisatie onderdelen beschikbaar voor gebruik in uw aangepaste weergaven.

Andere artikelen die beschikbaar zijn voor de ontwerpfunctie voor weergaven zijn:

- [Ontwerpfunctie voor weergaven](log-analytics-view-designer.md) - overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en aangepaste weergaven bewerken.
- [Overzicht van de tegel](log-analytics-view-designer-tiles.md) - overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven. 

De volgende tabel beschrijft de verschillende typen beschikbaar in de ontwerpfunctie voor weergaven tegels.  De onderstaande secties worden elk tegeltype in detail en hun eigenschappen.

| Weergavetype | Beschrijving |
|:--|:--|
| [Lijst met query 's](#list-of-queries-part) | Bevat een overzicht van log zoekopdrachten gaat uitvoeren.  De gebruiker kunt klikken op elke query om de resultaten weer te geven.  |
| [Getal & lijst](#number-amp-list-part) | Koptekst heeft een één getal waarin telling van records uit een zoekopdracht log.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd. |
| [Twee getallen en lijst](#two-numbers-amp-list-part) | Koptekst heeft twee getallen met het aantal records in een apart logboek zoekopdrachten gaat uitvoeren.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd. |
| [Donut & lijst](#donut-amp-list-part) | Koptekst weergegeven één getallen uit een waardekolom in een query log samengevat.  De ring geeft resultaten van de drie records die bovenste grafisch weer. |
| [Twee tijdlijnen & lijst](#two-timelines-amp-list-part) | Koptekst geeft de resultaten van twee log-query's na verloop van tijd als kolomdiagrammen met een bijschrift met een enkel getal samengevat uit de waardekolom van een in een query log.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd. |   
| [Informatie](#information-part) | Koptekst weergeven statische tekst en een optionele koppeling  Lijst met bevat een of meer items met statische tekst en titel. |
| [Lijndiagram, bijschrift en lijst](#line-chart-callout-amp-list-part) | Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst gedurende een bepaalde tijd een bijschrift met een samengevatte waarde.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd. |
| [Lijst- en lijndiagrammen](#line-chart-amp-list-part) | Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst na verloop van tijd.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd. |
| [Stapel lijn grafieken onderdeel](#stack-of-line-charts-part) | Hiermee wordt drie afzonderlijke lijndiagrammen met meerdere reeksen uit een query log weergegeven na verloop van tijd. |


## <a name="list-of-queries-part"></a>Lijst met deel van de query 's

Bevat een overzicht van log zoekopdrachten gaat uitvoeren.  De gebruiker kunt klikken op elke query om de resultaten weer te geven.  De weergave al dan niet standaard één query bevat en u kunt op **+ Query** als u wilt toevoegen van extra query's.

![Lijst met query's weergeven](media/log-analytics-view-designer/view-list-queries.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Titel | Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Vooraf geselecteerde filters | Door komma's gescheiden lijst met eigenschappen die u kunt opnemen in het deelvenster links filter wanneer de gebruiker selecteert een query. |
| Weergave van de modus | Eerste weergave weergegeven wanneer de query is geselecteerd.  De gebruiker kan alle beschikbare weergaven na het openen van de query selecteren. |
| **Query 's** |
| Zoekquery | De query om uit te voeren. |
| Beschrijvende naam | Beschrijvende naam van de query weer te geven voor de gebruiker. |


## <a name="number--list-part"></a>Getal & lijst onderdeel

Koptekst heeft een één getal waarin telling van records uit een zoekopdracht log.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd.


![Lijst met query's weergeven](media/log-analytics-view-designer/view-number-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst.
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Titel** |
| Legenda | Tekst om weer te geven aan de bovenkant van de koptekst. |
| Query | Query uitvoeren voor de kop.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  De eerste twee eigenschappen voor de eerste tien records in de zoekresultaten wordt weergegeven.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de opdracht sorteren in de query de records sorteren in de lijst.  De gebruiker kan alle Zie om de query uitvoeren en alle records klikken. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Naam en een scheidingsteken | Eén teken scheidingsteken als u wilt de eigenschap text parseren in meerdere waarden.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |


## <a name="two-numbers--list-part"></a>Twee getallen & deel van de lijst

Koptekst heeft twee getallen met het aantal records in een apart logboek zoekopdrachten gaat uitvoeren.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd.

![Twee getallen en lijstweergave](media/log-analytics-view-designer/view-two-numbers-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de weergave. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst.
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Titel** |
| Legenda | Tekst om weer te geven aan de bovenkant van de koptekst. |
| Query | Query uitvoeren voor de kop.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  De eerste twee eigenschappen voor de eerste tien records in de zoekresultaten wordt weergegeven.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Balken worden automatisch gemaakt op basis van de relatieve waarde van de numerieke kolom.<br><br>Gebruik de opdracht sorteren in de query de records sorteren in de lijst.  De gebruiker kan alle Zie om de query uitvoeren en alle records klikken. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking moet worden uitgevoerd voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam en een scheidingsteken | Eén teken scheidingsteken als u wilt de eigenschap text parseren in meerdere waarden.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="donut--list-part"></a>Donut & lijst onderdeel

Koptekst weergegeven één getallen uit een waardekolom in een query log samengevat.  De ring geeft resultaten van de drie records die bovenste grafisch weer.

![Donut & lijst weergave](media/log-analytics-view-designer/view-donut-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Koptekst** |
| Titel | Tekst om weer te geven aan de bovenkant van de koptekst.
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst.
| **Donut** |
| Query | Query uitvoeren voor de ring.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde. |
| **Donut** |  **> Centreren** |
| Tekst | Tekst die moet worden weergegeven onder de waarde in de ring. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde.<br><br>-Som: Telt de waarden van alle records.<br>-Percentage: Percentage van de records geretourneerd door de waarden in de **resultaatwaarden gebruikt in beheercentrum bewerking** naar het totaal aantal records in de query. |
| Resultaatwaarden gebruikt in beheercentrum bewerking | (Optioneel) Klik op het plusteken (+) om een of meer waarden te tellen.  De resultaten van de query worden beperkt tot records met de waarden van de eigenschappen die u opgeeft.  Als geen waarden zijn toegevoegd, klikt u vervolgens alle records die zijn opgenomen in de query. |
| **Aanvullende opties** | **> Kleuren** |
| Kleur 1<br>Kleur 2<br>Kleur 3 | Selecteer de kleur voor elk van de waarden in de ring weergegeven. |
| **Aanvullende opties** | **> Geavanceerde kleur toewijzen** |
| Veldwaarde | Typ de naam van een veld weer te geven als een andere kleur als deze is opgenomen in de ring. |
| Kleur | Selecteer de kleur voor het veld unieke. |
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking moet worden uitgevoerd voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam en een scheidingsteken | Eén teken scheidingsteken als u wilt de eigenschap text parseren in meerdere waarden.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="two-timelines--list-part"></a>Twee tijdlijnen & lijst onderdeel

Koptekst geeft de resultaten van twee log-query's na verloop van tijd als kolomdiagrammen met een bijschrift met een enkel getal samengevat uit de waardekolom van een in een query log.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd.

![Twee tijdlijnen & lijst weergeven](media/log-analytics-view-designer/view-two-timelines-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Eerst in een grafiek<br>tweede grafiek** |
| Legenda | Tekst die moet worden weergegeven onder het bijschrift voor de eerste reeks. |
| Kleur | De kleur voor de kolommen in de reeks wilt gebruiken. |
| Query | Query uitvoeren voor de eerste reeks.  Het aantal het aantal records op elk tijdsinterval wordt aangegeven door de grafiekkolommen. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde voor het bijschrift.<br><br>-Som: De som van de waarde van alle records.<br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Laatste voorbeeld: De waarde van het laatste interval opgenomen in de grafiek.<br>-Eerste voorbeeld: De waarde van het eerste interval opgenomen in de grafiek.<br>-Aantal: Telling van alle records geretourneerd door de query.|
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking moet worden uitgevoerd voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="information-part"></a>Informatiegedeelte

Koptekst weergeven statische tekst en een optionele koppeling  Lijst met bevat een of meer items met statische tekst en titel.

![Weergave](media/log-analytics-view-designer/view-information.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Kleur | Achtergrondkleur voor de kop. |
| **Koptekst** |
| Afbeelding | Het afbeeldingsbestand om weer te geven in de koptekst. |
| Label | Tekst als u wilt weergeven in de koptekst. |
| **Koptekst** | **> Koppeling** |
| Label | Koppelingstekst. |
| URL | De URL voor de koppeling. |
| **Gegevensitems** |
| Titel | Tekst die moet worden weergegeven voor titel van elk item. |
| Inhoud | Tekst die moet worden weergegeven voor elk item. |


## <a name="line-chart-callout--list-part"></a>Lijndiagram, bijschrift en deel van de lijst

Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst gedurende een bepaalde tijd een bijschrift met een samengevatte waarde.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd.

![Lijndiagram, bijschrift en lijstweergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Koptekst** |
| Titel | Tekst om weer te geven aan de bovenkant van de koptekst. |
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst. |
| **Lijndiagram** |
| Query | Query uitvoeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten.  Als de query wordt gebruikt voor het trefwoord **interval** wordt dit tijdsinterval gebruikt op de x-as van de grafiek.  Als de query niet bij het trefwoord **interval inbegrepen is** worden vervolgens per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Bijschrift** |
| Bijschrift met titel | Tekst om weer te geven boven de waarde van het bijschrift. |
| De reeksnaam van de | Waarde onroerend goed voor de reeks wilt gebruiken voor de waarde van het bijschrift.  Als er geen reeks wordt geleverd, worden alle records door de query worden gebruikt. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde voor het bijschrift.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Aantal telling van alle records geretourneerd door de query.<br>-Laatste voorbeeld: De waarde van het laatste interval opgenomen in de grafiek.<br>-Max: Maximumwaarde uit de intervallen die zijn opgenomen in de grafiek.<br>-Min: Minimumwaarde uit de intervallen die zijn opgenomen in de grafiek.<br>-Som: De som van de waarde van alle records. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die door de query.  Deze informatie wordt gebruikt om weer te geven van etiketten in het diagram waarin wordt aangegeven dat de typen en eventueel ook voor het converteren van de waarden.  Het Type eenheden Hiermee geeft u op de categorie van de eenheid en wordt de huidige eenheidstype-waarden die beschikbaar zijn gedefinieerd.  Als u een waarde in selecteert converteren naar en vervolgens de numerieke waarden van het type huidige eenheid worden geconverteerd naar de converteren om te typen. |
| Aangepast etiket | Tekst die moet worden weergegeven voor de Y-as naast het label voor het type eenheden.  Als u geen label opgeeft, wordt alleen het type eenheden weergegeven. |
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking moet worden uitgevoerd voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam en een scheidingsteken | Eén teken scheidingsteken als u wilt de eigenschap text parseren in meerdere waarden.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="line-chart--list-part"></a>De grafiek en de lijst deel lijn

Een lijndiagram met meerdere reeksen uit een query log weergegeven koptekst na verloop van tijd.  Lijst met bevat de bovenste tien resultaten van een query met een grafiek waarin wordt aangegeven dat de relatieve waarde van een numerieke kolom of de wijzigen na verloop van tijd.

![Lijn-grafiek & lijst weergave](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst. |
| Pictogram gebruiken | Selecteer het pictogram weergeven. |
| **Koptekst** |
| Titel | Tekst om weer te geven aan de bovenkant van de koptekst. |
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de koptekst. |
| **Lijndiagram** |
| Query | Query uitvoeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten.  Als de query wordt gebruikt voor het trefwoord **interval** wordt dit tijdsinterval gebruikt op de x-as van de grafiek.  Als de query niet bij het trefwoord **interval inbegrepen is** worden vervolgens per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die door de query.  Deze informatie wordt gebruikt om weer te geven van etiketten in het diagram waarin wordt aangegeven dat de typen en eventueel ook voor het converteren van de waarden.  Het Type eenheden Hiermee geeft u op de categorie van de eenheid en wordt de huidige eenheidstype-waarden die beschikbaar zijn gedefinieerd.  Als u een waarde in selecteert converteren naar en vervolgens de numerieke waarden van het type huidige eenheid worden geconverteerd naar de converteren om te typen. |
| Aangepast etiket | Tekst die moet worden weergegeven voor de Y-as naast het label voor het type eenheden.  Als u geen label opgeeft, wordt alleen het type eenheden weergegeven. |
| **Lijst** |
| Query | Query uitvoeren voor de lijst.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| Graph verbergen | Selecteer in de grafiek aan de rechterkant van de numerieke kolom uitschakelen. |
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Kleur | De kleur van de balken of sparklines. |
| Bewerking | De bewerking moet worden uitgevoerd voor de sparkline.  Zie [Gemeenschappelijke instellingen](#sparklines) voor meer informatie. |
| Naam en een scheidingsteken | Eén teken scheidingsteken als u wilt de eigenschap text parseren in meerdere waarden.  Zie [Gemeenschappelijke instellingen](#name-value-separator) voor meer informatie. |
| Navigatie-query | De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  Zie [Gemeenschappelijke instellingen](#navigation-query) voor meer informatie. |
| **Lijst** | **> Kolomtitels** |
| Naam | Tekst om weer te geven aan de bovenkant van de eerste kolom van de lijst. |
| Waarde | Tekst om weer te geven aan de bovenkant van de tweede kolom van de lijst. |
| **Lijst** | **> Drempelwaarden** |
| Drempelwaarden inschakelen | Selecteer drempelwaarden inschakelen.  Zie [Gemeenschappelijke instellingen](#thresholds) voor meer informatie. |

## <a name="stack-of-line-charts-part"></a>Stapel lijn grafieken onderdeel

Hiermee wordt drie afzonderlijke lijndiagrammen met meerdere reeksen uit een query log weergegeven na verloop van tijd.

![Gestapelde lijndiagrammen](media/log-analytics-view-designer/view-stack-line-charts.png)

| Instelling | Beschrijving |
|:--|:--|
| **Algemene** |
| Groepstitel | Tekst om weer te geven aan de bovenkant van de tegel. |
| Nieuwe groep | Selecteer een nieuwe groep maken in de weergave vanaf de huidige weergave. |
| Pictogram | Het afbeeldingsbestand om weer te geven naast het resultaat in de koptekst. |
| **Grafiek 1<br>grafiek 2<br>3 in een grafiek** | **> Koptekst** |
| Titel | Tekst om weer te geven aan het begin van de grafiek. |
| Ondertitel | De tekst moet worden weergegeven onder de titel aan de bovenkant van de grafiek. |
| **Grafiek 1<br>grafiek 2<br>3 in een grafiek** | **Lijndiagram** |
| Query | Query uitvoeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten.  Als de query wordt gebruikt voor het trefwoord **interval** wordt dit tijdsinterval gebruikt op de x-as van de grafiek.  Als de query niet bij het trefwoord **interval inbegrepen is** worden vervolgens per uur intervallen gebruikt voor de x-as. |
| **Grafiek** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die door de query.  Deze informatie wordt gebruikt om weer te geven van etiketten in het diagram waarin wordt aangegeven dat de typen en eventueel ook voor het converteren van de waarden.  Het Type eenheden Hiermee geeft u op de categorie van de eenheid en wordt de huidige eenheidstype-waarden die beschikbaar zijn gedefinieerd.  Als u een waarde in selecteert converteren naar en vervolgens de numerieke waarden van het type huidige eenheid worden geconverteerd naar de converteren om te typen. |
| Aangepast etiket | Tekst die moet worden weergegeven voor de Y-as naast het label voor het type eenheden.  Als u geen label opgeeft, wordt alleen het type eenheden weergegeven. |

## <a name="common-settings"></a>Algemene instellingen
De volgende secties worden gemeenschappelijke instellingen voor verschillende onderdelen van de visualisatie.

### <a name="name-value-separator">Naam en een scheidingsteken</a>
Eén teken scheidingsteken als u wilt de eigenschap text uit een lijstquery parseren in meerdere waarden.  Als u een scheidingsteken opgeeft, kunt u namen voor elk veld gescheiden door dezelfde als scheidingsteken in het vak naam opgeven.

Stel een eigenschap met de naam van de *locatie* die waarden zoals *Redmond-Building 41* en *Bellevue-Building12*opgenomen.  U kunt opgeven – voor de naam en scheidingsteken en *Plaats Building* voor de naam.  Dit zou elke waarde parseren in twee eigenschappen *plaats* en *Building*genoemd. 

### <a name="navigation-query">Navigatie-query</a>
De query worden uitgevoerd wanneer de gebruiker een item in de lijst selecteren.  *{Het geselecteerde item}* gebruiken om op te nemen de syntaxis voor een item dat de gebruiker is geselecteerd.

Als de query een kolom met de naam van de *Computer* en de navigatie-query heeft is bijvoorbeeld *{geselecteerde item}*, een query, zoals *Computer = "Computer"* zou worden uitgevoerd wanneer de gebruiker een computer geselecteerd.  Als de query navigatie *Type gebeurtenis {geselecteerde item} =* vervolgens de query *Type gebeurtenis Computer = "Computer" =* zou worden uitgevoerd.

### <a name="sparklines">Sparklines</a>
Een sparkline is een kleine lijndiagram dat de waarde van de lijstinvoer in een na verloop van tijd weergeeft.  Voor de onderdelen van de visualisatie met een lijst, kunt u selecteren of een horizontale balk die aangeeft dat de relatieve waarde van een numerieke kolom of een sparkline die aangeeft dat de waarde na verloop van tijd wordt weergegeven.

De volgende tabel beschrijft de instellingen voor sparklines.

| Instelling | Beschrijving |
|:--|:--|
| Sparklines inschakelen | Selecteer om sparkline in plaats van de horizontale balk weer te geven. |
| Bewerking | Als sparklines zijn ingeschakeld, is dit de bewerking uitvoeren op elke eigenschap in de lijst voor het berekenen van de waarden voor de sparkline.<br><br>-Laatste voorbeeld: De laatste waarde voor de reeks over het tijdsinterval.<br>-Max: Maximumwaarde voor de reeks over het tijdsinterval.<br>-Min: Minimumwaarde voor de reeks over het tijdsinterval.<br>-Som: De som van de waarden voor de reeks over het tijdsinterval.<br>-Overzicht: Gebruikt de opdracht met dezelfde afmetingen als de query in de koptekst. |

### <a name="thresholds">Drempelwaarden</a>
Drempelwaarden kunnen u een gekleurde pictogram naast elk item in een lijst waarin u een snelle visuele indicator van items die groter zijn dan een bepaalde waarde of een bepaald bereik vallen weergeven.  U kan bijvoorbeeld een groen pictogram voor items met een waarde niet is toegestaan, geel als de waarde in een bereik die aangeeft van een waarschuwing en rood weergegeven als de werkmap een foutwaarde als resultaat overschrijdt.

Wanneer u drempelwaarden voor een deel inschakelt, moet u een of meer drempelwaarden.  Als de waarde van een item groter dan de drempelwaarde en lager is dan de volgende drempelwaarde is, wordt die kleur gebruikt.  Als het item groter dan vervolgens hoogste drempelwaarde is, wordt die kleur ingesteld.   

Elke set drempel heeft een drempelwaarde met een waarde van de **standaard**.  Dit is de kleur die is ingesteld als er geen andere waarden worden overschreden.  U kunt toevoegen of verwijderen van drempelwaarden door te klikken op de **+** of knop **x** .

De volgende tabel beschrijft de instellingen voor tresholds.

| Instelling | Beschrijving |
|:--|:--|
| Drempelwaarden inschakelen | Selecteer een Kleurenpictogram links van elke waarde die aangeeft dat de servicestatus ten opzichte van de opgegeven drempelwaarden weergeven. |
| Naam | De naam voor de drempelwaarde. |
| Drempelwaarde | De waarde voor de drempelwaarde.  De kleur van de servicestatus voor elk item is ingesteld op de kleur van de hoogste drempelwaarde met de waarde van het artikel overschreden.  Er is een standaard-drempel die de kleur als u geen drempelwaarde wordt overschreden. |
| Kleur | Kleur voor de drempelwaarde. |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in de visualisatie onderdelen.