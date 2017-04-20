<properties
    pageTitle="Azure functies Service Bus triggers en bindingen | Microsoft Azure"
    description="Meer informatie over het gebruik van Azure Service Bus triggers en bindingen in Azure-functies."
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

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure functies Service Bus triggers en bindingen voor wachtrijen en onderwerpen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code Azure Service Bus triggers en bindingen in Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Service Bus wachtrij of onderwerp trigger

#### <a name="functionjson"></a>Function.JSON

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor de wachtrij onderwerp of het bericht wachtrij of onderwerp. 
- `queueName`: Voor wachtrij zo activeren dat alleen de naam van de wachtrij worden gecontroleerd.
- `topicName`: Voor onderwerp zo activeren dat alleen de naam van het onderwerp worden gecontroleerd.
- `subscriptionName`: Voor onderwerp zo activeren dat alleen de naam van het abonnement.
- `connection`: De naam van de instelling voor een app waarin een Service Bus-verbindingsreeks. De verbindingsreeks moet voor een Service Bus naamruimte, niet beperkt tot een specifieke wachtrij of onderwerp. Als de verbindingsreeks niet rechten hebben beheren, stelt u de `accessRights` eigenschap. Als u laat `connection` leeg en de inwerkingtreding of binding werkt met de verbindingsreeks van de standaard-Service Bus voor de functie-app, dat is opgegeven door de instelling van de app AzureWebJobsServiceBus.
- `accessRights`: Hiermee geeft u de toegangsrechten die beschikbaar zijn voor de verbindingsreeks. Standaardwaarde is `manage`. Stel op `listen` als u een verbindingsreeks die geen gebruikmaakt van machtigingen beheren. De functies runtime proberen en fail voor bewerkingen die opgevolgd moeten anders beheren rechten.
- `type`: Moet zijn ingesteld op *serviceBusTrigger*.
- `direction`: Moet worden ingesteld *in*. 

Voorbeeld *function.json* voor een Service Bus wachtrij trigger:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C#-codevoorbeeld die een bericht van de wachtrij Service Bus verwerkt

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F #-codevoorbeeld die een bericht van de wachtrij Service Bus verwerkt

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Voorbeeld van de node.js-code die een bericht van de wachtrij Service Bus verwerkt

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Ondersteunde typen

De Service Bus wachtrij bericht kan worden gedeserialiseerd naar een van de volgende typen:

* Object (van JSON)
* tekenreeks
* byte-matrix 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock gedrag

De functies runtime ontvangt een bericht in `PeekLock` modus en oproepen `Complete` op het bericht als de functie is voltooid, of oproepen `Abandon` als de functie mislukt. Als de functie wordt uitgevoerd langer dan de `PeekLock` time-out, de vergrendeling automatisch wordt verlengd.

#### <a id="sbpoison"></a>Verwerking van gevaarlijke berichten

Service Bus biedt een eigen verontreinigd bericht voor de verwerking die niet kan worden beheerd of geconfigureerd in de configuratie van functies van Azure of code. 

#### <a id="sbsinglethread"></a>Enkel threading

De functies verwerkt runtime standaard meerdere berichten tegelijk. Als u wilt leiden de runtime om alleen een enkele wachtrij of onderwerp bericht per keer, instellen `serviceBus.maxConcurrrentCalls` 1 in het bestand *host.json* . Zie voor informatie over het bestand *host.json* , [Mapstructuur](functions-reference.md#folder-structure) in het naslagartikel voor ontwikkelaars en [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in de WebJobs.Script opslagplaats wiki.

## <a id="sboutput"></a>Service Bus wachtrij of onderwerp uitvoer binding

#### <a name="functionjson"></a>Function.JSON

Het bestand *function.json* Hiermee geeft u de volgende eigenschappen.

- `name`: De naam van de variabele gebruikt in de functiecode voor de wachtrij of wachtrij bericht. 
- `queueName`: Voor wachtrij zo activeren dat alleen de naam van de wachtrij worden gecontroleerd.
- `topicName`: Voor onderwerp zo activeren dat alleen de naam van het onderwerp worden gecontroleerd.
- `subscriptionName`: Voor onderwerp zo activeren dat alleen de naam van het abonnement.
- `connection`: Gelijk aan die voor Service Bus activeren.
- `accessRights`: Hiermee geeft u de toegangsrechten die beschikbaar zijn voor de verbindingsreeks. Standaardwaarde is `manage`. Stel op `send` als u een verbindingsreeks die geen gebruikmaakt van machtigingen beheren. De functies runtime proberen en fail voor bewerkingen die opgevolgd moeten beheren anders rechten, zoals het maken van wachtrijen.
- `type`: Moet zijn ingesteld op *serviceBus*.
- `direction`: Moet zijn ingesteld op *af*. 

Voorbeeld *function.json* voor het gebruik van een timer-trigger Service Bus wachtrij berichten:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Ondersteunde typen

Azure-functies kunnen u een bericht van de wachtrij Service Bus maken vanaf een van de volgende typen.

* Object (altijd Hiermee maakt u een bericht JSON, maakt u het bericht met een null-object als de waarde null is wanneer de functie eindigt)
* tekenreeks (Hiermee maakt u een bericht als de waarde geen null-is wanneer de functie eindigt)
* byte-matrix (werkt als tekenreeks) 
* `BrokeredMessage`(C#, werkt als tekenreeks)

Voor het maken van meerdere berichten in een C#-functie, kunt u `ICollector<T>` of `IAsyncCollector<T>`. Een bericht wordt gemaakt wanneer u belt de `Add` methode.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C#-codevoorbeelden die Service Bus Wachtrijberichten maken

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F #-codevoorbeeld die Hiermee maakt u een bericht van de wachtrij Service Bus

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Voorbeeld van de node.js-code die wordt gemaakt van een bericht van de wachtrij Service Bus

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
