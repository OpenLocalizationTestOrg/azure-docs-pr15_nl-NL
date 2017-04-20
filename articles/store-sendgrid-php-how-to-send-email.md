<properties 
    pageTitle="Het gebruik van de SendGrid e-mailservice (PHP) | Microsoft Azure" 
    description="Leer hoe verzenden van e-mail met de e-mailservice SendGrid op Azure. Voorbeelden van de code is geschreven in PHP." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Het gebruik van de SendGrid e-mailservice van PHP

Deze handleiding wordt getoond hoe algemene programming taken met de e-mailservice SendGrid uitvoeren op Azure. In de voorbeelden zijn in PHP geschreven.
De scenario's waarvoor zijn **het bouwen van e-mail**, **e-mail verzenden**en **bijlagen toevoegen**. Zie voor meer informatie over SendGrid en verzenden van e-mail, de sectie van de [Volgende stappen](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid e-mailservice?

SendGrid is een [cloudgebaseerde e-mailservice] die betrouwbare [bezorging van transacties e-mail], schaalbaarheid en realtime analytics samen met flexibele API's die aangepaste integratie gemakkelijker biedt. Gebruikelijke scenario's voor SendGrid-gebruik opnemen:

-   Automatisch bevestigingen verzenden naar klanten
-   Beheer van de verdeling lijsten voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen
-   Realtime aan de doelstellingen voor items, zoals geblokkeerde e- en klant serverreactie verzamelen
-   Rapporten om trends identificeren te genereren
-   Doorsturen van vragen van klanten
- E-mailmeldingen uit uw toepassing

Zie [https://sendgrid.com][]voor meer informatie.

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Gebruik van SendGrid uit uw PHP-toepassing

SendGrid gebruikt in een toepassing Azure PHP, is geen speciale vereist configuratie of programmeren. Omdat SendGrid een service is, kan deze worden geopend op precies dezelfde manier vanuit een cloud-toepassing zoals deze vanuit een on-premises implementatie-toepassing kunt.

## <a name="how-to-send-an-email"></a>Hoe u: een e-mailbericht verzenden

U kunt e-mail met SMTP of de Web-API die is verstrekt door SendGrid verzenden.

### <a name="smtp-api"></a>SMTP-API

E-mail te verzenden de SendGrid SMTP-API gebruiken, gebruikt u *Swift e-mailprogramma*, een bibliotheek op basis van onderdelen voor het verzenden van e-mailberichten vanuit PHP-toepassingen. U kunt de bibliotheek *Swift e-mailprogramma* downloaden van [http://swiftmailer.org/download][] v5.3.0 (Gebruik [Composer] Swift e-mailprogramma installeren). Verzenden van e-mail met de bibliotheek worden gemaakt van exemplaren van de <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_e-mailprogramma</span>, en <span class="auto-style2">Swift\_bericht</span> klassen, de gewenste eigenschappen instellen en u belt de <span class="auto-style2">Swift\_Mailer::send</span> methode.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Web API

Gebruik van PHP [omslaan van de functie][] om de SendGrid Web-API gebruiken e-mail te verzenden.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid van Web API is vergelijkbaar met een REST API, hoewel het is niet echt een RESTful API aangezien beide ophalen in de meeste bellen, en POST werkwoorden door elkaar kunnen worden gebruikt.

## <a name="how-to-add-an-attachment"></a>Hoe u: een bijlage toevoegen

### <a name="smtp-api"></a>SMTP-API

Een bijlage de SMTP-API gebruiken verzenden heeft betrekking op één extra regel met code aan het voorbeeldscript voor het verzenden van een e-mailbericht met Swift e-mailprogramma.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

De extra regel met code is als volgt:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Deze regel met code oproepen de methode bijvoegen op het <span class="auto-style2">Swift\_bericht</span> object en het gebruik van statische methode <span class="auto-style2">fromPath</span> op de <span class="auto-style2">Swift\_bijlage</span> class voor het ophalen en een bestand toevoegen aan een bericht.

### <a name="web-api"></a>Web API

Een bijlage de Web-API gebruiken verzenden is vergelijkbaar met het verzenden van een e-mailbericht de Web-API gebruiken. Bedenk wel dat in het volgende voorbeeld, de parametermatrix moet bevatten met dit element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Voorbeeld:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Hoe u: Filters gebruiken om in te schakelen voetteksten, bijhouden en Analytics

SendGrid biedt extra e-functionaliteit met behulp van 'filters'. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit zoals het inschakelen van Klik op het bijhouden van wijzigingen, Google analytics, abonnement bijhouden, enzovoort.

Filters kunnen worden toegepast op een bericht met behulp van de eigenschap filters. Alle filters is opgegeven met een hash met filter taalspecifieke instellingen. Het volgende voorbeeld kunt het filter voettekst en Hiermee geeft u een SMS-bericht dat naar het einde van het e-mailbericht wordt toegevoegd.
In dit voorbeeld gebruiken we [sendgrid-php-bibliotheek].
Gebruik [Composer] bibliotheek installeren:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Voorbeeld:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd de basisprincipes van de SendGrid e-mailservice, volgt u deze koppelingen voor meer informatie.

-   SendGrid documentatie: <https://sendgrid.com/docs>
-   SendGrid PHP bibliotheek: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid speciale aanbieding voor klanten met Azure: <https://sendgrid.com/windowsazure.html>

Zie ook het [PHP Developer Center](/develop/php/)voor meer informatie.


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [functie omslaan]: http://php.net/curl
  [cloud gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
  [bezorging van transacties e-mail]: https://sendgrid.com/transactional-email
  [sendgrid-php-bibliotheek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Composer]: https://getcomposer.org/download/
