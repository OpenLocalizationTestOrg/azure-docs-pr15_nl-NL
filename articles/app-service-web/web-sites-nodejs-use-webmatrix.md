<properties 
    pageTitle="Bouw en implementeer een Node.js web-app in Azure WebMatrix gebruiken" 
    description="Een zelfstudie waarin u u hoe leert u met WebMatrix een toepassing Node.js ontwikkelen en het dashboard implementeren naar Azure App Service Web Apps." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Bouw en implementeer een Node.js web-app in Azure WebMatrix gebruiken

Deze zelfstudie wordt getoond hoe u met WebMatrix een toepassing Node.js ontwikkelen en het dashboard implementeren naar [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix is een hulpprogramma voor het ontwikkelen van gratis web van Microsoft die alles wat die u nodig hebt voor het ontwikkelen van website of web-apps. WebMatrix bevat diverse functies waardoor deze eenvoudig te gebruiken Node.js inclusief codevoltooiing, vooraf gedefinieerde sjablonen en editor ondersteuning voor Jade, minder en CoffeeScript. Meer informatie over [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Na het voltooien van deze handleiding, hebt u een Node.js web-app met App-Azure-Service.
 
Schermafbeelding van de voltooide toepassing is hieronder:

![Azure knooppunt-website][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="sign-into-azure"></a>Meld u aan bij Azure

Volg deze stappen om een WebApp maakt in Azure App-Service.

1. WebMatrix starten
2. Als dit de eerste keer is dat hebt u gebruikt WebMatrix, wordt u gevraagd u zich aanmeldt bij Azure.  Anders kunt u klikken op de knop **Aanmelden** en kiest u **Account toevoegen**.  Selecteer aan te **melden** met uw Microsoft-Account.

    ![Account toevoegen][addaccount]

3. Als u zich hebt geregistreerd voor een Azure-account, kunt u zich aanmelden met uw Microsoft-Account:

    ![Meld u aan bij Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Een site met behulp van een ingebouwde sjabloon voor Azure maken

1. Klik op het startscherm en klik op de knop **Nieuw** en kies **Galerie met sjablonen** een nieuwe site maken uit de galerie met sjablonen:

    ![Nieuwe site uit galerie met sjablonen][sitefromtemplate]

2. Klik in het dialoogvenster **Site van sjabloon** Selecteer **knooppunt** en selecteer **Express-Site**. Klik tot slot op **volgende**. Als u alle vereiste software voor de **Express** sitesjabloon ontbreekt, wordt u gevraagd deze worden geïnstalleerd.

    ![Selecteer express sjabloon][webmatrix-templates]

3. Als u bent aangemeld bij Azure, hebt u nu de optie voor het maken van een App Service-web-app voor uw lokale site.  Kies een unieke naam en selecteer van het datacenter waar u uw App Service-web-app wilt moet worden gemaakt: 

    ![Site maken op Azure][nodesitefromtemplateazure]
    
4. Nadat WebMatrix is voltooid voor het samenstellen van de lokale site en het maken van de App Service WebApp, worden de IDE WebMatrix wordt weergegeven.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>De toepassing Azure publiceren

1. In WebMatrix, klik op **publiceren** van het lint **Start** naar het dialoogvenster **Publiceren Preview** voor de site weergeven.

    ![voorbeeld publiceren][webmatrix-node-publishpreview]

2. Klik op **Doorgaan**. Wanneer de publicatie is voltooid, wordt de URL voor de App Service web-app weergegeven onder aan de WebMatrix IDE

    ![publiceren voltooid][webmatrix-publish-complete]

3. Klik op de koppeling om te openen van de App Service web-app in uw browser.

    ![Express WebApp][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Uw toepassing publiceren en wijzigen

U kunt gemakkelijk wijzigen en uw toepassing opnieuw publiceren. Hier, brengt u een eenvoudige wijzigen in de kop in het bestand **index.jade** en publiceer de toepassing opnieuw.

1. Selecteer **bestanden**in WebMatrix, en vouwt u de map **weergaven** . Open het bestand **index.jade** door erop te dubbelklikken.

    ![webmatrix bekijken index.jade][webmatrix-modify-index]

2. Wijzig de alinea-regel als volgt uit:

        p Welcome to #{title} with WebMatrix on Azure!

3. Uw wijzigingen op te slaan en klik vervolgens op het pictogram publiceren. Ten slotte, klikt u op **Doorgaan** in het dialoogvenster **Publiceren Preview** en wacht totdat de update worden gepubliceerd.

    ![voorbeeld publiceren][webmatrix-republish]

4. Wanneer de publicatie is voltooid, gebruikt u de koppeling geretourneerd wanneer de publiceren is voltooid om de bijgewerkte App Service web-app weer te geven.

    ![Azure knooppunt WebApp][webmatrix-node-completed]

##<a name="next-steps"></a>Volgende stappen

Zie meer informatie over de versies van Node.js die worden geleverd met Azure en het opgeven van de versie moet worden gebruikt met uw toepassing, [precisie van een versie Node.js in een Azure-toepassing](../nodejs-specify-node-version-azure-apps.md).

Als u problemen met uw toepassing ondervindt nadat deze is geïmplementeerd in Azure, raadpleegt u [hoe u fouten opsporen in een Node.js web-app in Azure App-Service](web-sites-nodejs-debug.md) voor meer informatie over het oplossen van het probleem.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 