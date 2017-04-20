<properties
    pageTitle="Node.js API-app in Azure App Service | Microsoft Azure"
    description="Informatie over het maken van een Node.js RESTful API en het dashboard implementeren naar een API-app in Azure App-Service."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Bouwen van een Node.js RESTful API en het dashboard implementeren naar een API-app in Azure wordt aangegeven

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Deze zelfstudie leert hoe u een eenvoudige [Node.js](http://nodejs.org) API maken en het dashboard implementeren naar een [API-app](app-service-api-apps-why-best-platform.md) in [Azure App-Service](../app-service/app-service-value-prop-what-is.md) met behulp van [cijfer](http://git-scm.com). U kunt een besturingssysteem dat Node.js kan worden uitgevoerd en u hebt al uw werk met hulpmiddelen voor de opdrachtregel zoals cmd.exe of bash.

## <a name="prerequisites"></a>Vereisten voor

1. Microsoft Azure-account (het[openen van een gratis account hier](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) geïnstalleerd (dit voorbeeld wordt ervan uitgegaan dat er Node.js versie 4.2.2)
2. [Cijfer](https://git-scm.com/) is geïnstalleerd
1. [GitHub](https://github.com/) -account

Tijdens het App-Service ondersteunt tal van manieren om te implementeren van uw code aan een API-app, wordt deze zelfstudie wordt aangegeven welke methode cijfer en wordt ervan uitgegaan dat er basiskennis van het werken met cijfer. Zie voor informatie over andere implementatiemethoden [Deploy uw app Azure App-Service](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Ophalen van de steekproef-code

1. Open een opdracht lijn-interface die Node.js en cijfer opdrachten kunt uitvoeren.

1. Navigeer naar een map die u voor een lokale cijfer opslagplaats en klonen de [GitHub-bibliotheek met de voorbeeldcode gebruiken kunt](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    De voorbeeld-API biedt twee eindpunten: een Get-aanvraag naar `/contacts` geeft als resultaat een lijst met namen en e-mailadressen in de indeling van JSON, terwijl `/contacts/{id}` retourneert alleen de geselecteerde contactpersoon.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (automatisch genereren) Node.js code op basis van Swagger metagegevens

[Swagger](http://swagger.io/) is een bestandsindeling voor metagegevens waarmee een RESTful API wordt beschreven. Azure App-Service heeft [ingebouwde ondersteuning voor Swagger metagegevens](app-service-api-metadata.md). In dit gedeelte van de zelfstudie model een API development-werkstroom waarin u Swagger metagegevens eerst maken en gebruiken die naar scaffold (automatisch genereren) servercode voor de API. 

>[AZURE.NOTE] Als u wilt weten hoe u scaffold Node.js code uit een bestand van de metagegevens Swagger niet wilt, kunt u dit gedeelte overslaan. Als u alleen steekproef code implementeren naar een nieuwe API-app wilt, gaat u rechtstreeks naar de sectie [maken een API-app in Azure wordt aangegeven](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Installeren en uitvoeren van Swaggerize

1. De volgende opdrachten voor het installeren van de **yo** **genereren swaggerize** NPM modules en globaal uitvoeren.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize is een functie die wordt gegenereerd servercode voor een API door een bestand met Swagger metagegevens beschreven. Het Swagger-bestand dat u wilt gebruiken met de naam *api.json* en bevindt zich in de map *starten* van de bibliotheek die u gekopieerd.

2. Navigeer naar de map *starten* en vervolgens uitvoeren de `yo swaggerize` opdracht. Swaggerize is een aantal vragen.  Voer "ContactList", voor het **pad naar het document swagger**, "api.json" voor **Wat u moet het bellen van dit project**, en typ 'express' voor **Express, tevreden, of Restify**.

        yo swaggerize

    ![Opdrachtregel swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Opmerking**: als u een fout in deze stap, de volgende stap wordt uitgelegd hoe u het probleem oplossen.

    Swaggerize Hiermee maakt u een map application bevindt, scaffolds handlers en van configuratiebestanden en genereert u een bestand **package.json** . De engine express weergave wordt gebruikt om de Swagger help-pagina.  

3. Als de `swaggerize` opdracht niet werkt met een 'onverwacht token' of "Ongeldige escape-reeks" fout, de oorzaak van de fout corrigeren door de gegenereerde *package.json* -bestand te bewerken. In de `regenerate` regel onder `scripts`, de backslash die voorafgaat aan *api.json* naar een slash wijzigen zodat de regel ziet er als volgt uit:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Navigeer naar de map waarin de scaffolded code (in dit geval de submap */start/ContactList* ).

1. Uitvoeren `npm install`.
    
        npm install
        
2. De **jsonpath** NPM module hebt geïnstalleerd. 

        npm install --save jsonpath
        
    ![Jsonpath installeren](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. De **swaggerize ui** NPM module hebt geïnstalleerd. 

        npm install --save swaggerize-ui
        
    ![Swaggerize Ui installeren](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>De scaffolded code aanpassen

1. Kopieer de map **bibliotheek** uit de map **starten** naar de map **ContactList** is gemaakt door de scaffolder. 

1. De code in het bestand **handlers/contacts.js** vervangen door de volgende code. 

    Deze code wordt gebruikt dat de JSON-gegevens die zijn opgeslagen in het bestand **lib/contacts.json** die wordt aangeboden door **lib/contactRepository.js**. De nieuwe contacts.js code reageert op HTTP-aanvragen voor het ophalen van alle contactpersonen en ze geretourneerd als een nettolading JSON. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. De code in het bestand **handlers/contacts/{id}.js** vervangen door de code fofllowing. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. De code in **server.js** vervangen door de volgende code. 

    De wijzigingen hebt aangebracht naar het bestand server.js zijn sectienummers met behulp van opmerkingen, zodat u kunt de wijzigingen zien. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Met de API lokaal uitgevoerd testen

1. Activeer de server met behulp van het uitvoerbare bestand voor de opdrachtregel van Node.js. 

        node server.js

1. Wanneer u naar **http://localhost:8000/contactpersonen zoeken**, u ziet dat de JSON-uitvoer van de lijst met contactpersonen (of u wordt gevraagd om deze te downloaden, afhankelijk van uw browser). 

    ![Alle contactpersonen Api-oproep](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Wanneer u naar **contactpersonen-http://localhost:8000/2 zoeken**, ziet u het contact dat wordt aangeduid met die id-waarde.

    ![Specifieke contactpersonen Api-oproep](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. De JSON Swagger gegevens is via het **eindpunt/swagger** served:

    ![Swagger Json met contactpersonen](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. De gebruikersinterface Swagger wordt geleverd via het **product** -eindpunt. In de gebruikersinterface Swagger, kunt u de uitgebreide functies voor HTML-client naar test u uw API.

    ![Swagger Ui](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Een nieuwe API-App maken

In deze sectie met de portal van Azure kunt u een nieuwe API-App maakt in Azure wordt aangegeven. Deze app API staat voor de berekeningscluster resources die Azure, vindt u in als uw code wilt uitvoeren. In de volgende onderdelen hebt u uw code dashboard implementeren naar de nieuwe API-app.

1. Blader naar de [Azure-Portal](https://portal.azure.com/). 

1. Klik op **Nieuw > Web + Mobile > API App**. 

    ![Nieuwe API-app in de portal](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Voer een **App-naam** die uniek is in het domein *azurewebsites.net* , zoals NodejsAPIApp plus een getal tot deze uniek. 

    Als de naam is bijvoorbeeld `NodejsAPIApp`, u kunt de URL van de `nodejsapiapp.azurewebsites.net`.

    Als u een naam die iemand anders al gebruikt invoeren heeft, ziet u een rood uitroepteken naar rechts.

6. In de vervolgkeuzelijst **Resourcegroep** op **Nieuw**en voer vervolgens in **nieuwe resource groepsnaam** "NodejsAPIAppGroup" of een andere naam als u liever. 

    Een [resourcegroep](../azure-resource-manager/resource-group-overview.md) is een verzameling Azure bronnen zoals API-apps, databases en VMs. Voor deze zelfstudie is het beste een nieuwe resourcegroep maken omdat die kunt u heel gemakkelijk verwijderen in één stap alle Azure bronnen die u voor de zelfstudie maken.

4. Klik op **App-Service plannen/locatie**en klik vervolgens op **Nieuw**.

    ![App-abonnement maken](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    In de volgende stappen uit maakt u een App Service-plan voor de nieuwe resourcegroep. Een App-serviceplan Hiermee geeft u de berekeningscluster bronnen die op uw API-app wordt uitgevoerd. Bijvoorbeeld als u de gratis laag kiest, uw API-app wordt uitgevoerd op gedeelde VMs, terwijl voor sommige betaalde lagen wordt uitgevoerd op speciale VMs. Zie [overzicht van de App Service-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)voor informatie over App Service-abonnementen.

5. Voer "NodejsAPIAppPlan" of een andere naam als u liever in het blad **App-Service plannen** .

5. Kies de locatie die zich het dichtst bij u in de vervolgkeuzelijst **locatie** .

    Deze instelling bepaalt welke Azure datacenter van de app wordt uitgevoerd in. Voor deze zelfstudie kunt u elke regio selecteren en deze won't Maak een opvallend verschil. Maar voor een app productie die u wilt uw server worden zo dicht mogelijk aan de clients die gebruikmaken van deze [Latentie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimaliseren.

5. Klik op **prijzen laag > alle weergave > F1 gratis**.

    Voor deze zelfstudie biedt de gratis prijzen laag voldoende prestaties.

    ![Selecteer gratis prijzen van laag](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. Klik op **OK**in het blad **App-Service plannen** .

7. Klik in het blad **API-App** op **maken**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Uw nieuwe API-app voor cijfer implementatie instellen

U kunt uw code bij de app API implementeren door te drukken doorvoeracties naar een bibliotheek cijfer in Azure App-Service. In dit gedeelte van de zelfstudie maakt u de referenties en cijfer opslagplaats in Azure wordt aangegeven dat u voor implementatie gebruikt.  

1. Nadat uw API-app is gemaakt, klikt u op **App Services > {uw API-app}** vanaf de startpagina van de portal. 

    De portal wordt weergegeven de bladen **API-App** en **Instellingen** .

    ![Portal API-app en instellingen blade](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. Schuif omlaag naar de sectie **publicatie** in het blad **Instellingen** en klik vervolgens op **implementatie referenties**.
 
3. Voer een gebruikersnaam en wachtwoord in het blad **implementatie referenties instellen** en klik vervolgens op **Opslaan**.

    Deze referenties gebruikt u voor het publiceren van uw code Node.js naar uw API-app. 

    ![Referenties voor implementatie](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Klik in het blad **Instellingen** op **implementatie bron > bron kiezen > lokale cijfer opslagplaats**, klikt u op **OK**.

    ![Cijfer cessies‑retrocessies maken](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Wanneer u uw bibliotheek cijfer de wijzigingen blade zodat u uw actieve implementaties hebt gemaakt. Aangezien de opslagplaats nieuw is, hebt u geen actieve installaties in de lijst. 

    ![Geen actieve installaties](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopieer de URL van de bibliotheek cijfer. Klik hiertoe Ga naar het blad voor de nieuwe API-App en kijkt u naar de sectie **Essentials** van het blad. Zoals u ziet het **cijfer klonen URL** in de sectie **Essentials** . Wanneer u de muisaanwijzer op deze URL, ziet u een pictogram aan de rechterkant die wordt de URL naar het Klembord kopiëren. Klik op het pictogram om de URL kopiëren.

    ![De Url cijfer krijgen van de Portal](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Opmerking**: moet u de URL in het volgende gedeelte Controleer dus of opslaan ergens voor het moment dat cijfer-klonen.

Nu dat u een API-App met een cijfer opslagplaats back-ups van hebt, kunt u de code push naar de bibliotheek de code bij de app API-implementatie. 

## <a name="deploy-your-api-code-to-azure"></a>Uw API-code implementeren naar Azure

In dit gedeelte maakt u een lokale cijfer opslagplaats waarin uw servercode voor de API bevat en klikt u uw code push uit die opslagplaats naar de bibliotheek in Azure wordt aangegeven dat u eerder hebt gemaakt.

1. Kopie de `ContactList` map naar een locatie die u voor een nieuwe lokale cijfer opslagplaats gebruiken kunt. Als u het eerste deel van de zelfstudie hebt gedaan, kopieert u `ContactList` uit de `start` map. anders kopiëren `ContactList` uit de `end` map.

1. In de tool opdrachtregel Navigeer naar de nieuwe map en voer de volgende opdracht uit om te maken van een nieuwe lokale cijfer opslagplaats. 

        git init

     ![Nieuwe lokale cijfer cessies‑retrocessies](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Voer de volgende opdracht om toe te voegen een cijfer externe voor uw API-app-bibliotheek. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Opmerking**: de tekenreeks 'YOUR_GIT_CLONE_URL_HERE' vervangen door uw eigen cijfer klonen URL die u eerder hebt gekopieerd. 

1. De volgende opdrachten voor het maken van een doorvoeren met alle code uitvoeren. 

        git add .
        git commit -m "initial revision"

    ![Cijfer doorvoeren uitvoer](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. De opdracht om uw code push naar Azure uitvoeren. Wanneer u wordt gevraagd om een wachtwoord, voert u de fase die u eerder in de portal van Azure gemaakt.

        git push azure master

    Hierdoor wordt een implementatie naar uw API-app.  

1. Ga terug naar het blad **implementaties** voor uw app API en u kunt zien dat de implementatie optreedt in uw browser. 

    ![Implementatie probleem optreedt](media/app-service-api-nodejs-api-app/deployment-happening.png)

    De opdracht lijn interface weerspiegelt tegelijk, de status van uw implementatie terwijl deze plaatsvindt. 

    ![Foutbericht gegeven knooppunt Js-implementatie](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Wanneer de implementatie is voltooid, weerspiegelt het blad **implementaties** de implementatie van uw codewijzigingen in uw API-App. 

## <a name="test-with-the-api-running-in-azure"></a>Met de API uitgevoerd in Azure testen
 
3. Kopieer de **URL** in het gedeelte **Essentials** van uw blade API-App. 

    ![Implementatie voltooid](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. De URL van uw contactpersonen API-aanroep, dat wil zeggen een REST API-client zoals Postman of Fiddler (of uw webbrowser), geef de `/contacts` eindpunt van de API-app. U kunt de URL van de`https://{your API app name}.azurewebsites.net/contacts`

    Als u een GET-aanvraag naar dit eindpunt verzendt, krijgt u de JSON-uitvoer van de API-app.

    ![Postman Api raken](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. In een browser, gaat u naar de `/docs` eindpunt van de gebruikersinterface Swagger uitproberen, als dit wordt uitgevoerd in Azure wordt aangegeven.

U kunt nu dat er continu bezorging wired omhoog, codewijzigingen aanbrengen en ze implementeren naar Azure door gewoon te doorvoeracties naar uw bibliotheek Azure cijfer te drukken.

## <a name="next-steps"></a>Volgende stappen

U hebt nu wel een API-App hebt gemaakt en Node.js API code geïmplementeerd in deze. Het volgende zelfstudie wordt getoond hoe [API-apps van JavaScript-clients, met CORS](app-service-api-cors-consume-javascript.md)in beslag neemt.
