<properties
 pageTitle="Handleiding voor ontwikkelaars - IoT Hub SDK's | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - informatie over en koppelingen naar de verschillende Azure IoT Hub apparaat en service SDK's."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>IoT Hub SDK 's

## <a name="iot-hub-device-sdks"></a>IoT Hub apparaat SDK 's

Het apparaat dat Microsoft Azure IoT SDK's bevatten code die vergemakkelijkt building apparaten en toepassingen die verbinding maken met en worden beheerd door Azure IoT Hub-services.

Het volgende IoT apparaat SDK's zijn beschikbaar om het downloaden van GitHub:

- [Azure IoT apparaat SDK voor C] [ lnk-c-device-sdk] geschreven in ANSI-C (C99) voor overdraagbaarheid en globaal platformcompatibiliteit.
- [Azure IoT apparaat SDK voor .NET][lnk-dotnet-device-sdk]
- [Azure IoT apparaat SDK voor Java][lnk-java-device-sdk]
- [Azure IoT apparaat SDK voor Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT apparaat SDK voor Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Zie het Leesmij-bestanden in de opslagplaatsen GitHub voor informatie over het gebruik van taal en platform / regiospecifieke pakket managers binaire bestanden en afhankelijkheden wilt installeren op uw computer ontwikkeling.

## <a name="os-platforms-and-hardware-compatibility"></a>OS Platforms en hardware compatibiliteit

Zie voor meer informatie over SDK compatibiliteit met bepaalde apparaten, het [Azure gecertificeerd voor IoT apparaat catalogus][lnk-certified].

## <a name="iot-hub-service-sdks"></a>SDK's IoT Hub-service

De Microsoft Azure IoT service SDK's bevatten de code die building toepassingen die rechtstreeks met IoT Hub vergemakkelijkt samenwerken voor het beheren van apparaten en beveiliging.

De volgende IoT service SDK's zijn beschikbaar om het downloaden van GitHub:

- [Azure IoT service SDK voor .NET][lnk-dotnet-service-sdk]
- [Azure IoT service SDK voor Node.js][lnk-node-service-sdk]
- [Azure IoT service SDK voor Java][lnk-java-service-sdk]

> [AZURE.NOTE] Zie het Leesmij-bestanden in de opslagplaatsen GitHub voor informatie over het gebruik van taal en platform / regiospecifieke pakket managers binaire bestanden en afhankelijkheden wilt installeren op uw computer ontwikkeling.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT Gateway SDK

Deze Azure IoT Gateway SDK bevat de infrastructuur en modules om IoT gateway oplossingen te maken. U kunt de SDK als u wilt maken die op een end-to-end-scenario is afgestemd gateways uitbreiden.

U kunt de [Azure IoT Gateway SDK] downloaden[ lnk-gateway-sdk] uit GitHub.

## <a name="online-api-reference-documentation"></a>Online API-documentatie

Hier volgt een lijst met koppelingen naar online API-documentatie bij het apparaat, de service en de gateway-bibliotheken Azure IoT:

- [Internet van dingen (IoT) .NET][lnk-dotnet-ref]
- [IoT Hub REST][lnk-rest-ref]
- [Azure IoT apparaat SDK voor C][lnk-c-ref]
- [Azure IoT apparaat SDK voor Java][lnk-java-ref]
- [Azure IoT service SDK voor Java][lnk-java-service-ref]
- [Azure IoT apparaat SDK voor Node.js][lnk-node-ref]
- [Azure IoT service SDK voor Node.js][lnk-node-service-ref]
- [Azure IoT gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Volgende stappen

Andere onderwerpen verwijzing in deze handleiding IoT Hub-ontwikkelaars omvatten:

- [IoT Hub eindpunten][lnk-devguide-endpoints]
- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [Quota en beperken][lnk-devguide-quotas]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md