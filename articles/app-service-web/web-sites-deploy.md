<properties
    pageTitle="Implementeer de app Azure App-service"
    description="Leer hoe u uw app implementeren naar Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Implementeer de app Azure App-service

In dit artikel helpt u bij het bepalen van de beste optie voor het implementeren van de bestanden die u voor uw WebApp, mobiele app backend of API-app naar [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714)en begeleidt u naar de juiste resources met instructies die specifiek zijn voor een andere optie.

## <a name="overview"></a>Overzicht van de implementatie van Azure App Service

Azure App Service onderhoudt application framework voor u (ASP.NET, PHP, Node.js, enzovoort). Sommige kaders al dan niet standaard terwijl de anderen, zoals Java en Python zijn ingeschakeld, mogelijk moet u een eenvoudige vinkje configuratie te schakelen. Bovendien kunt u uw framework toepassing, zoals de PHP-versie of de bitness van uw runtime aanpassen. Zie [uw app in Azure App-Service configureren](web-sites-configure.md)voor meer informatie.

Aangezien er geen zorgen over de web-server of toepassing framework te maken, implementeren van uw app met App-Service is een kwestie van het implementeren van uw code, binaire bestanden, bestanden met inhoud en hun bijbehorende mapstructuur voor de [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure wordt aangegeven (of de **/site/wwwroot/App_Data/taken/** directory voor WebJobs). App-Service ondersteunt de volgende implementatieopties: 

- [FTP- of FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): gebruik uw favoriete FTP of TRANSFEREERT venster voor het verplaatsen van bestanden naar Azure van [FileZilla](https://filezilla-project.org) naar volledige IDEs zoals [NetBeans](https://netbeans.org)ingeschakeld. Dit is een bestand uploadproces. Geen extra services worden verstrekt door het App-Service, zoals versiebeheer, structuur bestandsbeheer, enzovoort. 

- [Kudu (cijfer/volgt of OneDrive/Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Gebruik de [implementatie-engine](https://github.com/projectkudu/kudu/wiki) in App Service. Push uw code naar Kudu rechtstreeks vanuit een bibliotheek. Kudu bovendien toegevoegde services wanneer code wordt gedrukt, zoals versiebeheer, pakket herstellen, MSBuild en [web haken](https://github.com/projectkudu/kudu/wiki/Web-hooks) voor doorlopende implementatie en andere automatiseringstaken. De implementatie-engine Kudu ondersteunt 3 verschillende soorten implementatie bronnen:   
    * Inhoud synchroniseren op basis van OneDrive en Dropbox   
    * Op basis van een bibliotheek continue-implementatie waarbij automatische synchronisatie van GitHub, Bitbucket en Visual Studio Team Services  
    * Op basis van een bibliotheek-implementatie waarbij handmatige synchronisatie van lokale cijfer  

- [Web implementeren](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Deploy code wilt App Service rechtstreeks vanuit uw favoriete Microsoft tools zoals Visual Studio met behulp van dezelfde gereedschap die automatiseert implementatie naar IIS-servers. Dit hulpprogramma ondersteunt alleen het vergelijken van implementatie, maken van de database, transformaties van verbindingstekenreeksen met de, enzovoort. Web Deploy verschilt van Kudu in dat toepassing binaire bestanden worden gemaakt voordat ze worden geïmplementeerd op Azure. Net zoals bij FTP, geen aanvullende services worden verstrekt door de App-Service.

Ontwikkeling van populaire Webwerkset ondersteuning voor een of meer van deze implementatieprocessen. Terwijl de functie die u kiest de implementatieprocessen die u gebruikmaken van bepaalt kunt, is de functionaliteit van de werkelijke DevOps beschikt afhankelijk van de combinatie van het implementatieproces en de specifieke hulpmiddelen die u kiest. Bijvoorbeeld als u Web implementeren in [Visual Studio met Azure SDK](#vspros), uitvoert Hoewel u geen automatisering van Kudu krijgt, krijg u pakket herstellen en MSBuild automatisering in Visual Studio. 

>[AZURE.NOTE] Deze implementatieprocessen niet daadwerkelijk [inrichten van de Azure bronnen](../resource-group-template-deploy-portal.md) die u uw app moet mogelijk. Echter grootste deel van de gekoppelde artikelen uitgelegd hoe u voor het inrichten van de app en het implementeren van uw code naar deze end-to-end. U kunt ook extra opties voor het inrichten van Azure resources in de sectie [implementatie automatiseren met behulp van de opdrachtregel hulpprogramma's](#automate) vinden.
     
## <a name="ftp"></a>Bestanden handmatig kopiëren naar Azure via FTP implementeert
Als u vaak uw webinhoud handmatig kopiëren tot een webserver gebruikt, kunt u een [FTP-](http://en.wikipedia.org/wiki/File_Transfer_Protocol) programma gebruiken om bestanden, zoals Windows Verkenner of [FileZilla](https://filezilla-project.org/)te kopiëren.

De professionals bestanden handmatig gekopieerd zijn:

- Bekend zijn en de minimale complexiteit voor FTP tooling. 
- U weet precies waar uw bestanden kunnen worden verzonden.
- Extra beveiliging met TRANSFEREERT.

De nadelen van het handmatig kopiëren van bestanden zijn:

- Dat u moet weten hoe u bestanden distribueren naar de juiste mappen in de App-Service. 
- Geen versiebeheer voor ongedaan maken als er fouten optreden.
- Geen ingebouwde implementatie geschiedenis voor het oplossen van problemen met de implementatie.
- Mogelijke lange implementatie tijden omdat veel FTP-hulpprogramma's niet alleen het vergelijken van kopiëren bieden en kopieer in alle bestanden.  

### <a name="howtoftp"></a>Hoe implementeert bestanden handmatig kopiëren naar Azure
Bestanden kopiëren naar Azure, moet u een paar eenvoudige stappen:

1. De verbindingsgegevens FTP ervan uitgaande dat u al tot stand gebracht implementatie-referenties, verkrijgen door te gaan naar **Instellingen** > **Eigenschappen**en vervolgens de waarden voor de **Gebruiker FTP-implementatie**, **FTP-Host Name**en **TRANSFEREERT Host Name**te kopiëren. Kopieer de waarde van de gebruiker **FTP-implementatie gebruiker** zoals weergegeven met de naam van de app inclusief kan alleen worden aangeboden juiste context voor de FTP-server Azure-Portal.
2. Gebruik de verbindingsgegevens die u als u wilt verbinden met uw app verzameld in uw FTP-client.
3. Uw bestanden en hun bijbehorende mapstructuur kopiëren naar de [map **/site/wwwroot** ](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure wordt aangegeven (of de **/site/wwwroot/App_Data/taken/** directory voor WebJobs).
4. Blader naar de URL van uw app om te controleren of dat de app goed werkt. 

Zie de volgende informatiebron voor meer informatie:

* [Een PHP-MySQL-web-app maken en implementeren met FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Implementeren door het synchroniseren met een cloud-map
Een goed alternatief voor het [handmatig kopiëren van bestanden](#ftp) wordt gesynchroniseerd bestanden en mappen naar App Service uit een cloudopslagservice zoals OneDrive en Dropbox. Het synchroniseren met een cloud-map maakt gebruik van het proces Kudu voor implementatie (Zie [overzicht van implementatieprocessen](#overview)).

De experts van het synchroniseren met een cloud-map zijn:

- Eenvoudig implementatie. Services, zoals OneDrive en Dropbox bieden bureaublad synchronisatie-clients, zodat uw lokale werkmap ook het telefoonboek van uw implementatie is.
- Één muisklik-implementatie.
- Alle functionaliteit in de Kudu implementatie-engine is niet beschikbaar (bijvoorbeeld pakket herstellen, automatisering).

De nadelen van het synchroniseren met een cloud-map zijn:

- Geen versiebeheer voor ongedaan maken als er fouten optreden.
- Geen geautomatiseerde implementatie, handmatige synchronisatie is vereist.

### <a name="howtodropbox"></a>Hoe implementeert synchroniseren met een cloud-map
Klik in de [Portal van Azure](https://portal.azure.com)kunt u een map voor inhoud synchroniseren in uw OneDrive of Dropbox cloudopslag, werken met uw app-code en de inhoud in die map en synchroniseren met de App Service met een muisklik.

* [Inhoud van de synchronisatie van een map cloud naar Azure App-Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Continu implementeren van een service voor het beheer van cloudgebaseerde bron
Als het ontwikkelteam gebruikmaakt van een bron cloudgebaseerde code management (SCM)-service zoals [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)of [BitBucket](https://bitbucket.org/), kunt u de App-Service te integreren met uw bibliotheek implementeren continu kunt configureren. 

Professionals implementatie van een service voor het beheer van cloudgebaseerde bron van zijn:

- Versiebeheer inschakelen terugdraaien.
- Mogelijkheid voor het configureren van continue implementatie voor cijfer (en volgt indien van toepassing) opslagplaatsen. 
- Tak / regiospecifieke implementatie kunt verschillende vertakkingen dashboard implementeren naar verschillende [sleuven](web-sites-staged-publishing.md).
- Alle functionaliteit in de Kudu implementatie-engine is niet beschikbaar (bijvoorbeeld implementatie versiebeheer, terugdraaien, pakket herstellen, automatisering).

CON implementatie van een service voor het beheer van cloudgebaseerde bron van luidt als volgt:

- Sommige kennis van de desbetreffende SCM-service vereist.

###<a name="vsts"></a>Het implementeren van continu uit een bron cloudgebaseerde control-service
U kunt in de [Portal van Azure](https://portal.azure.com)continue implementatie van GitHub, Bitbucket en Visual Studio Team Services configureren.

* [Continu implementatie Azure App-service](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Implementeren van lokale cijfer
Als het ontwikkelteam gebruikmaakt van een on-premises implementatie lokale bron code management (SCM)-service op basis van cijfer, kunt u dit als een bron implementatie voor App-Service configureren. 

Professionals van het implementeren van lokale cijfer zijn:

- Versiebeheer inschakelen terugdraaien.
- Tak / regiospecifieke implementatie kunt verschillende vertakkingen dashboard implementeren naar verschillende [sleuven](web-sites-staged-publishing.md).
- Alle functionaliteit in de Kudu implementatie-engine is niet beschikbaar (bijvoorbeeld implementatie versiebeheer, terugdraaien, pakket herstellen, automatisering).

CON implementatie van lokale cijfer van luidt als volgt:

- Sommige kennis van het desbetreffende SCM systeem vereist.
- Geen oplossingen inschakelen-toets voor doorlopende implementatie. 

###<a name="vsts"></a>Het implementeren van lokale cijfer
U kunt lokale cijfer implementatie configureren in de [Portal van Azure](https://portal.azure.com).

* [Lokale cijfer implementatie Azure App-service](app-service-deploy-local-git.md). 
* [Publiceren naar de Web Apps vanaf een cijfer/hg cessies‑retrocessies](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Met een IDE implementeren
Als u [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) al met een [Azure SDK](https://azure.microsoft.com/downloads/)of andere IDE-suites zoals [Xcode](https://developer.apple.com/xcode/), [Eclips](https://www.eclipse.org)en [IntelliJ IDEE](https://www.jetbrains.com/idea/)gebruikt, kunt u implementeren naar Azure rechtstreeks vanuit uw IDE. Deze optie is ideaal voor een individuele ontwikkelaars.

Visual Studio ondersteuning biedt voor alle drie implementatieprocessen (FTP, cijfer en Web implementeren), afhankelijk van uw voorkeur, terwijl andere IDEs naar App Service implementeren kunt als ze geïntegreerd met het FTP- of cijfer is (Zie [overzicht van implementatieprocessen](#overview)).

De professionals van implementatie met behulp van een IDE zijn:

- Mogelijk de tooling voor de levenscyclus van end-to-end-toepassing minimaliseren. Ontwikkel, foutopsporing, bijhouden en implementeer de app aan Azure alle zonder het verplaatsen van buiten uw IDE. 

De nadelen van implementatie met behulp van een IDE zijn:

- Toegevoegde complexiteit in tooling.
- Nog steeds vereist een systeem voor bronbeheer gebruikt voor een teamproject.

<a name="vspros"></a>Aanvullende professionals van de implementatie van Azure SDK met Visual Studio zijn:

- Azure SDK wordt Azure resources eersteklas burgers in Visual Studio. Maken, verwijderen, bewerken, starten, en stoppen apps, de backend SQL-database query, de Azure-app en nog veel meer live-foutopsporing. 
- Live codebestanden op Azure worden bewerkt.
- Live foutopsporing met apps op Azure.
- Geïntegreerde Azure explorer.
- Vergelijking alleen-lezen-implementatie. 

###<a name="vs"></a>Hoe u rechtstreeks vanuit Visual Studio implementeren

* [Aan de slag met Azure en ASP.NET](web-sites-dotnet-get-started.md). Het maken en implementeren van een eenvoudige ASP.NET MVC webproject met behulp van Visual Studio en Web implementeren.
* [Hoe u de implementatie van Azure WebJobs gebruik van Visual Studio](websites-dotnet-deploy-webjobs.md). Hoe projecten Console-toepassing configureren zodat ze te als WebJobs implementeren.  
* [Een beveiligde ASP.NET MVC 5-app met lidmaatschap, OAuth, en SQL-Database voor het WebApps implementeren](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Het maken en implementeren van een ASP.NET-MVC webproject met een SQL-database met behulp van Visual Studio, Web distribueren en entiteit Framework Code eerste migraties.
* [ASP.NET-Web-implementatie gebruik van Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Een 12-onderdeel reeks waarin een gedetailleerde reeks implementatietaken dan de andere in deze lijst. Sommige functies Azure-implementatie is toegevoegd nadat de zelfstudie is geschreven, maar later toegevoegde notities wordt uitgelegd wat ontbreekt.
* [Een ASP.NET-Website voor het Azure in Visual Studio 2012 uit een cijfer opslagplaats rechtstreeks implementeren](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Dit artikel wordt uitgelegd hoe u een ASP.NET-web-project Visual Studio, met de invoegtoepassing cijfer doorvoeren van de code cijfer en verbindende Azure naar de bibliotheek cijfer implementeren. In Visual Studio 2013 starten, cijfer ondersteuning is ingebouwd en installatie van een invoegtoepassing wordt niet vereisen.

###<a name="aztk"></a>Het implementeren van de Azure Toolkits gebruiken voor Eclips en IntelliJ IDEE

Microsoft stelt de Web Apps implementeren naar Azure rechtstreeks vanuit Eclips en IntelliJ via de [Azure Toolkit voor Eclips](../azure-toolkit-for-eclipse.md) en [Azure Toolkit voor IntelliJ](../azure-toolkit-for-intellij.md). De volgende zelfstudies illustreren de stappen die betrekking hebben op bij de implementatie van eenvoudige een 'Hallo' wereld Web App naar Azure IDE gebruiken:

*  [Een Hallo wereld Web-App voor Azure in Eclips maken](./app-service-web-eclipse-create-hello-world-web-app.md). Deze zelfstudie ziet u het gebruik van de Azure Toolkit voor Eclips maken en implementeren van een Hallo wereld Web App voor Azure.
*  [Een Hallo wereld Web-App voor Azure in IntelliJ maken](./app-service-web-intellij-create-hello-world-web-app.md). Deze zelfstudie ziet u het gebruik van de Azure Toolkit voor IntelliJ maken en implementeren van een Hallo wereld Web App voor Azure.


## <a name="automate"></a>Implementatie automatiseren met behulp van de opdrachtregel hulpmiddelen

* [-Implementatie waarbij MSBuild automatiseren](#msbuild)
* [Bestanden met FTP-hulpprogramma's en scripts kopiëren](#ftp)
* [Implementatie met Windows PowerShell automatiseren](#powershell)
* [Automatiseren-implementatie waarbij API voor .NET-beheer](#api)
* [Implementatie van Azure opdrachtregel (Azure CLI)](#cli)
* [Deploy vanaf opdrachtregel Web implementeren](#webdeploy)
* [De Batch FTP-scripts gebruiken](http://support.microsoft.com/kb/96269).
 
Er is een andere implementatie-optie voor het gebruik van een cloudservice zoals [Octopus implementeren](http://en.wikipedia.org/wiki/Octopus_Deploy). Zie [implementeren ASP.NET-toepassingen op Azure-websites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites)voor meer informatie.

###<a name="msbuild"></a>-Implementatie waarbij MSBuild automatiseren

Als u de [Visual Studio IDE](#vs) voor de ontwikkeling gebruikt, kunt u [MSBuild](http://msbuildbook.com/) alles wat die u in uw IDE doen kunt automatiseren. U kunt configureren MSBuild gebruik van [Web implementeren](#webdeploy) of [FTP/TRANSFEREERT](#ftp) om bestanden te kopiëren. Web Deploy ook vele andere implementatie-gerelateerde u taken kunt automatiseren, zoals het implementeren van databases.

Zie de volgende bronnen voor meer informatie over de opdrachtregel implementatie MSBuild gebruiken:

* [ASP.NET Web implementatie gebruik van Visual Studio: opdrachtregel implementatie](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tiende in een reeks zelfstudies over de implementatie van Azure gebruik van Visual Studio. Ziet u hoe u met de opdrachtregel implementeren nadat het instellen van profielen publiceren in Visual Studio.
* [Binnen de Engine Microsoft opbouwen: gebruik MSBuild en Team Foundation opbouwen](http://msbuildbook.com/). Gedrukte boeken die bevat hoofdstukken over het gebruik van MSBuild voor implementatie.

###<a name="powershell"></a>Implementatie met Windows PowerShell automatiseren

U kunt functies van MSBuild of FTP-implementatie uitvoeren vanuit [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Als u dat doet, kunt u ook een verzameling Windows PowerShell-cmdlets die de API voor het beheer van de REST van de Azure gemakkelijker om te bellen.

Zie de volgende bronnen voor meer informatie:

* [Een WebApp gekoppeld aan een bibliotheek GitHub implementeren](app-service-web-arm-from-github-provision.md)
* [Inrichten van een web-app met een SQL-Database](app-service-web-arm-with-sql-database-provision.md)
* [Provision en implementeren van microservices volgens plan in Azure wordt aangegeven](app-service-deploy-complex-application-predictably.md)
* [Building echte Cloud-Apps gebruiken met Azure - automatiseren alles](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-adresboek hoofdstuk waarin wordt uitgelegd hoe de voorbeeldtoepassing wordt weergegeven in het e-adresboek Windows PowerShell-scripts gebruikt om een Azure-testomgeving maken en implementeren naar deze. Zie het gedeelte [Resources](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) voor koppelingen naar aanvullende Azure PowerShell-documentatie.
* [Windows PowerShell-scripts gebruiken publiceren naar ontwikkelaar en testomgevingen](../vs-azure-tools-publishing-using-powershell-scripts.md). Hoe de implementatie scripts om Windows PowerShell gebruiken die wordt Visual Studio gegenereerd.

###<a name="api"></a>Automatiseren-implementatie waarbij API voor .NET-beheer

U kunt C#-code als u wilt uitvoeren MSBuild of FTP-functies voor implementatie schrijven. Als u dat doet, kunt u het Azure beheer REST API om uit te voeren beheerfuncties site kunt openen.

Zie de volgende informatiebron voor meer informatie:

* [Alles met de Azure Management bibliotheken en .NET automatiseren](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Inleiding tot de API voor .NET-beheer en koppelingen naar meer documentatie.

###<a name="cli"></a>Implementatie van Azure opdrachtregel (Azure CLI)

U kunt de opdrachtregel gebruiken in Windows, Mac of Linux machines implementatie via FTP. Als u dat doet, kunt u ook het beheer van Azure REST API met behulp van de Azure CLI openen.

Zie de volgende informatiebron voor meer informatie:

* [Azure-opdracht op de opdrachtregel](/downloads/#cmd-line-tools). Portalpagina in Azure.com voor informatie over het programma van de opdrachtregel.

###<a name="webdeploy"></a>Deploy vanaf opdrachtregel Web implementeren

[Web implementeren](http://www.iis.net/downloads/microsoft/web-deploy) is Microsoft-software voor implementatie naar IIS die niet alleen intelligente bestand synchroniseren functies biedt, maar ook kunt uitvoeren of vele andere implementatie-taken die niet automatisch worden uitgevoerd coördineren wanneer u FTP gebruikt. Bijvoorbeeld kunt Web implementeren implementeren een nieuwe database of updates van de database samen met uw web-app. Web Deploy kunt ook de benodigde tijd voor het bijwerken van een bestaande site omdat deze slim alleen gewijzigde bestanden kunt kopiëren minimaliseren. Microsoft Visual Studio en Team Foundation Server hebben ondersteuning voor het Web ingebouwde implementeren, maar u kunt ook een Web implementeren rechtstreeks vanaf de opdrachtregel gebruiken om te automatiseren implementatie. Web Deploy opdrachten zijn zeer krachtig, maar leren scherpe kan zijn.

Zie de volgende informatiebron voor meer informatie:

* [Eenvoudige WebApps: implementatie](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog door David Ebbo over een hulpmiddel die hij geschreven zodat u eenvoudiger gebruik Web implementeren.
* [Web-implementatieprogramma](http://technet.microsoft.com/library/dd568996). Officiële documentatie op de Microsoft TechNet-site. Gedateerd is maar nog steeds een goede locatie om te beginnen.
* [Via Web implementeren](http://www.iis.net/learn/publish/using-web-deploy). Officiële documentatie op de Microsoft IIS.NET-site. Ook gedateerd is maar een goede locatie om te beginnen.
* [ASP.NET Web implementatie gebruik van Visual Studio: opdrachtregel implementatie](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild is de opbouwen engine die worden gebruikt door Visual Studio en deze kan ook worden gebruikt vanaf de opdrachtregel voor webtoepassingen tot Web Apps implementeren. Deze zelfstudie maakt deel uit van een reeks die voornamelijk over Visual Studio-implementatie.

##<a name="nextsteps"></a>Volgende stappen

In sommige gevallen wilt u mogelijk kunnen eenvoudig heen en weer schakelen tussen een gefaseerde installatie en een versie van de app. Zie [Gefaseerde implementatie op Web-Apps](web-sites-staged-publishing.md)voor meer informatie.

Een back-up en herstellen-abonnement hebt op hun plaats staan, is een belangrijk onderdeel van een werkstroom voor implementatie. Voor informatie over de App-Service back-up maken en terugzetten van de functie, Zie [Back-ups van Web Apps](web-sites-backup.md).  

Zie voor informatie over het gebruik van de Azure Rolgebaseerd toegangsbeheer voor het beheren van toegang tot de implementatie van de App Service [RBAC en App webpublicaties](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
