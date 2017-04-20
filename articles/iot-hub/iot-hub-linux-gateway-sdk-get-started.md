<properties
    pageTitle="Aan de slag met de IoT Hub Gateway SDK | Microsoft Azure"
    description="In dit scenario Azure IoT Gateway SDK wordt Linux gebruikt om te laten zien belangrijke concepten die u kennen moet wanneer u de SDK van Azure IoT Gateway gebruiken."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT Gateway SDK (beta) - aan de slag met Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Het maken van de steekproef

Voordat u begint, moet u [uw ontwikkelomgeving instellen] [ lnk-setupdevbox] voor het werken met de SDK op Linux.

1. Een shell opent.
2. Navigeer naar de hoofdmap in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek.
3. Voer het script **tools/build.sh** . Het hulpprogramma **cmake** dit script gebruikt om te maken van een map **maken** in de hoofdmap van uw lokale kopie van de bibliotheek **azure-iot-gateway-sdk** en genereren een make-bestand. Het script vervolgens de oplossing genereert en wordt de tests wordt uitgevoerd.

> [AZURE.NOTE]  Telkens wanneer u het script **build.sh** uitvoert, wordt verwijderd en vervolgens wordt de map **maken** in de hoofdmap van uw lokale kopie van de bibliotheek **azure-iot-gateway-sdk** opnieuw gemaakt.

## <a name="how-to-run-the-sample"></a>Het uitvoeren van de steekproef

1. Het script **build.sh** genereert de uitvoer in de map **maken** in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek. Dit geldt ook voor de twee modules in dit voorbeeld gebruikt.

    Het opbouwscript aantal_tekens hebt opgegeven **liblogger_hl.so** in de **Opbouwen/modules/logboek/** map en **libhello_world_hl.so** in de **Opbouwen/modules/hello_world/** map. Gebruik deze paden voor de waarde van de **module pad** zoals wordt weergegeven in het instellingenbestand JSON.

2. Het bestand **hello_world_lin.json** in de map **voorbeelden/hello_world/src** is een voorbeeld JSON instellingenbestand voor Linux die u gebruiken kunt om uit te voeren in het voorbeeld. De JSON-instellingen van het voorbeeld hieronder wordt ervan uitgegaan dat u de steekproef vanuit de hoofdsite van uw lokale kopie van de bibliotheek **azure-iot-gateway-sdk** wordt uitgevoerd.

3. Stel de waarde van de **bestandsnaam** op de naam en het pad van het bestand waarin u de logboekgegevens in de sectie **argumenten** voor de module **logger_hl** .

    Dit is een voorbeeld van een bestand met de JSON-instellingen voor Linux waarmee worden geschreven naar de **log.txt** naar de map waarin u de steekproef wordt uitgevoerd.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. Navigeer naar de map **azure-iot-gateway-sdk** in uw shell.
4. Voer de volgende opdracht:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
