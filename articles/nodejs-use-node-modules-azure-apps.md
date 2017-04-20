<properties
    pageTitle="Werken met Node.js Modules"
    description="Informatie over het werken met Node.js modules wanneer u Azure App Service of Cloud Services gebruikt."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Gebruik van Node.js Modules met Azure-toepassingen

In dit document bevat richtlijnen over het gebruik van Node.js modules met toepassingen die worden gehost op Azure. Vindt richtlijnen voor ervoor te zorgen dat uw toepassing een specifieke versie van de module gebruikt, evenals systeemeigen modules gebruikt met Azure.

Als u al bekend met bent is gebruik Node.js modules, **package.json** en **npm-shrinkwrap.json** -bestanden, volgt een beknopt overzicht van wat wordt beschreven in dit artikel:

* Azure App Service begrijpt **package.json** en **npm-shrinkwrap.json** bestanden en modules op basis van items in deze bestanden kunt installeren.
* Azure Cloudservices verwachten alle modules op de ontwikkelomgeving zijn geïnstalleerd en de **knooppunt\_modules** directory wilt opnemen als onderdeel van het implementatiepakket. Het is mogelijk ondersteuning voor het installeren van modules met **package.json** of **npm-shrinkwrap.json** -bestanden op Cloudservices, maar hiervoor is vereist aanpassing van de standaardscripts die worden gebruikt door Cloudservice projecten inschakelen. Zie voor een voorbeeld van hoe u dit doen, [Azure opstarten taak om uit te voeren npm installeren om te voorkomen dat knooppunt modules implementeren](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure virtuele Machines worden niet besproken in dit artikel, zoals de implementatie-ervaring in een VM afhankelijk van het besturingssysteem die worden gehost door de virtuele Machine zijn zal.

##<a name="nodejs-modules"></a>Node.js Modules

Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. Een module is meestal geïnstalleerd met het hulpprogramma voor de **npm** , maar sommige (zoals de HTTP-module) zijn opgenomen als onderdeel van het core Node.js-pakket.

Wanneer modules zijn geïnstalleerd, die worden opgeslagen in de **knooppunt\_modules** directory in de hoofdmap van de mapstructuur voor uw toepassing. Elke module binnen de **knooppunt\_modules** directory houdt een eigen **knooppunt\_modules** map waarin alle modules die deze afhankelijk is, en deze opnieuw wordt herhaald voor elke module helemaal verder in de afhankelijkheidsketen. Hiermee kunt elke module zodat u een eigen versievereisten voor de modules dit hangt af van, maar dit in heel grote mapstructuur resulteren kan hebt geïnstalleerd.

Bij het distribueren van de **knooppunt\_modules** directory als onderdeel van uw toepassing, duurt de grootte van de implementatie vergeleken met behulp van een bestand **package.json** of **npm-shrinkwrap.json** ; echter, betekent dit dat de versie van de modules gebruikt in productie zijn hetzelfde als die worden gebruikt in de ontwikkelingsfase bevindt.

###<a name="native-modules"></a>Systeemeigen Modules

Hoewel de meeste modules gewoon tekst zonder opmaak JavaScript-bestanden zijn, zijn sommige modules platform / regiospecifieke binaire afbeeldingen. Deze modules zijn gecompileerd tijdens de installatie, meestal met behulp van Python en knooppunt-gyp. Aangezien Azure Cloud Services zijn afhankelijk van de **knooppunt\_modules** map die wordt geïmplementeerd als onderdeel van de toepassing, een systeemeigen module opgenomen als onderdeel van de geïnstalleerde modules moet werken in een cloudservice zo lang maken als deze is geïnstalleerd en gecompileerd op een Windows-systeem ontwikkeling.

Azure App-Service biedt geen ondersteuning voor alle systeemeigen modules en mislukt mogelijk bij het compileren van mensen met zeer specifieke vereisten. Terwijl enkele populaire modules zoals MongoDB optioneel systeemeigen afhankelijkheden en werken dan grote zonder ze hebt, bewezen twee tijdelijke oplossingen geslaagd bijna alle systeemeigen modules vandaag beschikbaar:

* Uitgevoerd **npm installeren** op een Windows-computer waarop alle systeemeigen van de module vereisten geïnstalleerd. Vervolgens implementeren het gemaakte **knooppunt\_modules** map als onderdeel van de toepassing Azure App-Service.
* Azure-App-Service kan worden geconfigureerd voor het uitvoeren van aangepaste we vaker doen of shell-scripts tijdens de implementatie, zodat u de mogelijkheid om aangepaste opdrachten uitvoeren en nauwkeuriger configureren de manier waarop **npm installeren** wordt uitgevoerd. Zie voor een video waarin wordt getoond hoe u dit wilt doen, [Aangepaste Website implementatie Scripts met Kudu].

###<a name="using-a-packagejson-file"></a>Met behulp van een bestand package.json

Het bestand **package.json** is een manier om op te geven van het hoogste niveau afhankelijkheden uw toepassing is vereist zodat de host-platform installeren kunt de afhankelijkheden, in plaats van dat u wilt opnemen hoeft de **knooppunt\_pakketten** map als onderdeel van de implementatie. Nadat de toepassing is geïmplementeerd, wordt de opdracht **npm installeren** wordt gebruikt voor het bestand **package.json** parseren en installeer alle afhankelijkheden vermeld.

Tijdens de ontwikkeling, kunt u de **--Opslaan**, **--Opslaan-ontwikkelaar**, of **--Opslaan-optionele** parameters tijdens de installatie van modules als u wilt een fragment voor de module automatisch toevoegen aan uw bestand **package.json** . Zie [npm installeren](https://docs.npmjs.com/cli/install)voor meer informatie.

Een mogelijke probleem met het bestand **package.json** is dat deze alleen de versie voor het hoogste niveau afhankelijkheden bepaalt. Elke module hebt geïnstalleerd mogelijk of de versie van de modules die deze afhankelijk is niet mogelijk opgeven en is het mogelijk dat u mogelijk dit uiteindelijk een reeks verschillende afhankelijkheid dan de gebruikt in de ontwikkelingsfase bevindt.

> [AZURE.NOTE]
> Bij de implementatie van Azure App-service, als het bestand <b>package.json</b> verwijst naar een systeemeigen module ziet u een vergelijkbaar is met de volgende fout bij het publiceren van de toepassing cijfer gebruikt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Met behulp van een bestand npm-shrinkwrap.json

Het bestand **npm-shrinkwrap.json** is een poging tot de module versiebeheer beperkingen van het bestand **package.json** op te heffen. Terwijl de **package.json** -bestand alleen versies voor het hoogste niveau modules bevat, bevat het bestand **npm-shrinkwrap.json** de versievereisten voor de volledige module afhankelijkheidsketen.

Wanneer uw toepassing gereed is voor productie, kunt u vergrendelen-versievereisten omlaag en een **npm-shrinkwrap.json** -bestand maken met behulp van de opdracht **npm verpakking** . Hiermee wordt de geïnstalleerde versies gebruiken de **knooppunt\_modules** map, en deze naar het bestand **npm-shrinkwrap.json** vastleggen. Nadat de toepassing is geïmplementeerd naar de hostomgeving, wordt de opdracht **npm installeren** wordt gebruikt voor het bestand **npm-shrinkwrap.json** parseren en installeer alle afhankelijkheden vermeld. Zie [npm-verpakking](https://docs.npmjs.com/cli/shrinkwrap)voor meer informatie.

> [AZURE.NOTE]
>Bij de implementatie van Azure App-service, als het bestand <b>npm-shrinkwrap.json</b> verwijst naar een systeemeigen module ziet u een vergelijkbaar is met de volgende fout bij het publiceren van de toepassing cijfer gebruikt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Volgende stappen

Nu u weet hoe u Node.js modules met Azure, leren hoe u [de versie Node.js opgeven], [bouwen en implementeren van een Node.js web-app], en [het gebruik van de Interface van de opdrachtregel Azure voor Mac en Linux].

Zie het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[Geef de Node.js-versie]: nodejs-specify-node-version-azure-apps.md
[Het gebruik van de opdrachtregel Azure voor Mac en Linux]: xplat-cli-install.md
[bouwen en implementeren van een Node.js web-app]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Aangepaste Website implementatie Scripts met Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
