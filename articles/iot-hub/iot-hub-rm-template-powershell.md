<properties
    pageTitle="Maken van een IoT-Hub met behulp van een sjabloon van Azure resourcemanager en PowerShell | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure resourcemanager sjablonen voor het maken van een IoT Hub waarbij PowerShell."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Maken van een IoT hub via PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure resourcemanager maken en beheren van Azure IoT hubs via programmacode. Deze zelfstudie ziet u hoe u met een sjabloon Azure resourcemanager kunt maken van een hub IoT met PowerShell.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure resourcemanager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel resourcemanager Azure.

Deze zelfstudie hebt u het volgende nodig:

- Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

> [AZURE.TIP] Het artikel [Azure PowerShell gebruiken met Azure resourcemanager] [ lnk-powershell-arm] vindt u meer informatie over het gebruik van de PowerShell-scripts en Azure resourcemanager sjablonen Azure resources maken. 

## <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement

Voer de volgende opdracht uit te melden bij uw Azure-abonnement in een opdrachtprompt PowerShell:

```
Login-AzureRmAccount
```

U kunt de volgende opdrachten gebruiken om te ontdekken waar u een hub IoT en de ondersteunde API-versies kunt implementeren:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Een resourcegroep aanduiding van de hub van uw IoT met de volgende opdracht in een van de ondersteunde locaties voor IoT Hub maken. In dit voorbeeld wordt een resourcegroep **MyIoTRG1**genoemd:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Een resourcemanager Azure-sjabloon als u wilt maken van een hub IoT indienen

Gebruik een JSON-sjabloon maken van een hub IoT in uw resourcegroep. U kunt ook een resourcemanager Azure-sjabloon gebruiken om een bestaande IoT hub te wijzigen.

1. Gebruik een teksteditor om een **template.json** met de volgende definitie van de resource om te maken van een nieuwe standaard IoT hub genoemd resourcemanager Azure-sjabloon te maken. In dit voorbeeld wordt de IoT Hub toegevoegd in de regio **Oost Amerikaans** Hiermee maakt u twee groepen consumenten (**cg1** en **cg2**) aan het eindpunt van de gebeurtenis Hub-compatibele en gebruikt de **2016-02-03** API-versie. Deze sjabloon verwacht ook u in de naam van de hub IoT doorgeven als een parameter **hubName**genoemd. Zie [Azure Status]voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub[lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Sla het sjabloonbestand resourcemanager Azure op uw lokale computer. In dit voorbeeld wordt ervan uitgegaan dat u deze opslaat in een map genaamd **c:\templates**.

3. Voer de volgende opdracht om te implementeren van uw nieuwe IoT-hub, de naam van uw hub IoT geven als een parameter. In dit voorbeeld is de naam van de hub IoT **abcmyiothub** (Let erop dat deze naam moet uniek is zodat deze met uw naam of initialen opnemen moet):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. De uitvoer worden de toetsen voor de IoT hub die u hebt gemaakt.

5. U kunt controleren of uw toepassing de nieuwe IoT hub toegevoegd via de [portal van Azure] [ lnk-azure-portal] en weergeven van uw lijst met resources, of met behulp van de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] In dit voorbeeld-toepassing voegt een standaard IoT-Hub van S1, waarvoor u zijn gefactureerd. U kunt de hub IoT via de [portal van Azure] verwijderen[ lnk-azure-portal] of door de PowerShell-cmdlet **Verwijderen-AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT hub met een resourcemanager Azure-sjabloon met PowerShell hebt geïmplementeerd, wilt u mogelijk verder te onderzoeken:

- Lees meer over de mogelijkheden van de [provider van de resource IoT Hub REST API][lnk-rest-api].
- Lees [Azure resourcemanager overzicht] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.

Zie de volgende onderwerpen voor meer informatie over het ontwikkelen van voor IoT-Hub:

- [Inleiding tot C SDK][lnk-c-sdk]
- [IoT Hub SDK 's][lnk-sdks]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
