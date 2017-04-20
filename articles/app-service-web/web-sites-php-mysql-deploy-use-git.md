<properties
    pageTitle="Een PHP-MySQL-web-app maakt in Azure App-Service en implementeren cijfer gebruiken"
    description="Een zelfstudie die laat zien hoe een PHP web-app worden gegevens in MySQL opgeslagen maken en gebruiken van cijfer implementatie naar Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Een PHP-MySQL-web-app maakt in Azure App-Service en implementeren cijfer gebruiken

Deze zelfstudie wordt getoond hoe een PHP-MySQL-web-app maken en hoe u het dashboard implementeren naar [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) cijfer gebruiken. Gewenste [PHP][install-php], het hulpmiddel MySQL-opdrachtregel (een deel van [MySQL][install-mysql]), en [cijfer] [ install-git] op uw computer is geïnstalleerd. De instructies in deze zelfstudie kunnen op elk besturingssysteem, met inbegrip van Windows, Mac en Linux worden gevolgd. Na het voltooien van deze handleiding, hebt u een PHP/MySQL-web-app uitvoeren in Azure wordt aangegeven.

U leert:

* Het maken van een web-app en een MySQL-database met behulp van de [Portal van Azure][management-portal]. Omdat PHP is standaard ingeschakeld in [Web-Apps voor App-Service](http://go.microsoft.com/fwlink/?LinkId=529714) , verplicht niets speciale uw PHP-code uit te voeren.
* Het publiceren en uw toepassing Azure met cijfer opnieuw publiceren.
* Het inschakelen van de extensie Composer automatiseren Composer taken bij elke `git push`.

Door deze zelfstudie te volgen, wordt u een eenvoudige registratie-web-app in PHP maken. De toepassing wordt in Web-Apps worden gehost. Schermafbeelding van de voltooide toepassing is hieronder:

![Azure PHP-website][running-app]

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Deze zelfstudie wordt ervan uitgegaan dat u hebt [PHP][install-php], het hulpmiddel MySQL-opdrachtregel (een deel van [MySQL][install-mysql]), en [cijfer] [ install-git] op uw computer is geïnstalleerd.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Een WebApp maken en publiceren op cijfer instellen

Volg deze stappen om een web-app en een MySQL-database maken:

1. Meld u aan bij de [Portal van Azure][management-portal].
2. Klik op het pictogram **Nieuw** .

3. Klik op **Alle Zie** naast **Marketplace**. 

4. Klik op **Web + mobiele** **app + MySQL met webonderdelen**. Klik vervolgens op **maken**.

4. Voer een geldige naam voor de resourcegroep.

    ![De naam van de groep resource instellen][resource-group]

5. Voer waarden in voor de nieuwe WebApp.

    ![WebApp maken][new-web-app]

6. Voer waarden in voor uw nieuwe database, inclusief ermee akkoord met de juridische voorwaarden.

    ![Nieuwe MySQL-database maken][new-mysql-db]

7. Wanneer de web-app is gemaakt, ziet u het nieuwe blad van de web-app.

7. In **Instellingen** op **Doorlopend implementatie**, klik op _configureren instellingen vereist_.

    ![Cijfer publicatie instellen][setup-publishing]

8. Selecteer **Lokale cijfer opslagplaats** voor de bron.

    ![Cijfer bibliotheek instellen][setup-repository]


9. Als u wilt publiceren op cijfer inschakelt, moet u een gebruikersnaam en wachtwoord opgeven. Noteer de gebruikersnaam en wachtwoord die u maakt. (Als u een cijfer opslagplaats voordat u hebt ingesteld, wordt deze stap overgeslagen.)

    ![Publicatie-referenties maken][credentials]


## <a name="get-remote-mysql-connection-information"></a>Externe MySQL-verbindingsgegevens ophalen

Verbinding maken met de MySQL-database die wordt uitgevoerd op de Web-Apps, uw is de verbindingsgegevens nodig. Als u informatie over de databaseverbinding MySQL, als volgt te werk:

1. Vanuit de resourcegroep, klikt u op de database:

    ![Selecteer database][select-database]

2. Uit de database **Instellingen**, selecteert u **Eigenschappen**.

    ![Selecteer Eigenschappen][select-properties]

2. Noteer de waarden voor `Database`, `Host`, `User Id`, en `Password`.

    ![Notitie-eigenschappen][note-properties]

## <a name="build-and-test-your-app-locally"></a>Bouwen en testen van uw app lokaal

Nu dat u een web-app hebt gemaakt, kunt u uw toepassing lokaal ontwikkelen en vervolgens het dashboard implementeren na testen.

De aanvraag is een eenvoudige PHP-toepassing waarmee u te registreren voor een gebeurtenis doordat uw naam en e-mailadres. Informatie over het vorige geregistreerde wordt weergegeven in een tabel. Registratiegegevens wordt opgeslagen in een MySQL-database. De toepassing bestaat uit één bestand (beschikbaar onder kopiëren/plakken code):

* **index.php**: een formulier voor registratie en een tabel met registrant informatie wordt weergegeven.

Als u wilt maken en uitvoeren van de toepassing lokaal, de onderstaande stappen uit te voeren. Houd er rekening mee dat deze stappen wordt ervan uitgegaan dat u hebt de PHP en MySQL opdrachtregel hulpmiddel (onderdeel van MySQL) instellen op uw lokale computer en dat u hebt ingesteld dat de [Bob-extensie voor MySQL][pdo-mysql].

1. Verbinding maken met de externe MySQL-server, met de waarde voor `Data Source`, `User Id`, `Password`, en `Database` die u eerder hebt opgehaald:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. De MySQL-opdrachtprompt worden weergegeven:

        mysql>

3. Plakken in de volgende `CREATE TABLE` opdracht maken de `registration_tbl` tabel in uw database:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. In de hoofdmap van de map lokale toepassing **index.php** -bestand te maken.

5. Open het bestand **index.php** in een teksteditor of IDE en voeg de volgende code toe en voert u de benodigde wijzigingen die zijn gemarkeerd met `//TODO:` opmerkingen.


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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

4.  Ga naar de toepassingsmap in een terminal en typ de volgende opdracht:

        php -S localhost:8000

U kunt nu bladeren naar **http://localhost:8000 /** om de toepassing te testen.


## <a name="publish-your-app"></a>Uw app publiceren

Nadat u uw app lokaal getest hebt, kunt u deze naar de Web-Apps met cijfer publiceren. U uw lokale cijfer opslagplaats geïnitialiseerd en publiceer de toepassing.

> [AZURE.NOTE]
> Hierna ziet u de dezelfde stappen die worden weergegeven in de Portal Azure aan het einde van het een WebApp maken en de Set up cijfer publicerende sectie hierboven.

1. (Optioneel)  Als u bent vergeten of de URL van de externe repostitory cijfer bent kwijtgeraakt, gaat u naar de eigenschappen van het web-app op de Azure-Portal.

1. Open GitBash (of een terminal als cijfer is in uw `PATH`), mappen naar de hoofdmap van de toepassing wijzigen en voer de volgende opdrachten:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt.

    ![Eerste Push naar Azure via cijfer][git-initial-push]

2. Blader naar **http://[site name].azurewebsites.net/index.php** moet beginnen met de toepassing (deze informatie wordt opgeslagen op uw dashboard account):

    ![Azure PHP-website][running-app]

Nadat u uw app hebt gepubliceerd, kunt u wijzigingen aanbrengt in deze begint en gebruikt cijfer publiceren.

## <a name="publish-changes-to-your-app"></a>Wijzigingen publiceren naar uw app

Als u wijzigingen wilt uw app publiceren, als volgt te werk:

1. Wijzigingen aanbrengen in uw app lokaal.
2. Open GitBash (of een terminal it cijfer is in uw `PATH`), mappen wijzigen naar de hoofdmap van de app en voer de volgende opdrachten:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt.

    ![Sitewijzigingen naar Azure doordat via cijfer][git-change-push]

3. Blader naar **http://[site name].azurewebsites.net/index.php** om te zien van uw app en de wijzigingen die u hebt gemaakt:

    ![Azure PHP-website][running-app]

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Composer automatisering met de extensie Composer inschakelen

Standaard het installatieproces cijfer in App Service heeft geen andere items met composer.json, als er een in uw project PHP. U kunt inschakelen composer.json processing tijdens `git push` doordat de extensie Composer.

1. In uw PHP web-app van blade in de [portal van Azure][management-portal], klik op **Extra** > **uitbreidingen**.

    ![Composer extensie-instellingen][composer-extension-settings]

2. Klik op **toevoegen**en klik op **Composer**.

    ![Composer extensie toevoegen][composer-extension-add]
    
3. Klik op **OK** om te accepteren juridische voorwaarden. Klik op **OK** om opnieuw toe te voegen van de extensie.

    Het blad **geïnstalleerde extensies** wordt nu weergegeven voor de extensie Composer.  
    ![Composer extensie weergeven][composer-extension-view]
    
4. Nu uitvoeren `git add`, `git commit`, en `git push` zoals in de vorige sectie. U ziet nu dat Composer afhankelijkheden die zijn gedefinieerd in composer.json wordt geïnstalleerd.

    ![Composer extensie Success][composer-extension-success]

## <a name="next-steps"></a>Volgende stappen

Zie het [PHP Developer Center](/develop/php/)voor meer informatie.

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
