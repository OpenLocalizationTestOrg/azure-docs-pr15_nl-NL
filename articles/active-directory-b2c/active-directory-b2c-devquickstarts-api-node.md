<properties
    pageTitle="Azure AD B2C: Een web API beveiligen met behulp van Node.js | Microsoft Azure"
    description="Het maken van een Node.js web API die tokens afgeleid van een tenant B2C accepteert"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Een web API beveiligen met behulp van Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met Azure Active Directory (Azure AD) B2C, kunt u een web API beveiligen door het OAuth 2.0 access tokens gebruiken. Deze tokens toestaan uw client-apps die gebruikmaken van Azure AD B2C om te verifiëren tot de API. In dit artikel leest u hoe u een "takenlijst"-API waarmee gebruikers kunt toevoegen en een lijst met taken maken. Het web API is beveiligd met Azure AD B2C en kan alleen geverifieerde gebruikers hun takenlijst beheren.

> [AZURE.NOTE]  In dit voorbeeld is geschreven om te worden verbonden met door middel van onze [iOS B2C steekproef-toepassing](active-directory-b2c-devquickstarts-ios.md). De huidige overzicht eerst te doen en volg samen met de steekproef.

**Passport** is verificatie middleware voor Node.js. Flexibele en modulaire, Passport onopvallend kan worden geïnstalleerd in een Express gebaseerde of Restify webtoepassing. Een uitgebreide reeks strategieën ondersteunt verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Azure Active Directory (Azure AD). U deze module hebt geïnstalleerd en voegt u vervolgens de Azure AD `passport-azure-ad` invoegtoepassing.

Als u wilt uitvoeren in dit voorbeeld, moet u:

1. Een toepassing met Azure AD registreren.
2. Instellen van uw toepassing voor gebruik in combinatie van `azure-ad-passport` invoegtoepassing.
3. Een clienttoepassing om te bellen van de 'takenlijst' web API configureren.


## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant.  Een map is een container voor alle gebruikers, apps, groepen en meer.  Als u deze niet hebt, worden al doorgaan [een map B2C maken](active-directory-b2c-get-started.md) voordat u.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C waarmee Azure AD bepaalde gegevens die zijn vereist voor het veilig communiceren met uw app. In dit geval worden zowel de client-app en de web API aangegeven met een enkel **Toepassings-ID**, omdat ze bestaan uit één logische app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken. Zorg ervoor dat:

- Bevatten een **web app/web api** in de toepassing
- Voer `http://localhost/TodoListService` als een **antwoord-URL**. De standaard-URL voor dit voorbeeld is.
- Een **toepassing geheim** voor uw toepassing maken en deze kopiëren. U deze gegevens later nodig hebt. Houd er rekening mee dat deze waarde worden [voorafgegaan XML moet](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) voordat u deze gebruiken.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. U deze gegevens later nodig hebt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat twee identiteit ervaringen: aanmelden en meld u aan. U moet maken van één beleid van elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies de **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies de **naam weer te geven** en de **Object-ID** -toepassing claims in elk beleid.  U kunt ook andere claims.
- Kopieer omlaag de **naam** van elk beleid nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  U nodig beleid namen later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u uw drie beleidsregels hebt gemaakt, kunt u bent uw app maken.

Meer informatie over de werking van beleidsregels in Azure AD B2C, begint u met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Maak de steekproef terwijl u door kunt u [een skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

## <a name="download-nodejs-for-your-platform"></a>Node.js voor uw platform downloaden

Als u wilt gebruiken in dit voorbeeld, moet u eerst een werkende installatie van Node.js. 

Installeer Node.js vanaf [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>MongoDB installeren voor uw platform

Als u wilt gebruiken in dit voorbeeld, moet u eerst een werkende installatie van MongoDB. We MongoDB gebruiken om uw REST API permanente alle werkstroomexemplaren van de server.

Installeer MongoDB vanaf [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] In dit overzicht wordt ervan uitgegaan dat u de standaard-installatie- en server eindpunten gebruiken voor MongoDB, namelijk op het moment van deze schrijven `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Een installatiefout met de modules Restify in uw web API

We Restify gebruiken om te maken van uw REST API. Restify afkomstig is van een minimale en flexibele Node.js toepassing kader Express. Er wordt een reeks robuuste functies voor het samenstellen van REST API's boven op verbinding maken.

### <a name="install-restify"></a>Installatie Restify

Vanaf de opdrachtregel u vanuit de map naar `azuread`. Als de `azuread` map niet bestaat, wordt deze hebt gemaakt.

`cd azuread`of`mkdir azuread;`

Voer de volgende opdracht uit:

`npm install restify`

Deze opdracht wordt Restify geïnstalleerd.

#### <a name="did-you-get-an-error"></a>U een foutmelding krijg?

In sommige besturingssystemen, wanneer u `npm`, verschijnt de fout `Error: EPERM, chmod '/usr/local/bin/..'` en een nieuw vergaderverzoek dat u het account als beheerder uitvoeren. Als dit probleem optreedt, gebruikt u de `sudo` opdracht om uit te voeren `npm` op een hoger bevoegdhedenniveau.

#### <a name="did-you-get-a-dtrace-error"></a>Hebt u een fout DTrace hier?

Wordt er ongeveer zo deze tekst tijdens de installatie van Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify biedt een krachtige methode voor het traceren van REST-oproepen via DTrace. Veel besturingssystemen hebben echter geen DTrace beschikbaar. U kunt deze fouten negeren.

De uitvoer van de opdracht er ongeveer als deze tekst:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Een installatiefout met Passport in uw web API

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is.

Met de volgende opdracht Passport installeren:

`npm install passport`

De uitvoer van de opdracht moet er ongeveer als deze tekst:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Passport-azuread toevoegen aan uw web API

Voeg nu het OAuth-strategie met behulp van `passport-azuread`, een reeks strategieën die verbinding maken met Azure AD met Passport. Gebruik deze strategie voor vruchtdragende tokens in de steekproef REST API.

> [AZURE.NOTE] Maar OAuth2 een kader waarin alle bekende token type kan worden verleend biedt, hebben alleen bepaalde token typen veel gebruik ervaring. De tokens voor het beschermen van eindpunten zijn vruchtdragende tokens. Dit soort van tokens zijn de meest afgegeven OAuth2. Veel implementaties wordt ervan uitgegaan dat vruchtdragende tokens het enige type token uitgegeven zijn.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is.

Installeer het paspoort `passport-azure-ad` module met de volgende opdracht:

`npm install passport-azure-ad`

De uitvoer van de opdracht moet er ongeveer als deze tekst:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>MongoDB modules toevoegen aan uw web API

In dit voorbeeld gebruikt MongoDB als uw gegevensopslag. Voor die Mongoose, een veelgebruikte installeren invoegtoepassing voor het beheren van modellen en schema's.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Aanvullende modules installeren

Vervolgens installeert u de resterende vereiste modules.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Installeer de modules in uw `node_modules` directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Een bestand server.js maken met uw afhankelijkheden

De `server.js` bestand biedt de meeste van de functionaliteit voor uw Web API-server. 

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Maak een `server.js` bestand in een editor. Voeg de volgende informatie:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Sla het bestand. U hiernaar terugkeert later.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Maak een bestand config.js om op te slaan de Azure AD-instellingen

Dit codebestand de configuratie parameters uit uw Azure AD-Portal naar de `Passport.js` bestand. U kunt deze configuratiewaarden gemaakt wanneer u het web API toegevoegd bij de portal in het eerste deel van het overzicht. We wordt uitgelegd wat er in de waarden van deze parameters nadat u de code hebt gekopieerd.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Maak een `config.js` bestand in een editor. Voeg de volgende informatie:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Vereiste waarden

`clientID`: De client-ID van uw Web API-toepassing.

`IdentityMetadata`: Dit is waar `passport-azure-ad` Hiermee wordt gezocht naar uw configuratiegegevens voor de identiteitsprovider. Het lijkt erop ook voor de pijltoetsen om de JSON web tokens te valideren. 

`audience`: De uniform resource identifier (URI) van de portal waarmee uw bellen-toepassing. 

`tenantName`: De naam van uw tenant (bijvoorbeeld **contoso.onmicrosoft.com**).

`policyName`: Het beleid dat u de tokens eraan wilt uw server valideren. Dit beleid moeten hetzelfde beleid dat u op de clienttoepassing voor aanmelden.

> [AZURE.NOTE] Voor onze preview B2C gebruik van het dezelfde beleid over zowel clients en servers-instelling. Als u hebt al een overzicht voltooid en dit beleid hebt gemaakt, moet u niet opnieuw doen. Omdat u het overzicht hebt voltooid, moet u niet mag nieuw beleid voor de zelfstudies client instellen op de site.

## <a name="add-configuration-to-your-serverjs-file"></a>Configuratie aan uw bestand server.js toevoegen

Om te lezen van de waarden uit de `config.js` bestand dat u hebt gemaakt, voegt u de `.config` opslaan als een vereiste bron in uw toepassing en stel de globale variabelen die in de `config.js` document.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Open de `server.js` bestand in een editor. Voeg de volgende informatie:

```Javascript
var config = require('./config');
```
Een nieuwe sectie toevoegen `server.js` die bevat de volgende code:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Vervolgens enkele tijdelijke aanduidingen voor de gebruikers die we van onze bellen toepassingen ontvangen toevoegen.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

We gaan nu en onze logboek te maken.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Gegevens over de MongoDB model en schema toevoegen met behulp van Mongoose

De voorbereiding van de eerdere betaalt uitschakelen als u deze drie bestanden samen in een REST API-service weer.

Voor dit overzicht, gebruikt u MongoDB voor de opslag van uw taken, zoals eerder is beschreven.

In de `config.js` bestand dat u uw database **tasklist**genoemd. Die naam is ook wat u opnemen aan het einde van de `mongoose_auth_local` verbindings-URL. U hoeft te maken van deze database vooraf in MongoDB. Het de database voor u gemaakt op de eerste uitvoering van de server-toepassing.

Nadat u de server welke MongoDB database moet worden gebruikt zien, moet u enkele extra code als u wilt maken van de model en het schema voor uw servertaken schrijven.

### <a name="expand-the-model"></a>Vouw het model

Dit model schema is eenvoudig. Desgewenst kunt u deze uitbreiden.

`owner`: Die is toegewezen aan de taak. Dit object is een **tekenreeks**.  

`Text`: De taak zelf. Dit object is een **tekenreeks**.

`date`: De datum waarop de taak moet voltooid zijn. Dit object is een **datum/tijd**.

`completed`: Als de taak voltooid is. Dit object is een **Booleaanse**.

### <a name="create-the-schema-in-the-code"></a>Het schema maken in de code

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Open de `server.js` bestand in een editor. De volgende informatie onder het item configuratie toevoegen:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Maakt u eerst het schema en maakt u een object model dat u gebruikt voor de opslag van uw gegevens overal in de code, wanneer u uw **routes**definieert.

## <a name="add-routes-for-your-rest-api-task-server"></a>Routes voor de REST API taak server toevoegen

U hebt nu een databasemodel voor gebruik met, toevoegen de routes die u voor de REST API-server gebruikt.

### <a name="about-routes-in-restify"></a>Over routes in Restify

Routes werken in Restify op dezelfde manier waarop ze werken wanneer ze de stapel Express gebruiken. U definiëren routes met behulp van de URI dat u verwacht dat de clienttoepassingen om te bellen. 

Een typische patroon voor de route van een Restify luidt als volgt:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify en Express kunt veel grondigere functionaliteit bieden, zoals toepassingstypen definiëren en manieren complexe routeren tussen verschillende eindpunten. Voor de toepassing van deze zelfstudie Houd we deze routes eenvoudig.

#### <a name="add-default-routes-to-your-server"></a>Standaardroutes toevoegen aan uw server

U kunt nu de eenvoudige CRUD-routes van **maken** en de **lijst** toevoegen voor onze REST API. Andere routes vindt u in de `complete` tak van de steekproef.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

Open de `server.js` bestand in een editor. Hieronder toevoegen de items in de database die u hebt aangebracht boven de volgende informatie:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Foutverwerking voor de routes toevoegen

Voeg enkele foutverwerking zodat u eventuele problemen kan communiceren terug naar de client op een manier die deze kan begrijpen.

Voeg de volgende code:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Maak uw server

U hebt nu gedefinieerd van uw database en zet uw routes op hun plaats staan. Het laatste wat u moet doen is toe te voegen van de server-instantie die worden beheerd door uw oproepen worden gedaan.

Restify en Express installeren uitgebreide aanpassingen voor een REST API-server, maar we het meest basisinstellingen hier gebruiken. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>De routes toevoegen aan de server (zonder verificatie)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Verificatie toevoegen aan uw server REST API

Nu dat u een actieve REST API-server hebt, kunt u deze handige ten opzichte van Azure AD maken.

Vanaf de opdrachtregel u vanuit de map naar `azuread`, als dit nog niet is is:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Gebruik de OIDCBearerStrategy die deel uitmaakt van passport-azure-ad


> [AZURE.TIP]
Wanneer u API's schrijft, moet u altijd de gegevens koppelen aan een unieke van het token die de gebruiker kan geen nabootsen. Wanneer de server worden items die moeten worden opgeslagen, gebeurt dit dus op basis van de **oid** van de gebruiker in het token (genoemd tot en met token.oid), waardoor u in het veld "eigenaar gaat". Deze waarde zorgt ervoor dat alleen die gebruiker toegang hebben tot hun eigen items die moeten worden. Er is geen weergeven in de API van 'eigenaar', zodat een externe gebruiker ook zelf aanvragen anderen items die moeten worden kunt zelfs als ze worden geverifieerd.

Gebruik vervolgens de vruchtdragende strategie die wordt geleverd met `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Hetzelfde patroon wordt gebruikt voor alle bijbehorende strategieën Passport. Als u gebruikmaakt van een `function()` die heeft `token` en `done` als parameters. De strategie terugkomt u nadat deze al haar werk heeft gedaan. U moet vervolgens opslaan van de gebruiker en de token opslaan, zodat u niet nodig hebt om te vragen om deze opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code gaat elke gebruiker die er gebeurt om te verifiëren met de server. Dit proces is autoregistration genoemd. In productieservers, laat niet in alle gebruikers toegang tot de API zonder eerste ze leest u over een registratieproces. Dit proces is meestal het patroon dat u in consumenten-apps waarmee u kunt met behulp van Facebook hebt geregistreerd, maar vervolgens wordt u gevraagd te Vul aanvullende informatie te zien. Als u dit programma niet een opdrachtregel programma hebt, kan hebben we het e-mailbericht opgehaald uit het token object dat wordt geretourneerd en vervolgens gevraagd gebruikers om aanvullende informatie in te vullen. Omdat dit een voorbeeld is, wordt deze toevoegen aan een database in het geheugen.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>De servertoepassing om te controleren of deze u weigert uitvoeren

U kunt `curl` om te zien als u nu OAuth2 bescherming tegen de eindpunten hebt. De koppen als resultaat gegeven moet genoeg te geven dat u zich op het juiste pad.

Zorg ervoor dat uw MongoDB exemplaar wordt uitgevoerd:

    $sudo mongodb

Ga naar de map en de server wordt uitgevoerd:

    $ cd azuread
    $ node server.js

Klik in een nieuwe terminal venster uitvoeren`curl`

Probeer een eenvoudige bericht:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Een 401 fout is het antwoord dat u wilt. Hiermee wordt aangegeven dat de laag Passport probeert te leiden naar het eindpunt autoriseren.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>U hebt nu een REST API-service die gebruikmaakt van OAuth2

U hebt een REST API geïmplementeerd via Restify en OAuth! U hebt nu voldoende code zodat u kunt doorgaan met het ontwikkelen van uw service en combineren in dit voorbeeld. U hebt nu ook als u met deze server kunt zonder een OAuth2-compatibele-client. Gebruik een extra overzicht zoals onze [verbinding maken met een web API met behulp van iOS met B2C](active-directory-b2c-devquickstarts-ios.md) Stapsgewijze instructies voor die stap.


## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar meer geavanceerde onderwerpen, zoals:

[Verbinding maken met een web API via iOS met B2C](active-directory-b2c-devquickstarts-ios.md)