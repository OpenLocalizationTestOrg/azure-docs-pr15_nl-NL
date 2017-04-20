<properties
   pageTitle="Verbinding maken met een apparaat met C op mbed | Microsoft Azure"
   description="Wordt beschreven hoe u verbinding maakt u een apparaat met de Azure IoT Suite vooraf geconfigureerde externe controle oplossing met gebruikmaking van een toepassing in C uitgevoerd op een apparaat mbed geschreven."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Maken en uitvoeren van de oplossing van de steekproef C

De volgende instructies beschrijven de stappen voor het verbinden van een [Mbed ingeschakelde Freescale FRDM-K64F] [ lnk-mbed-home] apparaat met de externe controle oplossing.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Het apparaat mbed verbinden met uw netwerk- en bureaublad-computer

1. Het apparaat mbed verbinden met uw netwerk door gebruik van een Ethernet-kabel. Deze stap is nodig omdat de toepassing van de steekproef is internettoegang vereist.

2. Zie [Aan de slag met mbed] [ lnk-mbed-getstarted] naar mbed aansluit op uw desktop-PC.

3. Als uw desktop-PC wordt uitgevoerd van Windows, Zie [Configuratie van de PC] [ lnk-mbed-pcconnect] seriële poort toegang tot uw apparaat mbed configureren.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Een project mbed maken en importeren van de steekproef-code

1. Ga in uw webbrowser naar de mbed.org [ontwikkelaarssite](https://developer.mbed.org/). Als u dit nog niet hebt geregistreerd, ziet u een optie voor het maken van een account (dit is gratis). Anders Meld u aan met referenties van uw account. Klik vervolgens op **compileerprogramma** in de rechterbovenhoek van de pagina. Deze actie keert u terug naar de *werkruimte* -interface.

2. Zorg ervoor dat het hardwareplatform dat u gebruikt weergegeven in de rechterbovenhoek van het venster of klikt u op het pictogram in de rechter bovenhoek uw hardwareplatform selecteren.

3. Klik op **importeren** in het hoofdmenu. Klik vervolgens op **Klik hier** om uit URL-koppeling naast het logo van de wereldbol mbed te importeren.

    ![][6]

4. In het pop-upvenster, voert u de koppeling voor de steekproef code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ Klik op **importeren**.

    ![][7]

5. U kunt zien in het venster van mbed compileerprogramma dat dit project importeren ook verschillende bibliotheken invoer. Enkele worden verstrekt en beheerd door het team Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), terwijl andere bibliotheken van derden beschikbaar in de catalogus met mbed bibliotheken.

    ![][8]

6. Open het bestand remote_monitoring\remote_monitoring.c en zoek de volgende code in het bestand:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Vervangen [apparaat-Id] en [apparaat Key], met de apparaatgegevens van uw de steekproef programma kunt verbinden met uw hub IoT inschakelen. De hostnaam van de Hub IoT gebruiken voor het vervangen van [naam van IoTHub] en [IoTHub achtervoegsel, dat wil zeggen azure-devices.net] tijdelijke aanduidingen. Bijvoorbeeld: als uw IoT Hub Hostname **contoso.azure-devices.net is**, **contoso** is het **hubName** en alles nadat deze de **hubSuffix is**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Doorloop de code

Als u geïnteresseerd bent in de werking van het programma, wordt in dit gedeelte worden enkele belangrijke punten in de steekproef-code beschreven. Desgewenst kunt u alleen de code uit te voeren, gaat u verder wilt [maken en uitvoeren van het programma](#buildandrun).

#### <a name="defining-the-model"></a>Het model definiëren

In dit voorbeeld worden de [serialisatiefunctie] [ lnk-serializer] -bibliotheek met het definiëren van een model dat Hiermee geeft u de berichten die het apparaat kunt IoT Hub verzenden en ontvangen van IoT Hub. In dit voorbeeld wordt de **Contoso** -naamruimte gedefinieerd een **thermostaat** model waarmee de **temperatuur**, **ExternalTemperature**en **vochtigheid** telemetriegegevens samen met metagegevens, zoals de apparaat-id, apparaateigenschappen en de opdrachten die het apparaat moet reageren op:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Gerelateerd aan de modeldefinitie zijn de definities voor de opdrachten **SetTemperature** en **SetHumidity** die het apparaat reageert op:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Het model verbinden met de bibliotheek

De functies **sendMessage** en **IoTHubMessage** zijn standaardtekst code voor het verzenden van telemetrielogboek van het apparaat en berichten vanuit IoT Hub met de opdracht handlers verbinden.

#### <a name="the-remotemonitoringrun-function"></a>De functie remote_monitoring_run

De **belangrijkste** functie roept de functie **remote_monitoring_run** wanneer de toepassing wordt gestart gedrag van het apparaat als IoT Hub apparaat client uitvoeren. Deze functie **remote_monitoring_run** bevat voornamelijk geneste paren van functies:

- **Platform\_initialisatie** en **platform\_deinit** platform / regiospecifieke initialisatie en afsluiten bewerkingen uitvoeren.
- **serialisatiefunctie\_initialisatie** en **serialisatiefunctie\_deinit** geïnitialiseerd en maak geïnitialiseerd de serialisatiefunctie-bibliotheek.
- **IoTHubClient\_maken** en **IoTHubClient\_Destroy** client grip, **iotHubClientHandle**, met de referenties van het apparaat om verbinding te maken met uw hub IoT maken.

In het hoofdgedeelte van de functie **remote_monitoring_run** wordt de volgende bewerkingen met de greep **iotHubClientHandle** :

- Hiermee maakt u een exemplaar van het model van de thermostaat Contoso en de terugbellen bericht voor de twee opdrachten ingesteld.
- Informatie over het apparaat zelf, inclusief de opdrachten ondersteund, op uw IoT hub met de bibliotheek serialisatiefunctie verzendt. Wanneer de hub dit bericht ontvangt, verandert de status van het apparaat in het dashboard van **in behandeling** **uitgevoerd**.
- Hiermee start u een lus **terwijl** die temperatuur, externe temperatuur en vochtigheid waarden naar IoT Hub elke tweede.

Voor verwijzing, als volgt een voorbeeld **DeviceInfo** bericht verzonden naar IoT Hub bij het opstarten:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Hier volgt een steekproef **Telemetrielogboek** bericht verzonden naar IoT Hub voor een verwijzing naar:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Hier volgt een steekproef **die opdracht** van IoT Hub ontvangen voor een verwijzing naar:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Maken en uitvoeren van het programma

1. Klik op **compileren** om te maken van het programma. U kunt veilig eventuele waarschuwingen negeren, maar als het bouwen fouten genereert, problemen oplost voordat u verdergaat.

2. Als het bouwen gelukt is, wordt de website van mbed compileerprogramma genereert een BIN-bestand met de naam van uw project en ze gedownload naar uw lokale computer. Kopieer de BIN-bestand naar het apparaat. De BIN-bestand opslaan op het apparaat zorgt ervoor dat het apparaat opnieuw opstarten en het programma in de BIN-bestand uit te voeren. U kunt het programma handmatig op elk gewenst moment opnieuw door op de knop herstellen op het apparaat mbed te drukken.

3. Verbinding maken met het apparaat dat een SSH client-toepassing, zoals stopverf gebruikt. Hier ziet u de seriële poort die uw apparaat wordt gebruikt door te schakelen Apparaatbeheer van Windows.

    ![][11]

4. In stopverf, klikt u op de **seriële** verbindingstype. Het apparaat is meestal verbinding maakt met 9600 baud, dus voer 9600 in het vak **snelheid** . Klik vervolgens op **openen**.

5. Het programma wordt uitgevoerd. U moet mogelijk opnieuw instellen van het bord (druk op CTRL + Break of drukt u op de knop herstellen van het bord) als het programma niet automatisch wordt gestart wanneer u verbinding maakt.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
