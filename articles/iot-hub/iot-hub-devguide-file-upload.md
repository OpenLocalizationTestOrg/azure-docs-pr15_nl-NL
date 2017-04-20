<properties
 pageTitle="Handleiding voor ontwikkelaars - bestand uploaden | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - uploaden van bestanden vanaf een apparaat op IoT Hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Upload bestanden vanaf een apparaat

## <a name="overview"></a>Overzicht

Zoals vastgelegd in de [IoT Hub eindpunten] [ lnk-endpoints] -artikel apparaten kunnen bestandsuploads starten door te sturen van een melding via een eindpunt apparaat-omlaag (**/devices/ {deviceId} / bestanden**).  Als een apparaat melding IoT Hub van een voltooide upload weergeeft, genereert IoT Hub meldingen over uploaden bestand dat u via een eindpunt service-omlaag (**/messages/servicebound/filenotifications**) als berichten kunt ontvangen.

In plaats van berichten bemiddelen via IoT Hub zelf fungeert IoT Hub in plaats daarvan als een verzender op een gekoppelde opslag van Azure-account. Een apparaat vraagt een token opslag van IoT Hub die specifiek is voor het bestand dat het apparaat wil uploaden. Het apparaat gebruikt de URI SAS naar het bestand uploaden naar de opslag en wanneer de upload voltooid is verzendt het apparaat dat een melding van deelname naar IoT Hub. IoT Hub wordt gecontroleerd of het bestand is geüpload en vervolgens de melding van een bestand uploaden naar de nieuwe service-omlaag bestand melding SMS eindpunt telt.

Voordat u een bestand naar IoT Hub vanaf een apparaat uploaden, moet u uw hub configureren door te [koppelen van een Azure-opslag] [ lnk-associate-storage] account toe.

Uw apparaat kunt vervolgens [een upload geïnitialiseerd] [ lnk-initialize] en een [hoogte van IoT hub] [ lnk-notify] wanneer de upload is voltooid. (Optioneel) als een apparaat melding IoT Hub weergeeft dat de upload voltooid is, de service kunt genereren een [Meldingsbericht][lnk-service-notification].

### <a name="when-to-use"></a>Wanneer gebruikt u

Deze functie IoT Hub gebruiken wanneer u nodig hebt om een bestand vanaf een apparaat met uw back-enddatabase-service in plaats van een bericht via IoT Hub verzenden te uploaden.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Een opslag van Azure-account koppelen aan IoT Hub

Als u wilt de functionaliteit voor het uploaden van bestanden gebruiken, moet u eerst een Azure Storage-account koppelen aan de IoT Hub. U kunt dit doen via de [portal van Azure][lnk-management-portal], of via een programma via de [provider van de resource IoT Hub REST API's][lnk-resource-provider-apis]. Nadat u een account Azure opslagruimte aan uw IoT Hub hebt gekoppeld, retourneert de service een URI SAS naar een apparaat wanneer het apparaat geeft aan een bestand uploaden.

> [AZURE.NOTE] De [Azure IoT Hub SDK's] [ lnk-sdks] automatisch verwerken ophalen van de URI SAS, het bestand uploaden en IoT Hub van een voltooide upload de hoogte te stellen.

## <a name="initialize-a-file-upload"></a>Een bestand is geüpload geïnitialiseerd

IoT Hub heeft een eindpunt specifiek voor apparaten aanvragen van een SAS URI voor opslag om een bestand te uploaden. Het apparaat, start het bestand uploadproces door te sturen van een bericht naar de hub IoT bij `{iot hub}.azure-devices.net/devices/{deviceId}/files` met de volgende JSON hoofdtekst:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub retourneert het volgende voorbeeld, waarin het apparaat wordt gebruikt om het bestand te uploaden:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Afgeschaft: geïnitialiseerd een bestandsupload met een ophalen

> [AZURE.NOTE] Dit onderwerp vindt verminderde functionaliteit voor het ontvangen van een URI SAS van IoT Hub. Gebruik de methode POST hierboven is beschreven.

IoT Hub heeft twee REST eindpunten ter ondersteuning van bestand uploaden, een om de URI SAS voor opslagruimte en de andere op de hoogte stellen van de hub IoT van een voltooide uploaden. Het apparaat start het bestand uploadproces door te een GET verzenden naar de hub IoT bij `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. De hub retourneert een URI SAS specifiek naar het bestand te uploaden en een correlatie-ID die moet worden gebruikt wanneer de upload is voltooid.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Hoogte IoT Hub van een voltooide bestandsupload

Het apparaat is verantwoordelijk voor het bestand uploaden naar opslag met gebruik van de Azure opslag SDK's. Wanneer de upload is voltooid, wordt een bericht met het apparaat verzonden naar de hub IoT bij `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` met de volgende JSON hoofdtekst:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

De waarde van `isSuccess` is een Booleaanse die al dan niet het bestand is geüpload. De statuscode voor `statusCode` is de status voor het uploaden van het bestand voor de opslag, en de `statusDescription` overeenkomt met de `statusCode`.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over het uploaden van bestanden vanaf een apparaat.

## <a name="file-upload-notifications"></a>Meldingen over het uploaden van bestand

Wanneer een apparaat uploadt van een bestand en IoT Hub upload voltooiingspercentage krijgt, genereert de service (optioneel) een waarschuwingsbericht met de locatie van de naam en opslag van het bestand.

Zoals wordt uitgelegd in [eindpunten][lnk-endpoints], IoT Hub biedt bestand uploaden meldingen via een eindpunt service-omlaag (**/messages/servicebound/fileuploadnotifications**) als berichten. De semantiek ontvangen voor meldingen over het uploaden van bestand zijn hetzelfde als voor de cloud-to-device berichten en de hetzelfde [bericht levenscyclus]hebben[lnk-lifecycle]. Elk bericht dat wordt opgehaald uit het bestand uploaden melding eindpunt is een JSON-record met de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTimeUtc | Tijdstempel dat aangeeft waarop de melding is gemaakt. |
| DeviceId | **DeviceId** van het apparaat waarop het bestand hebt geüpload. |
| BlobUri | URI van het geüploade bestand. |
| BlobName | De naam van het geüploade bestand. |
| LastUpdatedTime | Tijdstempel dat aangeeft wanneer het bestand voor het laatst werd bijgewerkt. |
| BlobSizeInBytes | De grootte van het geüploade bestand. |

**Voorbeeld**. Dit is een voorbeeld van de hoofdtekst van het meldingsbericht voor een bestand uploaden.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Bestand uploaden meldingsopties configuratie

Elke hub IoT bevat de volgende configuratieopties voor meldingen over het uploaden van bestand:

| Eigenschap | Beschrijving | Bereik en standaard |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Hiermee bepaalt u of meldingen over het bestand uploaden naar het eindpunt van de meldingen bestand zijn geschreven. | BOOL. Standaard: waar. |
| **fileNotifications.ttlAsIso8601** | Standaard-TTL voor meldingen over het uploaden van bestand. | ISO_8601 interval maximaal 48 H (minimale 1 minuut). Standaard: 1 uur. |
| **fileNotifications.lockDuration** | Duur van de wachtrij bestand uploaden meldingen vergrendelen. | 5 tot en met 300 seconden (minimale 5 seconden). Standaard: 60 seconden. |
| **fileNotifications.maxDeliveryCount** | Maximale bezorging tellen voor het bestand uploaden melding wachtrij. | 1 tot en met 100. Standaard: 100. |

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe voor het uploaden van bestanden vanaf apparaten IoT Hub gebruikt, kunt u in de volgende onderwerpen voor de handleiding voor ontwikkelaars interessant kunnen zijn:

- [Apparaat identiteiten in IoT Hub beheren][lnk-devguide-identities]
- [Toegang tot IoT Hub beheren][lnk-devguide-security]
- [Apparaat twins gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Directe methode op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende IoT Hub zelfstudie interessant kunnen zijn:

- [Het uploaden van bestanden van apparaten in de cloud met IoT Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
