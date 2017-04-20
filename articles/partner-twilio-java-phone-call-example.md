<properties 
    pageTitle="Hoe u een telefoongesprek starten vanuit Twilio (Java) | Microsoft Azure" 
    description="Leer hoe u een telefoongesprek starten vanaf een webpagina Twilio gebruiken in een Java-toepassing op Azure." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Hoe u een telefoongesprek starten Twilio gebruiken in een Java-toepassing op Azure 

Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken om een gesprek te vanaf een webpagina die wordt gehost in Azure wordt aangegeven. De resulterende toepassing krijgt de gebruiker voor telefoongesprek waarden, zoals wordt weergegeven in de volgende schermopname.

![Azure gesprek formulier Twilio en Java gebruiken][twilio_java]

U moet de volgende handelingen uit als u de code gebruiken in dit onderwerp wilt doen:

1. In het bezit van een Twilio-account en verificatie token. Om te beginnen met Twilio evalueren prijzen bij [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich aanmelden bij [https://www.twilio.com/try-twilio][try_twilio]. Zie voor informatie over de API die is verstrekt door Twilio [http://www.twilio.com/api][twilio_api].
2. De Twilio oppervlak aanvragen. Bij [https://github.com/twilio/twilio-java][twilio_java_github], u kunt downloaden GitHub bronnen en maken van uw eigen oppervlak, of een vooraf gedefinieerde oppervlak (met of zonder afhankelijkheden).
De code in dit onderwerp is geschreven met het vooraf gedefinieerde TwilioJava 3.3.8 met afhankelijkheden oppervlak.
3. Het oppervlak toevoegen aan uw Java opbouwen pad.
4. Als u Eclips gebruikt om te maken van deze Java-toepassing, opnemen de Twilio JAR in uw toepassing implementatie bestand (WAR) van Eclips implementatie constructie-functie gebruiken. Als u Eclips niet gebruikt om te maken van deze Java-toepassing, controleert u de Twilio JAR is opgenomen binnen dezelfde Azure rol heeft als uw Java-toepassing en toegevoegd aan het pad class van uw toepassing.
5. Controleer of uw keystore cacerts bevat het certificaat Equifax Secure certificeringsinstantie met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het seriële getal is 35:DE:F4:CF en de vingerafdruk SHA1 is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat certificeringsinstantie (CA)-certificaat voor de [https://api.twilio.com] [ twilio_api_service] service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Zie voor informatie over het toevoegen van dit certificaat CA naar van uw JDK cacert store [een certificaat voor de de opslag van Java CA certificaat toe te voegen][add_ca_cert].

Daarnaast kunnen bekend zijn met de informatie bij het [maken van een Hallo wereld toepassing gebruikt de Toolkit Azure voor Eclips][azure_java_eclipse_hello_world], of met andere technieken voor het hosten van Java-toepassingen in Azure wordt aangegeven als u niet Eclips gebruikt, wordt ten zeerste aanbevolen.

## <a name="create-a-web-form-for-making-a-call"></a>Een webformulier voor het maken van een gesprek maken

De volgende code ziet hoe u een webformulier om op te halen gebruikersgegevens voor u een oproep plaatst maken. Voor toepassing van dit voorbeeld wordt een nieuw webproject dynamische **TwilioCloud**, met de naam is gemaakt en **callform.jsp** is toegevoegd als een JSP-bestand.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>De code als u het gesprek wilt maken
De volgende code die wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door callform.jsp voltooit, maakt het oproep-bericht en genereert het gesprek. Voor toepassing van dit voorbeeld wordt het bestand JSP heet **makecall.jsp** en is toegevoegd aan het project **TwilioCloud** . (Gebruik uw Twilio-account en verificatie in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan **accountSID** en **authToken** in de onderstaande code token.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Behalve als u de oproep plaatst, geeft makecall.jsp het eindpunt Twilio, API-versie, en de status van het gesprek. Een voorbeeld is de volgende schermopname:

![Azure gesprek antwoord Twilio en Java gebruiken][twilio_java_response]

## <a name="run-the-application"></a>Voer de toepassing
Hier volgen de hoog niveau stappen voor het uitvoeren van de toepassing niet omzeilen. Details voor deze stappen u op het [maken van een Hallo wereld toepassing gebruikt de Toolkit Azure voor Eclips vinden kunnen][azure_java_eclipse_hello_world].

1. Exporteer uw WAR TwilioCloud naar de map Azure **approot** . 
2. Wijzig **startup.cmd** als u wilt uw WAR TwilioCloud unzip.
3. Uw toepassing voor de emulator berekeningscluster compileren.
4. Start uw implementatie in de emulator berekeningscluster.
5. Open een browser en **http://localhost:8080/TwilioCloud/callform.jsp**uitvoeren.
6. Voer waarden in het formulier, klikt u op **dit gesprek**en klikt u vervolgens de resultaten bekijken in makecall.jsp.

Wanneer u klaar bent voor het implementeren naar Azure, opnieuw compileren voor implementatie in de cloud, implementeren naar Azure en http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp uitgevoerd in de browser (vervangt door de waarde voor *your_hosted_name*).

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om aan te geven basisfunctionaliteit Twilio in Java met op Azure. Voordat u de implementatie in Azure in productie, kunt u meer foutafhandeling of andere onderdelen toevoegen. Bijvoorbeeld:

* In plaats van een webformulier gebruikt, kunt u opslagruimte Azure BLOB's of SQL-Database kunt gebruiken voor het opslaan van telefoonnummers en tekst te bellen. Zie voor informatie over het gebruik van opslag in Azure BLOB's in Java [het gebruik van de Blob Storage-Service uit Java][howto_blob_storage_java]. Zie voor informatie over het gebruik van de SQL-Database in Java [SQL-Database gebruiken in Java][howto_sql_azure_java].
* U kunt voor het ophalen de Twilio account-ID en verificatietoken **RoleEnvironment.getConfigurationSettings** uit van uw implementatie configuratie-instellingen, in plaats van de harde codering de waarden in makecall.jsp. Zie [werken met de bibliotheek Azure Service Runtime in JSP] voor informatie over de klas **RoleEnvironment** ,[ azure_runtime_jsp] en de Runtime van Azure-Service-pakket-documentatie op [http://dl.windowsazure.com/javadoc][azure_javadoc].
* De code makecall.jsp wordt toegewezen geleverde Twilio URL, [http://twimlets.com/message][twimlet_message_url], naar de **Url** -variabele. Deze URL biedt een Twilio Markup Language (TwiML)-antwoord dat Twilio hoe informeert doorgaan met het gesprek. Bijvoorbeeld de TwiML die wordt geretourneerd kan bevatten een ** &lt;zeg&gt; ** werkwoord die het resultaat in tekst laten uitspreken voor de ontvanger. In plaats van met de URL van de geleverde Twilio kan u uw eigen service als u wilt reageren op verzoek van Twilio; maken voor meer informatie raadpleegt u [hoe u gebruik Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]. Meer informatie over TwiML kunt vinden op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over ** &lt;zeg&gt; ** en andere bewerkingen Twilio kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lees de richtlijnen van de beveiliging Twilio bij [https://www.twilio.com/docs/security][twilio_docs_security].

Zie voor meer informatie over Twilio, [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]
* [Een certificaat toevoegen aan de Java CA certificaat Store][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
