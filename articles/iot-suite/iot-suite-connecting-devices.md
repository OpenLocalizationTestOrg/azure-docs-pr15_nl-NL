<properties
   pageTitle="Verbinding maken met een apparaat met C in Windows | Microsoft Azure"
   description="Wordt beschreven hoe u verbinding maakt u een apparaat met de Azure IoT Suite vooraf geconfigureerde externe controle oplossing met gebruikmaking van een toepassing in C waarop Windows geschreven."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Een oplossing van de steekproef C maakt in Windows

De volgende stappen hoe u Visual Studio gebruiken om te maken van een clienttoepassing in C die met de oplossing externe controle vooraf geconfigureerde communiceert geschreven.

Maak een project starter in Visual Studio-2015 en de IoT Hub apparaat client NuGet-pakketten toevoegen:

1. In Visual Studio-2015 verlengt, een C-console-toepassing met de sjabloon visuele C++- **Console-Win32-toepassing** te maken. De naam van het project **RMDevice**.

2. Zorg ervoor dat **Console-programma** is geselecteerd en schakel **Precompiled kop** - en **beveiliging ontwikkeling levenscyclus (SDL) Hiermee wordt gecontroleerd**op de pagina **Instellingen voor toepassingen** in de **Wizard Win32-toepassing**.

3. Verwijder de bestanden stdafx.h, targetver.h en stdafx.cpp in **Solution Explorer**.

4. Geef het bestand RMDevice.cpp RMDevice.c in **Solution Explorer**.

5. Met de rechtermuisknop op het project **RMDevice** in **Solution Explorer**en klik op **beheren NuGet-pakketten**. Klik op **Bladeren**, en vervolgens zoeken naar en installeren van de volgende NuGet-pakketten in het project:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. Met de rechtermuisknop op het project **RMDevice** in **Solution Explorer**en klik vervolgens op **Eigenschappen** om van het project **Eigenschappenvensters** dialoogvenster te openen. Zie voor meer informatie [Instelling visuele C++ Projecteigenschappen][lnk-c-project-properties]. 

7. Klik op de **Linker** -map en klik op de pagina van de eigenschap **invoer** .

8. **Crypt32.lib** toevoegen aan de eigenschap **Extra afhankelijkheden** . Klik nogmaals **OK** en klik vervolgens op **OK** om te eigenschapswaarden van het project opslaan.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Het gedrag van het apparaat IoT Hub opgeven

De IoT Hub client-bibliotheken met een model kunt u de opmaak van de berichten die het apparaat wordt verzonden naar IoT Hub en de opdrachten die afkomstig zijn van IoT Hub opgeven.

1. Open het bestand RMDevice.c in Visual Studio. Het bestaande `#include` instructies met de volgende code:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Toevoegen van de volgende variabele declaraties na de `#include` instructies. Vervang de tijdelijke aanduiding voor waarden [apparaat-Id] en [apparaatsleutel] met waarden voor uw apparaat vanuit het externe controle oplossing dashboard. Gebruik de hostnaam van de Hub IoT vanuit het dashboard voor het vervangen van [naam van de IoTHub]. Bijvoorbeeld: als uw IoT Hub Hostname **contoso.azure-devices.net is**, vervangen door [naam van de IoTHub] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Voeg de volgende code als u wilt het model waarmee het apparaat kunt u communiceren met IoT Hub definiëren. Dit model geeft aan dat het apparaat temperatuur, externe temperatuur, vochtigheid en een apparaat-id als telemetrielogboek verzendt. Het apparaat verzendt ook metagegevens over het apparaat op IoT-Hub, inclusief een overzicht van opdrachten die ondersteuning biedt voor het apparaat. Dit apparaat reageert op de opdrachten **SetTemperature** en **SetHumidity**:

    ```
    // Define the Model
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

## <a name="implement-the-behavior-of-the-device"></a>Het gedrag van het apparaat implementeren

Voeg nu code die het gedrag gedefinieerd in het model implementeert toe.

1. De volgende functies die worden uitgevoerd wanneer het apparaat dat de opdrachten **SetTemperature** en **SetHumidity** van IoT Hub ontvangt toevoegen:

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

2. De volgende functie dat een bericht op IoT Hub verzendt toevoegen:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
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
    }
    ```

3. De volgende functie dat van de bibliotheek serialisatie in de SDK gebruikmaakt toevoegen:

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

4. Voeg de volgende functie verbinding maken met IoT Hub verzenden en ontvangen van berichten en de hub verbreken. Zoals u ziet hoe het apparaat verzendt metagegevens over zelf, inclusief de opdrachten ondersteund, op IoT Hub wanneer deze verbinding maakt. Deze metagegevens kunt de oplossing bij de status van het apparaat **uitgevoerd** op het dashboard:

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
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

5. De **belangrijkste** functie vervangen door een volgende code aan te roepen van de functie **remote_monitoring_run** :

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Klik op **maken** en vervolgens **Build Solution** om te maken van de apparaattoepassing.

7. Met de rechtermuisknop op het project **RMDevice** in **Solution Explorer**op **fouten opsporen in**en klik vervolgens op **nieuw exemplaar Start** als u wilt uitvoeren van de steekproef. De console worden berichten weergegeven, zoals de toepassing steekproef telemetrielogboek naar IoT Hub verzendt en opdrachten ontvangt.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx