<properties
 pageTitle="Handleiding voor ontwikkelaars - woordenlijst | Microsoft Azure"
 description="Een lijst van algemene termen met betrekking tot IoT Hub"
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

# <a name="glossary-of-iot-hub-terms"></a>Woordenlijst IoT Hub

In dit artikel worden enkele van de algemene voorwaarden die zijn gekoppeld aan IoT Hub.

## <a name="advanced-message-queueing-protocol-amqp"></a>Geavanceerde bericht wachtrij Protocol (AMQP)

[AMQP](https://www.amqp.org/) is een van de messaging-protocollen die ondersteuning biedt voor IoT Hub voor communicatie met apparaten. Zie voor meer informatie over de SMS-berichten protocollen die ondersteuning biedt voor IoT Hub [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Cloud-naar-apparaat (C2D)

Meestal gebruikt om te verwijzen naar berichten van IoT Hub naar een verbonden apparaat verzonden. Deze berichten zijn vaak opdrachten die Geef de instructie om het apparaat dat sommige actie te ondernemen. Zie [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md)voor meer informatie.

## <a name="condition"></a>Voorwaarde

Verwijst naar de gegevens van het apparaat staat, zoals de methode connectiviteit die momenteel in gebruik, volgens een apparaat-app. Apparaten kunnen ook de mogelijkheden melden. U kunt zoeken voorwaarde en de mogelijkheid met de dubbele apparaat.

## <a name="data-point-message"></a>Gegevenspunt bericht

Een gegevenspunt bericht is een cloud-to-device-bericht met telemetriegegevens zoals windsnelheid of temperatuur.

## <a name="desired-properties"></a>Gewenste eigenschappen

Gewenste eigenschappen worden in de context van apparaat twins gebruikt in combinatie met *Eigenschappen gerapporteerd* te synchroniseren met apparaatconfiguratie of voorwaarde. Gewenste eigenschappen kunnen alleen worden ingesteld door de toepassing weer einde en zijn waargenomen door het apparaat-app. 

## <a name="device-to-cloud-d2c"></a>Apparaat-naar-cloud (D2C)

Meestal gebruikt om te verwijzen naar verzonden berichten van een verbonden apparaat op IoT Hub. Deze berichten zijn mogelijk [gegevenspunt](#data-point-message) of [interactieve](#interactive-message) berichten. Zie [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md)voor meer informatie.

## <a name="device-identity-registry"></a>Apparaat identiteit register

Het [apparaat identiteit register](iot-hub-devguide-identity-registry.md) is de ingebouwde onderdeel van een hub IoT waarin informatie over de afzonderlijke apparaten mogen verbinding maken met een hub opgeslagen.

## <a name="device"></a>Apparaat

Een apparaat is in de context van IoT, meestal kleine, zelfstandige andere apparaten die mogelijk gegevens verzamelen of besturingselement andere apparaten. Bijvoorbeeld een apparaat mogelijk, een milieu controle-apparaat, of een controller voor de water en ventilatiesystemen systemen in een vergunning.

## <a name="device-twin"></a>Apparaat dubbele

Een [apparaat dubbele](iot-hub-devguide-device-twins.md) is een kopie in de IoT Hub van de voorwaarde en configuratie-instellingen van een fysiek apparaat. U kunt een dubbele apparaat gebruiken voor het beheren van de configuratie van het apparaat.

## <a name="direct-method"></a>Directe methode

Een [directe methode](iot-hub-devguide-direct-methods.md) is een manier om te activeren van een methode uit te voeren op een apparaat door aan te roepen een API op uw hub IoT.

## <a name="event-hub-compatible-endpoint"></a>Gebeurtenis-Hub-compatibele eindpunt

Als u wilt lezen apparaat naar cloud-berichten die zijn verzonden naar uw IoT-hub, kunt u verbinding maken met een eindpunt op uw hub en gebruiken van een gebeurtenis Hub-compatibele methode selecteert om te lezen die berichten. Gebeurtenis Hub-compatibele methoden opnemen met de gebeurtenis Hubs SDK's en Azure Stream Analytics.

## <a name="field-gateway"></a>Veld gateway

Een gateway veld maakt connectiviteit voor apparaten die rechtstreeks naar IoT Hub geen verbinding kunt maken en meestal wordt geïmplementeerd lokaal met uw apparaten. Zie voor meer informatie [Wat Azure IoT Hub is?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Taak

Uw oplossing back-end kunt taken voor het plannen en bijhouden van activiteiten op een reeks apparaten die zijn geregistreerd bij uw hub IoT gebruiken. Activiteiten omvatten dubbele gewenst apparaateigenschappen bijwerken en aanroepen van directe methoden apparaat dubbele labels bijwerken.

## <a name="protocol-gateway"></a>Protocol gateway

Een gateway protocol meestal wordt geïmplementeerd in de cloud en biedt protocol vertaalservices voor apparaten verbinding maakt met IoT Hub. Zie voor meer informatie [Wat Azure IoT Hub is?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interactieve bericht

Een interactieve bericht is een cloud-to-device-bericht dat een onmiddellijke actie in de toepassing back-end activeert. Een apparaat kan bijvoorbeeld een waarschuwing over een fout die automatisch moet worden vastgelegd in een systeem CRM verzenden.

## <a name="iot-hub"></a>IoT Hub

IoT Hub heeft een volledig beheerde Azure-service waarmee betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en oplossing back-enddatabase. Zie voor meer informatie [Wat Azure IoT Hub is?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure IoT Suite verpakt samen meerdere Azure services met vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen kunnen u snel aan de slag met end-to-end-implementaties van veelvoorkomende scenario's voor IoT. Zie voor meer informatie [Wat Azure IoT Suite is?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Taak

Een [taak](iot-hub-devguide-jobs.md) in de IoT Hub kunt u bewerkingen uitvoeren zoals een firmware uitvoeren op meerdere apparaten zijn verbonden met uw hub de upgrade.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) is een van de messaging-protocollen die ondersteuning biedt voor IoT Hub voor communicatie met apparaten. Zie voor meer informatie over de SMS-berichten protocollen die ondersteuning biedt voor IoT Hub [verzenden en ontvangen van berichten met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Gerapporteerde eigenschappen

Gerapporteerde eigenschappen worden in de context van apparaat twins gebruikt in combinatie met de *gewenste eigenschappen* te synchroniseren met apparaatconfiguratie of voorwaarde. Gerapporteerde eigenschappen kunnen kan alleen worden ingesteld door de app apparaat en worden gelezen en een query wordt uitgevoerd door de toepassing back-end.

## <a name="tags"></a>Labels

In de context van devcie twins zijn labels apparaat metagegevens opgeslagen en opgehaald door de toepassing back-end in de vorm van een document JSON. Labels zijn niet zichtbaar voor apps op een apparaat.

## <a name="system-properties"></a>Systeemeigenschappen

In de context van apparaat twins, Systeemeigenschappen zijn alleen-lezen en opnemen van informatie over het gebruik van het apparaat zoals laatste activiteit tijd en verbinding staat.