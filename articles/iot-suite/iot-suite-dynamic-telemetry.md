<properties
    pageTitle="Gebruik dynamische telemetrielogboek | Microsoft Azure"
    description="Volg deze zelfstudie om te leren werken met dynamische telemetrielogboek met de externe controle vooraf geconfigureerde oplossing."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynamische telemetrielogboek gebruiken met de externe controle vooraf geconfigureerde oplossing

## <a name="introduction"></a>Inleiding

Dynamische telemetrielogboek kunt u eventuele telemetrielogboek verzonden naar de externe controle vooraf geconfigureerde oplossing visualiseren. De gesimuleerd apparaten die u met de vooraf geconfigureerde oplossing implementeren verzenden temperatuur en vochtigheid telemetrielogboek, die u op het dashboard kunt visualiseren. Als u de bestaande gesimuleerd apparaten aanpassen, nieuwe gesimuleerd apparaten maken of fysieke apparaten verbinden met de vooraf geconfigureerde oplossing kunt u andere waarden telemetrielogboek zoals de externe temperatuur, RPM of windsnelheid verzenden. U kunt vervolgens deze aanvullende telemetrielogboek op het dashboard visualiseren.

Deze zelfstudie gebruikt een eenvoudige gesimuleerd apparaat voor Node.js, dat u eenvoudig aanpassen kunt om te experimenteren met dynamische telemetrielogboek.

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

- Een actieve Azure-abonnement. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie voor meer informatie, [Gratis proefversie van Azure][lnk_free_trial].
- [Node.js] [ lnk-node] versie 0.12.x of hoger.

U kunt deze zelfstudie op de besturingssystemen, zoals Windows of Linux, waar u Node.js kunt installeren voltooien.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>De Node.js gesimuleerd apparaat configureren

1. Op het dashboard van het externe controleren, klikt u op **+ een apparaat toevoegen** en voegt u een aangepast apparaat. Noteer de IoT Hub hostname, apparaat-id en de apparaatsleutel. Moet u deze later in deze zelfstudie wanneer u de remote_monitoring.js apparaat-clienttoepassing voorbereidt.

2. Zorg ervoor dat Node.js versie 0.12.x of hoger is geïnstalleerd op uw computer ontwikkeling. Uitvoeren `node --version` opdrachtprompt of in een shell om te controleren welke versie. Zie voor informatie over het gebruik van de manager van een pakket Node.js installeren op Linux [Node.js installeren via pakket manager][node-linux].

3. Wanneer u Node.js hebt geïnstalleerd, de nieuwste versie van de [azure-iot-SDK's] klonen[ lnk-github-repo] opslagplaats naar uw computer ontwikkeling. Gebruik altijd de **basispagina** tak voor de meest recente versie van de bibliotheken en de voorbeelden.

4. Vanuit uw lokale kopie van de [azure-iot-SDK's] [ lnk-github-repo] bibliotheek, de volgende twee bestanden kopiëren uit de map knooppunt/apparaat/voorbeelden naar een lege map op uw computer ontwikkeling:

  - Packages.JSON
  - remote_monitoring.js

5. Open het bestand remote_monitoring.js en zoek naar de volgende variabele definitie:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. **[Apparaat-verbindingsreeks IoT Hub]** vervangen door de verbindingsreeks van uw apparaat. Gebruik de waarden voor uw IoT Hub hostname, apparaat-id en apparaatsleutel die u een genoteerd in stap 1 hebt. Een verbindingsreeks apparaat heeft de volgende indeling:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Als uw hostname IoT Hub **contoso is** en uw apparaat-id **mydevice is**, is de verbindingsreeks ziet er als volgt uit:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Sla het bestand. Voer de volgende opdrachten in een shell of opdrachtprompt in de map waarin deze bestanden om de nodige pakketten installeren en vervolgens de toepassing van de steekproef uitvoeren:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Dynamische telemetrielogboek in actie zien

Het dashboard ziet u de temperatuur en vochtigheid telemetrielogboek vanuit de bestaande gesimuleerd apparaten:

![De standaarddashboard][image1]

Als u het Node.js gesimuleerd apparaat dat u in de vorige sectie hebt uitgevoerd selecteert, ziet u de temperatuur, vochtigheid en externe temperatuur telemetrielogboek:

![Externe temperatuur toevoegen aan het dashboard][image2]

De externe controle oplossing automatisch aanvullende externe temperatuur telemetrielogboek type wordt aangetroffen en toegevoegd aan de grafiek op het dashboard.

## <a name="add-a-telemetry-type"></a>Een type telemetrielogboek toevoegen

De volgende stap is het telemetrielogboek gegenereerd door het apparaat dat Node.js gesimuleerd met een nieuwe set met waarden vervangen:

1. Stoppen met het Node.js gesimuleerd apparaat door te typen **Ctrl + C** in uw opdrachtprompt of shell.

2. In het bestand remote_monitoring.js ziet u de basisgegevens waarden voor de bestaande temperatuur, vochtigheid en externe temperatuur telemetrielogboek. Een waarde basisgegevens voor **rpm** bijvoegen:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Het Node.js gesimuleerd apparaat gebruikt de functie **generateRandomIncrement** in het bestand remote_monitoring.js een willekeurig verhoging toevoegen aan de waarden basisgegevens. De waarde **rpm** willekeurige door het toevoegen van een regel met code na de bestaande randomizations als volgt:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. De nieuwe rpm waarde toevoegen aan de JSON-nettolading die het apparaat wordt verzonden naar IoT Hub:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Voer het Node.js gesimuleerd apparaat met de volgende opdracht uit:

    ```
    node remote_monitoring.js
    ```

6. Houd rekening met het nieuwe RPM telemetrielogboek type die moet worden weergegeven op de grafiek in het dashboard:

![RPM toevoegen aan het dashboard][image3]

> [AZURE.NOTE] Mogelijk moet u uitschakelen en schakel op het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging meteen weer te geven.

## <a name="customize-the-dashboard-display"></a>De dashboardweergave aanpassen

Het bericht **Apparaat-Info** kunt metagegevens over de telemetrielogboek die het apparaat naar IoT Hub verzenden kunt opnemen. Deze metagegevens kunt opgeven welke telemetrielogboek die het apparaat wordt verzonden. Wijzig de waarde **deviceMetaData** in het bestand remote_monitoring.js voor het opnemen van de definitie van een **Telemetrielogboek** na het opstellen van **opdrachten** . Het volgende codefragment toont de definitie **opdrachten** (Zorg ervoor dat u om toe te voegen een `,` na de definitie **opdrachten** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] De externe controle oplossing gebruikt een overeenkomst niet hoofdlettergevoelig om de metagegevensdefinitie met gegevens in de stream telemetrielogboek vergelijken.

Een definitie **Telemetrielogboek** toevoegen, zoals wordt weergegeven in het voorgaande codefragment, wordt het gedrag van het dashboard niet gewijzigd. De metagegevens kunt echter ook een kenmerk **om** de weergave in het dashboard aanpassen. De definitie van de metagegevens **Telemetrielogboek** bijwerken zoals wordt weergegeven in het volgende fragment:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

De volgende schermafbeelding ziet u hoe de legenda van de grafiek op het dashboard in deze wijziging wordt gewijzigd:

![De legenda van de grafiek aanpassen][image4]

> [AZURE.NOTE] Mogelijk moet u uitschakelen en schakel op het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging meteen weer te geven.

## <a name="filter-the-telemetry-types"></a>De typen telemetrielogboek filteren

Standaard wordt de grafiek op het dashboard elke gegevensreeks in de stream telemetrielogboek. U kunt de metagegevens **Apparaat-Info** de weergave van specifieke telemetrielogboek typen in het diagram onderdrukken. 

Als u de grafiek alleen temperatuur en vochtigheid telemetrielogboek weergeven, **ExternalTemperature** uit weglaten de metagegevens van de **Telemetrielogboek** **Apparaat-Info** als volgt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

De **Diversen temperatuur** is niet meer wordt weergegeven in het diagram:

![Het telemetrielogboek op het dashboard filteren][image5]

Deze wijziging is alleen van invloed op de grafiekweergave. De gegevenswaarden **ExternalTemperature** zijn nog steeds opgeslagen en beschikbaar voor alle bewerkingen backend worden gemaakt.

> [AZURE.NOTE] Mogelijk moet u uitschakelen en schakel op het apparaat Node.js op de pagina **apparaten** in het dashboard om de wijziging meteen weer te geven.

## <a name="handle-errors"></a>Greep van fouten

Voor een gegevensstroom op de grafiek wilt weergeven, moet het **Type** in de metagegevens **Apparaat-Info** overeenkomen met het gegevenstype van de waarden telemetrielogboek. Bijvoorbeeld als de metagegevens geeft aan dat het **Type** gegevens vochtigheid **int** en **dubbele** is gevonden in de stroom telemetrielogboek worden vervolgens het telemetrielogboek vochtigheid geen weergegeven in het diagram. De waarden **vochtigheid** zijn echter nog steeds opgeslagen en beschikbaar voor alle bewerkingen backend worden gemaakt.

## <a name="next-steps"></a>Volgende stappen

U kunt het gebruik van dynamische telemetrielogboek hebt gezien, kunt u meer informatie over het gebruik van gegevens van een apparaat in de vooraf geconfigureerde oplossingen: [apparaat informatie metagegevens in de externe monitoring vooraf geconfigureerde oplossing][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
