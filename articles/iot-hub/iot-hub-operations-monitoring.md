<properties
 pageTitle="Cmdlets voor controle IoT Hub-bewerkingen"
 description="Een overzicht van Azure IoT Hub bewerkingen monitoring, zodat u kunt de status van bewerkingen op uw hub IoT in realtime controleren"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Inleiding tot bewerkingen bewaken

IoT Hub-bewerkingen voor controle kunt u de status van bewerkingen op uw hub IoT in realtime controleren. IoT Hub wordt bijgehouden gebeurtenissen in verschillende categorieën onderverdeeld bewerkingen en u kunt er ook voor kiezen bij het verzenden van gebeurtenissen van een of meer categorieën naar het eindpunt van de hub IoT voor verwerking. U kunt de gegevens op fouten controleren of complexere verwerking op basis van gegevenspatronen instellen.

IoT Hub bewaakt vijf soorten gebeurtenissen:

- Apparaat identiteit bewerkingen
- Apparaat telemetrielogboek
- Opdrachten voor cloud-naar-apparaat
- Verbindingen
- Uploaden van bestanden

## <a name="how-to-enable-operations-monitoring"></a>Het inschakelen van bewerkingen bewaken

1. Maak een hub IoT. U vindt instructies voor het maken van een hub IoT in de [Aan de slag] [ lnk-get-started] handleiding.

2. Open het blad van uw hub IoT. Klik op **bewerkingen monitoring**daar.

    ![][1]

3. Selecteer de controle categorieën die u wilt u controleren en klik vervolgens op **Opslaan**. De gebeurtenissen zijn beschikbaar voor het lezen van de gebeurtenis Hub-compatibele eindpunt vermeld in de **controle-instellingen**. Het eindpunt IoT Hub heet `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Gebeurteniscategorieën en hoe ze worden gebruikt

Elke categorie nummers monitoring bewerkingen een ander type interactie met IoT Hub en elke categorie controle heeft een schema waarmee wordt gedefinieerd hoe gebeurtenissen in die categorie zijn gestructureerd.

### <a name="device-identity-operations"></a>Apparaat identiteit bewerkingen

De categorie apparaat identiteit bewerkingen wordt bijgehouden fouten die optreden wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in van de hub van uw IoT identiteit register. Het bijhouden van deze categorie is handig voor het inrichten van scenario's.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Apparaat telemetrielogboek

De categorie van de telemetrielogboek apparaat kunt u fouten die zich in de hub IoT en betrekking hebben op de pijplijn telemetrielogboek bijhouden. Deze categorie bevat fouten die optreden bij het telemetrielogboek gebeurtenissen (zoals beperking) verzenden en ontvangen van telemetrielogboek gebeurtenissen (zoals onbevoegde reader). Houd er rekening mee dat deze categorie fouten die worden veroorzaakt door code die wordt uitgevoerd op het apparaat zelf kan niet onderschept.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Opdrachten voor cloud-naar-apparaat

De categorie cloud-to-device opdrachten kunt u fouten die zich in de hub IoT en betrekking hebben op het apparaat opdracht verkooppijplijn bijhouden. Deze categorie bevat fouten die optreden bij het verzenden van opdrachten (zoals onbevoegde afzender), opdrachten (zoals de bezorging van het aantal hops) ontvangen en de opdracht feedback ontvangen (zoals feedback verlopen). Deze categorie onderscheppen niet fouten vanaf een apparaat die een opdracht niet goed worden verwerkt als de opdracht is bezorgd.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Verbindingen

De categorie verbindingen wordt bijgehouden fouten die optreden wanneer apparaten verbinding of een hub IoT verbreken. Het bijhouden van deze categorie is handig om onbevoegde verbindingen en wanneer een verbinding verbroken voor apparaten in gebieden van slechte connectivity is wilt bijhouden.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Uploaden van bestanden

Het bestand uploaden categorie wordt bijgehouden fouten die zich in de hub IoT en betrekking hebben op bestand uploaden functionaliteit. Deze categorie bevat fouten die zich bij de SAS URI voordoen (zoals wanneer het voordat u een apparaat krijgt de hub van een voltooide upload verloopt), mislukte uploads gerapporteerd door het apparaat en wanneer u een bestand niet wordt gevonden in opslag tijdens het maken van IoT Hub melding bericht. Houd er rekening mee dat deze categorie fouten die rechtstreeks optreden terwijl het apparaat een bestand naar opslag uploaden is kan geen onderschept.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Volgende stappen

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md