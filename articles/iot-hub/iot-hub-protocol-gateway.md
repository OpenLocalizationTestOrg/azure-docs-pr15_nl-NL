<properties
   pageTitle="Azure IoT protocol gateway | Microsoft Azure"
   description="Wordt beschreven hoe Azure IoT protocol gateway gebruiken voor het uitbreiden van de mogelijkheden en protocolondersteuning van Azure IoT Hub."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Aanvullende protocollen ondersteunende voor IoT Hub

Azure IoT Hub ondersteunt communicatie via de MQTT, AMQP en HTTP-protocollen. In sommige gevallen, apparaten of veld gateways mogelijk niet gebruikmaken van een van deze standaard protocollen en moeten worden protocol aanpassing. In dat geval kunt u een aangepaste gateway. Een aangepaste gateway kunt protocol aanpassing voor IoT Hub eindpunten inschakelen door het verkeer naar en vanuit IoT Hub bruggen. U kunt de [Azure IoT protocol gateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) als aangepaste gateway protocol aanpassing voor IoT Hub inschakelen.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protocol gateway

De Azure IoT protocol gateway is een kader voor aanpassing van protocol dat is ontworpen voor high-schaal bidirectionele apparaat communicatie met IoT Hub. De gateway protocol is een Pass Through-onderdeel die apparaatverbindingen via een specifieke protocol accepteert. Deze verbinding van het verkeer naar IoT Hub via AMQP 1.0. De Azure IoT protocol gateway is beschikbaar als een project openen-bron software biedt flexibiliteit voor het toevoegen van ondersteuning voor een groot aantal protocollen en protocol versies.

U kunt de gateway protocol in Azure implementeren in een zeer scalable manier met behulp van Azure-Cloudservices werknemer rollen. Bovendien kan de gateway protocol worden geïmplementeerd in on-premises omgevingen, zoals veld gateways.

De gateway van Azure IoT protocol bevat een MQTT protocol adapter waarmee u kunt het aanpassen van het gedrag van de protocol MQTT indien nodig. Aangezien IoT Hub ingebouwde ondersteuning voor het MQTT v3.1.1-protocol biedt, moet u alleen overwegen de MQTT protocol adapter als er een nodig voor protocol aanpassingen of over de vereisten voor extra functionaliteit.

De MQTT-adapter wordt ook het programmeren model voor het samenstellen van protocol adapters voor andere protocollen getoond. Bovendien kunt het Azure IoT protocol gateway programming model u aangepaste onderdelen voor gespecialiseerde verwerking zoals aangepaste verificatie, bericht transformaties, compressie/decompressie of ontsleutelen van verkeer tussen de apparaten en IoT Hub invoegtoepassing.

Voor flexibiliteit, het protocol gateway en de implementatie van MQTT vindt u in een open source software-project. Hiermee kunt u de uitvoering naar wens aanpassen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Azure IoT protocol gateway en het gebruik en deze als onderdeel van uw IoT-oplossing implementeert, raadpleegt u:

* [Azure IoT protocol gateway bibliotheek op GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Handleiding voor ontwikkelaars van Azure gateway voor opdracht protocol IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Zie meer informatie over het plannen van uw IoT Hub-implementatie:

- [Vergelijken met Hubs gebeurtenis][lnk-compare]
- [Schalen, HA en DR][lnk-scaling]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
