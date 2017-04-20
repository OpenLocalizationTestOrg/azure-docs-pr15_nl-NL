<properties 
    pageTitle="Het gebruik van io.js met Azure App Service Web Apps" 
    description="Leer hoe u een web-app gebruiken in Azure App-Service met io.js." 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Het gebruik van io.js met Azure App Service Web Apps

De populaire knooppunt zich splitsen [io.js] functies van verschillende verschillen aan de Joyent Node.js project, inclusief een meer geopende beheermodel, een snellere release-cyclus en een snellere aanvaarding van nieuwe en experiment JavaScript-functies.

Hoewel [Azure-Service voor App](http://go.microsoft.com/fwlink/?LinkId=529714) -WebApps vele Node.js versies vooraf geïnstalleerd heeft, kunt deze ook voor een gebruiker opgegeven Node.js-binair bestand. In dit artikel bevat twee methoden voor het inschakelen van het gebruik van io.js op App-Service Web Apps: het gebruik van een uitgebreide implementatiescript, Azure als u wilt gebruiken voor de meest recente versie van de beschikbare io.js, alsmede het handmatige uploaden van een binaire io.js automatisch te configureren. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Een implementatiescript gebruiken

Na implementatie van een Node.js-app, wordt een aantal kleine opdrachten om ervoor te zorgen dat de omgeving is geconfigureerd met App Service Web Apps uitgevoerd. Een implementatiescript gebruikt, kan dit proces worden aangepast om op te nemen het downloaden en configuratie van io.js.

De [io.js implementatiescript](https://github.com/felixrieseberg/iojs-azure) is beschikbaar op GitHub. Schakel io.js op uw web-app door gewoon **.deployment**, **deploy.cmd** en **IISNode.yml** kopiëren naar de hoofdmap van de toepassingsmap en implementeren naar Web Apps.  

Het eerste bestand, **.deployment**, Hiermee geeft u Web Apps om uit te voeren **deploy.cmd** na implementatie. Dit script wordt uitgevoerd de gebruikelijke stappen voor een Node.js-toepassing, maar ook de nieuwste versie van io.js is gedownload. Tot slot configureert **IISNode.yml** Web Apps als u wilt alleen de gedownloade io.js binaire gebruiken in plaats van een vooraf geïnstalleerde Node.js binair getal.

> [AZURE.NOTE] Als u wilt bijwerken van de gebruikte io.js binair getal, alleen uw toepassing - opnieuw te distribueren wordt het script gedownload een nieuwe versie van io.js elke één keer die de toepassing wordt geïmplementeerd.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Handmatige installatie gebruiken

De handmatige installatie van een aangepaste io.js-versie bevat slechts twee stappen. Download eerst het **win-x64** binaire rechtstreeks vanuit de [io.js verdeling]. Vereist zijn in twee bestanden - **iojs.exe** en **iojs.lib**. Beide bestanden opslaan op een map in uw web-app, bijvoorbeeld in **opslaglocatie/iojs**.

Maak een **IISNode.yml** -bestand in de hoofdmap van uw toepassing wilt configureren Web Apps als u wilt **iojs.exe** gebruiken in plaats van een vooraf geïnstalleerde versie van knooppunt, en voeg de volgende regel.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Volgende stappen

In dit artikel die u geleerd hoe u aangeboden io.js met App-Service Web Apps, met beide implementatie scripts ook als handmatige installatie. 

> [AZURE.NOTE] IO.js is in de dikke ontwikkelingsfase bevindt en vaker dan Node.js bijgewerkt. Een aantal Node.js modules werken mogelijk niet met io.js - neemt Raadpleeg [io.js op GitHub] voor probleemoplossing.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

[IO.js]: https://iojs.org
[IO.js verdeling]: https://iojs.org/dist/
[IO.js op GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 