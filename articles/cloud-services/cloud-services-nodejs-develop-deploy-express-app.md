<properties 
    pageTitle="Web App met Express (Node.js) | Microsoft Azure" 
    description="Een zelfstudie die is gebaseerd op de cloud service zelfstudie, en geeft aan hoe u de module Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Een webtoepassing voor Node.js Express gebruiken op een Cloudservice Azure maken

Node.js bevat een minimale functionaliteit in de runtime core.
Ontwikkelaars gebruiken vaak 3e partijen modules bieden extra functionaliteit bij het ontwikkelen van een toepassing voor de Node.js. In deze zelfstudie maakt u een nieuwe toepassing gebruik van de module [Express][] , dat een kader MVC biedt voor het maken van webtoepassingen Node.js.

Schermafbeelding van de voltooide toepassing is hieronder:

![Een webbrowser geeft Welkom weer aan Express in Azure wordt aangegeven](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Een Project van de Service Cloud maken

Voer de volgende stappen uit om een nieuwe cloud-service project met de naam 'expressapp' te maken:

1. Vanuit het **Menu Start** of het **Startscherm**, zoekt u **Windows PowerShell**. Ten slotte met de rechtermuisknop op de **Windows PowerShell** en selecteer **Als Administrator uitvoeren**.

    ![Azure PowerShell-pictogram](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Wijzigen van mappen naar de **c:\\knooppunt** directory en voer de volgende opdrachten om een nieuwe oplossing **expressapp** met de naam en een Webrol **WebRole1**met de naam te maken:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Standaard wordt **Toevoegen-AzureNodeWebRole** gebruikt een oudere versie van Node.js. Azure gebruik v0.10.21 van knooppunt Hiermee geeft u de bovenstaande **Set-AzureServiceProjectRole** -instructie.  Houd rekening met dat de parameters zijn hoofdlettergevoelig.  U kunt controleren of dat de juiste versie van Node.js door te schakelen van de eigenschap **engines** in **WebRole1\package.json**is geselecteerd.

##<a name="install-express"></a>Express installeren

1. Installeer de Express genereren door de volgende opdracht:

        PS C:\node\expressapp> npm install express-generator -g

    De uitvoer van de opdracht npm certificaat van Echtheid naar het volgende resultaat. 

    ![Windows PowerShell voor het weergeven van de uitvoer van de npm installeren express opdracht.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Mappen wijzigen in de map **WebRole1** en het gebruik van de express opdracht om een nieuwe toepassing:

        PS C:\node\expressapp\WebRole1> express

    U wordt gevraagd naar uw eerdere toepassing overschrijven. Voer **y** of **Ja** om door te gaan. Express genereert het bestand app.js en de mappenstructuur van een voor het samenstellen van uw toepassing.

    ![De uitvoer van de opdracht express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Als u wilt extra afhankelijkheden die zijn gedefinieerd in het bestand package.json hebt geïnstalleerd, voert u de volgende opdracht uit:

        PS C:\node\expressapp\WebRole1> npm install

    ![De uitvoer van de npm opdracht installeren](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Gebruik de volgende opdracht uit de **Prullenbak/www** -bestand kopiëren naar **server.js**. Dit is de cloudservice kan het ingangspunt vinden van deze toepassing.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Nadat u deze opdracht is voltooid, kunt u een bestand **server.js** in de adreslijst WebRole1 nodig hebt.

7.  Wijzigen van de **server.js** als u wilt verwijderen van een van de '.' tekens uit de volgende regel.

        var app = require('../app');

    Na deze wijziging, de regel moet worden weergegeven als volgt.

        var app = require('./app');

    Deze wijziging is vereist omdat we het bestand (voorheen **opslaglocatie/www**,) verplaatst naar dezelfde map als de app-bestand dat is vereist. Nadat u deze wijziging, sla het bestand **server.js** .

8.  Gebruik de volgende opdracht uit de toepassing wilt uitvoeren in de Azure emulator:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Een webpagina die Welkom Express bevat.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>De weergave wijzigen

Nu de weergave om het bericht 'Welkom op Express in Azure' weer te wijzigen.

1.  Voer de volgende opdracht uit het bestand index.jade te openen:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![De inhoud van het bestand index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade is de standaard weergave-engine gebruikt door Express-toepassingen. Zie voor meer informatie over de engine Jade weergave, [http://jade-lang.com][].

2.  De laatste regel van tekst wijzigen door toe te voegen **in Azure wordt aangegeven**.

    ![Het bestand index.jade, de laatste regel leest: p Welkom bij \#{title} in Azure wordt aangegeven](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Sla het bestand en sluit Kladblok af.

4.  Vernieuw de browser en u uw wijzigingen wilt zien.

    ![Een browservenster, bevat de pagina Welkom bij Express in Azure wordt aangegeven](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Na de toepassing testen, gebruikt u de cmdlet **Stop-AzureEmulator** de emulator stoppen.

##<a name="publishing-the-application-to-azure"></a>De toepassing Azure publiceren

Klik in het venster Azure PowerShell gebruikt u de cmdlet **Publiceren-AzureServiceProject** in de toepassing naar een cloudservice implementeren

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Zodra de implementatie-bewerking is voltooid, wordt uw browser openen en weergeven van de pagina met webonderdelen.

![Een webbrowser weergeven van de pagina Express. De URL wordt aangegeven die nu wordt gehost op Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Volgende stappen

Zie het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-lang.com]: http://jade-lang.com

 
