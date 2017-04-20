<properties
    pageTitle="Aan de slag met Node.js WebApps in Azure App-Service"
    description="Leer hoe u een toepassing Node.js in een WebApp in Azure App Service implementeren."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Aan de slag met Node.js WebApps in Azure App-Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Deze zelfstudie wordt getoond hoe u een eenvoudige [Node.js] -toepassing maken en het dashboard implementeren naar [Azure App-Service] uit een opdrachtregel-omgeving, zoals cmd.exe of bash. De instructies in deze zelfstudie kunnen worden gevolgd op een besturingssysteem dat Node.js kan worden uitgevoerd.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Vereisten voor

- [Node.js]
- [Bower]
- [Yeoman]
- [Cijfer]
- [Azure CLI]
- Een Microsoft Azure-account. Als u geen account hebt, kunt u [registreren voor een gratis proefversie] of [activeren van de voordelen van uw Visual Studio-abonnee].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Maak en implementeer een eenvoudige Node.js web-app

1. Open de opdrachtregel terminal van uw keuze en de [genereren voor Yeoman Express]installeren.

        npm install -g generator-express

2. `CD`in een werkmap en zorgt u voor een express-app met de volgende syntaxis:

        yo express
        
    De volgende opties wanneer u wordt gevraagd kiezen:  

    `? Would you like to create a new directory for your project?`**Ja**  
    `? Enter directory name`**{toepassingsnaam}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Geen**  
    `? Select a database to use:`**Geen**  
    `? Select a build tool to use:`**Knorvis**

3. `CD`naar de hoofdmap van de nieuwe app en start deze om ervoor te zorgen dat deze wordt uitgevoerd in uw ontwikkelomgeving:

        npm start

    Ga in uw browser naar <http://localhost:3000> om ervoor te zorgen dat u de startpagina Express kunt zien. Nadat u uw app wordt uitgevoerd correct hebt bevestigd, gebruikt u `Ctrl-C` te stoppen.
    
1. Wijzigen in de modus ASM en meld u aan bij Azure (u moet [Azure CLI](#prereq)):

        azure config mode asm
        azure login

    Volg de vraag die u kunt doorgaan met de aanmelding in een browser met een Microsoft-account met uw Azure-abonnement.

2. Zorg ervoor dat u zich nog steeds in de hoofdmap van de app en klik maken van de resource van de app App Service in Azure wordt aangegeven met een unieke app-naam met de volgende opdracht. Bijvoorbeeld: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Volg de vraag om een Azure regio om te implementeren naar te selecteren. Als u nooit cijfer/FTP-implementatie referenties voor uw Azure-abonnement instellen hebt, moet u ook gevraagd om deze te maken.

3. De./config/config.js-bestand openen vanuit de hoofdsite van uw toepassing en wijzig de productie poort `process.env.port`; uw `production` eigenschap in de `config` object ziet er als het volgende voorbeeld:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Hiermee kunt uw Node.js app beantwoorden als u wilt dat iisnode met aanvragen op de standaardpoort naar webonderdelen luistert.
    
4. Open./package.json en voeg de `engines` eigenschap om op te [geven van de gewenste Node.js-versie](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Uw wijzigingen op te slaan en het gebruik van cijfer om uw app implementeren naar Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Het apparaat Express biedt al een bestand .gitignore, dus uw `git push` niet bandbreedte probeert te uploaden de node_modules / Active directory.

5. Tot slot starten uw live Azure-app in de browser:

        azure site browse

    U ziet nu uw web-app Node.js uitgevoerd live in Azure App-Service.
    
    ![Voorbeeld van bladeren naar de gedistribueerde toepassing.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Bijwerken van uw Node.js web-app

Als u updates naar uw Node.js WebApp met App-Service, alleen uitvoeren `git add`, `git commit`, en `git push` zoals wanneer u uw web-app voor het eerst geïmplementeerd.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Hoe App Service uw app Node.js implementeren

Azure App-Service wordt [iisnode] gebruikt om uit te voeren Node.js apps. De CLI Azure en de Kudu-engine (cijfer-implementatie) samenwerken zodat u een gestroomlijnde ervaring bij het ontwikkelen en implementeren van Node.js apps vanaf de opdrachtregel. 

- `azure site create --git`de algemene Node.js patroon van server.js of app.js herkent en maakt een iisnode.yml in de hoofdmap. U kunt dit bestand gebruiken om aan te passen iisnode.
- Bij `git push azure master`, Kudu automatiseert de volgende implementatietaken:

    - Als package.json in de hoofdmap van de bibliotheek is, voert u `npm install --production`.
    - Genereer een Web.config voor iisnode die naar het begin script in package.json (bijvoorbeeld server.js of app.js verwijst).
    - Pas Web.config als u wilt uw app voor foutopsporing met knooppunt controle bent u er klaar.
    
## <a name="use-a-nodejs-framework"></a>Een kader Node.js gebruiken

Als u een populaire Node.js framework, zoals [Sails.js] [ SAILSJS] of [MEAN.js] [ MEANJS] voor het ontwikkelen van apps, kunt u de App Service implementeren. Populaire Node.js kaders hebt hun specifieke quirks en bijbehorende pakketafhankelijkheden behouden bijgewerkt. App-Service is echter de logboeken stdout en stderr beschikbaar, zodat u kunt weten precies wat er gebeurt met uw app en breng de wijzigingen dienovereenkomstig gewijzigd. Zie [stdout en stderr aanmeldingslogboeken uit iisnode ophalen](#iisnodelog)voor meer informatie.

De volgende zelfstudies wordt uitgelegd hoe u werken met een specifieke framework in App Service:

- [Een WebApp Sails.js implementeren naar Azure App-Service]
- [Een Node.js chat-toepassing bouwen met Socket.IO in Azure App-Service]
- [Het gebruik van io.js met Azure App Service Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Een specifieke Node.js-engine gebruiken

In de normale werkstroom, moet u App Service naar een specifieke Node.js-engine gebruiken zoals u gewend in package.json bent zien.
Bijvoorbeeld:

    "engines": {
        "node": "6.6.0"
    }, 

De Kudu implementatie-engine bepaalt welke engine Node.js gebruikt in de volgende volgorde:

- Bekijk eerst iisnode.yml om te zien of `nodeProcessCommandLine` is opgegeven. Indien Ja: gebruik vervolgens die.
- Bekijk vervolgens package.json om te zien of `"node": "..."` is opgegeven in de `engines` object. Indien Ja: gebruik vervolgens die.
- Kies een standaardversie Node.js al dan niet standaard.

>[AZURE.NOTE] Het wordt aanbevolen dat u de gewenste Node.js engine expliciet definiëren. De standaardversie van Node.js kunt wijzigen en mogelijk dat u fouten in uw Azure WebApp omdat de standaardversie Node.js niet geschikt voor uw app is.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Stdout en stderr aanmeldingslogboeken ophalen uit iisnode

Voer de volgende stappen uit als u wilt lezen iisnode Logboeken.

> [AZURE.NOTE] Nadat u deze stappen uitvoert, logboekbestanden bestaat mogelijk niet totdat er een fout optreedt.

1. Open het bestand iisnode.yml vindt u de CLI Azure.

2. De twee volgende parameters instellen: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Samen en ze iisnode zien in de App-Service in moeten worden geplaatst de uitvoer stdout en stderror de D:\home\site\wwwroot\**iisnode** directory.

3. Uw wijzigingen opslaan en vervolgens de gewenste wijzigingen aan Azure push met de volgende cijfer opdrachten:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode is nu geconfigureerd. De volgende stappen hoe u toegang tot deze logboeken.
     
4. Toegang tot de Kudu Foutopsporingsconsole voor de app, dat wil zeggen op in uw browser:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Deze URL verschilt van de URL van de web-app door toevoeging van "*.scm.*" de DNS-naam. Als u de URL in dat deze weglaat, wordt er een 404-fout.

5. Navigeer naar D:\home\site\wwwroot\iisnode

    ![Navigeren naar de locatie van de logboekbestanden iisnode.][iislog-kudu-console-find]

6. Klik op het pictogram **bewerken** voor het logboek die u wilt lezen. U kunt ook klikken op **downloaden** of **verwijderen** als u wilt.

    ![Een logboekbestand iisnode te openen.][iislog-kudu-console-open]

    Nu ziet u het logboek kunt u fouten opsporen in uw App Service-implementatie.
    
    ![Een logboekbestand iisnode onderzoeken.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Fouten opsporen in uw app met knooppunt-controle

Als u knooppunt-controle voor foutopsporing van uw apps Node.js gebruikt, kunt u deze kunt gebruiken voor uw live App Service-app. Knooppunt-controle is vooraf geïnstalleerd in de installatie iisnode voor App-Service. En als u via cijfer implementeert, de automatisch gegenereerde Web.config uit Kudu al bevat alle de configuratie moet u knooppunt-controle inschakelen.

Schakel knooppunt controle door de volgende stappen uit:

1. Open iisnode.yml in de hoofdmap van uw bibliotheek en de volgende parameters opgeven: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Uw wijzigingen opslaan en vervolgens de gewenste wijzigingen aan Azure push met de volgende cijfer opdrachten:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nu alleen Ga naar van uw app starten bestand als opgegeven door het script starten in uw package.json, met/Debug toegevoegd aan de URL. Bijvoorbeeld:

        http://{appname}.azurewebsites.net/server.js/debug
    
    Of,
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Meer informatiebronnen

- [Een versie Node.js geven in een Azure-toepassing](../nodejs-specify-node-version-azure-apps.md)
- [Aanbevolen procedures en gids voor probleemoplossing voor Node.js toepassingen op Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Hoe u fouten opsporen in een Node.js web-app in Azure App-Service](web-sites-nodejs-debug.md)
- [Gebruik van Node.js Modules met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
- [Azure App-Service-WebApps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Developer Center](/develop/nodejs/)
- [Aan de slag met WebApps in Azure App-Service](app-service-web-get-started.md)
- [De geweldig geheime Kudu Foutopsporingsconsole verkennen]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[de voordelen van uw Visual Studio-abonnee activeren]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Een Node.js chat-toepassing bouwen met Socket.IO in Azure App-Service]: ./web-sites-nodejs-chat-app-socketio.md
[Een WebApp Sails.js implementeren naar Azure App-Service]: ./app-service-web-nodejs-sails.md
[De geweldig geheime Kudu Foutopsporingsconsole verkennen]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express genereren voor Yeoman]: https://github.com/petecoop/generator-express
[Cijfer]: http://www.git-scm.com/downloads
[Het gebruik van io.js met Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[registreren voor een gratis proefversie]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
