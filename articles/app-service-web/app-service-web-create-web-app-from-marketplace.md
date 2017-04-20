<properties
    pageTitle="Een WebApp maken van Azure Marketplace | Microsoft Azure"
    description="Informatie over het maken van een nieuwe WordPress web-app van Azure Marketplace met behulp van de Azure-Portal."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Een WebApp maken van Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

De Azure Marketplace beschikbaar een breed scala van populaire web-apps die zijn ontwikkeld door Microsoft en derden bedrijven bron openen software initiatieven. Bijvoorbeeld: WordPress, Umbraco CMS, Drupal, enzovoort. Deze WebApps zijn gebaseerd op een breed scala van populaire kaders, zoals [PHP] in deze WordPress voorbeeld, [.NET] [Node.js], [Java]en [Python], om een paar te noemen. Is de browser die u voor de [Azure-Portal gebruikt]als u wilt maken van een web-app van Azure Marketplace de enige software die u nodig hebt.

In deze zelfstudie leert u hoe u:

* Zoek- en WebApp maken in Azure App Service die is gebaseerd op een sjabloon Azure Marketplace.
* Azure-App-Service-instellingen voor de nieuwe webtoepassing configureren.
* Starten en beheren van uw web-app.

Voor de toepassing van deze zelfstudie implementeert u een blogsite WordPress van Azure Marketplace. Wanneer u de stappen in deze zelfstudie hebt voltooid, kunt u uw eigen site WordPress omhoog en worden uitgevoerd in de cloud hebt.

![Voorbeelddashboard WordPress WEP-app][WordPressDashboard1]

De WordPress-site die u in deze zelfstudie implementeren gaat MySQL voor de database wordt gebruikt. Als u gebruiken in plaats daarvan SQL-Database voor de database wilt, raadpleegt u [Project Nami], die is ook beschikbaar via de Azure Marketplace.

> [AZURE.NOTE]
> Als u wilt deze zelfstudie hebt voltooid, moet u een Microsoft Azure-account. Als u geen account hebt, kunt u [de voordelen van uw Visual Studio-abonnee activeren] [ activate] of [zich registreren voor een gratis proefversie][free trial].
>
> Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert]. Vanaf hier kunt u direct een tijdelijk starter web-app maken in App Service, geen creditcard is vereist, en er zijn geen verplichtingen.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Zoeken en een WebApp maakt in Azure App-Service

1. Meld u aan bij de [Portal van Azure].

1. Klik op **Nieuw**.
    
    ![Een nieuwe Azure bron maken][MarketplaceStart]
    
1. Zoeken naar **WordPress**en klik vervolgens op **WordPress**. (Als u SQL-Database gebruiken in plaats van MySQL wilt, zoekt u **Project Nami**.)

    ![Zoeken naar WordPress op Marketplace][MarketplaceSearch]
    
1. Lees de beschrijving van de app WordPress en klikt u op **maken**.

    ![WordPress web-app maken][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Azure App-Service-instellingen voor uw nieuwe Web-App configureren

1. Nadat u een nieuwe WebApp hebt gemaakt, wordt het blad WordPress-instellingen weergegeven, waarin u wilt gebruiken voor de volgende stappen uit:

    ![WordPress web app-instellingen configureren][ConfigStart]

1. Voer een naam voor de web-app in het vak **WebApp** .

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web-app *{naam}*. azurewebsites.net. Als de naam die u invoert niet uniek, is een rood uitroepteken wordt weergegeven in het tekstvak.

    ![De naam van de WordPress web-app configureren][ConfigAppName]

1. Als u meer dan één abonnement hebt, kiest u de sectie die u wilt gebruiken. 

    ![Het abonnement voor de web-app configureren][ConfigSubscription]

1. Selecteer een **Resourcegroep** of maak een nieuwe record.

    Zie [overzicht van de Azure resourcemanager]voor meer informatie over resourcegroepen,[ResourceGroups].

    ![De resourcegroep voor de web-app configureren][ConfigResourceGroup]

1. Selecteer een **App Service abonnement/locatie** of maak een nieuwe record.

    Zie [overzicht van de Azure App-Service plannen]voor meer informatie over de App-Service-abonnementen,[AzureAppServicePlans]. 

    ![Het abonnement dat voor de web-app configureren][ConfigServicePlan]

1. Klik op **Database**en geef in het blad **Nieuwe MySQL-Database** de vereiste waarden voor het configureren van uw MySQL-database.

    een. Voer een nieuwe naam of de standaardnaam verlaten.

    b. Laat het **Type Database** die is ingesteld op **gedeeld**.

    c. Kies op dezelfde locatie als een door die u voor de web-app gekozen.

    d. Kies een prijzen laag. **Kwik** - die is gratis met minimale verbindingen en schijfruimte - is voor deze zelfstudie in orde.

    e. In het blad **Nieuwe MySQL-Database** , accepteer de juridische voorwaarden en klik vervolgens op **OK**. 

    ![De database configureren voor de web-app][ConfigDatabase]

1. Klik in het blad **WordPress** akkoord met de juridische voorwaarden en klik vervolgens op **maken**. 

    ![Stoppen met het web app-instellingen en klik op OK][ConfigFinished]

    Azure App-Service Hiermee maakt u de web-app, meestal in een handomdraai. U kunt de voortgang bekijken door te klikken op het belpictogram boven aan de pagina in de portal.

    ![Voortgangsindicator][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten en beheren van uw WordPress web-app
    
1. Als het maken van web app is voltooid, gaat u in de Portal Azure aan de resourcegroep waarin u de toepassing hebt gemaakt en ziet u de web-app en de database.

    De extra resource met het pictogram gloeilamp is [Toepassing inzichten][ApplicationInsights], waarmee controleren services voor uw web-app.

1. Klik op het web app-lijn in het blad **resourcegroep** .

    ![Selecteer uw WordPress WebApp][WordPressSelect]

1. Klik in het blad Web app, klikt u op **Bladeren**.

    ![Blader naar uw WordPress web-app][WordPressBrowse]

1. Als u wordt gevraagd of Selecteer de taal voor uw blog WordPress, selecteert u de gewenste taal en klik vervolgens op **Doorgaan**.

    ![De taal voor uw WordPress WebApp configureren][WordPressLanguage]

1. Voer de configuratiegegevens is vereist door WordPress WordPress **Welkom** op de pagina en klik op **WordPress installeren**.

    ![De instellingen van uw WordPress web-app configureren][WordPressConfigure]

1. Meld u aan met de referenties die u hebt gemaakt op **de welkomstpagina** .  

1. Uw site Dashboard-pagina wordt openen en weergeven van de gegevens die u hebt opgegeven.    

    ![Uw dashboard WordPress bekijken][WordPressDashboard2]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie Bovendien hebt u geleerd hoe maken en implementeren van een voorbeeld van de web-app van Azure Marketplace.

Zie de koppelingen voor meer informatie over het werken met App-Service Web Apps, aan de linkerkant van de pagina (voor windows breed browser) of boven aan de pagina (voor windows smalle browser).

Zie voor meer informatie over het ontwikkelen van WordPress WebApps op Azure [Ontwikkelen WordPress op Azure App Service][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Probeer de App-Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure-Portal]: https://portal.azure.com/
[Project-Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
