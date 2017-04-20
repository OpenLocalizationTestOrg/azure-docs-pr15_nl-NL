<properties
 pageTitle="Ontwikkelaars handleiding onderwerpen voor IoT Hub | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars waarin IoT Hub eindpunten, beveiliging, apparaat identiteit registry, Apparaatbeheer en messaging"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub-handleiding voor ontwikkelaars

Azure IoT Hub heeft een volledig beheerde service waarmee inschakelen betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en weer een toepassing beëindigen.

Azure IoT Hub beschikt u over:

* Beveiligde communicatie met behulp van de beveiligingsreferenties per apparaat en toegangsbeheer.
* Betrouwbare apparaat naar cloud en cloud-naar-apparaat hyper-schaal messaging.
* Eenvoudig apparaat connectiviteit met apparaatbibliotheken voor de populairste talen en platforms.

Deze handleiding voor ontwikkelaars van IoT Hub bevat de volgende artikelen:

- [Berichten verzenden en ontvangen met IoT Hub] [ devguide-messaging] beschrijving van de SMS-berichten functies (apparaat-naar-cloud en cloud-to-device) dat toegang biedt tot IoT Hub. Het artikel bevat ook informatie over onderwerpen zoals berichtindelingen, en de ondersteunde communicatieprotocollen en de poortnummers die ze gebruiken.
- [Upload bestanden vanaf een apparaat] [ devguide-upload] wordt beschreven hoe u bestanden vanaf een apparaat kunt uploaden. Het artikel bevat ook informatie over onderwerpen zoals de meldingen die het proces uplaod kunt verzenden.
- [Apparaat identiteiten beheren in de IoT Hub] [ devguide-identities] wordt beschreven wat informatie van de hub van elke IoT apparaat identiteit register worden opgeslagen, en hoe u kunt openen en wijzigen.
- [Toegang tot IoT Hub] [ devguide-security] het beveiligingsmodel dat wordt gebruikt voor toegang tot IoT Hub functionaliteit voor beide apparaten en cloud onderdelen wordt beschreven. Het artikel bevat informatie over het gebruik van tokens en x.509-certificaten en details van de machtigingen die u kunt verlenen.
- [Apparaat twins gebruiken voor het synchroniseren van staat en configuraties] [ devguide-device-twins] beschrijving van het *apparaat dubbele* concept en de functionaliteit voor zoals het synchroniseren van een apparaat met de dubbele worden getoond. Het artikel bevat informatie over de gegevens die zijn opgeslagen in een dubbele apparaat.
- [Directe methode op een apparaat] [ devguide-directmethods] beschrijving van de levensduur van een directe methode, informatie over het methoden op een apparaat uit uw back-enddatabase-app en afhandelen van de directe methode op uw apparaat.
- [Taken op meerdere apparaten plannen] [ devguide-jobs] wordt beschreven hoe u kunt taken op meerdere apparaten plannen. Het artikel wordt beschreven hoe taken die taken uitvoeren als een rechtstreekse methode, het bijwerken van een devcie met een dubbele devcie uitvoeren. Hierin worden ook hoe u de status van een taak opvragen.
- [Referentie - IoT Hub eindpunten] [ devguide-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft. Tevens wordt beschreven hoe u een veld gateway kunt gebruiken om in te schakelen sommige apparaten verbinding maken met uw IoT Hub-eindpunten.
- [Referentie - querytaal voor twins, methoden en taken] [ devguide-query] beschreven die querytaal waarmee gegevens ophalen uit uw hub over uw apparaat twins en taken.
- [Referentie - quota en beperken] [ devguide-quotas] bevat een overzicht van de quota instellen in de IoT Hub-service en het bandbreedteregeling gedrag dat u verwachten kunt om te zien wanneer u een limiet overschrijdt.
- [Referentie - apparaat en service SDK's] [ devguide-sdks] lijsten de SDK's kunt u zowel de apparaat en de service-toepassingen die met uw hub IoT samenwerken ontwikkelen. Het artikel bevat koppelingen naar API onlinedocumentatie.
- [Referentie - IoT Hub MQTT ondersteuning] [ devguide-mqtt] vindt u gedetailleerde informatie over hoe IoT Hub het protocol MQTT ondersteunt. Het artikel beschrijving van de ondersteuning voor het protocol MQTT ingebouwde naar de SDK's en informatie over het gebruik van het protocol MQTT rechtstreeks.
- [Woordenlijst] [ devguide-glossary] een lijst met algemene IoT Hub-termen.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

