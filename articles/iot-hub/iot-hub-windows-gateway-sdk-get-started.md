<properties
    pageTitle="Aan de slag met de IoT Hub Gateway SDK | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario Windows gebruiken om te laten zien belangrijke concepten die u moet kennen wanneer u de SDK van Azure IoT Gateway gebruiken."
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
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT Gateway SDK (beta) - aan de slag met Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het maken van de steekproef

Voordat u begint, moet u [uw ontwikkelomgeving instellen] [ lnk-setupdevbox] voor het werken met de SDK van Windows.

1. Open een **opdrachtprompt voor ontwikkelaars voor VS2015** opdrachtprompt.
2. Navigeer naar de hoofdmap in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek.
3. Voer de **Hulpmiddelen voor\\build.cmd** script. Dit script Hiermee maakt u een bestand van de oplossing Visual Studio, de oplossing maakt en wordt de tests wordt uitgevoerd. U vindt de Visual Studio-oplossing in de map **maken** in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek.

## <a name="how-to-run-the-sample"></a>Het uitvoeren van de steekproef

1. Het script **build.cmd** Hiermee maakt u een map genaamd **maken** in uw lokale kopie van de bibliotheek. Deze map bevat de twee modules in dit voorbeeld gebruikt.

    Het opbouwscript aantal_tekens hebt opgegeven **logger_hl.dll** in de **bouwen\\modules\\logboek\\foutopsporing** map en **hello_world_hl.dll** in de **bouwen\\modules\\hello_world\\foutopsporing** map. Gebruik deze paden voor de waarde van de **module pad** zoals wordt weergegeven in het instellingenbestand JSON.

2. Het bestand **hello_world_win.json** in de **voorbeelden\\hello_world\\src** map is een voorbeeld JSON instellingenbestand voor Windows die u gebruiken kunt om uit te voeren in het voorbeeld. De JSON-instellingen van het voorbeeld hieronder wordt ervan uitgegaan dat u de opslagplaats IoT Gateway SDK in de hoofdmap van uw station **C:** gekopieerd. Als u deze naar een andere locatie hebt gedownload, moet u aanpassen van de **module pad** waarden in de **voorbeelden\\hello_world\\src\\hello_world_win.json** bestand dienovereenkomstig gewijzigd.

3. Stel de waarde van de **bestandsnaam** op de naam en het pad van het bestand waarin u de logboekgegevens in de sectie **argumenten** voor de module **logger_hl** .

    Dit is een voorbeeld van een JSON instellingenbestand voor Windows waarmee worden geschreven naar het bestand **log.txt** in de hoofdmap van uw station **C:** .

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Navigeer naar de hoofdmap van uw lokale kopie van de **azure-iot-gateway-sdk** opslagplaats bij een opdrachtprompt.
4. Voer de volgende opdracht:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md