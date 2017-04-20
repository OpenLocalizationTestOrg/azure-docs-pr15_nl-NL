<properties
    pageTitle="Handleiding node.js aan de slag | Microsoft Azure"
    description="Leer hoe u een eenvoudige Node.js-webtoepassing maken en het dashboard implementeren naar een Azure-cloudservice."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Deze zelfstudie wordt getoond hoe u een eenvoudige Node.js-toepassing uitgevoerd in een Cloudservice Azure maakt. Cloudservices zijn de bouwstenen van scalable cloud-toepassingen in Azure wordt aangegeven. Hiermee kunnen de scheiding en onafhankelijke management en schaal van de front-end en back-end gedeelte van de toepassing.  Cloudservices bieden een robuuste speciale virtuele machine voor elke rol betrouwbaar hostingprovider.

Zie voor meer informatie over Cloud Services en hoe ze in vergelijking met Azure Websites en virtuele machines, [Azure Websites, Cloudservices en virtuele Machines vergelijking].

>[AZURE.TIP] Bekijken om te maken van een eenvoudige website? Als uw scenario betrekking alleen een eenvoudige website front heeft, kunt u overwegen [een lightweight web-app]. U kunt eenvoudig upgraden naar een Cloudservice, zoals uw web-app in omvang groeit, en uw vereisten voor wijzigen.

Door deze zelfstudie te volgen, wordt u een eenvoudige webtoepassing gehost binnen een Webrol maken. U wordt de emulator berekeningscluster gebruiken om uw toepassing lokaal te testen en vervolgens deze via PowerShell opdrachtregel extra implementeren.

De toepassing is een eenvoudige 'Hallo wereld':

![Een webbrowser weergeven van de webpagina Hallo allemaal][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Vereisten voor

> [AZURE.NOTE] Deze zelfstudie wordt Azure PowerShell, waarvoor u Windows gebruikt.

- Installeren en configureren van [Azure Powershell].
- Download en installeer de [Azure SDK voor .NET 2.7]. Selecteer in de configuratie installeren:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Een project Azure-Cloudservice maken

De volgende taken als u wilt maken van een nieuw project Azure-Cloudservice, samen met eenvoudige Node.js steigers uitvoeren:

1. **Windows PowerShell** uitvoeren als beheerder. vanuit het **Menu Start** of het **Startscherm**, zoekt u **Windows PowerShell**.

2. [PowerShell koppelen] aan uw abonnement.

3. Voer in het volgende PowerShell-cmdlet maken om het project te maken:

        New-AzureServiceProject helloworld

    ![Het resultaat van de opdracht Nieuw AzureService-Hallo wereld][The result of the New-AzureService helloworld command]

    De cmdlet **New-AzureServiceProject** genereert een eenvoudige structuur voor het publiceren van een toepassing Node.js naar een Cloudservice. Deze bevat configuratiebestanden die nodig zijn voor publicatie voor Azure. Uw werkmap de cmdlet ook gewijzigd naar de map voor de service.

    De cmdlet wordt de volgende bestanden gemaakt:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** en **ServiceDefinition.csdef**: Azure taalspecifieke bestanden nodig zijn voor uw toepassing publiceren. Zie [overzicht van het maken van een Service die worden gehost voor Azure]voor meer informatie.

    -   **deploymentSettings.json**: lokale instellingen die worden gebruikt door de Azure PowerShell-cmdlets voor implementatie zijn opgeslagen.

4.  Voer de volgende opdracht uit een nieuwe Webrol toe te voegen:

        Add-AzureNodeWebRole

    ![De uitvoer van de opdracht toevoegen-AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    De cmdlet **Toevoegen-AzureNodeWebRole** Hiermee maakt u een eenvoudige Node.js-toepassing. De bestanden **.csfg** en **.csdef** om toe te voegen configuratie items voor de nieuwe rol ook gewijzigd.

    > [AZURE.NOTE] Als u de naam van een rol niet opgeeft, wordt een standaardnaam gebruikt. U kunt ook een naam opgeven als eerste parameter cmdlet:`Add-AzureNodeWebRole MyRole`

De app Node.js is gedefinieerd in het bestand **server.js**, zich in de map voor de Webrol (**WebRole1** al dan niet standaard). Hier ziet u de code:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Deze code is in principe hetzelfde als de steekproef 'Hallo wereld' op de website van [nodejs.org] , behalve de site gebruikmaakt van het poortnummer in door de cloud-omgeving toegewezen.

## <a name="deploy-the-application-to-azure"></a>De toepassing Azure implementeren

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Downloaden van de Azure publicatie-instellingen

Als u wilt uw Azure-toepassing implementeren, moet u eerst de publicatie-instellingen voor uw abonnement Azure downloaden.

1.  De volgende Azure PowerShell-cmdlet uitvoeren:

        Get-AzurePublishSettingsFile

    Hiermee wordt uw browser gebruiken om te navigeren naar de downloadpagina voor publiceren-instellingen. Mogelijk wordt u gevraagd aan te melden met een Microsoft-Account. Als dat het geval is, gebruikt u het account dat is gekoppeld aan uw Azure-abonnement.

    Sla het gedownloade profiel op een locatie die u eenvoudig kunt openen.

2.  Na de publicatie profiel dat u hebt gedownload importeren-cmdlet uitvoeren:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Na het importeren van de publicatie-instellingen, kunt u het verwijderen van gedownloade .publishSettings bestand omdat deze informatie waardoor iemand toegang tot uw account bevat.

### <a name="publish-the-application"></a>Publiceer de toepassing

Als u wilt publiceren, voert u de volgende opdrachten:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-Servicenaam** bevat de naam voor de implementatie. Dit moet een unieke naam, anders kan het proces publiceren, mislukt. De opdracht **Get-datum** kopspijkertjes van een datum/tijd-tekenreeks die de naam uniek moet maken.

- **-Locatie** Hiermee geeft u het datacenter die de toepassing wordt beheerd in. Als u wilt zien van een lijst met beschikbare datacenters, gebruikt u de cmdlet **Get-AzureLocation** .

- **-Starten** wordt een browservenster geopend en u gaat naar de gehoste service nadat implementatie is voltooid.

Nadat de publicatie is geslaagd, ziet u een reactie ongeveer als volgt uit:

![De uitvoer van de opdracht Publiceren-AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Het kan enkele minuten duren voor de toepassing kunt implementeren en beschikbaar zijn wanneer eerst gepubliceerd.

Wanneer de implementatie is voltooid, wordt een browservenster open en navigeer naar de cloudservice.

![Een browservenster weergeven van de Hallo wereld-pagina. de URL wordt aangegeven op dat de pagina wordt gehost op Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

De toepassing wordt nu uitgevoerd op Azure.

De cmdlet **Publiceren AzureServiceProject** voert de volgende stappen uit:

1.  Hiermee maakt u een pakket om te implementeren. Het pakket bevat alle bestanden in de toepassingsmap.

2.  Hiermee maakt u een nieuw **account opslag** als dit nog niet bestaat. De opslag van Azure-account wordt gebruikt voor de opslag van de toepassingspakket tijdens de implementatie. U kunt het account opslag veilig verwijderen nadat implementatie is voltooid.

3.  Als er nog niet bestaat, maakt een nieuwe **cloudservice** . Een **cloudservice** is de container waarin uw toepassing wordt gehost wanneer deze wordt geïmplementeerd op Azure. Zie [overzicht van het maken van een Service die worden gehost voor Azure]voor meer informatie.

4.  Het implementatiepakket worden gepubliceerd naar Azure.


## <a name="stopping-and-deleting-your-application"></a>Stoppen en verwijderen van uw toepassing.

Nadat u hebt uw toepassing wordt geïmplementeerd, wilt u mogelijk uitgeschakeld zodat u kunt extra kosten voorkomen. Azure facturen web exemplaren van de rol per uur van de servertijd verbruikt. Servertijd verbruikt zodra uw toepassing wordt geïmplementeerd, zelfs als de exemplaren die niet worden uitgevoerd en gestopt worden.

1.  Klik in het venster Windows PowerShell stop de service-implementatie gemaakt in de vorige sectie met de volgende cmdlet:

        Stop-AzureService

    De service wordt gestopt, kan enkele minuten duren. Wanneer de service is gestopt, ontvangt u een bericht aangegeven dat dit is gestopt.

    ![De status van de opdracht stoppen-AzureService][The status of the Stop-AzureService command]

2.  Als u wilt de service verwijderen, belt u de volgende cmdlet:

        Remove-AzureService

    Wanneer hierom wordt gevraagd, typt u **Y** als u wilt verwijderen van de service.

    Verwijderen van de service kan enkele minuten duren. Nadat de service is verwijderd, ontvangt u een bericht aangegeven dat de service is verwijderd.

    ![De status van de opdracht verwijderen AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Als de service verwijdert, niet de opslag-account dat is gemaakt tijdens de service is in eerste instantie gepubliceerd en u blijft worden gefactureerd voor opslag gebruikt. Als er niets anders de opslag gebruikt wordt, wilt u mogelijk om deze te verwijderen.

## <a name="next-steps"></a>Volgende stappen

Zie het [Node.js Developer Center]voor meer informatie.

<!-- URL List -->

[Azure Websites, Cloudservices en virtuele Machines-vergelijking]: ../app-service-web/choose-web-site-cloud-service-vm.md
[een licht web-app gebruiken]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK voor .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Verbinding maken met PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Een gehoste Service voor Azure maken-overzicht]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
