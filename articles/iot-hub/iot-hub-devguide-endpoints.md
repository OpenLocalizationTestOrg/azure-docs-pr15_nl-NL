<properties
 pageTitle="Handleiding voor ontwikkelaars - IoT Hub eindpunten | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - naslaginformatie over IoT Hub eindpunten"
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

# <a name="reference---iot-hub-endpoints"></a>Referentie - IoT Hub eindpunten

## <a name="list-of-iot-hub-endpoints"></a>Lijst met IoT Hub eindpunten

Azure IoT Hub is een meerdere tenant-service dat toegang biedt tot de functionaliteit aan verschillende betrokkenen. In het volgende diagram ziet u de verschillende eindpunten dat IoT Hub beschrijft.

![IoT Hub eindpunten][img-endpoints]

Hier volgt een beschrijving van de eindpunten:

* **Resource-provider**. De provider van de resource IoT Hub beschrijft een [Azure resourcemanager] [ lnk-arm] interface waarmee Azure abonnement eigenaars die u wilt maken en verwijderen van IoT hubs en IoT hub eigenschappen bijwerken. Eigenschappen van IoT Hub [beveiliging op gebruikersniveau hub beleid]sturen[lnk-accesscontrol], in tegenstelling tot apparaatniveau toegangsbeheer en functionele opties voor messaging cloud-naar-apparaat en apparaat-naar-cloud. De provider van de resource IoT Hub kunt u ook als u wilt [exporteren van apparaat identiteiten][lnk-importexport].
* **Apparaatbeheer identiteit**. Elke hub IoT beschikt over een set van HTTP REST eindpunten naar het apparaat nu identiteiten beheer (maken, ophalen, bijwerken en verwijderen). [Apparaat identiteiten] [ lnk-device-identities] worden gebruikt voor de verificatie en toegangsbeheer apparaat.
* **Dubbele Apparaatbeheer**. Elke hub IoT beschikt over een set van service-omlaag HTTP REST eindpunt kunnen opvragen en bijwerken van [apparaat twins] [ lnk-twins] (voor het bijwerken van labels en eigenschappen).
* **Beheer van taken**. Elke hub IoT beschikt over een set van service-omlaag HTTP REST eindpunt opvragen en [taken]beheren[lnk-jobs].
* **Apparaat eindpunten**. Voor elk apparaat deze is ingericht in het register van de identiteit apparaat beschikt IoT Hub over een set eindpunten die een apparaat gebruiken kunt om te verzenden en ontvangen van berichten:
    - *Apparaat-naar-cloud-berichten verzenden*. Gebruik dit eindpunt apparaat naar cloud berichten te [verzenden][lnk-d2c].
    - *Berichten van ontvangen cloud-naar-apparaat*. Een apparaat gebruikt dit eindpunt gerichte [cloud-naar-apparaat berichten]ontvangen[lnk-c2d].
    - *Bestandsuploads initiëren*. Een apparaat dit eindpunt gebruikt voor het ontvangen van een Azure opslag SA's URI van IoT Hub voor het [uploaden van een bestand][lnk-upload].
    - *Ophalen en de update dubbele eigenschappen*. Een apparaat gebruikt deze eindpunten voor toegang tot het [apparaat dubbele][lnk-twins]van eigenschappen.
    - *Ontvangen rechtstreekse methoden aanvragen*. Een apparaat gebruikt deze eindpunten moeten beluisteren [directe methoden][lnk-methods]van aanvragen.

    Deze eindpunten worden weergegeven met [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 en [AMQP 1.0] [ lnk-amqp] protocollen. Houd er rekening mee dat AMQP ook beschikbaar via [WebSockets is] [ lnk-websockets] op poort 443.
    
    De twins en methoden endpoins zijn alleen beschikbaar is met [MQTT v3.1.1][lnk-mqtt].

* **Service-eindpunten**. Elke hub IoT beschrijft een reeks eindpunten die uw toepassing back-end gebruiken kunt om te communiceren met uw apparaten. Deze eindpunten zijn momenteel alleen weergegeven gebruikt de [AMQP] [ lnk-amqp] protocol, behalve voor de methode aanroep-eindpunt dat wordt weergegeven via HTTP 1.1.
    - *Apparaat-naar-cloud-berichten ontvangen*. Dit eindpunt is compatibel met [Azure gebeurtenis Hubs][lnk-event-hubs]. Een back-enddatabase-service deze kunt gebruiken om te lezen van alle [berichten van apparaat-naar-cloud] [ lnk-d2c] verzonden door uw apparaten.
    - *Cloud-naar-apparaat berichten verzenden en ontvangen van bezorging bevestigingen*. Deze eindpunten inschakelen uw toepassing back-end naar betrouwbaar [cloud-naar-apparaat berichten]verzenden[lnk-c2d], en de bijbehorende levering of verlooptijd bevestigingen ontvangen.
    - *Ontvangen bestandsmeldingen*. Dit SMS eindpunt kunt u meldingen van ontvangen wanneer uw apparaten kunt een bestand uploaden. 
    - *Oproepen direct methode*. Dit eindpunt kan een back-enddatabase-service een [directe methode] aan te roepen[ lnk-methods] op een apparaat.

De [IoT Hub-API's en SDK's] [ lnk-sdks] artikel beschrijft de verschillende manieren voor toegang tot deze eindpunten.

Ten slotte, is het belangrijk om alle IoT Hub eindpunten gebruiken de [TLS] [ lnk-tls] protocol en geen eindpunt ooit op niet-versleuteld/onbeveiligde kanalen worden weergegeven.

## <a name="field-gateways"></a>Veld gateways

Een *veld gateway* bevindt in een oplossing IoT zich tussen uw apparaten en uw IoT Hub-eindpunten. Deze bevindt zich doorgaans dicht bij uw apparaten. Uw apparaten communiceren rechtstreeks met de gateway veld met behulp van een protocol wordt ondersteund door de apparaten. De gateway veld verbindt met een via een protocol dat wordt ondersteund door IoT Hub IoT Hub-eindpunt. Een gateway veld zijn zeer gespecialiseerde hardware of een lage power-software waarmee de werkstroom het end-to-end-scenario waarvoor de gateway is bedoeld voor computers.

U kunt de [Azure IoT Gateway SDK] [ lnk-gateway-sdk] willen implementeren van een gateway veld. Deze SDK biedt specifieke functionaliteit zoals de mogelijkheid om te multiplexen communicatie op meerdere apparaten naar dezelfde verbinding op IoT Hub.

## <a name="next-steps"></a>Volgende stappen

Andere onderwerpen verwijzing in deze handleiding IoT Hub-ontwikkelaars omvatten:

- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [Quota en beperken][lnk-devguide-quotas]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md