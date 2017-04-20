<properties
    pageTitle="Maken van een IoT Hub met Azure CLI | Microsoft Azure"
    description="Ga als volgt in dit artikel als u wilt maken van een IoT Hub voor de opdrachtregel Azure."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Een IoT Hub met Azure CLI maken

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure opdrachtregel-Interface maken en beheren van Azure IoT hubs via programmacode. In dit artikel leest u hoe u een IoT Hub maken met de CLI Azure.

Deze zelfstudie hebt u het volgende nodig:

- Een actieve Azure-account. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- [Azure CLI 0.10.4] [ lnk-CLI-install] of hoger. Als u al Azure CLI kunt u de huidige versie bij de opdrachtprompt met de volgende opdracht kunt valideren:
```
    azure --version
```

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure resourcemanager en klassiek](../resource-manager-deployment-model.md). De CLI Azure moet in de modus van Azure resourcemanager:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en een abonnement 

1. Bij de aanmelding opdrachtprompt door te typen van de volgende opdracht uit
```
    azure login
```
Gebruik de voorgestelde webbrowser en de code om te verifiëren.

2. Als u meerdere Azure abonnementen hebt, wordt verbinding maken met Azure toegang tot alle Azure abonnementen die is gekoppeld aan uw referenties verleend. U kunt weergeven de Azure abonnementen, evenals welke standaard wordt met de opdracht
```
    azure account list 
```

Voor het instellen van het abonnement context Selecteer onder welke u wilt uitvoeren van de rest van het gebruik van de opdrachten

```
    azure account set <subscription name>
```

3. Als er geen een resourcegroep kunt u één benoemde **exampleResourceGroup** maken 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Het [gebruik van de Azure CLI als u wilt beheren Azure resources en resourcegroepen] artikel[ lnk-CLI-arm] vindt u meer informatie over het gebruik van Azure CLI Azure bronnen beheren. 


## <a name="create-an-iot-hub"></a>Een Hub IoT maken

Vereiste parameters:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Als u wilt zien van alle parameters beschikbaar zijn voor het maken van kunt u de opdracht help in opdrachtprompt
```
    azure iothub create -h 
```
Snel voorbeeld:

 Als u wilt maken van een IoT Hub **exampleIoTHubName** de naam in de resource groep **exampleResourceGroup** uit de volgende opdracht uit te voeren
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Deze opdracht Azure CLI Hiermee maakt u een standaard IoT-Hub van S1, waarvoor u zijn gefactureerd. U kunt de IoT hub **exampleIoTHubName** volgende opdracht gebruiken verwijderen 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over het ontwikkelen van voor IoT-Hub:
- [IoT Hub SDK 's][lnk-sdks]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Met behulp van de Azure-Portal voor het beheren van IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
