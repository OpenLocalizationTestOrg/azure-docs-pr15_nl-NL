<properties 
    pageTitle="Met behulp van Twilio voor spraak, VoIP en SMS-berichten in Azure wordt aangegeven" 
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van de code in Node.js geschreven." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Met behulp van Twilio voor spraak, VoIP en SMS-berichten in Azure wordt aangegeven

Deze handleiding wordt beschreven hoe u apps die met Twilio en node.js op Azure communiceren kunt maken.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Wat is Twilio?

Twilio is een API-platform waarmee u gemakkelijk voor ontwikkelaars aanbrengen en telefoongesprekken ontvangen, verzenden en ontvangen van SMS-berichten, en om te bellen kwijt VoIP in op basis van een browser en systeemeigen mobiele toepassingen insluiten.  Laten we eens kort eens hoe dit werkt voordat Dubbelklik.

### <a name="receiving-calls-and-text-messages"></a>Oproepen en SMS-berichten ontvangen

Twilio Hiermee kunnen ontwikkelaars voor het [aanschaffen van programmeerbaar telefoonnummers] [ purchase_phone] die kan worden gebruikt om te verzenden en ontvangen van gesprekken en SMS-berichten.  Wanneer een getal Twilio een oproep binnenkomt of de tekst ontvangt, stuurt Twilio uw webtoepassing een HTTP POST of GET-aanvraag, waarin u wordt gevraagd voor instructies over het afhandelen van het gesprek of de tekst.  Uw server reageert op Twilio van HTTP-aanvraag met [TwiML][twiml], een eenvoudige set van XML-codes die instructies voor hoe u omgaat met een gesprek of tekst bevatten.  Voorbeelden van TwiML zien we in even.

### <a name="making-calls-and-sending-text-messages"></a>Gesprekken voeren en het verzenden van tekstberichten

Door middel van HTTP-aanvragen tot de API Twilio web service, kunnen ontwikkelaars SMS-berichten verzenden of uitgaande telefoongesprekken starten.  Voor uitgaande oproepen, moet de ontwikkelaar ook opgeven voor een URL die resulteert in TwiML instructies voor hoe u omgaat met de uitgaande oproep nadat dit is aangesloten.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Mogelijkheden voor VoIP insluiten in UI-code (JavaScript, iOS of Android)

Twilio biedt een aan de clientzijde SDK die een bureaublad webbrowser, -app van iOS of Android-app in een VoIP-telefoon omzetten kunt.  In dit artikel gaan we in op het gebruik van VoIP bellen in de browser.  Naast de JavaScript Twilio SDK uitgevoerd in de browser, moet een servertoepassing (onze node.js-toepassing) worden gebruikt voor een 'mogelijkheid token"verlenen aan de JavaScript-client.  U kunt meer informatie over het gebruik van VoIP met node.js [in het blog van de ontwikkelaar Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Aanmelden voor Twilio (Microsoft korting)

Voordat u de Twilio services gebruikt, moet u [zich registreren voor een account][signup].  Klanten van Microsoft Azure ontvangen een speciale korting - [Zorg ervoor dat u zich registreren hier][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Maak en implementeer een node.js Azure-Website

Vervolgens moet u een node.js website waarop Azure te maken.  [De officiële documentatie hiervoor bevindt zich hier][azure_new_site].  Op hoog niveau, wordt u worden als volgt:

* Zich registreert voor een Azure-account als u nog niet hebt
* Gebruik van de Azure beheerconsole om een nieuwe website te maken
* Bron besturingselement ondersteuning (wordt ervan uitgegaan dat u cijfer gebruikt) toe te voegen
* Maken van een bestand `server.js` met een eenvoudige node.js webtoepassing.
* Deze eenvoudige toepassing Azure implementeren

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>De Module Twilio configureren

Vervolgens gaat we een eenvoudige node.js-toepassing die gebruik maakt van de API Twilio schrijven.  Voordat we beginnen, moeten we onze accountreferenties Twilio configureren.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Twilio referenties in omgevingsvariabelen configureren

Om te verifiëren aanvragen ten opzichte van de Twilio back-end maken, moeten we onze account en auth token, welke functie als de gebruikersnaam en wachtwoord instellen voor onze Twilio-account. De meest veilige manier om te configureren voor gebruik met de module knooppunt in Azure is via de systeem-omgevingsvariabelen, die u rechtstreeks in de Azure beheerconsole kunt instellen.

Selecteer uw website node.js en klik op de koppeling 'Configureren'.  Als u iets omlaagschuiven, ziet u een gebied waar u configuratie-eigenschappen voor uw toepassing instellen kunt.  Voer de referenties van uw Twilio-account ([gevonden op uw dashboard Twilio][twilio_dashboard]) zoals - Zorg ervoor dat deze de naam "TWILIO_ACCOUNT_SID" en "TWILIO_AUTH_TOKEN", respectievelijk:

![Azure-beheerconsole][azure-admin-console]

Wanneer u deze variabelen hebt geconfigureerd, start u de toepassing opnieuw in de Azure console.

### <a name="declaring-the-twilio-module-in-packagejson"></a>De module Twilio in package.json declareren

Vervolgens moeten we een package.json voor het beheren van onze afhankelijkheden knooppunt module via [npm]maken.  Op hetzelfde niveau als het 'server.js'-bestand dat u hebt gemaakt in de zelfstudie Azure/node.js, door een bestand met de naam "package.json" te maken.  In dit bestand, plaatst u het volgende:

  {"naam": 'naam-toepassing', 'versie': "0.0.1", "Privé": waar, "scripts": {"starten": "knooppunt-server"}, "afhankelijkheden": {"express": "3.1.0", "ejs": "*", "twilio": "*"}}

Hiermee wordt de module twilio gedeclareerd als een afhankelijkheid, evenals de populaire [express web framework] [ express] en de EJS sjabloon engine.  Oké, nu we verder te doen - laten we eens enkele programmacode schrijven!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Een uitgaande gesprek

Laten we maken een eenvoudig formulier dat wordt een oproep naar een getal dat we Kies te doen.  Open omhoog server.js en voer de volgende code.  Houd er rekening mee zoals "CHANGE_ME" - plaatst u de naam van uw website azure er hierheen:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Vervolgens de map 'weergaven' - binnen deze map maken, maakt u een bestand met de naam 'index.ejs' met de volgende onderwerpen:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Nu uw website implementeren naar Azure en open het thuisnetwerk.  U moet kunnen Voer uw telefoonnummer in het tekstvak en u een oproep ontvangt van het nummer van uw Twilio!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>SMS-bericht verzenden

Nu kunnen we een gebruikersinterface en formulier logica voor de verwerking te instellen voor een tekstbericht versturen.  Open omhoog 'server.js' en voegt u de volgende code toe na de laatste aanroep 'app.post':

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

Toevoegen in "views/index.ejs", een ander formulier onder de eerste fase om in te dienen van een getal en een SMS-bericht:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Uw toepassing naar Azure opnieuw distribueren en u moet nu mogelijk om in te dienen die het formulier en uzelf (of een van uw vrienden eerstvolgende) een tekstbericht versturen.

<a id="nextsteps"/>
## <a name="next-steps"></a>Volgende stappen

U hebt nu de basisbeginselen van het gebruik van node.js en Twilio voor het maken van de apps die communiceren geleerd.  Maar deze voorbeelden van het oppervlak van de mogelijkheden van Twilio en node.js nauwelijks krassen.  Raadpleeg de volgende bronnen voor meer informatie Twilio gebruikt met node.js:

* [Officiële module-documenten][docs]
* [Zelfstudie over VoIP met node.js-toepassingen][voipnode]
* [Votr - een realtime SMS stemmen toepassing met node.js en CouchDB (drie delen)][votr]
* [Paar hoeft te programmeren in de browser met node.js][pair]

We hopen dat u al kent node.js en Twilio hacken op Azure.

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



