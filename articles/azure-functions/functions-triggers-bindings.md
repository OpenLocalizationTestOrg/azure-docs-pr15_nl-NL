<properties
    pageTitle="Azure functies triggers en bindingen | Microsoft Azure"
    description="Meer informatie over het gebruik van triggers en bindingen in Azure-functies."
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
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure functies triggers en bindingen naslaginformatie voor ontwikkelaars


In dit onderwerp bevat algemene verwijzing voor triggers en bindingen. Het bevat enkele functies van geavanceerde binding en syntaxis wordt ondersteund door alle bindingstypen.  

Als u voor gedetailleerde informatie zoeken wilt over het configureren en kleurcodering aan een bepaalde soort trigger of binding, wilt u mogelijk klikt u op een van de trigger of bindingen hieronder ziet u in plaats daarvan:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Deze artikelen wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure-functies](functions-reference.md)en de artikelen [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)of [Node.js](functions-reference-node.md) ontwikkelaars verwijzing hebt gelezen.



## <a name="overview"></a>Overzicht

Triggers zijn gebeurtenis antwoorden gebruikt voor het starten van uw aangepaste code. Ze kunnen u reageren op gebeurtenissen tussen het Azure platform of on-premises. Bindingen vertegenwoordigen de benodigde metagegevens waarmee uw code verbinding met de gewenste trigger of de bijbehorende invoer of de uitvoergegevens.

Als u een beter beeld van de verschillende bindingen die u met uw app Azure-functie integreren kunt, raadpleegt u de volgende tabel.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Voor een beter begrip activeert en bindingen Stel in het algemeen, dat u wilt uitvoeren van bepaalde code wordt een nieuw item in een wachtrij Azure Storage geplaatst proces. Azure functies biedt een Azure wachtrij-trigger om dit te ondersteunen. Moet u, de volgende informatie om de de wachtrij te houden:
 
- De opslagruimte rekening waar de wachtrij bestaat.
- De naam van de wachtrij.
- Een variabelennaam die uw code gebruiken zou om te verwijzen naar het nieuwe item dat in de wachtrij is verwijderd.  
 
Een trigger wachtrij binding deze gegevens voor een Azure-functie bevat. Het bestand *function.json* voor elke functie bevat alle gerelateerde bindingen.  Hier volgt een voorbeeld *function.json* met een wachtrij binding activeren. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Uw code mogelijk verschillende soorten uitvoer afhankelijk van hoe het nieuwe item in de wachtrij wordt verwerkt verzenden. U wilt bijvoorbeeld een nieuwe record aan een tabel Azure Storage schrijven.  Hiervoor kunt u een binding uitvoer aan een tabel Azure Storage instellen. Hier volgt een voorbeeld *function.json* waarin een opslagruimte tabel uitvoer binding die kan worden gebruikt met een wachtrij-trigger. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


De volgende C#-functie moet reageren op een nieuw item in de wachtrij wordt verwijderd en schrijft een nieuwe vermelding van de gebruiker in een tabel Azure Storage.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Zie [Azure functies triggers en bindingen voor Azure-opslag](functions-bindings-storage.md)voor meer codevoorbeelden en specifiekere informatie aangaande Azure opslagtypen die worden ondersteund.


Als u wilt de meer geavanceerde binding-functies gebruiken in de portal van Azure, klikt u op de optie **Geavanceerde editor** op het tabblad **integratie** van de functie. De geavanceerde editor kunt u de *function.json* rechtstreeks in de portal bewerken.

## <a name="dynamic-parameter-binding"></a>Dynamische parameter binding 

In plaats van een statische configuratie instellen voor uw uitvoer bindingeigenschappen, kunt u de instellingen dynamisch zijn gekoppeld aan gegevens die deel uitmaakt van de invoer binding van uw trigger. Houd rekening met een scenario waarbij nieuwe orders worden verwerkt met behulp van een wachtrij Azure Storage. Elke nieuwe wachtrij-item is een JSON-tekenreeks met ten minste de volgende eigenschappen:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

U kunt een SMS-bericht met uw account Twilio als een update dat de volgorde is ontvangen van de klant verzenden.  U kunt configureren de `body` en `to` veld van uw Twilio uitvoer binding dynamisch gekoppeld is aan de `name` en `mobileNumber` die deel uit van de invoer als volgt zijn.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
De functiecode heeft nu alleen initialisatie van de uitvoerparameter als volgt. Tijdens de uitvoering wordt de uitvoer eigenschappen zijn gekoppeld aan de gewenste invoergegevens.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Willekeurige GUID 's

Azure functies biedt een syntaxis voor het genereren van willekeurige GUID's met uw bindingen. De syntaxis van de volgende binding schrijft uitvoer naar een nieuwe BLOB met een unieke naam in een container Azure Storage: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Een enkel uitvoer retourneren

In gevallen waarin uw functiecode geeft als een enkel uitvoer resultaat, kunt u een uitvoer binding genaamd `$return` moeten worden bewaard een meer natuurlijke functiehandtekening in uw code. Dit kan alleen worden gebruikt met talen die ondersteuning bieden voor een retourwaarde (C#, Node.js, F #). De binding is vergelijkbaar met de volgende blob uitvoer binding die wordt gebruikt met een wachtrij-trigger.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


De volgende C#-code geeft als resultaat de uitvoer natuurlijke wijze zonder een `out` parameter in de functiehandtekening.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Dit dezelfde wordt geïllustreerd onder met Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # voorbeeld hieronder vermeld.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Dit kan ook worden gebruikt met meerdere uitvoerparameters door aan te duiden een enkel uitvoer met `$return`.


## <a name="route-support"></a>Route-ondersteuning

Wanneer u een functie voor een HTTP-trigger of WebHook, is de functie standaard gebruikt met een route van het formulier:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

U kunt deze route met het optionele `route` eigenschap op de HTTP-trigger bevindt zich binding invoer. Als u bijvoorbeeld de volgende *function.json* -bestand definieert een `route` eigenschap voor een HTTP-trigger:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Met deze configuratie, de functie kan worden nu geadresseerd met de volgende route in plaats van de oorspronkelijke route.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

In deze sectie geeft de functiecode ter ondersteuning van twee parameters in het adres, `category` en `id`. U kunt een [Web API Route beperking](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) gebruiken met uw parameters. De volgende C# functiecode maakt gebruik van beide parameters.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Hier ziet u Node.js functiecode dezelfde Routeparameters gebruiken.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Standaard worden alle functie routes voorafgegaan door *api*. U kunt ook aanpassen of verwijderen van het gebruik van voorvoegsel de `http.routePrefix` eigenschap in uw bestand *host.json* . Het volgende voorbeeld Hiermee verwijdert u het voorvoegsel *api* -doorsturen met behulp van een lege tekenreeks voor het voorvoegsel in het bestand *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Voor gedetailleerde informatie over hoe u het bestand *host.json* voor uw functie, Zie [hoe de functie app bijwerken bestanden](functions-reference.md#fileupdate)wilt bijwerken. 

De functie is nu gebruikt met de volgende route door deze configuratie toe te voegen:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Zie voor informatie over andere eigenschappen die u in uw bestand *host.json configureren kunt* [host.json verwijzing](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Een functie testen](functions-test-a-function.md)
* [De schaal van een functie aanpassen](functions-scale.md)
