<properties 
    pageTitle="Hiermee kunt u het configureren van gegevens voor taken van de Stream Analytics | Microsoft Azure" 
    description="Uitvoer configureren voor taken van de Stream Analytics | leren padsegment."
    keywords="gegevens uitvoert, verplaatsing van gegevens"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Hiermee kunt u het configureren van gegevens voor Stream Analytics-taken

Azure Stream Analytics-taken worden verbonden met een of meer gegevens-uitvoer, waarin een verbinding met een bestaande datasink definieert. Als uw taak Stream Analytics verwerkt en binnenkomende gegevens worden omgezet, wordt een reeks gegevens uitvoer gebeurtenissen naar de uitvoer van uw taak geschreven.

Stream Analytics-gegevens uitvoer kunnen worden gebruikt om de gegevensbron realtime dashboards of waarschuwingen, gegevens verkeer werkstromen activeren, of gewoon archiveren gegevens voor het batch later verwerken. Stream Analytics heeft eerste-klas integratie met verschillende Azure-services, die hier worden beschreven.

Een uitvoer aan uw taak Stream Analytics toevoegen:

1. In de klassieke Azure-Portal, klik op de **uitvoer van** en klik op **Uitvoer toevoegen** in uw Stream Analytics-taak.

    ![Uitvoer van toevoegen](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Klik op de tegel **uitvoer** in uw Stream Analytics-taak in de portal van Azure.

    ![Azure Portal uitvoer toevoegen](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Geef het type van de uitvoer:

    ![Kies verplaatsing van gegevenstype](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure-portal Kies verplaatsing van gegevenstype](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Een beschrijvende naam voor deze uitvoer in het vak **Uitvoeralias** opgeven. Deze naam kan worden gebruikt in de query van uw taak later te verwijzen naar de uitvoer.  
    
    Vul de rest van de vereiste verbindingseigenschappen verbinding maken met uw uitvoer.  Deze velden is afhankelijk van het uitvoertype en zijn gedefinieerd in detail hier.  

    ![Eigenschappen van de uitvoer gegevens toevoegen](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Afhankelijk van het uitvoertype moet u mogelijk opgeven hoe de gegevens zijn serienummer of opgemaakt. De specifieke serialisatie-instellingen voor elk uitvoertype worden hier beschreven.

    Vul de rest van de vereiste verbindingseigenschappen verbinding maken met uw gegevensbron. Deze velden is afhankelijk van het type van invoer en de bron en zijn gedefinieerd in detail [hier](stream-analytics-create-a-job.md).  

    ![Gegevensuitvoer toevoegen aan gebeurtenis hub](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Azure portal gegevensuitvoer op gebeurtenis hub](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Elk element uitvoer is toegevoegd aan de taak, moet bestaan voordat de taak is gestart en gebeurtenissen die doorloopt. Bijvoorbeeld als u blobopslag als een uitvoer gebruikt, maakt de taak niet een opslag-account automatisch. Deze moet worden gemaakt door de gebruiker om de taak ASA is gestart.

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
