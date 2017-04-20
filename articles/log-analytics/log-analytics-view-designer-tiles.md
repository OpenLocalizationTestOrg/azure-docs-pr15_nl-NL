<properties
    pageTitle="Meld u aan analyses weergave ontwerpfunctie tegel verwijzing | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Log Analytics weergave ontwerpfunctie tegel verwijzing
De ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven.

Andere artikelen die beschikbaar zijn voor de ontwerpfunctie voor weergaven zijn:

- [Ontwerpfunctie voor weergaven](log-analytics-view-designer.md) - overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en aangepaste weergaven bewerken.
- [Visualisatie deel verwijzing](log-analytics-view-designer-parts.md) - overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven. 


De volgende tabel bevat de verschillende soorten tegels beschikbaar in de ontwerpfunctie voor weergaven.  De onderstaande secties worden elk tegeltype in detail en hun eigenschappen.

| Tegel | Beschrijving |
|:--|:--|
| [Getal](#number-tile) | Één getal met het aantal records uit een query. |
| [Twee getallen](#two-numbers-tile) | Twee één getallen met aantallen records uit twee verschillende query's. |
| [Donut](#donut-tile) | Donut grafiek op basis van een query met een samenvatting waarde in het beheercentrum. |
| [Lijndiagram en bijschrift](#line-chart-amp-callout-tile) | Lijndiagram op basis van een query en een bijschrift met een samenvatting waarde. |
| [Lijndiagram](#line-chart-tile) | Lijndiagram op basis van een query. |
| [Twee tijdlijnen](#two-timelines-tile) | Kolomdiagram met twee reeks die elk op basis van een andere query. |



## <a name="number-tile"></a>Getal-tegel

De tegel **getal** weergegeven één getallen met het aantal records uit een query log en een label.

![Getal-tegel](media/log-analytics-view-designer/tile-number.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| **Tegel** |
| Legenda | Tekst die moet worden weergegeven onder de waarde. |
| Query | De query om uit te voeren.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** |  **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |

## <a name="two-numbers-tile"></a>Tegel van twee getallen

De tegel **Twee getal** weergave van twee getallen met het aantal records uit twee verschillende log-query's en een label voor elk label.

![Tegel van twee getallen](media/log-analytics-view-designer/tile-two-numbers.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| **Eerste tegel** |
| Legenda | Tekst die moet worden weergegeven onder de waarde. |
| Query | De query om uit te voeren.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| **Tweede tegel** |
| Legenda | Tekst die moet worden weergegeven onder de waarde. |
| Query | De query om uit te voeren.  Het aantal het aantal records geretourneerd door de query wordt weergegeven. |
| **Geavanceerde** | **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |

## <a name="donut-tile"></a>Ring-tegel

De tegel **Ring** weergegeven één getallen uit een waardekolom in een query log samengevat.  De ring geeft resultaten van de drie records die bovenste grafisch weer.

![Ring-tegel](media/log-analytics-view-designer/tile-donut.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| **Donut** |
| Query | Query uitvoeren voor de ring.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten. |
| **Donut** | **> Centreren** |
| Tekst | Tekst die moet worden weergegeven onder de waarde in de ring. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde.<br><br>-Som: Telt de waarden van alle records met de waarde van de eigenschap.<br>-Percentage: Percentage van de opgetelde waarden uit de records met de waarde van de eigenschap vergeleken met de opgetelde waarden van alle records. |
| Resultaatwaarden gebruikt in beheercentrum bewerking | (Optioneel) Klik op het plusteken (+) om een of meer waarden te tellen.  De resultaten van de query worden beperkt tot records met de waarden van de eigenschappen die u opgeeft.  Als er geen waarden zijn toegevoegd, dan u alle records in de query vindt. |
| **Donut** | **> Extra opties** |
| Kleuren | De kleur voor elk van de drie belangrijkste eigenschappen weergeven.  Als u opgeven alternatieve kleuren voor specifieke eigenschapswaarden wilt, gebruik geavanceerde kleur toewijzen. |
| Geavanceerde kleur toewijzen | Hiermee wordt een kleur voor specifieke eigenschapswaarden weergegeven.  Als de waarde die u opgeeft in de bovenste drie, wordt klikt u vervolgens de alternatieve kleur weergegeven in plaats van de standaard kleur.  Als de eigenschap niet in de bovenste drie is, wordt klikt u vervolgens de kleur niet weergegeven. |
| **Geavanceerde** | **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |

## <a name="line-chart-tile"></a>File is Downloaded with

De tegel met **lijndiagram** geeft een lijndiagram met meerdere reeksen uit een query log na verloop van tijd.  

![Tegel met lijndiagram en bijschrift](media/log-analytics-view-designer/tile-line-chart.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| **Lijndiagram** |  
| Query | Query uitvoeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten.  Als de query wordt gebruikt voor het trefwoord **interval** wordt dit tijdsinterval gebruikt op de x-as van de grafiek.  Als de query niet bij het trefwoord **interval inbegrepen is** worden vervolgens per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die door de query.  Deze informatie wordt gebruikt om weer te geven van etiketten in het diagram waarin wordt aangegeven dat de typen en eventueel ook voor het converteren van de waarden.  Het **Type eenheid** Hiermee geeft u op de categorie van de eenheid en wordt de **Huidige eenheidstype** -waarden die beschikbaar zijn gedefinieerd.  Als u een waarde in het **converteren naar** worden vervolgens de numerieke waarden geconverteerd in het **Huidige eenheid** type naar het type **converteren naar** . |
| Aangepast etiket | Tekst die moet worden weergegeven voor de Y-as naast het label voor het type eenheden.  Als u geen label opgeeft, wordt alleen het type eenheden weergegeven. |
| **Geavanceerde** | **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |


## <a name="line-chart--callout-tile"></a>De grafiek en bijschrift tegel lijn

De tegel met **lijndiagram en bijschrift met** weergeven een lijndiagram met meerdere reeksen uit een query log over tijd en een bijschrift met een samengevatte waarde  

![Tegel met lijndiagram en bijschrift](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| **Lijndiagram** |  
| Query | Query uitvoeren voor het lijndiagram.  De eerste eigenschap moet een tekstwaarde en de tweede eigenschap een numerieke waarde.  Dit is meestal een query die het trefwoord **maateenheid** wordt gebruikt om samen te vatten resultaten.  Als de query wordt gebruikt voor het trefwoord **interval** wordt dit tijdsinterval gebruikt op de x-as van de grafiek.  Als de query niet bij het trefwoord **interval inbegrepen is** worden vervolgens per uur intervallen gebruikt voor de x-as. |
| **Lijndiagram** | **> Bijschrift** |
| Bijschrift | De tekst om weer te geven boven de waarde van het bijschrift. |
| De reeksnaam van de | Waarde onroerend goed voor de reeks wilt gebruiken voor de waarde van het bijschrift.  Als er geen reeks wordt geleverd, worden alle records door de query worden gebruikt. |
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde voor het bijschrift.<br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br><br>-Aantal: Telling van alle records geretourneerd door de query.<br>-Laatste voorbeeld: De waarde van het laatste interval opgenomen in de grafiek.<br>-Max: Maximumwaarde uit de intervallen die zijn opgenomen in de grafiek.<br>-Min: Minimumwaarde uit de intervallen die zijn opgenomen in de grafiek.<br>-Som: De som van de waarde van alle records. |
| **Lijndiagram** | **> Y-as** |
| Logaritmische schaal | Selecteer een logaritmische schaal gebruiken voor de y-as. |
| Eenheden | Geef de eenheden voor de waarden die door de query.  Deze informatie wordt gebruikt om weer te geven van etiketten in het diagram waarin wordt aangegeven dat de typen en eventueel ook voor het converteren van de waarden.  Het **Type eenheid** Hiermee geeft u op de categorie van de eenheid en wordt de **Huidige eenheidstype** -waarden die beschikbaar zijn gedefinieerd.  Als u een waarde in het **converteren naar** worden vervolgens de numerieke waarden geconverteerd in het **Huidige eenheid** type naar het type **converteren naar** . |
| Aangepast etiket | Tekst die moet worden weergegeven voor de Y-as naast het label voor het type eenheden.  Als u geen label opgeeft, wordt alleen het type eenheden weergegeven. |
| **Geavanceerde** | **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |

## <a name="two-timelines-tile"></a>Twee tijdlijnen-tegel

De tegel **twee tijdlijnen** geeft de resultaten van twee log-query's na verloop van tijd als kolomdiagrammen.  Een bijschrift wordt weergegeven voor elke reeks.  

![Twee tijdlijnen-tegel](media/log-analytics-view-designer/tile-two-timelines.png)

| Instelling | Beschrijving |
|:--|:--|
| Naam        | Tekst om weer te geven aan de bovenkant van de tegel. |
| Beschrijving | Tekst die moet worden weergegeven onder de naam van de tegel.    |
| Eerste grafiek   
| Legenda | Tekst die moet worden weergegeven onder het bijschrift voor de eerste reeks.
| Kleur | Kleur voor de kolommen in de eerste reeks.
| Grafiek-Query | Query uitvoeren voor de eerste reeks.  Het aantal het aantal records op elk tijdsinterval wordt aangegeven door de grafiekkolommen.
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde voor het bijschrift.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Aantal: Telling van alle records geretourneerd door de query.<br>-Laatste voorbeeld: De waarde van het laatste interval opgenomen in de grafiek.<br>-Max: Maximumwaarde uit de intervallen die zijn opgenomen in de grafiek.
| **Tweede grafiek** |
| Legenda | Tekst die moet worden weergegeven onder het bijschrift voor de tweede reeks.
| Kleur | Kleur voor de kolommen in de tweede reeks.
| Grafiek-Query | Query uitvoeren voor de tweede reeks.  Het aantal het aantal records op elk tijdsinterval wordt aangegeven door de grafiekkolommen.
| Bewerking | De bewerking uitvoeren op de eigenschap value om samen te vatten tot één enkele waarde voor het bijschrift.<br><br>-Gemiddelde: Het gemiddelde van de waarde van alle records.<br>-Aantal: Telling van alle records geretourneerd door de query.<br>-Laatste voorbeeld: De waarde van het laatste interval opgenomen in de grafiek.<br>-Max: Maximumwaarde uit de intervallen die zijn opgenomen in de grafiek. |
| **Geavanceerde** | **> Gegevensstroom-verificatie** |
| Ingeschakeld | Selecteer als gegevensstroom verificatie moet worden ingeschakeld voor de tegel.  Hierdoor wordt een alternatieve bericht als gegevens niet beschikbaar voor de tegel is.  Dit wordt meestal gebruikt op te geven van een bericht in de tijdelijke periode wanneer de weergave is geïnstalleerd en gegevens afkomstig zijn beschikbaar. |
| Query | Query om uit te voeren om te controleren of gegevens beschikbaar zijn voor de weergave.  Als de query geen resultaten worden geretourneerd, wordt een bericht weergegeven in plaats van de waarde van de belangrijkste query. |
| Bericht | Bericht dat wordt weergegeven als de gegevensstroom verificatie-query geen gegevens retourneert.  Als u geen bericht opgeeft, wordt *Uitvoering Assessment* weergegeven. |
| **Tijdsinterval** |
| Duur | De duur van de huidige datum gebruiken voor het tijdsinterval van de query.  Bijvoorbeeld als **7 dagen** is opgegeven, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van zeven dagen geleden op de huidige datum. |
| Einde gegevens verschuiving | Optionele offset van de huidige gegevens kunnen worden gebruikt voor het tijdsinterval van de belangrijkste query.  Bijvoorbeeld als **-1 dag** wordt gebruikt voor het **einde datum verschuiving** en **7 dagen** gebruikt voor de **duur**, klikt u vervolgens de query is beperkt tot de records die zijn gemaakt op basis van 8 dagen geleden naar gisteren. |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) ter ondersteuning van de query's in tegels.
- [Onderdelen van de visualisatie](log-analytics-view-designer-parts.md) toevoegen aan uw aangepaste weergave.