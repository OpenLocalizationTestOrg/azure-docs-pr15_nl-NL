<properties
    pageTitle="Maken van een IoT Hub met de REST API | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag de REST API gebruiken om te maken van een IoT Hub."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Zelfstudie: Een IoT hub met een C#-programma en de REST API maken

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt de [provider van de resource IoT Hub REST API] [ lnk-rest-api] maken en beheren van Azure IoT hubs via programmacode. Deze zelfstudie ziet u hoe u de IoT Hub resource provider REST API gebruiken om te maken van een hub IoT vanuit een C#-programma.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure resourcemanager en klassiek](../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel resourcemanager Azure.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

- Microsoft Visual Studio 2015.
- Een actieve Azure-account. <br/>Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Uw project Visual Studio voorbereiden

1. Maak in Visual Studio, een Visual C# Windows-project met de project-sjabloon van **Console-toepassing** . De naam van het project **CreateIoTHubREST**.

2. In Solution Explorer met de rechtermuisknop op het project en klik vervolgens op **NuGet-pakketten beheren**.

3. In Nuget Package Manager controleren **voorlopige versie opnemen**en zoek naar **Microsoft.Azure.Management.ResourceManager**. Klik op **installeren**, klik op **OK**in **Wijzigingen controleren** en klik op **ik ga akkoord** accepteer de licenties.

4. In Nuget Package Manager, zoekt u **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klik op **installeren**, klik op **OK**in **Wijzigingen controleren** en klik op **ik ga akkoord** accepteer de licentie.

6. In Program.cs, vervangen door de bestaande instructies voor het **gebruik van** de volgende:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Program.cs, voeg de volgende statische variabelen die de waarden van de tijdelijke aanduiding vervangen. U hebt een genoteerd **ApplicationId**, **SubscriptionId** **TenantId**en **wachtwoord** eerder in deze zelfstudie. **De naam van de resource-groep** is de naam van de resourcegroep die u gebruikt wanneer u de hub IoT maakt, kunt werken met een bestaande groep of een nieuwe record. **De naam van de IoT Hub** is de naam van de IoT Hub die u hebt gemaakt, zoals **MyIoTHub** (deze naam globaal uniek zijn, zodat deze uw naam of initialen moet opnemen). **De naam van de implementatie** is een naam voor de implementatie, zoals **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>De REST API gebruiken om te maken van een hub IoT

Gebruik de [IoT Hub REST API] [ lnk-rest-api] een hub IoT maken in de resourcegroep. U kunt ook de REST API gebruiken om een bestaande IoT hub te wijzigen.

1. De volgende methode toevoegen aan Program.cs:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. De volgende code hebt toegevoegd aan de methode **CreateIoTHub** om te maken van een object **HttpClient** met de verificatietoken in de kop:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Voeg de volgende code toe met de methode **CreateIoTHub** om te beschrijven van de hub IoT voor het maken en een JSON-weergave genereren (Zie [Azure Status]voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub[lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. De volgende code hebt toegevoegd aan de methode **CreateIoTHub** naar de aanvraag REST bij Azure indienen, schakelt het antwoord en de URL die u kunt de status van de taak implementatie controleren ophalen:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. De volgende code toevoegen aan het einde van de methode **CreateIoTHub** het adres te gebruiken **asyncStatusUri** opgehaald in de vorige stap moet wachten om door de implementatie om uit te voeren:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. De volgende code toevoegen aan het einde van de methode **CreateIoTHub** voor het ophalen van de toetsen van de hub IoT u hebt gemaakt en deze aan de console afdrukken:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing wordt uitgevoerd

U kunt de toepassing nu voltooien door de methode **CreateIoTHub** roepen voordat u bouwen en worden uitgevoerd.

1. De volgende code toevoegen aan het einde van de methode **Main** :

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klik op **maken** en **oplossing maken**. Corrigeer eventuele fouten.

3. Op **fouten opsporen in** en klik vervolgens **Starten voor foutopsporing in** als de toepassing wilt uitvoeren. Het kan enkele minuten duren voor de implementatie om uit te voeren.

4. U kunt controleren of uw toepassing de nieuwe IoT hub toegevoegd via de [portal van Azure] [ lnk-azure-portal] en weergeven van uw lijst met resources, of met behulp van de PowerShell-cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] In dit voorbeeld-toepassing voegt een standaard IoT-Hub van S1, waarvoor u zijn gefactureerd. Wanneer u klaar bent, kunt u de hub IoT via de [portal van Azure] verwijderen[ lnk-azure-portal] of door de PowerShell-cmdlet **Verwijderen-AzureRmResource** wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT hub met de REST API hebt geïmplementeerd, wilt u mogelijk verder te onderzoeken:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
