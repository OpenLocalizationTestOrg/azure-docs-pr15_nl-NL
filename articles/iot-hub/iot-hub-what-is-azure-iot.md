<properties
 pageTitle="Azure oplossingen voor Internet van mogelijke | Microsoft Azure"
 description="Een overzicht van IoT op Azure, met inbegrip van een steekproef oplossingsarchitectuur en hoe deze zich verhoudt tot Azure IoT Hub apparaat SDK's en vooraf geconfigureerde oplossingen"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Volgende stappen

Azure IoT Hub is een Azure-service secure waarmee en betrouwbaar bidirectionele communicatie tussen uw toepassing een back-end en miljoenen apparaten. Dit kunt de toepassing back-end naar:

- Ontvangen telemetrielogboek bij het op schaal van uw apparaten.
- Gegevens uit uw apparaten een stream gebeurtenis processor routeert.
- Ontvangen bestandsuploads van apparaten.
- Cloud-to-device opdrachten sturen naar bepaalde apparaten.

IoT Hub kunt u uw eigen oplossing back-end implementeren. Daarnaast bevat IoT Hub een apparaat identiteit register gebruikt voor het inrichten van apparaten, hun beveiligingsreferenties en hun rechten verbinding maken met de hub. Zie voor meer informatie over IoT Hub [Wat is IoT Hub?] [lnk-iot-hub].

Meer informatie over hoe Azure IoT Hub gestandaardiseerde IoT device management voor u extern beheren, configureren en bijwerken van uw apparaten kunt, raadpleegt u [overzicht van Azure IoT Hub Apparaatbeheer][lnk-device-management].

Als u wilt implementeren clienttoepassingen op tal van apparaat hardwareplatforms en verschillende besturingssystemen, kunt u het apparaat IoT SDK's. Het apparaat IoT SDK's bevatten bibliotheken die verzendende telemetrielogboek in een hub IoT en ontvangen van cloud-to-device opdrachten te vergemakkelijken. Wanneer u de SDK's gebruikt, kunt u kiezen uit verschillende netwerkprotocollen om te communiceren met IoT Hub. Meer informatie raadpleegt u de [informatie over apparaat SDK's][lnk-device-sdks].

Als u wilt beginnen sommige code schrijft en enkele voorbeelden uitvoeren, raadpleegt u de [aan de slag met IoT Hub] [ lnk-getstarted] zelfstudie.

U mogelijk ook geïnteresseerd in [Azure IoT Suite][lnk-iot-suite], namelijk een verzameling vooraf geconfigureerde oplossingen. IoT Suite kunt u snel aan de slag en schaal IoT projecten om algemene IoT-scenario's--zoals externe controle, activabeheer en bekijk onderhoud op te lossen.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md