<properties
 pageTitle="Handleiding voor ontwikkelaars - directe methoden | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - directe methoden gebruiken om aan te roepen code op uw apparaten"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Directe methode op een apparaat (preview)

## <a name="overview"></a>Overzicht

IoT Hub kunt u methoden op apparaten vanuit de cloud. Methoden vertegenwoordigen een interactie aanvraag beantwoorden met een apparaat overeen met een HTTP-oproep in dat ze slagen of meteen (na een door de gebruiker opgegeven time-out) mislukken kan de gebruiker de status van het gesprek weten. Dit is handig voor scenario's waarin de loop van onmiddellijke actie verschilt afhankelijk van of het apparaat kunnen reageren is, zoals een wekken SMS sturen naar een telefoon als een apparaat is offline (SMS wordt meer duur dan een methode-oproep).

U kunt een methode zien als een remote procedure call rechtstreeks aan op het apparaat. Alleen methoden die zijn geïmplementeerd op een apparaat mag worden aangeroepen vanuit de cloud. Als de cloud probeert een methode op een apparaat die geen die methode gedefinieerd aan te roepen, mislukt de methode-oproep.

Elke methode apparaat is bedoeld voor één apparaat. [Taken] [ lnk-devguide-jobs] zelf een formule methoden op meerdere apparaten en oproepen van de methode voor verbroken apparaten in de wachtrij.

Iedereen met machtigingen op IoT Hub **service verbinding maken met** een methode op een apparaat kan worden gestart.

### <a name="when-to-use"></a>Wanneer gebruikt u

Apparaat methoden zijn vergelijkbaar met de [cloud-naar-apparaat berichten] [ lnk-devguide-messages] in dat beide de cloud-back-enddatabase naar informatie doorgeven aan een apparaat inschakelen, maar ze in de fundamentele manier verschillen. In dat geval zijn synchroon en niet duurzame, terwijl cloud-to-device berichten asynchroon met maximaal 48 uur van levensduur zijn.

Methoden een aanvraag en respons patroon volgen en niet duurzaam zijn. Het gebrek aan levensduur heeft twee onmiddellijke voordelen wanneer u de apparaten zijn bestuurt:

- **Onmiddellijke feedback over de uitvoering van methode** betekent dat er geen nodig is voor het beheer van de correlatie tussen aanvraag en beantwoorden.
- **Sneller worden verwerkt** betekent dat de bewerkingen sneller kunnen worden uitgevoerd omdat IoT Hub geen eventuele levensduur verleent. Hub voor IoT beschikken meer methode oproepen per eenheid dan cloud-to-device berichten.

Cloud-to-device berichten zijn niet per se-opdrachten bij het apparaat, maar liever vertegenwoordigen een cloudservice doorgeven van een deel van de informatie bij het apparaat voor deze volgt te werk om op het gemak en die het apparaat al dan niet reageert mogelijk. Cloud-to-device berichten hebben een time-out langer (maximaal 48 uur) terwijl methoden veel sneller verloopt.

Apparaat methoden voor onmiddellijke opdrachtaanroep op een apparaat en taken voor geplande aanroep van een opdracht op een apparaat gebruiken.

## <a name="method-lifecycle"></a>Levenscyclus van de methode

Methoden worden geïmplementeerd op het apparaat en mogelijk invoeritems van nul of meer in de methode nettolading juist wordt gestart. Aanroepen van een directe methode via een URI service-omlaag (`{iot hub}/twins/{device id}/methods/`). Een apparaat ontvangt directe methoden door een apparaat-specifieke MQTT onderwerp (`$iothub/methods/POST/{method name}/`). We ondersteunen mogelijk methoden op Extra apparaat aan de clientzijde netwerkprotocollen in de toekomst.

> [AZURE.NOTE] Als u een directe methode op een apparaat aanroept, namen en waarden mogen alleen US-ASCII-afdrukbaar alfanumerieke, behalve een in het volgende instellen: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Methoden zijn synchroon en hetzij slagen of mislukken achter de punt time-out (standaard: 30 seconden, worden ingesteld omhoog op 3600 seconden). Methoden zijn handig in interactieve scenario's waar u een apparaat fungeert als het apparaat online en ontvangen opdrachten is, zoals het inschakelen van een light vanaf een telefoon. In deze scenario's die u wilt zien van een onmiddellijke slagen of mislukken zodat de cloudservice op het resultaat zo snel mogelijk handelen kan. Het apparaat kan sommige hoofdtekst van het bericht als gevolg van de methode retourneren, maar deze niet vereist voor de methode kunt doen. Er is geen garanties op rangschikking of een semantiek gelijktijdigheid methode oproepen.

Apparaat methode oproepen zijn HTTP alleen-lezen van de cloud-kant en MQTT alleen-lezen van de kant van het apparaat.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over het gebruik van directe methoden.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Directe methode uit een back-enddatabase-app

### <a name="method-invocation"></a>Oproepen methode

Directe methode aanroepen op een apparaat zijn HTTP-oproepen die bestaat uit:

- De specifieke bij het apparaat *URI* (`{iot hub}/twins/{device id}/methods/`)
- De POST- *methode*
- *Kopteksten* waarin de vergunning aanvragen-ID, type inhoud en inhoud codering
- Een transparante JSON *hoofdtekst* in de volgende indeling:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Er is een time-out in seconden. Als time-out niet is ingesteld, wordt standaard 30 seconden.
  
### <a name="response"></a>Antwoord

De back-enddatabase ontvangt een antwoord die bestaat uit:

- *HTTP-statuscode*, die wordt gebruikt voor fouten die afkomstig zijn uit de IoT-Hub, inclusief een 404-fout voor apparaten die momenteel niet verbonden
- *Kopteksten* waarin de etag, aanvragen-ID, type inhoud en inhoud codering
- Een JSON *hoofdtekst* in de volgende indeling:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Beide `status` en `body` worden geleverd door het apparaat en gebruikt om te reageren met de statuscode van het apparaat en/of de beschrijving.

## <a name="handle-a-direct-method-on-a-devcie"></a>Greep van een directe methode op een devcie

### <a name="method-invocation"></a>Oproepen methode

Apparaten ontvangen directe methodeaanvragen op het onderwerp MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

De hoofdtekst die ontvangt van het apparaat heeft de volgende indeling:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodeaanvragen worden QoS 0.

### <a name="response"></a>Antwoord

Het apparaat antwoorden stuurt `$iothub/methods/res/{status}/?$rid={request id}`, waarbij:

 - De `status` eigenschap is de status apparaat geleverde van methode worden uitgevoerd.
 - De `$rid` eigenschap is de aanvraag-ID van de oproepen van de methode hebt ontvangen van IoT Hub.

De hoofdtekst kan is ingesteld door het apparaat en elke status.

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe om directe methoden te gebruiken, kunt u in het volgende onderwerp in de handleiding voor ontwikkelaars interessant kunnen zijn:

- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende IoT Hub zelfstudie interessant kunnen zijn:

- [Cloud-to-device methoden gebruiken][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
