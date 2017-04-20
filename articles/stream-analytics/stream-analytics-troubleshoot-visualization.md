<properties
    pageTitle="Visualiseren en taken van de Stream Analytics oplossen | Microsoft Azure"
    description="Leer hoe u een Stream Analytics taak pijplijn voor selfservice-problemen oplossen met de functie van de diagram diagnostische gegevens visualiseren."
    keywords=""
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


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualiseren en problemen met Stream Analytics-taken

In Stream Analytics, net als met andere technologieën cloudgebaseerde is probleemoplossing soms nodig om te zoeken naar waarom een taak geen levert de verwachte uitvoer (of uitvoer eigenlijk). Met dit in gedachten biedt Stream Analytics de mogelijkheid voor het visualiseren van een streaming taak. Dit is ook handig als een hulpmiddel voor het modelleren en kant voordeel voor deze waarvoor documentatie van hun werk is.

In het deelvenster visualisatie zijn de invoer alsmede de query wordt uitgevoerd en klikt u vervolgens alle de uitvoer van geconfigureerd zichtbaar. Connectiviteit of configuratie problemen kunnen duidelijker en kan ook het handig zijn om een visuele weergave van uw configuratie weer te geven.

## <a name="using-the-diagnosis-diagram-tool"></a>Hulpprogramma voor het diagnose-diagram

Voor toegang tot deze visualizer, klik u op de knop 'Diagnose diagram' in het blad "Instellingen" van de van de Stream Analytics-taak.

![Stream-Analytics-Troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Alle invoer en uitvoer is kleurcode om aan te geven van de huidige status van die onderdeel, zoals hieronder wordt weergegeven.

![Stream-Analytics-Troubleshoot-visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Wanneer de gebruiker wil bekijken tussenliggende query-stappen voor meer informatie over de patronen voor het stroom van gegevens binnen een project, bevat het hulpmiddel visualisatie een weergave van de verdeling van de query in de bijbehorende stappen onderdeel en de volgorde van de stroom. Klikken op elke querystap, wordt de bijbehorende sectie in een query bewerken deelvenster zoals weergegeven. 

![Stream-Analytics-Troubleshoot-visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
