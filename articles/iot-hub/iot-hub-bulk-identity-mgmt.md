<properties
 pageTitle="Uitvoer van IoT Hub apparaat identiteiten importeren | Microsoft Azure"
 description="Concepten en .NET codefragmenten voor bulksgewijs beheer van IoT Hub apparaat identiteiten"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Beheer van IoT Hub apparaat identiteiten bulksgewijs

Elke hub IoT heeft een apparaat identiteit register die kunt u per apparaat resources in de service, zoals een rij met tijdens een vlucht cloud-to-device berichten maken. Het apparaat identiteit register kunt u ook toegang tot de eindpunten apparaat-omlaag te beheren. In dit artikel wordt beschreven hoe importeren en exporteren van apparaat identiteiten bulksgewijs en naar een apparaat identiteit register.

Importeren en exporteren van bewerkingen plaatsvinden in de context van *taken* waarmee u kunt de service bulkbewerkingen ten opzichte van een hub IoT uitvoeren.

De klasse **RegistryManager** bevat de **ExportDevicesAsync** en **ImportDevicesAsync** methoden die gebruikmaken van de **taak** framework. Deze methoden kunnen u exporteren, importeren en het geheel van een register IoT hub apparaat synchroniseren.

## <a name="what-are-jobs"></a>Wat zijn taken?

Apparaat identiteit registerbewerkingen het systeem **taak** gebruiken wanneer de bewerking:

*  Heeft mogelijk execution lang vergeleken met standaard runtime bewerkingen uitvoeren, of
*  Geeft als resultaat een grote hoeveelheid gegevens aan de gebruiker.

In dat geval in plaats van één API aanroep wachten of de blokkering van het resultaat van de bewerking maakt de bewerking asynchroon u een **taak** voor deze hub IoT. Klik met de bewerking retourneert direct een **JobProperties** .

De volgende C#-codefragment ziet u het maken van een taak voor het exporteren:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Vervolgens kunt u de klasse **RegistryManager** om query's in de status van de **taak** met de resulterende **JobProperties** -metagegevens.

Het volgende C#-codefragment ziet u hoe u een poll uitvoeren onder elke vijf seconden om te zien als de taak is uitgevoerd:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Apparaten exporteren

Gebruik de methode **ExportDevicesAsync** het geheel van een IoT hub apparaat register exporteren naar een [Opslag van Azure](https://azure.microsoft.com/documentation/services/storage/) blob container met een [Access-handtekening gedeeld](https://msdn.microsoft.com/library/ee395415.aspx).

Deze methode kunt u naar betrouwbaar back-ups van uw apparaatgegevens maken in een container blob waarmee u kunt u bepalen.

De methode **ExportDevicesAsync** worden twee parameters:

*  Een *tekenreeks* die een URI van een container blob bevat. Deze URI moet een token SA's die schrijven toegang tot de container verleent bevatten. De taak Hiermee maakt u een blok blob in deze container voor de opslag van de gegevens van het apparaat serienummer exporteren. Het token SA's omvat de volgende machtigingen:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  Een *Booleaanse* waarmee wordt aangegeven of u wilt uitsluiten van verificatiesleutels van uw gegevens exporteren. Als **Onwaar**, verificatie sleutels zijn opgenomen in exporteren uitvoert; anders worden toetsen geëxporteerd als **null**.

Het volgende C#-codefragment ziet u hoe u een taak voor het exporteren met daarin apparaat verificatie toetsen in de gegevens voor uitvoer starten en vervolgens poll uitvoeren onder voltooid:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

De uitvoer in de taak in de container meegeleverde blob als een blob blok met de naam **devices.txt**worden opgeslagen. De uitvoergegevens bestaat uit JSON serieel apparaatgegevens, met één apparaat per regel.

Het volgende voorbeeld ziet de uitvoergegevens:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Als u toegang tot deze gegevens in code nodig hebt, kunt u deze gegevens met behulp van de klasse **ExportImportDevice** gemakkelijk terug converteert. Het volgende C#-codefragment ziet u hoe gegevens van een apparaat die eerder is geëxporteerd naar een blob blokkeren:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  U kunt ook de methode **GetDevicesAsync** van de klasse **RegistryManager** gebruiken voor het ophalen van een lijst met uw apparaten. Deze methode heeft echter een vaste initiaal van 1000 van het aantal apparaatobjecten die worden geretourneerd. De verwachte use-case voor de methode **GetDevicesAsync** is bedoeld voor de ontwikkeling scenario's voor foutopsporing en wordt niet aanbevolen voor productie werkbelasting.

## <a name="import-devices"></a>Apparaten importeren

De methode **ImportDevicesAsync** in de klas **RegistryManager** kunt u importeren en synchronisatie bulkbewerkingen uitvoeren in een IoT hub apparaat register. Als de methode **ExportDevicesAsync** gebruikt de methode **ImportDevicesAsync** het kader van de **taak** .

Wees voorzichtig met behulp van de methode **ImportDevicesAsync** omdat naast nieuwe apparaten in het register van de identiteit apparaat is geïnstalleerd, kan er ook bijwerken en verwijderen van bestaande apparaten.

> [AZURE.WARNING]  Een importbewerking kan niet ongedaan worden gemaakt. Altijd back-up van uw bestaande gegevens met behulp van de methode **ExportDevicesAsync** naar een andere blob container voordat u bulksgewijs wijzigingen in uw apparaat identiteit register aanbrengen.

De methode **ImportDevicesAsync** nodig twee parameters:

*  Een *tekenreeks* met een URI van een [Opslag van Azure](https://azure.microsoft.com/documentation/services/storage/) blob container als *invoer* aan de taak moet worden gebruikt. Deze URI moet een token SA's die alleen toegang tot de container verleent bevatten. Deze container moet een blob met de naam **devices.txt** met de serienummer apparaatgegevens importeren in uw apparaat identiteit register bevatten. De gegevens importeren moet apparaatinformatie in de dezelfde JSON-indeling die de taak **ExportImportDevice** wordt gebruikt bij het maken van een blob **devices.txt** bevatten. Het token SA's omvat de volgende machtigingen:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  Een *tekenreeks* met een URI van een [Opslag van Azure](https://azure.microsoft.com/documentation/services/storage/) blob container als *uitvoer* van de taak moet worden gebruikt. De taak Hiermee maakt u een blok blob in deze container voor de opslag van eventuele foutinformatie uit de voltooide invoer **taak**. Het token SA's omvat de volgende machtigingen:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  De twee parameters kunnen verwijzen naar dezelfde blob container. De afzonderlijke parameters inschakelen gewoon meer controle over uw gegevens als de container uitvoer is vereist voor extra machtigingen.

Het volgende C#-codefragment ziet u hoe een taak importeren wordt gestart:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Gedrag importeren

U kunt de methode **ImportDevicesAsync** de volgende bulksgewijs-bewerkingen uitvoeren in het register van de identiteit apparaat:

-   Bulksgewijs registratie van nieuwe apparaten
-   Bulksgewijs verwijderingen van bestaande apparaten
-   Wijzigingen in de status bulksgewijs (in- of uitschakelen van apparaten)
-   Toewijzing van grote aantallen van nieuwe apparaat verificatiesleutels
-   Bulksgewijs auto-opnieuw genereren van apparaat verificatie toetsen

U kunt elke combinatie van de voorgaande bewerkingen binnen één **ImportDevicesAsync** aanroep uitvoeren. Bijvoorbeeld, kunt u nieuwe apparaten registreren en verwijdert of bestaande apparaten op hetzelfde moment bijwerken. Wanneer u samen met de methode **ExportDevicesAsync** gebruikt, kunt u volledig al uw apparaten van één IoT hub migreren naar een andere.

De eigenschap optioneel **ImportMode %** in de gegevens importeren serialisatie voor elk apparaat gebruiken voor het importeren proces per apparaat. De eigenschap **ImportMode %** heeft de volgende opties:

| ImportMode % |  Beschrijving |
| -------- | ----------- |
| **createOrUpdate** | Als u een apparaat met de opgegeven **id**niet bestaat, is het zojuist geregistreerd. <br/>Als het apparaat al bestaat, worden bestaande gegevens wordt overschreven door de meegeleverde invoergegevens zonder **ETag** -waarde. |
| **maken** | Als u een apparaat met de opgegeven **id**niet bestaat, is het zojuist geregistreerd. <br/>Als het apparaat al bestaat, wordt een fout naar het logboekbestand geschreven. |
| **bijwerken** | Als er al een apparaat met de opgegeven **id bestaat**, worden bestaande gegevens wordt overschreven door de meegeleverde invoergegevens zonder **ETag** -waarde. <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. |
| **updateIfMatchETag** | Als er al een apparaat met de opgegeven **id bestaat**, worden bestaande gegevens wordt overschreven door de meegeleverde invoergegevens alleen als er een overeenkomst **ETag** . <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. <br/>Als er een overeenkomend **ETag** gevonden, wordt een fout naar het logboekbestand geschreven. |
| **createOrUpdateIfMatchETag** | Als u een apparaat met de opgegeven **id**niet bestaat, is het zojuist geregistreerd. <br/>Als het apparaat al bestaat, worden bestaande gegevens wordt overschreven door de meegeleverde invoergegevens alleen als er een overeenkomst **ETag** . <br/>Als er een overeenkomend **ETag** gevonden, wordt een fout naar het logboekbestand geschreven. |
| **verwijderen** | Als er al een apparaat met de opgegeven **id bestaat**, wordt het verwijderd zonder de **ETag** -waarde. <br/>Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. |
| **deleteIfMatchETag** | Als er al een apparaat met de opgegeven **id bestaat**, is dit alleen als er een overeenkomst **ETag** is verwijderd. Als het apparaat niet bestaat, wordt een fout naar het logboekbestand geschreven. <br/>Als er een overeenkomend ETag gevonden, wordt een fout naar het logboekbestand geschreven. |

> [AZURE.NOTE] Als een vlag **ImportMode %** voor een apparaat niet expliciet wordt gedefinieerd door de serialisatiegegevens, wordt standaard **createOrUpdate** tijdens het importeren.

## <a name="import-devices-example--bulk-device-provisioning"></a>Voorbeeld van apparaten importeren – bulksgewijs inrichting van apparaat 

De volgende C# voorbeeld ziet u hoe u meerdere apparaat identiteiten genereren die:

- Verificatie toetsen opnemen.
- Deze apparaatinformatie naar een blok blob schrijven.
- Importeer de apparaten in het register van de identiteit apparaat.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Importeren apparaten voorbeeld – bulksgewijs verwijderen

Het volgende voorbeeld ziet u hoe u de apparaten die u met behulp van het vorige voorbeeld toegevoegd verwijderen:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>De container SAS URI ophalen


Het volgende voorbeeld ziet u hoe u het genereren van een [SA's URI](../storage/storage-dotnet-shared-access-signature-part-2.md) met lezen, schrijven en machtigingen voor een container blob verwijderen:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel, hebt u geleerd hoe bulkbewerkingen ten opzichte van het apparaat identiteit register in een hub IoT uitvoeren. Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Gebruik de doelstellingen][lnk-metrics]
- [Bewerkingen bewaken][lnk-monitor]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md