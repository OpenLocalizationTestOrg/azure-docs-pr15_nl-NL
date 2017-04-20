<properties
    pageTitle="Met behulp van het apparaat Azure IoT SDK voor C | Microsoft Azure"
    description="Meer informatie over en aan de slag met het voorbeeldcode in het apparaat Azure IoT SDK voor C."
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Inleiding tot het apparaat Azure IoT SDK voor C

Het **apparaat van de Azure IoT SDK** is een verzameling bibliotheken die zijn ontworpen om het proces van gebeurtenissen verzenden en ontvangen berichten van de **Azure IoT Hub** -service. Er zijn verschillende variaties van de SDK, elke doelgroepen een specifiek platform, maar in dit artikel worden de **Azure IoT apparaat SDK voor C**.

Het apparaat Azure IoT SDK voor C is in ANSI-C (C99) om te maximaliseren overdraagbaarheid geschreven. Hierdoor geschikt voor het werken met een aantal platforms en verschillende apparaten, met name wanneer minimaliseren schijf en geheugengebruik is een prioriteit.  

Er zijn tal van platforms waarop de SDK is getest (Zie de [Azure gecertificeerd voor IoT apparaat catalogus](https://catalog.azureiotsuite.com/) voor meer informatie). Hoewel in dit artikel scenario's van de steekproef-code op het Windows-platform bevat, houd er rekening mee dat de code die wordt beschreven in dit artikel precies hetzelfde over het bereik van de ondersteunde platforms is.

In dit artikel moet u worden kennis met de architectuur van het apparaat Azure IoT SDK voor C. Krijgt te zien hoe het initialisatie van de apparaatbibliotheek, gebeurtenissen verzenden op IoT-Hub, evenals berichten ontvangt van deze. De informatie in dit artikel moet selecteren om aan de slag met de SDK, maar ook bevat verwijzingen naar aanvullende informatie over de bibliotheken.

## <a name="sdk-architecture"></a>SDK architectuur

U kunt het **Azure IoT apparaat SDK voor C** vinden in de bibliotheek met [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [verwijzing C API](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

De nieuwste versie van de bibliotheken vindt u in de **basispagina** tak van deze opslagplaats:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Deze bibliotheek bevat het hele gezin van Azure IoT apparaat SDK's. In dit artikel is echter over het apparaat Azure IoT SDK *voor C* die u in de map **c vinden kunt** .

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* De core uitvoering van de SDK vindt u in de **iothub\_client** map waarin de uitvoering van de onderste laag API in de SDK: de **IoTHubClient** -bibliotheek. De bibliotheek **IoTHubClient** bevat API's implementeren onbewerkte messaging voor het verzenden van berichten op IoT-Hub, evenals berichten van deze ontvangen. Wanneer u met deze bibliotheek, u bent verantwoordelijk voor de uitvoering van bericht serialisatie (uiteindelijk via de serialisatiefunctie steekproef Zie hieronder), maar andere details communiceren met IoT Hub voor u zijn afgehandeld.
* De map **serialisatiefunctie** bevat Help-functies en voorbeelden waarin wordt getoond hoe gegevens serialiseren alvorens op Azure IoT Hub met behulp van de clientbibliotheek. Houd er rekening mee dat het gebruik van de serializer niet verplicht en alleen is als service aangeboden. Als u de bibliotheek **serialisatiefunctie** gebruikt, begint u met een model dat Hiermee geeft u de gebeurtenissen die u verzenden naar IoT-Hub, evenals de berichten die u verwacht wilt te ontvangen van deze definiëren. Als het model is gedefinieerd, biedt de SDK een API-oppervlak waarmee u kunt eenvoudig werken met evenementen en berichten zonder te hoeven maken over serialisatie details.
De bibliotheek, is afhankelijk van andere bron openen-bibliotheken die implementeren transport met behulp van verschillende protocollen (MQTT, AMQP).
* De bibliotheek **IoTHubClient** , is afhankelijk van andere bibliotheken bron openen:
   * De bibliotheek met [Azure C gedeeld hulpprogramma](https://github.com/Azure/azure-c-shared-utility) die algemene functionaliteit voor basistaken biedt (zoals tekenreeks, lijst bewerken, IO, enz....) nodig over de verschillende Azure-gerelateerde C SDK's
   * De bibliotheek van [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) client kant implementatie van AMQP geoptimaliseerd voor resource beperking apparaten.
   * De bibliotheek van [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) dat is een algemene bibliotheek het protocol MQTT implementeren en geoptimaliseerd voor resource beperking apparaten.

Dit alles is overzichtelijker door te kijken voorbeeldcode. De volgende secties begeleiden u enkele van de steekproef-toepassingen die deel uitmaken van de SDK. Hiermee geeft u een goed idee voor de verschillende mogelijkheden van de architectuur lagen van de SDK, evenals een introductie over de werking van de API's.

## <a name="before-running-the-samples"></a>Voordat u uitvoert in de voorbeelden

Voordat u de voorbeelden in het apparaat Azure IoT SDK voor C uitvoeren kunt moet u een exemplaar van de service voor uw abonnement op Azure maken als u niet al hebt en 2 taken uitvoeren:
* uw ontwikkelomgeving voorbereiden
* apparaat referenties aanvragen.

Als u maken van een exemplaar van Azure IoT Hub voor uw abonnement op Azure wilt, volgt u de instructies [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

Het [Leesmij-bestand](https://github.com/Azure/azure-iot-sdks/tree/master/c) wordt geleverd bij de SDK bevat instructies voor het voorbereiden van uw ontwikkelomgeving en apparaat referenties te verkrijgen.
De volgende secties vindt enkele extra commentaar op deze instructies.

### <a name="preparing-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Terwijl pakketten zijn beschikbaar voor sommige platforms (zoals NuGet voor Windows of apt_get voor Debian en Ubuntu) en de voorbeelden Gebruik deze pakketten indien beschikbaar, de volgende instructies Detailstijlen het maken van de bibliotheek en de voorbeelden formulier rechtstreeks de code.

U moet eerst een kopie van de SDK van GitHub en verkrijgen vervolgens de bron. U moet een kopie van de bron krijgen van de **basispagina** tak van de [GitHub-bibliotheek](https://github.com/Azure/azure-iot-sdks).

Wanneer u een kopie van de bron hebt gedownload, moet u de stappen in het artikel SDK ["Voorbereiden uw ontwikkelomgeving"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)uitvoeren.


Hier volgen enkele tips waarmee u de procedure wordt beschreven in de handleiding voorbereiden:

-   Wanneer u het hulpprogramma **CMake** hebt geïnstalleerd, kiest u de optie **CMake** toevoegen aan het systeem pad voor **alle gebruikers** (toe te voegen aan **de huidige gebruiker** werkt ook):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Voordat u de **opdrachtprompt voor ontwikkelaars voor VS2015**opent, installeert u de opdrachtregel cijfer-hulpmiddelen. Als u wilt deze's zijn geïnstalleerd, kunt u de volgende stappen uitvoeren:

    1. Start de **Visual Studio-2015** installatieprogramma (of kies **Microsoft Visual Studio 2015** uit de **programma's en onderdelen** het Configuratiescherm en selecteer **wijzigen**).
    
    2. Controleer of de functie **Cijfer voor Windows** is geselecteerd in het installatieprogramma van, maar u kunt ook de optie **GitHub-extensie voor Visual Studio** te leveren IDE-integratie inschakelen:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. De wizard domeinen instellen om te kunnen installeren van de hulpmiddelen voor uitvoeren.

    4. Het cijfer extra **bin** -map toevoegen aan de systeemomgevingsvariabele **pad** . In Windows levert dit het volgende:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Wanneer u alle stappen die worden beschreven in de pagina ['Voorbereiden uw ontwikkelomgeving'](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) hebt voltooid, bent u compileren van de steekproef-toepassingen.

### <a name="obtaining-device-credentials"></a>Het verkrijgen van apparaat referenties

Nu dat uw ontwikkelomgeving is ingesteld, is het volgende wat u moet doen om een reeks apparaat referenties.  Voor een apparaat kunnen voor toegang tot een hub IoT, moet u eerst het apparaat toevoegen aan het register IoT Hub-apparaat. Wanneer u uw apparaat toevoegt krijgt u een reeks apparaat referenties op die u nodig hebt om het apparaat kunnen verbinding maken met een hub IoT. De voorbeeldtoepassingen die we in het volgende gedeelte bespreken verwachten deze referenties in de vorm van een **apparaat-verbindingsreeks**.

Er zijn een paar hulpmiddelen die beschikbaar zijn in de bibliotheek SDK bron openen om te helpen beheren van de IoT Hub. Een is een Windows toepassing met de naam van apparaat Verkenner in het tweede voorbeeld is dat een node.js op basis van cross platform CLI hulpmiddel iothub explorer genoemd. U kunt meer informatie over deze functies [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Terwijl we kunnen worden verzonden vanwege de voorbeelden in dit artikel waarop Windows, gebruiken we het hulpmiddel apparaat Explorer. Maar u kunt ook iothub explorer gebruiken als u liever CLI hulpmiddelen.

De hulpmiddel [Apparaat Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) maakt gebruik van de Azure IoT service-bibliotheken naar diverse functies uitvoeren op IoT-Hub, inclusief het toevoegen van apparaten. Als u een apparaat toevoegen apparaat Explorer hebt gebruikt, krijgt u een bijbehorende verbindingsreeks. U moet deze verbindingsreeks om te maken van de steekproef toepassingen worden uitgevoerd.

Als u niet al bekend met het proces bent, heeft de volgende procedure wordt beschreven hoe apparaat Explorer gebruiken voor het toevoegen van een apparaat en een verbindingsreeks apparaat verkrijgen.

U kunt een Windows installer vinden voor het hulpmiddel apparaat Explorer op de [SDK los pagina](https://github.com/Azure/azure-iot-sdks/releases). Maar u kunt ook het hulpprogramma uitvoeren rechtstreeks vanuit de code **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** openen in **Visual Studio-2015** en het bouwen van de oplossing.

Wanneer u het programma uitvoert ziet u deze interface:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Uw **IoT Hub-verbindingsreeks** in het eerste veld en klik op **bijwerken**. Hiermee wordt het hulpmiddel zodat deze met IoT Hub communiceren kan.

Klik op het tabblad **rollenbeheer** wanneer de verbindingsreeks IoT Hub is geconfigureerd:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Dit is waar u de apparaten die zijn geregistreerd in de hub IoT kunt beheren.

U kunt een apparaat maken door te klikken op de knop **maken** . Een dialoogvenster wordt weergegeven met een set vooraf ingestelde sleutels (primaire en secundaire). Enige u hoeft te doen is een **Apparaat-ID** in te voeren en klik op **maken**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Nadat het apparaat dat is gemaakt, wordt de lijst met apparaten wordt bijgewerkt met alle geregistreerde apparaten, waaronder die u zojuist hebt gemaakt. Als u met de rechtermuisknop op het nieuwe apparaat, ziet u het menu deze:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Als u de optie **Kopieer de verbindingsreeks voor geselecteerde apparaat** kiest, wordt de verbindingsreeks voor uw apparaat wordt gekopieerd naar het Klembord. Houd een kopie van de verbindingsreeks. U moet deze bij de voorbeeldtoepassingen in de volgende secties wordt beschreven.

Wanneer u de bovenstaande stappen hebt uitgevoerd, bent u bent klaar om te beginnen met het uitvoeren van bepaalde code. Beide voorbeelden hebben een constante boven aan de belangrijkste bronbestand waarmee u kunt een verbindingsreeks invoeren. Bijvoorbeeld, de bijbehorende regel van de **iothub\_client\_voorbeeld\_amqp** toepassing als volgt wordt weergegeven.

```
static const char* connectionString = "[device connection string]";
```

Als u volgen wilt, voert u hier de verbindingsreeks van uw apparaat, de oplossing compileren en u kunt wel de steekproef worden uitgevoerd.

## <a name="iothubclient"></a>IoTHubClient

Binnen de **iothub\_client** map in de bibliotheek azure-iot-SDK's is een map met **voorbeelden** waarin een toepassing genoemd **iothub\_client\_voorbeeld\_amqp**.

De Windows-versie van de **iothub\_client\_voorbeeld\_ampq** toepassing bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Deze oplossing bevat één project. Het is handig om te weten dat er vier NuGet-pakketten in deze oplossing geïnstalleerd:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

U moet altijd het pakket **Microsoft.Azure.C.SharedUtility** wanneer u met de SDK werkt. Aangezien dit voorbeeld is afhankelijk van AMQP, moet u ook de **Microsoft.Azure.uamqp** en **Microsoft.Azure.IoTHub.AmqpTransport** -pakketten (er equivalente voor MQTT en HTTP-pakketten). Omdat het voorbeeld wordt de bibliotheek **IoTHubClient** gebruikt, moet u ook het pakket **Microsoft.Azure.IoTHub.IoTHubClient** opnemen in uw oplossing.

U vindt de uitvoering voor de steekproef-toepassing in de **iothub\_client\_voorbeeld\_amqp.c** bronbestand.

We gebruiken deze steekproef-toepassing te doorlopen dat u wat is vereist voor het gebruik van de bibliotheek **IoTHubClient** .

### <a name="initializing-the-library"></a>Initialisatie van de bibliotheek

> [AZURE.NOTE] Voordat u begint te werken met de bibliotheken, moet u mogelijk enkele specifieke initialisatie platform. Als u van plan bent het gebruik van AMQP op Linux moet u bijvoorbeeld de bibliotheek OpenSSL geïnitialiseerd. In de voorbeelden in de [bibliotheek GitHub](https://github.com/Azure/azure-iot-sdks) belt u het hulpprogramma functie **platform_init** wanneer de client wordt gestart en bellen van de functie **platform_deinit** voordat u afsluit. Deze functies zijn gedefinieerd in het bestand van de kop "platform.h". U moet de definities van deze functies voor uw platform doeltoepassing in de [bibliotheek](https://github.com/Azure/azure-iot-sdks) om te bepalen of u moet een code, platform initialisatie opnemen in uw client onderzoeken.

Als u wilt werken met de bibliotheken die u moet eerst een greep van de client IoT Hub toewijzen:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Houd er rekening mee dat geven we een kopie van de verbindingsreeks van onze apparaat voor deze functie (de één we apparaat Explorer afkomstig). We ook opgeven dat het protocol dat we wilt gebruiken. In dit voorbeeld wordt AMQP, maar MQTT en HTTP zijn ook een optie.

Als u hebt een geldig **IOTHUB\_CLIENT\_verwerken**, kunt u de API's gebeurtenissen verzenden en ontvangen van berichten van IoT Hub bellen starten. Besproken om het dialoogvenster.

### <a name="sending-events"></a>Verzenden van gebeurtenissen

Verzenden van gebeurtenissen naar IoT Hub is vereist dat u de volgende stappen uitvoeren:

Maak eerst een bericht:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Vervolgens het bericht verzenden:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Telkens wanneer u een bericht verzendt, kunt u een verwijzing naar een terugbellen-functie die wordt aangeroepen wanneer de gegevens worden verzonden opgeven:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Houd rekening met de oproep door naar de **IoTHubMessage\_Destroy** werken wanneer u klaar bent met het bericht. U moet bellen om vrij de resources die zijn toegewezen wanneer u het bericht hebt gemaakt.

### <a name="receiving-messages"></a>Ontvangen van berichten

Het bericht is een asynchrone bewerking. U hebt geregistreerd eerst een terugbellen moet worden aangeroepen wanneer het apparaat dat een bericht ontvangt:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

De laatste parameter is een void aanwijzer op wat u nodig hebt. Is het een verwijzing naar een geheel getal in de steekproef, maar wordt een verwijzing naar een meer complexe gegevensstructuur. Hierdoor wordt de functie terugbellen om te werken met gedeelde status met de beller van deze functie.

Wanneer het apparaat dat een bericht ontvangt, wordt de functie geregistreerde terugbellen aangeroepen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Opmerking die u gebruikt de **IoTHubMessage\_GetByteArray** functie om op te halen van het bericht, die in dit voorbeeld een tekenreeks is.

### <a name="uninitializing-the-library"></a>De bibliotheek uninitializing

Wanneer u klaar bent met het verzendende evenementen en berichten ontvangt, kunt u de bibliotheek IoT initialisatie. Klik hiervoor probleem de volgende functieoproep:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Dit vrijgemaakt de resources die eerder zijn toegewezen door de **IoTHubClient\_CreateFromConnectionString** functie.

Zoals u ziet, is het is eenvoudig gebeurtenissen verzenden en ontvangen van berichten met de bibliotheek **IoTHubClient** . De bibliotheek omgaat met de details van de communicatie met IoT-Hub, inclusief te gebruiken protocol (dit is vanuit het perspectief van de ontwikkelaar, de optie van een eenvoudige configuratie).

De bibliotheek **IoTHubClient** bevat ook de controle over hoe u de gebeurtenissen die uw apparaat wordt verzonden naar IoT Hub serialiseren. In sommige gevallen dit is een voordeel, maar in andere gevallen is dit een implementatie details waarmee u niet wilt worden weerspiegeld. Als dit het geval is, kunt u overwegen met de bibliotheek **serialisatiefunctie** , waarin wordt beschreven in het volgende gedeelte.

## <a name="serializer"></a>Serialisatiefunctie

Conceptueel de **serialisatiefunctie** -bibliotheek bevindt zich boven aan de bibliotheek **IoTHubClient** in de SDK. Wordt de bibliotheek **IoTHubClient** voor de onderliggende communicatie met IoT Hub gebruikt, maar modelleringsmogelijkheden die de belasting van omgaan met bericht serialisatie uit de ontwikkelaar verwijderen wordt toegevoegd. Hoe blijkt werkt ook deze bibliotheek aanbevolen uit een voorbeeld.

Binnen de **serialisatiefunctie** map in de bibliotheek azure-iot-SDK's is een map met **voorbeelden** dat een toepassing genoemd bevat **simplesample\_amqp**. De Windows-versie van het voorbeeld bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Net als met het vorige voorbeeld, bevat dit item verschillende NuGet-pakketten:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

We de meeste van de volgende in het vorige voorbeeld hebt gezien, maar **Microsoft.Azure.IoTHub.Serializer** nieuw is. Dit is vereist als we de bibliotheek **serialisatiefunctie** gebruiken.

U vindt de uitvoering van de steekproef-toepassing in de **simplesample\_amqp.c** bestand.

De volgende secties begeleiden u door de belangrijkste onderdelen van dit voorbeeld.

### <a name="initializing-the-library"></a>Initialisatie van de bibliotheek

Als u wilt werken met de bibliotheek **serialisatiefunctie** , moet u de initialisatie API's bellen:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

De oproep door naar de **serialisatiefunctie\_initialisatie** functie is een eenmalige oproep en initialisatie van de onderliggende bibliotheek wordt gebruikt. Klik, dat u belt de **IoTHubClient\_CreateFromConnectionString** , functie, dat wil de dezelfde API zoals in de steekproef **IoTHubClient zeggen** . In dit gesprek Hiermee stelt u de verbindingsreeks van uw apparaat (dit is ook waarin u het protocol kiezen u wilt gebruiken). Houd er rekening mee dat dit voorbeeld AMQP als het transport gebruikt, maar kan hebben HTTP.

Tot slot bellen de **maken\_MODEL\_exemplaar** functie. Houd er rekening mee dat **WeatherStation** de naamruimte van het model is en **ContosoAnemometer** de naam van het model is. Nadat het model exemplaar is gemaakt, kunt u deze gebeurtenissen verzenden en ontvangen van berichten. Maar het is belangrijk om te begrijpen wat een model is.

### <a name="defining-the-model"></a>Het model definiëren

Een model in de bibliotheek **serialisatiefunctie** Hiermee definieert u de gebeurtenissen die uw apparaat kunt verzenden naar IoT Hub en de berichten *Acties* genoemd in de taal voor modellering, waaraan deze kan ontvangen. U definieert een model met een set C-macro's als in de **simplesample\_amqp** voorbeeld van de toepassing:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

De **BEGINT\_NAAMRUIMTE** en **einde\_NAAMRUIMTE** macro's beide de naamruimte van het model als argument nemen. Wordt naar verwachting dat tussen deze macro's is de definitie van uw modellen en de gegevensstructuren die de modellen gebruiken.

In dit voorbeeld is één model **ContosoAnemometer**genoemd. Dit model definieert twee gebeurtenissen die uw apparaat naar IoT Hub verzenden kunt: **DeviceId** en **windsnelheid**. Deze drie acties (berichten) die uw apparaat kan ontvangen ook wordt gedefinieerd: **TurnFanOn**, **TurnFanOff**en **SetAirResistance**. Elke gebeurtenis heeft een type en elke actie heeft een naam (en eventueel ook een set parameters).

De gebeurtenissen en acties die zijn gedefinieerd in het model definiëren een API-oppervlak dat u kunt gebeurtenissen verzenden op IoT-Hub, evenals reageren op berichten die naar het apparaat dat is verzonden. Dit is het best begrijpen tot en met een voorbeeld.

### <a name="sending-events"></a>Verzenden van gebeurtenissen

Het model Hiermee definieert u de gebeurtenissen die u naar IoT Hub verzenden kunt. In dit voorbeeld betekent dat een van de twee gebeurtenissen die zijn gedefinieerd met behulp van de macro **WITH_DATA** . Bijvoorbeeld als u een gebeurtenis **windsnelheid** verzenden naar een hub IoT wilt, zijn er een paar stappen betrokken bij het maken van dit gebeurt. De eerste is voor het instellen van de gegevens die we wilt verzenden:

```
myWeather->WindSpeed = 15;
```

Het model die we eerder hebt gedefinieerd, kunnen wij dit wilt doen door in te stellen van een lid van een **structuur**. We coderen met vervolgens serienummer de gebeurtenis die we wilt verzenden:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Deze code serialiseert de gebeurtenis aan een buffer (waarnaar wordt verwezen door de **bestemming**). Tot slot sturen we de gebeurtenis naar IoT hub met deze code:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Dit is een Help-functie in de steekproef-toepassing die onze serienummer gebeurtenis naar IoT Hub verzendt:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Deze code is vergelijkbaar met we in gezien het **iothub\_client\_voorbeeld\_amqp** -toepassing, waarin wordt gemaakt van een bericht van een matrix van bytes en vervolgens gebruikte **IoTHubClient\_SendEventAsync** naar IoT Hub te verzenden. Na die we zojuist hebben om de greep bericht vrij en serienummer gegevensbuffer die we eerder hebt toegewezen.

De tweede naar laatste parameter van **IoTHubClient\_SendEventAsync** is een verwijzing naar een terugbellen-functie die wordt aangeroepen wanneer de gegevens is verzonden. Hier volgt een voorbeeld van een functie Terugbellen:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

De tweede parameter is een aanwijzer naar de gebruikerscontext; de dubbele pijl wordt doorgegeven aan **IoTHubClient\_SendEventAsync**. In dit geval de context van een eenvoudige item is kan, maar elke gewenste.

Dat is alles er is met het verzenden van gebeurtenissen. Het enige wat links bedekt wordt uitgelegd hoe berichten ontvangt.

### <a name="receiving-messages"></a>Ontvangen van berichten

Het bericht werkt op dezelfde manier op de manier waarop berichten in de bibliotheek **IoTHubClient werken** . U hebt geregistreerd eerst een functie van de terugbellen bericht:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Vervolgens kunt u de functie voor terugbellen die wordt aangeroepen wanneer een bericht is ontvangen schrijven:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Deze code is standaardtekst, dat hetzelfde is voor elke oplossing is. Deze functie ontvangt het bericht en zorgt voor deze zoekresultaten omleiden naar de juiste functie tot en met de oproep door naar **EXECUTE\_opdracht**. De functie genoemd is op dit moment is afhankelijk van de definitie van de acties in onze model.

Wanneer u een actie in uw model definieert, moet u een functie die wordt aangeroepen wanneer uw apparaat het bijbehorende bericht ontvangt implementeren. Stel dat uw model Hiermee definieert u deze actie:

```
WITH_ACTION(SetAirResistance, int, Position)
```

U moet een functie met deze handtekening definiëren:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Houd er rekening mee dat de naam van de functie overeenkomt met de naam van de actie in het model en dat de parameters van de functie de parameters zijn opgegeven voor de actie. De eerste parameter is altijd vereist en bevat een aanwijzer naar het exemplaar van onze model.

Wanneer het apparaat dat een bericht dat overeenkomt met deze handtekening ontvangt, wordt de bijbehorende functie wordt genoemd. Naast het lukt om op te nemen de standaardtekst code uit **IoTHubMessage**, dus ontvangen van berichten alleen een kwestie van het definiëren van een eenvoudige functie voor elke actie die is gedefinieerd in het model.

### <a name="uninitializing-the-library"></a>De bibliotheek uninitializing

Wanneer u klaar bent met het verzendende gegevens en berichten ontvangt, kunt u de bibliotheek IoT initialisatie:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Elk van deze drie functies worden uitgelijnd met de drie initialisatie-functies die eerder is beschreven. Aanroepen van deze API's zorgt ervoor dat u eerder hebt toegewezen bronnen vrij.

## <a name="next-steps"></a>Volgende stappen

In dit artikel waarvoor de basisbeginselen van het gebruik van de bibliotheken in het **Azure IoT apparaat SDK voor C**. Deze voorzien u weinig gegevens om te begrijpen wat inbegrepen in de SDK, de architectuur en hoe u wegwijs in de voorbeelden van Windows. Het volgende artikel blijft de beschrijving van de SDK wordt uitgelegd [meer informatie over de IoTHubClient-bibliotheek](iot-hub-device-sdk-c-iothubclient.md).

Meer informatie over het ontwikkelen van voor IoT-Hub, raadpleegt u de [IoT Hub SDK's][lnk-sdks].

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
