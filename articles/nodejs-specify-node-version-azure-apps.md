<properties
    pageTitle="Een versie Node.js opgeven"
    description="Informatie over het opgeven van de versie van Node.js die worden gebruikt door Azure-websites en Cloud Services"
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

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Een versie Node.js geven in een Azure-toepassing

Wanneer een toepassing Node.js host, is het raadzaam om ervoor te zorgen dat uw toepassing gebruikmaakt van een specifieke versie van Node.js. Er zijn verschillende manieren om dit te doen voor toepassingen die worden gehost op Azure.

##<a name="default-versions"></a>Standaardversies

De Node.js versies geleverd door Azure worden voortdurend bijgewerkt. Tenzij anders vermeld, de standaardversie die is opgegeven in de `WEBSITE_NODE_DEFAULT_VERSION` omgevingsvariabele wordt gebruikt. Als u wilt deze waarde overschrijven, volg de stappen in de volgende secties van dit artikel

> [AZURE.NOTE] Als u uw toepassing in een Cloudservice Azure (web- of werknemer rol) host en is de eerste keer dat u de toepassing hebt geïmplementeerd, probeert Azure dezelfde versie van Node.js gebruiken zoals u hebt geïnstalleerd op uw ontwikkelomgeving als deze overeenkomt met een van de standaardversies die beschikbaar zijn op Azure.

##<a name="versioning-with-packagejson"></a>Versiebeheer met package.json

U kunt de versie van Node.js moet worden gebruikt door het volgende toe te voegen aan uw bestand **package.json** opgeven:

    "engines":{"node":version}

*Versie* is waar het specifieke versienummer gebruiken. U kunt kunt complexere voorwaarden opgeven voor versie, zoals:

    "engines":{"node": "0.6.22 || 0.8.x"}

Aangezien 0.6.22 een van de versies beschikbaar in de hostomgeving is, gebruikt de laatste versie van de 0,8 reeks die beschikbaar zijn in plaats daarvan - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Versiebeheer Websites met App-instellingen
Als u de toepassing van een Website host, kunt u de omgevingsvariabele **WEBSITE_NODE_DEFAULT_VERSION** naar de gewenste versie instellen. 

##<a name="versioning-cloud-services-with-powershell"></a>Versiebeheer Cloudservices met PowerShell

Als u de toepassing in een Cloudservice host en de toepassing via Azure PowerShell implementeert, kunt u de standaardversie Node.js overschrijven met behulp van de PowerShell-cmdlet **Set-AzureServiceProjectRole** . Bijvoorbeeld:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Houd rekening met dat de parameters in de bovenstaande instructie zijn hoofdlettergevoelig.  U kunt controleren of dat de juiste versie van Node.js door te schakelen van de eigenschap **engines** in van de functie **package.json**is geselecteerd.

U kunt ook de **Get-AzureServiceProjectRoleRuntime** gebruiken om op te halen van een lijst met Node.js versies beschikbaar voor toepassingen die worden gehost als een Cloudservice.  Altijd controleren of de versie van uw project afhankelijk van is Node.js in deze lijst.

##<a name="using-a-custom-version-with-azure-websites"></a>Een aangepaste versie gebruiken met Azure-Websites

Terwijl Azure diverse standaardversies van Node.js biedt, wilt u mogelijk een versie gebruikt die is niet beschikbaar al dan niet standaard. Als uw toepassing wordt gehost als een Azure-Website, kunt u dit doen met behulp van het bestand **iisnode.yml** . De volgende stappen doorlopen van het proces van het gebruik van een aangepaste versie van Node.Js met een Azure-Website:

1. Een nieuwe map maken en maak een bestand **server.js** binnen de adreslijst. Het bestand **server.js** moet de volgende elementen bevatten:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Hierdoor wordt de Node.js-versie die wordt gebruikt wanneer u de website bladeren weergegeven.

2. Maak een nieuwe Website en noteer de naam van de site. Bijvoorbeeld: de volgende gebruikt de [Azure opdrachtregel hulpmiddelen voor] een nieuwe Azure-Website met de naam **MijnWebsite**maken, en vervolgens een cijfer opslagplaats voor de website inschakelen.

        azure site create mywebsite --git

3. Maak een nieuwe map met de naam **opslaglocatie** als onderliggend item van de map die de **server.js** -bestand.

4. Download de specifieke versie van **node.exe** (de versie van Windows) die u wilt gebruiken met uw toepassing. De volgende gebruikt bijvoorbeeld **omslaan** versie 0.8.1 downloaden:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Sla het bestand **node.exe** naar de map **bin** eerder hebt gemaakt.

5. Een bestand **iisnode.yml** maken in dezelfde map als het bestand **server.js** en voegt u de volgende inhoud toe aan het bestand **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Dit pad is waar het bestand **node.exe** binnen uw project bevinden zich wanneer u uw toepassing naar de Website van Azure hebt gepubliceerd.

6. Uw toepassing publiceren. Bijvoorbeeld nadat ik een nieuwe website eerder met de parameter--cijfer gemaakt, worden de volgende opdrachten de toepassingsbestanden toevoegen aan mijn lokale cijfer opslagplaats en push ze vervolgens naar de website-bibliotheek:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nadat de toepassing heeft gepubliceerd, opent u de website in een browser. U ziet een bericht ontvangt ' Hallo van Azure actieve knooppunt-versie: v0.8.1 ".

##<a name="next-steps"></a>Volgende stappen

Nu dat u weten hoe u kunt opgeven van de versie van Node.js gebruikt door de toepassing wordt uitgelegd hoe u [werken met modules], [bouwen en implementeren van een website Node.js], en [het gebruik van de Azure opdrachtregel hulpprogramma's voor Mac en Linux].

Zie het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[Het gebruik van de Azure opdrachtregel hulpprogramma's voor Mac en Linux]: xplat-cli-install.md
[Azure opdrachtregel hulpmiddelen]: xplat-cli-install.md
[werken met modules]: nodejs-use-node-modules-azure-apps.md
[bouwen en implementeren van een website Node.js]: web-sites-nodejs-develop-deploy-mac.md
