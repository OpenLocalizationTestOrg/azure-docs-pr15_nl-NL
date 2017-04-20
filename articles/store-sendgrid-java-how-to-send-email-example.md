<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-email-example" 
    description="Hoe u e-mail verzenden met SendGrid van Java in een Azure-implementatie" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Hoe u E-mail verzenden met SendGrid van Java in een Azure-implementatie

Het volgende voorbeeld ziet u hoe u SendGrid kunt gebruiken om te verzenden van e-mailberichten vanaf een webpagina die wordt gehost in Azure wordt aangegeven. De resulterende toepassing wordt de gebruiker voor e-waarden, zoals wordt weergegeven in de volgende schermopname.

![E-mailformulier][emailform]

Het e-mailbericht ziet er ongeveer de volgende schermafbeelding die u uit.

![E-mailbericht][emailsent]

U moet de volgende handelingen uit als u de code gebruiken in dit onderwerp wilt doen:

1. Ophalen met de potten javax.mail, bijvoorbeeld <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. De potten toevoegen aan uw Java opbouwen pad.
3. Als u Eclips maken van deze toepassing Java gebruikt, kunt u de bibliotheken SendGrid opnemen in uw toepassing implementatie-bestand (WAR) met behulp van de Eclips implementatie constructie-functie. Als u niet Eclips gebruikt om te maken van deze toepassing Java, controleert u de bibliotheken zijn opgenomen in dezelfde Azure rol heeft als uw Java-toepassing en toegevoegd aan het pad class van uw toepassing.


U moet ook uw eigen SendGrid gebruikersnaam en wachtwoord hebben, kunnen het e-mailbericht verzenden. Zie [e-mail versturen via SendGrid van Java](store-sendgrid-java-how-to-send-email.md)om te beginnen met SendGrid.

Daarnaast kunnen wordt bekend zijn met de informatie bij het [maken van een Hallo wereld-toepassing voor Azure in Eclips](http://msdn.microsoft.com/library/windowsazure/hh690944)of met andere technieken voor het hosten van Java-toepassingen in Azure wordt aangegeven als u niet Eclips gebruikt, ten zeerste aanbevolen.

## <a name="create-a-web-form-for-sending-email"></a>Een webformulier voor het verzenden van e-mailbericht maken

De volgende code ziet hoe u een webformulier om op te halen gebruikersgegevens voor het verzenden van e-mail maken. Voor doeleinden van dit inhoudstype heet het bestand JSP **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>De code als u wilt verzenden, het e-mailbericht maken

De volgende code, die wordt aangeroepen wanneer u het formulier in emailform.jsp hebt voltooid, wordt het e-mailbericht gemaakt en verzonden. Voor doeleinden van dit inhoudstype heet het bestand JSP **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Naast het e-mailbericht verzendt, vindt emailform.jsp u een resultaat voor de gebruiker. een voorbeeld is de volgende schermopname:

![Resultaat van de e-mail verzenden][emailresult]

## <a name="next-steps"></a>Volgende stappen

Uw toepassing naar de emulator berekeningscluster implementeren uitvoeren vanuit een browser emailform.jsp, waarden invoeren in het formulier, klikt u op **Dit e-mailbericht verzenden**en resultaten in sendemail.jsp vervolgens weer te geven.

Deze code is opgegeven om aan te geven het gebruik van SendGrid in Java op Azure. Voordat u de implementatie in Azure in productie, kunt u meer foutafhandeling of andere onderdelen toevoegen. Bijvoorbeeld: 

* U kunt opslagruimte Azure BLOB's of SQL-Database voor de opslag van e-mailadressen en e-mailberichten, in plaats van via een formulier. Zie voor informatie over het gebruik van opslag in Azure BLOB's in Java [het gebruik van de Blob Storage-Service van Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Zie voor informatie over het gebruik van de SQL-Database in Java [SQL-Database gebruiken in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* U kunt `RoleEnvironment.getConfigurationSettings` SendGrid gebruikersnaam en wachtwoord ophalen uit van uw implementatie configuratie-instellingen, in plaats van het webformulier gebruiken om op te halen die waarden. Voor informatie over de `RoleEnvironment` klasse, raadpleegt u [de bibliotheek Azure Service Runtime in JSP gebruikt](http://msdn.microsoft.com/library/windowsazure/hh690948) en de Runtime van Azure-Service-pakket-documentatie op <http://dl.windowsazure.com/javadoc>.
* Zie voor meer informatie over het gebruik van SendGrid in Java [e-mail versturen via SendGrid van Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
