<properties
 pageTitle="Azure IoT Hub met Azure gebeurtenis Hubs vergelijken | Microsoft Azure"
 description="Een vergelijking van de Azure IoT Hub en Azure gebeurtenis Hubs services functionele verschillen en gebruik dozen markeren."
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
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergelijking van Azure IoT Hub en Azure gebeurtenis Hubs

Een van de belangrijkste gebruik dozen voor IoT Hub is telemetrielogboek Verzamel van apparaten. Daarom IoT Hub vaak wordt vergeleken met [Azure gebeurtenis Hubs][]. Zoals IoT Hub is gebeurtenis Hubs de verwerking van service waarmee de gebeurtenis en telemetrielogboek ingress in de cloud op grote schaal met lage latentie en hoge betrouwbaarheid van een gebeurtenis.

De services hebben echter veel verschillen, die worden beschreven in de volgende tabel.

| Gebied | IoT Hub | Gebeurtenis Hubs |
| ---- | ------- | ---------- |
| Communicatiepatronen | Apparaat-naar-cloud en cloud-to-device messaging inschakelen | U kunt alleen gebeurtenis ingress (meestal beschouwd voor apparaat naar cloud-scenario's). |
| Ondersteuning voor apparaten protocol | Ondersteunt MQTT, AMQP, AMQP via WebSockets en HTTP. Daarnaast kunnen IoT Hub werkt met de [Azure IoT protocol gateway][lnk-azure-protocol-gateway], een gateway-implementatie aanpasbare protocol ter ondersteuning van aangepaste protocollen. | Ondersteunt AMQP, AMQP via WebSockets en HTTP. |
| Beveiliging | Biedt per apparaat-identiteit en terughalen toegangsbeheer. Zie de [sectie beveiliging van de handleiding voor ontwikkelaars van IoT Hub]. | Bevat gebeurtenis Hubs hele [gedeelde clienttoegangsbeleid][Event Hubs - security], met beperkte intrekkingsreferenties ondersteuning via het [beleid van publisher][Event Hubs publisher policies]. IoT oplossingen moeten zijn vaak implementeren van een aangepaste oplossing ter ondersteuning van per apparaat referenties en maateenheden anti-mailspoofing. |
| Bewerkingen bewaken | Kunnen IoT oplossingen abonneren op een uitgebreide set apparaat identiteit management en connectiviteit gebeurtenissen zoals afzonderlijke apparaat verificatiefouten, beperken en ongeldige indeling uitzonderingen. Deze gebeurtenissen kunnen u snel identificeren verbindingsproblemen op afzonderlijke apparaatniveau. | Beschrijft alleen de statistische doelstellingen. |
| Schaal | Is geoptimaliseerd naar ondersteuning miljoenen van tegelijk aangesloten apparaten. | Een meer beperkt aantal gelijktijdige verbindingen--maximaal 5.000 AMQP verbindingen, aan de hand van [Azure Service Bus quota][]ondersteunt. Aan de andere kant kunt gebeurtenis Hubs u opgeven van de partition voor elk bericht verzonden. |
| Apparaat SDK 's | Biedt [apparaat SDK's] [ Azure IoT Hub SDKs] voor een grote verscheidenheid aan platforms en verschillende talen. | Wordt ondersteund op .NET en C. Bovendien AMQP en HTTP-interfaces verzenden. |
| Bestand uploaden | Kunt IoT oplossingen voor het uploaden van bestanden van apparaten in de cloud. Een bestand melding eindpunt voor Werkstroomintegratie en een categorie voor ondersteuning voor foutopsporing monitoring bewerkingen bevat. | Hiermee gebruikt een patroon claimen om handmatig bestanden aanvraagt bij apparaten en geef de apparaten met een opslag-toets voor de transactie. |

In het overzicht, zelfs als de enige use-case-apparaat in cloud telemetrielogboek ingress, biedt IoT Hub een service die is speciaal ontworpen voor IoT apparaat connectivity. Blijft om uit te vouwen van de toegevoegde waarden voor deze scenario's met IoT taalspecifieke functies. Gebeurtenis Hubs is bedoeld voor de gebeurtenis ingress bij een enorme schaal, zowel in de context van inter-datacenter en binnen-datacenter-scenario's.

Het is niet vaak gebruikt zowel IoT Hub gebeurtenis Hubs in dezelfde oplossing--IoT Hub omgaat met het apparaat naar cloud-communicatie waarbij gebeurtenis Hubs later stadium gebeurtenis ingress in realtime verwerking engines afgehandeld.

## <a name="next-steps"></a>Volgende stappen

Zie meer informatie over het plannen van uw implementatie IoT Hub [schaal, HA en DR][lnk-scaling].

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[Azure gebeurtenis Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Beveiligingssectie van de handleiding voor ontwikkelaars van IoT Hub]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure-Service Bus quota]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
