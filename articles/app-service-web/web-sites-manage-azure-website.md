<properties 
    pageTitle="Een WebApp in Azure App-Service beheren" 
    description="Koppelingen naar bronnen voor het beheren van een WebApp in Azure App-Service." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Een WebApp in Azure App-Service beheren

In dit onderwerp bevat koppelingen naar resources voor het beheren van een WebApp in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714). Beheer omvat alle taken die uw web-app soepel behouden. 

In de loop van een web-app, wordt u verschillende beheertaken uitvoeren wanneer u uit de eerste installatie hebt verplaatst naar de normale werking, onderhoud en updates.

Veel beheertaken voor web-app kunnen worden uitgevoerd in de Portal Azure.

## <a name="before-you-deploy-your-web-app-to-production"></a>Voordat u uw web-app dashboard naar productie implementeren

### <a name="choose-a-tier"></a>Kies een laag

Azure-App-Service wordt aangeboden in vijf niveaus: vrij, gedeeld, Basic, Standard en Premium. Zie [details prijzen](/pricing/details/app-service/)voor informatie over de functies en prijzen voor elke laag. 

- [App-Service-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kunt u meerdere WebApps onder hetzelfde niveau groeperen.
- Kunt u altijd [schakelen lagen](web-sites-scale.md) nadat u uw web-app hebt gemaakt.

### <a name="configuration"></a>Configuratie

Verschillende configuratieopties instellen via de [Portal van Azure](https://portal.azure.com/) . Zie [web-apps in Azure App-Service configureren](web-sites-configure.md)voor meer informatie. Hier volgt een snelle Controlelijst:

- Selecteer **runtime versies** voor .NET, PHP, Java of Python, indien nodig.
- **WebSockets** inschakelen als uw web-app wordt gebruikt voor het WebSocket-protocol. (Inclusief apps die gebruikmaken van [ASP.NET SignalR](http://www.asp.net/signalr) of [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Voert u doorlopend web taken? Als dat het geval is, schakelt u **Altijd op**.
- Stel de **standaarddocument**, zoals index.html.

Naast deze eenvoudige configuratie-instellingen wilt u mogelijk het volgende configureren:

- **Secure Socket Layer (SSL)** -codering. Als u wilt gebruiken SSL met een aangepaste domeinnaam, moet u een SSL-certificaat en configureren van uw web-app als u wilt gebruiken. Zie [HTTPS inschakelen voor een web-app in Azure App-Service](web-sites-configure-ssl-certificate.md).
- **Aangepaste domeinnaam.** Uw web-app heeft automatisch een subdomein onder azurewebsites.net. U kunt een aangepaste domeinnaam, zoals contoso.com koppelen. Zie [een aangepaste domeinnaam in Azure App-Service configureren](web-sites-custom-domain-name.md).

Taalspecifieke configuratie:

- **PHP**: [PHP in WebApps Azure App-Service configureren](web-sites-php-configure.md).
- **Python**: [Python met WebApps Azure App-Service configureren](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Terwijl uw web-app wordt uitgevoerd

Terwijl uw web-app wordt uitgevoerd, die u wilt controleren of deze beschikbaar is en dat het schalen om te voldoen aan de gebruiker-verkeer is toegestaan. Mogelijk moet u ook synchronisatiefouten oplossen.

### <a name="monitoring"></a>Cmdlets voor controle

- U kunt de [prestatiegegevens toevoegen](web-sites-monitor.md) zoals CPU-gebruik en het aantal clientaanvragen uit te voeren via de Portal Azure.
- [Schaal uw web-app](web-sites-scale.md) reactie op verkeer. Afhankelijk van uw laag, kunt u het aantal VMs en/of de grootte van de exemplaren VM schalen. In de standaardweergave en de Premium-lagen, kunt u ook instellen autoscaling, zodat uw web-app wordt automatisch aangepast op een vaste planning, of in antwoord op laden.  
 
### <a name="backups"></a>Back-ups

- Stel [Automatische back-ups](web-sites-backup.md) van uw web-app. Meer informatie over back-ups in [deze video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Meer informatie over de opties voor het [herstellen van databases](../sql-database/sql-database-business-continuity.md) van Azure SQL-Database.

### <a name="troubleshooting"></a>Problemen oplossen

- Als er iets mis gaat, kunt u [problemen met in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), met diagnostische logboeken en live foutopsporing in de cloud. 
- Er zijn verschillende manieren voor het verzamelen van diagnostische logboeken buiten Visual Studio. Zie [Diagnostische gegevens vastleggen voor web-apps in Azure App-Service inschakelen](web-sites-enable-diagnostic-log.md).
- Zie voor Node.js-toepassingen, [hoe u fouten opsporen in een Node.js web-app in Azure App-Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Upgegevens terugzetten

- [Herstellen](web-sites-restore.md) een WebApp die eerder back-up.


## <a name="when-you-update-your-web-app"></a>Wanneer u uw web-app bijwerken

Als u automatische back-ups niet hebt ingeschakeld, kunt u een [handmatig back-up](web-sites-backup.md)maken.

Kunt u overwegen [gefaseerde implementatie](web-sites-staged-publishing.md). Deze optie kunt u updates publiceren naar een tijdelijk opslaan implementatie die wordt uitgevoerd naast elkaar met uw productie-implementatie. 

Als u Visual Studio Team Services gebruikt, kunt u doorlopend implementatie van een besturingselement voor gegevensbronnen instellen:

- [Versiebeheer voor Team Foundation (TFVC) gebruiken](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Cijfer gebruiken](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
