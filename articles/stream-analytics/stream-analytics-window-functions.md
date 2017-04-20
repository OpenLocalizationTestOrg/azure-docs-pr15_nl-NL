<properties
    pageTitle="Inleiding tot functies van de Stream Analytics venster | Microsoft Azure"
    description="Meer informatie over de drie venster-functies in Stream Analytics (tumbling, hopping, schuiven)."
    keywords="tumbling venster venster hopping venster schuiven"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Inleiding tot functies van de Stream Analytics-venster

In veel realtime streaming scenario's, is het benodigde bewerkingen alleen op de gegevens in de tijdelijke windows uit te voeren. Ondersteuning voor windowing functies is een belangrijke functie van Azure Stream Analytics die Hiermee verplaatst u de naald op ontwikkelaars productiviteit in authoring taken complexe stream verwerkt. Stream Analytics kan ontwikkelaars [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) en [**beweegbare**](https://msdn.microsoft.com/library/dn835051.aspx) windows gebruiken om te tijdelijke bewerkingen uitvoeren op streaming gegevens. Het is handig om te weten dat alle bewerkingen van het [venster](https://msdn.microsoft.com/library/dn835019.aspx) resultaten aan het **einde** van het venster uitvoeren. De uitvoer van het venster worden enkele gebeurtenis op basis van de statistische functie die wordt gebruikt. De gebeurtenis heeft het tijdstempel van het einde van het venster en alle functies van het venster worden gedefinieerd met een vaste lengte. Ten slotte is het belangrijk te weten dat alle functies van het venster in een [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) -component moeten worden gebruikt.

![Stream Analytics-venster functies concepten](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Gewor-venster

Tumbling venster functies worden gebruikt voor een gegevensstroom segmenteren in afzonderlijke tijd segmenten en uitvoeren van een functie tegen deze, zoals in het onderstaande voorbeeld. De belangrijkste differentiators van het venster gewor zijn dat ze herhalen, elkaar niet overlappen en een gebeurtenis kan geen deel uitmaakt van meer dan één gewor-venster.

![Stream Analytics-venster functies inleidende tumbling](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Venster Hopping

Hopping venster functies hop doorsturen in door een bepaalde termijn. Het is mogelijk dat deze eenvoudig als het ware ze gewor windows die overlappen kunnen, zodat gebeurtenissen deel van meer dan één resultatenset voor Hopping-venster uitmaken kunnen. Als u wilt maken van een venster Hopping hetzelfde als een gewor geeft venster één gewoon de hop grootte als u wilt niet hetzelfde zijn als het vensterformaat. 

![Stream Analytics venster werkt inleidende hopping](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Schuifregelaar venster

Schuifregelaar venster-functies, in tegenstelling tot gewor of Hopping windows, produceren een uitvoer **alleen** wanneer een gebeurtenis plaatsvindt. Alle vensters zichtbaar moet ten minste één gebeurtenis en het venster continu springt doorsturen een € (epsilon). Gebeurtenissen kunnen zoals Windows Hopping behoren tot meer dan één venster schuiven.

![Stream Analytics venster werkt schuifregelaar inleidende](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Hulp bij het venster functies

Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
