<properties
    pageTitle="Een Node.js chat-toepassing bouwen met Socket.IO in Azure App-Service"
    description="Een zelfstudie die laat zien socket.io gebruiken in een web-app van node.js die worden gehost op Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Een Node.js chat-toepassing bouwen met Socket.IO in Azure App-Service

Socket.IO biedt realtime communicatie tussen uw node.js-server en klanten met behulp van WebSockets. Ondersteunt ook gebruik andere weergeven (zoals lange polling) die met oudere browsers werken. Deze zelfstudie wordt u begeleid bij hostingprovider een Socket.IO gebaseerd chat-toepassing als een Azure WebApp en leert u hoe u de schaal van de toepassing [Azure bestand Vgx. Cache]gebruikt. Zie voor meer informatie over Socket.IO, <http://socket.io/>.

> [AZURE.NOTE] De procedures in deze taak toepassen op [De Service-WebApps App]; voor Cloud Services, raadpleegt u [een toepassing Node.js Chat met Socket.IO op een Cloudservice Azure maakt].

## <a name="download-the-chat-example"></a>Download het chat-voorbeeld

Voor dit project gebruiken we het voorbeeld chat uit de [Socket.IO GitHub opslagplaats]. Voer de volgende stappen uit om het voorbeeld downloaden toe te voegen aan het project dat u eerder hebt gemaakt.

1.  Download een [ZIP- of GZ gearchiveerd release] van het project Socket.IO (versie 1.3.5 is gebruikt voor dit document)

1.  Extraheren van de archief en kopieer de **voorbeelden\\chat** map naar een nieuwe locatie. Bijvoorbeeld ** \\knooppunt\\chat**.

## <a name="modify-appjs-and-install-modules"></a>App.js wijzigen en modules installeren

1.  Wijzig de **index.js** naar **app.js**. Hierdoor Azure om te bepalen dat dit een Node.js-toepassing is.

1.  Open het bestand **app.js** in een teksteditor. Wijzig de regel waarop `var io = require('../..')(server);` zoals hieronder wordt weergegeven:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Open het bestand **package.json** en toevoegen van een verwijzing naar socket.io onder `dependencies`, zoals hieronder wordt weergegeven:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Vanaf de opdrachtregel wijzigen in de ** \\knooppunt\\chat** gids en gebruik npm voor het installeren van de modules vereist door deze toepassing:

        npm install

    Hiermee wordt de modules in een submap **node_modules**geïnstalleerd.

## <a name="create-an-azure-web-app"></a>Een Azure WebApp maken

Volg deze stappen om een Azure web-app maken, cijfer publicatie inschakelen en vervolgens inschakelen WebSocket ondersteuning voor de web-app.

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure gratis proefversie</a>voor meer informatie.

1. Installeer de opdrachtregel Azure (Azure CLI) en verbinding maken met uw Azure-abonnement. Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md).

1. Als dit de eerste keer bij het instellen van een bibliotheek in Azure wordt aangegeven, moet u aanmeldingsreferenties maken. Voer de volgende opdracht uit de CLI Azure:

        azure site deployment user set [username] [password]

1. Wijzig in het ** \\node\chat** directory en gebruik de volgende opdracht om te maken van een nieuwe Azure web app en een lokale cijfer opslagplaats. Deze opdracht maakt ook een cijfer RAS benoemde 'azure'.

        azure site create mysitename --git

    U moet 'mysitename' vervangen door een unieke naam voor uw web-app.

1. Het toewijzen van de bestaande bestanden naar de lokale bibliotheek met behulp van de volgende opdrachten:

        git add .
        git commit -m "Initial commit"

1. Push de bestanden naar de Azure Web Apps-bibliotheek met de volgende opdracht uit:

        git push azure master

    Wanneer u wordt gevraagd, voert u uw referenties uit stap 2. U ontvangt statusberichten zoals modules zijn geïmporteerd op de server. Wanneer dit proces is voltooid, wordt de toepassing op uw Azure web-app worden uitgevoerd.

    > [AZURE.NOTE] Tijdens de installatie van de module, ziet u fouten die ' het geïmporteerde project... is niet gevonden '. Dit kunnen veilig worden genegeerd.

1. Socket.IO gebruikt WebSockets, die niet op Azure standaard zijn ingeschakeld. Gebruik de volgende opdracht uit om in te schakelen web sockets:

        azure site set -w

    Als u wordt gevraagd, typt u de naam van de web-app.

    >[AZURE.NOTE]
    >De 'azure site set -w' opdracht hebben alleen met versie 0.7.4 werk- of hoger van de Azure-opdrachtregel. U kunt ook met behulp van de [Portal van Azure](https://portal.azure.com)WebSocket-ondersteuning inschakelen.
    >
    >Als u wilt inschakelen WebSockets met behulp van de Azure-Portal, klik op de WebApp vanaf het Web Apps-blad, klikt u op **alle instellingen** > **Toepassingsinstellingen**. Klik onder **Web Sockets**, **op**. Klik vervolgens op **Opslaan**.

1. Gebruik de volgende opdracht uit om starten van uw webbrowser en navigeer naar de gehoste WebApp om weer te geven de web-app op Azure:

        azure site browse

Uw app wordt nu uitgevoerd op Azure en chatberichten tussen verschillende clients met Socket.IO kan doorsturen.

## <a name="scale-out"></a>De schaal van aanpassen

Socket.IO toepassingen kunnen worden vergroot af met behulp van een __adapter__ distributie van berichten en gebeurtenissen tussen meerdere toepassingsexemplaren. Hoewel er verschillende adapters beschikbaar zijn, kan de adapter [socket.io-bestand Vgx.] eenvoudig worden gebruikt met de functie Azure bestand Vgx. Cache.

> [AZURE.NOTE] Een extra vereiste voor een oplossing Socket.IO schalen is er ondersteuning voor Plaktoetsen sessies. Plaktoetsen sessies zijn al dan niet standaard ingeschakeld voor Azure-WebApps via Azure aanvragen routering. Zie [Exemplaar affiniteit in Azure websites]voor meer informatie.

### <a name="create-a-redis-cache"></a>Een bestand Vgx. cache maken

Voer de stappen in [een cache in Azure bestand Vgx. Cache maken] naar een nieuwe cache maken.

> [AZURE.NOTE] Sla de __hostnaam__ en de __primaire sleutel__ voor uw cache, zoals deze nodig in de volgende stappen zijn.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Voeg het bestand Vgx. en modules socket.io-bestand Vgx.

1. Wijzigen van een opdrachtregel, in de __ \\knooppunt\\chat__ gids en gebruik de volgende opdracht uit.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] De opgegeven in deze opdracht versies zijn de versies gebruikt die in dit artikel.

1. Wijzigen van het bestand __app.js__ om toe te voegen van de volgende regels direct na`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    __Redishostname__ en __rediskey__ vervangen door de hostnaam en de sleutel voor uw bestand Vgx. cache.

    Hiermee maakt u een publiceren en abonneren client naar het bestand Vgx. cache eerder hebt gemaakt. De clients worden gebruikt met de adapter voor Socket.IO als u wilt de cache bestand Vgx. gebruiken voor het doorgeven van berichten en gebeurtenissen tussen exemplaren van uw toepassing configureren

    > [AZURE.NOTE] Terwijl de adapter __socket.io-bestand Vgx.__ kan communiceren rechtstreeks naar het bestand Vgx., is de huidige versie ondersteunt geen de cache Azure bestand Vgx. vereist verificatie. Zodat de eerste verbinding wordt gemaakt met behulp van de module __bestand Vgx.__ en vervolgens de client is doorgegeven aan de adapter __socket.io-bestand Vgx.__ .
    >
    > Terwijl Azure bestand Vgx. Cache beveiligde verbindingen met poort 6380 ondersteunt, ondersteund beveiligde verbindingen vanaf 14-7-2014 niet door de modules in dit voorbeeld gebruikt. De bovenstaande code gebruikt het standaard onbeveiligde poort van 6379.

1. De gewijzigde __app.js__ opslaan

### <a name="commit-changes-and-redeploy"></a>Wijzigingen vastleggen en implementeer deze opnieuw

Vanaf de opdrachtregel in de __ \\knooppunt\\chat__ directory, gebruikt u de volgende opdrachten wijzigingen doorvoeren en implementeren van de toepassing.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Nadat u de wijzigingen hebt op de server is geplaatst, kunt u uw site over meerdere exemplaren schalen met behulp van de volgende opdracht uit.

    azure site scale instances --instances #

Waar __#__ is het aantal exemplaren maken.

U kunt verbinding maken met uw web-app van meerdere browsers of computers om te bevestigen dat berichten correct zijn verzonden naar alle clients.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="connection-limits"></a>Verbindingslimieten

Azure-WebApps is beschikbaar in meerdere SKU's, de resources die beschikbaar zijn voor uw site te bepalen. Het gaat hierbij om het aantal toegestane WebSocket verbindingen. Zie de [pagina prijzen van Web-Apps]voor meer informatie.

### <a name="messages-arent-being-sent-using-websockets"></a>Berichten niet worden verzonden met WebSockets

Als de clientbrowsers behouden weer gebruikgemaakt van lang verbindingsintegriteit in plaats van WebSockets, is het mogelijk om een van de volgende handelingen uit.

* **Kunt u het transport NET WebSockets beperken**

    In de volgorde voor Socket.IO WebSockets gebruiken als de transport van berichten, moeten de server en de client WebSockets ondersteunen. Als een of de andere niet, wordt een andere transport, zoals lange polling Onderhandel over Socket.IO. De standaardlijst met transport die worden gebruikt door Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`. U kunt instellen dat WebSockets alleen worden gebruikt door de volgende code toe te voegen aan het bestand **app.js** , na de lijn die bevat `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Houd er rekening mee dat oudere browsers die WebSockets niet ondersteunen is geen mogelijk verbinding maken met de site terwijl de bovenstaande code actief is, zoals deze wordt alleen naar WebSockets communicatie beperkt.

* **SSL gebruiken**

    WebSockets, is afhankelijk van enkele lagere gebruikte HTTP-headers, zoals de koptekst **upgraden** . Sommige tussenliggende netwerkapparaten, zoals web proxy's, kunnen deze kopteksten verwijderen. Als u wilt voorkomen dat dit probleem, kunt u de WebSocket-verbinding tot stand brengen via SSL.

    Een eenvoudige manier om dit te doen is voor het configureren van Socket.IO naar `match origin protocol`. Hiermee wordt aangegeven wanneer Socket.IO beveiligde communicatie instellen WebSockets hetzelfde als het oorspronkelijke HTTP-/ HTTPS-verzoek voor de pagina met webonderdelen. Als een browser een HTTPS-URL gebruikt naar uw website te gaan, worden latere WebSocket communicatie via Socket.IO via SSL beveiligd.

    Als u wilt wijzigen in dit voorbeeld als deze configuratie wilt inschakelen, voegt u de volgende code naar het bestand **app.js** na de lijn die bevat `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Controleer of web.config-instellingen**

    Azure-WebApps die als Node.js toepassingen host gebruiken het bestand **web.config** om te verzoeken voor oproepen doorsturen naar de toepassing Node.js. Voor WebSockets-functie juist met Node.js-toepassingen, moet de **web.config** de volgende vermelding bevatten.

        <webSocket enabled="false"/>

    Hierdoor wordt de module IIS WebSockets, waaronder een eigen implementatie van WebSockets en conflicten met Node.js specifieke WebSocket modules zoals Socket.IO uitgeschakeld. Als deze regel ontbreekt of is ingesteld op `true`, dit de reden waarom het transport WebSocket niet voor uw toepassing werkt kan zijn.

    Normaal gesproken vergt Neem Node.js toepassingen niet een **web.config** -bestand, zodat Azure Websites automatisch een voor Node.js toepassingen genereren wordt wanneer ze worden gebruikt. Aangezien dit bestand automatisch wordt gegenereerd op de server, moet u de FTP- of TRANSFEREERT-URL voor uw website in dit bestand weer te geven. U vindt de FTP- en TRANSFEREERT URL's voor uw site in de klassieke portal door uw web-app en de koppeling **Dashboard** te selecteren. De URL's worden weergegeven in de sectie **snel aan te duiden** .

    > [AZURE.NOTE] Het bestand **web.config** wordt alleen gegenereerd door Azure Websites als uw toepassing geen een biedt. Als u een **web.config** -bestand in de hoofdmap van uw toepassingsproject opgeeft, wordt deze gebruikt in WebApps Azure.

    Als het fragment ontbreekt of is ingesteld op een waarde van `true`, moet u een **web.config** in de hoofdmap van uw Node.js-toepassing maken en geef een waarde van `false`.  Voor een verwijzing naar de hieronder vindt u een standaard- **web.config** voor een toepassing die gebruikmaakt van **app.js** als het ingangspunt.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Als uw toepassing een ingangspunt dan **app.js gebruikt**, moet u alle exemplaren van **app.js** vervangen door het juiste ingangspunt. Bijvoorbeeld, kunt vervangen **app.js** **server.js**.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert], waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een chat-toepassing die worden gehost in een Azure web-app maakt. U kunt ook deze toepassing als een Cloudservice Azure hosten. Zie [een Node.js Chat-toepassing met Socket.IO op een Cloudservice Azure maken]voor stapsgewijze instructies voor het om dit te doen.

Zie ook het [Node.js Developer Center]voor meer informatie.

## <a name="whats-changed"></a>Wat er gewijzigd

* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services].

<!-- URL List -->

[Cache Azure bestand Vgx.]: /documentation/services/redis-cache/
[App-Service WebApps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Pagina met webonderdelen Apps prijzen]: http://go.microsoft.com/fwlink/?LinkId=511643
[Een toepassing Node.js Chat met Socket.IO maken op een Azure Cloudservice]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure App-Service en de gevolgen voor bestaande Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Developer Center]: /develop/nodejs/
[Probeer de App-Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[Exemplaar affiniteit op Azure-websites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Een cache maken in de Cache van Azure bestand Vgx.]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.IO-bestand Vgx.]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub opslagplaats]: https://github.com/socketio/socket.io
[ZIP- of GZ gearchiveerd release]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
