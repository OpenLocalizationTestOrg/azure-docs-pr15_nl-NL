<properties
 pageTitle="Ontwikkelaars begeleiden - apparaat twins begrijpen | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - gebruik apparaat twins staat en configuratie van gegevens tussen IoT Hub en uw apparaten synchroniseren"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Meer informatie over apparaat twins - voorbeeld

## <a name="overview"></a>Overzicht

*Apparaat twins* zijn JSON-documenten die apparaat staat informatie (metagegevens, configuraties en voorwaarden) opslaan. IoT Hub zich blijft voordoen een dubbele apparaat voor elk apparaat dat u verbinding met IoT Hub maakt. In dit artikel wordt beschreven:

* de structuur van het apparaat dubbele: *labels*, *gewenste* en *gerapporteerd eigenschappen*, en
* de bewerkingen die apparaat apps en achterste eindigt op apparaat twins kunnen uitvoeren.

> [AZURE.NOTE] Op dit moment kan zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

### <a name="when-to-use"></a>Wanneer gebruikt u

Apparaat twins om te gebruiken:

* Sla apparaat specifieke metagegevens in de cloud, bijvoorbeeld implementatie locatie van een Verkoopautomaat.
* Rapport huidige statusgegevens zoals beschikbare mogelijkheden en voorwaarden uit de app van uw apparaat, bijvoorbeeld een verbinding via Wi-Fi- of mobiel apparaat.
* De status van werkstromen langdurige tussen apparaat app en terug einde, zoals back-end precisie van de nieuwe firmwareversie te installeren, en het apparaat app rapportage van de verschillende fasen van het updateproces synchroniseren.
* Query uitvoeren op uw apparaat metagegevens, configuratie of staat.

[Apparaat-naar-cloud berichten] gebruiken[ lnk-d2c] voor reeks voorzien van een tijdstempel gebeurtenissen zoals tijdreeks van sensorgegevens of alarmen. [Cloud-naar-apparaat methoden] gebruiken[ lnk-methods] voor interactieve besturingselement apparaten, zoals het inschakelen van een ventilator.

## <a name="device-twins"></a>Apparaat twins

Apparaat twins apparaat-gerelateerde gegevens worden opgeslagen die:

- Uiteinden van een apparaat en terug kunnen gebruiken om te synchroniseren apparaat voorwaarden en configuratie.
- De toepassing back-end kunt gebruiken om te query en doelsites langdurige bewerkingen.

De levensduur van een dubbele apparaat is gekoppeld aan de bijbehorende [apparaat identiteit][lnk-identity]. Twins worden impliciet gemaakt en verwijderd wanneer een nieuwe apparaat identiteit wordt gemaakt of verwijderd in IoT Hub.

Een dubbele apparaat is een JSON-document dat bevat:

* **Tags**. Een document JSON gelezen en geschreven door de back-end. Labels zijn niet zichtbaar voor apparaat apps.
* **Gewenste eigenschappen**. Gebruikt in combinatie met gerapporteerde eigenschappen te synchroniseren met apparaatconfiguratie of voorwaarde. Gewenste eigenschappen kunnen alleen worden ingesteld door de toepassing weer einde en kunnen worden gelezen door de app apparaat. De app apparaat kan ook worden gewaarschuwd in realtime van wijzigingen weergeven op de gewenste eigenschappen.
* **Gemeld eigenschappen**. Gebruikt in combinatie met de gewenste eigenschappen te synchroniseren met apparaatconfiguratie of voorwaarde. Gerapporteerde eigenschappen kunnen kan alleen worden ingesteld door de app apparaat en worden gelezen en een query wordt uitgevoerd door de toepassing back-end.

Bovendien de hoofdsite van de twee apparaat bevat de alleen-lezen-eigenschappen van de corresponderende identiteit, als die in het [apparaat identiteit register][lnk-identity].

![][img-twin]

Hier volgt een voorbeeld van een apparaat dubbele JSON document:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

In de hoofdmap-object, zijn de Systeemeigenschappen en container objecten voor `tags` en beide `reported` en `desired properties`. De `properties` container bevat bepaalde elementen alleen-lezen (`$metadata`, `$etag`, en `$version`) die respectievelijk worden beschreven in de [metagegevens van de twee] [ lnk-twin-metadata] en [optimistische gelijktijdigheid] [ lnk-concurrency] secties.

### <a name="reported-property-example"></a>Voorbeeld van de gerapporteerde eigenschap

Klik in het bovenstaande voorbeeld de dubbele apparaat bevat een `batteryLevel` eigenschap aan die door de app apparaat wordt gerapporteerd. Deze eigenschap kan query en gebruiken op apparaten op basis van het laatste gerapporteerde niveau. Een ander voorbeeld moet het apparaat app rapport apparaat mogelijkheden of connectiviteitsopties.

Houd er rekening mee hoe gerapporteerde eigenschappen scenario's waarin de back-end is geïnteresseerd in de laatste bekende waarde van een eigenschap vereenvoudigen. [Apparaat-naar-cloud berichten] gebruiken[ lnk-d2c] of de back-end moet worden verwerkt van apparaat telemetrielogboek in de vorm van reeksen gebeurtenissen te voorzien van een tijdstempel, zoals tijdreeks.

### <a name="desired-configuration-example"></a>Voorbeeld van de gewenste configuratie

Klik in het bovenstaande voorbeeld de `telemetryConfig` gewenste en gerapporteerde eigenschappen worden gebruikt door de vorige einde en apparaat-app voor het synchroniseren van de configuratie telemetrielogboek voor dit apparaat. Bijvoorbeeld:

1. De app back-end Hiermee stelt u de gewenste eigenschap met de gewenste configuratie-waarde. Dit is het gedeelte van het document met de gewenste eigenschap:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. De app apparaat ontvangt een melding van de wijziging meteen als verbonden of op de eerste opnieuw verbinding te maken. De app apparaat vervolgens de bijgewerkte configuratie-rapporten (of een fout voorwaarde met de `status` eigenschap). Dit is het gedeelte van gerapporteerde eigenschappen:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. De app back-end kunt laten bijhouden van de resultaten van de configuratie-bewerking over veel apparaten, door de [query's uitvoeren] [ lnk-query] twins.

> [AZURE.NOTE] De bovenstaande fragmenten ziet u voorbeelden, geoptimaliseerd voor leesbaarheid, van een mogelijke manieren voor het coderen van de configuratie van een apparaat en de status ervan. IoT Hub bevat geen opleggen voor een bepaald schema voor de gewenste en gerapporteerde eigenschappen in het twins apparaat.

In veel gevallen twins gebruikt voor het synchroniseren van langdurige bewerkingen zoals firmware-updates. Verwijzen naar de [gewenste eigenschappen als u wilt configureren apparaten gebruiken] [ lnk-twin-properties] voor meer informatie over het gebruik van eigenschappen te synchroniseren en lang bijhouden die u bewerkingen over apparaten.

## <a name="back-end-operations"></a>Back-enddatabase bewerkingen

De back-end werkt op het gebruik van de volgende atomaire bewerkingen, die worden aangeboden via HTTP dubbele:

1. **Dubbele op id ophalen**. Deze bewerking geeft als resultaat de inhoud van de de dubbele document, wanneer u markeringen opneemt en desgewenst gerapporteerd en Systeemeigenschappen.
2. **Dubbele gedeeltelijk bijwerken**. Deze bewerking kunt de back-end gedeeltelijk bijwerken van de twee labels of gewenste eigenschappen. De gedeeltelijke update wordt uitgedrukt als een JSON-document dat wordt toegevoegd of een eigenschap genoemd bijgewerkt. Eigenschappen ingesteld op `null` worden verwijderd. Bijvoorbeeld de volgende Hiermee maakt u een nieuwe gewenste eigenschap met waarde `{"newProperty": "newValue"}`, overschrijft de huidige waarde van `existingProperty` met `"otherNewValue"`, en volledig verwijderd `otherOldProperty`. Geen wijzigingen optreden naar andere bestaande gewenste eigenschappen of codes:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Vervang de gewenste eigenschappen**. Deze bewerking kunt u de back-end naar volledig overschrijven alle bestaande gewenste eigenschappen te vervangen door een nieuw document JSON voor `properties/desired`.
4. **Tags vervangen**. Als u wilt vervangen gewenste eigenschappen, analogously kunnen deze bewerkingen de back-end naar volledig overschrijven alle bestaande tags te vervangen door een nieuw document JSON voor `tags`.

Ondersteuning voor de bovenstaande bewerkingen [optimistische gelijktijdigheid] [ lnk-concurrency] en de machtiging **ServiceConnect** vereisen, zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

Naast deze bewerkingen, de back-end de twins met een SQL-achtige [query taal]kunt query[lnk-query], en bewerkingen uitvoeren in grote sets met twins met behulp van [taken][lnk-jobs].

## <a name="device-operations"></a>Apparaatbewerkingen

De app apparaat uitgevoerd op de dubbele met de volgende atomaire bewerkingen:

1. **Dubbele ophalen**. Deze bewerking geeft als resultaat de inhoud van de de dubbele document (wanneer u markeringen opneemt en gewenst, gerapporteerd en Systeemeigenschappen) voor het momenteel verbonden apparaat.
2. **Gedeeltelijk update eigenschappen gerapporteerd**. Deze bewerking kunt de gedeeltelijke update van de gerapporteerde eigenschappen van het apparaat dat momenteel bent verbonden. Dit dezelfde JSON update opmaak gebruikt als de back-end die tegenover elkaar liggen gedeeltelijke update van de gewenste eigenschappen.
3. **Observe gewenste eigenschappen**. Het apparaat dat momenteel verbonden kunt kiezen om de hoogte gesteld van updates naar de gewenste eigenschappen zodra deze worden aangebracht. Het apparaat ontvangt hetzelfde formulier van update (gedeeltelijke of volledige vervanging) uitgevoerd door de back-end.

De bovenstaande bewerkingen toestemming **DeviceConnect** , zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

Het [apparaat van de Azure IoT SDK's] [ lnk-sdks] om eenvoudig te gebruiken van de bovenstaande bewerkingen uit veel talen en platforms. Meer informatie over de details van primitieven IoT-Hub voor de gewenste eigenschappen synchronisatie vindt u in het [apparaat opnieuw verbinden stroom][lnk-reconnection].

> [AZURE.NOTE] Apparaat twins zijn momenteel alleen toegankelijk vanaf apparaten die verbinding maken met IoT Hub met het MQTT-protocol.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over toegang tot uw hub IoT beheren.

## <a name="tags-and-properties-format"></a>Labels en eigenschappen opmaken

Labels, eigenschappen gewenste en gerapporteerde zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON objecten zijn hoofdlettergevoelig 128-teken Unicode-tekenreeksen. Toegestane tekens uitsluiten stuurcodes UNICODE (segmenten C0 en C1), en `'.'`, `' '`, en `'$'`.
* Alle waarden in de JSON-object van de volgende JSON-typen kunnen zijn: Booleaanse waarde, getal, een tekenreeks, een object. Matrices zijn niet toegestaan.

## <a name="twin-size"></a>Dubbele grootte

IoT Hub worden toegepast in een beperking 8KB grootte van de waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van elementen voor alleen-lezen.
De grootte wordt berekend door tellen alle tekens, met uitzondering van UNICODE bepalen tekens (segmenten C0 en C1) en de ruimte `' '` wanneer deze wordt weergegeven buiten een tekenreeksconstante.
IoT Hub weigert met fout alle bewerkingen die de grootte van die documenten boven de limiet wilt vergroten.

## <a name="twin-metadata"></a>Dubbele metagegevens

IoT Hub onderhoudt het tijdstempel van de laatste update voor elk object JSON in de gewenste en gerapporteerde eigenschappen. De tijdstempels in UTC en codering in de indeling [ISO8601] `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Bijvoorbeeld:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Deze informatie wordt bewaard op elk niveau (niet alleen de bladeren van de JSON-structuur) als u wilt behouden updates die objectsleutels verwijderen.

## <a name="optimistic-concurrency"></a>Optimistische gelijktijdigheid

Tags gewenste gerapporteerde eigenschappen en alle ondersteuning optimistische gelijktijdigheid.
Labels hebben een etag, aan de hand van [RFC7232], die van de tag JSON weergave aangeeft. U kunt dit gebruiken in voorwaardelijke bewerkingen uit de back-end samenhang.

Eigenschappen van het gewenste en gerapporteerde etags niet heb, maar wel een `$version` waarde die gegarandeerd oplopen. Analogously naar een etag, kan de versie worden gebruikt door de bijwerken partij zoals (een apparaat-app voor een gerapporteerde eigenschap) of de back-end voor een gewenste eigenschap om af te dwingen consistentie van updates.

Versies zijn ook handig wanneer een observing agent (zoals de apparaat-app naleving van de gewenste eigenschappen) heeft races tussen het resultaat van een bewerking ophalen en een updatebericht afstemmen. De sectie [apparaat opnieuw verbinden stroom] [ lnk-reconnection] vindt u meer informatie.

## <a name="device-reconnection-flow"></a>Stroom van apparaat opnieuw verbinden

IoT Hub behoudt niet de gewenste eigenschappen updatemeldingen voor apparaten met verbroken. Volgt dat een apparaat dat verbinding maakt het document volledige gewenste eigenschappen naast abonneren voor updatemeldingen moet ophalen. De mogelijkheid van races tussen updatemeldingen en volledige ophalen gegeven, ervoor de volgende stroom wordt gezorgd

1. Apparaat-app maakt verbinding met een hub IoT.
2. Apparaat app afsluit voor de gewenste eigenschappen updatemeldingen.
3. Apparaat app Hiermee haalt u het volledige document voor de gewenste eigenschappen.

De app apparaat kunt negeren alle berichten met `$version` kleiner dan of gelijk zijn dan de versie van de volledige opgehaalde document. Dit is mogelijk omdat IoT Hub zorgt ervoor dat versies altijd verhogen.

> [AZURE.NOTE] Deze logica wordt al geïmplementeerd in het [Azure IoT apparaat SDK's][lnk-sdks]. Deze beschrijving is nuttig alleen als de app apparaat een van de Azure IoT apparaat SDK's niet gebruiken en de interface MQTT rechtstreeks moet programma.

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd over apparaat twins, hebt u misschien interesse in de volgende onderwerpen voor de handleiding voor ontwikkelaars:

- [Directe methode op een apparaat][lnk-methods]
- [Taken plannen op meerdere apparaten][lnk-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende zelfstudies voor IoT Hub interessant kunnen zijn:

- [Het gebruik van de twee apparaat][lnk-twin-tutorial]
- [Het gebruik van dubbele eigenschappen][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png