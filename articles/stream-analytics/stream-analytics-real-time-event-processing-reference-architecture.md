<properties 
    pageTitle="Realtime gebeurtenis verwerken met Stream gebeurtenis analyseverwerking | Microsoft Azure" 
    description="Informatie over hoe een set van Azure services voor het inschakelen van realtime gebeurtenis verwerking en analyses kunt samenwerken." 
    keywords="realtime verwerking, verwerking van de gebeurtenis, verwijzing architectuur"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Overzicht van de architectuur: realtime gebeurtenis verwerken met Microsoft Azure Stream Analytics

De verwijzing architectuur voor realtime gebeurtenis verwerken met Azure Stream Analytics is bedoeld om u te bieden van een algemene blauwdruk voor de implementatie van een realtime platform als een service (PaaS) stream-verwerking-oplossing met Microsoft Azure.

## <a name="summary"></a>Overzicht

Traditioneel zijn analytics-oplossingen gebaseerd op mogelijkheden zoals ETL (uitpakken, transformeren, laden) en gegevensopslag, waar gegevens worden opgeslagen voordat u analyse. Veranderende vereisten, met inbegrip van meer snel binnenkomende gegevens, zet dit bestaande model op de limiet voor nieuwe. De mogelijkheid om het analyseren van gegevens binnen het verplaatsen van streams vóór opslag is een oplossing en het is niet een nieuwe mogelijkheid, de methode niet breed is vastgesteld in alle industriële verticalen. 

Microsoft Azure biedt een uitgebreide catalogus van analytics-technologieën die zijn die een matrix van verschillende oplossing scenario's en vereisten ondersteunt. Selecteren welke Azure services om te implementeren voor een end-to-end-oplossing, kan een gegeven van de breedte van de aanbiedingen van uitdaging zijn. Dit artikel is bedoeld om de mogelijkheden en samenwerking van de verschillende Azure services die ondersteuning bieden voor een gebeurtenis streaming-oplossing te beschrijven. Ook wordt uitgelegd sommige scenario's waarin klanten van dit type methode profiteren kunnen.

## <a name="contents"></a>Inhoud

- Samenvatting voor leidinggevenden
- Inleiding tot realtime Analytics
- Toegevoegde waarde van realtimegegevens in Azure wordt aangegeven
- Gebruikelijke scenario's voor realtime Analytics
- Architectuur en de onderdelen
    - Gegevensbronnen
    - Gegevens-integratie laag
    - Realtime Analytics laag
    - Gegevens opslaglaag
    - Presentatie / verbruik van lagen
- Sluiten

**Auteur:** Jeroen Feddersen, oplossing ontwerper, Datacenter inzichten van Excellence, Microsoft Corporation

**Gepubliceerd:** Januari 2015

**Revisie:** 1.0

**Downloaden:** [Realtime gebeurtenis verwerken met Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
