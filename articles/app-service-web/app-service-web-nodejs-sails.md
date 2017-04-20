<properties
    pageTitle="Een WebApp Sails.js implementeren naar Azure App-Service"
    description="Leer hoe u een toepassing Node.js Azure App Service implementeren. Deze zelfstudie ziet u hoe u een Sails.js web-app implementeren."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Een WebApp Sails.js implementeren naar Azure App-Service

Deze zelfstudie ziet u hoe u een app Sails.js implementeren naar Azure App-Service. Klik in het proces, kunt u enkele algemene kennis voor het configureren van uw app Node.js uitvoeren in App Service vindt. 

U kunt werken kennis van Sails.js nodig hebt. Deze zelfstudie is niet bedoeld om u te helpen met met betrekking tot Sail.js in het algemeen wordt uitgevoerd.


## <a name="prerequisites"></a>Vereisten voor

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Cijfer](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Een Microsoft Azure-account. Als u geen account hebt, kunt u [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) of [activeren van de voordelen van uw Visual Studio-abonnee](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Als u wilt zien Azure App-Service in actie voordat u zich registreert voor een Azure-account, gaat u naar [De App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751). Er, u direct een tijdelijk starter-app kunt maken in de App Service, geen creditcard vereist, niet verplichtingen.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Stap 1: Een app Sails.js lokaal maken

Snel Maak eerst een standaard-Sails.js-app in uw ontwikkelomgeving door deze stappen uit:

1. Open de opdrachtregel terminal van uw keuze en `CD` in een werkmap.

2. Een Sails.js-app maken en uit te voeren:

        sails new <appname>
        cd <appname>
        sails lift

    Zorg ervoor dat u naar de standaard-startpagina op http://localhost:1377 kunt navigeren.

## <a name="step-2-create-the-azure-app-resource"></a>Stap 2: De resource Azure-app maken

Maak vervolgens de App Service resource in Azure wordt aangegeven. U gaat nu later wilt distribueren uw app Sails.js toe.

1. Meld u aan bij Azure vind ik leuk dus:
1. In de dezelfde terminal wijzigen in de modus ASM en meld u aan bij Azure:

        azure config mode asm
        azure login

    Volg de vraag die u kunt doorgaan met de aanmelding in een browser met een Microsoft-account met uw Azure-abonnement.

2. Controleer of dat u bent nog steeds in de hoofdmap van uw project Sails.js. Maak de App Service app resource in Azure wordt aangegeven met een unieke app-naam met de volgende opdracht. Uw web-app-URL is http://&lt;toepassingsnaam >. azurewebsites.net.

        azure site create --git <appname>

    Volg de vraag om een Azure regio om te implementeren naar te selecteren. Als u nooit cijfer/FTP-implementatie referenties voor uw Azure-abonnement instellen hebt, moet u ook gevraagd om deze te maken.

    Nadat de App Service app resource is gemaakt:

    - Sails.js app is cijfer-geïnitialiseerd,
    - Uw lokale cijfer-geïnitialiseerd opslagplaats is verbonden met de nieuwe App Service-app als een cijfer remote, toepasselijke naam 'azure', en
    - En iisnode.yml bestand is gemaakt in de hoofdmap. U kunt dit bestand gebruiken voor het configureren van [iisnode](https://github.com/tjanczuk/iisnode), waarin App Service wordt gebruikt om uit te voeren Node.js apps.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Stap 3: Configureren en implementeer de app Sails.js

 Werken met een Sails.js-app in de App Service bestaat uit drie basisstappen:

 - Uw app voor deze uitvoeren in de App-Service configureren
 - Het dashboard implementeren naar App Service
 - Lees stderr en stdout logboeken implementatieproblemen oplossen

Volg deze stappen:

1. Het nieuwe iisnode.yml-bestand openen in de hoofdmap en voeg de volgende twee regels:

        loggingEnabled: true
        logDirectory: iisnode

    Logboekregistratie is nu beschikbaar voor iisnode. Zie  [stdout en stderr aanmeldingslogboeken uit iisnode ophalen](app-service-web-nodejs-get-started.md#iisnodelog)voor meer informatie over hoe dit werkt.

2. Open config/env/production.js als u wilt instellen en configureren van uw productieomgeving `port` en `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    U vindt de documentatie van deze configuratie-instellingen in de  [Documentatie van Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Vervolgens moet u om ervoor te zorgen dat [knorvis](https://www.npmjs.com/package/grunt) compatibel met Azure van netwerkstations is. Knorvis-versies die kleiner is dan 1.0.0 gebruikt een verouderde [glob](https://www.npmjs.com/package/glob) -pakket (minder dan 5.0.14), die niet netwerkstations ondersteunt. 

3. Open package.json en wijzig de `grunt` versie moet `1.0.0` en verwijder alle `grunt-*` pakketten. Uw `dependencies` eigenschap ziet er als volgt:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Voeg het volgende in package.json, `engines` eigenschap waarmee de Node.js-versie instellen op een die we horen.

        "engines": {
            "node": "6.6.0"
        },

6. Uw wijzigingen op te slaan en test uw wijzigingen om ervoor te zorgen dat uw app nog steeds wordt uitgevoerd lokaal. Klik hiertoe verwijderen de `node_modules` map en klik vervolgens uitvoeren:

        npm install
        sails lift

4. Nu gebruiken cijfer om uw app implementeren naar Azure:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Tot slot starten alleen uw live Azure-app in de browser:

        azure site browse

    U ziet nu de dezelfde Sails.js-startpagina.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Problemen met de implementatie

Als uw toepassing Sails.js om welke reden in App Service mislukt, vindt u de logboeken stderr voor het oplossen van deze.
Zie [stdout en stderr aanmeldingslogboeken uit iisnode ophalen](app-service-web-nodejs-sails.md#iisnodelog)voor meer informatie.
Als deze is gestart, het logboek stdout moet u de vertrouwde bericht weergeven:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

U kunt bepalen granulatie van de logboeken stdout in het bestand [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) . 

## <a name="connect-to-a-database-in-azure"></a>Verbinding maken met een database in Azure wordt aangegeven

Als u wilt verbinden met een database in Azure wordt aangegeven, die u kunt maken van de database van uw keuze in Azure wordt aangegeven, zoals Azure SQL-Database, MySQL, MongoDB, Cache Azure (bestand Vgx.), enzovoort, en de bijbehorende [gegevensopslag adapter](https://github.com/balderdashy/sails#compatibility) gebruiken tot stand te brengen. De stappen in deze sectie leest u hoe u verbinding maakt met een MySQL-database in Azure wordt aangegeven.

1. Volg de zelfstudie [hier](../store-php-create-mysql-database.md) als u wilt maken van een MySQL-database in Azure wordt aangegeven.

2. Vanaf de opdrachtregel terminal, de MySQL-adapter te installeren:

        npm install sails-mysql --save

3. Open config/connections.js en het volgende object toevoegen aan de lijst: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Voor elke omgevingsvariabele (`process.env.*`), moet u deze eigenschap instellen in de App-Service. Voer de volgende opdrachten vanaf uw computer hiervoor. Alle verbindingsgegevens die u nodig hebt, wordt in de portal van Azure (Zie [verbinding maken met uw MySQL-database](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Plaatsen van uw instellingen in de instellingen van Azure app blijft gevoelige gegevens uit uw bronbeheer (cijfer). Vervolgens stelt u uw ontwikkelomgeving als de dezelfde verbindingsinformatie wilt gebruiken.

4. Open config/local.js en de volgende verbindingen object toevoegen:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Deze configuratie vervangt de instellingen in uw bestand config/connections.js voor de lokale omgeving. Dit bestand wordt uitgesloten door de standaard-.gitignore in uw project, zodat deze niet worden opgeslagen in cijfer. U bent nu verbinding maken met uw MySQL-database van uw Azure web-app en van uw lokale ontwikkelomgeving.

4. Open config/env/production.js om te configureren van uw productieomgeving en voeg de volgende `models` object:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Open config/env/development.js om te configureren van uw ontwikkelomgeving en voeg de volgende `models` object:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`kunt u migratie databasefuncties gebruiken om te maken en de tabellen in uw database in uw MySQL eenvoudig bijwerken. Echter `migrate: 'safe'` wordt gebruikt voor uw omgeving Azure (productie) omdat Sails.js kunnen niet worden gebruik `migrate: 'alter'` in een productieomgeving (Zie  [Sails.js documentatie](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Vanaf de terminal, [genereren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) een Sails.js [blauwdruk API](http://sailsjs.org/documentation/concepts/blueprints) zoals u normaal zou doen, voert u `sails lift` de database maken met Sails.js database wilt migreren. Bijvoorbeeld:

         sails generate api mywidget
         sails lift

    De `mywidget` model gegenereerd door deze opdracht leeg is, maar we deze kunt gebruiken om weer te geven dat we database connectivity hebben.
    Wanneer u uitvoert `sails lift`, deze Hiermee maakt u de ontbrekende tabellen voor de modellen voor uw app wordt gebruikt.

6. Toegang tot de blauwdruk API die u zojuist hebt gemaakt in de browser. Bijvoorbeeld:

        http://localhost:1337/mywidget/create
    
    De API moet de gemaakte invoer retourneren terug naar u in het browservenster, wat betekent dat de database is gemaakt.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Nu uw wijzigingen aan Azure push en bladert u naar uw app om ervoor te zorgen dat het nog steeds werkt.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Toegang tot de blauwdruk API van uw Azure web-app. Bijvoorbeeld:

        http://<appname>.azurewebsites.net/mywidget/create

    Als de API nog een nieuw item retourneert, klikt u vervolgens praten uw Azure WebApp met uw MySQL-database.

## <a name="more-resources"></a>Meer informatiebronnen

- [Aan de slag met Node.js WebApps in Azure App-Service](app-service-web-nodejs-get-started.md)
- [Gebruik van Node.js Modules met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
