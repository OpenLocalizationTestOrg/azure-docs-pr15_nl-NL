<properties
 pageTitle="Externe controle vooraf geconfigureerde oplossing scenario | Microsoft Azure"
 description="Een beschrijving van de Azure IoT vooraf geconfigureerde oplossing externe controle en de architectuur."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Externe controle vooraf geconfigureerde oplossing Stapsgewijze instructies

## <a name="introduction"></a>Inleiding

De IoT Suite remote monitoring [vooraf geconfigureerde oplossing] [ lnk-preconfigured-solutions] is een implementatie van een end-to-end monitoring oplossing voor meerdere computers in externe locaties. De oplossing combineert belangrijke Azure services een algemene implementatie van het bedrijfsscenario en u deze kunt gebruiken als uitgangspunt voor uw eigen implementatie. U kunt [aanpassen] [ lnk-customize] de oplossing om te voldoen aan uw eigen specifieke bedrijfsbehoeften.

In dit artikel begeleidt u bij enkele van de belangrijkste elementen van de externe controle oplossing om te kunnen begrijpen hoe dit werkt. Deze kennis helpt u bij:

- Los problemen met de oplossing.
- Plan hoe u met de oplossing aan uw eigen specifieke behoeften aanpassen. 
- Ontwerp uw eigen IoT-oplossing die Azure services gebruikt.

## <a name="logical-architecture"></a>Logische architectuur

In het volgende diagram bevat een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![Logische architectuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Gesimuleerd apparaten

In de vooraf geconfigureerde oplossing staat het gesimuleerd apparaat een koelingsapparaat (zoals een building airconditioner of faciliteit air afhandeling eenheid). Wanneer u de vooraf geconfigureerde oplossing implementeert, u ook automatisch inrichten van vier gesimuleerd apparaten die worden uitgevoerd in een [Azure WebJob][lnk-webjobs]. De gesimuleerd apparaten kunnen u eenvoudig kunt verkennen het gedrag van de oplossing zonder dat u naar het implementeren van de fysieke apparaten. Als u wilt implementeren een reële fysiek apparaat, raadpleegt u de [verbinding maken met uw apparaat naar de externe monitoring vooraf geconfigureerde oplossing] [ lnk-connect-rm] zelfstudie.

Elk gesimuleerd apparaat kunt u de volgende berichttypen verzenden naar IoT Hub:

| Bericht  | Beschrijving |
|----------|-------------|
| Opstarten  | Wanneer het apparaat wordt gestart, stuurt een **apparaat-info** -bericht met informatie over zelf naar de back-end. Deze gegevens bevatten de apparaat-id, de Apparaatmetagegevens, een lijst met de opdrachten voor het apparaat ondersteunt en de huidige configuratie van het apparaat. |
| Aanwezigheid | Een apparaat verzendt periodiek een bericht **aanwezigheid** rapporteren of het apparaat kan worden gebruikt voor het instellingen van de aanwezigheid van een sensor. |
| Telemetrielogboek | Een apparaat verzendt periodiek een **telemetrielogboek** -bericht dat gesimuleerd waarden voor de temperatuur en vochtigheid die worden verzameld van gesimuleerd sensoren van het apparaat rapporten. |


De volgende apparaateigenschappen verzenden de gesimuleerd apparaten in een bericht **apparaat-info** :

| Eigenschap               |  Doel |
|------------------------|--------- |
| Apparaat-ID              | ID, waarmee wordt opgegeven of toegewezen wanneer een apparaat wordt gemaakt in de oplossing. |
| De fabrikant van de           | De fabrikant van apparaat |
| Model getal           | Modelnummer van het apparaat |
| Geeft het seriële getal          | Het seriële getal van het apparaat |
| Firmware               | Huidige versie van firmware op het apparaat |
| Platform               | Platformarchitectuur van het apparaat |
| Processor              | Het apparaat uitgevoerd processor |
| Geïnstalleerde RAM          | Hoeveelheid RAM op het apparaat is geïnstalleerd |
| Hub ingeschakelde status      | IoT Hub staat eigenschap van het apparaat |
| Aanmaaktijd           | Het apparaat dat is gemaakt in de oplossing tijd |
| Bijgewerkte tijd           | Laatst eigenschappen zijn bijgewerkt voor het apparaat |
| Breedtegraad               | Breedtegraad locatie van het apparaat |
| Lengtegegevens              | Lengtegegevens locatie van het apparaat |

De simulator seeding deze eigenschappen in gesimuleerd apparaten met voorbeeldwaarden.  Telkens wanneer die de simulator geïnitialiseerd een gesimuleerd apparaat, berichten het apparaat dat de vooraf gedefinieerde metagegevens op IoT Hub. Houd er rekening mee hoe dit overschrijft eventuele updates van metagegevens in de portal apparaat.


De volgende opdrachten verzonden vanuit het dashboard oplossing via de hub IoT kunnen worden verwerkt door de gesimuleerd apparaten:

| Opdracht                | Beschrijving                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Een _ping_ verzendt naar het apparaat controleren dat dit actief is   |
| StartTelemetry         | Hiermee start u het apparaat dat u verzendt telemetrielogboek                 |
| StopTelemetry          | Het apparaat verzenden telemetrielogboek stopt             |
| ChangeSetPointTemp     | De waarde van de set punt waaromheen de willekeurige gegevens wordt gegenereerd wijzigt |
| DiagnosticTelemetry    | Activeert de apparaatsimulator als u wilt verzenden van een extra telemetrielogboek-waarde (externalTemp) |
| ChangeDeviceState      | Een eigenschap uitgebreide status voor het apparaat wordt gewijzigd en het bericht apparaat info van het apparaat |

De bevestiging van de opdracht apparaat naar de oplossing back-end is beschikbaar via de hub IoT.

## <a name="iot-hub"></a>IoT Hub

De [IoT hub] [ lnk-iothub] ingests gegevens die zijn verzonden vanuit de apparaten in de cloud en voor de taken Azure Stream Analytics (ASA) beschikbaar stelt. IoT hub stuurt opdrachten ook naar uw apparaten namens de portal apparaat. Elke taak van de ASA stream gebruikt een aparte IoT Hub consumenten groep om te lezen van de stroom van berichten op uw apparaten.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

In de externe controle oplossing [Azure Stream Analytics] [ lnk-asa] (ASA) verzendt apparaat berichten ontvangen door de hub IoT en andere back-end-componenten voor verwerking of opslag. Verschillende ASA taken uitvoeren specifieke functies op basis van de inhoud van uw berichten.

**Taak 1: apparaat Info** filters apparaat informatieberichten van de binnenkomende bericht stream en doorgestuurd naar een gebeurtenis Hub-eindpunt. Een apparaat verzendt apparaat informatieberichten bij het opstarten en reactie op een opdracht **SendDeviceInfo** . Deze taak gebruikt de definitie van de volgende query aan te geven van **apparaat-info** berichten:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Deze taak verzendt zijn uitvoer op een gebeurtenis-Hub voor nadere verwerking.

**Taak 2: regels** binnenkomende temperatuur en vochtigheid telemetrielogboek waarden ten opzichte van per apparaat drempelwaarden evalueert. Drempelwaarden worden ingesteld in de regeleditor beschikbaar in het dashboard oplossing. Elk paar apparaat/waarde is opgeslagen door tijdstempel in een blob welke Stream Analytics als de **Verwijzingsgegevens luidt**. De taak verschilt van de waarde van een niet-lege ten opzichte van de drempel instellen voor het apparaat. Als de werkmap overschrijdt de ' >' voorwaarde, de taak Hiermee kunt u een **Waarschuwing** gebeurtenis die aangeeft dat de drempelwaarde wordt overschreden en het apparaat, waarde en tijdstempel waarden bevat. Deze taak gebruikt de definitie van de volgende query aan te geven telemetrielogboek berichten waarmee een waarschuwing moeten worden geactiveerd:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

De taak verzendt zijn uitvoer op een gebeurtenis-Hub voor nadere verwerking en details van een melding voor elke opslaat met blob storage vanaf waar het dashboard oplossing de waarschuwing informatie kunt lezen.

**Taak 3: Telemetrielogboek** werkt op de binnenkomende apparaat telemetrielogboek stream op twee manieren. De eerste verzendt alle telemetrielogboek berichten uit de apparaten met permanente blob storage langdurige opslag. De tweede average, minimum en maximum vochtigheid waarden worden berekend worden via een schuifregelaar vijf minuten-venster en deze gegevens verzendt naar blobopslag. Het dashboard oplossing leest de gegevens van het telemetrielogboek van blobopslag om te vullen de grafieken. Deze taak gebruikt de definitie van de volgende query:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Gebeurtenis Hubs

De taken van de ASA **apparaat info** en **regels** hun gegevens uitvoeren naar gebeurtenis Hubs naar betrouwbaar doorsturen naar de **Gebeurtenis Processor** in de WebJob.

## <a name="azure-storage"></a>Azure-opslag

De oplossing wordt Azure-blobopslag gebruikt om de telemetrielogboek onbewerkte en samengevatte gegevens uit de apparaten in de oplossing. Het dashboard leest de gegevens van het telemetrielogboek van blobopslag om te vullen de grafieken. Als u wilt meldingen weergeven, leest het dashboard de gegevens van blobopslag die records wanneer telemetrielogboek waarden de geconfigureerde drempelwaarden overschreden. De oplossing wordt ook gebruikt voor blobopslag opnemen van de drempelwaarden die u in het dashboard instellen.

## <a name="webjobs"></a>WebJobs

Naast het apparaat simulatoren host, hosten het WebJobs in de oplossing ook de **Gebeurtenis Processor** in een WebJob Azure die grepen apparaat informatieberichten en antwoorden van de opdracht. Wordt gebruikt:

- Berichten van apparaat informatie bij het apparaat register (opgeslagen in de database DocumentDB) met de huidige apparaat-informatie.
- Berichten van de opdracht antwoord bij het apparaat opdrachtgeschiedenis (opgeslagen in de database DocumentDB).

## <a name="documentdb"></a>DocumentDB

De oplossing gebruikt een database DocumentDB gegevens over de apparaten die zijn verbonden met de oplossing wilt opslaan. Deze informatie bevat metagegevens voor apparaten en de geschiedenis van opdrachten die zijn verzonden naar apparaten vanuit het dashboard.

## <a name="web-apps"></a>WebApps

### <a name="remote-monitoring-dashboard"></a>Externe controle dashboard
Deze pagina in de webtoepassing gebruikt PowerBI javascript-besturingselementen (Zie [PowerBI-visuele elementen cessies‑retrocessies](https://www.github.com/Microsoft/PowerBI-visuals)) om de gegevens telemetrielogboek anders uit de apparaten. De oplossing gebruikt de ASA telemetrielogboek taak om de telemetriegegevens om blob storage schrijven.


### <a name="device-administration-portal"></a>Apparaat beheer van de portal

Deze WebApp kunt u:

- Een nieuwe apparaat inrichten. Deze actie Hiermee stelt u de unieke apparaat-id en de toets verificatie gegenereerd. Deze gegevens worden geschreven informatie over het apparaat naar het register IoT Hub-identiteit en de oplossing / regiospecifieke DocumentDB-database.
- Apparaateigenschappen beheren. Deze actie bevat de bestaande eigenschappen weergeven en bijwerken met nieuwe eigenschappen.
- Opdrachten verzenden naar een apparaat.
- De opdrachtgeschiedenis voor een apparaat weergeven.
- In- en uitschakelen van apparaten.

## <a name="next-steps"></a>Volgende stappen

De volgende TechNet-blogberichten bevatten meer informatie over de externe controle vooraf geconfigureerde oplossing:

- [IoT Suite - achter de schermen - externe controle](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - externe controle - Live en gesimuleerd apparaten toevoegen](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

U kunt doorgaan met het aan de slag met IoT Suite door te lezen van de volgende artikelen:

- [Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing][lnk-connect-rm]
- [Machtigingen voor de site azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md