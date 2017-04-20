<properties
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (PHP) | Microsoft Azure"
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van de code is geschreven in PHP."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in PHP
Deze handleiding wordt getoond hoe algemene programming taken met de service API Twilio uitvoeren op Azure. De scenario's waarvoor bevatten een telefoongesprek bellen, en u een kort bericht Service (SMS)-bericht verzendt. Zie voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen, de sectie van de [Volgende stappen](#NextSteps) .

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is dat de toekomstige zakelijke communicatie, waarmee ontwikkelaars op het insluiten van spraak, VoIP en messaging in toepassingen. Ze virtueel alle benodigde in een omgeving cloudgebaseerde, globale, weer via het Twilio communicatie API-platform infrastructuur. Toepassingen zijn eenvoudig te maken en scalable. Naar eigen wens met betaald-als-u Ga prijzen en profiteren van cloud betrouwbaarheid.

**Twilio Voice** geeft uw toepassingen bellen en gebeld telefoongesprekken. **Twilio SMS** kan uw toepassing verzenden en ontvangen van SMS-berichten. **Twilio Client** kunt u VoIP-oproepen op een telefoon, tablet of een browser en WebRTC ondersteunt.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen

Azure klanten ontvangen een [speciale aanbieding] $10 van Twilio krediet wanneer u een upgrade uitvoert van uw Twilio Account. Deze creditcard Twilio kunnen worden toegepast op elke Twilio gebruik ($10 creditcard gelijk aan dan 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice minuten, afhankelijk van de locatie van uw telefoon en bericht of gesprek bestemming). Deze creditcard Twilio inwisselen en aan de slag op: [ahoy.twilio.com/azure].

Twilio is een pay-as-you-go service. Er zijn geen ingesteld in rekening gebracht en u kunt uw account op elk gewenst moment sluiten. U kunt meer informatie vinden op [Twilio prijzen] [twilio_pricing].

## <a id="Concepts"></a>Concepten
De API Twilio is een RESTful API met spraak en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst [Twilio API bibliotheken] [twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio werkwoorden en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio werkwoorden
De API gebruik maakt van Twilio werkwoorden; bijvoorbeeld de ** &lt;zeg&gt; ** werkwoord Hiermee geeft u voor Twilio hoorbaar een bericht te bezorgen op een gesprek.

Hier volgt een lijst met Twilio woorden. Meer informatie over de andere werkwoorden en mogelijkheden via [Twilio Markup Language documentatie] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Inbelverbinding&gt;**: de beller verbonden met een andere telefoon.
* ** &lt;Verzamelen&gt;**: verzamelt cijfers ingevoerd op het toetsenblok van de telefoon.
* ** &lt;Ophangen&gt;**: een oproep eindigt.
* ** &lt;Afspelen&gt;**: het afspelen van een audiobestand.
* ** &lt;Onderbreken&gt;**: stilte wacht op een opgegeven aantal seconden.
* ** &lt;Record&gt;**: de voicemail van de beller Records en geeft als resultaat een URL van een bestand met de opname.
* ** &lt;Omleiden&gt;**: draagt de controle van een gesprek of SMS aan de TwiML bij een andere URL.
* ** &lt;Negeren&gt;**: een binnenkomende oproep naar uw telefoonnummer Twilio zonder facturering u weigert
* ** &lt;Zeg&gt;**: converteert tekst naar spraak die bestaat uit een gesprek.
* ** &lt;Sms&gt;**: SMS-bericht stuurt.

### <a id="TwiML"></a>TwiML
TwiML is een reeks op basis van de Twilio woorden die informeren Twilio van het verwerken van een gesprek of SMS op XML gebaseerde-instructies.

Als voorbeeld, zou het volgende TwiML **Hallo allemaal** van de tekst converteren naar spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de API Twilio u belt, is een van de parameters API de URL die resulteert in het antwoord TwiML. Voor ontwikkelingsdoeleinden, kunt u geleverde Twilio URL's op te geven van de TwiML antwoorden die worden gebruikt door uw toepassingen. U kunt ook uw eigen URL's om te maken van de antwoorden TwiML host en een andere optie is het object **TwiMLResponse** gebruiken.

Zie voor meer informatie over Twilio werkwoorden, hun kenmerken en TwiML, [TwiML] [twiml]. Zie voor meer informatie over de API Twilio, [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om aan een account Twilio, registreren bij [Probeert Twilio] [try_twilio]. U kunt beginnen met een gratis account en uw account later bijwerken.

Wanneer u zich aanmeldt voor een Twilio-account, ontvangt u een account-ID en een verificatietoken. Beide nodig Twilio API-gesprekken. Als u wilt voorkomen dat onbevoegde toegang tot uw account, uw authenticatie token veilig houden. Uw account-ID en verificatie token kunnen worden weergegeven op de [accountpagina Twilio] [twilio_account]in de velden **beveiligings-id-ACCOUNT** en **AUTH TOKEN**, respectievelijk label.

## <a id="create_app"></a>Een PHP-toepassing maken
Een PHP-toepassing die gebruikmaakt van de service Twilio en wordt uitgevoerd op Azure is niet anders dan andere PHP toepassingen die de service Twilio gebruikt. Terwijl Twilio services op basis van een REST zijn en kunnen worden aangeroepen vanuit PHP op verschillende manieren, in dit artikel wordt richten op het gebruik van Twilio services met [Twilio-bibliotheek voor PHP uit GitHub][twilio_php]. Zie voor meer informatie over het gebruik van de bibliotheek Twilio voor PHP, [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Gedetailleerde instructies voor het maken en implementeren van een toepassing Twilio/PHP Azure vindt u op [hoe u een telefoongesprek Twilio gebruiken in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="configure_app"></a>Uw toepassing configureren om te Twilio bibliotheken gebruiken
U kunt uw toepassing voor het gebruik van de bibliotheek Twilio voor PHP op twee manieren configureren:

1. Download de bibliotheek Twilio voor PHP uit GitHub ([https://github.com/twilio/twilio-php][twilio_php]) en de adreslijst **Services** toevoegen aan uw toepassing.

    -OF-

2. De bibliotheek Twilio voor PHP installeren als een pakket PERENBOMEN. Dit kan worden geïnstalleerd met de volgende opdrachten:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Nadat u de bibliotheek Twilio voor PHP hebt geïnstalleerd, kunt u een instructie **require_once** vervolgens toevoegen aan de bovenkant van uw bestanden PHP om te verwijzen naar de bibliotheek:

        require_once 'Services/Twilio.php';

Zie voor meer informatie [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Hoe u: uitbellen
Hieronder ziet u hoe u een uitgaande oproep met de klasse **Services_Twilio** . Deze code wordt ook een geleverde Twilio-site om terug te keren het antwoord Twilio Markup Language (TwiML). Vervangt door uw waarden voor de telefoonnummers **van** en **tot** , en zorg ervoor dat u controleren of het telefoonnummer **van** voor uw account Twilio voordat het uitvoeren van de code.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Zoals vermeld, gebruikt deze code een geleverde Twilio-site om terug te keren het antwoord TwiML. U kunt in plaats hiervan uw eigen site op te geven van het antwoord TwiML; Lees [hoe u bieden TwiML antwoorden uit uw eigen website](#howto_provide_twiml_responses)voor meer informatie.


- **Opmerking**: om op te lossen validatiefouten SSL-certificaat, raadpleegt u [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Hoe u: SMS-bericht verzenden
Hieronder ziet u hoe u met de klasse **Services_Twilio** SMS-bericht verzendt. Het getal **uit** wordt geleverd door Twilio voor proefabonnement accounts SMS-berichten te verzenden. **Het nummer** moet worden geverifieerd voor uw account Twilio voordat het uitvoeren van de code.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Hoe u: bieden TwiML antwoorden van uw eigen Website
Wanneer uw toepassing tot stand een oproep tot de API Twilio brengt, stuurt Twilio uw verzoek voor een URL die wordt naar verwachting om terug te keren een antwoord TwiML. Met het bovenstaande voorbeeld gebruikt de geleverde Twilio URL- [http://twimlets.com/message][twimlet_message_url]. (Terwijl TwiML is ontworpen voor gebruik door Twilio, ziet u de it in uw browser. Bijvoorbeeld, klikt u op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een leeg `<Response>` element; een ander voorbeeld, klikt u op [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] om te zien een `<Response>` element met een `<Say>` element.)

In plaats van te vertrouwen op de geleverde Twilio URL, kunt u uw eigen site die resulteert in HTTP-antwoorden. U kunt de site maken in een taal die resulteert in XML-antwoorden; in dit onderwerp wordt ervan uitgegaan dat u PHP gebruiken om te maken van de TwiML.

De volgende pagina van de PHP levert een TwiML antwoord met de mededeling **Hallo allemaal** het gesprek dat.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De bibliotheek Twilio voor PHP bevat klassen die TwiML voor u genereren. Het volgende voorbeeld levert het overeenkomstige antwoord zoals hierboven, maar met de **Services\_Twilio\_Twiml** class in de bibliotheek Twilio voor PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Zie voor meer informatie over TwiML, [https://www.twilio.com/docs/api/twiml][twiml_reference].

Nadat u uw PHP-pagina ingesteld voor TwiML optionele antwoorden hebt, gebruikt u de URL van de pagina PHP zoals de URL doorgegeven aan de `Services_Twilio->account->calls->create` methode. Als er een webtoepassing met de naam **MyTwiML** geïmplementeerd in een Azure-service, die worden gehost en de naam van de pagina PHP is **mytwiml.php**en bijvoorbeeld de URL kan worden doorgegeven aan **Services_Twilio -> account -> oproepen -> maken** zoals wordt weergegeven in het volgende voorbeeld:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Zie voor meer informatie over het gebruik van Twilio in Azure wordt aangegeven met PHP, [hoe u een telefoongesprek Twilio gebruiken in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Hoe u: extra Twilio Services gebruiken
Naast de voorbeelden die u hier kunt zien, biedt Twilio web-API's die u gebruiken kunt om te profiteren van extra Twilio functionaliteit uit uw Azure-toepassing. Zie de [Twilio API-documentatie]voor volledige informatie [twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
U kunt de basisbeginselen van de service Twilio hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging] [twilio_security_guidelines]
* [Twilio procedure hulplijnen en voorbeeld] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts]
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
