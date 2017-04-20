<properties
    pageTitle="Azure AD NodeJS aan de slag | Microsoft Azure"
    description="Het maken van een Node.js REST Web API werkt naadloos met Azure AD voor verificatie samen."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="getting-started-with-web-api-for-node"></a>Aan de slag met WEB-API voor knooppunt

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Passport** is verificatie middleware voor Node.js. Zeer flexibel en modulaire, Passport kan worden onopvallend verwijderd voor elk Express- of Resitify webtoepassing. Een uitgebreide reeks strategieën ondersteuning voor verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We dit module hebt geïnstalleerd en voegt u de Microsoft Azure Active Directory `passport-azure-ad` invoegtoepassing.

Hiervoor moet u:

1. Een toepassing met Azure AD registreren
2. Uw app instellen voor gebruik in Passport azure-ad-combinatie invoegtoepassing.
3. Een clienttoepassing om te bellen van de naar doen lijst Web API configureren

De code voor deze zelfstudie worden [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi)bijgehouden.

> [AZURE.NOTE] In dit artikel besproken niet hoe u implementeert aanmeldingsproblemen, aanmelding en Profielbeheer met Azure AD B2C.  Dit is gericht op bellen web API's nadat de gebruiker al is geverifieerd.  Als u nog niet is gedaan, moet u beginnen met het [integreren met Azure Active Directory-document](./develop/active-directory-how-to-integrate.md) voor meer informatie over de basisbeginselen van Azure Active Directory.


We alle de broncode voor dit actieve voorbeeld in GitHub onder een MIT-licentie hebt uitgebracht, dus je mag rustig klonen (of beter nog, zich splitsen!) en feedback geven en aanvragen halen.

## <a name="about-nodejs-modules"></a>Over Node.js Modules

We gebruiken Node.js modules in dit scenario. Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. U meestal modules installeren met behulp van het hulpprogramma voor de Node.js NPM in de NPM-installatiemap, maar sommige modules, zoals de HTTP-module, worden opgenomen het core Node.js-pakket.
Geïnstalleerde modules worden opgeslagen in de map node_modules in de hoofdmap van uw Node.js-installatiemap. Elke module in de adreslijst node_modules onderhoudt een eigen node_modules-map waarin alle modules waarvan deze afhankelijk en elke vereiste module heeft een node_modules-map. Deze directory recursieve structuur geeft de afhankelijkheidsketen.

Deze afhankelijkheid ketting structuur resulteert in een grotere footprint van toepassing, maar dit zorgt ervoor dat alle afhankelijkheden wordt voldaan en dat de versie van de modules gebruikt in de ontwikkelingsfase bevindt ook worden gebruikt in productie. Dit zorgt ervoor dat het gedrag van de app productie bieden en voorkomt versiebeheer problemen die mogelijk van invloed op gebruikers.

## <a name="1-register-a-azure-ad-tenant"></a>1. een Azure AD-Tenant registreren

Als u wilt gebruiken in dit voorbeeld moet u een Azure Active Directory-Tenant. Als u niet zeker wat een tenant is of hoe u een krijgt, raadpleegt u [hoe u een Azure AD-tenant](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2. maken van een toepassing

U moet nu een app maken in uw adreslijst, waarmee Azure AD bepaalde gegevens die zijn vereist voor het veilig communiceren met uw app.  Zowel de client-app en de web API wordt aangegeven door een enkel **Toepassings-ID** in dit geval, aangezien ze bestaan uit één logische app.  Volg [deze instructies](active-directory-how-applications-are-added.md)om een app maken. Als u een lijn of Business app [Deze aanvullende instructies zijn mogelijk nuttig](active-directory-applications-guiding-developers-for-lob-applications.md).

Zorg ervoor dat:

- Meld u aan bij de Portal Azure Management.
- Klik in de navigatiebalk linkerpagina op **Active Directory**.
- Selecteer de tenant waar u wilt registreren van de toepassing.
- Klik op het tabblad **toepassingen** en klik op toevoegen in de onder-vel.
- Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
    - De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    - De **URL voor eenmalige aanmelding** is de basis-URL van uw app.  De standaardinstelling van de code van de steekproef is `https://localhost:8080`.
    - De **App-ID-URI** is een unieke id voor uw toepassing.  De overeenkomst is via `https://<tenant-domain>/<app-name>`, bijvoorbeeld`https://contoso.onmicrosoft.com/my-first-aad-app`
- Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad configureren.

- Overzicht: een **Toepassing geheim** voor uw toepassing maken en deze omlaag te kopiëren.  U moet deze binnenkort.
- HERINNERING: De kopie van omlaag de **Toepassings-ID** die is toegewezen aan uw app.  U moet ook deze binnenkort.


## <a name="3-download-nodejs-for-your-platform"></a>3. node.js voor uw platform downloaden
Als u wilt gebruiken in dit voorbeeld, moet u een werkende installatie van Node.js hebben.

Installeer Node.js vanaf [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4. MongoDB installeren op uw platform

Als u wilt gebruiken in dit voorbeeld, moet u een werkende installatie van MongoDB hebben. MongoDB gebruiken we onze persistant REST API zodat alle werkstroomexemplaren van de server.

Installeer MongoDB vanaf [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] In dit scenario wordt ervan uitgegaan dat u de standaard-installatie- en server eindpunten gebruiken voor MongoDB, namelijk op het moment van deze schrijven: mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. de modules Restify in installeren naar uw Web-API

We gebruiken Resitfy onze REST API maken. Restify afkomstig is van een minimale en flexibele Node.js toepassing kader Express met een reeks robuuste functies voor het samenstellen van REST API's boven op verbinding maken.

### <a name="install-restify"></a>Installatie Restify

Wijzig de mappen naar de map azuread vanaf de opdrachtregel. Als de map **azuread** niet bestaat, maakt u er.

`cd azuread - or- mkdir azuread; cd azuread`

Typ de volgende opdracht uit:

`npm install restify`

Deze opdracht wordt Restify geïnstalleerd.

#### <a name="did-you-get-an-error"></a>U EEN FOUTMELDING KRIJG?

Wanneer u met npm op enkele besturingssystemen, kan er een foutbericht van fout: EPERM, type chmod ' / usr/lokale/opslaglocatie /..' en een verzoek om te proberen het account uitvoeren als beheerder. Als dit gebeurt, gebruikt u de opdracht sudo npm uitvoeren op een hoger bevoegdhedenniveau.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>HEBT U EEN FOUT MET BETREKKING TOT DTRACE HIER?

Wordt er ongeveer zo uitziet tijdens de installatie van Restify:

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


Restify krachtige techniek wilt traceren REST oproepen DTrace gebruiken. Veel besturingssystemen hebben echter geen DTrace beschikbaar. U kunt deze fouten negeren.


De uitvoer van deze opdracht ziet er ongeveer als volgt uit:


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


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. Passport.js in installeren op uw Web API

[Passport](http://passportjs.org/) is verificatie middleware voor Node.js. Zeer flexibel en modulaire, Passport kan worden onopvallend verwijderd voor elk Express- of Resitify webtoepassing. Een uitgebreide reeks strategieën ondersteuning voor verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Azure Active Directory. We dit module hebt geïnstalleerd en voegt u de invoegtoepassing Azure Active Directory-strategie toe.

Wijzig de mappen naar de map azuread vanaf de opdrachtregel.

Voer de volgende opdracht passport.js installeren

`npm install passport`

De uitvoer van de opdracht ziet er ongeveer als volgt uit:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7. Passport-Azure-AD toevoegen aan uw Web API

Vervolgens gaat toevoegen we de strategie OAuth passport-azuread, een reeks strategieën die verbinding maken met Azure Active Directory met Passport gebruiken. We gebruiken deze strategie voor vruchtdragende Tokens in dit voorbeeld Rest API.

> [AZURE.NOTE] Maar OAuth2 een kader waarin alle bekende token type kan worden verleend biedt, hebben uitgebreide gebruiken door alleen bepaalde token typen ervaring. Voor het beschermen eindpunten, die moeten vruchtdragende Tokens af heeft ingeschakeld. Vruchtdragende tokens zijn de meest uitgegeven sterk uiteen type token in OAuth2 en veel implementaties wordt ervan uitgegaan dat dat vruchtdragende tokens het enige type token uitgegeven zijn.

Vanaf de opdrachtregel door mappen te wijzigen naar de map azuread

Typ de volgende opdracht Passport.js passport-azure-ad-module installeren:

`npm install passport-azure-ad`

De uitvoer van de opdracht ziet er ongeveer als volgt uit:

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



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8. MongoDB modules toevoegen aan uw Web-API

We gebruiken MongoDB als onze gegevensopslag om die reden dan ook, moeten we installeren beide de gebruikte invoegtoepassing om te beheren-modellen en schema's genoemd Mongoose, alsmede het stuurprogramma voor MongoDB, ook wel genoemd MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9. aanvullende modules installeren

Vervolgens gaat we de overige vereiste modules installeren.


Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`


Voer de volgende opdrachten voor het installeren van de volgende modules in uw adreslijst node_modules:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10. een server.js maken met uw afhankelijkheden

Het bestand server.js zullen leveren om de meeste van onze functionaliteit voor onze Web API-server. We wilt meest van onze code toevoegen aan dit bestand. Voor productiedoeleinden zou u de functionaliteit in op kleinere bestanden, zoals verschillende routes en controllers refactoring. Voor deze demo we server.js gebruiken voor deze functie.

Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

Maak een `server.js` bestand in onze favoriete editor en voer de volgende gegevens:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Sla het bestand. We keert kort terug naar deze.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Maken van een configuratiebestand om op te slaan de Azure AD-instellingen

Dit codebestand geeft de configuratieparameters van de Azure Active Directory-Portal naar Passport.js. U kunt deze configuratiewaarden gemaakt wanneer u de Web API toegevoegd bij de portal in het eerste deel van de procedure. Wordt uitgelegd wat er in de waarden van deze parameters nadat u de code hebt gekopieerd.


Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

Maak een `config.js` bestand in onze favoriete editor en voer de volgende gegevens:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Sla het bestand.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. configuratie aan uw bestand server.js toevoegen

Moeten we deze waarden uit de Config-bestand dat u zojuist hebt gemaakt via de toepassing te lezen. Klik hiertoe we gewoon de .config-bestand toevoegen als een vereiste resource in onze-toepassing en stel vervolgens de globale variabelen die in het document config.js

Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voer de volgende gegevens:

```Javascript
var config = require('./config');
```
Voegt u een nieuwe sectie naar `server.js` met de volgende code:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Sla het bestand.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. de MongoDB Model en schemainformatie toe aan Moongoose toevoegen

Alle deze voorbereidende is nu gaat om te betalen uitschakelen als we liquidatie deze drie bestanden samen naar een REST API-service starten.

Voor deze procedure we gebruiken MongoDB voor de opslag van onze taken, zoals is beschreven in ***stap 4***.

Als u weet uit de `config.js` bestand die u hebt gemaakt in ***stap 11*** we onze database genoemd `tasklist` zoals die was wat er aan het einde van de URL van onze mogoose_auth_local verbinding opnemen. U hoeft te maken van deze database vooraf in MongoDB, wordt deze dit maken voor ons op de eerste uitvoeren van onze servertoepassing (ervan uitgaande dat deze nog niet bestaat).

Nu dat we de server welke MongoDB database willen we gebruiken servernamen hebt, moeten we enkele extra code als u wilt maken van de model en het schema voor taken van onze server schrijven.

#### <a name="discussion-of-the-model"></a>Discussie van het model

Onze model Schema is heel eenvoudig en daarop zoals vereist.

De naam van - de naam van wie de taak is toegewezen. Een ***tekenreeks***

TAAK - de taak zelf. Een ***tekenreeks***

DATUM: de datum waarop de taak moet voltooid zijn. Een ***datum/tijd***

VOLTOOID - als de taak is voltooid of niet. Een ***BOOLEAANSE***

#### <a name="creating-the-schema-in-the-code"></a>Het schema maken in de code


Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voer de volgende gegevens onder het item configuratie:

```Javascript
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    task: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Zoals u van de code ziet, we onze Schema maken en maak een model object die we gebruiken voor de opslag van onze gegevens overal in de code als we onze ***Routes***definieert.

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. onze Routes voor onze taak REST API-server toevoegen

Nu we hebben een databasemodel voor gebruik met, laten we toevoegen de routes die we voor onze REST API-server gebruiken.

### <a name="about-routes-in-restify"></a>Over Routes in Restify

Routes werken in Restify op precies dezelfde manier die ze doen met het gebruik van de stapel Express. U definiëren routes de URI dat u verwacht dat de clienttoepassingen om te bellen met. U meestal definiëren uw routes in een afzonderlijk bestand. Voor ons doel, wordt we onze routes in het bestand server.js geplaatst. Het is raadzaam factor deze naar hun eigen bestand voor gebruik in productieomgeving.

Een typische patroon voor de Route van een Restify luidt als volgt:

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


Dit is het patroon op het niveau van de belangrijkste. Resitfy (en Express) bieden veel grondigere functionaltiy zoals toepassingstypen definiëren en manieren complexe routeren tussen verschillende eindpunten. Wij gebruiken blijft we deze routes heel eenvoudig.

### <a name="1-add-default-routes-to-our-server"></a>1. standaardroutes aan onze server toevoegen

We nu toevoegen de eenvoudige CRUD-routes van maken, ophalen, bijwerken en verwijderen.

Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

Open uw `server.js` bestand in onze favoriete editor en voer de volgende gegevens onder de database-items die u hierboven hebt aangebracht:

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

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
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


// Delete a task by name

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

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

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
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

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2. volgende, laten we eens enkele foutafhandeling in onze API's toevoegen:

```

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


## <a name="15-create-your-server"></a>15. maken uw Server!

We onze database gedefinieerd hebben, zijn er onze routes op hun plaats staan en het laatste wat u moet doen is onze server-instantie waarmee onze oproepen beheert toevoegen.

Restify (en Express) een groot aantal uitgebreide aanpassing die u kunt doen voor een REST API-server, maar opnieuw we voor ons doel het meest basisinstellingen gebruiken.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
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
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. toevoegen de routes aan de server (zonder de verificatie voor nu)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. voordat we OAuth ondersteuning toevoegen, laten we de server wordt uitgevoerd.

Test u uw server voordat we verificatie toevoegen

De eenvoudigste manier hiervoor is met behulp van krul in een opdrachtregel. Voordat we dat doet, moeten we een eenvoudige hulpprogramma waarmee wij uitvoer als JSON parseren. Om dat te doen, installeert u het hulpmiddel json omdat de onderstaande voorbeelden die worden gebruikt.

`$npm install -g jsontool`

Hiermee installeert het hulpmiddel JSON globaal. Afspelen met de server nu dat we die – hebt uitgevoerd:

Controleer eerst of uw isntance monogoDB actief is...

`$sudo mongod`

Vervolgens Ga naar de map en start curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Vervolgens kunt we toevoegen een taak op deze manier:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

Het antwoord moet zijn:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
En wij kunt in de lijst taken voor Brandon deze manier:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Als dit werkt, zijn wij OAuth toevoegen aan de REST API-server.

**U beschikt over een REST API-server met MongoDB!**


## <a name="18-add-authentication-to-our-rest-api-server"></a>18. verificatie toevoegen aan onze REST API-Server

Nu dat we een actieve REST API hebben (Gefeliciteerd, btw!) We gaan naar nuttige ten opzichte van Azure AD.

Vanaf de opdrachtregel, mappen naar de map **azuread** als dat niet al er te wijzigen:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: de OIDCBearerStrategy die deel uitmaakt van passport-azure-ad gebruiken

Dusverre hebben we een typisch REST doen server zonder elk type autorisatie ingebouwd. Dit is waar we het samenstellen van die begint.

Eerst moeten we aangeven dat we wilt gebruik in combinatie met. Dit recht na de andere serverconfiguratie opnemen:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Bij het schrijven van API's moet u altijd de gegevens te vestigen unieke koppeling van het token die de gebruiker kan geen nabootsen. Als deze server worden items die moeten worden opgeslagen, wordt de opgeslagen ze op basis van de object-ID van de gebruiker in het token (genoemd tot en met token.oid) die we in het veld "eigenaar plaatsen". Dit zorgt ervoor dat alleen die gebruiker toegang heeft tot zijn taken en niemand anders toegang de taken die zijn ingevoerd tot hebben. Er is geen weergeven in de API van "eigenaar" zodat een externe gebruiker ook zelf van andere taken aanvragen kunt zelfs als ze worden geverifieerd.

Vervolgens gebruiken we de vruchtdragende strategie die wordt geleverd met passport-azure-ad. Kijk eens naar de code voorlopig, ik deze wordt uitgelegd. Plaats dit na wat u pated hierboven:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/

var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.sub === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var bearerStrategy = new BearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                users.push(token);
                owner = token.sub;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(bearerStrategy);
```

Een soortgelijke patroon Passport gebruikt voor alle is strategieën (Twitter, Facebook, enzovoort) die voor alle strategie schrijvers worden aangehouden. Bekijken via de strategie ziet u geven we een function() met een token en een gereed als de parameters. De strategie wordt Plichtsgetrouw keert u terug naar ons wanneer deze wordt het werk. Zodra de optie doet wilt we opslaan van de gebruiker en niet het token initialiseren zodat we niet nodig om aan te vragen om deze opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code gaat elke gebruiker die er gebeurt om te verifiëren met onze server. Dit wordt automatisch registratie genoemd. In productieservers wilt u wouldn't kiesopenbaar als iedereen zonder eerste ze leest u over een registratieproces die u hebt besloten. Dit is meestal het patroon dat u ziet in consumenten-apps die kunnen u met Facebook hebt geregistreerd, maar wordt u aanvullende informatie invullen gevraagd. Als u dit niet een opdrachtregel-programma hebt, kan hebben we alleen het e-mailbericht opgehaald uit het token object dat wordt geretourneerd en vervolgens te laten invullen aanvullende informatie gevraagd. Aangezien dit een testserver we gewoon de database in het geheugen toevoegen is.

### <a name="2-finally-protect-some-endpoints"></a>2. Klik tot slot beveiligen met een enkele eindpunten

U eindpunten beveiligen door het opgeven van de `passport.authenticate()` bellen met het protocol dat u wilt gebruiken.

Laten we onze route in onze servercode moet iets interessanter bewerken:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. uit uw servertoepassing opnieuw en zorg ervoor dat u weigert

Document met `curl` opnieuw in te we nu OAuth2 bescherming tegen onze eindpunten staan. We wordt dit doen voordat u gestart een van onze client SDK's op basis van dit eindpunt. De koppen als resultaat gegeven moet genoeg om ons te laten dat we zijn niet beschikbaar het juiste pad.

Controleer eerst of uw monogoDB exemplaar wordt uitgevoerd:

  $sudo mongod

Vervolgens Ga naar de map en start curling...

  $ cd azuread $ knooppunt server.js

Probeer een eenvoudige bericht:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Een 401 is het antwoord dat u hier zoekt, zoals die aangeeft dat de laag Passport probeert te leiden naar het eindpunt autoriseren, welke precies is wat u wilt.

## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Gefeliciteerd! U hebt een REST API-Service met OAuth2!

U hebt er fout is als kunt u met deze server zonder een compatibel OAuth2-client. U moet een extra scenario doorlopen.

Als u zijn slechts gastgebruikers voor informatie over het implementeren van een REST API Restify en OAuth2 gebruiken, hebt u meer dan voldoende code wilt behouden voor het ontwikkelen van uw service en te leren hoe om aan te vullen in dit voorbeeld.

Als u geïnteresseerd in de volgende stappen in de ADAL reis bent, zijn sommige ondersteunde ADAL clients wordt aangeraden om te blijven werken:

Gewoon Klonen naar beneden af op uw computer ontwikkelaars en configureren van de wijze beschreven in de stapsgewijze instructies.

[ADAL voor iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL voor Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
