<properties
 pageTitle="Apparaat informatie metagegevens in de externe controle oplossing | Microsoft Azure"
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
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Apparaat informatie metagegevens in de externe controle vooraf geconfigureerde oplossing

De externe controle vooraf geconfigureerde oplossing Azure IoT Suite ziet u een methode voor het beheren van metagegevens voor apparaten. In dit artikel bevat een overzicht van de methode die deze oplossing gaat om te kunnen begrijpen:

- Welke Apparaatmetagegevens de oplossing worden opgeslagen.
- Hoe beheert de oplossing de Apparaatmetagegevens.

## <a name="context"></a>Context

De externe controle vooraf geconfigureerde oplossing maakt gebruik van [Azure IoT Hub] [ lnk-iot-hub] waarmee uw apparaten gegevens te verzenden naar de cloud. IoT Hub bevat een [apparaat identiteit register] [ lnk-identity-registry] om toegang tot IoT Hub beheren. Het register IoT Hub apparaat identiteit staat los van de externe oplossing / regiospecifieke *apparaat register* die worden opgeslagen apparaat informatie metagegevens voor controle. De externe controle oplossing maakt gebruik van een [DocumentDB] [ lnk-docdb] database voor het implementeren van de Register apparaat voor metagegevens van apparaat informatie op te slaan. De [Architectuur van Microsoft Azure IoT verwijzing] [ lnk-ref-arch] beschrijving van de functie van het register apparaat in een normale IoT-oplossing.

> [AZURE.NOTE] De externe controle vooraf geconfigureerde oplossing blijft het apparaat identiteit register synchroon met het apparaat-register. Beide dezelfde apparaat-id gebruiken om elk apparaat is aangesloten op uw hub IoT uniek te identificeren.

De [IoT Hub apparaat management preview] [ lnk-dm-preview] functies toevoegt aan IoT Hub die vergelijkbaar zijn met beheerfuncties van het apparaat informatie in dit artikel beschreven. De externe controle oplossing gebruikt algemeen beschikbaar (GA)-functies op dit moment alleen in IoT Hub.

## <a name="device-information-metadata"></a>Apparaat informatie metagegevens

Een apparaat informatie metagegevens JSON document dat is opgeslagen in de database apparaten register DocumentDB heeft de volgende structuur:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: het apparaat zelf schrijft deze eigenschappen en het apparaat is de instantie voor deze gegevens. Andere apparaateigenschappen voorbeeld zijn fabrikant, model getal en geeft het seriële getal. 
- **DeviceID**: de unieke apparaat-id. Deze waarde is in het register IoT Hub apparaat identiteit hetzelfde.
- **HubEnabledState**: de status van het apparaat in de IoT Hub. Deze waarde is ingesteld op **null** totdat het apparaat voor het eerst verbinding maakt. Klik in de portal oplossing wordt een **null** -waarde weergegeven als het apparaat wordt "geregistreerde, maar niet aanwezig."
- **CreatedTime**: het tijdstip waarop het apparaat dat is gemaakt.
- **DeviceState**: de stand gerapporteerd door het apparaat.
- **UpdatedTime**: de tijd die het apparaat dat het laatst werd bijgewerkt via de portal van de oplossing.
- **SystemProperties**: de oplossing-portal schrijft de Systeemeigenschappen en het apparaat heeft geen kennis van deze eigenschappen. De eigenschap van een voorbeeld-systeem is de **ICCID** als de oplossing wordt geautoriseerd met en verbonden met een service SIM-compatibele apparaten beheren.
- **Opdrachten**: een lijst met de opdrachten voor het apparaat wordt ondersteund. Het apparaat, levert deze informatie aan de oplossing.
- **CommandHistory**: een lijst met de opdrachten die is verzonden door de externe controle oplossing naar het apparaat en de status van deze opdrachten.
- **IsSimulatedDevice**: een vlag waarmee een apparaat als een gesimuleerd apparaat.
- **id**: de unieke id van de DocumentDB voor dit apparaat-document.

> [AZURE.NOTE] Gegevens van een apparaat kan ook metagegevens om te beschrijven van het telemetrielogboek dat het apparaat wordt verzonden naar IoT Hub bevatten. De externe controle oplossing gebruikt deze metagegevens telemetrielogboek om de weergave van het dashboard op [dynamische telemetrielogboek][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Levenscyclus

Wanneer u eerst een apparaat in de portal oplossing maakt, wordt een vermelding in de oplossing gemaakt in het register apparaat zoals eerder weergegeven. Veel van de informatie is in eerste instantie stubbed out en de **HubEnabledState** is ingesteld op **null**. De oplossing maakt een item voor het apparaat nu ook in het register apparaat identiteit, waardoor de toetsen die het apparaat wordt gebruikt om te verifiëren met IoT Hub gegenereerd.

Wanneer een apparaat is eerst verbinding met de oplossing maakt, stuurt een informatiebericht apparaat. Dit apparaat informatiebericht bevat apparaateigenschappen zoals de fabrikant, model getal en geeft het seriële getal. Een informatiebericht apparaat bevat ook een lijst met de opdrachten voor die het apparaat met inbegrip van informatie over de opdrachtparameters van een ondersteunt. Wanneer de oplossing dit bericht ontvangt, wordt de metagegevens van de informatie apparaat in het register apparaat bijgewerkt.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Apparaatinformatie in de portal oplossing weergeven en bewerken

De lijst met apparaten in de portal oplossing toont de volgende apparaateigenschappen van het als kolommen: **Status**, **DeviceId**, **fabrikant**, **Model getal**, **geeft het seriële getal**, **Firmware**, **Platform**, **Processor**en **RAM is geïnstalleerd**. De apparaateigenschappen **breedte** - en **lengtegegevens** station de locatie in de Bing-kaart op het dashboard. 

![Lijst met apparaten][img-device-list]

Als u op **bewerken** in het detailvenster van **Apparaat** in de oplossing-portal, kunt u alle deze eigenschappen bewerken. De record voor het apparaat in de database DocumentDB deze eigenschappen bewerken worden bijgewerkt. Als een apparaat een bijgewerkte apparaat info-mailbericht verzendt, overschrijft deze wijzigingen in de portal oplossing. U kunt de **DeviceId**, **Hostname** **HubEnabledState**, **CreatedTime**, **DeviceState**en **UpdatedTime** eigenschappen in de portal oplossing niet bewerken, omdat alleen het apparaat gemachtigd via deze eigenschappen is.

![Apparaat bewerken][img-device-edit]

U kunt de oplossing-portal gebruiken als u een apparaat verwijderen uit uw oplossing. Wanneer u een apparaat verwijdert, de oplossing verwijdert de metagegevens van apparaat informatie uit de oplossing apparaat register en het apparaat-fragment in het register IoT Hub apparaat identiteit. Voordat u een apparaat verwijderen kunt, moet u deze uitschakelen.

![Apparaat verwijderen][img-device-remove]

## <a name="device-information-message-processing"></a>Verwerking van apparaat gegevens bericht

Apparaat informatieberichten van een apparaat zijn telemetrielogboek berichten. Apparaat informatieberichten bevatten gegevens zoals apparaateigenschappen en de opdrachten die een apparaat kan reageren op een opdrachtgeschiedenis. IoT Hub zelf heeft geen kennis van de metagegevens in een informatiebericht apparaat en het bericht op dezelfde manier elk apparaat in cloud-bericht verwerkt verwerkt. In de externe controle oplossing, een [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) taak leest u de berichten van IoT Hub. De analyse van de stream **DeviceInfo** vervullen van filters voor berichten met **"ObjectType": "DeviceInfo"** en doorsturen naar het **EventProcessorHost** host exemplaar die wordt uitgevoerd in een project web. Logica in het exemplaar **EventProcessorHost** gebruikt de apparaat-id de DocumentDB record zoeken voor het gewenste apparaat en bijwerken van de record. De record van de Register apparaat bevat nu gegevens, zoals apparaateigenschappen, opdrachten en de opdrachtgeschiedenis.

> [AZURE.NOTE] Een informatiebericht apparaat is een standaard-apparaat in cloud-bericht. De oplossing onderscheid gemaakt tussen apparaat informatieberichten en telemetrielogboek berichten met behulp van ASA query's.

## <a name="example-device-information-records"></a>Voorbeeld apparaat informatie records

De externe controle vooraf geconfigureerde oplossing gebruikt twee typen apparaat informatie records: records voor de gesimuleerd apparaten geïmplementeerd met de oplossing en records voor de aangepaste apparaten u verbinding met de oplossing maakt.

### <a name="simulated-device"></a>Gesimuleerd apparaat

Het volgende voorbeeld ziet u de record JSON apparaat informatie voor een gesimuleerd apparaat. Deze record heeft een waarde die is ingesteld voor **UpdatedTime**, waarmee wordt aangegeven dat het apparaat heeft een **DeviceInfo** -bericht heeft gestuurd naar IoT Hub. De record bevat enkele veelvoorkomende apparaateigenschappen, definieert de zes opdrachten de gesimuleerd apparaten ondersteunen heeft de vlag **IsSimulatedDevice** is ingesteld op **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Aangepaste apparaat

Het volgende voorbeeld ziet u de JSON apparaat informatie-record voor een aangepast apparaat en de **IsSimulatedDevice** vlag is ingesteld op **0**heeft. U kunt zien dat deze aangepaste twee opdrachten ondersteunt en dat de oplossing-portal een opdracht **SetTemperature** heeft verstuurd naar het apparaat:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Hierna ziet u het bericht JSON **DeviceInfo** het apparaat dat is verzonden naar het bijwerken van de metagegevens van de informatie apparaat:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Nu u klaar bent met het leren hoe u de vooraf geconfigureerde oplossingen kunt aanpassen, vindt u enkele andere functies en mogelijkheden van de IoT Suite vooraf geconfigureerde oplossingen:

- [Overzicht van de blog onderhoud vooraf geconfigureerde-oplossingen][lnk-predictive-overview]
- [Veelgestelde vragen over IoT Suite][lnk-faq]
- [IoT beveiliging helemaal omhoog][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
