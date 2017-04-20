<properties
 pageTitle="Handleiding voor ontwikkelaars - apparaat identiteit register | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - beschrijving van het apparaat identiteit register en hoe gebruiken voor het beheren van uw apparaten"
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

# <a name="manage-device-identities-in-iot-hub"></a>Apparaat identiteiten in IoT Hub beheren

## <a name="overview"></a>Overzicht

Elke hub IoT heeft een apparaat identiteit register met informatie over de apparaten die verbinding maken met de hub zijn toegestaan. Voordat u een apparaat kunt verbinden met een hub, moet er een vermelding voor dat apparaat in de hub apparaat identiteit register. Een apparaat moet ook met de hub op basis van de referenties die zijn opgeslagen in het register van apparaat identiteit verifiëren.

Op hoog niveau is het apparaat identiteit register een REST-ondersteuning verzameling apparaat identiteit resources. Wanneer u een fragment aan het register toevoegen, IoT Hub Hiermee maakt u een reeks per apparaat resources in de service zoals de rij met tijdens een vlucht cloud-to-device berichten.

### <a name="when-to-use"></a>Wanneer gebruikt u

Gebruik het apparaat identiteit register wanneer u nodig hebt voor het inrichten van apparaten die verbinding maken met u IoT hub en wanneer u moet per apparaat-toegang tot de eindpunten apparaat-omlaag in de hub.

> [AZURE.NOTE] Het apparaat identiteit register bevat niet alle toepassingsspecifieke-metagegevens.

## <a name="device-identity-registry-operations"></a>Apparaat identiteit registerbewerkingen

Het register IoT Hub apparaat identiteit bevat de volgende bewerkingen:

* Apparaat identiteit maken
* Apparaat identiteit bijwerken
* De identiteit apparaat op ID ophalen
* Apparaat identiteit verwijderen
* Lijst tot 1000 identiteiten
* Alle identiteiten exporteren naar Azure-blobopslag
* Identiteiten importeren uit Azure-blobopslag

Alle deze bewerkingen optimistische gelijktijdigheid kunnen gebruiken als de opgegeven in [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] De enige manier om op te halen, alle identiteiten in van een hub identiteit register is het gebruik van de [exporteren] [ lnk-export] functionaliteit.

Een Hub voor IoT apparaat identiteit register:

- Bevat geen de metagegevens van een toepassing.
- Kunnen worden geopend als een woordenlijst, met behulp van de **deviceId** als de sleutel.
- Ondersteunt geen duidelijke query's.

Een oplossing IoT heeft meestal een afzonderlijke oplossing / regiospecifieke archief met metagegevens toepassingsspecifieke. De oplossing / regiospecifieke store in een oplossing smart building legt bijvoorbeeld de ruimte waarin een temperatuursensor wordt geïmplementeerd.

> [AZURE.IMPORTANT] Gebruik alleen het apparaat identiteit register voor Apparaatbeheer en inrichten van bewerkingen. Hoge doorvoer bewerkingen tijdens runtime moeten niet hangt af van het uitvoeren van bewerkingen in het register van de identiteit apparaat. De status van de verbinding van een apparaat controleren voordat u een opdracht verzendt is bijvoorbeeld niet een ondersteunde patroon. Controleer de [tarieven beperken] [ lnk-quotas] voor het register apparaat-identiteit en de [apparaat heartbeat] [ lnk-guidance-heartbeat] patroon.

## <a name="disable-devices"></a>Apparaten uitschakelen

U kunt apparaten uitschakelen door de eigenschap **status** van een identiteit in het register bij te werken. Meestal gebruikt u deze eigenschap in twee scenario's:

- Tijdens een inrichten van de integratie. Zie voor meer informatie, [Apparaat inrichting][lnk-guidance-provisioning].
- Als u om een reden, kunt u een apparaat is gehackt is of is geworden onbevoegde.

## <a name="import-and-export-device-identities"></a>Apparaat-identiteiten importeren / exporteren

U kunt apparaat identiteiten bulksgewijs exporteren uit een IoT-hub identiteit register, met behulp van asynchrone bewerkingen op het [IoT Hub resource provider eindpunt][lnk-endpoints]. Exports zijn langdurige-functies die gebruikmaken van een klant opgegeven blob container apparaat identiteitsgegevens lezen in het register identiteit worden opgeslagen.

Kunt u apparaat identiteiten bulksgewijs importeren in een IoT-hub identiteit register, met behulp van asynchrone bewerkingen op het [IoT Hub resource provider eindpunt][lnk-endpoints]. Invoer zijn langdurige-functies die gebruikmaken van gegevens in een container blob klant opgegeven bij het wegschrijven van apparaat identiteitsgegevens in het register van de identiteit apparaat.

- Zie [IoT Hub resource provider REST API's]voor gedetailleerde informatie over het importeren en exporteren API's,[lnk-resource-provider-apis].
- Zie voor meer informatie over het uitvoeren van importeren en exporteren van taken, [bulksgewijs van het beheer van IoT Hub apparaat identiteiten][lnk-bulk-identity].

## <a name="device-provisioning"></a>Mobiele apparaten inrichten

Het apparaatgegevens die een opgegeven IoT-oplossing zijn opgeslagen, is afhankelijk van de specifieke vereisten van deze oplossing. Maar ten minste een oplossing moet apparaat identiteiten en opslaan verificatie toetsen. Azure IoT Hub bevat een identiteit register die waarden voor elk apparaat zoals-id's, verificatiesleutels en statuscodes kunt opslaan. Een oplossing kunt u andere Azure services zoals Azure-tabelopslag, Azure-blobopslag of Azure DocumentDB gebruiken voor de opslag van apparaatgegevens extra.

*Mobiele apparaten inrichten* , is het proces van het eerste apparaatgegevens toevoegen aan de winkels in uw oplossing. Als u wilt een nieuwe apparaat wilt aansluiten op uw hub inschakelt, moet u een nieuwe apparaat-ID en sleutels toevoegen aan het register IoT Hub-identiteit. Als onderdeel van het inrichten moet u mogelijk geïnitialiseerd apparaat-specifieke gegevens in andere winkels oplossing.

## <a name="device-heartbeat"></a>Apparaat heartbeat

Het register van de identiteit IoT Hub bevat een veld met de naam van **connectionState**. U moet het veld **connectionState** alleen gebruiken tijdens de ontwikkeling en foutopsporing, IoT oplossingen niet query moeten het veld tijdens runtime (bijvoorbeeld om te controleren als een apparaat is aangesloten om te bepalen of u een bericht cloud-naar-apparaat of een SMS-bericht verzenden).

Als uw oplossing IoT weet ik of een apparaat is aangesloten (tijdens runtime, of met meer nauwkeurigheid dan vindt u de eigenschap **connectionState** ), uw oplossing het *heartbeat patroon*te implementeren.

In het patroon heartbeat, worden berichten van apparaat-naar-cloud ten minste eenmaal voor elke vaste hoeveelheid tijd (bijvoorbeeld ten minste één keer per uur) het apparaat verzonden. Dit betekent dat zelfs als een apparaat geen gegevens om te verzenden, deze nog steeds stuurt een lege apparaat naar cloud-bericht (meestal met een eigenschap dat het een heartbeat). Klik op de zijden van de service, de oplossing onderhoudt een kaart met de laatste heartbeat ontvangen voor elk apparaat en wordt ervan uitgegaan dat er een probleem met een apparaat is als deze niet een heartbeat-bericht binnen de verwachte tijd ontvangt.

Een meer complexe implementatie kan bevat de gegevens uit [bewerkingen monitoring] [ lnk-devguide-opmon] apparaten die zijn probeert verbinding maken of communiceren, maar verbroken identificeren. Wanneer u het patroon heartbeat implementeert, Controleer [IoT Hub quota en Throttles][lnk-quotas].

> [AZURE.NOTE] Als een oplossing IoT moet de verbindingsstatus apparaat uitsluitend om te bepalen of cloud-to-device berichten verzenden en berichten niet tot grote sets met apparaten uitgezonden worden, is een veel eenvoudiger patroon aandachtspunten voor het gebruik van een korte verlooptijd. Dit bereikt hetzelfde resultaat als een apparaat verbinding staat register met het patroon heartbeat, terwijl deze wordt aanzienlijk efficiënter worden onderhouden. Het is ook mogelijk door het aanvragen van een bericht bevestigingen, moeten meldingen ontvangen door de IoT Hub van welke apparaten zijn ontvangen van berichten en die niet online zijn of zijn is mislukt.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over het devcie identiteit register.

## <a name="device-identity-properties"></a>Apparaateigenschappen identiteit

Apparaat identiteiten worden weergegeven als JSON-documenten met de volgende eigenschappen.

| Eigenschap | Opties | Beschrijving |
| -------- | ------- | ----------- |
| deviceId | vereist, alleen-lezen op updates | Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens) van ASCII-7-bits alfanumerieke tekens + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | vereist, alleen-lezen | Een hub gegenereerde, hoofdlettergevoelige tekenreeks maximaal 128 tekens. Dit wordt gebruikt om te onderscheiden van apparaten waarop de dezelfde **deviceId**wanneer ze zijn verwijderd en opnieuw gemaakt. |
| ETag | vereist, alleen-lezen | Een tekenreeks die vertegenwoordigt een zwakke etag voor de identiteit van het apparaat, aan de hand van [RFC7232][lnk-rfc7232].|
| auth | optionele | Een samengestelde object met verificatie-informatie en beveiliging materiaal. |
| auth.symkey | optionele | Een samengestelde-object met een primaire en secundaire toets heeft, opgeslagen in base64-indeling. |
| status | Vereist | Een access-indicator. Kan worden **ingeschakeld** of **uitgeschakeld**. Als **ingeschakeld**, het apparaat dat is toegestaan om verbinding te maken. Als **uitgeschakeld**, dit apparaat geen toegang een eindpunt apparaat-omlaag tot. |
| statusReason | optionele | Een 128 tekens lang tekenreeks die de reden voor de apparaatstatus is identiteit opgeslagen. Alle UTF-8 tekens zijn toegestaan. |
| statusUpdateTime | alleen-lezen | Een tijdelijke aanduiding, met de datum en tijd van de laatste statusupdate. |
| connectionState | alleen-lezen | Een veld waarin wordt aangegeven dat de verbindingsstatus: **verbonden** of **verbroken**. Dit veld staat voor de weergave van de IoT Hub van de verbindingsstatus van het apparaat. **Belangrijk**: dit veld alleen ter ontwikkeling/foutopsporing moet worden gebruikt. De verbindingsstatus wordt alleen voor apparaten met MQTT of AMQP bijgewerkt. Daarnaast is gebaseerd op ping-protocol niveau opdrachten (MQTT ping-opdrachten of AMQP ping-opdrachten), en er een maximale vertraging van alleen 5 minuten. Daarom kunnen er onrechte, zoals apparaten gemeld verbonden, maar die daadwerkelijk de verbinding is verbroken. |
| connectionStateUpdatedTime | alleen-lezen | Een tijdelijke aanduiding, met de datum en de laatste keer dat de verbindingsstatus werd bijgewerkt. |
| lastActivityTime  | alleen-lezen | Een tijdelijke aanduiding, waarin de datum en de laatste keer dat het apparaat dat verbinding, ontvangen of een bericht heeft gestuurd. |

> [AZURE.NOTE] Verbindingsstatus kan alleen bestaan uit de weergave van de IoT Hub de status van de verbinding. Updates voor deze status kunnen worden vertraagd, afhankelijk van netwerkcondities en configuraties.

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u het gebruik van het register IoT Hub apparaat identiteit hebt geleerd, kunt u in de volgende onderwerpen voor de handleiding voor ontwikkelaars interessant kunnen zijn:

- [Toegang tot IoT Hub beheren][lnk-devguide-security]
- [Apparaat twins gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Directe methode op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende IoT Hub zelfstudie interessant kunnen zijn:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md