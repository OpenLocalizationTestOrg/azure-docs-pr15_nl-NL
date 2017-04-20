<properties
    pageTitle="Azure functies gebeurtenis Hub bindingen | Microsoft Azure"
    description="Meer informatie over het gebruik van Azure gebeurtenis Hub bindingen in Azure-functies."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure functies gebeurtenis Hub bindingen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code [Azure gebeurtenis Hub](../event-hubs/event-hubs-overview.md) bindingen voor Azure-functies. Azure functies ondersteuning van inwerkingtreding en uitvoer bindingen voor Azure gebeurtenis Hubs.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Als u nog niet eerder met Azure gebeurtenis Hubs, raadpleegt u het [overzicht van de Hub van Azure-gebeurtenis](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure gebeurtenis Hub binding activeren

Een Azure gebeurtenis Hub-trigger kan worden gebruikt om te reageren op een gebeurtenis die is verzonden naar een gebeurtenis hub gebeurtenis stream. U moet alleen toegang hebben tot de gebeurtenis hub voor het instellen van een binding trigger.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON voor gebeurtenis Hub activeert binding

Het bestand *function.json* voor een Azure gebeurtenis Hub-trigger Hiermee geeft u de volgende eigenschappen:

- `type`: Moet zijn ingesteld op *eventHubTrigger*.
- `name`: De naam van de variabele gebruikt in de functiecode voor het bericht van de hub gebeurtenis. 
- `direction`: Moet worden ingesteld *in*. 
- `path`: De naam van de hub gebeurtenis.
- `consumerGroup`: Dit is een optionele eigenschap gebruikt voor het instellen van de [groep consumenten](../event-hubs-overview.md#consumer-groups) gebruikt om u te abonneren op gebeurtenissen in de hub. Indien weggelaten de `$Default` consumenten groep wordt gebruikt. 
- `connection`: De naam van de instelling voor een app met de verbindingsreeks de naamruimte waarmee de gebeurtenis hub bevindt zich in de. Kopieer deze verbindingsreeks door te klikken op de knop **Verbindingsinformatie** voor de naamruimte, niet de gebeurtenis hub zelf.  Deze verbindingsreeks moet ten minste leesmachtigingen te activeren de trigger.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure gebeurtenis Hub trigger C#-voorbeeld
 
Met het bovenstaande voorbeeld-function.json, worden de hoofdtekst van bericht met de gebeurtenis geregistreerd met de C# functie onderstaande code:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Voorbeeld van Azure gebeurtenis Hub-trigger F #

Met het bovenstaande voorbeeld-function.json, worden de hoofdtekst van bericht met de gebeurtenis geregistreerd met de F # functie onderstaande code:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure gebeurtenis Hub trigger Node.js voorbeeld
 
Met het bovenstaande voorbeeld-function.json, worden de hoofdtekst van bericht met de gebeurtenis geregistreerd met de onderstaande Node.js functiecode:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure gebeurtenis Hub uitvoer binding

Een Hub van de gebeurtenis Azure uitvoer binding wordt gebruikt om te schrijven van gebeurtenissen naar een gebeurtenis hub gebeurtenis stream. U moet gemachtigd verzenden op een gebeurtenis hub om ernaar te schrijven gebeurtenissen. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON voor gebeurtenis Hub uitvoer binding

Het bestand *function.json* voor een Azure gebeurtenis Hub uitvoer binding Hiermee geeft u de volgende eigenschappen:

- `type`: Moet zijn ingesteld op *eventHub*.
- `name`: De naam van de variabele gebruikt in de functiecode voor het bericht van de hub gebeurtenis. 
- `path`: De naam van de hub gebeurtenis.
- `connection`: De naam van de instelling voor een app met de verbindingsreeks de naamruimte waarmee de gebeurtenis hub bevindt zich in de. Kopieer deze verbindingsreeks door te klikken op de knop **Verbindingsinformatie** voor de naamruimte, niet de gebeurtenis hub zelf.  Deze verbindingsreeks moet gemachtigd zijn verzenden het bericht verzenden naar de gebeurtenis Hub-stream.
- `direction`: Moet zijn ingesteld op *af*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure gebeurtenis Hub C# codevoorbeeld voor uitvoer binding
 
De volgende C# voorbeeld van de functiecode ziet het schrijven van een gebeurtenis aan een gebeurtenis Hub gebeurtenis stream. In dit voorbeeld wordt voorgesteld de Hub gebeurtenis uitvoer binding hierboven wordt toegepast op een C# timer trigger.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure gebeurtenis Hub F # codevoorbeeld voor uitvoer binding

De volgende F # voorbeeld van de functiecode ziet het schrijven van een gebeurtenis aan een gebeurtenis Hub gebeurtenis stream. In dit voorbeeld wordt voorgesteld de Hub gebeurtenis uitvoer binding hierboven wordt toegepast op een C# timer trigger.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure voorbeeld gebeurtenis Hub Node.js voor uitvoer binding
 
De volgende functiecode van de Node.js voorbeeld ziet het schrijven van een gebeurtenis aan een gebeurtenis Hub gebeurtenis stream. In dit voorbeeld wordt voorgesteld de Hub gebeurtenis uitvoer binding hierboven wordt toegepast op een Node.js timer-trigger.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
