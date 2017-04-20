<properties
    pageTitle="Azure functies HTTP en webhook bindingen | Microsoft Azure"
    description="Meer informatie over het gebruik van HTTP en webhook triggers en bindingen in Azure-functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure functies HTTP en webhook bindingen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code HTTP en webhook triggers en bindingen in Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON voor HTTP en webhook bindingen

Het bestand *function.json* biedt eigenschappen die betrekking op zowel de vergaderverzoeken en antwoorden hebben.

Eigenschappen voor de HTTP-aanvraag:

- `name`: Naam van de variabele gebruikt in de functiecode voor het object request (of het hoofdgedeelte van de aanvraag voor Node.js functies).
- `type`: Moet zijn ingesteld op *httpTrigger*.
- `direction`: Moet worden ingesteld *in*. 
- `webHookType`: Geldige waarden zijn voor WebHook triggers, *github*, *toegestane vertraging*en *genericJson*. Voor een HTTP-trigger dat niet wordt vermeld een WebHook, moet u deze eigenschap instellen op een lege tekenreeks. Zie voor meer informatie over WebHooks, de volgende sectie voor [WebHook triggers](#webhook-triggers) .
- `authLevel`: Niet van toepassing op WebHook triggers. Ingesteld op ","de API-sleutel, 'anoniem' voor het vereiste voor decoratieve de API-sleutel, of "beheerder" vereisen van de basispagina API-sleutel vereisen functie. Zie [API toetsen](#apikeys) hieronder voor meer informatie.

Eigenschappen voor het HTTP-antwoord:

- `name`: Naam van de variabele gebruikt in de functiecode voor het antwoordobject.
- `type`: Moet zijn ingesteld op *http*.
- `direction`: Moet zijn ingesteld op *af*. 
 
Voorbeeld *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook-triggers

Een trigger WebHook is een HTTP-trigger die de volgende functies die zijn ontworpen voor WebHooks heeft:

* Voor bepaalde WebHook providers (momenteel GitHub en toegestane vertraging worden ondersteund), de runtime van de functies van de provider handtekening valideert.
* De runtime functies bevat voor Node.js-functies, het hoofdgedeelte van de aanvraag in plaats van het object request. Er is geen speciale verwerking voor C#-functies, omdat u wat bepalen door het opgeven van het parametertype is opgegeven. Als u hebt opgegeven `HttpRequestMessage` u krijgt het verzoekobject. Als u een type POCO opgeeft, wordt de runtime functies geprobeerd parseren van een JSON-object in de hoofdtekst van het verzoek om te vullen de objecteigenschappen van het.
* Als u wilt activeren, een WebHook moet de functie de HTTP-aanvraag een API-sleutel bevatten. Deze vereiste geldt voor niet - WebHook HTTP-triggers, optioneel.

Zie voor informatie over het instellen van een GitHub WebHook, [GitHub ontwikkelaars - WebHooks maken](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL van de functie activeren

Als u wilt activeren een functie, kunt u een HTTP-aanvraag verzendt naar een URL die is een combinatie van de URL van de functie-app en de naam van de functie:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API-toetsen

Een API-sleutel moet worden opgenomen in een HTTP-aanvraag voor het starten van een functie HTTP of WebHook standaard. De toets kan worden opgenomen in een queryreeks-variabele met de naam `code`, of kan worden opgenomen in een `x-functions-key` HTTP koptekst. Voor niet-WebHook-functies, kunt u aangeven dat een API-sleutel is vereist door in te stellen de `authLevel` eigenschap 'anonieme' in het bestand *function.json* .

U vindt de API-sleutelwaarden in de map *D:\home\data\Functions\secrets* in het bestandssysteem van de functie-app.  De basispagina en functie sleutel worden ingesteld in het bestand *host.json* , zoals in dit voorbeeld. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

De functie-sleutel uit *host.json* kan worden gebruikt voor het activeren van elke functie, maar een uitgeschakelde functie won't activeren. De basispagina-toets kan worden gebruikt voor het activeren van elke functie en een functie wordt geactiveerd, zelfs als deze uitgeschakeld. U kunt configureren dat een functie als u de toets outmodel door in te stellen de `authLevel` eigenschap naar "beheerder". 

Als de map *geheimen* een JSON-bestand met dezelfde naam als een functie bevat, de `key` eigenschap in dat bestand kan ook worden gebruikt voor het starten van de functie en deze toets werkt alleen met de functie dit logboekitem verwijst. Bijvoorbeeld de API-sleutel voor een functie met de naam `HttpTrigger` is opgegeven in *HttpTrigger.json* in de map *geheimen* . Hier volgt een voorbeeld:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Wanneer u een trigger WebHook instelt, niet de toets outmodel delen met de WebHook-provider. Gebruik een sleutel die alleen werkt met de functie die de WebHook verwerkt.  De basispagina-toets kan worden gebruikt voor het starten van elke functie, zelfs functies uitgeschakeld.

## <a name="example-c-code-for-an-http-trigger-function"></a>C#-voorbeeldcode voor een HTTP-trigger-functie 

De voorbeeldcode Hiermee wordt gezocht naar een `name` parameter in de queryreeks- of de hoofdtekst van het HTTP-verzoek.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>F #-voorbeeldcode voor een HTTP-trigger-functie

De voorbeeldcode Hiermee wordt gezocht naar een `name` parameter in de queryreeks- of de hoofdtekst van het HTTP-verzoek.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

U moet een `project.json` bestand waarbij NuGet wordt gebruikt om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` stroombaan, als volgt:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Dit NuGet gebruiken om op te halen uw afhankelijkheden en wordt deze in uw script verwijzen.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Voorbeeld Node.js code voor een HTTP-trigger-functie 

In dit voorbeeldcode Hiermee wordt gezocht naar een `name` parameter in de queryreeks- of de hoofdtekst van het HTTP-verzoek.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>C#-voorbeeldcode voor een functie GitHub WebHook 

In dit voorbeeldcode logboeken GitHub probleem opmerkingen.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>F #-voorbeeldcode voor een functie GitHub WebHook

In dit voorbeeldcode logboeken GitHub probleem opmerkingen.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Voorbeeld Node.js code voor een functie GitHub WebHook 

In dit voorbeeldcode logboeken GitHub probleem opmerkingen.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
