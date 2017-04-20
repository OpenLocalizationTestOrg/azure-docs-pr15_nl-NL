<properties
    pageTitle="Hoe u fouten opsporen in een Node.js web-app in Azure App-Service"
    description="Leer hoe u fouten opsporen in een Node.js web-app in Azure App-Service."
    tags="azure-portal"
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

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Hoe u fouten opsporen in een Node.js web-app in Azure App-Service

Azure biedt ingebouwde diagnostische hulpprogramma's om u te helpen met foutopsporing Node.js toepassingen die worden gehost in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. In dit artikel leert u hoe u gegevens vastleggen van stdout en stderr, foutgegevens weergeven in de browser en hoe u downloadt en logboekbestanden weergeven.

Diagnostische hulpprogramma's voor Node.js toepassingen die worden gehost op Azure wordt geleverd door [IISNode]. Terwijl u in dit artikel wordt beschreven hoe de meest voorkomende instellingen voor het verzamelen van diagnostische informatie, biedt geen volledige informatie over voor het werken met IISNode. Zie voor meer informatie over het werken met IISNode de [IISNode Leesmij] op GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Logboekregistratie inschakelen

Standaard wordt een App Service web-app alleen diagnostische informatie over implementaties, zoals wanneer u een web-app met cijfer implementeert vastgelegd. Deze informatie is handig als u problemen ondervindt tijdens de implementatie, zoals een fout tijdens de installatie van een module waarnaar wordt verwezen in **package.json**, of als u een aangepaste implementatiescript gebruikt.

Als u wilt de logboekregistratie van stdout en stderr streams inschakelt, moet u een **IISNode.yml** -bestand maken in de hoofdmap van uw Node.js-toepassing en voer de volgende:

    loggingEnabled: true

Hiermee worden de registratie van stderr en stdout uit uw Node.js-toepassing.

Het bestand **IISNode.yml** kan ook worden gebruikt om te bepalen of beschrijvende fouten of ontwikkelaars fouten worden geretourneerd naar de browser wanneer er een fout optreedt. Voeg de volgende regel naar het bestand **IISNode.yml** zodat ontwikkelaars fouten:

    devErrorsEnabled: true

Als deze optie is ingeschakeld, retourneert IISNode de laatste 64 kB van gegevens die worden verzonden naar stderr in plaats van een beschrijvende fout, zoals 'Er is een interne server-fout opgetreden'.

> [AZURE.NOTE] DevErrorsEnabled is handig bij het oplossen van problemen tijdens de ontwikkeling, zodat deze in een productieomgeving kan dit leiden tot ontwikkeling fouten naar eindgebruikers is verzonden.

Als het bestand **IISNode.yml** niet al in uw toepassing bestaat, moet u uw web-app opnieuw starten na de bijgewerkte toepassing publiceren. Als u gewoon instellingen in een bestaand **IISNode.yml** -bestand die eerder zijn gepubliceerd wijzigt, is geen opnieuw starten vereist.

> [AZURE.NOTE] Als uw web-app is gemaakt met de hulpmiddelen voor Azure-opdrachtregel of Azure PowerShell-Cmdlets, wordt er automatisch een standaard **IISNode.yml** bestand gemaakt.

Als u wilt de WebApp opnieuw te starten, selecteert u de web-app in de [Portal van Azure](https://portal.azure.com)en klik vervolgens op de knop **Start opnieuw** :

![knop opnieuw][restart-button]

Als de Azure opdrachtregel hulpprogramma's in uw ontwikkelomgeving zijn geïnstalleerd, kunt u de volgende opdracht uit om de web-app opnieuw te starten:

    azure site restart [sitename]

> [AZURE.NOTE] Hoewel loggingEnabled en devErrorsEnabled de meestgebruikte IISNode.yml configuratieopties zijn voor het vastleggen van diagnostische gegevens, kan IISNode.yml worden gebruikt voor het configureren van een verscheidenheid aan opties voor uw hostingprovider-omgeving. Zie voor een volledige lijst met de configuratieopties, het [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) -bestand.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Toegang krijgen tot logboeken

Diagnostische logboeken kunnen worden geopend op drie manieren; Met de FTP File Transfer Protocol (), een Zip-archief, downloaden of bijgewerkt als een live stream van het logboek (ook wel bekend als een uiteinde). Downloaden van het Zip-archief van de logboekbestanden of bekijken van de live gegevensstroom moet de hulpmiddelen van de opdrachtregel Azure. Deze kunnen worden geïnstalleerd met behulp van de volgende opdracht uit:

    npm install azure-cli -g

Zodra geïnstalleerd, kunnen de hulpprogramma's worden geopend met de opdracht 'azure'. De opdrachtregel hulpmiddelen moeten eerst worden geconfigureerd als uw Azure abonnement wilt gebruiken. Zie voor informatie over het uitvoeren van deze taak de **het downloaden en importeren publiceren instellingen** sectie van het artikel [hoe u gebruik het Azure opdrachtregel hulpprogramma's](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Voor toegang tot de diagnostische gegevens via FTP, Ga naar de [Portal van Azure](https://portal.azure.com), selecteert u uw web-app en selecteer vervolgens het **DASHBOARD**. Klik in de sectie **Snelle koppelingen** bieden de koppelingen **FTP diagnostische LOGBOEKEN** en **TRANSFEREERT diagnostische LOGBOEKEN** toegang tot de logboeken met het FTP-protocol.

> [AZURE.NOTE] Als u niet eerder geconfigureerd gebruikersnaam en wachtwoord voor FTP of implementatie, kunt u dat doen vanaf de beheerpagina **QuickStart** door in te schakelen **implementatie referenties instellen**.

De FTP-URL als resultaat gegeven in het dashboard is voor de map **logboekbestanden** , die de volgende submappen bevat:

* [Methode voor implementatie](web-sites-deploy.md) - als u een implementatiemethode zoals cijfer gebruikt, een map met dezelfde naam wordt gemaakt en bevat informatie met betrekking tot implementaties.

* nodejs - Stdout en stderr informatie vastgelegd uit alle exemplaren van de toepassing (wanneer loggingEnabled waar is.)

###<a name="zip-archive"></a>ZIP-archief

Als u wilt downloaden van een Zip-archief van de diagnostische logboeken, gebruikt u de volgende opdracht uit de Azure voor hulpmiddelen voor:

    azure site log download [sitename]

Hiermee wordt een **diagnostics.zip** in de huidige map downloaden. In dit archief bevat de volgende mapstructuur:

* implementaties - een logboek met informatie over implementaties van uw toepassing

* Logboekbestanden

    * [Methode voor implementatie](web-sites-deploy.md) - als u een implementatiemethode zoals cijfer gebruikt, een map met dezelfde naam wordt gemaakt en bevat informatie met betrekking tot implementaties.

    * nodejs - Stdout en stderr informatie vastgelegd uit alle exemplaren van de toepassing (wanneer loggingEnabled waar is.)

###<a name="live-stream-tail"></a>Live stream (waarde)

Gebruik de volgende opdracht vanaf de opdrachtregel-hulpmiddelen van Azure om weer te geven een live gegevensstroom van diagnostische logboekgegevens:

    azure site log tail [sitename]

Hiermee herstelt u een reeks gebeurtenissen die zijn bijgewerkt terwijl ze plaats op de server. Deze stroom retourneert informatie over de implementatie, evenals stdout en stderr informatie (wanneer loggingEnabled waar is.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u inschakelt en gegevens van de diagnostische hulpprogramma's voor Azure weer te geven. Deze informatie is handig in lidmaatschap problemen die zich bij uw toepassing voordoen, kan deze verwijst naar een probleem met een module die u gebruikt of dat de versie van Node.js die worden gebruikt door de Service-WebApps App verschilt van het account waarmee uw implementatie-omgeving.

Zie voor informatie in werken met modules op Azure [Node.js Modules gebruiken met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md).

Zie voor informatie over het opgeven van een versie Node.js voor uw toepassing [precisie van een versie Node.js in een Azure-toepassing].

Zie ook het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

[IISNode]: https://github.com/tjanczuk/iisnode
[Leesmij voor IISNode]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Een versie Node.js geven in een Azure-toepassing]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
