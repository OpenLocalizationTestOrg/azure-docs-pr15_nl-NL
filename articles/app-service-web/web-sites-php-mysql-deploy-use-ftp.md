<properties 
    pageTitle="Een PHP-MySQL-web-app maakt in Azure App-Service en implementeren met FTP" 
    description="Een zelfstudie die laat zien hoe een PHP web-app waarin de gegevens in MySQL en gebruik FTP-implementatie naar Azure maken." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Een PHP-MySQL-web-app maakt in Azure App-Service en implementeren met FTP

Deze zelfstudie ziet u hoe u een PHP-MySQL-web-app maken en het implementeren via FTP. Deze zelfstudie wordt ervan uitgegaan dat u hebt [PHP][install-php], [MySQL][install-mysql], een webserver, en een FTP-client op uw computer is geïnstalleerd. De instructies in deze zelfstudie kunnen op elk besturingssysteem, met inbegrip van Windows, Mac en Linux worden gevolgd. Na het voltooien van deze handleiding, hebt u een PHP/MySQL-web-app uitvoeren in Azure wordt aangegeven.
 
U leert:

* Het maken van een web-app en een MySQL-database met behulp van de Azure-Portal. Omdat PHP is standaard ingeschakeld in Web-Apps, verplicht niets speciale uw PHP-code uit te voeren.
* Hoe u uw Azure met FTP-toepassing publiceert.
 
Door deze zelfstudie te volgen, wordt u een eenvoudige registratie-web-app in PHP maken. De toepassing wordt in een Web-App worden gehost. Schermafbeelding van de voltooide toepassing is hieronder:

![Azure PHP-website][running-app]

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist, niet verplichtingen. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Een WebApp maken en publiceren op FTP instellen

Volg deze stappen om een web-app en een MySQL-database maken:

1. Meld u aan bij de [Portal van Azure][management-portal].
2. Klik op het pictogram **+ Nieuw** op de voorgrond links van de Azure-Portal.

    ![Nieuwe Azure website maken][new-website]

3. Typ in het zoeken **in de browser + MySQL** en klik op het **WebApp + MySQL**.

    ![Aangepaste maken een nieuwe website][custom-create]

4. Klik op **maken**. Voer de naam van een unieke app-service, een geldige naam voor de resourcegroep en een nieuwe serviceplan.

    ![De naam van de groep resource instellen][resource-group]


6. Voer waarden in voor uw nieuwe database, inclusief ermee akkoord met de juridische voorwaarden.

    ![Nieuwe MySQL-database maken][new-mysql-db]
    
7. Wanneer de web-app is gemaakt, ziet u het nieuwe blad voor app-service.


6. Klik op **Instellingen** > **implementatie referenties**. 

    ![Implementatie-referenties instellen][set-deployment-credentials]

7. Als u wilt publiceren op FTP inschakelt, moet u een gebruikersnaam en wachtwoord opgeven. Sla de referenties en noteer de gebruikersnaam en wachtwoord die u maakt.

    ![Publicatie-referenties maken][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Bouwen en testen van uw app lokaal

De aanvraag is een eenvoudige PHP-toepassing waarmee u te registreren voor een gebeurtenis doordat uw naam en e-mailadres. Informatie over het vorige geregistreerde wordt weergegeven in een tabel. Registratiegegevens wordt opgeslagen in een MySQL-database. De app bestaat uit twee bestanden:

* **index.php**: een formulier voor registratie en een tabel met registrant informatie wordt weergegeven.
* **CreateTable.php**: Hiermee maakt u de MySQL-tabel voor de toepassing. Dit bestand wordt slechts eenmaal worden gebruikt.

Als u wilt maken en uitvoeren van de app lokaal, de onderstaande stappen uit te voeren. Merk op dat deze stappen wordt ervan uitgegaan dat u hebt PHP, MySQL, en een webserver instellen op uw lokale computer, en die u hebt ingesteld dat de [Bob-extensie voor MySQL][pdo-mysql].

1. Maken van een MySQL-database genoemd `registration`. U kunt dit doen vanuit de MySQL-opdrachtprompt met deze opdracht:

        mysql> create database registration;

2. In de hoofdmap van de webserver, maakt u een map genaamd `registration` en maakt u twee bestanden in deze - een genoemd `createtable.php` en een genoemd `index.php`.

3. Open de `createtable.php` bestand in een teksteditor of IDE en de onderstaande code hebt toegevoegd. Deze code wordt gebruikt om te maken de `registration_tbl` van een tabel de `registration` database.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > U moet bijwerken van de waarden voor <code>$user</code> en <code>$pwd</code> met uw lokale MySQL-gebruikersnaam en wachtwoord.

4. Open een webbrowser en bladert u naar [http://localhost/registration/createtable.php][localhost-createtable]. Hiermee maakt u de `registration_tbl` tabel in de database.

5. Open het bestand **index.php** in een teksteditor of IDE en de eenvoudige HTML en CSS-code voor de pagina (de code PHP worden toegevoegd in de volgende stappen) hebt toegevoegd.

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. Voeg binnen de tags PHP, PHP code om verbinding te maken met de database.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Nogmaals, moet u de waarden voor bijwerken <code>$user</code> en <code>$pwd</code> met uw lokale MySQL-gebruikersnaam en wachtwoord.

7. Na de code van de database-verbinding, moet u code voor het invoegen van registratiegegevens in de database toevoegen.

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. Tot slot na de bovenstaande code code voor het ophalen van gegevens uit de database toevoegen.

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

U kunt nu bladeren naar [http://localhost/registration/index.php] [ localhost-index] om te testen van de app.

##<a name="get-mysql-and-ftp-connection-information"></a>MySQL- en FTP-verbinding-informatie opvragen

Verbinding maken met de MySQL-database die wordt uitgevoerd op de Web-Apps, uw is de verbindingsgegevens nodig. Als u informatie over de databaseverbinding MySQL, als volgt te werk:

1. Van de app-service wordt web app blade Klik op de koppeling van de groep resource:

    ![Selecteer resourcegroep][select-resourcegroup]

1. Vanuit de resourcegroep, klikt u op de database:

    ![Selecteer database][select-database]

2. Selecteer **Instellingen**in de database samenvatting, > **Eigenschappen**.

    ![Selecteer Eigenschappen][select-properties]
    
2. Noteer de waarden voor `Database`, `Host`, `User Id`, en `Password`.

    ![Notitie-eigenschappen][note-properties]

3. Vanuit uw web-app, klikt u op de koppeling **downloaden profiel publiceren** in de rechterbenedenhoek van de pagina:

    ![Download profiel publiceren][download-publish-profile]

4. Open de `.publishsettings` bestand in een XML-editor. 

3. Zoek de `<publishProfile >` element met `publishMethod="FTP"` die er ongeveer zo uitziet:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Noteer de `publishUrl`, `userName`, en `userPWD` kenmerken.

##<a name="publish-your-app"></a>Uw app publiceren

Nadat u uw app lokaal getest hebt, kunt u deze kunt publiceren naar uw web-app met FTP. Echter, moet u eerst de informatie over de databaseverbinding in de toepassing bijwerken. Met de informatie over de databaseverbinding u verkregen eerder (in de sectie **MySQL ophalen en informatie over de databaseverbinding FTP** ), de volgende informatie bijwerkt in **zowel** de `createdatabase.php` en `index.php` bestanden met de juiste waarden:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

U bent nu klaar om te publiceren van uw app met FTP.

1. Open uw FTP-client van keuze.

2. Voer het *host-gedeelte* van de `publishUrl` kenmerk die u hierboven hebt genoteerd in uw FTP-client.

3. Voer de `userName` en `userPWD` kenmerken die u hierboven hebt genoteerd ongewijzigd in uw FTP-client.

4. Verbinding maken.

Nadat u verbinding hebt gemaakt is mogelijk om te uploaden en downloaden van bestanden naar wens. Zorg ervoor dat u uploaden van bestanden naar de hoofdmap, dat wil zeggen `/site/wwwroot`.

Na het uploaden van beide `index.php` en `createtable.php`, blader naar **http://[site name].azurewebsites.net/createtable.php** u de MySQL-tabel voor de toepassing maken en blader naar **http://[site name].azurewebsites.net/index.php** moet beginnen met de webtoepassing.
 
## <a name="next-steps"></a>Volgende stappen

Zie het [PHP Developer Center](/develop/php/)voor meer informatie.

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
