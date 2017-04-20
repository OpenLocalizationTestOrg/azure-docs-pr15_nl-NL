<properties 
    pageTitle="Het gebruik van de SendGrid e-mailservice (.NET) | Microsoft Azure" 
    description="Leer hoe verzenden van e-mail met de e-mailservice SendGrid op Azure. Voorbeelden geschreven in C#-code en de .NET-API gebruiken." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Hoe u E-mail verzenden via SendGrid met Azure


## <a name="overview"></a>Overzicht

Deze handleiding wordt getoond hoe algemene programming taken met de e-mailservice SendGrid uitvoeren op Azure. In de voorbeelden zijn geschreven in C\#
en de .NET-API gebruiken. De scenario's waarvoor opnemen **het bouwen van e-mail**, **e-mail verzenden**, **bijlagen toevoegen**en **filters gebruiken**. Zie voor meer informatie over SendGrid en verzenden van e-mail, de sectie van de [volgende stappen][] .

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid e-mailservice?

SendGrid is een [cloudgebaseerde e-mailservice] die betrouwbare [bezorging van transacties e-mail], schaalbaarheid en realtime analytics samen met flexibele API's die aangepaste integratie gemakkelijker biedt. Gebruikelijke scenario's voor SendGrid-gebruik opnemen:

-   Automatisch bevestigingen verzenden naar klanten.
-   Beheer van de verdeling bevat voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen.
-   Het verzamelen van realtime aan de doelstellingen voor items, zoals geblokkeerde e-mail en klant serverreactie.
-   Rapporten om trends identificeren te genereren.
-   Het doorsturen van vragen van klanten.
-   Verwerking van binnenkomende e-mailberichten.

Zie voor meer informatie, [https://sendgrid.com](https://sendgrid.com) of onze [C#-bibliotheek][sendgrid-csharp]

## <a name="create-a-sendgrid-account"></a>Een SendGrid-account maken

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Verwijst naar de bibliotheek van de klasse SendGrid .NET

Het [pakket SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) is de eenvoudigste manier om de API SendGrid en voor het configureren van uw toepassing met alle afhankelijkheden. NuGet is inbegrepen in het Visual Studio 2015 Microsoft, waarmee u gemakkelijk installeren en bijwerken van bibliotheken en hulpprogramma's voor een Visual Studio-extensie. 

> [AZURE.NOTE] Als u wilt installeren NuGet als u een eerdere versie van Visual Studio dan Visual Studio-2015 uitvoert, gaat u naar [http://www.nuget.org](http://www.nuget.org)en klik op de knop **Installeren NuGet** .

Ga als volgt te werk als u wilt het pakket SendGrid NuGet in uw toepassing installeert:

1.  Een nieuw project gemaakt.

    ![Een nieuw project maken][create-new-project]

2.  Selecteer een sjabloon.

    ![Selecteer een sjabloon][select-a-template]

3.  Klik in **Solution Explorer**met de rechtermuisknop op **verwijzingen**en klik **NuGet-pakketten beheren**.

4.  Zoeken naar **SendGrid** en selecteer het item **SendGrid** in de lijst met resultaten.

    ![SendGrid NuGet pakket][SendGrid-NuGet-package]

5.  Klik op **installeren** om de installatie te voltooien en sluit dit dialoogvenster.

SendGrid van .NET-klassenbibliotheek heet **SendGridMail**. Bevat de volgende naamruimten:

-   **SendGridMail** voor het maken en werken met e-mailitems.
-   **SendGridMail.Transport** voor het verzenden van e-mailbericht met het **SMTP-** protocol, of het protocol HTTP 1.1 met **Web/REST**.

De volgende code naamruimtedeclaraties toevoegen aan het begin van elke C\# bestand waarin u wilt via programmacode toegang tot de SendGrid e-mailservice.
**System.Net** en **System.Net.Mail** zijn in .NET Framework-naamruimten die opgenomen zijn omdat u het meest met de SendGrid APIs gebruiken zult typen opties omvatten.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Hoe u: een e-mailbericht maken

Gebruik het object **SendGridMessage** maken van een e-mailbericht. Nadat het berichtobject is gemaakt, kunt u eigenschappen en methoden, inclusief de afzender van een e, het e-mailgeadresseerde, en het onderwerp en de hoofdtekst van het e-mailbericht instellen.

Het volgende voorbeeld wordt getoond hoe een volledig ingevulde e-object maken:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Zie voor meer informatie over alle eigenschappen en methoden die worden ondersteund door het type **SendGrid** , [sendgrid-csharp][] op GitHub.

## <a name="how-to-send-an-email"></a>Hoe u: een e-mailbericht verzenden

Na het maken van een e-mailbericht, kunt u deze via de Web-API die is verstrekt door SendGrid verzenden. U kunt ook [gebruiken. NET van ingebouwd in bibliotheek](https://sendgrid.com/docs/Code_Examples/csharp.html).

Verzenden van e-mail is vereist dat u opgeeft dat uw accountreferenties SendGrid (gebruikersnaam en wachtwoord) of de SendGrid API-sleutel. API-sleutel is de voorkeursmethode. Als u meer informatie over het configureren van API toetsen, gaat u naar onze [documentatie](https://sendgrid.com/docs/Classroom/Send/api_keys.html) nodig hebt

U opslaan deze referenties via de Portal van uw Azure door te klikken configureren en de toets/waardeparen onder "app-instellingen" toevoegen.

 ![Azure app-instellingen][azure_app_settings]

 Moet u mogelijk deze openen als volgt: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Gebruik van referenties:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

API-sleutel gebruiken:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


De volgende voorbeelden wordt getoond hoe u een bericht verzenden met de Web-API.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Hoe u: een bijlage toevoegen

Bijlagen kunnen worden toegevoegd aan een bericht door te bellen van de methode **AddAttachment** en de naam en het pad van het bestand dat u wilt bijvoegen op te geven.
U kunt meerdere bijlagen opnemen door de ondersteuning voor deze methode wanneer voor elk bestand dat u wilt koppelen. Het volgende voorbeeld ziet een bijlage toevoegen aan een bericht:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
U kunt ook bijlagen toevoegen uit de gegevens van de **Stream**. Dit kan worden uitgevoerd door de ondersteuning voor dezelfde methode als bovenstaande **AddAttachment**, maar dan met doorgeven in de stroom van de gegevens en de bestandsnaam moet worden weergeven zoals in het bericht. In dit geval moet u de bibliotheek System.IO toevoegen.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Hoe u: apps gebruiken om in te schakelen voetteksten, bijhouden en analytics

SendGrid biedt extra e-functionaliteit met behulp van apps. Dit zijn instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit zoals Klik op het bijhouden van wijzigingen, Google analytics, abonnement bijhouden, enzovoort. Zie voor een volledige lijst met apps, [App-instellingen][].

Apps kunnen worden toegepast op **SendGrid** e-mailberichten met behulp van methoden geïmplementeerd als onderdeel van de klasse **SendGrid** .

De volgende voorbeelden laten zien van de voettekst en klik op filters bijhouden:

### <a name="footer"></a>Voettekst

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klik op bijhouden

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Hoe u: extra SendGrid services gebruiken

SendGrid biedt op het web API's en webhooks die u gebruiken kunt om te profiteren van extra SendGrid functionaliteit uit uw Azure-toepassing. Zie de [SendGrid API-documentatie][]voor volledige informatie.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd de basisprincipes van de SendGrid e-mailservice, volgt u deze koppelingen voor meer informatie.

*   SendGrid C\# bibliotheek cessies‑retrocessies: [sendgrid-csharp][]
*   SendGrid API-documentatie: <https://sendgrid.com/docs>
*   SendGrid speciale aanbieding voor klanten met Azure: [https://sendgrid.com](https://sendgrid.com)

  [Volgende stappen]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [App-instellingen]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API-documentatie]: https://sendgrid.com/docs
  
  [cloud gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
  [bezorging van transacties e-mail]: https://sendgrid.com/transactional-email
 
