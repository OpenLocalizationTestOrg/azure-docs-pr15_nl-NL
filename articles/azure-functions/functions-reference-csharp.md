<properties
    pageTitle="Azure naslaginformatie voor ontwikkelaars van functies | Microsoft Azure"
    description="Meer informatie over het ontwikkelen van Azure-functies met C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure functies C# naslaginformatie voor ontwikkelaars

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
De C#-ervaring voor functies van Azure is gebaseerd op de SDK van Azure WebJobs. Gegevens doorloopt in uw functie C# via argumenten van de methode. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot items, zoals de functie logboek en annulering tokens.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="how-csx-works"></a>De werking van .csx

De `.csx` opmaak kunt minder "standaardtekst" schrijven en te belichten alleen een C# functie schrijven. Voor Azure-functies, u zojuist hebt toe te een constructie verwijzingen en naamruimten voegen u nodig hebt boven, zoals u gewend bent omhoog en in plaats van tekstterugloop alles in een naamruimte en class, alleen kunt u uw `Run` methode. Als u nodig hebt om op te nemen alle klassen, bijvoorbeeld POCO om objecten te definiëren, kunt u een klasse binnen hetzelfde bestand opnemen.

## <a name="binding-to-arguments"></a>Aan de argumenten

De verschillende bindingen zijn gekoppeld aan een functie C# via de `name` eigenschap in de configuratie *function.json* . Elke binding heeft een eigen ondersteunde typen die wordt beschreven per binding; een trigger blob kunt bijvoorbeeld een tekenreeks, een POCO of diverse andere typen ondersteunen. U kunt het beste past bij uw nodig type gebruiken. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Logboekregistratie

Uitvoer om aan te melden uw streaming Logboeken in C#, kunt u ook een `TraceWriter` argument getypt. Het is raadzaam dat u deze de naam `log`. We raden u vermijden `Console.Write` in Azure-functies.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchrone

Als u een functie asynchroon, gebruikt u de `async` trefwoord en als resultaat een `Task` object.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Annulering Token

In bepaalde gevallen wellicht bewerkingen gevoelige wordt af te sluiten. Terwijl het is verstandig om te schrijven code Number vast, in zaken waar u vraagt u omgaat met correcte afsluiten, u definieert een [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument getypt.  A `CancellationToken` krijgt als een afsluiten host wordt geactiveerd. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Naamruimten importeren

Als u importeren naamruimten wilt, kunt u doen dus gebruikelijke met de `using` component.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

De volgende naamruimten automatisch worden geïmporteerd en daarom zijn optioneel:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijst naar externe Assemblies

Voor framework stroombaan, verwijzingen toevoegen met behulp van de `#r "AssemblyName"` richtlijn.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

De volgende stroombaan, worden automatisch door de functies van de Azure hostomgeving toegevoegd:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Bovendien het volgende stroombaan speciale zijn geïntegreerd en kan worden verwezen in simplename (bijvoorbeeld `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Als u verwijzen naar een privé constructie wilt, kunt u het bestand constructie in uploaden een `bin` map ten opzichte van de functie- en verwijzingsfunctie deze met behulp van het bestand (bijvoorbeeld een naam geven  `#r "MyAssembly.dll"`). Zie voor informatie over het uploaden van bestanden naar de map van uw functie, de volgende sectie op pakket management.

## <a name="package-management"></a>Pakket management

Als u wilt gebruiken NuGet-pakketten in een C#-functie, een *project.json* bestand uploadt naar de map van de functie in de functie-app-bestandssysteem. Hier volgt een voorbeeld van een *project.json* bestand waarmee een verwijzing naar Microsoft.ProjectOxford.Face versie 1.1.0 worden toegevoegd:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Alleen de .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat het bestand *project.json* geeft `net46` zoals hier wordt getoond.

Wanneer u een bestand *project.json* uploaden, wordt de runtime krijgt de pakketten en verwijzingen naar het pakket stroombaan wordt automatisch toegevoegd. U hoeft te voegen `#r "AssemblyName"` richtlijnen. Toe te voegen de vereiste `using` instructies naar het bestand *run.csx* gebruik van de typen die is gedefinieerd in de NuGet-pakketten.


### <a name="how-to-upload-a-projectjson-file"></a>Het uploaden van een bestand project.json

1. Begin door ervoor te zorgen dat uw functie-app wordt uitgevoerd, waarin u kunt doen door de functie openen in de portal van Azure. 

    Dit ook biedt toegang tot de streaming logboeken waarin pakket installatie uitvoer worden weergegeven. 

2. Als u wilt een project.json-bestand uploaden, een van de methoden beschreven in de sectie **het bijwerken van de functie app-bestanden** van de [functies van Azure ontwikkelaars verwijzing onderwerp](functions-reference.md#fileupdate)te gebruiken. 

3. Nadat het bestand *project.json* is geüpload, ziet u de uitvoer zoals in het volgende voorbeeld in de functie log bevindt zich streaming:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261 
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.189 
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een waarde-app, gebruikt u `System.Environment.GetEnvironmentVariable`, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>.Csx code hergebruiken

U kunt klassen en methoden die zijn gedefinieerd in andere bestanden *.csx* in uw bestand *run.csx* gebruiken. Daarvoor gebruik `#load` richtlijnen in uw bestand *run.csx* , zoals wordt weergegeven in het volgende voorbeeld.

Voorbeeld *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Voorbeeld *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

U kunt een relatief pad met de `#load` richtlijn:

* `#load "mylogger.csx"`een bestand in de map van de functie laadt.

* `#load "loadedfiles\mylogger.csx"`een bestand in een map in de map functie laadt.

* `#load "..\shared\mylogger.csx"`een bestand in een map op hetzelfde niveau als de functie aan te map, dat wil zeggen, direct onder *wwwroot*laadt.
 
De `#load` richtlijn werkt alleen met *.csx* (C# script)-bestanden, niet met *:. cs* -bestanden. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure naslaginformatie voor ontwikkelaars van functies](functions-reference.md)
* [Azure functies F # naslaginformatie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure functies NodeJS-naslaginformatie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies triggers en bindingen](functions-triggers-bindings.md)

