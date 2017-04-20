<properties 
    pageTitle="Het gebruik van Twilio voor spraak- en SMS (Java) | Microsoft Azure" 
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van de code is geschreven in Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java

Deze handleiding wordt getoond hoe algemene programming taken met de service API Twilio uitvoeren op Azure. De scenario's waarvoor bevatten een telefoongesprek bellen, en u een kort bericht Service (SMS)-bericht verzendt. Zie voor meer informatie over Twilio en het gebruik van spraak en SMS in uw toepassingen, de sectie van de [Volgende stappen](#NextSteps) .

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een telefonie-webservice-API waarmee u uw bestaande web talen en vaardigheden gebruiken om te maken van spraak en SMS-toepassingen. Twilio is een derde partij-service (niet een Azure functie en niet een Microsoft-product).

**Twilio Voice** geeft uw toepassingen bellen en gebeld telefoongesprekken. **Twilio SMS** geeft uw toepassingen bellen en gebeld SMS-berichten. **Twilio Client** geeft uw toepassingen met bestaande internetverbindingen, met inbegrip van verbindingen met mobiele spraakcommunicatie inschakelen.

## <a id="Pricing"></a>Twilio prijzen en speciale aanbiedingen
Informatie over Twilio prijzen is beschikbaar op [Twilio prijzen] [twilio_pricing]. Azure klanten ontvangen een [speciale aanbieding][special_offer]: een gratis creditcard van 1000 teksten of 1000 inkomende minuten. Als u wilt registreren voor deze aanbieding of meer informatie vinden, gaat u naar [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Concepten
De API Twilio is een RESTful API met spraak en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen; Zie voor een lijst [Twilio API bibliotheken] [twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio werkwoorden en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio werkwoorden
De API gebruik maakt van Twilio werkwoorden; bijvoorbeeld de ** &lt;zeg&gt; ** werkwoord Hiermee geeft u voor Twilio hoorbaar een bericht te bezorgen op een gesprek. 

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

## <a id="create_app"></a>Een Java-toepassing maken
1. De Twilio JAR aanvragen en toe te voegen aan uw pad Java-opbouwen en uw WAR implementatie-vergadering. Bij [https://github.com/twilio/twilio-java][twilio_java], u kunt downloaden GitHub bronnen en maken van uw eigen oppervlak, of een vooraf gedefinieerde oppervlak (met of zonder afhankelijkheden).
2. Zorgen dat van uw JDK **cacerts** keystore bevat het certificaat Equifax Secure certificeringsinstantie met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het seriële getal is 35:DE:F4:CF en de vingerafdruk SHA1 is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat certificeringsinstantie (CA)-certificaat voor de [https://api.twilio.com] [ twilio_api_service] service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Voor informatie over ervoor zorgen dat van uw JDK **cacerts** keystore het juiste CA-certificaat bevat, raadpleegt u [een certificaat naar de Store Java CA certificaat toe te voegen][add_ca_cert].

Gedetailleerde instructies voor het gebruik van de bibliotheek van de client Twilio voor Java vindt u op [hoe u een telefoongesprek Twilio gebruiken in een Java-toepassing op Azure][howto_phonecall_java].

## <a id="configure_app"></a>Uw toepassing configureren om te Twilio bibliotheken gebruiken
U kunt uw code, **importeren** -instructies toevoegen boven aan uw bronbestanden voor de Twilio-pakketten of klassen die u wilt gebruiken in uw toepassing. 

Voor bronbestanden van Java:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Klik op bronbestanden voor Java serverpagina (JSP):

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Afhankelijk van welke Twilio-pakketten of klassen die u gebruiken wilt, enigszins uw instructies **importeren** afwijken.

## <a id="howto_make_call"></a>Hoe u: uitbellen
Hieronder ziet u hoe u een uitgaande oproep met de klasse **CallFactory** . Deze code wordt ook een geleverde Twilio-site om terug te keren het antwoord Twilio Markup Language (TwiML). Vervangt door uw waarden voor de telefoonnummers **van** en **tot** , en zorg ervoor dat u controleren of het telefoonnummer **van** voor uw account Twilio voordat het uitvoeren van de code.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Zie voor meer informatie over de parameters doorgegeven aan de methode **CallFactory.create** , [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Zoals vermeld, gebruikt deze code een geleverde Twilio-site om terug te keren het antwoord TwiML. U kunt in plaats hiervan uw eigen site op te geven van het antwoord TwiML; Lees [hoe u bieden TwiML antwoorden in een Java-toepassing op Azure](#howto_provide_twiml_responses)voor meer informatie.

## <a id="howto_send_sms"></a>Hoe u: SMS-bericht verzenden
Hieronder ziet u hoe u met de klasse **SmsFactory** SMS-bericht verzendt. De **van** getal, **4155992671**, is verstrekt door Twilio voor proefabonnement accounts SMS-berichten te verzenden. **Het nummer** moet worden geverifieerd voor uw account Twilio voordat het uitvoeren van de code.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Zie voor meer informatie over de parameters doorgegeven aan de methode **SmsFactory.create** , [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Hoe u: bieden TwiML antwoorden van uw eigen Website
Wanneer uw toepassing tot stand een oproep tot de API Twilio brengt, stuurt bijvoorbeeld via de methode **CallFactory.create** Twilio uw aanvraag naar een URL die wordt naar verwachting om terug te keren een antwoord TwiML. Met het bovenstaande voorbeeld gebruikt de geleverde Twilio URL- [http://twimlets.com/message][twimlet_message_url]. (TwiML is ontworpen voor gebruik door webservices, ziet u de TwiML in uw browser. Bijvoorbeeld, klikt u op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een leeg ** &lt;antwoord&gt; ** element; een ander voorbeeld, klikt u op [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] om te zien een ** &lt;antwoord&gt; ** element met een ** &lt;zeg&gt; ** element.)

In plaats van te vertrouwen op de geleverde Twilio URL, kunt u uw eigen URL-site die resulteert in HTTP-antwoorden. U kunt de site maken in een taal die resulteert in HTTP-antwoorden; in dit onderwerp wordt ervan uitgegaan dat u de URL in een JSP-pagina wordt hosten.

De volgende pagina van de JSP levert een TwiML antwoord met de mededeling **Hallo allemaal** het gesprek dat.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

De volgende pagina van de JSP levert een TwiML-antwoord dat tekst wordt gemeld dat, heeft verschillende pauzes en informatie over de Twilio API-versie en de naam van de Azure rol staat.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

De parameter **ApiVersion** is beschikbaar in Twilio voice aanvragen (niet SMS aanvragen). De beschikbare verzoek parameters voor Twilio spraak- en SMS aanvragen, Zie <https://www.twilio.com/docs/api/twiml/twilio_request> en <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectievelijk. De omgevingsvariabele **functienaam** is beschikbaar als onderdeel van een Azure-implementatie. (Als u toevoegen, aangepaste omgevingsvariabelen wilt zodat ze kunnen worden opgehaald uit **System.getenv**, raadpleegt u het gedeelte van de variabelen omgeving aan [Diverse rol configuratie-instellingen][misc_role_config_settings].)

Nadat u uw JSP-pagina ingesteld voor TwiML optionele antwoorden hebt, kunt u de URL van de pagina JSP gebruiken als de URL doorgegeven aan de methode **CallFactory.create** . Als er een webtoepassing met de naam MyTwiML geïmplementeerd in een Azure-service, die worden gehost en de naam van de pagina JSP is mytwiml.jsp en bijvoorbeeld de URL kan worden doorgegeven aan **CallFactory.create** zoals wordt weergegeven in de volgende handelingen uit:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Er is een andere optie voor het reageren met TwiML via de klasse **TwiMLResponse** , die beschikbaar in het pakket **com.twilio.sdk.verbs is** .

Zie voor meer informatie over het gebruik van Twilio in Azure wordt aangegeven met Java, [hoe u een telefoongesprek Twilio gebruiken in een Java-toepassing op Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Hoe u: extra Twilio Services gebruiken
Naast de voorbeelden die u hier kunt zien, biedt Twilio web-API's die u gebruiken kunt om te profiteren van extra Twilio functionaliteit uit uw Azure-toepassing. Zie de [Twilio API-documentatie]voor volledige informatie [twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
U kunt de basisbeginselen van de service Twilio hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging] [twilio_security_guidelines]
* [De Twilio procedure en voorbeeld] [twilio_howtos]
* [Twilio Quickstart zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub] [twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
