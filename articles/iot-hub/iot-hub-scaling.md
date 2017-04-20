<properties
 pageTitle="Azure IoT Hub schaalbaarheid | Microsoft Azure"
 description="Wordt beschreven hoe de schaal van Azure IoT Hub aanpassen."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Schaalbaarheid van IoT Hub

Azure IoT Hub kan maximaal een miljoen tegelijk verbonden apparaten ondersteunen. Zie voor meer informatie [IoT Hub prijzen][lnk-pricing]. Elke eenheid IoT Hub kunt een bepaald aantal dagelijkse berichten.

Als uw oplossing correct wilt verkleinen, kunt u uw bepaalde benut IoT Hub. Houd rekening met de vereiste piek doorvoer voor de volgende categorieën van bewerkingen met name:

* Apparaat-naar-cloud-berichten
* Berichten van de cloud-naar-apparaat
* Identiteit registerbewerkingen

Naast deze informatie doorvoer Zie [IoT Hub quota en throttles][] en ontwerpen van uw oplossing dienovereenkomstig gewijzigd.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Apparaat-naar-cloud en cloud-to-device bericht doorvoer

De beste manier om de grootte van een IoT Hub-oplossing is het verkeer op basis van de per eenheid wordt berekend.

Apparaat-naar-cloud berichten Volg deze richtlijnen continue doorvoer.

| Laag | Continue doorvoer | Tarief van continue verzenden |
| ---- | -------------------- | ------------------- |
| S1 | Snel aan de 1111 KB/minuten per eenheid<br/>(1,5 GB/dag/eenheid) | Het gemiddelde van 278 berichten/minuten per eenheid<br/>(400.000 berichten/dag per eenheid) |
| S2 | Snel aan de 16 MB/minuten per eenheid<br/>(22.8 GB/dag/eenheid) | Het gemiddelde van 4167 berichten/minuten per eenheid<br/>(6 miljoen berichten/dag per eenheid) |
| S3 | Snel aan de 814 MB/minuten per eenheid<br/>(1144.4 GB/dag/eenheid) | Het gemiddelde van 208,333 berichten/minuten per eenheid<br/>(300 miljoen berichten/dag per eenheid) |

## <a name="identity-registry-operation-throughput"></a>Identiteit register bewerking doorvoer

IoT Hub identiteit registerbewerkingen zijn niet moet runtime bewerkingen uitvoeren, zoals ze voornamelijk betrekking op mobiele apparaten inrichten hebben.

Zie voor specifieke burst prestaties nummers, [IoT Hub quota en throttles][].

## <a name="sharding"></a>Sharding

Terwijl een één IoT hub kan worden aangepast aan miljoenen apparaten, soms uw oplossing specifieke prestatie-eigenschappen die een één IoT hub kan niet garanderen is vereist. In dat geval wordt het aanbevolen dat u uw apparaten in meerdere IoT hubs verdelen. Meerdere IoT hubs verkeer bursts vloeiend en de vereiste doorvoer of de bewerking tarieven die vereist zijn.

## <a name="next-steps"></a>Volgende stappen

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quota en throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
