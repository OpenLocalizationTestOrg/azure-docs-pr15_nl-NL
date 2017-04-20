<properties
   pageTitle="Verbinding maken met een apparaat met C op Linux | Microsoft Azure"
   description="Beschrijving van het koppelen van een apparaat met de Azure IoT Suite vooraf geconfigureerde externe controle oplossing met gebruikmaking van een toepassing in C waarop Linux geschreven."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Maken en uitvoeren van een steekproef C-client Linux

De volgende procedures wordt uitgelegd hoe u een clienttoepassing, geschreven in C en ingebouwd en uitvoeren op Ubuntu Linux maakt die communiceert met de externe controle vooraf geconfigureerde oplossing. Als u wilt deze stappen hebt voltooid, moet u een apparaat met Ubuntu versie 15.04 of 15.10. Voordat u verder gaat, installeert u de vereiste pakketten op uw apparaat Ubuntu is met de volgende opdracht:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>De clientbibliotheken op uw apparaat installeren

De Hub van Azure IoT client-bibliotheken zijn beschikbaar als een pakket die kunt u installeren op uw apparaat Ubuntu is met de opdracht **enz ophalen** . Voer de volgende stappen uit om het pakket waarin de IoT Hub client bibliotheek en kop bestanden op uw computer Ubuntu te installeren:

1. De AzureIoT-bibliotheek toevoegen aan de computer:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Installeer het pakket azure-iot-sdk-c-ontwikkelaar

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Code als u wilt opgeven van het gedrag van het apparaat toevoegen

Maak een map op uw computer Ubuntu **externe\_monitoring**. In de **externe\_monitoring** map maken de vier bestanden **main.c**, **externe\_monitoring.c**, **externe\_monitoring.h**, en **CMakeLists.txt**.

De IoT Hub serialisatiefunctie client-bibliotheken met een model kunt u de opmaak van het apparaat wordt verzonden naar IoT Hub en de opdrachten die afkomstig zijn van IoT Hub berichten opgeven.

1. Open in een teksteditor, de **externe\_monitoring.c** bestand. Voeg de volgende `#include` instructies:

    ```
    #include "iothubtransportamqp.h"
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
    static const char* hubName = "IoTHub Name]";
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

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Code implementatie van het gedrag van het apparaat toevoegen

De functies moet worden uitgevoerd wanneer het apparaat ontvangt een opdracht uit de hub en de code gesimuleerd telemetrielogboek versturen naar de hub toevoegen.

1. De volgende functies die worden uitgevoerd wanneer het apparaat de **SetTemperature** en **SetHumidity** opdrachten gedefinieerd in het model ontvangt toevoegen:

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
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
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
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
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
        platform_deinit();
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

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Code voor het aanroepen van de functie remote_monitoring_run toevoegen

Open het bestand **remote_monitoring.h** in een teksteditor. Voeg de volgende code:

```
void remote_monitoring_run(void);
```

Open het bestand **main.c** in een teksteditor. Voeg de volgende code:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>CMake gebruiken om te maken van de clienttoepassing

De volgende stappen wordt beschreven hoe u *CMake* gebruikt om u te maken van de clienttoepassing.

1. Open het bestand **CMakeLists.txt** in de map **remote_monitoring** in een teksteditor.

2. De volgende instructies voor het maken van de clienttoepassing definiëren toevoegen:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. Klik in de map **remote_monitoring** een map voor het opslaan van bestanden *aanbrengen* CMake genereert en voert u de opdrachten **cmake** en **brengt u** als volgt te maken:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Voer de clienttoepassing en telemetrielogboek op IoT Hub verzenden:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

