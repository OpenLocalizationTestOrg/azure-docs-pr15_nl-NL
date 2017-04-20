<properties
    pageTitle="Azure naslaginformatie voor ontwikkelaars van functies NodeJS | Microsoft Azure"
    description="Meer informatie over het ontwikkelen van Azure-functies met NodeJS."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure functies NodeJS-naslaginformatie voor ontwikkelaars

> [AZURE.SELECTOR]
- [C#-script](../articles/azure-functions/functions-reference-csharp.md)
- [F # script](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Het knooppunt/JavaScript-ervaring voor Azure-functies kunt u heel gemakkelijk een functie die wordt doorgegeven exporteren een `context` object voor communicatie met de runtime en voor ontvangen en verzenden van gegevens via bindingen.

In dit artikel wordt ervan uitgegaan dat u de [Naslaginformatie voor ontwikkelaars van Azure functies](functions-reference.md)al hebt gelezen.

## <a name="exporting-a-function"></a>Exporteren van een functie

Alle JavaScript-functies moeten één exporteren `function` via `module.exports` voor de runtime om te zoeken van de functie en worden uitgevoerd. Deze functie moet altijd bevatten een `context` object.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindingen van `direction === "in"` worden doorgegeven als functieargumenten, wat betekent dat u hebt de beschikking [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) u dynamisch omgaat met nieuwe ingangen (bijvoorbeeld met behulp van `arguments.length` worden herhaald voor alle uw invoer). Deze functionaliteit is erg handig als u alleen een trigger waarvoor geen extra invoer, zoals u beter toegang uw triggergegevens zonder verwijst naar tot kunt uw `context` object.

De argumenten worden altijd doorgegeven aan de functie in de volgorde waarin dat ze voorkomen in *function.json*, zelfs als u niet achter de komma in de uitvoer-instructie opgeven. Als u bijvoorbeeld `function(context, a, b)` en wijzig deze naar `function(context, a)`, krijgt u nog steeds de waarde van `b` in de functiecode door te verwijzen naar `arguments[3]`.

Alle bindingen, ongeacht de richting, worden ook doorgegeven aan de `context` object (Zie hieronder). 

## <a name="context-object"></a>contextobject

De runtime gebruikt een `context` object om gegevens naar en vanuit uw functie te geven en dat u communiceren met de runtime kunt.

Het contextobject is altijd de eerste parameter aan een functie en moeten altijd worden opgenomen omdat er methoden zoals `context.done` en `context.log` die zijn vereist correct gebruik van de runtime. U kunt het object naam wat u maar wilt (dat wil zeggen `ctx` of `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.bindings

De `context.bindings` object al uw invoer- en uitvoerbereik gegevens worden verzameld. De gegevens worden toegevoegd naar de `context.bindings` object via de `name` eigenschap van de binding. Bijvoorbeeld, gegeven de definitie van de volgende binding in *function.json*, u hebt toegang tot de inhoud van de wachtrij via `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

De `context.done` functie de runtime worden vermeld dat u hebt uitgevoerd. Dit is belangrijk om te bellen wanneer u klaar bent met de functie. Als u dat niet de runtime nog nooit te weten dat uw functie is voltooid. 

De `context.done` functie kunt u weer een door de gebruiker gedefinieerde fout om aan te geven de runtime, evenals een eigenschap zak met eigenschappen die de eigenschappen op overschreven de `context.bindings` object.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(Message)

De `context.log` methode kunt u uitvoer log-instructies die aan elkaar zijn gekoppeld samen voor de registratie. Als u `console.log`, uw berichten worden alleen weergegeven voor proces niveau van logboekregistratie, dat niet zo handig is.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

De `context.log` methode ondersteunt dezelfde parameter opmaak die ondersteuning biedt voor het knooppunt [util.format methode](https://nodejs.org/api/util.html#util_util_format_format) . Zo is, bijvoorbeeld code als volgt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

kan worden geschreven als volgt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-triggers: context.req en context.res

In het geval van HTTP-Triggers, omdat dit een algemene patroon is gebruiken `req` en `res` voor de HTTP-objecten voor vergaderverzoeken en antwoorden, willen we vergemakkelijkt computers op het contextobject, in plaats van dat u voor het gebruik van de volledige toegang tot `context.bindings.name` patroon.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Knooppunt versie & pakket Management

De versie van het knooppunt is vergrendeld op `5.9.1`. We bent wordt onderzocht toe te voegen ondersteuning voor meer versies en het configureerbare gemaakt.

U kunt pakketten opnemen in uw functie door een *package.json* -bestand uploaden naar de map van de functie in de functie-app-bestandssysteem. Voor bestand uploaden instructies, raadpleegt u de sectie **het bijwerken van de functie app-bestanden** van de [functies van Azure ontwikkelaars verwijzing onderwerp](functions-reference.md#fileupdate). 

U kunt ook `npm install` in van de functie app SCM (Kudu) opdracht lijn interface:

1. Ga naar: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klik op **Console Foutopsporing > CMD**.

3. Navigeer naar `D:\home\site\wwwroot\<function_name>`.

4. Uitvoeren `npm install`.

Als de benodigde pakketten zijn geïnstalleerd, u het importeren ze aan uw functie op de gebruikelijke manieren (dat wil zeggen via `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Omgevingsvariabelen

Als u een omgevingsvariabele of een waarde-app, gebruikt u `process.env`, zoals wordt weergegeven in het volgende voorbeeld:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Ondersteuning voor machineschrift/CoffeeScript

Er is niet nog enkele directe ondersteuning voor het automatisch-compileren machineschrift/CoffeeScript via de runtime, zodat die alle zou moeten worden afgehandeld buiten de runtime, bij de implementatie. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Azure naslaginformatie voor ontwikkelaars van functies](functions-reference.md)
* [Azure functies C# naslaginformatie voor ontwikkelaars](functions-reference-csharp.md)
* [Azure functies F # naslaginformatie voor ontwikkelaars](functions-reference-fsharp.md)
* [Azure functies triggers en bindingen](functions-triggers-bindings.md)
