<properties
    pageTitle="Azure App Service, virtuele Machines Service stof en Cloud Services-vergelijking | Microsoft Azure"
    description="Leer hoe u kiezen tussen Azure App-Service, virtuele Machines Service stof en Cloud Services voor het hosten van webtoepassingen."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure App Service, virtuele Machines Service stof en Cloud Services-vergelijking

## <a name="overview"></a>Overzicht

Azure biedt verschillende manieren om te host websites: [Azure App-Service][], [virtuele Machines][] [Service stof][]en [Cloudservices][]. Dit artikel vindt u informatie over de filteropties en het beste kiezen voor uw webtoepassing.

Azure App-Service is de beste keuze voor de meeste web-apps. Implementatie en beheer zijn geïntegreerd in het platform, sites kunnen snel schalen dat voor het afhandelen van hoge verkeer laadtijd en de manager van taakverdeling en verkeer ingebouwde laden beschikbaarheid bieden. U kunt bestaande sites naar Azure App Service eenvoudig gaan met een [migratieprogramma van online](https://www.migratetoazure.net/), een open source-app gebruiken uit de galerie met webonderdelen toepassing of een nieuwe site met de framework en hulpprogramma's van uw keuze maken. De functie [WebJobs][] kunt u gemakkelijk naar achtergrondtaak verwerking van uw App Service web-app toevoegen.

Service stof is een goede keuze als u een nieuwe app maakt of een bestaande app als u wilt gebruiken, een architectuur microservice opnieuw te schrijven. Apps, waarin op een gedeelde groep van machines uitvoert, kunnen kleine starten en toenemen tot enorme schalen met honderden of duizenden machines naar wens. Statuscontrole services vergemakkelijkt consistente en betrouwbaar opslaan app staat, en Service stof automatisch worden beheerd door service partitioneren, schaalbaarheid en beschikbaarheid voor u.  Service stof ondersteunt ook WebAPI met Open Web-Interface voor .NET (OWIN) en ASP.NET Core.  Vergeleken met App-Service, vindt Service stof ook u meer controle over of directe toegang tot de onderliggende infrastructuur. U kunt externe in uw servers of server opstarttaken configureren. Cloudservices is vergelijkbaar met Service-structuur in de mate van besturingselement versus gebruiksgemak, maar het is nu een oudere service en Service stof geschikt is voor de ontwikkeling van nieuwe.

Als u een bestaande toepassing waarvoor aanzienlijke wijzigingen aan de App Service of Service stof uitvoeren hebt, kunt u virtuele Machines om te vereenvoudigen migreren naar de cloud. Correct vereist configureren, beveiligen en beheren van VMs echter nog veel meer tijd en IT-expertise vergeleken met Azure App-Service en Service stof. U bent Azure virtuele Machines plan, zorg er dan voor dat u rekening houden met het doorlopend onderhoud inspanning patch, bijwerken en beheren van uw omgeving VM. Azure virtuele Machines is infrastructuur-als-een-Service (IaaS), terwijl de App Service en Service stof zijn Platform-als-een-Service (Paas). 

## <a name="features"></a>Vergelijking van de functie

De volgende tabel worden de mogelijkheden van de App Service, Cloudservices virtuele Machines en Service stof om de beste keuze te vergeleken. Zie [Azure niveau serviceovereenkomsten](/support/legal/sla/)voor actuele informatie over de SERVICEOVEREENKOMST voor elke optie.

Functie|App-Service (WebApps)|Cloudservices (web rollen)|Virtuele Machines|Service stof|Notities
---|---|---|---|---|---
Handomdraai-implementatie|X|||X|Een toepassing of een toepassingsupdate van de implementeren naar een Cloudservice of het maken van een VM, gaat enkele minuten ten minste; implementatie van een toepassing naar een web-app, duurt seconden.
Uitgebreid naar grotere machines zonder in dat geval|X|||X|
Web serverexemplaren delen inhoud en configuratie, wat betekent dat u niet hoeft dat te implementeren of te configureren, zoals u de schaal van.|X|||X|
Meerdere implementatieomgevingen (productie en tijdelijke)|X|X||X|Service stof kunt u meerdere omgevingen voor uw apps of verschillende versies van uw app naast elkaar implementeren.
Automatische OS management bijwerken|X|X|||Automatische updates voor OS zijn gepland voor een toekomstige Service stof loslaat.
Naadloze platform overstappen (eenvoudig verplaatsen tussen de 32-bits en 64-bits)|X|X|||
Code met cijfer, FTP-implementatie|X||X||
Code implementeren met Web implementeren|X||X||Cloudservices ondersteunt het gebruik van het Web implementeren naar updates implementeren in afzonderlijke rol exemplaren. Echter u deze niet gebruiken voor de eerste installatie van een rol en als u een Web implementeren om een update gebruikt u moet implementeren afzonderlijk op elk exemplaar van een rol. Meerdere exemplaren zijn vereist om in aanmerking voor de Cloud Service SLA voor productieomgevingen te.
Ondersteuning voor WebMatrix|X||X||
Toegang tot services zoals Service Bus, opslag, SQL-Database|X|X|X|X|
Host Internet of een web services laag van een architectuur met meerdere niveaus|X|X|X|X|
Host middelste laag van een architectuur met meerdere niveaus|X|X|X|X|App Service-WebApps kunnen eenvoudig een REST API middelste laag hosten en de functie [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) kunt hosten achtergrond verwerking van taken. U kunt WebJobs uitvoeren in een eigen website om te bereiken onafhankelijke schaalbaarheid voor de laag. De voorbeeldfunctie [API apps](../app-service-api/app-service-api-apps-why-best-platform.md) biedt nog meer functies voor het hosten van andere services.
Geïntegreerde MySQL-als-een-service ondersteuning|X|X|X||Cloudservices kunnen MySQL-als-een-service integreren via de ClearDB aanbiedingen, maar niet als onderdeel van de werkstroom Azure-Portal.
Ondersteuning voor ASP.NET, klassieke ASP, Node.js, PHP, Python|X|X|X|X|Service stof ondersteunt het maken van een webpagina front elk type toepassing (Node.js, Java, enzovoort) als een [Gast uitvoerbare](../service-fabric/service-fabric-deploy-existing-app.md)met [ASP.NET-5](../service-fabric/service-fabric-add-a-web-frontend.md) , of u kunt implementeren.
De schaal van uit naar meerdere exemplaren zonder in dat geval aanpassen|X|X|X|X|Virtuele Machines kunnen worden aangepast aan meerdere exemplaren, maar de services worden uitgevoerd op deze moeten worden afgehandeld deze schalen worden geschreven. U moet een taakverdeling om te routeren aanvragen op de computers en maken van een groep affiniteit om te voorkomen dat tegelijk wordt opgestart overal vanwege onderhoud of hardware pas uit fouten configureren.
Ondersteuning voor SSL|X|X|X|X|App Service web-apps voor wordt SSL voor aangepaste domeinnamen alleen ondersteund voor Basic en standaard-modus. Zie [configureren van een SSL-certificaat voor een Azure-Website](../app-service-web/web-sites-configure-ssl-certificate.md)voor informatie over het gebruik van SSL met WebApps.
Visual Studio-integratie|X|X|X|X|
Foutopsporing op afstand|X|X|X||
Code met TFS implementeren|X|X|X|X|
Netwerk moeten worden geïsoleerd met [Azure Virtual Network](/services/virtual-network/)|X|X|X|X|Zie ook [Azure Websites Virtual Network-integratie](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Ondersteuning voor [Azure verkeer Manager](/services/traffic-manager/)|X|X|X|X|
Geïntegreerde eindpunt bewaken|X|X|X||
De toegang tot extern bureaublad met servers||X|X|X|
Een aangepaste MSI installeren||X|X|X|Service stof kunt u voor het hosten van een uitvoerbaar bestand als een [Gast uitvoerbare](../service-fabric/service-fabric-deploy-existing-app.md) of kunt u een app installeren op de VMs.
Mogelijkheid opstart taken definiëren/uitvoeren||X|X|X|
ETW gebeurtenissen kunt volgen||X|X|X|

## <a name="scenarios"></a>Scenario's en aanbevelingen

Hier volgen enkele gangbare toepassingen-scenario's met aanbevelingen over welke Azure webhosting optie meest geschikt zijn voor elk mogelijk zijn.

- [Ik heb nodig een front-end van web met achtergrond verwerking en database backend zakelijke toepassingen geïntegreerd met op premises activa.](#onprem)
- [Ik zoek een betrouwbare manier voor het hosten van mijn zakelijke website die ook schalen en aanbiedingen globale hebt bereikt.](#corp)
- [Ik heb een IIS6-toepassing op Windows Server 2003.](#iis6)
- [Ik ben de eigenaar van een klein bedrijf en ik heb een goedkope manier voor het hosten van Mijn site nodig, maar met toekomstige groei mee.](#smallbusiness)
- [Ik ben een Internet of een afbeelding designer, maar ik wil ontwerpen en bouwen van websites voor mijn klanten.](#designer)
- [Ik ben mijn met meerdere niveaus-toepassing met een front web migreren naar de Cloud.](#multitier)
- [Mijn-toepassing, hangt af van hoge mate aangepaste Windows of Linux-omgevingen en ik wilt verplaatsen naar de cloud.](#custom)
- [Mijn site gebruikt bron openen software en ik wil het hosten in Azure wordt aangegeven.](#oss)
- [Ik heb een LOB-toepassing die vereist zijn voor het verbinding maken met het bedrijfsnetwerk bevinden.](#lob)
- [Ik wil een REST API of de webservice voor mobiele clients hosten.](#mobile)


### <a id="onprem"></a>Ik heb nodig een front-end van web met achtergrond verwerking en database backend zakelijke toepassingen geïntegreerd met op premises activa.

Azure App-Service is een goede oplossing voor complexe zakelijke toepassingen. U kunt het ontwikkelen van apps die schaal automatisch op een gebalanceerde platform laden, zijn beveiligd met Active Directory en verbinding maken met uw lokale bronnen. Wordt deze apps eenvoudig via een hoogwaardige portal en API's beheren, en kunt u inzicht in de hoe klanten ze worden gebruikt met app inzicht hulpmiddelen. De functie [Webjobs][] kunt u achtergrondprocessen uitvoeren en taken als onderdeel van uw weblaag, terwijl de hybride connectiviteit en VNET functies wordt vergemakkelijkt on-premises implementatie resources verbinding te maken. Azure App-Service biedt drie 9 SLA voor WebApps en kunt u:

* Uw toepassingen betrouwbaar uitvoeren op een zelfreparerende, automatisch patches cloud-platform.
* Schaal automatisch over een globale netwerk van datacenters.
* Een back-up en herstellen voor herstel.
* ISO, SOC2 en PCI die compatibel zijn.
* Integreren met Active Directory

### <a id="corp"></a>Ik zoek een betrouwbare manier voor het hosten van mijn zakelijke website die ook schalen en aanbiedingen globale hebt bereikt.

Azure App-Service is een goede oplossing voor het hosten van zakelijke websites. WebApps aan de nieuwe schaal snel en gemakkelijk aanvraag vergaderen via een globale netwerk datacenters kunt. Deze optie biedt lokale bereik, fouttolerantie en intelligente verkeer management. Alle op een platform vindt u hulpmiddelen voor projectbeheer hoogwaardige, zodat u inzicht in de site gezondheid en siteverkeer snel en gemakkelijk. Azure App-Service biedt drie 9 SLA voor WebApps en kunt u:

* Uw websites betrouwbaar uitvoeren op een zelfreparerende, automatisch patches cloud-platform.
* Schaal automatisch over een globale netwerk van datacenters.
* Een back-up en herstellen voor herstel.
* Logboeken beheren en verkeer met geïntegreerde hulpmiddelen.
* ISO, SOC2 en PCI die compatibel zijn.
* Integreren met Active Directory

### <a id="iis6"></a>Ik heb een IIS6-toepassing op Windows Server 2003.

Azure App-Service kunt u gemakkelijk om te voorkomen dat de infrastructuurkosten die is gekoppeld aan het migreren van oudere IIS6-toepassingen. Microsoft heeft gemaakt [eenvoudig te gebruiken migratiehulpmiddelen en gedetailleerde migratie richtlijnen](https://www.movemetowebsites.net/) waarmee u kunt de compatibiliteit controleren en de wijzigingen aan die moeten worden aangebracht identificeren. Integratie met Visual Studio, TFS en algemene CMS's kunt u heel gemakkelijk implementeren IIS6 toepassingen rechtstreeks naar de cloud. Zodra geïmplementeerd, Portal de Azur krachtige beheerprogramma's waarmee u kunt de schaal omlaag voor het beheren van kosten en maximaal vergaderen vraag zo nodig. Met het Migratiehulpmiddel uitvoert, kunt u het volgende doen:

* Snel en gemakkelijk migreren uw oudere Windows Server 2003-webtoepassing in de cloud.
* Ook voor kiezen om uw bijgevoegde SQL-database on-premises als u wilt maken van een toepassing voor hybride verlaten.
* Automatisch verplaatsen uw SQL-database samen met uw oudere toepassing.

### <a id="smallbusiness"></a>Ik ben de eigenaar van een klein bedrijf en ik heb een goedkope manier voor het hosten van Mijn site nodig, maar met toekomstige groei mee.

Azure App-Service is een goede oplossing voor dit scenario, omdat u kunt deze gratis gebruiken en vervolgens meer mogelijkheden toevoegen wanneer u deze nodig hebt. Elke gratis WebApp wordt geleverd met een domein verstrekt door Azure (*your_company*. azurewebsites.net), en het platform bevat geïntegreerde hulpmiddelen voor implementatie en beheer, evenals een toepassing galerie waarmee u snel aan de slag. Zijn er veel andere services en schaalopties waarmee de site met de verbeterde gebruiker aanvraag ontwikkelen. Met Azure App-Service, kunt u het volgende doen:

- Beginnen met de gratis laag en vervolgens vergroten naar wens.
- Gebruik de galerie toepassing om in te stellen snel populaire webtoepassingen, zoals WordPress.
- Aanvullende Azure services en functies toevoegen aan uw toepassing naar wens.
- Beveiligen van uw web-app met HTTPS.

### <a id="designer"></a>Ik ben een Internet of een afbeelding designer, maar ik wil ontwerpen en bouwen van websites voor mijn klanten

Voor webontwikkelaars en ontwerpers, Azure App-Service eenvoudig werkt naadloos samen met een groot aantal kaders en hulpmiddelen voor implementatie-ondersteuning voor cijfer en FTP bevat en biedt naadloze integratie met hulpprogramma's en -services zoals Visual Studio en SQL-Database. Met de App-Service, kunt u het volgende doen:

- Gebruik van de opdrachtregel hulpmiddelen voor [geautomatiseerde taken][scripting].
- Werken met populaire talen zoals [.net][dotnet], [PHP][], [Node.js][nodejs], en [Python][].
- Selecteer drie verschillende schaal niveaus voor schaalbaarheid omhoog naar zeer hoog capaciteiten.
- Integreren met andere Azure-services, zoals [SQL-Database][sqldatabase], [Service Bus] [ servicebus] en [opslag][]of partner aanbiedingen van de [Azure Store][azurestore], zoals MySQL en MongoDB.
- Integreren met hulpprogramma's zoals Visual Studio, cijfer WebMatrix, WebDeploy, TFS en FTP.

### <a id="multitier"></a>Ik ben mijn met meerdere niveaus-toepassing met een front web migreren naar de Cloud

Als u werkt met een met meerdere niveaus-toepassing, zoals een webserver die is verbonden met een database, is Azure-Service voor App een goede optie die naadloze integratie met Azure SQL-Database biedt. En u kunt de functie WebJobs gebruiken voor het uitvoeren van back-end-processen.

Kies een of meer van uw niveaus Service stof als nodig hebt u meer controle over de server-omgeving, zoals de mogelijkheid om te externe naar uw server of server opstarttaken configureren.

Kies virtuele Machines voor een of meer van uw niveaus als u wilt gebruiken van de afbeelding van uw eigen computer of Serversoftware of services die u niet op stof Service configureren.

### <a id="custom"></a>Mijn-toepassing, hangt af van hoge mate aangepaste Windows of Linux-omgevingen en ik wilt verplaatsen naar de cloud.

Als uw toepassing vereist complexe installatie of configuratie van software en het besturingssysteem, is virtuele Machines waarschijnlijk de beste oplossing. Met virtuele Machines, kunt u het volgende doen:

- De galerie virtuele Machine gebruiken om te beginnen met een besturingssysteem gebruikt, zoals Windows of Linux, en vervolgens aanpassen voor uw toepassingsvereisten voor de.
- Maken en uploaden van een aangepaste afbeelding van een bestaande on-premises implementatie-server om uit te voeren op een virtuele machine in Azure wordt aangegeven.

### <a id="oss"></a>Mijn site bron openen software wordt gebruikt, maar ik wil host deze in Azure wordt aangegeven

Als uw framework bron openen wordt ondersteund op App-Service, de talen en kaders die nodig zijn voor uw toepassing automatisch voor u zijn geconfigureerd. App Service kunt u:

- Het gebruik van vele populaire openen bron talen, zoals [.NET][dotnet], [PHP][], [Node.js][nodejs], en [Python][].
- WordPress, Drupal Umbraco, DNN en vele andere webtoepassingen van derden instellen.
- Migreren van een bestaande toepassing of maak een nieuwe record in de galerie-toepassing.

Als uw framework bron openen niet wordt ondersteund op App-Service, kunt u deze kunt uitvoeren op een van de andere Azure webhosting opties. Met virtuele Machines, installeert en configureert u de software op de afbeelding machine Windows kan zijn of Linux-gebaseerde.

### <a id="lob"></a>Ik heb een LOB-toepassing die vereist zijn voor het verbinding maken met het bedrijfsnetwerk bevinden

Als u een LOB-toepassing maken wilt, moet uw website mogelijk directe toegang tot services of gegevens op het bedrijfsnetwerk bevinden. Dit is mogelijk op App-Service, Service stof en virtuele Machines met de [Azure virtuele netwerkservice](/services/virtual-network/). Klik op App Service kunt u de [functie van de integratie VNET](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)waarmee uw Azure toepassingen om uit te voeren alsof ze op uw bedrijfsnetwerk.

### <a id="mobile"></a>Ik wil hosten een REST API of de webservice voor mobiele clients

HTTP gebaseerde webservices kunnen u een groot aantal clients, met inbegrip van mobiele clients ondersteunen. Kaders zoals ASP.NET Web API integreren met Visual Studio om eenvoudiger kunt maken en gebruiken van andere services.  Deze services zijn blootgesteld aan een web-eindpunt, zodat het gebruiken van een methode op Azure voor webhosting ter ondersteuning van dit scenario mogelijk is. App-Service is echter een uitstekende keus voor het hosten van REST API's. Met de App-Service, kunt u het volgende doen:

- Snel een [mobiele app](../app-service-mobile/app-service-mobile-value-prop.md) of [API-app](../app-service-api/app-service-api-apps-why-best-platform.md) voor het hosten van de HTTP-webservice in een van de Azure globaal verdeelde datacenters maken.
- Migreren van bestaande services of nieuwe bestanden maken.
- SLA bereiken voor beschikbaarheid met één exemplaar of schaal af op meer specifieke computers.
- De gepubliceerde site geven een HTTP-clients, met inbegrip van mobiele clients REST API's gebruiken.

> [AZURE.NOTE]
> Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een account, gaat u naar <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, waar u kunt direct een tijdelijk starter app maakt in Azure App Service gratis. Geen creditcard vereist, niet verplichtingen.

## <a id="nextsteps"></a>Volgende stappen

Voor meer informatie over de drie hostingprovider Webopties, raadpleegt u [Inleiding tot Azure](../fundamentals-introduction-to-azure.md).

Als u wilt beginnen met de optie (s) die u voor uw toepassing kiest, raadpleegt u de volgende bronnen:

* [Azure App-Service](/documentation/services/app-service/)
* [Azure Cloudservices](/documentation/services/cloud-services/)
* [Azure virtuele Machines](/documentation/services/virtual-machines/)
* [Service stof](/documentation/services/service-fabric)

<!-- URL List -->

[Azure App-Service]: /services/app-service/
[Cloudservices]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuele Machines]: http://go.microsoft.com/fwlink/?LinkID=306053
[Service stof]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Opslag]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
