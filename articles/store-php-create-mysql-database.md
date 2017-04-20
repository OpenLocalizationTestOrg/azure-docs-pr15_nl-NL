<properties
    pageTitle="Maken en verbinding maken met een MySQL-database in Azure wordt aangegeven"
    description="Informatie over het gebruik van de Azure-portal maken van een MySQL-database en maak verbinding met deze vanuit een PHP-web-app in Azure wordt aangegeven."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Maken en verbinding maken met een MySQL-database in Azure wordt aangegeven

Deze zelfstudie wordt getoond hoe u een MySQL-database maakt in de [portal van Azure](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) en hoe u verbinding maken met deze vanuit een PHP WebApp met [App-Azure-Service](./app-service/app-service-value-prop-what-is.md). 

> [AZURE.NOTE] U kunt ook een MySQL-database als onderdeel van een [sjabloon voor Marketplace-app](./app-service-web/app-service-web-create-web-app-from-marketplace.md)maken.

## <a name="create-a-mysql-database-in-azure-portal"></a>Een MySQL-database maken in Azure-portal

U maakt een MySQL-database in de portal van Azure door het volgende doen:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Klik op **Nieuw**in het linkermenu > **gegevens + opslagruimte** > **MySQL-Database**.

    ![Een MySQL-database maken in Azure - starten](./media/store-php-create-mysql-database/create-db-1-start.png)

2. In de nieuwe MySQL-Database [blade](azure-portal-overview.md), als volgt uw nieuwe MySQL-database configureren (*blade*: een pagina in de portal dat wordt geopend horizontaal):

    - **Databasenaam**: Typ een unieke herkenbare naam
    - **Abonnement**: Kies het abonnement te gebruiken
    - **Database-Type**: Selecteer **gedeeld** voor lage kosten of gratis lagen of **speciaal** om speciale resources. 
    - **Resourcegroep**: de MySQL-database toevoegen aan een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md) of zet dit in een nieuwe record. Resources in dezelfde groep kunnen eenvoudig worden beheerd samen.
    - **Locatie**: Selecteer een locatie dicht bij u. Wanneer u deze toevoegt aan een bestaande resourcegroep, bent u naar de locatie van de resourcegroep vergrendeld.
    - **Laag prijzen**: **Prijzen laag**, klikt u op Selecteer een optie voor prijzen (**kwik** laag is gratis), en klik vervolgens op **selecteren**. 
    - **Juridische voorwaarden**: klik op de **Juridische voorwaarden**, Controleer de details aanschaffen en klikt u op **kopen**.
    - **Vastmaken aan het dashboard**: Selecteer als u wilt deze rechtstreeks vanuit het dashboard te openen. Dit is vooral handig als u niet bekend zijn met nog portal navigatie.
    
    De volgende schermafbeelding is slechts een voorbeeld van hoe u uw MySQL-database kunt configureren.  
    ![Een MySQL-database maken in Azure - configureren](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Als u klaar bent met configureren, klikt u op **maken**.

    ![Een MySQL-database maken in Azure - maken](./media/store-php-create-mysql-database/create-db-3-create.png)

    Hier ziet u een pop-upmelding dat die u weet dat de implementatie is gestart.

    ![Een MySQL-database maken in Azure - Bezig](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Krijgt u een andere pop-zodra implementatie is voltooid. Uw blade MySQL-database wordt ook automatisch geopend door de portal.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Verbinding maken met uw MySQL-database

Als u de verbindingsinformatie voor uw nieuwe MySQL-database, klikt u op **Eigenschappen** in uw web-app blade.
    
![Een MySQL-database maken in Azure - MySQL-database blade](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

U kunt nu verbindingsgegevens gebruiken in een WebApp. Een steekproef die wordt uitgelegd hoe u de gegevens van een eenvoudige PHP-app beschikbaar is [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Verbinding maken met een Laravel web-app (vanuit de PHP get gestart zelfstudie)

Stel dat u alleen de zelfstudie [maken, configureert, en een PHP-web-app naar Azure implementeren](./app-service-web/app-service-web-php-get-started.md) klaar en hebben een [Laravel](https://www.laravel.com/) WebApp uitgevoerd in Azure wordt aangegeven. U kunt eenvoudig mogelijkheden van een database toevoegen aan uw Laravel-app. Volg de onderstaande stappen:

>[AZURE.NOTE] De volgende stappen wordt ervan uitgegaan dat u de zelfstudie [maken, configureert, en een PHP-web-app naar Azure implementeren hebt](./app-service-web/app-service-web-php-get-started.md).

1. De app Laravel in uw lokale ontwikkelomgeving zodat deze verwijzen naar de MySQL-database configureren. Klik hiertoe open `.env` uit uw Laravel-app de hoofdmap en configureren van de opties voor MySQL-database.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] De naam van uw MySQL-database mogelijk of mogelijk niet zoals wordt weergegeven in het veld **Naam van de DATABASE** in het blad **Eigenschappen** . Is het beter is om te controleren van de Database-parameter in het veld **VERBINDINGSREEKS** . 
    >
    >![Een MySQL-database maken in Azure - Bezig](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. De snelste manier om te controleren of u kunt beschikken MySQL nu is [de Laravel standaard verificatie steigers](https://laravel.com/docs/5.2/authentication#authentication-quickstart)gebruiken. Voer de volgende opdrachten vanaf de hoofdmap van uw Laravel-app in de opdrachtregel terminal:

         php artisan migrate
         php artisan make:auth

    De eerste opdracht maakt de tabellen in Azure op basis van vooraf gedefinieerde migraties in de `database/migrations` directory en de tweede opdracht scaffolds de eenvoudige weergaven en routes voor gebruikersregistratie en verificatie.

3. De ontwikkelingsserver nu uitvoeren:

        php artisan serve

4. Ga naar http://localhost:8000 en een nieuwe gebruiker registreren, zoals wordt weergegeven in de browser:

    ![Verbinding maken met een MySQL-database in Azure - gebruiker registreren](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Volg de volledige UI-prompt de registratie. Zodra registratie is voltooid, wordt u zijn aangemeld.
    
    ![Verbinding maken met een MySQL-database in Azure - gebruiker registreren](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    U bent nu ontwikkelen van uw app ten opzichte van de MySQL-database in Azure wordt aangegeven.

5. Nu u moet alleen repliceren uw `.env` instellingen bij uw Azure web-app. Voer de volgende Azure CLI-opdrachten:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Ontdek hoe dit in [configureren de Azure WebApp werkt](./app-service-web/app-service-web-php-get-started.md#configure).

6. Vervolgens doorvoeren en push naar Azure de lokale wijzigingen eerder tijdens de uitvoering `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Blader naar de Azure WebApp.

        azure site browse

8. Meld u aan met de referenties van de gebruiker die u eerder hebt gemaakt.

    ![Verbinding maken met MySQL-database in Azure - bladeren naar Azure WebApp](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Nadat u zich hebt aangemeld, ziet u het beschrijvende post-aanmeldingsvenster.
    
    ![Verbinding maken met een MySQL-database in Azure - aangemeld](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gefeliciteerd, uw PHP web-app in Azure is nu toegang tot gegevens vanaf uw MySQL-database. 

## <a name="next-steps"></a>Volgende stappen

Zie het [PHP Developer Center](/develop/php/)voor meer informatie.
