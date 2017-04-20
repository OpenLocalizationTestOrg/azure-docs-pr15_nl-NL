<properties
    pageTitle="Een reële apparaat gebruiken met de SDK van de Gateway IoT | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario een Texas instrumenten SensorTag-apparaat met gegevens te verzenden naar IoT Hub via een gateway die wordt uitgevoerd op een Intel-Module voor het berekenen van Edison"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT Gateway SDK (beta) – apparaat naar cloud-berichten verzenden met een reële apparaat gebruik Linux

Deze stapsgewijze instructies voor de [Bluetooth lage energie steekproef] [ lnk-ble-samplecode] ziet u hoe u het gebruik van de [Azure IoT Gateway SDK] [ lnk-sdk] naar doorsturen apparaat naar cloud-telemetrielogboek op IoT Hub uit een fysiek apparaat en hoe u opdrachten uit IoT Hub naar een fysiek apparaat.

Dit scenario omvat:

* **Architectuur**: belangrijke architectuur informatie over het voorbeeld van de laag energie Bluetooth.

* **Maken en uitvoeren**: de benodigde stappen voor het maken en uitvoeren van de steekproef.

## <a name="architecture"></a>Architectuur

De rondleiding ziet u hoe u bouwen en uitvoeren van een IoT Gateway op een Intel Edison berekenen Module die wordt uitgevoerd Linux. De gateway is gemaakt met de SDK van de Gateway IoT. In het voorbeeld wordt een apparaat Texas instrumenten SensorTag Bluetooth laag energie (bel) gebruikt voor het verzamelen van temperatuurgegevens.

Bij het uitvoeren van de gateway deze:

- Maakt verbinding met een SensorTag-apparaat met het protocol Bluetooth laag energie (bel).
- Maakt verbinding met IoT Hub via het HTTP-protocol.
- Stuurt telemetrielogboek van het apparaat SensorTag op IoT Hub.
- Opdrachten uit IoT Hub bij het apparaat SensorTag stuurt.

De gateway bevat de volgende modules:

- Een *module Bel* die interfaces met een apparaat Bel temperatuurgegevens ontvangen van de opdrachten apparaat en verzenden naar het apparaat.
- Een *Bel wolk apparaat module* dat de JSON-berichten die afkomstig zijn uit de cloud in Bel-instructies voor de *Bel module*equivalent.
- Een *logboek module* die alle berichten van de gateway registreert.
- Een *Identiteitstoewijzing module* die tussen Bel apparaat MAC-adressen en Azure IoT Hub apparaat identiteiten equivalent.
- Een *module IoT Hub* die telemetriegegevens uploadt naar een hub IoT en apparaatopdrachten vanuit een hub IoT ontvangt.
- Een *Bel printer module* dat telemetrielogboek van het apparaat Bel interpreteert en afgedrukt opgemaakte gegevens aan de console om oplossen en voor foutopsporing in te schakelen.

### <a name="how-data-flows-through-the-gateway"></a>Hoe gegevens via de Gateway loopt

De volgende blokdiagram ziet u de telemetrielogboek upload gegevensstroom pijplijn:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

De stappen waarmee een item van telemetrielogboek reizen vanaf een apparaat Bel op IoT Hub zijn:

1. Het apparaat Bel genereert een steekproef temperatuur en verzonden via Bluetooth naar de module Bel in de gateway.
2. De module Bel ontvangt van de steekproef en deze worden gepubliceerd naar de makelaar samen met het MAC-adres van het apparaat.
3. De identiteit toewijzing module dit bericht wordt opgehaald en gebruikmaakt van een interne tabel voor de MAC-adres van het apparaat vertalen in een IoT Hub apparaat identiteit (een apparaat-ID en de apparaatsleutel). Vervolgens wordt een nieuw bericht met de voorbeeldgegevens temperatuur, het MAC-adres van het apparaat, de apparaat-ID en de toets apparaat gepubliceerd.
4. De module IoT Hub ontvangt deze nieuw bericht (gegenereerd door de identiteit toewijzing module) en deze worden gepubliceerd naar IoT Hub.
5. De module logboek registreert alle berichten van de makelaar naar een diskette.

De volgende blokdiagram ziet u het apparaat opdracht gegevensstroom verkooppijplijn:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. De module IoT Hub controleert regelmatig de hub IoT voor nieuwe berichten van de opdracht.
2. Wanneer de module IoT Hub ontvangt een nieuw opdrachtbericht, worden deze in de makelaar gepubliceerd.
3. De identiteit toewijzing module opgehaald bericht en een interne tabel gebruikt om te vertalen van de IoT Hub apparaat-ID naar een apparaat MAC-adres. Vervolgens wordt een nieuw bericht met het MAC-adres van het doelapparaat in de map eigenschappen van het bericht gepubliceerd.
4. De bel wolk apparaat module dit bericht wordt opgehaald en omgezet in de juiste Bel-instructie voor de bel-module. Vervolgens wordt een nieuw bericht gepubliceerd.
5. De module Bel dit bericht wordt opgehaald en het i/o-instructie worden uitgevoerd door te communiceren met de bel-apparaat.
6. De module logboek registreert alle berichten van de makelaar naar een diskette.

## <a name="prepare-your-hardware"></a>De hardware voorbereiden

Deze zelfstudie wordt ervan uitgegaan dat u gebruikt een [Texas instrumenten SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) apparaat dat is verbonden aan een bord Intel Edison.

### <a name="set-up-the-edison-board"></a>Het bord Edison instellen

Voordat u begint, moet u ervoor zorgen dat u uw apparaat Edison verbinding met uw draadloos netwerk maken kunt. Als u wilt instellen van uw apparaat Edison, moet u deze verbinden met een hostcomputer. Intel biedt aan de slag-handleidingen voor de volgende besturingssystemen:

- [Aan de slag met het bord Intel Edison ontwikkeling in Windows 64-bits][lnk-setup-win64].
- [Aan de slag met het bord Intel Edison ontwikkeling op Windows 32-bits][lnk-setup-win32].
- [Aan de slag met het bord Intel Edison ontwikkeling in Mac OS X][lnk-setup-osx].
- [Aan de slag met het bord Intel® Edison op Linux][lnk-setup-linux].

Als u uw apparaat Edison instellen en maak uzelf vertrouwd met deze, moet u alle voltooid de stappen in deze "aan de slag' artikelen, behalve de laatste stap, 'Kies IDE', die voor de huidige zelfstudie is niet nodig. U hebt aan het einde van de installatieprocedure voor Edison:

- Geknipperd uw Edison met de meest recente firmware.
- Seriële verbinding van uw host naar de Edison gemaakt.
- De **configure_edison** script uitvoeren om een wachtwoord instellen en inschakelen van WiFi op uw Edison.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Connectiviteit bij het apparaat SensorTag van uw bord Edison inschakelen

Voordat u het voorbeeld, moet u controleren of dat uw bord Edison verbinding met het apparaat SensorTag maken kunt.

U moet eerst om te bevestigen dat uw Edison verbinding met de SensorTag-apparaat maken kunt.

1. Blokkering opheffen bluetooth op de Edison en controleer of het versienummer **5.37**is.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. De opdracht **bluetoothctl** uitgevoerd. U bent nu in een interactieve bluetooth-shell. 

3. Hiermee voert u de opdracht **power op** tot de macht van de controller bluetooth. Hier ziet u de uitvoer is vergelijkbaar met:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Terwijl u nog steeds in de interactieve bluetooth-shell, voert u de opdracht **scannen op** om te zoeken naar Bluetooth-apparaten. Hier ziet u de uitvoer is vergelijkbaar met:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Het apparaat SensorTag detecteerbaar maken door op de kleine knop (de groene LED moet flash). De Edison moet kennismaken met het apparaat SensorTag:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    In dit voorbeeld ziet u dat het MAC-adres van het apparaat SensorTag **A0:E6:F8:B5:F6:00**is.

6. Uitschakelen scannen door de opdracht **scannen uit** te voeren.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Verbinding maken met uw SensorTag-apparaat met het MAC-adres door in te voeren **verbinding \<MAC-adres >**. Houd er rekening mee dat het onderstaande voorbeelduitvoer is afgekort:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Opmerking: U kunt een lijst met de kenmerken GATT van het apparaat opnieuw met de opdracht **lijst-kenmerken** .

8. U kunt nu loskoppelen van het apparaat met de opdracht **verbinding verbreken** en sluit uit de bluetooth-shell met de opdracht **Afsluiten** :
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

U bent nu klaar om uit te voeren van de steekproef Bel Gateway op uw apparaat Edison.

## <a name="run-the-ble-gateway-sample"></a>De steekproef Bel Gateway worden uitgevoerd

Als u wilt uitvoeren in het voorbeeld Bel op uw Edison, moet u drie taken uitvoeren:

- Twee steekproeven apparaten in de IoT Hub configureren.
- De SDK van de Gateway IoT op uw apparaat Edison maken.
- Configureren en uitvoeren van de steekproef Bel op uw apparaat Edison.

Op het moment van schrijven ondersteunt de SDK van de Gateway IoT alleen gateways dat Bel modules op Linux gebruiken.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Twee steekproeven apparaten in de IoT Hub configureren

- [Maken van een hub IoT] [ lnk-create-hub] in uw Azure-abonnement, moet u de naam van uw hub om te voltooien van deze procedure. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- Voeg één apparaat **SensorTag_01** op uw hub IoT genoemd en noteer de sleutel-id en apparaat. U kunt het [apparaat Explorer of iothub-explorer] [ lnk-explorer-tools] hulpmiddelen voor dit apparaat toevoegen aan de IoT hub die u in de vorige stap hebt gemaakt en de sleutel terughalen. U kunt dit apparaat worden toegewezen aan het apparaat SensorTag bij het configureren van de gateway.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>De SDK van de Gateway IoT op uw apparaat Edison maken

De versie van **cijfer** op het Edsion biedt geen ondersteuning voor submodules. Als u wilt downloaden van de volledige bron voor de SDK van de Gateway IoT naar de Edison hebt u twee opties:

- Optie #1: De [Azure IoT Gateway SDK] klonen[ lnk-sdk] bibliotheek op uw Edison en klikt u vervolgens de opslagplaats voor elke submodule handmatig te klonen.
- Optie #2: De [Azure IoT Gateway SDK] klonen[ lnk-sdk] bibliotheek op een desktop waar **cijfer** submodules ondersteunt en kopieer de volledige opslagplaats met submodules naar uw Edison.

Als u de optie #2 kiest, moet u de volgende **cijfer** -opdrachten gebruiken om te klonen de SDK van de Gateway IoT en alle bijbehorende submodules:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

U moet de hele lokale opslagplaats vervolgens zip in een enkel archiefbestand voordat u deze naar de Edison kopiëren. U kunt een hulpprogramma zoals **pscp** die deel uitmaakt van **stopverf** naar het bestand kopiëren naar de Edison gebruiken. Bijvoorbeeld:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Wanneer u een volledig exemplaar van de bibliotheek IoT Gateway SDK op uw Edison hebt, kunt u deze met de volgende opdracht uit de map waarin de SDK kunt maken:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Configureren en uitvoeren van de steekproef Bel op uw apparaat Edison

Als u wilt bootstrap en uitvoeren van de steekproef, moet u elke module die deel uitmaakt configureren in de gateway. Deze configuratie is opgegeven in een bestand JSON en moet u alle vijf deelnemende modules configureren. Er is een voorbeeldbestand van JSON die beschikbaar zijn in de bibliotheek **gateway_sample.json** die u als uitgangspunt gebruiken kunt voor het samenstellen van uw eigen configuratiebestand genoemd. Dit bestand is opgeslagen in de map **voorbeelden/ble_gateway_hl/src** in lokale kopie van de bibliotheek IoT Gateway SDK.

De volgende gedeelten wordt beschreven hoe u dit configuratiebestand voor de steekproef Bel bewerken en wordt ervan uitgegaan dat de opslagplaats IoT Gateway SDK in de **/home/root/azure-iot-gateway-sdk /** map op uw apparaat Edison. Als de opslagplaats elders is, moet u de paden overeenkomstig aanpassen:

#### <a name="logger-configuration"></a>Configuratie van het logboek

Ervan uitgaande dat de gateway-bibliotheek bevindt zich in de map **/home/root/azure-iot-gateway-sdk /**, de module logboek als volgt configureren:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Bel module configuratie

De configuratie van de steekproef voor de bel-apparaat wordt ervan uitgegaan een Texas instrumenten SensorTag-apparaat. Elk standaard Bel-apparaat, dat werken kan, zoals een externe GATT moet werken, maar u moet de kenmerkende GATT-id's en gegevens (voor schrijven-instructies) bijwerken. Het MAC-adres van uw apparaat SensorTag toevoegen: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT Hub-module

De naam van uw IoT Hub toevoegen. De Achtervoegselwaarde is meestal **azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identiteit toewijzing module configuratie

De MAC-adres van uw SensorTag en het apparaat-Id en de toets van het **SensorTag_01** apparaat dat u hebt toegevoegd aan uw IoT Hub toevoegen:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>De configuratie van de bel Printer-module

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Configuratie van de routering

De volgende configuratie zorgt ervoor dat het volgende:
- De module **logboek** ontvangt en meld u aan alle berichten.
- De module **SensorTag** verzendt berichten naar de **toewijzing** en de **Bel Printer** modules.
- De module **toewijzing** worden berichten verzonden naar de module **IoTHub** omhoog naar uw IoT Hub wordt verzonden.
- De module **IoTHub** stuurt berichten terug naar de module **toewijzing** .
- De module **toewijzing** stuurt berichten terug naar de module **SensorTag** .

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Als u wilt uitvoeren in het voorbeeld kunt u de binaire doorgeven van het pad naar het configuratiebestand JSON **ble_gateway_hl** uitvoeren. Als u het bestand **gateway_sample.json** gebruikt, is de opdracht uitvoeren ziet er zo uit:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Mogelijk moet u drukt op de kleine knop op het apparaat SensorTag detecteerbaar voordat u de steekproef uitvoert.

Wanneer u de steekproef uitvoert, kunt u het [apparaat Explorer of iothub-explorer] [ lnk-explorer-tools] venster voor het controleren van de berichten die de gateway van het apparaat SensorTag doorstuurt.

## <a name="send-cloud-to-device-messages"></a>Cloud-to-device berichten verzenden

De module Bel ondersteunt ook verzendende instructies van de Hub van Azure IoT bij het apparaat. U kunt de [Azure IoT Hub apparaat Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) of de [IoT Hub Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) JSON-berichten die de bel gateway-module worden doorgegeven op het apparaat Bel te verzenden. Bijvoorbeeld als u het apparaat Texas instrumenten SensorTag gebruikt kunt u verzend de volgende JSON-berichten bij het apparaat van IoT Hub.

- Opnieuw alle LED's en jaar ouder (deze functie uitschakelen)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- I/O configureren als 'remote'

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- De rode LED inschakelen

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- De groene LED inschakelen

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Jaar ouder inschakelen

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Het standaardgedrag voor een apparaat met het HTTP-protocol verbinding maken met IoT Hub is om te controleren van elke 25 minuten voor een nieuwe opdracht. Als u meerdere afzonderlijke opdrachten verzendt moet u er daarom 25 minuten voor het apparaat voor het ontvangen van elke opdracht wachten.

> [AZURE.NOTE] De gateway ook Hiermee wordt gecontroleerd op nieuwe opdrachten als het wordt gestart, zodat u dat instellen kunt verwerkingstijd van een opdracht door de gateway te starten en stoppen.

## <a name="next-steps"></a>Volgende stappen

Als u wilt krijgen een geavanceerdere begrip van de SDK van de Gateway IoT en experimenteren met codevoorbeelden, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

- [Azure IoT Gateway SDK][lnk-sdk]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
