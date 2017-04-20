<properties 
    pageTitle="Node.js-toepassing via van Socket.io | Microsoft Azure" 
    description="Informatie over het gebruik van socket.io in een node.js-toepassing die worden gehost op Azure." 
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

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Een toepassing Node.js Chat met Socket.IO maken op een Azure Cloudservice

Socket.IO biedt realtime communicatie tussen tussen uw node.js-server en clients. Deze zelfstudie wordt u begeleid hostingprovider een socket. IO gebaseerd chat-toepassing op Azure. Zie voor meer informatie over Socket.IO, <http://socket.io/>.

Schermafbeelding van de voltooide toepassing is hieronder:

![Een browservenster weergeven van de service die worden gehost op Azure][completed-app]  

## <a name="prerequisites"></a>Vereisten voor

Zorg ervoor dat de volgende producten en versies zijn geïnstalleerd om te kunnen voltooien in het voorbeeld in dit artikel:

* [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) installeren
* [Node.js](https://nodejs.org/download/) installeren
* [Python versie 2.7.10](https://www.python.org/) installeren

## <a name="create-a-cloud-service-project"></a>Een Project van de Service Cloud maken

De volgende stappen uit de cloud service project maken dat de toepassing Socket.IO wordt gehost.

1. Vanuit het **Menu Start** of het **Startscherm**, zoekt u **Windows PowerShell**. Ten slotte met de rechtermuisknop op de **Windows PowerShell** en selecteer **Als Administrator uitvoeren**.

    ![Azure PowerShell-pictogram][powershell-menu]

2. Maak een map genaamd **c:\\knooppunt**. 
 
        PS C:\> md node

3. Wijzigen van mappen naar de **c:\\knooppunt** directory
 
        PS C:\> cd node

4. Voer de volgende opdrachten om een nieuwe oplossing **chatapp** met de naam en een werknemer rol **WorkerRole1**met de naam te maken:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Hier ziet u het volgende antwoord:

    ![De uitvoer van de nieuwe azureservice en toevoegen azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Download het Chat-voorbeeld

Voor dit project gebruiken we het voorbeeld chat uit de [Socket.IO GitHub opslagplaats]. Voer de volgende stappen uit om het voorbeeld downloaden toe te voegen aan het project dat u eerder hebt gemaakt.

1.  Maak een lokale kopie van de bibliotheek met behulp van de knop **klonen** . U kunt ook de knop **ZIP** downloaden van het project.

    ![Een browservenster weergeven https://github.com/LearnBoost/socket.io/tree/master/examples/chat, met het ZIP-download-pictogram is gemarkeerd][chat-example-view]

3.  De structuur van de map van de lokale opslagplaats navigeren totdat u bij komt de **voorbeelden\\chat** directory. Kopieer de inhoud van deze map naar de **C:\\knooppunt\\chatapp\\WorkerRole1** directory eerder hebt gemaakt.

    ![Verkenner met de inhoud van de voorbeelden\\chat directory opgehaald uit het archief][chat-contents]

    De gemarkeerde items in de bovenstaande schermafbeelding zijn de bestanden hebt gekopieerd uit de **voorbeelden\\chat** directory

4.  In de **C:\\knooppunt\\chatapp\\WorkerRole1** directory, verwijder het bestand **server.js** en wijzig vervolgens het bestand **app.js** in **server.js**. Hiermee wordt het standaard **server.js** bestand eerder hebt gemaakt door de cmdlet **Toevoegen-AzureNodeWorkerRole** verwijderd en vervangen door de toepassingsbestand van het voorbeeld chat.

### <a name="modify-serverjs-and-install-modules"></a>Server.js wijzigen en Modules installeren

Voordat u de toepassing in de Azure emulator testen, moet we enkele kleine wijzigingen doorvoeren. De volgende stappen naar het bestand server.js uitvoeren:

1.  Open het bestand **server.js** in Visual Studio of elke teksteditor.

2.  Zoek de sectie **Module afhankelijkheden** aan het begin van server.js en wijzig de regel met **sio = require('.. //.. lib//socket.IO')** naar **sio require('socket.io') =** zoals hieronder wordt weergegeven:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Om ervoor te zorgen voor dat de toepassing op de juiste poort luistert, opent u server.js in Kladblok of uw favoriete editor en wijzig de volgende regel kunt vervangen door **3000** **process.env.port** zoals hieronder wordt weergegeven:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Gebruik van de volgende stappen voor het installeren van de vereiste modules na de wijzigingen opslaan op **server.js**, en test vervolgens de toepassing in de Azure emulator:

1.  Mappen naar via **Azure PowerShell**, wijzigen de **C:\\knooppunt\\chatapp\\WorkerRole1** directory en gebruik de volgende opdracht voor het installeren van de modules vereist door deze toepassing:

        PS C:\node\chatapp\WorkerRole1> npm install

    Hiermee installeert u de weergegeven in het bestand package.json modules. Nadat de opdracht is voltooid, worden er ongeveer als volgt uit:

    ![De uitvoer van de npm opdracht installeren][The-output-of-the-npm-install-command]

4.  Aangezien in dit voorbeeld een deel van de bibliotheek Socket.IO GitHub oorspronkelijk en rechtstreeks wordt verwezen de bibliotheek Socket.IO door relatief pad, is niet Socket.IO verwezen in het bestand package.json, zodat we deze door de volgende opdracht moet installeren:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testen en implementeren

1.  De emulator starten door de volgende opdracht:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Open een browser en Ga naar **http://127.0.0.1**.

3.  Wanneer het browservenster wordt geopend, voert u een bijnaam en klik vervolgens op ENTER drukken.
    Hiermee worden alle u om berichten te posten als een specifieke bijnaam. Als u wilt testen functionaliteit voor meerdere gebruikers, open extra browservensters met dezelfde URL en voer van verschillende bijnamen.

    ![Twee browservensters chatberichten van Gebruiker1 en Gebruiker2 weergeven](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Na het testen van de toepassing, stopt u de emulator door de volgende opdracht:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Als u wilt de toepassing Azure implementeert, gebruikt u de cmdlet **Publiceren-AzureServiceProject** . Bijvoorbeeld:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Zorg ervoor dat u een unieke naam te gebruiken, anders kan het proces publiceren, mislukt. Nadat de installatie is voltooid, wordt de browser openen en navigeer naar de service gedistribueerde.
    > 
    > Als er een met de mededeling foutbericht dat de naam van de meegeleverde abonnement niet in het profiel geïmporteerde publiceren bestaat, moet u downloaden en de publicatie profiel voor uw abonnement voordat u de implementatie in Azure importeren. Zie het gedeelte **implementeren van de toepassing Azure** van [bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Een browservenster weergeven van de service die worden gehost op Azure][completed-app]

    > [AZURE.NOTE] Als er een met de mededeling foutbericht dat de naam van de meegeleverde abonnement niet in het profiel geïmporteerde publiceren bestaat, moet u downloaden en de publicatie profiel voor uw abonnement voordat u de implementatie in Azure importeren. Zie het gedeelte **implementeren van de toepassing Azure** van [bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

De toepassing nu wordt uitgevoerd op Azure en chatberichten tussen verschillende clients met Socket.IO kan doorsturen.

> [AZURE.NOTE] In dit voorbeeld is voor eenvoudig, beperkt tot chatten tussen gebruikers die zijn gekoppeld aan hetzelfde exemplaar. Dit betekent dat als de cloudservice Hiermee maakt u twee instanties van werknemer rol, gebruikers is alleen mogelijk om te chatten met anderen verbonden met dezelfde werknemer rol-sessie. Als u de toepassing om te werken met meerdere exemplaren van de rol wilt verkleinen, kunt u een technologie zoals Service Bus delen van de status van de store Socket.IO alle werkstroomexemplaren. Zie de Service Bus wachtrijen en onderwerpen gebruik voorbeelden in de [SDK van Azure voor Node.js GitHub opslagplaats](https://github.com/WindowsAzure/azure-sdk-for-node)voor voorbeelden.

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een eenvoudige chat-toepassing die worden gehost in een Cloudservice Azure maakt. Als u wilt weten hoe u deze toepassing in een Azure-Website hosten, Zie [maken van een toepassing Node.js Chat met Socket.IO op een website van Azure][chatwebsite].

Zie ook het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub opslagplaats]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
