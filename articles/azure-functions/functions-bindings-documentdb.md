<properties
    pageTitle="Azure functies DocumentDB bindingen | Microsoft Azure"
    description="Meer informatie over het gebruik van Azure DocumentDB bindingen in Azure-functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure werkt, functies, verwerking van gebeurtenis, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure functies DocumentDB bindingen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code Azure DocumentDB bindingen in Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB input binding

Invoer bindingen kunnen laden van een document uit een verzameling DocumentDB en doorgegeven rechtstreeks aan uw binding. De document-id kan worden bepaald op basis van de trigger die de functie aangeroepen. In een functie C# eventuele wijzigingen aan de record automatisch wordt verzonden terug naar de verzameling wanneer de functie is afgesloten.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON voor DocumentDB invoer binding

Het bestand *function.json* biedt de volgende eigenschappen:

- `name`: Naam van de variabele gebruikt in de functiecode voor het document.
- `type`: moet zijn ingesteld op "documentdb".
- `databaseName`: De database met van het document.
- `collectionName`: De siteverzameling die het document bevat.
- `id`: De Id van het document om op te halen. Deze eigenschap ondersteunt bindingen die vergelijkbaar is met "{queueTrigger}", waarin de tekenreekswaarde van het bericht wachtrij wordt gebruikt als het document-id.
- `connection`: Deze tekenreeks moet een set toepassingsinstelling naar het eindpunt voor uw account DocumentDB. Als u uw account via het tabblad integreren kiest, wordt een nieuwe App-instelling voor u gemaakt met een naam die de volgende vorm, yourAccount_DOCUMENTDB heeft. Als u nodig hebt handmatig maken de App-instelling, de werkelijke verbindingsreeks moet er als volgt uit, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- ' richting: moet zijn ingesteld op *'in'*.

Voorbeeld *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Voorbeeld van Azure DocumentDB invoer code voor een C# wachtrij trigger
 
Het bovenstaande voorbeeld-function.json gebruikt, wordt de DocumentDB invoer binding ophalen van het document aan de id die overeenkomt met de wachtrij bericht tekenreeks en doorgegeven aan de parameter 'document'. Als dat document niet wordt gevonden, wordt de parameter 'document' niet null zijn. Het document wordt vervolgens met de nieuwe tekstwaarde bijgewerkt wanneer de functie afgesloten.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Voorbeeld van Azure DocumentDB invoer code voor een F # wachtrij-trigger

Het bovenstaande voorbeeld-function.json gebruikt, wordt de DocumentDB invoer binding ophalen van het document aan de id die overeenkomt met de wachtrij bericht tekenreeks en doorgegeven aan de parameter 'document'. Als dat document niet wordt gevonden, wordt de parameter 'document' niet null zijn. Het document wordt vervolgens met de nieuwe tekstwaarde bijgewerkt wanneer de functie afgesloten.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

U moet een `project.json` bestand met NuGet om op te geven de `FSharp.Interop.Dynamic` en `Dynamitey` pakketten als pakketafhankelijkheden, zoals hier:

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

Dit NuGet gebruiken om op te halen uw afhankelijkheden en wordt deze in uw script verwijzen.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB invoer codevoorbeeld voor een Node.js wachtrij-trigger
 
Met het bovenstaande voorbeeld-function.json, de DocumentDB invoer binding wordt opgehaald het document aan de id die overeenkomt met de wachtrij bericht tekenreeks en doorgegeven aan de `documentIn` binden eigenschap. Bijgewerkte documenten worden niet in Node.js functies verzonden terug naar de siteverzameling. Echter, kunt u doorgeven de invoer binding rechtstreeks naar de uitvoer van een DocumentDB binden benoemde `documentOut` ter ondersteuning van updates. In dit codevoorbeeld de eigenschap text van de invoer document wordt bijgewerkt en ingesteld als het uitvoerdocument.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB uitvoer bindingen

Uw functies kunnen schrijven JSON documenten met een DocumentDB Azure-database met behulp van de **Azure DocumentDB Document** uitvoer binding. Bekijk de [Inleiding tot DocumentDB](../documentdb/documentdb-introduction.md) en de [zelfstudie aan de slag](../documentdb/documentdb-get-started.md)voor meer informatie over Azure DocumentDB.

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON voor DocumentDB uitvoer binding

Het bestand function.json biedt de volgende eigenschappen:

- `name`: Naam van de variabele gebruikt in de functiecode voor het nieuwe document.
- `type`: moet zijn ingesteld op *"documentdb"*.
- `databaseName`: De database met van de siteverzameling waar het nieuwe document wordt gemaakt.
- `collectionName`: De siteverzameling waar het nieuwe document wordt gemaakt.
- `createIfNotExists`: Dit is een Booleaanse waarde om aan te geven of de verzameling worden gemaakt als deze nog niet bestaat. De standaardinstelling is *Onwaar*. De reden voor dit nieuwe is siteverzamelingen worden gemaakt met gereserveerde doorvoer, met prijzen consequenties. Ga naar de [pagina prijzen](https://azure.microsoft.com/pricing/details/documentdb/)voor meer informatie.
- `connection`: Deze tekenreeks moet dat een **Toepassingsinstelling** naar het eindpunt voor uw account DocumentDB instellen. Als u uw account via het tabblad **integreren** , een nieuwe App-instelling wordt gemaakt voor u met een naam die de volgende vorm heeft `yourAccount_DOCUMENTDB`. Als u nodig hebt handmatig maken de App-instelling, de werkelijke verbindingsreeks moet er als volgt uit, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: moet zijn ingesteld op *'meer'*. 
 
Voorbeeld function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB uitvoer codevoorbeeld voor een Node.js wachtrij-trigger

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Het uitvoerdocument:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB uitvoer codevoorbeeld voor een F # wachtrij-trigger

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB uitvoer codevoorbeeld voor een C# wachtrij-trigger


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB uitvoer code instelling bestand Voorbeeldnaam

Als u instellen van de naam van het document in de functie wilt,,, stelt u de `id` waarde.  Als bijvoorbeeld JSON inhoud voor een werknemer is wordt verwijderd in de wachtrij ongeveer als volgt uit:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

U kunt de volgende C#-code in een functie van de trigger wachtrij: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Of de overeenkomstige F #-code:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Voorbeeld van de uitvoer:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
