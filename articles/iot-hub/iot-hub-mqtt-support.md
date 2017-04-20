<properties
 pageTitle="Ondersteuning voor IoT Hub MQTT | Microsoft Azure"
 description="Beschrijving van MQTT ondersteuning in IoT hub-niveau"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT Hub MQTT ondersteuning

IoT Hub kunt apparaten om over te communiceren met de eindpunten van IoT Hub apparaat met de [MQTT v3.1.1] [ lnk-mqtt-org] protocol op poort 8883 of MQTT v3.1.1 via WebSocket protocol op poort 443. IoT Hub alle apparaat communicatie wordt beveiligd via TLS/SSL vereist (dus IoT Hub ondersteunt geen niet-beveiligde verbindingen via poort. 1883).

## <a name="connecting-to-iot-hub"></a>Verbinding maken met IoT Hub

Een apparaat kunt met het protocol MQTT verbinding maken met een IoT hub via de bibliotheken in de [Microsoft Azure IoT SDK's] [ lnk-device-sdks] of met behulp van de MQTT protocol rechtstreeks.

## <a name="using-the-device-client-sdks"></a>De apparaat-client SDK's gebruiken

[Apparaat-client SDK's] [ lnk-device-sdks] die ondersteuning voor het protocol MQTT zijn beschikbaar voor Java, Node.js, C, C# en Python. Het apparaat-client SDK's gebruiken de standaard IoT Hub-verbindingsreeks tot stand brengen van een verbinding met een hub IoT. Als u wilt gebruiken het MQTT-protocol, moet de parameter van de client-protocol zijn ingesteld op **MQTT**. Standaard de apparaat-client SDK's verbinding maken met een IoT Hub met de **CleanSession** vlag ingesteld op **0** en **1 QoS** voor uitwisseling van berichten met de hub IoT gebruiken.

Wanneer een apparaat is aangesloten op een hub IoT, bieden de apparaat-client SDK's methoden waarmee het apparaat dat berichten om te verzenden en ontvangen van berichten van een hub IoT.

De volgende tabel bevat koppelingen naar voorbeelden van de code voor elke ondersteunde taal en Hiermee geeft u de parameter gebruiken tot stand brengen van een verbinding met IoT Hub met het MQTT-protocol.

| Taal                   | Parameter protocol        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure-iot-apparaat-mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migreren van een apparaat-app van AMQP naar MQTT
Als u het [apparaat client SDK's]gebruikt[lnk-device-sdks], is de parameter protocol in de initialisatie van de client wijzigen, zoals hierboven overstappen van het gebruik van AMQP naar MQTT vereist.

Wanneer u dit doet, zorg er Controleer de volgende items:

* AMQP retourneert fouten voor veel voorwaarden voldoen, terwijl MQTT de verbinding beëindigt. Hierdoor kan wijzigingen moeten worden de logica verwerking van uw uitzonderingen.
* MQTT biedt geen ondersteuning van de bewerkingen *negeren* bij het ontvangen [berichten C2D][lnk-messaging]. Als uw back-end ontvangt een antwoord van de app apparaat moet, kunt u overwegen [directe methoden][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Het protocol MQTT rechtstreeks gebruiken

Als u een apparaat niet de apparaat-client SDK's gebruiken, kan deze nog steeds verbinding maken met de eindpunten van de openbare apparaat met het MQTT-protocol. In het **CONNECT** -pakket moet de volgende waarden worden gebruikt door het apparaat:

- Gebruik de **deviceId**voor het veld **ClientId** . 
- Gebruik voor het veld **Username** `{iothubhostname}/{device_id}`, waarbij {iothubhostname} de volledige CName van de hub IoT.

    Bijvoorbeeld als de naam van uw hub IoT **contoso.azure-devices.net** is en de naam van uw apparaat **MyDevice01 is**, het volledige veld **Username** moet bevatten `contoso.azure-devices.net/MyDevice01`.

- Gebruik een token SA's voor het veld voor **wachtwoord** . De indeling van het token SA's is hetzelfde als de HTTP- en AMQP protocollen voor:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Zie het gedeelte van het apparaat van [beveiligingstokens IoT Hub gebruiken]voor meer informatie over het genereren van SA's tokens[lnk-sas-tokens].
    
    Wanneer u test, kunt u ook de [Apparaat Explorer] gebruiken[ lnk-device-explorer] hulpmiddel snel een SA's om token te genereren die u kunt kopiëren en plakken in uw eigen code:
    
    1. Ga naar het tabblad **rollenbeheer** in Verkenner apparaat.
    2. Klik op **Token SA's** (rechtsboven).
    3. Klik op **SASTokenForm**, selecteer uw apparaat in de **DeviceID** vervolgkeuzelijst voor aggregaatfuncties. Stel uw **TTL**.
    4. Klik op **genereren** als u wilt maken van uw token.
    
    De SA's token die wordt gegenereerd heeft deze structuur:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Het deel van deze token als het veld voor **wachtwoord** verbinding maken met MQTT gebruikt:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

Voor MQTT verbinden en pakketten verbreken, IoT Hub problemen een gebeurtenis in het kanaal **Bewerkingen bewaken** met aanvullende informatie die u voor het oplossen van problemen met de serververbinding kan helpen.

### <a name="sending-messages-to-iot-hub"></a>Berichten verzenden naar IoT Hub

Nadat u een verbinding, een apparaat kunt berichten verzenden op IoT Hub met `devices/{device_id}/messages/events/` of `devices/{device_id}/messages/events/{property_bag}` als **De naam van het onderwerp**. De `{property_bag}` element kunt u het apparaat naar het verzenden van berichten met aanvullende eigenschappen in een url-codering-indeling. Bijvoorbeeld:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Dit `{property_bag}` element gebruikmaakt van dezelfde codering voor querytekenreeksen in het HTTP-protocol.

De clienttoepassing apparaat ook de beschikking over `devices/{device_id}/messages/events/{property_bag}` als de **wordt de onderwerpnaam van het** *worden berichten* wordt doorgestuurd als een bericht telemetrielogboek definiëren.

IoT Hub biedt geen ondersteuning voor QoS-2-berichten. Als een client apparaat een bericht met **QoS 2 publiceert**, gesloten IoT Hub de netwerkverbinding.
IoT Hub blijven behouden berichten niet worden bewaard. IoT Hub wordt toegevoegd als een apparaat een bericht met de vlag **behouden** is ingesteld op 1 verzendt, de **x-opt-behouden** de eigenschap application aan het bericht. In dit geval in plaats van persistent met het maken van het bericht behouden, IoT Hub wordt doorgegeven aan de backend-toepassing.

### <a name="receiving-messages"></a>Ontvangen van berichten

Als u wilt berichten ontvangt van IoT-Hub, een apparaat moet zich hebt geabonneerd met `devices/{device_id}/messages/devicebound/#` als **Onderwerp Filter**. De met meerdere niveaus jokertekens **#** in het onderwerp Filter wordt alleen gebruikt om toestaan van het apparaat aanvullende eigenschappen ontvangen in de naam van het onderwerp. IoT Hub mag niet het gebruik van de **#** of **?** jokertekens voor het filteren van submappen onderwerpen. Aangezien IoT Hub een algemene Publisher-sub SMS makelaar is, ondersteunt dit alleen de onderwerpnamen van de beschreven en onderwerp filters.

Neem de notitie, dat het apparaat niet in aanmerking alle berichten van IoT Hub voordat deze zich heeft geabonneerd op het apparaat specifieke eindpunt, voorgesteld door het `devices/{device_id}/messages/devicebound/#` onderwerp filter. Nadat de succesvolle abonnement is gebracht, start het apparaat ontvangt alleen cloud-to-device-berichten die ernaar zijn verzonden, na het tijdstip waarop het abonnement. Wanneer het apparaat dat is verbonden met **CleanSession** -markering is ingesteld op **0**, wordt het abonnement tussen verschillende sessies worden vastgelegd. De volgende keer dat deze verbinding met **CleanSession 0** het apparaat maakt ontvangt in dit geval uitstaande berichten die zijn verzonden naar deze terwijl deze nadat de verbinding is verbroken. Als het apparaat **CleanSession** -markering is ingesteld op **1 gebruikt** , hoewel er geen alle berichten van IoT Hub ontvangen wordt totdat het voldoet aan het apparaat-eindpunt.

IoT Hub bezorgen van berichten met de **Naam van het onderwerp** `devices/{device_id}/messages/devicebound/`, of `devices/{device_id}/messages/devicebound/{property_bag}` als er een berichteigenschappen. `{property_bag}`url-codering sleutel/waardeparen van berichteigenschappen van het bevat. Alleen de toepassingseigenschappen en de gebruiker worden ingesteld Systeemeigenschappen (zoals **messageId** of **correlationId**) worden opgenomen in de eigenschap zak. Namen van de eigenschap System hebben het voorvoegsel **$**, eigenschappen van toepassingen gebruiken de naam van de oorspronkelijke eigenschap met geen voorvoegsel.

Als een apparaat-client zich naar een onderwerp met **QoS 2 abonneert**, verleent IoT Hub maximale QoS niveau 1 in het **SUBACK** -pakket. Daarna bieden IoT Hub berichten bij het apparaat met behulp van QoS-1.

### <a name="additional-considerations"></a>Aanvullende overwegingen

Als een definitief aandacht, wanneer u nodig hebt om aan te passen van het gedrag van het protocol MQTT aan de zijden van de cloud, raadpleegt u de [Azure IoT protocol gateway] [ lnk-azure-protocol-gateway] waarmee u kunt een aangepaste protocol krachtige gateway rechtstreeks aan de IoT Hub implementeren. De gateway van Azure IoT protocol kunt u het apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen aanpassen. Deze benadering is echter vereist dat u uitvoert en een gateway aangepaste protocol functioneren.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie raadpleegt u [notities op MQTT ondersteunen] [ lnk-mqtt-devguide] in de handleiding voor ontwikkelaars van Azure IoT Hub.

Meer informatie over het protocol MQTT, raadpleegt u de [documentatie van MQTT][lnk-mqtt-docs].

Zie meer informatie over het plannen van uw IoT Hub-implementatie:

- [Azure gecertificeerd voor IoT apparaat catalogus][lnk-devices]
- [Ondersteuning voor aanvullende protocollen][lnk-protocols]
- [Vergelijken met Hubs gebeurtenis][lnk-compare]
- [Schalen, HA en DR][lnk-scaling]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
