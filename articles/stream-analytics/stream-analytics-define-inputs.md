<properties
    pageTitle="Gegevensverbinding: Data stream invoeritems uit de stroom van een gebeurtenis | Microsoft Azure"
    description="Meer informatie over het instellen van een gegevensverbinding met Stream Analytics 'invoer' genoemd. Ingangen een gegevensstroom van gebeurtenissen opnemen, en ook verwijzen naar gegevens."
    keywords="gegevensstroom, gegevensverbinding, gebeurtenis stream"
    services="stream-analytics"
    documentationCenter=""
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

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Gegevensverbinding: meer informatie over gegevens stream invoer van gebeurtenissen voor Stream Analytics

De gegevensverbinding met Stream Analytics is een gegevensstroom van gebeurtenissen uit een gegevensbron. Heet dit een 'invoer." Stream Analytics heeft eersteklas integratie met Azure stream bronnen gebeurtenis Hub IoT Hub en Blob gegevensopslag van het dezelfde of een andere Azure abonnement als uw analytics-taak.

## <a name="data-input-types-data-stream-and-reference-data"></a>Gegevens typen input: gegevens stream- en verwijzingsfuncties gegevens
Als u gegevens naar een gegevensbron is verplaatst, is deze verbruikt door de Stream Analytics-taak en verwerkt in realtime. Ingangen worden onderverdeeld in twee verschillende typen: gegevens streamen invoeritems en verwijzen naar de ingevoerde gegevens.

### <a name="data-stream-inputs"></a>Gegevensstroom ingangen
Een gegevensstroom is niet-gebonden reeks gebeurtenissen die zich binnenkort na verloop van tijd. Stream Analytics taken moeten ten minste één gegevensinvoer stream worden verbruikt en getransformeerd door de taak bevatten. Blobopslag, gebeurtenis Hubs en IoT Hubs worden ondersteund als gegevens stream invoerbronnen. Gebeurtenis Hubs worden gebruikt voor het verzamelen van gebeurtenis streams uit meerdere apparaten en -services, zoals sociale media activiteit-feeds, voorraad trade gegevens of gegevens van sensoren. IoT Hubs zijn geoptimaliseerd voor het verzamelen van gegevens van verbonden apparaten in Internet van dingen (IoT) scenario's.  Blobopslag kan worden gebruikt als een invoerbron voor het bulksgewijs gegevens als een stream ingesting.  

### <a name="reference-data"></a>Referentiegegevens
Stream Analytics ondersteunt een tweede type invoer referentiegegevens genoemd. Dit is aanvullende gegevens die is statische of langzaam wijzigen na verloop van tijd en wordt meestal gebruikt voor de uitvoering van de correlatie en op te zoeken. Azure-blobopslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens. Verwijzing gegevensbron BLOB's zijn beperkt tot 100MB groot.
Als u wilt weten hoe u de verwijzing invoer van gegevens maken, raadpleegt u de [Gegevens van de verwijzing gebruiken](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Een stream invoer van gegevens met de Hub van een gebeurtenis maken

[Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) zijn zeer scalable gebeurtenis ingestor publicatie-abonnementen. Dit kan miljoenen gebeurtenissen per seconde verzamelen zodat u kunt verwerken en analyseren van de enorme hoeveelheid gegevens die zijn gemaakt met uw verbonden apparaten en toepassingen. Dit is een van de meest gebruikte ingangen voor Stream Analytics. Gebeurtenis Hubs en Stream Analytics bieden samen klanten een end-to-end oplossing voor realtime analytics. Gebeurtenis Hubs kunnen klanten gebeurtenissen in realtime ontstaan Azure en Stream Analytics taken kunnen verwerken in realtime. Klanten kunnen bijvoorbeeld web klikken, sensor metingen, online gebeurtenissen verzenden naar gebeurtenis Hubs en Stream Analytics-taken voor de gebeurtenis Hubs gebruiken als de invoergegevens-streams voor het filteren van realtime, aggregeren en correlatie maken.

Het is belangrijk te weten dat de standaard-tijdstempel van gebeurtenissen die afkomstig zijn uit gebeurtenis Hubs in Stream Analytics de tijdstempel die de gebeurtenis zijn aangekomen in gebeurtenis Hub namelijk EventEnqueuedUtcTime is. Als u wilt de gegevens als een gegevensstroom met een tijdstempel in de gebeurtenis nettolading verwerken, moet het trefwoord [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) worden gebruikt.

### <a name="consumer-groups"></a>Groepen consumenten

De invoer van elke Stream Analytics gebeurtenis Hub moet worden geconfigureerd hebben een eigen groep consumenten. Wanneer een taak Self-joins of meerdere ingangen bevat, kan bepaalde invoer door meer dan één lezer voorbij, waarmee het aantal lezers in een groep één consumenten invloed worden gelezen. Als u wilt voorkomen dat gebeurtenis Hub limiet van 5 lezers per groep consumenten per partition, is een goede gewoonte om aan te geven van een groep consumenten voor elke taak Stream Analytics. Houd er rekening mee dat er ook een limiet van 20 groepen voor consumenten per gebeurtenis Hub is. Zie de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md)voor meer informatie.

### <a name="configure-event-hub-as-an-input-data-stream"></a>Gebeurtenis-Hub configureren als een stroom invoergegevens

De onderstaande tabel wordt elke eigenschap uitgelegd in het geval Hub tabblad met de beschrijving Input:

| NAAM VAN EIGENSCHAP | BESCHRIJVING |
|------|------|
| Invoer Alias | Een beschrijvende naam die wordt gebruikt in de query taak om te verwijzen naar deze invoer |
| Service Bus Namespace | Een Service Bus naamruimte is een container voor een reeks entiteiten messaging. Wanneer u een nieuwe gebeurtenis Hub hebt gemaakt, worden ook de naamruimte van een Service Bus gemaakt. |
| Gebeurtenis-Hub | De naam van de gebeurtenis Hub invoer. |
| De naam van een gebeurtenis Hub-beleid | Het gedeelde-beleid, die kan worden gemaakt op het tabblad gebeurtenis Hub configureren. Elk gedeelde-beleid wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Gebeurtenis-Hub beleid-toets | De Shared toegangstoets gebruikt om te verifiëren toegang tot de Service Bus naamruimte. |
| Gebeurtenis-Hub consumenten groep (optioneel) | De groep consumenten voor het nemen van gegevens uit de Hub gebeurtenis. Als u niet opgeeft, bestaat Stream Analytics taken uit het gebruik van de groep consumenten standaard naar het nemen van gegevens uit de Hub gebeurtenis. Het wordt aanbevolen een afzonderlijke consumenten groep gebruiken voor elke taak Stream Analytics. |
| Gebeurtenis serialisatie-indeling | Om ervoor te zorgen dat uw query's werkt zoals verwacht, Stream Analytics moet weten welke serialisatie-indeling (JSON, CSV- of Avro) u gebruikt voor binnenkomende gegevensstromen. |
| Codering | De enige ondersteunde codering indeling is UTF-8 op dit moment. |

Als uw gegevens is die afkomstig zijn uit een bron van gebeurtenis-Hub, kunt u toegang tot enkele metagegevensvelden in uw query Stream Analytics. De onderstaande tabel bevat de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt door Stream Analytics. |
| EventEnqueuedUtcTime | De datum en tijd waarop de gebeurtenis is ontvangen door de gebeurtenis Hubs. |
| PartitionId | De nul partition-ID voor de invoer adapter. |

U mogelijk bijvoorbeeld een query als volgt schrijven:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Een IoT Hub-gegevensinvoer stream maken

Azure Iot Hub is een zeer scalable publiceren abonnementen gebeurtenis ingestor geoptimaliseerd voor IoT scenario's.
Het is belangrijk te weten dat de standaard-tijdstempel van gebeurtenissen die afkomstig zijn uit IoT Hubs in Stream Analytics de tijdstempel die de gebeurtenis zijn aangekomen in IoT-Hub, dat wil EventEnqueuedUtcTime zeggen is. Als u wilt de gegevens als een gegevensstroom met een tijdstempel in de gebeurtenis nettolading verwerken, moet het trefwoord [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) worden gebruikt.

> [AZURE.NOTE] Alleen de berichten die zijn verzonden met een eigenschap DeviceClient kunnen worden verwerkt.

### <a name="consumer-groups"></a>Groepen consumenten

De invoer van elke Stream Analytics IoT Hub moet worden geconfigureerd hebben een eigen groep consumenten. Wanneer een taak Self-joins of meerdere ingangen bevat, kan bepaalde invoer door meer dan één lezer voorbij, waarmee het aantal lezers in een groep één consumenten invloed worden gelezen. Als u wilt voorkomen dat IoT Hub limiet van 5 lezers per groep consumenten per partition, is een goede gewoonte om aan te geven van een groep consumenten voor elke taak Stream Analytics.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>IoT Hub configureren als een gegevensstroom invoer

De onderstaande tabel wordt elke eigenschap in de IoT Hub invoer tabblad met de beschrijving beschreven:

| NAAM VAN EIGENSCHAP | BESCHRIJVING |
|------|------|
| Invoer Alias | Een beschrijvende naam die wordt gebruikt in de query taak om te verwijzen naar deze invoer. |
| IoT Hub | Een IoT Hub is een container voor een reeks berichten entiteiten. |
| Eindpunt | De naam van uw IoT Hub-eindpunt. |
| De naam van de gedeelde Access-beleid | Het gedeelde-beleid voor toegang tot de IoT Hub. Elk gedeelde-beleid wordt een naam hebben, machtigingen die u hebt ingesteld, en de toegangstoetsen. |
| Beleid voor gedeelde toegangstoets | De Shared toegangstoets gebruikt om te verifiëren toegang tot de IoT Hub. |
| Consumenten groep (optioneel) | De groep consumenten naar gegevens uit de IoT Hub nemen. Als u niet opgeeft, bestaat Stream Analytics taken uit het gebruik van de groep consumenten standaard naar gegevens uit de IoT Hub nemen. Het wordt aanbevolen een afzonderlijke consumenten groep gebruiken voor elke taak Stream Analytics. |
| Gebeurtenis serialisatie-indeling | Om ervoor te zorgen dat uw query's werkt zoals verwacht, Stream Analytics moet weten welke serialisatie-indeling (JSON, CSV- of Avro) u gebruikt voor binnenkomende gegevensstromen. |
| Codering | De enige ondersteunde codering indeling is UTF-8 op dit moment. |

Als uw gegevens is die afkomstig zijn uit een bron IoT-Hub, kunt u toegang tot enkele metagegevensvelden in uw query Stream Analytics. De onderstaande tabel bevat de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt. |
| EventEnqueuedUtcTime | De datum en tijd waarop de gebeurtenis is ontvangen door de IoT Hub. |
| PartitionId | De nul partition-ID voor de invoer adapter. |
| IoTHub.MessageId | Wordt gebruikt om te relateren communicatie in twee richtingen in IoT Hub. |
| IoTHub.CorrelationId | In antwoorden en feedback in IoT Hub gebruikt. |
| IoTHub.ConnectionDeviceId | De geverifieerde id die wordt gebruikt voor het verzenden van dit bericht is een tijdstempel op berichten die servicebound door IoT Hub. |
| IoTHub.ConnectionDeviceGenerationId | De generationId van de geverifieerde apparaat gebruikt voor het verzenden van dit bericht is een tijdstempel op berichten die servicebound door IoT Hub. |
| IoTHub.EnqueuedTime | De tijd waarop het bericht is ontvangen door IoT Hub. |
| IoTHub.StreamId | Aangepaste gebeurteniseigenschap toegevoegd door het apparaat van de afzender. |

## <a name="create-a-blob-storage-data-stream-input"></a>Een Blob storage gegevensinvoer stream maken

Scenario's met grote hoeveelheden ongestructureerde gegevens worden opgeslagen in de cloud, biedt blobopslag een efficiënt en scalable oplossing. Gegevens in [Blob storage](https://azure.microsoft.com/services/storage/blobs/) wordt in het algemeen beschouwd als gegevens "at rest', maar deze kan worden verwerkt als een gegevensstroom door Stream Analytics. Eén gebruikelijk voor Blob storage ingangen met Stream Analytics is logboekverwerking, waar telemetrielogboek van een systeem is geregistreerd en moet worden geparseerd en verwerkt als u wilt extraheren zinvolle gegevens.

Het is belangrijk te weten dat de standaard-tijdstempel van Blob storage gebeurtenissen in Stream Analytics de tijdstempel dat de blob het laatst is gewijzigd welke *isBlobLastModifiedUtcTime*is. Als u wilt de gegevens als een gegevensstroom met een tijdstempel in de gebeurtenis nettolading verwerken, moet het trefwoord [Tijdstempel door](https://msdn.microsoft.com/library/azure/dn834998.aspx) worden gebruikt.

Bedenk ook dat CSV indeling invoeritems een veldnamenrij en geef de velden voor de gegevensgroep **vereisen** . Verdere koptekst rijvelden moet alle **uniek zijn.**

> [AZURE.NOTE] Stream Analytics biedt geen ondersteuning voor inhoud toevoegen aan een bestaande blob. Stream Analytics wordt alleen weergeven voor een blob eenmaal en de wijzigingen die zijn uitgevoerd nadat deze lezen niet worden verwerkt. De beste manier is om alle gegevens eenmaal uploaden en eventuele aanvullende gebeurtenissen niet toevoegen aan de store blob.

De onderstaande tabel wordt elke eigenschap in het Blob storage invoer tabblad met een beschrijving beschreven:

<table>
<tbody>
<tr>
<td>NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Invoer Alias</td>
<td>Een beschrijvende naam die wordt gebruikt in de query taak om te verwijzen naar deze invoer.</td>
</tr>
<tr>
<td>Opslag-Account</td>
<td>De naam van de opslag-account waarin uw blob-bestanden opgeslagen worden.</td>
</tr>
<tr>
<td>Accountsleutel opslag</td>
<td>De geheime sleutel die is gekoppeld aan het account opslag.</td>
</tr>
<tr>
<td>Opslag Container
</td>
<td>Containers bieden een logische groepering voor BLOB's die zijn opgeslagen in de service Microsoft Azure Blob. Wanneer u een blob naar de Blob-service uploaden, moet u een container voor die blob.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [optionele]</td>
<td>Het bestandspad gebruikt om te zoeken van uw BLOB's binnen de opgegeven container.
U kunt kiezen in het pad, kunt u een of meer exemplaren van de volgende 3 variabelen opgeven:<BR>{date}, {tijd},<BR>{partition}<BR>Voorbeeld 1: cluster1/logboeken / {date} / {afstemmen} / {partitioneren}<BR>Voorbeeld 2: cluster1/logboeken / {date}<P>Houd er rekening mee dat "*" is niet een toegestane waarde voor pathprefix. Alleen geldige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">tekens Azure blob</a> zijn toegestaan.</td>
</tr>
<tr>
<td>Datumnotatie [optionele]</td>
<td>Als het token datum wordt gebruikt in het voorvoegsel pad, kunt u de datumnotatie waarin uw bestanden worden opgeslagen. Voorbeeld: Jjjj/MM-DD</td>
</tr>
<tr>
<td>Tijdnotatie [optionele]</td>
<td>Als de tijd token wordt gebruikt in het voorvoegsel pad, geeft u de tijdnotatie waarin uw bestanden worden opgeslagen. De enige ondersteunde waarde is momenteel [HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Om ervoor te zorgen dat uw query's werkt zoals verwacht, Stream Analytics moet weten welke serialisatie-indeling (JSON, CSV- of Avro) u gebruikt voor binnenkomende gegevensstromen.</td>
</tr>
<tr>
<td>Codering</td>
<td>Voor de CSV- en JSON is UTF-8 de enige ondersteunde codering indeling op dit moment.</td>
</tr>
<tr>
<td>Een scheidingsteken</td>
<td>Stream Analytics ondersteunt een aantal veelvoorkomende scheidingstekens voor serialiseren gegevens in een CSV-indeling. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk.</td>
</tr>
</tbody>
</table>

Als uw gegevens is die afkomstig zijn uit een bron van Blob storage, zijn er enkele metagegevensvelden in uw query Stream Analytics. De onderstaande tabel bevat de velden en de bijbehorende beschrijvingen.

| EIGENSCHAP | BESCHRIJVING |
|------|------|
| BlobName | De naam van de invoer blob die afkomstig zijn de gebeurtenis. |
| EventProcessedUtcTime | De datum en tijd waarop de gebeurtenis is verwerkt door Stream Analytics. |
| BlobLastModifiedUtcTime | De datum en tijd waarop de label voor het laatst is gewijzigd. |
| PartitionId | De nul partition-ID voor de invoer adapter. |

U mogelijk bijvoorbeeld een query als volgt schrijven:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
U hebt geleerd over opties voor gegevensverbinding in Azure wordt aangegeven voor uw taken Stream Analytics. Meer informatie over Stream analyses, raadpleegt u:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
