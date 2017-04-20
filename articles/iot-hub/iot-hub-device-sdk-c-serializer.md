<properties
    pageTitle="Azure IoT apparaat SDK voor C - serialisatiefunctie | Microsoft Azure"
    description="Meer informatie over het gebruik van de bibliotheek serialisatiefunctie op het apparaat Azure IoT SDK voor C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT apparaat SDK voor C – meer informatie over serialisatiefunctie

Het [eerste artikel](iot-hub-device-sdk-c-intro.md) in deze reeks geïntroduceerd het **Azure IoT apparaat SDK voor C**. Het volgende artikel aangeboden een gedetailleerde beschrijving van de [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). In dit artikel is licenties voor de SDK voltooid doordat een gedetailleerde beschrijving van de resterende component: de **serialisatiefunctie** -bibliotheek.

Het inleidende artikel wordt beschreven hoe u met de bibliotheek **serialisatiefunctie** welke gebeurtenissen worden berichten verzenden en ontvangen van IoT Hub. In dit artikel, breiden we die discussie uit doordat een gedetailleerde uitleg van de manier waarop uw gegevens met de taal van de macro **serialisatiefunctie** model. Het artikel ook bevat meer informatie over hoe de bibliotheek berichten serialiseert (en in sommige gevallen hoe u het gedrag serialisatie kunt bepalen). Sommige parameters u kunt wijzigen die bepalen van de grootte van de modellen die u maakt ook beschreven.

Ten slotte revisits het artikel sommige onderwerpen in de vorige artikelen zoals bericht en afhandeling van de eigenschap. Terwijl we leert, worden sommige functies werken op dezelfde manier met behulp van de bibliotheek **serialisatiefunctie** als met de bibliotheek **IoTHubClient** .

Alles beschreven in dit artikel is gebaseerd op de **serialisatiefunctie** SDK voorbeelden. Als u volgen wilt, raadpleegt u de **simplesample\_amqp** en **simplesample\_http** toepassingen die zijn opgenomen in het apparaat Azure IoT SDK voor C.

U kunt het **Azure IoT apparaat SDK voor C** vinden in de bibliotheek met [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub en details van de API in de [verwijzing C API](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html)bekijken.

## <a name="the-modeling-language"></a>De taal modellering

Het [inleidende artikel](iot-hub-device-sdk-c-intro.md) in deze reeks kennis het **Azure IoT apparaat SDK voor C** modeling taal instelt in het voorbeeld in het **simplesample\_amqp** toepassing:

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

Zoals u ziet, wordt de taal modellering is gebaseerd op C macro's. U altijd de definitie van uw met begint **BEGIN\_NAAMRUIMTE** en eindigen altijd op **einde\_NAAMRUIMTE**. Algemene naam opgeven voor de naamruimte voor uw bedrijf of, zoals in dit voorbeeld het project dat u bezig is.

Wat gaat binnen de naamruimte model definities zijn. In dit geval is er één model voor een anemometer. Nogmaals, het model kunt naam geven, maar gewoonlijk deze voor het apparaat of type gegevens dat u wilt uitwisselen met IoT Hub wordt genoemd.  

Modellen bevatten een definitie van de gebeurtenissen die u kunt ingress op IoT Hub (de *gegevens*) en de berichten die u van IoT Hub (de *acties ontvangen kunt*). Zoals u in het voorbeeld zien kunt, hebben gebeurtenissen een type en een naam; acties hebben een naam en optionele parameters (elk met een type).

Wat niet in dit voorbeeld wordt gedemonstreerd zijn extra gegevenstypen die worden ondersteund door de SDK. Besproken om het dialoogvenster.

> [AZURE.NOTE] IoT Hub verwijst naar de gegevens die een apparaat als *gebeurtenissen*, stuurt terwijl de taal modellering ernaar als de *gegevens verwijst* (met **WITH_DATA**gedefinieerd). Op dezelfde manier verwijst IoT Hub naar de gegevens die u naar apparaten als *berichten*, verzendt terwijl de taal modellering ernaar als *Acties verwijst* (gedefinieerd met **WITH_ACTION**). Let erop dat deze voorwaarden door elkaar kunnen worden gebruikt in dit artikel.

### <a name="supported-data-types"></a>Ondersteunde gegevenstypen

De volgende gegevenstypen worden ondersteund in modellen die zijn gemaakt met de bibliotheek **serialisatiefunctie** :

| Type                    | Beschrijving                            |
|-------------------------|----------------------------------------|
| dubbele                  | dubbele precisie Drijvendekommagetal |
| int                     | 32-bits geheel getal                         |
| toegestane achterstand                   | Drijvendekommagetal één precisie van formules |
| lange                    | lange integer                           |
| int8\_t                 | 8-bits geheel getal                          |
| Int16\_t                | 16-bits geheel getal                         |
| Int32\_t                | 32-bits geheel getal                         |
| Int64\_t                | 64-bits geheel getal                         |
| BOOL                    | Booleaanse waarde                                |
| ASCII\_teken\_ptr        | ASCII-tekenreeks                           |
| EDM\_datum\_tijd\_OFFSET | datum-tijdverschil                       |
| EDM\_GUID               | GUID                                   |
| EDM\_binaire             | binair getal                                 |
| DECLAREREN\_structuur         | complexe gegevens                      |

Laten we beginnen met het laatste gegevenstype. De **DECLARE\_structuur** kunt u complexe gegevenstypen, dat wil zeggen de andere primitieve typen groepen definiëren. Deze groeperingen kunnen we definiëren van een model dat ziet er zo uit:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Onze model bevat een eenmalige gebeurtenis van het type **TestType**. **TestType** is een complexe type die verschillende leden, die gezamenlijk aantonen primitieve typen worden ondersteund door de **serialisatiefunctie** modeling taal bevat.

Met een model als volgt, kunnen we code om gegevens te verzenden naar IoT Hub dat wordt weergegeven als volgt schrijven:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

In principe bent we het toewijzen van een waarde aan elk lid van de **Test** -structuur en vervolgens roepen **SendAsync** als u wilt verzenden de **Test** -gebeurtenis in de cloud. **SendAsync** is een Help-functie die een eenmalige gebeurtenis naar IoT Hub verzendt:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Deze functie serialiseert van de opgegeven gebeurtenis en verzonden naar IoT Hub met behulp van **IoTHubClient\_SendEventAsync**. Dit is dezelfde code die in vorige artikelen (**SendAsync** omvat de logica in een handige functie).

Eén van andere Help-functie gebruikt in de vorige code is **GetDateTimeOffset**. Deze functie Hiermee transformeert u de opgegeven tijd in een waarde van het type **EDM\_datum\_tijd\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Als u deze code uitvoert, wordt het volgende bericht verzonden naar IoT Hub:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Houd er rekening mee dat de serialisatie JSON is, die is de notatie die is gegenereerd door de **serialisatiefunctie** -bibliotheek. Bedenk ook dat elk lid van het serienummer JSON-object komt overeen met de leden van het **TestType** die we gedefinieerd in het model. De waarden ook precies overeenkomen met die worden gebruikt in de code. Bedenk wel dat de binaire gegevens base64-codering is: "AQID" is de base64 codering van wiskundige expressies {0x01, 0x02, 0x03}.

In dit voorbeeld wordt het voordeel van het gebruik van de bibliotheek **serialisatiefunctie** --Hierdoor kunnen wij JSON verzenden naar de cloud, zonder dat u moet expliciet handelen serialisatie in onze-toepassing. Alle bang te zijn er is de waarden van de Gegevensgebeurtenissen die instellen in onze model en klikt u vervolgens het aanroepen van eenvoudige API's als u wilt verzenden die gebeurtenissen in de cloud.

Met deze gegevens kunt we modellen die het bereik van de ondersteunde gegevenstypen, waaronder complexe typen (we kan zelfs complexe typen binnen andere complexe typen omvatten) bevatten definiëren. Echter hij serienummer JSON gegenereerd door het bovenstaande voorbeeld Hiermee worden een belangrijk punt. Bepaalt *hoe* de infobalk voor berichten gegevens met de bibliotheek **serialisatiefunctie** precies hoe de JSON is gevormd. Dat bepaald punt wat besproken volgende is.

## <a name="more-about-serialization"></a>Meer informatie over serialisatie

De vorige sectie wordt een voorbeeld van de uitvoer gegenereerd door het bestand **serialisatiefunctie** gemarkeerd. In dit gedeelte wordt we wordt uitgelegd hoe gegevens in de bibliotheek worden serialiseert en hoe u dit gedrag met de serialisatie API's kunt bepalen.

Als u wilt doorgaan naar de discussie op serialisatie, werkt we met een nieuw model op basis van een thermostaat. Eerst we bieden achtergrondinformatie van scenario we te adres probeert.

Willen we een thermostaat die temperatuur en vochtigheid metingen model. Elke gegevensitem dient te worden verzonden naar IoT Hub anders. Standaard de ingresses thermostaat een gebeurtenis temperatuur eenmaal elke 2 minuten; een gebeurtenis vochtigheid is ingressed elke 15 minuten. Wanneer een gebeurtenis ingressed is, moet een tijdstempel dat wordt de tijd wordt weergegeven dat de bijbehorende temperatuur of vochtigheid is gemeten bevatten.

Dit scenario gegeven, we krijgt te zien hoe twee verschillende manieren om de gegevens modelleren en leggen we het effect dat modellering op de serienummer uitvoer heeft.

### <a name="model-1"></a>Model 1

Hier is de eerste versie van een model dat ondersteuning biedt voor het vorige scenario:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Dat het model twee Gegevensgebeurtenissen bevat notitie: **temperatuur** en **vochtigheid**. In tegenstelling tot de voorgaande Column, is het type van elke gebeurtenis een structuur die is gedefinieerd met behulp van **DECLARE\_structuur**. **TemperatureEvent** bevat een temperatuur maat en een tijdstempel; **HumidityEvent** bevat een vochtigheid maat en een tijdstempel. Dit model ontstaat een natuurlijke manier om het modelleren van de gegevens voor het scenario hierboven is beschreven. Wanneer we een gebeurtenis naar de cloud verzendt, hetzij vervolgens versturen we een temperatuur/tijdstempel of een paar vochtigheid/tijdstempel.

We kunnen een gebeurtenis temperatuur verzenden naar de cloud met bijvoorbeeld de volgende code:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

We hier hard gecodeerde waarden gebruiken voor temperatuur en vochtigheid in de steekproef-code, maar Stel dat we werkelijk deze waarden ophaalt van de corresponderende sensors op thermostaat steekproeven.

De bovenstaande code gebruikt de **GetDateTimeOffset** helper die eerder is geïntroduceerd. Redenen die wissen later worden, worden deze code expliciet gescheiden door de taak van serialiseren en het verzenden van de gebeurtenis. De vorige code serialiseert de temperatuur gebeurtenis in een buffer. **SendMessage** is een Help-functie (opgenomen in **simplesample\_amqp**) die de gebeurtenis op IoT Hub verzendt:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Deze code is een subset van de **SendAsync** helper die worden beschreven in de vorige sectie, zodat we u naar het opnieuw gaat Won't.

Als we de vorige code voor het verzenden van de gebeurtenis temperatuur uitvoert, wordt dit serienummer formulier van de gebeurtenis verzonden naar IoT Hub:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

We een temperatuur van het type **TemperatureEvent** verzendt en die structuur bevat een lid **temperatuur** en **tijd** . Dit is rechtstreeks doorgevoerd in de serienummer gegevens.

Daarnaast kunnen we een gebeurtenis vochtigheid met deze code verzenden:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Het serienummer formulier dat wordt verzonden naar IoT Hub ziet er als volgt uit:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Dit is nogmaals, zoals verwacht.

Met dit model, kunt u zich voorstellen hoe extra gebeurtenissen kunnen eenvoudig worden toegevoegd. U meer structuren met definiëren **DECLARE\_structuur**, en de bijbehorende gebeurtenis opnemen in het model met **door\_gegevens**.

Nu kunnen we het model wijzigen zodat deze dezelfde gegevens bevat, maar met een andere structuur.

### <a name="model-2"></a>Model 2

U kunt deze alternatieve model een:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

In dit geval we hebt opgeheven, de **DECLARE\_structuur** macro's en gegevensitems uit onze scenario met eenvoudige typen van de taal modellering gewoon definieert.

Alleen betrekking heeft op het moment dat we u de gebeurtenis **tijd** negeren. Met deze braakgelegde volgt de code wilt ingress **temperatuur**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code stuurt de volgende serienummer gebeurtenis op IoT Hub:

```
{"Temperature":75}
```

En de code voor het verzenden van de gebeurtenis vochtigheid wordt als volgt weergegeven:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code sturen deze naar IoT Hub:

```
{"Humidity":45}
```

Er zijn dusverre nog geen verrassingen. Nu we wijzigen hoe we de macro GESERIALISEERD gebruiken.

De macro **GESERIALISEERD** kunt werken met meerdere Gegevensgebeurtenissen als argumenten. Hierdoor kunnen wij de **temperatuur** en **vochtigheid** gebeurtenis samen serialiseren en deze in een oproep naar IoT Hub verzenden:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

U kunt aan te raden dat het resultaat van deze code is waar twee Gegevensgebeurtenissen IoT Hub worden verzonden:

[

{"Temperatuur": 75},

{"Vochtigheid": 45}

]

U kunt met andere woorden, denkt dat deze code hetzelfde is als de **temperatuur** en **vochtigheid** afzonderlijk verzenden. Alleen een gemak beide gebeurtenissen om aan te geven **GESERIALISEERD** in dezelfde aanroep is. Dat is echter niet de hoofdletters/kleine letters. De bovenstaande code wordt in plaats daarvan deze gegevensgebeurtenis één verzonden naar IoT Hub:

{"Temperatuur": 75, "vochtigheid": 45}

Dit kan lijken vreemd omdat onze model **temperatuur** en **vochtigheid** wordt gedefinieerd als twee *afzonderlijke* gebeurtenissen:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Niet model meer bondig, deze gebeurtenissen waar **temperatuur** en **vochtigheid** zich in dezelfde structuur bevinden we:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Als we dit model hebt gebruikt, zou zij beter te begrijpen hoe **temperatuur** en **vochtigheid** in hetzelfde serienummer bericht zou worden verzonden. Echter niet mogelijk wissen waarom deze op die manier werkt wanneer u beide Gegevensgebeurtenissen doorgeven aan **GESERIALISEERD** met model 2.

Dit probleem is beter te begrijpen als u weet dat de hypothesen die de bibliotheek **serialisatiefunctie** is aangebracht. Om inzicht te krijgen van dit artikel leest u terug naar onze model:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Denk na van dit model object-georiënteerd genoemd. In dit geval we een fysiek apparaat (een thermostaat) bent modeling en dat apparaat bevat kenmerken zoals **temperatuur** en **vochtigheid**.

We kunnen de hele status van onze model met code zoals de volgende verzenden:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Als de waarden van temperatuur, vochtigheid en tijd zijn ingesteld, zien we een gebeurtenis als volgt naar IoT Hub verzonden:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Soms komt u mogelijk alleen wilt verzenden *bepaalde* eigenschappen van het model in de cloud (dit is met name waar als het model een groot aantal Gegevensgebeurtenissen bevat). Is het handig om alleen een subset van Gegevensgebeurtenissen, zoals in het eerdere voorbeeld verzenden:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Hierdoor wordt gegenereerd precies dezelfde serienummer gebeurtenis alsof we een **TemperatureEvent** hebt gedefinieerd met een lid van **temperatuur** en **tijd** , net zoals we hebben gedaan met model 1. In dit geval kon precies dezelfde serienummer gebeurtenis genereren met behulp van een ander model (model 2), omdat we **GESERIALISEERD** op een andere manier aangeroepen.

Het belangrijkste is dat als u meerdere Gegevensgebeurtenissen aan **GESERIALISEERD, doorgeven** en vervolgens hierbij wordt ervan uitgegaan elke gebeurtenis een eigenschap in een enkel JSON-object.

De beste manier is afhankelijk van u en hoe u uw model nadenken. Als u 'gebeurtenissen' naar de cloud verzendt en elke gebeurtenis een gedefinieerde set met eigenschappen bevat, klikt u vervolgens kunt de eerste methode u een groot aantal komen. In dat geval zou u gebruiken **DECLARE\_structuur** definieert de structuur van elke gebeurtenis en deze vervolgens opnemen in uw model met de **door\_gegevens** macro. Klik verzenden u elke gebeurtenis als we hebben gedaan in het eerste voorbeelddiagram. In deze benadering zou u alleen een eenmalige gebeurtenis doorgeven aan **SERIALISATIEFUNCTIE**.

Als u het model op een wijze object-georiënteerd nadenken, klikt u vervolgens de tweede aanpak mogelijk structureren zoals u. In dit geval de elementen gedefinieerd met behulp van **door\_gegevens** zijn de "eigenschappen" van het object. U geeft ongeacht subset van gebeurtenissen aan **GESERIALISEERD** die u tevreden bent, afhankelijk van hoeveel van uw 'van object"staat die u wilt verzenden naar de cloud.

Nether benadering is gegaan of naar rechts. Alleen Houd rekening met de werking van de bibliotheek **serialisatiefunctie** en kies de methode modellen die het beste aan uw wensen voldoet.

## <a name="message-handling"></a>Afhandeling van berichten

Dusverre in dit artikel is alleen verzenden gebeurtenissen op IoT Hub besproken en ontvangen van berichten niet is gericht. De reden hiervoor is dat wat moeten we weten over het ontvangen van berichten is grotendeels voldaan in een [eerdere artikel](iot-hub-device-sdk-c-intro.md). Intrekken van dit artikel u berichten door te registreren van een bericht terugbellen functie verwerkt:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

U vervolgens schrijven de terugbellen-functie die wordt aangeroepen wanneer een bericht is ontvangen:

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

Deze implementatie van **IoTHubMessage** roept de specifieke functie voor elke actie in uw model. Stel dat uw model Hiermee definieert u deze actie:

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

**SetAirResistance** wordt vervolgens aangeroepen wanneer dit bericht wordt verzonden naar uw apparaat.

Wat we nog niet hebt uitgelegd is hoe de serienummer versie van bericht eruitziet nog. Met andere woorden, als u een **SetAirResistance** bericht verzenden naar uw apparaat wilt, wat ziet die eruit?

Als u een bericht naar een apparaat verzendt, wilt u dat doen door de service Azure IoT SDK. Nog steeds moet u weten welke tekenreeks te verzenden naar een bepaalde actie oproepen. De notatie Standaard voor het verzenden van een bericht ziet er als volgt uit:

```
{"Name" : "", "Parameters" : "" }
```

U verzendt een serienummer JSON-object met twee eigenschappen: **is de naam van de actie (bericht)** en **Parameters** bevat de parameters van deze actie.

U kunt bijvoorbeeld om aan te roepen **SetAirResistance** dit bericht verzenden naar een apparaat:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

De actienaam van de moet precies overeenkomen met een actie die is gedefinieerd in het model. De parameternamen moeten ook overeenkomen. Bedenk ook hoofdlettergevoeligheid. **Naam** en **Parameters** zijn altijd hoofdletters. Zorg ervoor dat overeenkomen met de hoofdletters/kleine letters van de naam van de actie en parameters in uw model. In dit voorbeeld is de actienaam van de "SetAirResistance" en niet "setairresistance".

In deze sectie beschreven alles wat die u moet weten wanneer gebeurtenissen verzenden en ontvangen berichten met de bibliotheek **serialisatiefunctie** . Voordat u verdergaat, moet u laten we eens enkele parameters die u kunt configureren en waarmee wordt bepaald hoe groot is in uw model duidelijk.

## <a name="macro-configuration"></a>Macro-configuratie

Als u gebruikmaakt van de bibliotheek **serialisatiefunctie** is een belangrijk onderdeel van de SDK letten gevonden in de bibliotheek azure-c-gedeeld-hulpprogramma.
Als u de opslagplaats Azure-iot-SDK's van GitHub met de optie--recursieve hebt gekopieerd, vindt u hier deze gedeelde hulpprogrammabibliotheek:

```
.\\c\\azure-c-shared-utility
```

Als u de bibliotheek niet hebt gekopieerd, kunt u vinden het [hier](https://github.com/Azure/azure-c-shared-utility).

De bibliotheek gedeelde hulpprogramma vindt u de volgende map:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Deze map bevat een Visual Studio-oplossing genoemd **macro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Gegenereerd door het programma in deze oplossing de **macro\_utils.h** bestand. Er is een standaardmacro\_utils.h-bestand wordt geleverd bij de SDK. Deze oplossing kunt u sommige parameters wijzigen en vervolgens opnieuw maken het koptekst-bestand op basis van deze parameters.

De twee belangrijkste parameters worden weerspiegeld zijn **nArithmetic** en **nMacroParameters** die zijn gedefinieerd in de volgende twee regels in macro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Deze waarden zijn de standaardparameters wordt geleverd bij de SDK. Elke parameter heeft de volgende zin:

-   nMacroParameters – bepaalt hoe veel parameters kunt u beschikken over in één DECLARE\_MODEL macro definiëren.

-   nArithmetic – Hiermee stelt u het totale aantal leden die zijn toegestaan in een model.

De reden dat deze parameters belangrijk zijn is omdat ze bepalen hoe groot uw model kan zijn. Stel de modeldefinitie van dit:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Zoals eerder is vermeld, **DECLARE\_MODEL** is alleen een macro C. De namen van het model en de **door\_gegevens** instructie (nog een nieuwe macro) zijn parameters van **DECLARE\_MODEL**. **nMacroParameters** wordt gedefinieerd hoe veel parameters kunnen worden opgenomen in **DECLARE\_MODEL**. Hiermee definieert effectief, hoeveel gegevens gebeurtenis en actie declaraties kunt u beschikken over. Als zodanig met de standaardbeperking van 124 betekent dit dat u een model met een combinatie van ongeveer 60 acties en Gegevensgebeurtenissen die kunt definiëren. Als u deze limiet overschrijdt wilt, ontvangt u compileerprogramma fouten die er ongeveer als volgt uitzien:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

De parameter **nArithmetic** is meer over de interne werking van de macrotaal dan uw toepassing.  Hiermee kunt u het totale aantal leden die u in het model, inclusief **DECLARE_STRUCT** macro's kunt hebben. Als u compileerprogramma fouten zoals dit zien begint, te klikt u vervolgens proberen **nArithmetic**vergroten:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Als u wilt deze instellingen wilt wijzigen, wijzigt u de waarden in de macro\_utils.tt bestand, de macro compileren\_utils\_h\_generator.sln oplossing en het gecompileerd programma start. Als u dit doet, een nieuwe macro\_utils.h bestand is gegenereerd en worden in de. \\algemene\\Inc. directory.

Pas de nieuwe versie van macro's gebruiken\_utils.h, het **serialisatiefunctie** NuGet pakket verwijderen uit uw oplossing en opnemen in plaats daarvan de **serialisatiefunctie** Visual Studio-project. Hiermee worden uw code compileren ten opzichte van de broncode van de bibliotheek serialisatiefunctie. Dit geldt ook voor de bijgewerkte macro\_utils.h. Als u deze stap herhalen wilt voor **simplesample\_amqp**, begint u met het pakket NuGet voor de bibliotheek serialisatiefunctie verwijderen uit de oplossing:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Voeg vervolgens dit project toe aan uw Visual Studio-oplossing:

> . \\c\\serialisatiefunctie\\bouwen\\windows\\serializer.vcxproj

Wanneer u klaar bent, moet uw oplossing er zo uit:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nu wanneer u uw oplossing, de bijgewerkte macro compileren\_utils.h is opgenomen in uw binair getal.

Houd er rekening mee dat deze waarden hoog genoeg met groter wordende compileerprogramma overschrijden kunt. Tot dat moment weer is de **nMacroParameters** de belangrijkste parameter waarmee u betrokken. De specificatie C99 geeft aan dat een minimum aan 127 parameters zijn toegestaan in de macrodefinitie van een. De Microsoft-compileerprogramma volgt de specificatie exact (en geldt een limiet van 127), zodat het niet mogelijk om uit te breiden **nMacroParameters** voorbij de standaardwaarde. Andere compileerprogramma's mogelijk kunt u dat doen (bijvoorbeeld het GNU-compileerprogramma ondersteunt voor een hogere limiet).

We hebben dusverre bijna alles wat die u moet weten over het schrijven van code met de bibliotheek **serialisatiefunctie** besproken. Vóór de sluiting, bezoekt u laten we eens enkele onderwerpen uit de vorige artikelen die u mogelijk worden afvragen.

## <a name="the-lower-level-apis"></a>De lagere niveaus API 's

De voorbeeldtoepassing waarop in dit artikel is gericht is **simplesample\_amqp**. In dit voorbeeld gebruikt het op een hoger niveau (de niet-"l") API's gebeurtenissen verzenden en ontvangen van berichten. Als u deze API's gebruikt, thread op de achtergrond wordt uitgevoerd waarin zorgt voor zowel gebeurtenissen berichten verzenden en ontvangen. U kunt echter de lagere niveaus (l)-API's gebruiken om te voorkomen deze achtergrondthread en besturen expliciete wanneer u gebeurtenissen berichten verzenden of ontvangen vanuit de cloud.

Zoals beschreven in een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md), moet u er een aantal functies die uit de een hoger niveau API's bestaat is:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

Deze API's worden weergegeven in **simplesample\_amqp**.

Er is ook een vergelijkbaar reeks lagere niveaus API's.

-   IoTHubClient\_l\_CreateFromConnectionString

-   IoTHubClient\_l\_SendEventAsync

-   IoTHubClient\_l\_SetMessageCallback

-   IoTHubClient\_l\_Destroy

Houd er rekening mee dat de lagere niveaus API's werkt op precies dezelfde manier zoals is beschreven in de vorige artikelen. Als u een achtergrondthread worden afgehandeld gebeurtenissen verzenden en ontvangen berichten wilt, kunt u de eerste reeks API's gebruiken. U kunt de tweede reeks API's gebruiken als u expliciete controle wilt over wanneer u gegevens verzenden en ontvangen van IoT Hub. Een reeks API's werk even goed met de bibliotheek **serialisatiefunctie** .

Zie voor een voorbeeld van hoe de lagere niveaus API's worden gebruikt met de bibliotheek **serialisatiefunctie** , de **simplesample\_http** toepassing.

## <a name="additional-topics"></a>Aanvullende onderwerpen

Een paar andere onderwerpen vermelden we zijn opnieuw eigendom afhandelen, alternatieve apparaat referenties en configuratieopties gebruiken. Hierna ziet u alle onderwerpen in een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md). De belangrijkste is dat al deze functies op dezelfde manier met de bibliotheek **serialisatiefunctie werken** als met de bibliotheek **IoTHubClient** . Bijvoorbeeld als u eigenschappen koppelen aan een gebeurtenis in uw model wilt, u **IoTHubMessage\_eigenschappen** en **Map**\_**AddorUpdate**, dezelfde manier zoals eerder is beschreven:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Of de gebeurtenis is gegenereerd op basis van de bibliotheek **serialisatiefunctie** of gemaakt handmatig met behulp van de bibliotheek **IoTHubClient** maakt niet uit.

Voor de referenties van alternatieve apparaat, met **IoTHubClient\_l\_maken** net zo goed werkt als **IoTHubClient\_CreateFromConnectionString** voor het toewijzen van een **IOTHUB\_CLIENT\_verwerken**.

Als u de bibliotheek **serialisatiefunctie** gebruikt, kunt u tot slot configuratieopties met instellen **IoTHubClient\_l\_SetOption** net zoals bij gebruik van de bibliotheek **IoTHubClient** .

Een functie die uniek is voor de bibliotheek **serialisatiefunctie** zijn de initialisatie API's. Voordat u kunt werken met de bibliotheek, moet u belt **serialisatiefunctie\_initialisatie**:

```
serializer_init(NULL);
```

Dit gebeurt alleen voordat u belt **IoTHubClient\_CreateFromConnectionString**.

Op dezelfde manier als u klaar bent met de laatste oproep kunt u ervoor zorgen is om te werken met de bibliotheek, **serialisatiefunctie\_deinit**:

```
serializer_deinit();
```

Alle andere bovenstaande functies werken anders in de bibliotheek **serialisatiefunctie** hetzelfde als in de bibliotheek **IoTHubClient** . Zie voor meer informatie over deze onderwerpen, het [vorige artikel](iot-hub-device-sdk-c-iothubclient.md) in deze reeks.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven in detail de unieke aspecten van de bibliotheek **serialisatiefunctie** is opgenomen in het **Azure IoT apparaat SDK voor C**. Met de informatie die er is een goed inzicht in het gebruik van modellen gebeurtenissen verzenden en ontvangen van berichten van IoT Hub.

Hiermee wordt de reeks uit drie delen op het ontwikkelen van toepassingen bij het **apparaat van de Azure IoT SDK voor C**ook afgesloten. Dit moet weinig gegevens niet alleen u kunt beginnen, maar kunt u een goed begrip van de werking van de API's. Voor aanvullende informatie, zijn er enkele voorbeelden in de SDK die hier niet wordt beschreven. Anders is de [SDK-documentatie](https://github.com/Azure/azure-iot-sdks) een goede informatiebron voor meer informatie.


Meer informatie over het ontwikkelen van voor IoT-Hub, raadpleegt u de [IoT Hub SDK's][lnk-sdks].

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
