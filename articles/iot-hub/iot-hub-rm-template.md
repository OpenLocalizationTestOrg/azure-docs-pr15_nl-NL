<properties
    pageTitle="Maken van een IoT-Hub met behulp van een sjabloon ARM en C# | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure resourcemanager sjablonen voor het maken van een IoT Hub waarbij een C#-programma."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Een IoT hub een C#-programma gebruikt met een resourcemanager Azure-sjabloon maken

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure resourcemanager maken en beheren van Azure IoT hubs via programmacode. Deze zelfstudie ziet u hoe u met een sjabloon Azure resourcemanager kunt maken van een hub IoT vanuit een C#-programma.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure resourcemanager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel resourcemanager Azure.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

- Microsoft Visual Studio 2015.
- Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- Een [account Azure Storage] [ lnk-storage-account] waar u uw Azure resourcemanager sjabloonbestanden kunt opslaan.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Uw project Visual Studio voorbereiden

1. Maak in Visual Studio, een Visual C# Windows-project met de project-sjabloon van **Console-toepassing** . De naam van het project **CreateIoTHub**.

2. In Solution Explorer met de rechtermuisknop op het project en klik vervolgens op **NuGet-pakketten beheren**.

3. In Nuget Package Manager controleren **voorlopige versie opnemen**en zoek naar **Microsoft.Azure.Management.ResourceManager**. Klik op **installeren**, klik op **OK**in **Wijzigingen controleren** en klik op **ik ga akkoord** accepteer de licenties.

4. In Nuget Package Manager, zoekt u **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klik op **installeren**, klik op **OK**in **Wijzigingen controleren** en klik op **ik ga akkoord** accepteer de licentie.

5. In Program.cs, vervangen door de bestaande instructies voor het **gebruik van** de volgende:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Program.cs, voeg de volgende statische variabelen die de waarden van de tijdelijke aanduiding vervangen. U hebt een genoteerd **ApplicationId**, **SubscriptionId** **TenantId**en **wachtwoord** eerder in deze zelfstudie. **Uw Azure-opslagaccountnaam** is de naam van het account Azure Storage waar u uw sjabloonbestanden Azure resourcemanager opslaan. **De naam van de resource-groep** is de naam van de resourcegroep die u gebruikt wanneer u de IoT Hub maakt, kunt werken met een bestaande groep of een nieuwe record. **De naam van de implementatie** is een naam voor de implementatie, zoals **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Een resourcemanager Azure-sjabloon als u wilt maken van een hub IoT indienen

Met een JSON-sjabloon en parameter-bestand te maken van een hub IoT in uw resourcegroep. U kunt ook een resourcemanager Azure-sjabloon gebruiken om een bestaande IoT hub te wijzigen.

1. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**en klik vervolgens op **Nieuw Item**. Voeg een JSON-bestand dat is genoemd **template.json** aan uw project.

2. De inhoud van **template.json** vervangen door de volgende definitie van de resource een standaard IoT hub toevoegen aan het gebied **Oost Amerikaans** (Zie [Azure Status]voor de huidige lijst met regio's die ondersteuning bieden voor IoT Hub[lnk-status]):

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

3. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**en klik vervolgens op **Nieuw Item**. Voeg een JSON-bestand dat is genoemd **parameters.json** aan uw project.

4. De inhoud van **parameters.json** vervangen door de volgende parameterinformatie die Hiermee stelt u een naam voor de nieuwe IoT-hub, zoals **{uw initialen} mynewiothub**. Houd er rekening mee dat de naam van de hub IoT moet uniek is zodat deze moet opnemen met uw naam of initialen):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Verbinding maken met uw Azure-abonnement in **Server Explorer**en in uw account Azure Storage maken een zogenaamd **sjablonen**. In het deelvenster **Eigenschappen** de **openbare leestoegang** machtigingen instellen voor de container **sjablonen** naar **Blob**.

6. Met de rechtermuisknop op de container **sjablonen** in **Server Explorer**en klik op **Weergave Blob Container**. Klik op de knop **Uploaden Blob** , selecteert u de twee bestanden, **parameters.json** en **templates.json**en klik vervolgens op **openen** om de JSON-bestanden uploaden naar de container **sjablonen** . De URL's van de BLOB's met de JSON-gegevens die zijn:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. De volgende methode toevoegen aan Program.cs:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Voeg de volgende code toe met de methode **CreateIoTHub** om in te dienen de sjabloon en parameter bestanden naar de resourcemanager van Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. De volgende code toevoegen aan de methode **CreateIoTHub** waarin de status en de sleutels voor de nieuwe IoT hub worden weergegeven:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing wordt uitgevoerd

U kunt de toepassing nu voltooien door de methode **CreateIoTHub** roepen voordat u bouwen en worden uitgevoerd.

1. De volgende code toevoegen aan het einde van de methode **Main** :

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klik op **maken** en **oplossing maken**. Corrigeer eventuele fouten.

3. Op **fouten opsporen in** en klik vervolgens **Starten voor foutopsporing in** als de toepassing wilt uitvoeren. Het kan enkele minuten duren voor de implementatie om uit te voeren.

4. U kunt controleren of uw toepassing de nieuwe IoT hub toegevoegd via de [portal van Azure] [ lnk-azure-portal] en weergeven van uw lijst met resources, of met behulp van de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] In dit voorbeeld-toepassing voegt een standaard IoT-Hub van S1, waarvoor u zijn gefactureerd. U kunt de hub IoT via de [portal van Azure] verwijderen[ lnk-azure-portal] of door de PowerShell-cmdlet **Verwijderen-AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT-hub gebruikt een resourcemanager Azure-sjabloon met een C#-programma hebt geïmplementeerd, wilt u mogelijk verder te onderzoeken:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
