<properties
 pageTitle="Handleiding voor ontwikkelaars - quota en beperken | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - beschrijving van de quota's die van toepassing op IoT Hub en bandbreedteregeling normaal"
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

# <a name="reference---quotas-and-throttling"></a>Referentie - quota en beperken

## <a name="quotas-and-throttling"></a>Quota en beperken

Elk Azure abonnement kan maximaal 10 IoT hubs hebben.

Elke hub IoT is voorzien van een bepaald aantal eenheden in een specifieke SKU (Zie [Azure IoT Hub prijzen]voor meer informatie[lnk-pricing]). De SKU en het aantal eenheden Hiermee bepaalt u het quotum voor maximale dagelijkse van berichten die u kunt verzenden.

De SKU bepaalt ook de bandbreedteregeling limieten die IoT Hub worden toegepast op alle bewerkingen.

## <a name="operation-throttles"></a>Bewerking throttles

Bewerking throttles zijn rente-beperkingen die zijn toegepast in de minuut bereiken en zijn bedoeld om te voorkomen abuse. IoT Hub probeert te voorkomen dat er fouten zo veel mogelijk geretourneerd, maar deze begint uitzonderingen retourneren als de beperking is geschonden te lang.

Ga als volgt de lijst met afgedwongen throttles. Waarden verwijzen naar een afzonderlijke hub.

| Beperken | Per hub waarde |
| -------- | ------------- |
| Identiteit registerbewerkingen (maken, ophalen, lijst, bijwerken en verwijderen) | 5000/min/eenheid (voor S3) <br/> 100/min/eenheid (voor S1 en S2). |
| Apparaatverbindingen | 6000/sec/eenheid (voor S3), 120/sec/eenheid (voor S2), 12/sec/eenheid (voor S1). <br/>Minimum van 100/sec. <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100/sec over uw eenheden. Met negen S1 eenheden, hebt u 108/sec (9\*12) over uw eenheden. |
| Apparaat-naar-cloud wordt verzonden | 6000/sec/eenheid (voor S3), 120/sec/eenheid (voor S2), 12/sec/eenheid (voor S1). <br/>Minimum van 100/sec. <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100/sec over uw eenheden. Met negen S1 eenheden, hebt u 108/sec (9\*12) over uw eenheden. |
| Cloud-to-device verzendt | 5000/min/eenheid (voor S3), 100/min/eenheid (voor S1 en S2). |
| Cloud-to-device ontvangt | 50000/min/eenheid (voor S3), 1000/min/eenheid (voor S1 en S2). |
| Bestand uploadbewerkingen | 5000 bestand uploaden meldingen/min/eenheid (voor S3), 100 bestand uploaden meldingen/min/eenheid (voor S1 en S2). <br/> 10000 SA's-URI's kan zijn uit voor een account Azure-opslag in één keer.<br/> 10 SA's URI's / apparaat kan zijn af in één keer. | 

Het is belangrijk om aan te geven dat de beperking *apparaatverbindingen* het tarief weer waarop de nieuwe apparaatverbindingen met een hub IoT en niet het maximum aantal tegelijk verbonden apparaten kunnen worden vastgesteld bepaalt. De beperking is afhankelijk van het aantal eenheden dat voor de hub is ingericht.

Als u een eenheid S1 koopt, krijgt u bijvoorbeeld een vertraging van 100 verbindingen per seconde. Dit betekent dat als verbinden met 100.000 apparaten, duurt het ten minste 1000 seconden (ongeveer 16 minuten). U kunt echter zoveel tegelijk verbonden apparaten zoals u geregistreerd in uw apparaat identiteit register apparaten hebt hebben.

Voor een gedetailleerde beschrijving van IoT Hub beperken gedrag, Zie het blogbericht [IoT Hub beperken en u][lnk-throttle-blog].

>[AZURE.NOTE] Op elk gewenst is het mogelijk om uit te breiden quota of vertraging limieten door het aantal ingerichte eenheden in een hub IoT te verhogen.

>[AZURE.IMPORTANT] Identiteit registerbewerkingen zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en inrichten van scenario's. Lezen of bijwerken van een groot aantal apparaat identities wordt ondersteund door [importeren en exporteren van taken][lnk-importexport].

## <a name="next-steps"></a>Volgende stappen

Andere onderwerpen verwijzing in deze handleiding IoT Hub-ontwikkelaars omvatten:

- [IoT Hub eindpunten][lnk-devguide-endpoints]
- [Querytaal voor twins, methoden en taken][lnk-devguide-query]
- [IoT Hub MQTT ondersteuning][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md