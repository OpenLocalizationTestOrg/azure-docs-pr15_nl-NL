<properties
 pageTitle="Handleiding voor ontwikkelaars - taken | Microsoft Azure"
 description="Handleiding voor ontwikkelaars van Azure IoT Hub - taken uitvoeren op meerdere apparaten plant verbonden met uw hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Taken plannen op meerdere apparaten (preview)

## <a name="overview"></a>Overzicht

Volgens de beschrijving namelijk vorige artikelen, Azure IoT Hub kunt een aantal bouwstenen ([dubbele apparaateigenschappen en tags] [ lnk-twin-devguide] en [cloud-naar-apparaat methoden][lnk-dev-methods]).  Meestal inschakelen IoT back-end-toepassingen beheerders van het apparaat en operatoren voor bijwerken en interactie met IoT apparaten bulksgewijs en geplande tegelijkertijd.  Taken onderbrengen de uitvoering van apparaat dubbele updates en C2D methoden ten opzichte van een reeks apparaten per keer planning.  Een operator zou bijvoorbeeld een back-end-toepassing die wilt starten en bijhouden van een taak om een reeks apparaten in het samenstellen van 43 en floor 3 op een moment dat niet zou storend voor de bewerkingen van het gebouw opnieuw te gebruiken.

### <a name="when-to-use"></a>Wanneer gebruikt u

Houd rekening met behulp van taken wanneer: de achtergrond van een oplossing beëindigen nodig heeft om te plannen en de voortgang bijhouden een van de volgende activiteiten op een reeks apparaat:

- Dubbele gewenst apparaateigenschappen bijwerken
- Apparaat dubbele labels bijwerken
- Roepen C2D methoden

## <a name="job-lifecycle"></a>Levenscyclus van de taak

Taken worden gestart door de oplossing back-end en beheerd door IoT Hub.  U kunt een taak tot en met een service-omlaag URI starten (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) en de query voor de voortgang van een taak uitvoeren via een URI service-omlaag (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Als een taak wordt gestart, worden query's uitvoeren voor taken ingeschakeld in de back-end-toepassing te vernieuwen van de status van het uitvoeren van taken.

> [AZURE.NOTE] Wanneer u een taak, namen en waarden mogen alleen US-ASCII-afdrukbaar alfanumerieke, behalve een in het volgende instellen: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over het gebruik van taken.

## <a name="jobs-to-execute-direct-methods"></a>Taken rechtstreekse methoden uit te voeren

Hieronder ziet u de details van de aanvraag HTTP 1.1 voor het uitvoeren van een [directe methode] [ lnk-dev-methods] op een set met behulp van een taak apparaten:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Taken dubbele apparaateigenschappen bijwerken

Hieronder ziet u de details van de aanvraag HTTP 1.1 voor het bijwerken van dubbele apparaateigenschappen behulp van een taak.

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Query's uitvoeren voor de voortgang van taken

Hieronder ziet u de details van de aanvraag HTTP 1.1 voor [query's uitvoeren voor taken][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
De continuationToken wordt geleverd door het antwoord.  

## <a name="jobs-properties"></a>Eigenschappen van taken

Hier volgt een lijst met eigenschappen en de bijbehorende beschrijvingen, die kunnen worden gebruikt voor het uitvoeren van query's voor taken of de resultaten van de taak.

| Eigenschap | Beschrijving |
| -------------- | -----------------|
| **taak-id** | Toepassing opgegeven ID voor de taak. |
| **Starttijd** | Toepassing aangeboden begintijd (ISO-8601) voor de taak. |
| **Eindtijd** | IoT Hub opgegeven datum (ISO-8601) voor de voltooiing van de taak. Geldige alleen nadat de taak de 'voltooide' status bereikt. | 
| **type** | Typen taken: |
| | **scheduledUpdateTwin**: een taak wordt gebruikt bij een reeks dubbele gewenste eigenschappen of labels. |
| | **scheduledDeviceMethod**: een taak wordt gebruikt om een apparaat methode op een reeks dubbele te starten. |
| **status** | Huidige status van de taak. Mogelijke waarden voor status: |
| | **in behandeling** : gepland en die nog moet worden opgenomen door de service taak. |
| | **geplande** : gepland voor een tijd in de toekomst. |
| | **actief** : actieve taak. |
| | **geannuleerd** : taak is geannuleerd. |
| | **is mislukt** : taak is mislukt. |
| | **voltooid** : taak is voltooid. |
| **deviceJobStatistics** | Statistische gegevens over de uitvoering van de taak. |

In de voorbeeldweergave is het object deviceJobStatistics alleen beschikbaar nadat de taak is voltooid.

| Eigenschap | Beschrijving |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Het aantal apparaten in de taak. |
| **deviceJobStatistics.failedCount** | Het aantal apparaten waarop de taak is mislukt. |
| **deviceJobStatistics.succeededCount** | Het aantal apparaten waarop de taak is voltooid. |
| **deviceJobStatistics.runningCount** | Het aantal apparaten die worden momenteel uitgevoerd voor de taak. |
| **deviceJobStatistics.pendingCount** | Het aantal apparaten die in behandeling de taak uit te voeren. |


### <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub-apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende IoT Hub zelfstudie interessant kunnen zijn:

- [Planning en uitzending taken][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
