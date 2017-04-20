<properties
    pageTitle="Een app in Azure App-Service beveiligen"
    description="Leer hoe u een WebApp, de mobiele app backend of de API-app in Azure App-Service beveiligen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Een app in Azure App-Service beveiligen

In dit artikel vindt u aan de slag over het beveiligen van uw WebApp, mobiele app backend of API-app in Azure App-Service. 

Beveiliging in Azure App-Service heeft twee niveaus: 

- **Beveiliging infrastructuur en platform** - You trust Azure als u wilt dat de services die u nodig hebt om uit te voeren dat is wel dingen veilig in de cloud.
- **Beveiliging van toepassing** - moet u de app zelf veilig ontwerpen. Dit geldt ook hoe u integreren met Azure Active Directory, hoe u certificaten beheren en hoe u ervoor zorgen dat u veilig met andere services kunt praten. 

#### <a name="infrastructure-and-platform-security"></a>Beveiliging infrastructuur en platform
Omdat het App-Service de VMs Azure, opslag, netwerkverbindingen, web kaders, beheer en integratiefuncties en nog veel meer, houdt dit actief is beveiligd en harde en doorloopt krachtige naleving en Hiermee wordt gecontroleerd op doorlopend om ervoor te zorgen dat:

- Uw App Service-apps worden geïsoleerd van beide Internet en naar de andere klanten Azure resources.
- Communicatie van geheimen (bijvoorbeeld verbindingstekenreeksen) tussen uw App Service-app en andere Azure resources (bijvoorbeeld een SQL-Database) in een resourcegroep blijft binnen Azure en niet de grenzen van een netwerk cross. Geheimen worden altijd gecodeerd.
- Alle communicatie tussen uw App Service-app en externe bronnen, zoals PowerShell management, opdrachtregel-interface, Azure SDK's, REST API's en hybride verbindingen, zijn correct gecodeerd.
- 24-uursnotatie bedreiging management App Service resources beschermen tegen malware, distributed weigering-van-service (DDoS), man-in-het-midden (MITM), en andere threats. 

Zie voor meer informatie over de beveiliging van infrastructuur en platform in Azure wordt aangegeven, [Azure Vertrouwenscentrum](/support/trust-center/security/).

#### <a name="application-security"></a>Toepassingsbeveiliging

Azure is verantwoordelijk voor het beveiligen van de infrastructuur en de platform die op uw toepassing wordt uitgevoerd, is uw verantwoordelijkheid te beveiligen van uw toepassing zelf. Met andere woorden, moet u ontwikkelen en beheren van uw toepassingscode en de inhoud in een veilige manier implementeren. Zonder deze kan uw toepassingscode of inhoud nog steeds vatbaar voor threats, zoals:

- SQL-webweergave
- Sessie hackprogramma
- Cross-site-scripting
- Niveau van de webtoepassing MITM
- Niveau van de webtoepassing DDoS

Een volledige discussie van beveiligingsoverwegingen voor webtoepassingen valt buiten het bereik van dit document. Als uitgangspunt voor verdere instructies over het beveiligen van uw toepassing, raadpleegt u de [Toepassing van Open Web Project beveiliging (OWASP)](https://www.owasp.org/index.php/Main_Page), specifiek de [top 10-project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), waarin u de huidige bovenste 10 kritieke web application beveiligingsfouten, afhankelijk van de OWASP leden.

## <a name="perform-penetration-testing-on-your-app"></a>Er testen op uw app uitvoeren

Een van de beste manieren om het nog niet begonnen met het testen op problemen bij de app van uw App Service is de [integratie met folie beveiliging](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) via één muisklik beveiligingsprobleem scannen op uw app uitvoeren. U kunt de resultaten bekijken in een rapport gemakkelijk te begrijpen en informatie over het oplossen van elk beveiligingsprobleem met stapsgewijze instructies.

Als u liever uw eigen er onderzoeken of een andere scanner suite of provider wilt gebruiken, moet u het [goedkeuringsproces van Azure er testen](https://security-forms.azure.com/penetration-testing/terms) Volg en verkrijgen van voorafgaande toestemming om de gewenste er tests uitvoeren.

##<a name="https"></a>Beveiligde communicatie met klanten

Als u de ** \*. azurewebsites.net** domeinnaam die zijn gemaakt voor uw App Service-app, u kunt direct HTTPS, zoals een SSL-certificaat is opgegeven voor alle ** \*. azurewebsites.net** domeinnamen. Als u uw site een [aangepaste domeinnaam](web-sites-custom-domain-name.md)gebruikt, kunt u een SSL-certificaat uploaden naar [HTTPS inschakelen](web-sites-configure-ssl-certificate.md) voor het aangepaste domein.

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) inschakelen kan bijdragen aan beveiliging tegen MITM aanvallen op de communicatie tussen uw app en de gebruikers.

## <a name="secure-data-tier"></a>Secure van gegevens

App-Service wordt ten zeerste geïntegreerd met SQL-Database, zodat alle verbindingstekenreeksen met de via het bord zijn versleuteld en alleen worden ontsleuteld op de VM die de app wordt uitgevoerd op *en* alleen wanneer de app wordt uitgevoerd. Bovendien bevat Azure SQL-Database veel beveiligingsfuncties waarmee u de toepassingsgegevens uit cyber threats, inclusief [versleuteling in rust](https://msdn.microsoft.com/library/dn948096.aspx), [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamische gegevens Masking](../sql-database/sql-database-dynamic-data-masking-get-started.md)en [Bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md)secure. Als u vertrouwelijke gegevens of nalevingsvereisten, raadpleegt u de [beveiliging van uw SQL-Database](../sql-database/sql-database-security.md) voor meer informatie over het beveiligen van uw gegevens.

Als u een databaseprovider van derden, zoals ClearDB, gebruikt moet u contact opnemen met de documentatie van de provider rechtstreeks op aanbevolen procedures voor beveiliging.  

##<a name="develop"></a>Secure ontwikkeling en implementatie

### <a name="publishing-profiles-and-publish-settings"></a>Publicatie van gebruikersprofielen en instellingen publiceren

Bij het ontwikkelen van toepassingen, beheertaken voor uitvoering of automatiseren met hulpprogramma's zoals **Visual Studio**, **Web Matrix**, **Azure PowerShell** of de **Interface van Azure-opdrachtregel (Azure CLI)**, kunt u een bestand *publiceren instellingen* of een *publicerende profiel*. Beide bestandstypen u verificatie met Azure en om te voorkomen dat onbevoegden toegang moeten worden beveiligd.

* Een bestand **publiceren instellingen** bevat

    * Uw Azure abonnements-ID

    * Een certificaat management waarmee u beheertaken voor uw abonnement *zonder op te geven van een accountnaam of een wachtwoord*.

* Een bestand **publiceren profiel** bevat

    * Informatie voor publicatie voor uw app

Als u een hulpprogramma waarin een instellingen publiceren of publiceren profielbestand gebruikt, moet u het bestand dat met de publicatie-instellingen of profiel in het hulpprogramma en vervolgens **verwijderen** het bestand importeren. Als u het bestand, om te delen met anderen werken aan het project bijvoorbeeld bewaart het opslaan op een veilige locatie, zoals een *versleutelde* directory met beperkte machtigingen.

Daarnaast moet u controleren of dat de geïmporteerde referenties zijn beveiligd. Bijvoorbeeld **Azure PowerShell** en de **Interface van Azure-opdrachtregel (Azure CLI)** geïmporteerde gegevens opslaan in uw **basismap** (*~* op Linux of OS X-systemen en */users/yourusername* op Windows-systemen.) Voor extra beveiliging en u mogelijk wilt **versleutelen** deze locaties met behulp van versleuteling hulpprogramma's voor uw besturingssysteem.

### <a name="configuration-settings-and-connection-strings"></a>Configuratie-instellingen en verbindingstekenreeksen
Is het gebruikelijk verbindingstekenreeksen, verificatiereferenties en andere vertrouwelijke gegevens opgeslagen in configuratiebestanden. Helaas kunnen deze bestanden worden weergegeven op uw website of ingecheckt in een openbare opslagplaats, dat deze informatie. Een eenvoudige zoeken op [GitHub](https://github.com), bijvoorbeeld kunt talloze configuratiebestanden met weergegeven geheimen in de openbare opslagplaatsen schuiven.

De beste manier is om te verhinderen deze informatie uit van uw app configuratie-bestanden. App Service kunt u configuratiegegevens opslaan als onderdeel van de runtimeomgeving: **app-instellingen** en **verbindingstekenreeksen met de**. De waarden worden blootgesteld aan uw toepassing gedurende runtime tot en met *omgevingsvariabelen* voor de meeste programming talen. Voor .NET-toepassingen, worden deze waarden in de configuratie van uw .NET gedurende runtime toegevoegd. Afgezien van deze situaties blijft deze configuratie-instellingen versleutelde tenzij u weergeven of ze via de [Portal van Azure](https://portal.azure.com) of hulpprogramma's zoals PowerShell of de CLI Azure configureren. 

Configuratiegegevens opslaan in App Service kan voor de beheerder van de app vergrendelen vertrouwelijke informatie voor de productie-apps. Ontwikkelaars kunnen een aparte set configuratie-instellingen gebruiken voor het ontwikkelen van Apps en de instellingen kunnen automatisch worden vervangen door de instellingen die in de App-Service. Zelfs niet de ontwikkelaars moeten de geheimen die is geconfigureerd voor de app productie weten. Zie voor meer informatie over het configureren van instellingen van de app en verbindingstekenreeksen in de App-Service [configureren WebApps](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure App-Service biedt secure FTP-toegang tot het bestandssysteem voor de app via **TRANSFEREERT**. Hiermee kunt u veilig toegang tot de toepassingscode op het WebApp, evenals de hulpprogramma's voor diagnose Logboeken. Het wordt aanbevolen dat u altijd TRANSFEREERT in plaats van FTP gebruiken. 

De koppeling TRANSFEREERT voor uw app vindt u de volgende stappen uit:

1. Open de [Portal van Azure](https://portal.azure.com).
2. Selecteer **door alles bladeren**.
3. Selecteer in het blad **Blader** **App-Services**.
4. Selecteer de gewenste app in het blad **App-Services** .
5. Selecteer **alle instellingen**van de app blade.
6. Selecteer de **Eigenschappen**van het blad **Instellingen** .
7. De FTP- en TRANSFEREERT koppelingen vindt u op het blad **Instellingen** . 

Zie voor meer informatie over TRANSFEREERT, [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Volgende stappen

Zie de Beveiligingssectie van het [Vertrouwenscentrum in Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/)voor meer informatie over de beveiliging van de Azure platform, informatie over het rapporteren van een **waardepapier incident of abuse**, of voor Microsoft informeren dat u **er testen** van uw site uitvoeren zult.

Zie [configuratieopties ontgrendelen in Azure App Service web-apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)voor meer informatie over **web.config** of **applicationhost.config** -bestanden in App Service-apps.

Voor informatie over logboekinformatie voor App Service-apps, die mogelijk nuttig bij het opsporen van aanvallen, raadpleegt u [Diagnostische gegevens vastleggen](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter-app in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd

* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)
