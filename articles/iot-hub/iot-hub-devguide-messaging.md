<properties
 pageTitle="Handleiding voor ontwikkelaars - messaging | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - apparaat in cloud en cloud-to-device messaging"
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

# <a name="send-and-receive-messages-with-iot-hub"></a>Berichten verzenden en ontvangen met IoT Hub

## <a name="overview"></a>Overzicht

IoT Hub biedt de volgende SMS primitieven om te communiceren met een apparaat:

- [Apparaat-naar-cloud] [ lnk-d2c] vanaf een apparaat aan een toepassing een back-end.
- [Cloud-to-device] [ lnk-c2d] van een toepassing een back-end (*service* of *cloud*).

Core-eigenschappen van IoT Hub SMS-functionaliteit zijn de betrouwbaarheid en de levensduur van berichten. Deze eigenschappen inschakelen flexibiliteit naar niet altijd verbinding aan het apparaat en pieken in aan de kant cloud verwerking van gebeurtenis laden. IoT Hub implementeert *ten minste eenmaal* bezorging garanties voor zowel apparaat naar cloud en cloud-to-device messaging.

IoT Hub ondersteunt meerdere [apparaat-omlaag protocollen] [ lnk-protocols] (zoals MQTT, AMQP en HTTP). Om te ondersteunen naadloze interoperabiliteit via protocollen, IoT Hub definieert een [algemene berichtindeling] [ lnk-message-format] die ondersteuning bieden voor alle apparaat-omlaag protocollen.

IoT Hub beschrijft een [gebeurtenis Hub-compatibele eindpunt] [ lnk-compatible-endpoint] om in te schakelen van de back-end-toepassingen om het apparaat naar cloud-berichten ontvangen door de hub te lezen.

### <a name="when-to-use"></a>Wanneer gebruikt u

Messaging is een core videomogelijkheden van IoT Hub. Gebruik dit wanneer u wilt verzenden van berichten van het apparaat naar uw back-end of berichten verzenden vanuit uw back-end naar een apparaat.

Zie [vergelijking van IoT Hub en Hubs gebeurtenis]voor een vergelijking van de services IoT Hub en gebeurtenis Hubs,[lnk-compare].

## <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten

U apparaat-naar-cloud berichten verzenden via een eindpunt apparaat-omlaag (**/devices/ {deviceId} / berichten/gebeurtenissen**). Uw service back-enddatabase ontvangt apparaat naar cloud berichten via een eindpunt service-omlaag (**/messages/events**) die compatibel is met [Gebeurtenis Hubs][lnk-event-hubs]. Daarom kunt u standaard [gebeurtenis Hubs integratie en SDK's] [ lnk-compatible-endpoint] voor het ontvangen van berichten van apparaat-naar-cloud.

IoT Hub implementeert apparaat naar cloud messaging op een manier die vergelijkbaar is met [Gebeurtenis Hubs][lnk-event-hubs]. IoT-Hub apparaat naar cloud berichten lijken meer op gebeurtenis Hubs *gebeurtenissen* dan [Service Bus] [ lnk-servicebus] *berichten*.

Deze implementatie heeft de volgende gevolgen:

* Op dezelfde manier op gebeurtenissen die gebeurtenis Hubs apparaat naar cloud berichten zijn duurzame en behouden in een hub IoT gedurende maximaal zeven dagen (Zie [apparaat naar cloud configuratieopties][lnk-d2c-configuration]).
* Apparaat-naar-cloud berichten zijn partities over een vaste reeks partities die is ingesteld op Aanmaaktijd (Zie [apparaat naar cloud configuratieopties][lnk-d2c-configuration]).
* Aan de gebeurtenis Hubs, moeten clients apparaat naar cloud berichten leest analogously verwerken partities en plaatst controlepunten. Zie [Gebeurtenis Hubs - door andere gebeurtenissen][lnk-event-hubs-consuming-events].
* Zoals gebeurtenis Hubs gebeurtenissen, apparaat-naar-cloud berichten mag niet meer dan 256 KB en kunnen worden gegroepeerd in batches optimaliseren verzendt. Batches mag niet meer dan 256 KB en maximaal 500 berichten.

Er zijn echter enkele belangrijke verschillen tussen het IoT Hub apparaat naar cloud messaging en gebeurtenis Hubs:

* Zoals wordt beschreven in de [toegang tot IoT Hub] [ lnk-devguide-security] sectie IoT Hub beschikken per apparaat verificatie en toegangsbeheer.
* Hub voor IoT beschikken miljoenen tegelijk verbonden apparaten (Zie [quota en beperken][lnk-quotas]), terwijl de gebeurtenis Hubs is beperkt tot 5000 AMQP verbindingen per naamruimte.
* Er kan geen IoT Hub willekeurige partitioneren met een **PartitionKey**. Apparaat-naar-cloud berichten zijn partities op basis van hun oorspronkelijke **deviceId**.
* Schaalbaarheid van IoT Hub verschilt enigszins schaalbaarheid van de gebeurtenis Hubs. Zie voor meer informatie [Schaalbaarheid IoT Hub][lnk-guidance-scale].

> [AZURE.NOTE] U kunt IoT Hub voor gebeurtenis Hubs niet vervangen in alle scenario's. Bijvoorbeeld, in sommige berekeningen van de verwerking van gebeurtenis deze mogelijk opnieuw partitioneren gebeurtenissen met betrekking tot een andere eigenschap of veld eerst de gegevensstromen te analyseren. In dit scenario kunt u een Hub gebeurtenis loskoppelen van twee gedeelten van de stream verkooppijplijn verwerken. Zie voor meer informatie, *partities* in [Azure gebeurtenis Hubs overzicht][lnk-eventhub-partitions].

Zie voor meer informatie over het gebruik van apparaat-naar-cloud messaging, [IoT Hub-API's en SDK's][lnk-sdks].

> [AZURE.NOTE] Wanneer u met HTTP apparaat naar cloud berichten te verzenden, namen en waarden mag alleen ASCII-alfanumerieke tekens bevatten, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Niet-telemetrielogboek verkeer

Vaak naast het telemetrielogboek gegevenspunten verzenden apparaten ook van berichten en aanvragen die opgevolgd worden uitgevoerd en afhandeling van toepassingsobjectlaag van de bedrijfslogica moeten. Bijvoorbeeld kritieke waarschuwingen die door een bepaalde actie in de back-end of apparaat antwoorden op opdrachten die zijn verzonden vanuit de back-end moeten worden geactiveerd.

Zie voor meer informatie over de beste manier om het verwerken van dit soort bericht, de [Zelfstudie: het verwerken van IoT Hub apparaat naar cloud berichten] [ lnk-d2c-tutorial] zelfstudie.

### <a name="device-to-cloud-configuration-options"></a>Apparaat-naar-cloud configuratieopties

Een hub IoT beschrijft de volgende eigenschappen zodat u kunt het apparaat naar cloud messaging bepalen.

* **Aantal partities**. Stel deze eigenschap bij maken om het aantal partities voor apparaat naar cloud gebeurtenis opname te definiëren.
* **Bewaarbeleid tijd**. Deze eigenschap wordt het tijdstip van het bewaarbeleid voor apparaat naar cloud-berichten. De standaardinstelling is één dag, maar deze kan worden verhoogd tot zeven dagen.

Ook analogously naar gebeurtenis Hubs, IoT Hub kunt u beheren consumenten groepen op het apparaat in de cloud ontvangen eindpunt.

U kunt alle deze eigenschappen, hetzij via een programma via de [provider van de resource IoT Hub REST API's]wijzigen[lnk-resource-provider-apis], of met behulp van de [Azure portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Eigenschappen van anti-mailspoofing

Om te voorkomen apparaat mailspoofing in apparaat-naar-cloud berichten IoT Hub stempelen alle berichten met de volgende eigenschappen:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

De eerste twee bevatten de **deviceId** en **generationId** van het oorspronkelijke apparaat, aan de hand van [identiteit apparaateigenschappen][lnk-device-properties].

De eigenschap **ConnectionAuthMethod** bevat een object JSON serienummer, klikt u met de volgende eigenschappen:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Berichten van de cloud-naar-apparaat

U cloud-to-device berichten verzenden via een eindpunt service-omlaag (**/messages/devicebound**). Een apparaat ontvangt deze via een apparaat-specifieke eindpunt (**/devices/ {deviceId} / berichten/devicebound**).

Elk bericht cloud-naar-apparaat is gericht op één apparaat door in te stellen van de eigenschap **to** naar **/devices/ {deviceId} / berichten/devicebound**.

>[AZURE.IMPORTANT] Elke apparaatwachtrij bevat maximaal 50 cloud-to-device berichten. Wilt meer berichten verzenden naar dezelfde apparaat resultaten in een fout.

> [AZURE.NOTE] Wanneer u cloud-to-device berichten verzendt, namen en waarden mag alleen ASCII-alfanumerieke tekens bevatten, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Levenscyclus van bericht

Om te garanderen ten minste eenmaal bericht is bezorgd, IoT Hub zich blijft voordoen cloud-to-device berichten in per apparaat wachtrijen. Apparaten moeten expliciet *voltooiing* voor IoT Hub om ze te verwijderen uit de wachtrij bevestigen. Dit zorgt ervoor tolerantie tegen connectiviteit en -apparaat fouten.

In het volgende diagram ziet u de grafiek levenscyclus staat voor een bericht cloud-naar-apparaat.

![Levenscyclus van de cloud-to-device bericht][img-lifecycle]

Wanneer de service een bericht verzendt, wordt *in wachtrij*. Wanneer een apparaat wil *ontvangen* een bericht, IoT Hub *vergrendelingen* het bericht (Hiermee stelt u de status op **onzichtbare**) andere threads toestaan op hetzelfde apparaat naar andere berichten ontvangen. Wanneer een apparaat thread is voltooid voor de verwerking van een bericht, krijgt deze IoT Hub door te *voltooien van* het bericht.

Een apparaat kunt ook het volgende:

- *Negeren* het bericht, waardoor IoT-Hub en stel deze in op de **Deadlettered** staat. Opmerking: apparaten die verbinding maken met MQTT geen C2D berichten weigeren.
- *Verlaten* het bericht, waardoor IoT Hub moeten worden geplaatst van het bericht weer in de wachtrij, met de status ingesteld op **in wachtrij**.

Een thread kan mislukken verwerkingstijd van een bericht zonder IoT Hub de hoogte te stellen. In dit geval overgang berichten automatisch van de **onzichtbare** staat terug naar de stand **in wachtrij** na een *time-out voor zichtbaarheid (of vergrendelen)*. De standaardwaarde van deze time-out is één minuut.

Een bericht kunt overgang tussen de **wachtrij** en **onzichtbare** Staten voor maximaal, het aantal keren dat is opgegeven in de eigenschap **bezorging van het maximale aantal** op IoT Hub. Na dit aantal overgangen stelt IoT Hub u de status van het bericht naar **Deadlettered**. Daarnaast IoT Hub stelt u de status van een bericht naar **Deadlettered** na de verlooptijd (Zie [Time to live][lnk-ttl]).

Zie voor een zelfstudie op berichten die cloud-naar-apparaat, [Zelfstudie: het verzenden van berichten van de cloud-naar-apparaat met IoT Hub][lnk-c2d-tutorial]. Voor verwijzing onderwerpen over de manier waarop verschillende API's en SDK's laten zien dat de functionaliteit voor cloud-naar-apparaat, raadpleegt u [IoT Hub-API's en SDK's][lnk-sdks].

> [AZURE.NOTE] Meestal voltooien cloud-to-device berichten wanneer de verlies van het bericht zou geen invloed op de toepassingslogica. Bijvoorbeeld: de inhoud van het bericht heeft behouden in de lokale opslag of een bewerking is uitgevoerd. Het bericht kan ook worden die tijdelijke informatie, waarvan verlies geen voor de functionaliteit van de toepassing gevolgen heeft. Soms kunt u het bericht cloud-to-device voltooien na persistent maken van de taakbeschrijving van de in de lokale opslag langdurige taken. Vervolgens kunt u de toepassing back-end met een of meer berichten van het apparaat naar cloud melden in verschillende fasen van de voortgang van de taak.

### <a name="message-expiration-time-to-live"></a>Bericht verlooptijd (tijd naar live)

Elk bericht cloud-naar-apparaat heeft een verlooptijd. Nu is ingesteld door de service (in de eigenschap **ExpiryTimeUtc** ) of door IoT Hub met de standaard- *TTL* opgegeven als een eigenschap IoT Hub. Zie [Cloud-naar-apparaat configuratieopties][lnk-c2d-configuration].

> [AZURE.NOTE] Een gebruikelijke manier om te profiteren van bericht verlooptijd en voorkomen dat is berichten verzenden naar verbroken apparaten, even naar live waarden instellen. Deze methode realiseert hetzelfde resultaat als de verbindingsstatus apparaat terwijl deze wordt efficiënter worden onderhouden. Wanneer u een bericht bevestigingen aanvraagt, krijgt IoT Hub u welke apparaten zijn ontvangen van berichten en de voor- of mislukt zijn niet online welke apparaten.

### <a name="message-feedback"></a>Bericht feedback

Wanneer u een cloud-to-device-bericht verzendt, kan de service de bezorging van per bericht feedback met betrekking tot de laatste staat van dat bericht aanvragen.

- Als u de eigenschap **Ack** op **positieve instelt**, genereert IoT Hub een feedbackbericht als en alleen als het bericht cloud-to-device bereikt de stand **voltooid** .
- Als u de eigenschap **Ack** op **negatieve instelt**, genereert IoT Hub een feedbackbericht als, het bericht cloud-naar-apparaat de status **Deadlettered** bereikt.
- Als u de eigenschap **Ack** op **volledige instelt**, genereert IoT Hub een feedbackbericht in beide gevallen.

> [AZURE.NOTE] Als **Ack** **vol**is en u niet een feedbackbericht ontvangt, betekent dit dat het Feedbackbericht is verlopen. De service kan niet weet wat is er gebeurd met het oorspronkelijke bericht. In Word Web App moet een service dat het soort feedback verwerken kan voordat het verloopt. De maximale verlooptijd is twee dagen, waarmee voldoende tijd hebt om de service opnieuw uit te voeren als er een fout optreedt.

Zoals wordt uitgelegd in [eindpunten][lnk-endpoints], IoT Hub biedt feedback via een eindpunt service-omlaag (**/messages/servicebound/feedback**) als berichten. De semantiek voor het ontvangen van feedback zijn hetzelfde als voor de cloud-to-device berichten en de hetzelfde [bericht levenscyclus][lnk-lifecycle]. Zo veel mogelijk batch bericht feedback wordt verwerkt in een enkel bericht, klikt u met de volgende indeling:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTime | Tijdstempel dat aangeeft waarop het bericht is gemaakt. |
| Gebruikers-id | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

De hoofdtekst is een JSON serienummer matrix van records, elk voorzien van de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| EnqueuedTimeUtc | Tijdstempel dat aangeeft wanneer het resultaat van het bericht is er gebeurd. Bijvoorbeeld het apparaat dat is voltooid of het bericht is verlopen. |
| OriginalMessageId | **MessageId** van de cloud-to-device bericht waaraan feedback informatie geldt. |
| StatusCode | Vereiste geheel getal. In de feedbackberichten die zijn gegenereerd door IoT Hub gebruikt. <br/> 0 = geslaagd <br/> 1 = bericht is verlopen <br/> 2 = maximum bezorging van het aantal hops <br/> 3 = message geweigerd |
| Beschrijving | Waarden voor **StatusCode**tekenreeks. |
| DeviceId | **DeviceId** van het doel-apparaat van de cloud-to-device bericht waaraan dit stukje feedback betrekking heeft. |
| DeviceGenerationId | **DeviceGenerationId** van het doel-apparaat van de cloud-to-device bericht waaraan dit stukje feedback betrekking heeft. |


>[AZURE.IMPORTANT] De service moet een **MessageId** voor het bericht cloud-to-device kunnen de feedback correlatie met het oorspronkelijke bericht opgeven.

Het volgende voorbeeld ziet u de hoofdtekst van een feedbackbericht.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Configuratieopties cloud-naar-apparaat

Elke hub IoT beschrijft de volgende configuratieopties voor messaging cloud-naar-apparaat.

| Eigenschap | Beschrijving | Bereik en standaard |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Standaard-TTL voor cloud-to-device berichten. | ISO_8601 interval maximaal 2D (minimale 1 minuut). Standaard: 1 uur. |
| maxDeliveryCount | Bezorging van het maximum aantal voor wachtrijen cloud-to-device per apparaat. | 1 tot en met 100. Standaard: 10. |
| feedback.ttlAsIso8601 | Bewaarbeleid voor service-gebonden Feedbackberichten. | ISO_8601 interval maximaal 2D (minimale 1 minuut). Standaard: 1 uur. |
| feedback.maxDeliveryCount | De bezorging van de maximale aantal voor feedback wachtrij. | 1 tot en met 100. Standaard: 100. |

Zie voor meer informatie [IoT maken hubs][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Apparaat-naar-cloud berichten lezen

IoT Hub beschrijft een eindpunt voor uw back-enddatabase services om het apparaat naar cloud-berichten ontvangen door de hub te lezen. Het eindpunt is gebeurtenis Hub-compatibele, waarmee u een van de regelingen de gebeurtenis Hubs-service biedt ondersteuning voor het lezen van berichten.

Wanneer het gebruiken van de [Azure Service Bus SDK voor .NET] [ lnk-servicebus-sdk] of de [Gebeurtenis Hubs - gebeurtenis Processor Host][lnk-eventprocessorhost], kunt u de IoT Hub verbindingstekenreeksen met de juiste machtigingen. Gebruik vervolgens **berichten/gebeurtenissen** als de naam van de Hub van de gebeurtenis.

Wanneer u SDK's (of product integraties) gebruiken die zich niet bewust van IoT-Hub, moet u een gebeurtenis Hub-compatibele eindpunt en de naam van de gebeurtenis Hub-compatibele ophalen van de IoT Hub-instellingen in de [portal van Azure][lnk-management-portal]:

1. Klik in het blad van de hub IoT op **Messaging**.
2. In de sectie **instellingen van het apparaat naar cloud** u de volgende waarden vinden: **gebeurtenis Hub-compatibele eindpunt**, **de naam van de gebeurtenis Hub-compatibele**en **partities**.

    ![Apparaat-naar-cloud-instellingen][img-eventhubcompatible]

> [AZURE.NOTE] Als de SDK een waarde voor **hostnaam** of **Namespace vereist** , moet u het schema verwijderen vanuit het **eindpunt van de gebeurtenis Hub-compatibele**. Stel dat uw eindpunt gebeurtenis Hub-compatibele is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, de **Hostname** zou **iothub-ns-myiothub-1234.servicebus.windows.net**en de **Namespace** zou **iothub-ns-myiothub-1234**.

Vervolgens kunt u een gedeelde-beveiligingsbeleid met de machtigingen **ServiceConnect** verbinding maken met de opgegeven Hub voor de gebeurtenis.

Als u maken van een gebeurtenis Hub-verbindingsreeks wilt met behulp van de vorige informatie, gebruikt u het volgende patroon:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Hier volgt een lijst met SDK's en integraties die u kunt gebruiken met gebeurtenis Hub-compatibele eindpunten dat toegang biedt tot IoT Hub:

* [Java gebeurtenis Hubs-client](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). U kunt de [bron spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) bekijken op GitHub.
* [Een Apache-integratie](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over berichten uitwisselen met IoT Hub.

## <a name="message-format"></a>Berichtindeling

IoT Hub berichten omvatten:

* Een reeks *Systeemeigenschappen*. Eigenschappen die IoT Hub interpreteert of ingesteld. Deze waarde is vooraf ingestelde.
* Een set met *toepassingseigenschappen*. Een woordenlijst met tekenreekseigenschappen van een die de toepassing kunt definiëren en open, zonder te converteren van de berichttekst. IoT Hub wijzigt nooit deze eigenschappen.
* Een ondoorzichtig binaire hoofdtekst.

Zie voor meer informatie over hoe het bericht is gecodeerd in verschillende protocollen, [IoT Hub-API's en SDK's][lnk-sdks].

De volgende tabel bevat de set Systeemeigenschappen in IoT Hub berichten.

| Eigenschap | Beschrijving |
| -------- | ----------- |
| MessageId | Een gebruiker worden ingesteld id voor het bericht, die wordt gebruikt voor de aanvraag beantwoorden patronen. Indeling: Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens) van ASCII-7-bits alfanumerieke tekens + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Volgnummer | Een getal (unieke per apparaat-wachtrij) door IoT Hub toegewezen aan elk bericht cloud-naar-apparaat. |
| Aan | Een bestemming die is opgegeven in de [Cloud-to-Device] [ lnk-c2d] berichten. |
| ExpiryTimeUtc | Datum en tijd van verloopdatum voor het bericht. |
| EnqueuedTime | Datum en tijd waarop die het bericht is ontvangen door IoT Hub. |
| CorrelationId | De tekenreekseigenschap van een in een antwoordbericht dat meestal de MessageId van de aanvraag, in de aanvraag beantwoorden patronen bevat. |
| Gebruikers-id | Een ID die wordt gebruikt om op te geven van de oorsprong van berichten. Wanneer berichten worden gegenereerd door IoT-Hub, is ingesteld op `{iot hub name}`. |
| ACK | Een bericht genereren van feedback. Deze eigenschap wordt gebruikt in de cloud-to-device berichten aanvragen IoT Hub voor het genereren van Feedbackberichten grond van het verbruik van het bericht door het apparaat. Mogelijke waarden: **geen** (standaard): er wordt geen Feedbackbericht wordt gegenereerd, **positieve**: een feedbackbericht ontvangen als het bericht is voltooid, **negatieve**: een feedbackbericht ontvangen als het bericht is verlopen (of bezorging van het maximum aantal is bereikt) zonder te worden uitgevoerd door het apparaat, of de **volledige**: positieve en negatieve. Zie voor meer informatie, [bericht feedback][lnk-feedback]. |
| ConnectionDeviceId | Een ID instellen door IoT Hub op berichten die apparaat-naar-cloud. De presentatie bevat de **deviceId** van het apparaat dat het bericht verzonden. |
| ConnectionDeviceGenerationId | Een ID instellen door IoT Hub op berichten die apparaat-naar-cloud. Bevat de **generationId** (aan de hand van [identiteit apparaateigenschappen][lnk-device-properties]) van het apparaat dat het bericht verzonden. |
| ConnectionAuthMethod | Een verificatiemethode instellen door IoT Hub op berichten die apparaat-naar-cloud. Deze eigenschap bevat informatie over de verificatiemethode gebruikt om te verifiëren van het apparaat dat het bericht verstuurt. Zie voor meer informatie [apparaat naar cloud anti-mailspoofing][lnk-antispoofing].|

## <a name="communication-protocols"></a>Communicatieprotocollen

Hub voor IoT beschikken die u wilt gebruiken, [MQTT][lnk-mqtt], MQTT via WebSockets, [AMQP][lnk-amqp], AMQP worden WebSockets en het HTTP-protocollen voor apparaat aan de clientzijde communicatie. De volgende tabel bevat de hoog niveau aanbevelingen voor uw keuze van protocol:

| Protocol | Wanneer u dit protocol moet kiezen |
| -------- | ------------------------------------ |
| MQTT <br> MQTT via WebSocket     | Op alle apparaten die niet nodig hebben om meerdere apparaten (elk met een eigen referenties per apparaat) verbinding via de dezelfde TLS-verbinding te gebruiken. |
| AMQP <br> AMQP via WebSocket    | Gebruik op veld en cloud gateways om te profiteren van de verbinding multiplexing op apparaten. |
| HTTP    | Wordt gebruikt voor apparaten die andere protocollen niet ondersteunt. |

Houd rekening met de volgende punten wanneer u het protocol voor apparaat aan de clientzijde communicatie kiest:

* **Cloud-naar-apparaat patroon**. HTTP heeft geen een efficiënte manier om het implementeren van server push. Als zodanig wanneer u HTTP gebruikt, apparaten poll uitvoeren onder IoT Hub voor berichten cloud-naar-apparaat. Deze methode werkt niet efficiënt voor zowel het apparaat en de IoT Hub. Klik onder huidige HTTP-richtlijnen weergegeven, moet elk apparaat peilingpagina voor berichten elke 25 minuten of meer. Aan de andere kant ondersteund MQTT AMQP en server push bij het ontvangen berichten cloud-naar-apparaat. Hiermee direct ladingen van berichten van IoT Hub bij het apparaat wordt ingeschakeld. Als bezorging latentie is een probleem, zijn MQTT of AMQP de beste protocollen waarmee. Voor zelden verbonden apparaten, HTTP ook werkt.
* **Veld gateways**. Wanneer u MQTT en HTTP, u meerdere apparaten (elk met een eigen referenties per apparaat) kan geen verbinding kunt maken met behulp van de verbinding met dezelfde TLS. Dus voor [veld gateway-scenario's][lnk-azure-gateway-guidance], deze protocollen optimale zijn, omdat ze een TLS-verbinding tussen het veld gateway en IoT Hub voor elk apparaat met de gateway veld verbonden vereisen.
* **Lage resource-apparaten**. De MQTT en HTTP-bibliotheken hebben een kleiner oppervlak dan de AMQP-bibliotheken. Als zodanig als het apparaat resources (voor bijvoorbeeld minder dan 1 MB RAM) beperkt heeft, mogelijk deze protocollen de uitvoering van de beschikbare alleen protocol.
* **Netwerk transport**. Het standaard AMQP-protocol gebruikt poort 5671, terwijl MQTT luistert op poort 8883, die problemen kan veroorzaken in netwerken die naar niet-HTTP-protocollen zijn gesloten. MQTT via WebSockets, AMQP via WebSockets en HTTP zijn beschikbaar voor gebruik in dit scenario.
* **Pakketgrootte**. MQTT en AMQP zijn binaire protocollen die in compactere nettoladingen dan HTTP resulteren.

> [AZURE.NOTE] Wanneer u een HTTP gebruikt, moet elk apparaat voor cloud-to-device berichten peilingpagina elke 25 minuten of meer. Tijdens de ontwikkeling is het echter aanvaardbaar poll uitvoeren onder vaker dan elke 25 minuten.

## <a name="port-numbers"></a>Poortnummers

Apparaten kunnen communiceren met IoT Hub in Azure wordt aangegeven met verschillende protocollen. Meestal wordt de keuze van protocol bepaald door de specifieke vereisten van de oplossing. De volgende tabel bevat de uitgaande poorten die moeten zijn geopend voor een apparaat om te kunnen gebruiken van een specifiek protocol:

| Protocol | Poorten |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT via WebSockets | 443    |
| AMQP     | 5671    |
| AMQP via WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (Device management) | 5684 |

Als u een hub IoT hebt gemaakt in een Azure regio, blijft de hub hetzelfde IP-adres voor de levensduur van deze hub. Echter behoud van kwaliteit van de service, als Microsoft de hub IoT naar een andere schaaleenheid verplaatst is vervolgens deze toegewezen een nieuwe IP-adres.

## <a name="notes-on-mqtt-support"></a>Notities op MQTT ondersteuning

IoT Hub implementeert het MQTT v3.1.1-protocol met de volgende beperkingen en specifieke werking:

  * **QoS 2 wordt niet ondersteund**. Wanneer een apparaat-client een bericht met **QoS 2 publiceert**, gesloten IoT Hub de netwerkverbinding. Als een apparaat-client zich naar een onderwerp met **QoS 2 abonneert**, verleent IoT Hub maximale QoS niveau 1 in het pakket **SUBACK** .
  * **Berichten behouden blijven niet bewaard**. IoT Hub wordt toegevoegd als een apparaat-client een bericht met de vlag behouden is ingesteld op 1 publiceert, de **x-opt-behouden** de eigenschap application aan het bericht. In dit geval IoT Hub niet het bericht behouden blijft bestaan, maar in plaats daarvan wordt doorgegeven aan de toepassing back-enddatabase.

Zie voor meer informatie [IoT Hub MQTT ondersteunen][lnk-devguide-mqtt].

Als een definitief aandacht, moet u de [Azure IoT protocol gateway] nagaan[ lnk-azure-protocol-gateway] waarmee u kunt een aangepaste protocol krachtige gateway rechtstreeks aan de IoT Hub implementeren. De gateway van Azure IoT protocol kunt u het apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen aanpassen. Deze benadering is echter vereist dat u uitvoert en een gateway aangepaste protocol functioneren.

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe berichten verzenden en ontvangen met IoT-Hub, kunt u de volgende onderwerpen voor de handleiding voor ontwikkelaars interessant kan zijn:

- [Upload bestanden vanaf een apparaat][lnk-devguide-upload]
- [Apparaat identiteiten in IoT Hub beheren][lnk-devguide-identities]
- [Toegang tot IoT Hub beheren][lnk-devguide-security]
- [Apparaat twins gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Directe methode op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende zelfstudies voor IoT Hub interessant kunnen zijn:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]
- [Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub][lnk-c2d-tutorial]
- [Het proces IoT Hub apparaat-naar-cloud-berichten][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
