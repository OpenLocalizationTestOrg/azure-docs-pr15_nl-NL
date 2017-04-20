<properties 
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (Ruby) | Microsoft Azure" 
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van de code in Ruby geschreven." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Ruby
Deze handleiding wordt getoond hoe algemene programming taken met de service API Twilio uitvoeren op Azure. De scenario's waarvoor bevatten een telefoongesprek bellen, en u een kort bericht Service (SMS)-bericht verzendt. Zie voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen, de sectie van de [Volgende stappen](#NextSteps) .

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een telefonie-webservice-API waarmee u uw bestaande web talen en vaardigheden gebruiken om te maken van spraak en SMS-toepassingen. Twilio is een derde partij-service (niet een Azure functie en niet een Microsoft-product).

**Twilio Voice** geeft uw toepassingen bellen en gebeld telefoongesprekken. **Twilio SMS** geeft uw toepassingen bellen en gebeld SMS-berichten. **Twilio Client** geeft uw toepassingen met bestaande internetverbindingen, met inbegrip van verbindingen met mobiele spraakcommunicatie inschakelen.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen
Informatie over Twilio prijzen is beschikbaar op [Twilio prijzen] [twilio_pricing]. Azure klanten ontvangen een [speciale aanbieding][special_offer]: een gratis creditcard van 1000 teksten of 1000 inkomende minuten. Als u wilt registreren voor deze aanbieding of meer informatie vinden, gaat u naar [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Concepten
De API Twilio is een RESTful API met spraak en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst [Twilio API bibliotheken] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML is een reeks op XML gebaseerde instructies die informeren Twilio van het verwerken van een gesprek of SMS.

Als voorbeeld, zou het volgende TwiML **Hallo allemaal** van de tekst converteren naar spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML documenten hebt `<Response>` als hun hoofdelement. Hierin kunt u Twilio werkwoorden gebruiken om het gedrag van uw toepassing te definiëren.

### <a id="Verbs"></a>TwiML werkwoorden
Twilio bewerkingen zijn XML-codes waarop Twilio wat te **doen**. Bijvoorbeeld de ** &lt;zeg&gt; ** werkwoord Hiermee geeft u voor Twilio hoorbaar een bericht te bezorgen op een gesprek. 

Hier volgt een lijst met Twilio woorden.

* ** &lt;Inbelverbinding&gt;**: de beller verbonden met een andere telefoon.
* ** &lt;Verzamelen&gt;**: verzamelt cijfers ingevoerd op het toetsenblok van de telefoon.
* ** &lt;Ophangen&gt;**: een oproep eindigt.
* ** &lt;Afspelen&gt;**: het afspelen van een audiobestand.
* ** &lt;Onderbreken&gt;**: stilte wacht op een opgegeven aantal seconden.
* ** &lt;Record&gt;**: Hiermee wordt een van de beller spraak en geeft als resultaat een URL van een bestand met de opname.
* ** &lt;Omleiden&gt;**: draagt de controle van een gesprek of SMS aan de TwiML bij een andere URL.
* ** &lt;Negeren&gt;**: een binnenkomende oproep naar uw telefoonnummer Twilio zonder facturering u weigert
* ** &lt;Zeg&gt;**: converteert tekst naar spraak die bestaat uit een gesprek.
* ** &lt;Sms&gt;**: SMS-bericht stuurt.

Zie voor meer informatie over Twilio werkwoorden, hun kenmerken en TwiML, [TwiML] [twiml]. Zie voor meer informatie over de API Twilio, [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om aan een account Twilio, registreren bij [Probeert Twilio] [try_twilio]. U kunt beginnen met een gratis account en uw account later bijwerken.

Wanneer u zich aanmeldt voor een account Twilio, krijgt u een gratis telefoonnummer voor uw toepassing. U ontvangt ook een account en een token auth. Beide nodig Twilio API-gesprekken. Als u wilt voorkomen dat onbevoegde toegang tot uw account, uw authenticatie token veilig houden. Uw account en auth token kunnen worden weergegeven op de [accountpagina Twilio][twilio_account]in de velden **beveiligings-id-ACCOUNT** en **AUTH TOKEN**, respectievelijk label.

### <a id="VerifyPhoneNumbers"></a>Controleer of telefoonnummers
Naast het nummer krijgt u door Twilio, u kunt ook controleren of het getal dat u besturingselement (dat wil zeggen uw mobiele telefoon of start telefoonnummer) voor gebruik in uw toepassingen. 

Zie voor informatie over het om te controleren of een telefoonnummer [Beheren getallen] [verify_phone].

## <a id="create_app"></a>Een Ruby-toepassing bouwen
Een Ruby-toepassing die gebruikmaakt van de service Twilio en wordt uitgevoerd op Azure is niet anders dan andere Ruby toepassingen die de service Twilio gebruikt. Terwijl Twilio services RESTful zijn en kunnen worden aangeroepen vanuit Ruby op verschillende manieren, in dit artikel wordt richten op het gebruik van Twilio services met [Twilio Help-bibliotheek voor Ruby][twilio_ruby].

Eerste, [stel ik een nieuwe Azure Linux VM] [ azure_vm_setup] fungeert als host voor uw nieuwe Ruby-webtoepassing. De stappen voor het maken van een app Rails, wordt alleen ingesteld de VM negeren. Zorg ervoor dat u een eindpunt maken met een externe poort 80 en een interne poort van 5000.

In de onderstaande voorbeelden we [Sinatra]gebruiken[sinatra], een zeer eenvoudige web kader voor Ruby. Maar u kunt de Twilio Help-bibliotheek zeker voor Ruby gebruiken met een andere web framework, met inbegrip van Ruby op Rails.

SSH naar uw nieuwe VM en een map voor de nieuwe app te maken. In die map, maakt u een bestand met de naam van Gemfile en kopieer de volgende code erin:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Klik op de opdrachtregel uitvoeren `bundle install`. Hiermee installeert u de bovenstaande afhankelijkheden. Maak een bestand met de naam `web.rb`. Dit is waar de code voor uw web-app zich bevindt. Plak de volgende code erin:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Nu u zou moeten kunnen het uitvoeren van de opdracht `ruby web.rb -p 5000`. Hiermee wordt kringveld-ups van een kleine webserver op poort 5000. U moet mogelijk om naar deze app in uw browser via de URL u ingesteld voor uw VM Azure. Nadat u uw web-app in de browser bereiken kunt, bent u bent klaar om te beginnen met het samenstellen van een Twilio-app.

## <a id="configure_app"></a>Uw toepassing Twilio configureren
U kunt uw web-app als u wilt gebruiken van de bibliotheek Twilio door het bijwerken van uw `Gemfile` deze regel wilt opnemen:

    gem 'twilio-ruby'

Voer op de opdrachtregel `bundle install`. Nu openen `web.rb` en deze regel aan de bovenkant waaronder:

    require 'twilio-ruby'

U bent nu alle ingesteld op de Twilio Help-bibliotheek voor Ruby gebruiken in uw web-app.

## <a id="howto_make_call"></a>Hoe u: uitbellen
Hieronder ziet u hoe u een uitgaande oproep. Belangrijke concepten opnemen gebruik van de bibliotheek Twilio helper voor Ruby REST API-gesprekken en het weergeven van TwiML. Vervangt door uw waarden voor de telefoonnummers **van** en **tot** , en zorg ervoor dat u controleren of het telefoonnummer **van** voor uw account Twilio voordat het uitvoeren van de code.

Deze functie toevoegen `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Als u openen-up `http://yourdomain.cloudapp.net/make_call` in een browser die de oproep tot de API Twilio door naar de oproep wordt geactiveerd. De eerste twee parameters in `client.account.calls.create` zijn vrij geen uitleg: het nummer is van het gesprek `from` en het nummer is van het gesprek `to`. 

De derde parameter (`url`) is de URL die Twilio aanvraagt voor instructies over wat u moet doen wanneer het gesprek wordt verbonden. In dit geval we stel ik een URL (`http://yourdomain.cloudapp.net`) die geeft als resultaat een eenvoudige TwiML-document en gebruik de `<Say>` werkwoord moet u doen bepaalde tekst naar spraak en zeg "Hallo Aap" aan de persoon die de oproep ontvangt.

## <a id="howto_recieve_sms"></a>Hoe u: ontvangen SMS-bericht
In het vorige voorbeeld we een **uitgaande** telefoongesprek gestart. Deze tijd, laten we gebruiken het telefoonnummer dat Twilio ons tijdens het aanmelden heeft voor een **binnenkomend** SMS-bericht.

Eerste, aanmelden bij uw [dashboard Twilio][twilio_account]. Klik op 'Getallen' in de bovenste navigatiebalk en klik vervolgens op in de Twilio in dat die u hebt opgegeven. Hier ziet u twee URL's die u kunt configureren. Aanvragen URL de URL van een Voice-aanvraag en een SMS-bericht. Hierna ziet u de URL's die Twilio belt wanneer een telefoongesprek wordt gemaakt of een SMS-bericht wordt verzonden naar uw telefoonnummer. De URL's zijn ook bekend als "web haken".

Willen we inkomende SMS-berichten verwerken, zodat we de URL die u wilt bijwerken `http://yourdomain.cloudapp.net/sms_url`. Verdergaan en klik op wijzigingen opslaan onder aan de pagina. Nu weer in `web.rb` we onze toepassing u omgaat met dit programma:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Zorg dat u uw web-app opnieuw te starten na het aanbrengen van de wijziging. Nu verricht uw telefoon en een SMS-bericht verzenden naar uw telefoonnummer Twilio. U moet een SMS-antwoord met de mededeling "Hoi, Bedankt voor de ping dat direct ophalen! Twilio en Azure uit! ".

## <a id="additional_services"></a>Hoe u: extra Twilio Services gebruiken
Naast de voorbeelden die u hier kunt zien, biedt Twilio web-API's die u gebruiken kunt om te profiteren van extra Twilio functionaliteit uit uw Azure-toepassing. Zie de [Twilio API-documentatie]voor volledige informatie [twilio_api_documentation].

### <a id="NextSteps"></a>Volgende stappen
U kunt de basisbeginselen van de service Twilio hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging] [twilio_security_guidelines]
* [Twilio HowTos en voorbeeld] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
