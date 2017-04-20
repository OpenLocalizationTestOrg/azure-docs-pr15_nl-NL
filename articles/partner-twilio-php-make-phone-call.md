<properties
    pageTitle="Hoe u een telefoongesprek starten vanuit Twilio (PHP) | Microsoft Azure"
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden zijn geschikt voor PHP-toepassing."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hoe u een telefoongesprek starten Twilio gebruiken in een PHP-toepassing op Azure

Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken om een gesprek te vanaf een webpagina voor PHP gehost in Azure wordt aangegeven. De resulterende toepassing krijgt de gebruiker voor telefoongesprek waarden, zoals wordt weergegeven in de volgende schermopname.

![Azure gesprek formulier Twilio en PHP gebruiken][twilio_php]

U moet de volgende handelingen uit als u de code gebruiken in dit onderwerp wilt doen:

1. In het bezit van een Twilio-account en verificatie token. Om te beginnen met Twilio evalueren prijzen bij [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich registreren voor een proefaccount bij [https://www.twilio.com/try-twilio][try_twilio]. Zie voor informatie over de API die is verstrekt door Twilio [http://www.twilio.com/api][twilio_api].
2. De [bibliotheek Twilio voor PHP](https://github.com/twilio/twilio-php) verkrijgen of installeren als een pakket PERENBOMEN. Zie voor meer informatie het [Leesmij-bestand](https://github.com/twilio/twilio-php/blob/master/README.md).
3. De Azure SDK voor PHP installeren. Zie [de SDK Azure voor PHP instellen]voor een overzicht van de SDK en instructies voor het installeren van deze,[setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Een webformulier voor het maken van een gesprek maken

De volgende HTML-code ziet u het maken van een webpagina (**callform.html**) die worden opgehaald gebruikersgegevens voor u een oproep plaatst:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>De code als u het gesprek wilt maken
De volgende code ziet u het maken van een webpagina (**makecall.php**) die wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door **callform.html**indient. Onderstaande code wordt gemaakt van het oproep-bericht en genereert het gesprek. (Gebruik uw Twilio-account en verificatie in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan **$sid** en **$token** in de onderstaande code token.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Behalve als u de oproep plaatst, geeft **makecall.php** sommige gesprek metagegevens (Zie voorbeeld in onderstaande schermopname weergegeven). Zie voor meer informatie over gesprek metagegevens, [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure gesprek antwoord Twilio en PHP gebruiken][twilio_php_response]

## <a name="run-the-application"></a>Voer de toepassing
De volgende stap wordt geïmplementeerd uw toepassing naar Azure-Websites. De volgende artikelen bevatten de gegevens voor een website maken en implementeren van uw code met cijfer, FTP of WebMatrix (maar niet alle informatie in elke artikel relevant is):

* [Een PHP-MySQL Azure-website maken en implementeren cijfer gebruiken][website-git]
* [Een Azure PHP-MySQL-website maken en implementeren met FTP][website-ftp]

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om aan te geven basisfunctionaliteit Twilio gebruiken in PHP op Azure. Voordat u de implementatie in Azure in productie, kunt u meer foutafhandeling of andere onderdelen toevoegen. Bijvoorbeeld:

* In plaats van een webformulier gebruikt, kunt u opslagruimte Azure BLOB's of SQL-Database kunt gebruiken voor het opslaan van telefoonnummers en tekst te bellen. Zie voor informatie over het gebruik van opslag in Azure BLOB's in PHP [Azure-opslag gebruiken met PHP toepassingen][howto_blob_storage_php]. Zie voor informatie over het gebruik van de SQL-Database in PHP [SQL-Database gebruiken met PHP toepassingen][howto_sql_azure_php].
* De code **makecall.php** geleverde Twilio URL wordt gebruikt ([http://twimlets.com/message][twimlet_message_url]) een reactie Twilio Markup Language (TwiML) melding Twilio hoe verder te gaan met het gesprek wordt aan te geven. Bijvoorbeeld de TwiML die wordt geretourneerd kan bevatten een `<Say>` werkwoord die het resultaat in tekst laten uitspreken voor de ontvanger. In plaats van met de URL van de geleverde Twilio kan u uw eigen service als u wilt reageren op verzoek van Twilio; maken voor meer informatie raadpleegt u [hoe u gebruik Twilio voor spraak- en SMS-mogelijkheden in PHP][howto_twilio_voice_sms_php]. Meer informatie over TwiML kunt vinden op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over `<Say>` en andere bewerkingen Twilio kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lees de richtlijnen van de beveiliging Twilio bij [https://www.twilio.com/docs/security][twilio_docs_security].

Zie voor meer informatie over Twilio, [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
