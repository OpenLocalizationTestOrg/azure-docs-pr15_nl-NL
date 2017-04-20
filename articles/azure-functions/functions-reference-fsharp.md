<properties
    pageTitle="Azure functies F # naslaginformatie voor ontwikkelaars | Microsoft Azure"
    description="Meer informatie over het ontwikkelen van Azure-functies met F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure-functies, functies, gebeurtenis processing, webhooks, dynamische berekeningscluster, als u kiest architectuur, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure functies naslaginformatie voor ontwikkelaars F #

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # voor functies van Azure is een oplossing voor het uitvoeren van eenvoudig kleine stukjes code of 'functies,' in de cloud. Gegevens doorloopt in uw functie F # via functieargumenten. Argumentnamen zijn opgegeven in `function.json`, en er zijn vooraf gedefinieerde namen voor toegang tot items, zoals de functie logboek en annulering tokens.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="how-fsx-works"></a>De werking van .fsx

Een `.fsx` bestand is een script F #. Dit kan worden beschouwd als een F #-project dat deel van één bestand uitmaakt. Het bestand bevat zowel de code voor het programma (in dit geval uw Azure-functie) en richtlijnen voor het beheren van afhankelijkheden.

Wanneer u gebruikt een `.fsx` voor een functie Azure, meestal vereist stroombaan vindt u hier automatisch voor u, zodat u kunt richten op de functie in plaats van "standaardtekst" code.

## <a name="binding-to-arguments"></a>Aan de argumenten

Elke binding ondersteunt bepaalde set met argumenten, zoals wordt beschreven in de [functies van Azure triggers en naslaginformatie voor ontwikkelaars van bindingen](functions-triggers-bindings.md). Een van de argument bindingen die ondersteuning biedt voor een trigger blob is bijvoorbeeld een POCO, die kan worden uitgedrukt met een F #-record. Bijvoorbeeld:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

De functie F # Azure duurt een of meer argumenten. Als we het gaan over de functies van Azure argumenten hebben, Raadpleeg we _invoer_ argumenten en _uitvoer_ argumenten. Een invoer argument is precies wat het klinkt als: invoer aan uw F # Azure-functie. Een _uitvoer_ -argument is veranderlijke gegevens of een `byref<>` van de argumenten die fungeert als een manier om door te geven gegevens weer _af_ van de functie.

In het bovenstaande voorbeeld `blob` is een invoer argument en `output` is een argument uitvoer. U ziet dat we gebruikt `byref<>` voor `output` (hoeft om toe te voegen de `[<Out>]` aantekeningen). Met een `byref<>` type kunt u uw gebruiken om welke record of een object het argument naar verwijst te wijzigen.

Wanneer een record F # wordt gebruikt als een invoertype, de definitie van de record moet worden gemarkeerd met `[<CLIMutable>]` zodat het kader Azure-functies voor het instellen van de velden correct voordat de record aan uw functie doorgegeven. Geavanceerde instellingen weergeven, `[<CLIMutable>]` genereert setters voor de recordeigenschappen van de. Bijvoorbeeld:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Een klasse F # kan ook worden gebruikt voor zowel in- en uitfaden argumenten. Voor een klasse moet eigenschappen meestal getters en setters. Bijvoorbeeld:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Logboekregistratie

Uitvoer om aan te melden uw [streaming logboeken](../app-service-web/web-sites-streaming-logs-and-console.md) in F #, de functie een argument van het type moet aandacht `TraceWriter`. Voor consistentie, wordt u geadviseerd dit argument heet `log`. Bijvoorbeeld:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchrone

De `async` werkstroom kan worden gebruikt, maar het resultaat moet retourneren een `Task`. U kunt dit doen met `Async.StartAsTask`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Annulering Token

Als uw functie, moet u omgaat met afsluiten zonder problemen, kunt u de geven een [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument. Dit kan worden gecombineerd met `async`, bijvoorbeeld:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Naamruimten importeren

Naamruimten kan worden geopend in de gebruikelijke manier:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

De volgende naamruimten worden automatisch geopend:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verwijst naar externe Assemblies

Daarnaast framework constructie verwijzingen worden toegevoegd aan de `#r "AssemblyName"` richtlijn.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
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
* `Microsoft.AspNEt.WebHooks.Common`.

Als u verwijzen naar een privé constructie wilt, kunt u het bestand constructie in uploaden een `bin` map ten opzichte van de functie- en verwijzingsfunctie deze met behulp van het bestand (bijvoorbeeld een naam geven  `#r "MyAssembly.dll"`). Zie voor informatie over het uploaden van bestanden naar de map van uw functie, de volgende sectie op pakket management.

## <a name="editor-prelude"></a>Editor Prelude

Een editor die ondersteuning biedt voor F # compileerprogramma Services worden niet op de hoogte van de naamruimten en samenstellen die Azure functies automatisch bevat. Als zodanig, kan het handig zijn om op te nemen een prelude waarmee de editor zoeken de stroombaan die u gebruikt en naamruimten expliciet te openen. Bijvoorbeeld:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wanneer Azure functies wordt uitgevoerd van uw code, wordt de bron met verwerkt `COMPILED` gedefinieerd, zodat de prelude editor worden genegeerd.

## <a name="package-management"></a>Pakket management

Toevoegen als u wilt gebruiken NuGet-pakketten in een functie F #, een `project.json` van het bestand in de map van de functie in de functie-app-bestandssysteem. Hier volgt een voorbeeld `project.json` bestand die worden toegevoegd een NuGet pakket verwijzing naar `Microsoft.ProjectOxford.Face` versie 1.1.0:

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

Alleen de .NET Framework 4.6 wordt ondersteund, dus zorg ervoor dat uw `project.json` bestand `net46` zoals hier wordt getoond.

Wanneer u bestanden uploadt een `project.json` bestand, de runtime krijgt de pakketten en verwijzingen naar het pakket stroombaan wordt automatisch toegevoegd. U hoeft te voegen `#r "AssemblyName"` richtlijnen. Toe te voegen de vereiste `open` instructies naar uw `.fsx` bestand.

U kunt desgewenst in moeten worden geplaatst automatisch verwijzingen stroombaan uw prelude editor, van uw editor interactie met F # compilatie-Services te verbeteren.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Het toevoegen van een `project.json` bestand aan uw Azure-functie

1. Begin door ervoor te zorgen dat uw functie-app wordt uitgevoerd, waarin u kunt doen door de functie openen in de portal van Azure. Dit ook biedt toegang tot de streaming logboeken waarin pakket installatie uitvoer worden weergegeven.

2. Voor het uploaden van een `project.json` bestand, voert u een van de methoden beschreven in [het bijwerken van de functie app-bestanden](functions-reference.md#fileupdate). Als u [Doorlopend implementatie van Azure-functies](functions-continuous-deployment.md)gebruikt, u kunt toevoegen een `project.json` bestand voor het tijdelijk opslaan tak om te experimenteren met deze voordat u deze toevoegt aan uw implementatie-vertakking.

3. Na het `project.json` bestand wordt toegevoegd, ziet u de uitvoer is vergelijkbaar met het volgende voorbeeld in de functie log bevindt zich streaming:

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

Als u een omgevingsvariabele of een waarde-app, gebruikt u `System.Environment.GetEnvironmentVariable`, bijvoorbeeld:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.Fsx code hergebruiken

U kunt code van andere `.fsx` bestanden met behulp van een `#load` richtlijn. Bijvoorbeeld:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Paden biedt tot de `#load` richtlijn zijn ten opzichte van de locatie van uw `.fsx` bestand.

* `#load "logger.fsx"`een bestand in de map van de functie laadt.

* `#load "package\logger.fsx"`laadt een bestand in de `package` map in de map van de functie.

* `#load "..\shared\mylogger.fsx"`laadt een bestand in de `shared` map op hetzelfde niveau als de functie aan te map, dat wil zeggen, direct onder `wwwroot`.

De `#load` richtlijn werkt alleen met `.fsx` (F #)-scriptbestanden, en niet met `.fs` bestanden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Werken met een F #](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure naslaginformatie voor ontwikkelaars van functies](functions-reference.md)
* [Azure functies C# naslaginformatie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure functies NodeJS-naslaginformatie voor ontwikkelaars](functions-reference-node.md)
* [Azure functies triggers en bindingen](functions-triggers-bindings.md)
* [Azure functies testen](functions-test-a-function.md)
* [Azure functies schaalbaarheid](functions-scale.md)
