<properties
   pageTitle="Log Analytics-gegevens exporteren naar Power BI | Microsoft Azure"
   description="Power BI is een cloud gebaseerd business analytics-service van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets met gegevens biedt.  Log Analytics kunt continu gegevens exporteren uit de bibliotheek OMS in Power BI zodat u van de visualisaties en hulpprogramma's voor gegevensanalyse gebruikmaken kunt.  In dit artikel wordt beschreven hoe query's in Log Analytics die automatisch naar Power BI met regelmatige tussenpozen exporteren configureren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Log Analytics-gegevens exporteren naar Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een cloud gebaseerd business analytics-service van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets met gegevens biedt.  Log Analytics kunt automatisch gegevens exporteren uit de bibliotheek OMS in Power BI zodat u van de visualisaties en hulpprogramma's voor gegevensanalyse gebruikmaken kunt.

Wanneer u Power BI met Log analyses configureren, maakt u log-query's die hun resultaten naar een overeenkomstige gegevenssets in Power BI exporteren.  De query en exporteren blijft automatisch worden uitgevoerd op een planning die u definieert als de gegevensset up-to-date houden met de meest recente gegevens die worden verzameld door Log Analytics.

![Log-analyses op Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-planningen

Een *Power BI planning* bevat een logboek zoeken die een reeks gegevens uit de bibliotheek OMS exporteert naar een overeenkomstige gegevensset in Power BI en een planning waarmee wordt gedefinieerd hoe vaak deze zoekopdracht wordt uitgevoerd de gegevensset als actueel wilt houden.

De velden in de gegevensset komen overeen met de eigenschappen van de records die het resultaat van de zoekfunctie.  Als de zoekactie records van verschillende typen retourneert vervolgens bevat de gegevensset alle eigenschappen uit elk van de opgenomen recordtypen.  

> [AZURE.NOTE] Dit is een goede gewoonte gebruik van een zoekopdracht log die resulteert in onbewerkte gegevens in plaats van een consolidatie met opdrachten, zoals [maateenheid](log-analytics-search-reference.md#measure)uitvoert.  U kunt een aggregatie en berekeningen in Power BI uitvoeren vanaf de onbewerkte gegevens.

## <a name="connecting-oms-workspace-to-power-bi"></a>OMS werkruimte verbinden met Power BI

Voordat u van Log analyseprogramma voor Power BI exporteren kunt, moet u uw werkruimte OMS koppelen aan uw Power BI-account met de volgende procedure.  

1. Klik op de tegel van de **Instellingen** in de OMS-console.
2. **Accounts**selecteren.
3. Klik op **verbinding maken met Power BI-Account**in de sectie **Informatie over de werkruimte** .
4. Voer de referenties voor uw Power BI-account.

## <a name="create-a-power-bi-schedule"></a>Een Power BI-planning maken

Maak een Power BI-planning voor elke gegevensset met de volgende procedure.

1. Klik op de tegel **Log zoeken** in de OMS-console.
2. Typ een nieuwe query of Selecteer een opgeslagen zoekactie die de gegevens die u wilt exporteren naar **Power BI**geretourneerd.  
3. Klik op de knop **Power BI** boven aan de pagina het **Power BI** -dialoogvenster openen.
4. Vul de informatie in de volgende tabel en klik op **Opslaan**.

| Eigenschap | Beschrijving |
|:--|:--|
| Naam | De naam voor de planning wanneer u de lijst met Power BI-planningen weergeeft. |
| Opgeslagen zoekactie | De zoekfunctie om uit te voeren.  U kunt de huidige query selecteren of Selecteer een bestaande opgeslagen zoekactie in de vervolgkeuzelijst. |
| Planning | Hoe vaak u de opgeslagen zoekopdracht uitvoeren en exporteren naar de Power BI-gegevensset.  De waarde moet liggen tussen 15 minuten en 24 uur. |
| De naam van de gegevensset | De naam van de gegevensset in Power BI.  Deze worden als deze niet gemaakt en bijgewerkt als deze nog bestaat. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Weergeven en verwijderen van Power BI-planningen

Bekijk de lijst met bestaande Power BI-schema's met de volgende procedure.

1. Klik op de tegel van de **Instellingen** in de OMS-console.
2. Selecteer **Power BI**.

Naast de details van de planning, worden het aantal keren dat de planning is uitgevoerd in de afgelopen week en de status van de laatste synchronisatie weergegeven.  Als de synchronisatie fouten opgetreden, kunt u de koppeling om uit te voeren log zoeken naar records met details van de fout klikt u op.

U kunt een planning verwijderen door te klikken op de **X** in de **kolom verwijderen**.  U kunt een planning uitschakelen door in te schakelen **uit**.  Als u wilt wijzigen van een planning moet u deze verwijderen en opnieuw te maken met de nieuwe instellingen.

![Power BI-planningen](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Voorbeeld Stapsgewijze instructies
De volgende sectie begeleidt bij een voorbeeld van een Power BI-planning maken en gebruiken van de gegevensset een eenvoudig rapport maken.  In dit voorbeeld alle prestatiegegevens voor een set computers is geëxporteerd naar Power BI en vervolgens een lijndiagram om weer te geven processorgebruik is gemaakt.

### <a name="create-log-search"></a>Log zoeken maken
We beginnen met het maken van een zoekopdracht log voor de gegevens die we wilt verzenden naar de gegevensset.  In dit voorbeeld gebruiken we een query die resulteert in alle prestatiegegevens voor computers met een naam die met *srv begint*.  

![Power BI-planningen](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Maken van Power BI zoeken
We klikt u op de knop **Power BI** om het Power BI-dialoogvenster openen en de vereiste informatie opgeven.  We horen deze zoekopdracht uitvoeren één keer per uur en maken van een gegevensset *Contoso Perf*genoemd.  Omdat we al de zoekopdracht openen waarmee de gegevens we horen, wordt de standaardinstelling van *de huidige zoekopdracht gebruiken* voor **Zoekprogramma's opgeslagen**behouden.

![Power BI zoeken](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Controleer of de Power BI zoeken
Om te bevestigen dat we de planning correct hebt gemaakt, wordt de lijst van Power BI zoekopdrachten onder de tegel **-Instellingen** weergeven in het dashboard OMS.  We wacht enkele minuten en deze weergave worden vernieuwd voordat deze rapporten dat de synchronisatie is uitgevoerd.

![Power BI zoeken](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Controleer of de gegevensset in Power BI
We Meld u aan bij onze account [powerbi.microsoft.com](http://powerbi.microsoft.com/) en gaat u naar **gegevenssets** onderaan in het linkerdeelvenster.  U ziet dat de gegevensset *Contoso Perf* wordt weergegeven dat aangeeft dat onze exporteren met succes is uitgevoerd.

![Power BI gegevensset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Op basis van de gegevensset-rapport maken
We de gegevensset **Contoso Perf** selecteren en klikt op **resultaten** in het deelvenster **velden** aan de rechterkant om de velden die deel uitmaken van deze dataset te bekijken.  Als u wilt een lijndiagram met processorgebruik voor elke computer hebt gemaakt, wordt de volgende acties uitvoeren.

1. Selecteer de regel aan de slag.
2. Sleep **objectnaam** naar **niveau rapportfilter** en **Processor**controleren.
3. Sleep **CounterName** naar **niveau rapportfilter** en **percentage processortijd**controleren.
4. Sleep **tegenwaarde** naar **waarden**.
5. Sleep de **Computer** naar **legenda**.
6. Sleep **TimeGenerated** naar **as**.

U ziet dat de resulterende lijngrafiek met de gegevens van onze gegevensset wordt weergegeven.

![Power BI-lijndiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Het rapport opslaan
We het rapport opslaat door te klikken op de knop opslaan boven aan het scherm en valideren dat deze wordt nu in het gedeelte rapporten in het linkerdeelvenster weergegeven.

![Power BI-rapporten](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekopdrachten in het logboek](log-analytics-log-searches.md) maken van query's die kunnen worden geëxporteerd naar Power BI.
- Meer informatie over [Power BI](http://powerbi.microsoft.com) maken op basis van Log Analytics exports visualisaties.
