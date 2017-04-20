<properties
    pageTitle="Azure IoT apparaat SDK voor C - IoTHubClient | Microsoft Azure"
    description="Meer informatie over het gebruik van de bibliotheek IoTHubClient op het apparaat Azure IoT SDK voor C"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT apparaat SDK voor C – meer informatie over het IoTHubClient

Het [eerste artikel](iot-hub-device-sdk-c-intro.md) in deze reeks geïntroduceerd het **Microsoft Azure IoT apparaat SDK voor C**. Dit artikel wordt uitgelegd dat er twee architectuur lagen in SDK. Aan de basis is de bibliotheek **IoTHubClient** die rechtstreeks communicatie met IoT Hub beheert. Er is ook de **serialisatiefunctie** -bibliotheek die op deze maakt om serialisatie services te bieden. In dit artikel vindt aanvullende informatie op de bibliotheek **IoTHubClient** .

Het vorige artikel wordt beschreven hoe u met de bibliotheek **IoTHubClient** gebeurtenissen IoT Hub verzenden en ontvangen van berichten. In dit artikel breidt de mogelijkheden van die discussie wordt uitgelegd hoe u nog nauwkeuriger beheren *wanneer* u gegevens verzenden en ontvangen, aan de **lagere niveaus API's**. We wordt ook wordt uitgelegd hoe u eigenschappen koppelen aan gebeurtenissen (en deze weer ophalen uit berichten die) met de eigenschap verwerking van functies in de bibliotheek **IoTHubClient** . Tot slot vindt aanvullende uitleg van de verschillende manieren om te verwerken die zijn ontvangen van IoT Hub.

Het artikel eind die betrekking hebben op een aantal diverse onderwerpen, waaronder meer over apparaat referenties en hoe u het gedrag van de **IoTHubClient** tot en met configuratieopties wijzigen.

We gebruiken in de voorbeelden van de SDK **IoTHubClient** uitleg over deze onderwerpen. Als u volgen wilt, raadpleegt u de **iothub\_client\_voorbeeld\_http** en **iothub\_client\_voorbeeld\_amqp** toepassingen die van het apparaat Azure IoT SDK uitmaken deel voor C. alles in de volgende secties wordt beschreven in deze voorbeelden wordt geïllustreerd.

U kunt het **Azure IoT apparaat SDK voor C** vinden in de bibliotheek met [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [verwijzing C API](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

## <a name="the-lower-level-apis"></a>De lagere niveaus API 's

Het vorige artikel beschreven de werking van de **IotHubClient** binnen de context van de **iothub\_client\_voorbeeld\_amqp** toepassing. Bijvoorbeeld deze uitleg over hoe u de bibliotheek met deze code geïnitialiseerd.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Deze ook beschreven hoe u met deze functieoproep gebeurtenissen verzendt.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Ook wordt beschreven hoe u berichten ontvangt door een functie terugbellen registreren.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Het artikel blijkt ook hoe u met code zoals de volgende bronnen vrij te maken.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Er zijn echter companion functies op elk van deze API's:

-   IoTHubClient\_l\_CreateFromConnectionString

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy


"L" opnemen deze alle functies in de API-naam. De parameters van elk van deze functies zijn dan die, gelijk aan hun tegenhangers niet-l. Het gedrag van deze functies is echter anders in een belangrijke manier.

Wanneer u belt **IoTHubClient\_CreateFromConnectionString**, een nieuwe thread die wordt uitgevoerd op de achtergrond van de onderliggende bibliotheken maken. Deze thread welke gebeurtenissen worden verzendt en ontvangt berichten, IoT Hub. Geen dergelijke thread wordt gemaakt wanneer u werkt met de API's "l". Het maken van de achtergrondthread is gemakkelijker te maken voor de ontwikkelaar. U hoeft niet te bang expliciet gebeurtenissen berichten verzenden en ontvangen van IoT Hub--worden automatisch op de achtergrond. Daarentegen hebt de "l" API's u expliciete controle over communicatie met IoT Hub als u deze nodig hebt.

Om dit beter te begrijpen, bekijk een voorbeeld:

Wanneer u belt **IoTHubClient\_SendEventAsync**, wat u daadwerkelijk doet de gebeurtenis in een buffer is plaatsen. De achtergrondthread gemaakt wanneer u belt **IoTHubClient\_CreateFromConnectionString** continu deze buffer bewaakt en alle gegevens die deze bevat verzendt naar IoT Hub. Dit gebeurt op de achtergrond op hetzelfde moment dat de belangrijkste thread andere werk wordt uitgevoerd.

Op dezelfde manier als u een functie terugbellen voor berichten met geregistreerd **IoTHubClient\_SetMessageCallback**, u bent de SDK om de achtergrondthread aanroepen van de functie terugbellen als een bericht ontvangen, onafhankelijk van de belangrijkste thread is instructie.

De API's "l" maken geen achtergrondthread. Een nieuwe API moet in plaats daarvan worden aangeroepen expliciet verzenden en ontvangen van gegevens uit IoT Hub. Dit is geïllustreerd in het volgende voorbeeld.

De **iothub\_client\_voorbeeld\_http** -toepassing die opgenomen in de SDK demonstreert de lagere niveaus API's. In dit voorbeeld verzenden we gebeurtenissen op IoT Hub met bijvoorbeeld de volgende code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

De eerste drie regels Maak het bericht en de laatste regel stuurt de gebeurtenis. Echter, zoals eerder is vermeld, "verzenden" de gebeurtenis betekent dat de gegevens alleen in een buffer wordt geplaatst. Niets in het netwerk wordt verzonden wanneer verwijst naar de **IoTHubClient\_l\_SendEventAsync**. In de volgorde naar daadwerkelijk ingress de gegevens op IoT-Hub, moet u aanroepen **IoTHubClient\_l\_DoWork**, zoals in dit voorbeeld:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code (uit de **iothub\_client\_voorbeeld\_http** toepassing) herhaaldelijk belt **IoTHubClient\_l\_DoWork**. Elke keer **IoTHubClient\_l\_DoWork** wordt genoemd, stuurt sommige gebeurtenissen uit de buffer naar IoT Hub en deze een bericht in de wachtrij dat wordt verzonden naar het apparaat zijn opgehaald. Het laatste geval betekent dat als we een functie terugbellen voor berichten hebt geregistreerd, klikt u vervolgens de terugbellen wordt aangeroepen (ervan uitgaande dat alle berichten worden in de wachtrij staat). Zou dat we een dergelijke terugbellen functie hebt geregistreerd met bijvoorbeeld de volgende code:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

De reden dat **IoTHubClient\_l\_DoWork** heet vaak in een lus is die telkens wanneer dit wordt genoemd, stuurt *Sommige* buffer gebeurtenissen naar IoT Hub en haalt *het volgende* bericht in de wachtrij van het apparaat. Elk gesprek is niet gegarandeerd alle buffer gebeurtenissen verzenden of om op te halen alle berichten in de wachtrij. Als u wilt dat alle gebeurtenissen in de buffer verzenden en klik vervolgens op met de andere verwerking dat kunt u deze lus vervangen door de volgende code:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code belt **IoTHubClient\_l\_DoWork** totdat alle gebeurtenissen in de buffer zijn verzonden naar IoT Hub. Opmerking dat dit ook betekent niet dat alle berichten in de wachtrij zijn ontvangen. Deel van de reden hiervoor is dat controleren of er "alle" berichten niet als deterministic een actie. Wat gebeurt er als u "alle" berichten ophalen, maar klikt u vervolgens een ander postvakbeleid wordt verzonden naar het apparaat dat direct na? Er is een betere manier om te verwerken die met een geprogrammeerde time-out. De functie van de terugbellen bericht kan bijvoorbeeld een timer opnieuw telkens wanneer deze wordt geactiveerd. U kunt vervolgens logica als u wilt doorgaan verwerking als, bijvoorbeeld geen berichten zijn ontvangen in de afgelopen *X* seconden schrijven.

Wanneer u bent klaar ingressing gebeurtenissen en ontvangen van berichten, zorg ervoor dat u de bijbehorende functie om resources op te schonen.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Er is in principe slechts één reeks API's verzenden en ontvangen van gegevens met een achtergrondthread en een andere reeks API's die hetzelfde zonder de achtergrondthread doet. Een groot aantal ontwikkelaars heeft mogelijk uw voorkeur de niet - l API's, maar de lagere niveaus API's zijn handig als de ontwikkelaar wil expliciete controle over netwerk. Bijvoorbeeld: sommige apparaten gegevens over tijd en gebeurtenissen alleen ingress verzamelen op gezette (bijvoorbeeld eenmaal uur of één keer per dag). De lagere niveaus API's kunnen u de mogelijkheid op expliciet besturingselement wanneer u gegevens verzenden en van IoT Hub ontvangen. Anderen wordt gewoon de voorkeur aan die de lagere niveaus API's bieden. Alles wat gebeurt er in de belangrijkste thread in plaats van enkele werk foutbericht gegeven op de achtergrond.

Ongeacht model dat u hebt gekozen, moet u eenduidig in welke API's die u gebruikt. Als u door te bellen starten **IoTHubClient\_l\_CreateFromConnectionString**, zorg ervoor dat u alleen de bijbehorende lagere niveaus-API's gebruiken voor opvolging werk:

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy

-   IoTHubClient\_l\_DoWork

Het tegenovergestelde is ook waar. Als u met begint **IoTHubClient\_CreateFromConnectionString**, gebruikt u de niet - l API's voor alle andere bewerkingen.

In het Azure IoT apparaat SDK voor C, gaat u naar de **iothub\_client\_voorbeeld\_http** toepassing voor een volledig voorbeeld van de lagere niveaus API's. De **iothub\_client\_voorbeeld\_amqp** toepassing kan worden verwezen voor een volledige voorbeeld van de niet - l API's.

## <a name="property-handling"></a>Afhandeling van de eigenschap

Dusverre wanneer we verzendende gegevens hebt die worden beschreven, we hebt zijn die verwijzen naar de hoofdtekst van het bericht. Bijvoorbeeld, kunt u deze code overwegen:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In dit voorbeeld stuurt een bericht naar IoT Hub met de tekst "Hallo allemaal." IoT Hub kunnen echter ook eigenschappen aan elk bericht worden gekoppeld. Eigenschappen zijn naam/waardeparen die kunnen worden gekoppeld aan het bericht. We kunt bijvoorbeeld de vorige code als u een eigenschap als bijlage toevoegen aan het bericht wilt aanpassen:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

We starten door te bellen **IoTHubMessage\_eigenschappen** en door te geven de greep van onze bericht. Wat we terugkeren is een **kaart\_verwerken** verwijzing die kan we beginnen met het toevoegen van eigenschappen. De laatste wordt uitgevoerd door te bellen **kaart\_AddOrUpdate**, waarin krijgt een verwijzing naar een kaart\_greep, de naam van de eigenschap en de waarde van de eigenschap. Met deze API kunt we zoveel eigenschappen als we wilt toevoegen.

Wanneer de gebeurtenis van **Gebeurtenis Hubs**wordt gelezen, kan de ontvanger de eigenschappen opsommen en de bijbehorende waarden ophalen. Bijvoorbeeld in .NET zou dit doen door de toegang tot de [collectie Properties op het object EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

In het vorige voorbeeld koppelt we eigenschappen aan een gebeurtenis die we naar IoT Hub verzenden. Eigenschappen kunnen ook worden gekoppeld aan berichten ontvangen van IoT Hub. Als u wilt ophalen van de eigenschappen van een bericht, kunt we code zoals de volgende handelingen uit in de functie van onze bericht terugbellen gebruiken:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

De oproep door naar **IoTHubMessage\_eigenschappen** geeft als resultaat de **kaart\_verwerken** verwijzing. Geven we vervolgens die verwijzing naar **kaart\_GetInternals** een verwijzing naar een matrix van de naam/waardeparen (evenals een telling van de eigenschappen). Op dat moment is een eenvoudige kwestie van de eigenschappen om de waarden die we horen opsommen.

U hoeft niet te gebruiken van eigenschappen in de toepassing. Echter als u wilt deze over alle gebeurtenissen instelt of deze weer ophalen uit berichten, kunt de bibliotheek **IoTHubClient** u gemakkelijk.

## <a name="message-handling"></a>Afhandeling van berichten

Zoals eerder, wordt vermeld bij van IoT Hub berichten reageert door het aanroepen van een functie geregistreerde terugbellen in de bibliotheek **IoTHubClient** . Er is een retour-parameter van deze functie die enkele extra uitleg verdient. Hier is een uittreksel van de functie terugbellen in de **iothub\_client\_voorbeeld\_http** voorbeeld van de toepassing:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Opmerking: het retourtype is **IOTHUBMESSAGE\_voor bestemming\_resultaat** en in dit geval we retourneren **IOTHUBMESSAGE\_GEACCEPTEERDE**. Er zijn andere waarden we van deze functie kan retourneren die wijzigen hoe de bibliotheek **IoTHubClient** reageert op de bericht-terugbellen. Hier vindt u de opties.

-   **IOTHUBMESSAGE\_GEACCEPTEERDE** – het bericht is verwerkt. De bibliotheek **IoTHubClient** wordt de functie terugbellen opnieuw met hetzelfde bericht niet geactiveerd.

-   **IOTHUBMESSAGE\_geweigerd** – het bericht niet is verwerkt en er is geen willen doen in de toekomst. De functie Terugbellen met hetzelfde bericht opnieuw moet niet worden aangeroepen door de **IoTHubClient** -bibliotheek.

-   **IOTHUBMESSAGE\_ABANDONED** – het bericht niet is verwerkt, maar de bibliotheek **IoTHubClient** moet de functie terugbellen opnieuw met hetzelfde bericht roepen.

Voor de eerste twee afzender codes stuurt de bibliotheek **IoTHubClient** een bericht naar IoT Hub dat aangeeft dat het bericht moet worden verwijderd uit de apparaatwachtrij en niet opnieuw worden bezorgd. Het uiteindelijke resultaat is hetzelfde (het bericht wordt verwijderd uit de apparaatwachtrij), maar nog steeds opgenomen of het bericht is geaccepteerd of genegeerd.  Opname van dit onderscheid is nuttig om afzenders van het bericht wie kunnen luisteren naar feedback en achterhalen als een apparaat heeft geaccepteerd of geweigerd van een bepaald bericht.

In het laatste geval ook een bericht is verzonden naar IoT-Hub, maar deze wordt aangegeven dat het bericht moet worden geleverd. Meestal kunt u een bericht annuleren als u een fout tegenkomen maar wilt proberen het bericht om opnieuw te verwerken. In tegenstelling is te negeren van een bericht nodig wanneer u een onherstelbare fout tegenkomen (of als u gewoon besluit dat u niet wilt verwerken van het bericht).

In elk geval moet houden de verschillende afzender codes zodat u kunt het gewenste uit de diatheek **IoTHubClient** gedrag roepen.

## <a name="alternate-device-credentials"></a>Alternatieve apparaat referenties

Zoals eerder is vermeld, het eerste wat u moet doen wanneer u werkt met de bibliotheek **IoTHubClient** is voor een **IOTHUB\_CLIENT\_verwerken** via een oproep zoals de volgende handelingen uit:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

De argumenten van **IoTHubClient\_CreateFromConnectionString** zijn de verbindingsreeks van onze apparaat en een parameter die het protocol dat we gebruiken om te communiceren met IoT Hub aangeeft. De verbindingsreeks heeft een indeling die wordt weergegeven als volgt:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Er zijn vier soorten informatie in deze reeks: de naam van de IoT Hub, IoT Hub achtervoegsel apparaat-ID en gedeelde toegangstoets. U kunt de FQDN-naam (Fully Qualified Domain Name) van een hub IoT aanvragen wanneer u uw exemplaar van de hub IoT in de portal van Azure maakt-Hiermee geeft u de naam van de IoT hub (het eerste deel van de FQDN-naam) en de IoT hub achtervoegsel (de rest van de FQDN-naam). U krijgt de ID van het apparaat en de toegangstoets gedeeld wanneer u uw apparaat hebt geregistreerd met IoT Hub (zoals beschreven in de [vorige artikel](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** kunt u één initialisatie van de bibliotheek. Als u liever, kunt u een nieuwe **IOTHUB\_CLIENT\_verwerken** met behulp van deze afzonderlijke parameters in plaats van de verbindingsreeks. Dit wordt bereikt met de volgende code:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dit gebeurt hetzelfde als **IoTHubClient\_CreateFromConnectionString**.

Lijkt duidelijke zou willen gebruiken **IoTHubClient\_CreateFromConnectionString** in plaats van deze methode uitgebreidere van initialisatie. Houd er rekening mee echter dat wanneer u een apparaat in de IoT registreren Hub wat er een apparaat-ID en het apparaatsleutel (niet een verbindingsreeks is). Het **Onderdeel Apparaatbeheer** SDK-hulpmiddel geïntroduceerd in het [vorige artikel](iot-hub-device-sdk-c-intro.md) maakt gebruik van bibliotheken in de **IoT Azure-service SDK** de verbindingsreeks uit het apparaat-ID, apparaat-toets en IoT Hub hostnaam maken. Dus bellen **IoTHubClient\_l\_maken** wellicht beter omdat deze u de stap bespaart voor het genereren van een verbindingsreeks. Gebruik ongeacht welke methode is handig.

## <a name="configuration-options"></a>Configuratieopties

Alles over de werking van de bibliotheek **IoTHubClient** beschreven weerspiegelt mooi vindt het standaardgedrag. Er zijn echter enkele opties die u instellen kunt om te wijzigen van de werking van de bibliotheek. Hiervoor moet gebruikmaken van de **IoTHubClient\_l\_SetOption** API. Bekijk het volgende voorbeeld:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Er zijn een paar van de opties die vaak worden gebruikt:

-   **SetBatching** (Boole) – als **waar**en vervolgens gegevens verzonden naar IoT Hub in batches wordt verzonden. Als **Onwaar**en vervolgens berichten afzonderlijk worden verzonden. De standaardinstelling is **Onwaar**. Houd er rekening mee dat de optie **SetBatching** is alleen van toepassing op de HTTP-protocol en niet op het MQTT of AMQP-protocol.

-   **Time-out** (niet-ondertekende int) – deze waarde wordt weergegeven (in milliseconden). Als een HTTP-aanvraag of ontvangen van een antwoord langer duren dan ditmaal wilt verzenden, klikt u vervolgens de verbinding wordt geblokkeerd.

De optie batchen is belangrijk. Standaard wordt in de bibliotheek ingresses gebeurtenissen afzonderlijk (een eenmalige gebeurtenis is wat u doorgeven aan **IoTHubClient\_l\_SendEventAsync**). Als de optie batchen **waar**is, wordt in de bibliotheek zo veel gebeurtenissen indien deze uit de buffer (snel aan de maximale berichtgrootte dat IoT Hub accepteert) kunt verzamelt.  De batch gebeurtenis wordt verzonden naar IoT Hub in één HTTP aanroep (de afzonderlijke gebeurtenissen zijn samengevoegd in een matrix JSON). Inschakelen meestal batchen levert groot prestatieverbeteringen sinds u de gegevens van het netwerk bent verkleinen. Bovendien vermindert bandbreedte aangezien u één set HTTP-headers met een gebeurtenis batch in plaats van een reeks kopteksten voor elke afzonderlijke gebeurtenis verzendt. Tenzij u een bepaalde reden hebt hebt, meestal wilt u batchen inschakelen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven in detail de werking van de **IoTHubClient** -bibliotheek zijn gevonden in het **Azure IoT apparaat SDK voor C**. Met deze informatie, moet u een goed inzicht in de mogelijkheden van de bibliotheek **IoTHubClient** hebben. Het [volgende artikel](iot-hub-device-sdk-c-serializer.md) biedt vergelijkbare detail aan de bibliotheek **serialisatiefunctie** .

Meer informatie over het ontwikkelen van voor IoT-Hub, raadpleegt u de [IoT Hub SDK's][lnk-sdks].

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
