<properties
    pageTitle="Azure functies Twilio binding | Microsoft Azure"
    description="Meer informatie over het gebruik van Twilio bindingen met Azure-functies."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure functies Twilio uitvoer binding

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe configureren en gebruiken van Twilio bindingen met Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure functies ondersteunt Twilio uitvoer Bindingen om in te schakelen van de functies voor het verzenden van SMS-berichten met een paar regels met code en een [Twilio](https://www.twilio.com/) -account. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON voor Azure melding Hub uitvoer binding

Het bestand function.json biedt de volgende eigenschappen:

- `name`: Naam van de variabele gebruikt in de functiecode voor het Twilio SMS-bericht.
- `type`: moet zijn ingesteld op *"twilioSms"*.
- `accountSid`: Met deze waarde moet worden ingesteld op de naam van de instelling voor een App waarin uw beveiligings-id Twilio Account.
- `authToken`: Met deze waarde moet worden ingesteld op de naam van de instelling voor een App waarin uw Twilio verificatietoken.
- `to`: Met deze waarde is ingesteld op het nummer dat de SMS-tekst wordt verzonden naar.
- `from`: Met deze waarde is ingesteld op het nummer dat de SMS-tekst is verzonden vanuit.
- `direction`: moet zijn ingesteld op *'meer'*.
- `body`: Met deze waarde kan worden gebruikt voor de SMS-bericht hard code als u niet nodig hebt voor het dynamisch instellen in de code voor uw functie. 

 
Voorbeeld function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Voorbeeld C# wachtrij trigger met Twilio uitvoer binding

#### <a name="synchronous"></a>Synchroon

Een parameter out wordt deze synchroon voorbeeldcode voor een Azure Storage wachtrij-trigger een tekstbericht versturen naar een klant die een order geplaatst.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchroon

Deze asynchrone voorbeeldcode voor een Azure Storage wachtrij-trigger stuurt een SMS-bericht naar een klant die een order geplaatst.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Voorbeeld Node.js wachtrij trigger met Twilio uitvoer binding

In dit voorbeeld Node.js stuurt een SMS-bericht naar een klant die een order geplaatst.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
