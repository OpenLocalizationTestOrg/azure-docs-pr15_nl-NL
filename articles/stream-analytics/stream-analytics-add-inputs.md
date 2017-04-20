<properties
    pageTitle="Een invoer van gegevens aan uw projecten Stream Analytics toevoegen | Microsoft Azure"
    description="Leer hoe u een gegevensbron aan uw project Stream Analytics als invoer van gegevens van gebeurtenis Hubs of verwijzing gegevens uit de Blog opslag streaming aansluiten."
    keywords="gegevens invoert, gegevensstromen"
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


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Een streaming gegevens invoer of een verwijzing gegevens toevoegen aan een taak Stream Analytics

Leer hoe u een gegevensbron aan uw project Stream Analytics als invoer van gegevens uit de gebeurtenis Hubs of een verwijzing gegevens uit Blob storage streaming aansluiten.

Azure Stream Analytics-taken kunnen worden verbonden aan invoer van gegevens voor één of meer, die elk een verbinding met een bestaande gegevensbron definieert. Gegevens is verzonden naar die gegevensbron, is deze worden gebruikt door de Stream Analytics-taak en verwerkt in realtime als streaming gegevens. Stream Analytics heeft eerste-klas integratie met [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure-blobopslag](../storage/storage-dotnet-how-to-use-blobs.md) binnen en buiten van het project-abonnement.

In dit artikel is een stap in de [Stream Analytics leerpad](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Invoer van gegevens: gegevens en referentiegegevens Streaming

Er zijn twee soorten distinct invoeritems in Stream Analytics: gegevensstromen en referentiegegevens.

- **Gegevensstromen**: Stream Analytics taken moeten ten minste één gegevensinvoer stream worden verbruikt en getransformeerd door de taak bevatten. Azure-blobopslag en Azure gebeurtenis Hubs worden ondersteund als gegevens stream invoerbronnen. Azure gebeurtenis Hubs worden gebruikt voor het verzamelen van gebeurtenis streams uit verbonden apparaten, services en toepassingen. Azure-blobopslag kan worden gebruikt als een invoerbron voor het bulksgewijs gegevens als een stream ingesting.  
- **Referentiegegevens**: Stream Analytics ondersteunt een tweede soort referentiegegevens aux, invoer en genoemd.  In tegenstelling tot de gegevens in beweging is deze gegevens statische of vertraging wijzigen.  Dit wordt meestal gebruikt voor de uitvoering op te zoeken en correlatie met gegevensstromen maken van een uitgebreidere gegevensverzameling.  Azure-blobopslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens.  

Een invoer voor uw werk Stream Analytics toevoegen:

1. In de portal van Azure **invoeritems** klikt u op en klik op **toevoegen een invoer** in de Stream Analytics-taak.

    ![Azure klassieke portal - een invoertaal toevoegen.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Klik op de tegel **invoeritems** in uw Stream Analytics-taak in de portal van Azure.  

    ![Azure-portal - gegevensinvoer toevoegen.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Het type van de invoer opgeven: **gegevensstroom** of **referentiegegevens**.

    ![Input, streamen of verwijzen naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Input, streamen of verwijzen naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Als u maakt een invoer gegevensstroom, geeft u het brontype voor de invoer.  Deze stap worden overgeslagen tijdens het maken van referentiegegevens als alleen Blob storage wordt ondersteund op dit moment.

    ![Gegevensinvoer stream gegevens toevoegen](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Gegevensinvoer stream gegevens toevoegen](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Geef een beschrijvende naam voor deze invoer in het vak Invoerbereik Alias.  Deze naam wordt gebruikt in de query van uw taak later te verwijzen naar de invoer.

    Vul de rest van de vereiste verbindingseigenschappen verbinding maken met uw gegevensbron. Deze velden is afhankelijk van het type van invoer en de bron en zijn gedefinieerd in detail [hier](stream-analytics-create-a-job.md).  

    ![Hub-gegevensinvoer gebeurtenis toevoegen](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Geef de serialisatie-instellingen voor de invoergegevens:
    - Om te controleren of uw query's werkt zoals verwacht, geef de **Gebeurtenis serialisatie-indeling** van binnenkomende gegevens.  Ondersteunde serialisatie indelingen zijn JSON, CSV- en Avro.
    - Controleer of de **codering** voor de gegevens.  De enige ondersteunde codering indeling is UTF-8 op dit moment.

    ![Instellingen voor het serialisatie van gegevens voor de gegevensinvoer](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Instellingen voor het serialisatie van gegevens voor de gegevensinvoer](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Na het voltooien van de invoer aanmaakdatum, verifiëren Stream Analytics dat deze verbinding met de invoerbron maken kunt.  U kunt de status van de bewerking testverbinding weergeven in de hub melding.

    ![De verbindingstest van de streaming gegevensinvoer](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![De verbindingstest van de streaming gegevensinvoer](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Hulp krijgen bij streaming invoer van gegevens
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
