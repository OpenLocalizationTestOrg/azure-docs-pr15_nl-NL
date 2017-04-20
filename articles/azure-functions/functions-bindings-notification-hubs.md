<properties
    pageTitle="Azure functies melding Hub binding | Microsoft Azure"
    description="Meer informatie over het gebruik van Azure melding Hub binding in Azure-functies."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure functies melding Hub uitvoer binding

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe u configureert en code Azure melding Hub bindingen in Azure-functies. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Uw functies kunnen push-meldingen een Hub voor melding van geconfigureerde Azure gebruikt met een paar regels met code verzenden. Echter is de Hub van de melding Azure geconfigureerd voor het Platform meldingen Services (PNS) u wilt gebruiken. Voor meer informatie over het configureren van een Azure melding Hub en ontwikkeling van een clienttoepassingen die registreren meldingen wilt ontvangen, Zie [aan de slag met melding Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) en klikt u op uw doel-client-platform aan de bovenkant.

De meldingen die u verzendt zijn systeemeigen meldingen of Sjabloonmeldingen. Systeemeigen meldingen doelgerichte een specifieke melding platform zoals geconfigureerd in de `platform` eigenschap van de uitvoer binding. De melding van een sjabloon kan worden gebruikt om meerdere platforms.   

## <a name="notification-hub-output-binding-properties"></a>Melding hub uitvoer bindingeigenschappen

Het bestand function.json biedt de volgende eigenschappen:

- `name`: Variabelennaam in functiecode voor het meldingsbericht voor de hub gebruikt.
- `type`: moet zijn ingesteld op *"notificationHub"*.
- `tagExpression`: Tag expressies kunnen u opgeven dat berichten worden bezorgd bij een reeks apparaten die zich hebben geregistreerd die overeenkomen met de code-expressie-meldingen wilt ontvangen.  Zie [Routering en tag expressies](../notification-hubs/notification-hubs-tags-segment-push-message.md)voor meer informatie.
- `hubName`: De naam van de resource van de hub melding in de portal van Azure.
- `connection`: Deze verbindingsreeks moet een verbindingsreeks **Toepassingsinstelling** is ingesteld op de waarde *DefaultFullSharedAccessSignature* voor uw hub melding.
- `direction`: moet zijn ingesteld op *'meer'*. 
- `platform`: De eigenschap platform geeft het platform melding aan uw doelen melding. Moet een van de volgende waarden:
    - `template`: Dit is de standaard-platform als de eigenschap platform wordt weggelaten uit de uitvoer binding. Sjabloonmeldingen kunnen worden gebruikt afstemmen van elk geconfigureerd op de Hub van de melding Azure platform. Zie voor meer informatie over het gebruik van sjablonen in het algemeen cross platform meldingen met een Azure melding Hub [sjablonen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple Push Notification-Service. De melding hub voor APNS en de melding ontvangt in een client-app, Zie voor meer informatie over het configureren van [push-meldingen voor iOS met Azure melding Hubs verzenden](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Amazon apparaat Messaging](https://developer.amazon.com/device-messaging). Zie voor meer informatie over het configureren van de melding hub voor ADM en de meldingen ontvangen in een app Kindle, [Aan de slag met melding Hubs voor Kindle-apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, dat wil de nieuwe versie van GCM zeggen, wordt ook ondersteund. De melding hub voor GCM/FCM en ontvangen van de melding in een app voor Android-client, Zie voor meer informatie over het configureren van [push-meldingen verzenden naar android-apparaat met Azure melding Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) hebt samengesteld Windows-platforms. Windows Phone 8.1 en hoger wordt ook ondersteund door WNS. De melding hub voor WNS en de melding ontvangt in een app universele Windows-Platform (UWP), Zie voor meer informatie over het configureren van [aan de slag met melding Hubs voor Windows universele Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoft Push Notification-Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Dit platform ondersteuning biedt voor Windows Phone 8 en eerdere Windows Phone-platforms. De melding hub voor MPNS en de melding ontvangt in een Windows Phone-app, Zie voor meer informatie over het configureren van [push-meldingen verzenden met Azure melding Hubs op Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Voorbeeld function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Melding hub verbinding tekenreeks instellen

Als u wilt een melding hub-uitvoer binden gebruiken, moet u de verbindingsreeks voor de hub configureren. Dit kan worden uitgevoerd op het tabblad *integreren* door uw hub melding selecteren of maken van een nieuwe record. 

U kunt ook handmatig een verbindingsreeks voor een bestaande hub toevoegen door een verbindingsreeks voor de *DefaultFullSharedAccessSignature* toe te voegen aan uw hub melding. Deze verbindingsreeks biedt uw functie toegangsmachtigingen meldingsberichten te verzenden. De tekenreekswaarde *DefaultFullSharedAccessSignature* verbinding kan worden geopend via de knop **toetsen** in het hoofdvenster blad van de resource van de hub melding in de portal van Azure. Handmatig een verbindingsreeks voor uw hub Voeg via de volgende stappen uit: 

1. Klik op het blad **app van de functie** van de Azure-portal, op **functie App-instellingen > Ga naar de App-Service-instellingen**.

2. Klik in het blad **Instellingen** op **Instellingen voor toepassingen**.

3. Schuif omlaag naar de sectie **verbindingstekenreeksen** en toevoegen van een benoemd item voor *DefaultFullSharedAccessSignature* waarde voor de hub van uw melding. Het type **schaal**wijzigen.
4. Verwijzen naar de naam van de tekenreeks verbinding in de uitvoer bindingen. Vergelijkbaar met **MyHubConnectionString** gebruikt in het bovenstaande voorbeeld.

## <a name="native-notification-examples"></a>Voorbeelden van systeemeigen melding

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>Systeemeigen meldingen APNS met C# wachtrij triggers

In dit voorbeeld wordt getoond hoe typen gedefinieerd in de [Microsoft Azure melding Hubs bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) gebruiken om een systeemeigen APNS-melding te verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>Systeemeigen meldingen GCM met C# wachtrij triggers

In dit voorbeeld wordt getoond hoe typen gedefinieerd in de [Microsoft Azure melding Hubs bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) gebruiken om een systeemeigen GCM melding te verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>Systeemeigen meldingen WNS met C# wachtrij triggers

In dit voorbeeld wordt getoond hoe typen die zijn gedefinieerd in de [Microsoft Azure melding Hubs bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) via een systeemeigen WNS mailpop-upmelding verzenden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Sjabloon melding voorbeelden

#### <a name="template-example-for-nodejs-timer-triggers"></a>Voorbeeld van de sjabloon voor Node.js timer triggers 

In dit voorbeeld wordt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met gestuurd `location` en `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Voorbeeld van de sjabloon voor F # timer triggers

In dit voorbeeld wordt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met gestuurd `location` en `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Voorbeeld van sjabloon een out parameter gebruiken 

In dit voorbeeld wordt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met gestuurd een `message` tijdelijke plaatsaanduiding in de sjabloon.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Voorbeeld van sjabloon met de functie asynchroon

Als u van asynchrone code gebruikmaakt, zijn parameters uit niet toegestaan. In dit geval gebruikt `IAsyncCollector` om terug te keren de melding van uw sjabloon. De volgende code is een asynchrone voorbeeld van de bovenstaande code. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Voorbeeld van sjabloon met JSON 

In dit voorbeeld wordt een melding voor een [sjabloon registratie](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) met gestuurd een `message` tijdelijke plaatsaanduiding in de sjabloon met een geldig JSON-tekenreeks.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Voorbeeld van sjabloon met melding Hubs bibliotheek typen

In dit voorbeeld wordt getoond hoe typen die zijn gedefinieerd in de [Microsoft Azure melding Hubs bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)gebruiken. 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
