<properties 
    pageTitle="Besturingssysteemfunctionaliteit op Azure App-Service" 
    description="Meer informatie over de functionaliteit voor OS die beschikbaar zijn voor de WebApps, mobiele app backends en API-apps op Azure App-Service" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Besturingssysteemfunctionaliteit op Azure App-Service #

In dit artikel worden de algemene basislijn besturingssysteemfunctionaliteit die beschikbaar is voor alle apps waarop [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714). Deze functionaliteit bevat bestand, netwerk, en toegang tot het register en diagnostische logboeken en gebeurtenissen. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>App-Service plannen lagen

App-Service wordt uitgevoerd klant-apps in een hostomgeving meerdere tenant. Apps die zijn geïmplementeerd in de lagen **gratis** en **gedeeld** uitvoeren in werknemer processen op gedeelde virtuele machines, terwijl de apps die zijn geïmplementeerd in de **Standard** en **Premium** lagen worden uitgevoerd op virtual machine(s) speciale specifiek voor de apps die is gekoppeld aan één klant.

Omdat App Service een schaal aantrekkelijker tussen verschillende niveaus ondersteunt, blijft de configuratie van de beveiliging afgedwongen voor App Service apps hetzelfde. Dit zorgt ervoor dat apps niet ineens zich anders gedragen, mislukt onverwachte resultaten bij het App serviceplan naar de andere overschakelt van één niveau.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Kaders ontwikkeling

App Service prijzen lagen bepalen welke berekeningscluster resources (CPU, schijfruimte geheugen en netwerk egress) beschikbaar is voor apps. De breedte van framework beschikbare functies voor apps blijft echter hetzelfde, ongeacht de schaal lagen.

App Service ondersteunt een groot aantal ontwikkeling kaders, inclusief ASP.NET, klassieke ASP, node.js, PHP en Python - die allemaal als extensies in IIS uitvoeren. Om te vereenvoudigen en te normaliseren beveiliging, uitvoeren App Service apps meestal de verschillende ontwikkeling kaders met de standaardinstellingen. Een methode voor het configureren van apps kan zijn de API oppervlakte en de functionaliteit voor elke afzonderlijke ontwikkeling framework aanpassen. Een algemene aanpak gaat App Service in plaats daarvan doordat een algemene basislijn van besturingssysteemfunctionaliteit ongeacht van een app ontwikkeling framework.

De volgende secties samenvatting maken van de algemene soorten besturingssysteem beschikbare functies voor App Service-apps.

<a id="FileAccess"></a>
##<a name="file-access"></a>Toegang tot bestanden

Verschillende stations bestaan binnen App-Service, inclusief lokale stations en netwerkstations.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Lokale stations

De centrale component App-Service is uitgevoerd op de infrastructuur van Azure PaaS (platform als een service). De lokale stations die zijn "gekoppeld" aan een virtuele machine zijn daardoor dezelfde stationstypen beschikbaar zijn voor elke werknemer rol uitgevoerd in Azure wordt aangegeven. Dit geldt ook voor een besturingssysteem-station (de D:\-station), een toepassing-station met Azure cspkg pakketbestanden gebruikte uitsluitend door het App-Service (en niet toegankelijk voor klanten) en een "gebruiker" station (de C:\), met een grootte afhankelijk van de grootte van de VM varieert.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Stations netwerk (of UNC deelt)

Een van de unieke aspecten van App-Service waarmee app-implementatie en onderhoud ingewikkelde is dat alle gebruikersinhoud is opgeslagen op een verzameling waarden voor aandelen UNC. Dit model zeer aangepast aan de algemene patroon van inhoud kunnen opslaan die worden gebruikt door omgevingen met meerdere taakverdeling servers voor webhosting on-premises implementatie toegewezen. 

Binnen App-Service, zijn er aantal UNC aandelen die zijn gemaakt in elke Datacenter. Een percentage van de gebruikersinhoud voor alle klanten in elke datacenter is toegewezen aan elk UNC-aandeel. Bovendien delen het bestand de inhoud van het abonnement van één klant altijd op dezelfde UNC is geplaatst. 

Houd rekening met het vanwege hoe cloud services voor werk, de specifieke virtuele machine die verantwoordelijk is voor het hosten van een UNC-share later stadium wordt gewijzigd. Er is gegarandeerd dat UNC aandelen wordt gekoppeld door verschillende virtuele machines terwijl ze omhoog en omlaag in de normale loop van cloud bewerkingen worden gebracht. Daarom moeten apps nooit vastgelegde hypothesen dat de computer informatie in een UNC-pad stabiele na verloop van tijd blijft ervoor. In plaats daarvan deze moeten gebruiken de handige *faux* absolute pad **D:\home\site** die App Service biedt. Dit faux absolute pad biedt een portable, app-en-gebruiker-agnostic methode voor het verwijzen naar eigen app. Met behulp van **D:\home\site**, kan een gedeelde bestanden overbrengen van app aan app zonder dat u moet een nieuw absolute pad configureren voor elke doorverbinden.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Soorten bestandstoegang verleend aan een app

Er is een gereserveerde mapstructuur van elke klant abonnement op een specifieke UNC-share binnen een datacenter. Een klant mogelijk meerdere apps die zijn gemaakt in een specifieke Datacenter, zodat alle van de mappen die deel uitmaken van een abonnement één klant zijn gemaakt op dezelfde UNC delen. De optie voor delen kan onder andere bestaan mappen zoals die voor inhoud, fout en diagnostische logboeken en eerdere versies van de app gemaakt door het besturingselement voor gegevensbronnen. Zoals verwacht, is van een klant app mappen zijn beschikbaar voor lees- en schrijftoegang tijdens runtime door code van de toepassing van de app.

Op de lokale vaste schijven die zijn bijgevoegd bij de virtuele machine die wordt uitgevoerd als een app, behoudt App Service een hoeveelheid ruimte op de schijf C:\ voor de app-specifieke tijdelijke lokale opslag. Hoewel een app volledige alleen-lezen/schrijven toegang tot een eigen tijdelijke lokale opslag heeft, is niet die opslagruimte bedoeld rechtstreeks door de toepassingscode worden gebruikt. In plaats daarvan is de bedoeling om te kunnen tijdelijk bestand opslaan IIS- en web application kaders. App Service beperkt ook de hoeveelheid tijdelijke lokale opslag beschikbaar voor elke app om te voorkomen dat afzonderlijke apps verbruikt overtollige hoeveelheden lokale bestandsopslag.

Twee voorbeelden van het gebruik van lokale tijdelijke in App Service zijn de map voor tijdelijke ASP.NET-bestanden en de adreslijst op IIS gecomprimeerde bestanden. Het ASP.NET compilatie-systeem wordt de map 'Tijdelijke ASP.NET-bestanden' als de locatie van een tijdelijke compilatie-cache. De "IIS gecomprimeerde map Tijdelijke bestanden" wordt gebruikt om op te slaan gecomprimeerde antwoord uitvoer IIS. Beide soorten bestand gebruik (evenals anderen) worden opnieuw toegewezen in de App Service aan lokale tijdelijke per app. Deze opnieuw toe te wijzen, zorgt ervoor dat de functionaliteit doorgaat zoals verwacht.

Elke app in de App Service wordt uitgevoerd als de identiteit van een willekeurig unieke weinig rechten werkproces genoemd de "identiteit groep van toepassingen', verder hier beschreven: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Toepassing wordt deze identiteit gebruikt voor eenvoudige alleen-lezen toegang tot het besturingssysteem station (het D:\-station). Dit betekent toepassingscode kunt een lijst met algemene mapstructuren en algemene bestanden op station besturingssysteem lezen. Hoewel dit een enigszins globaal niveau van access, de dezelfde mappen en bestanden toegankelijk zijn lijken als u inrichten wordt de rol van een werknemer in een Azure service gehost en de inhoud van het station lezen. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Toegang tot bestanden op meerdere exemplaren

De basismap van een app-inhoud bevat en toepassingscode naartoe kunt schrijven. Als een app wordt uitgevoerd op meerdere exemplaren, wordt de basismap worden gedeeld door alle instanties zodat alle exemplaren dezelfde map zien. Zo is, bijvoorbeeld als een app geüploade bestanden naar de basismap opslaat, de bestanden die zijn onmiddellijk beschikbaar voor alle exemplaren. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Netwerktoegang
Toepassingscode de beschikking over TCP/IP en UDP op basis van protocollen zodat uitgaande verbindingen met Internet toegankelijke eindpunten die worden van externe services netwerk. Apps kunnen deze dezelfde protocollen gebruiken om verbinding maken met services vanuit Azure & #151, bijvoorbeeld door HTTPS verbinding maken met SQL-Database.

Er is ook een beperkte mogelijkheid voor apps voor een lokale loopback-verbinding tot stand brengen en een app luisteren op die lokale loopback socket hebben. Deze functie is hoofdzakelijk apps die op de lokale loopback sockets als onderdeel van hun functionaliteit luisteren inschakelen. Houd er rekening mee dat elke app een verbinding "Privé" loopback ziet; App "A" kan niet luisteren met een lokale loopback socket tot stand gebracht door app "B".

Named pipes worden ook ondersteund als een communicatiemethode (IPC) om tussen verschillende processen die samen een app worden uitgevoerd. De module IIS-FastCGI is bijvoorbeeld afhankelijk op named pipes te coördineren van de afzonderlijke processen die PHP pagina's worden uitgevoerd.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Code worden uitgevoerd, processen en geheugen
Zoals hierboven is, wordt de apps uitgevoerd in weinig rechten werknemer processen de identiteit voor een willekeurige groep van toepassingen gebruiken. Toepassingscode heeft toegang tot de geheugenruimte die is gekoppeld aan het werkproces, evenals alle onderliggende processen die mogelijk worden geïnitieerd met CGI-processen of andere toepassingen. Echter één app geen toegang tot het geheugen of de gegevens van een andere app zelfs als er op dezelfde virtuele machine.

Apps kunnen worden uitgevoerd scripts of pagina's met ondersteunde web ontwikkeling kaders zijn geschreven. App-Service configureren niet alle instellingen framework naar meer beperkte modi. Bijvoorbeeld uitvoeren ASP.NET-apps waarop App Service in 'volledig' vertrouwen in plaats van een meer beperkte vertrouwen-modus. Web kaders, waaronder zowel klassieke ASP en ASP.NET, kunnen bellen in het proces COM-onderdelen (maar niet bij proces COM-onderdelen) zoals ADO (ActiveX Data Objects) die zijn geregistreerd al dan niet standaard op de Windows-besturingssysteem.

Apps kunnen brengen gang en willekeurige code uitvoeren. Het is toegestaan voor een app aan doet dingen zoals vermenigvuldigen een opdrachtshell of een PowerShell-script uitvoeren. Hoewel bij een willekeurige code en processen kunnen worden geïnitieerd vanaf een app, zijn uitvoerbaar programma's en scripts echter nog steeds beperkt tot machtigingen worden verleend aan de bovenliggende groep van toepassingen. Bijvoorbeeld: een app kunt brengen gang een uitvoerbaar bestand waarmee een uitgaande HTTP-gesprekken, maar dat dezelfde uitvoerbare losmaken van het IP-adres van een virtuele machine uit de NIC kan niet proberen Een oproep uitgaande netwerk is toegestaan om te weinig rechten code, maar u probeert te configureren, netwerkinstellingen op een virtuele machine beheerdersbevoegdheden vereist.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnostische logboeken en gebeurtenissen
Logboekgegevens is een extra Stel dat sommige apps proberen te openen. De soorten logboekgegevens die beschikbaar zijn voor de code die wordt uitgevoerd in de App Service diagnostische gegevens bevat en logboekgegevens gegenereerd door een app die ook gemakkelijk toegankelijk is voor de app. 

W3C-HTTP-logboeken gegenereerd door een actieve app zijn bijvoorbeeld beschikbaar zijn op een logboekmap in het netwerk delen locatie voor de app, of beschikbaar in blobopslag gemaakt als een klant W3C-logboekregistratie met storage heeft ingesteld. De laatste optie kunt grote hoeveelheden logboeken zonder de kans op pesterijen het overschrijden van het bestand opslaglimieten die is gekoppeld aan een netwerkshare worden verzameld.

In een soortgelijke vein realtime diagnostische informatie van .NET-apps kan ook worden geregistreerd gebruik van de .NET tracering en hulpprogramma's voor diagnose-infrastructuur met opties voor het schrijven van de trace-informatie aan van de app netwerkshare, of u kunt ook naar een opslaglocatie van blob.

Gebieden van diagnostische gegevens vastleggen en aanwijzen die niet beschikbaar voor apps zijn zijn Windows ETW gebeurtenissen en algemene gebeurtenislogboeken van Windows (bijvoorbeeld systeem, toepassingen en beveiliging gebeurtenislogboeken). Aangezien ETW doelcellen informatie zijn mogelijk zichtbaar alle computers (met het recht ACL's), zijn de lees- en schrijftoegang tot ETW gebeurtenissen worden geblokkeerd. Ontwikkelaars ervaart mogelijk dat API-oproepen te lezen en schrijven ETW gebeurtenissen en algemene gebeurtenislogboeken van Windows weergegeven om te werken, maar dat is omdat het App-Service is 'faking' de oproepen zodat ze worden weergegeven op voltooien. In feite de toepassingscode geen toegang heeft tot deze gebeurtenisgegevens.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Toegang tot het register
Apps alleen-lezen toegang hebt tot veel (maar niet voor alle) van het register van de virtuele machine ze op worden uitgevoerd. In Word Web App, betekent dit dat registersleutels die alleen-lezen toegang tot de lokale groep gebruikers toestaan zijn toegankelijk voor apps. Een gebied van het register die wordt momenteel niet ondersteund voor lees- of schrijftoegang is de HKEY\_huidige\_gebruikerscomponent.

Schrijftoegang tot het register is geblokkeerd, inclusief toegang tot registersleutels per gebruiker. Vanuit het oogpunt van de app schrijftoegang tot het register moet nooit worden gebruikt in de Azure-omgeving omdat apps kunnen (en voer) worden overgezet op verschillende virtuele computers. Het alleen permanente schrijfbare opslag die kan worden afhankelijk van door een app is de structuur van de per app-map met inhoud die is opgeslagen op de App Service UNC aandelen. 

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
