<properties
    pageTitle="Controleren en problemen met opslag diagnose stellen bij | Microsoft Azure"
    description="Functies gebruiken, zoals opslag analytics, logboekregistratie op clients en andere hulpprogramma's van derden identificeren, een diagnose stellen bij en problemen met Azure Storage."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Bewaken, diagnosticeren en problemen met Microsoft Azure Storage

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Overzicht

Diagnose en probleemoplossing van problemen in een gedistribueerde toepassing gehost in een cloud-omgeving is complexer dan in traditionele omgevingen. Toepassingen kunnen worden geïmplementeerd in een PaaS of IaaS-infrastructuur on-premises, op een mobiel apparaat of in een combinatie van deze. Meestal van uw toepassing netwerkverkeer mogelijk overdragen openbare en particuliere netwerken en meerdere opslagtechnologieën zoals Microsoft Azure opslag tabellen, BLOB's, wachtrijen mogelijk worden gebruikt in uw toepassing of bestanden naast andere gegevens worden opgeslagen bijvoorbeeld als relationele en vastleggen van databases.

Als u wilt beheren van dergelijke toepassingen succes moet u deze proactief te controleren en begrijpen hoe opsporen en oplossen van alle aspecten van deze en hun afhankelijke technologieën. Als een gebruiker van Azure Storage-services, moet u continu controleren van de services opslag van die uw toepassing voor onverwachte wijzigingen in gedrag (zoals langzamer dan gebruikelijke antwoord tijden) wordt gebruikt en gegevens vastleggen gebruiken om meer gedetailleerde gegevens te verzamelen en voor het analyseren van een probleem in diepteas. De informatie van de diagnostische hulpprogramma's die u krijgt van zowel controle en registratie helpen u om te bepalen de oorzaak van het probleem dat uw toepassing heeft voorgedaan. Vervolgens kunt u het probleem oplossen en bepalen van de juiste stappen die u ondernemen kunt om dit verhelpen. Azure opslag is een core Azure-service en vormt een belangrijk onderdeel van de meeste van oplossingen die klanten naar de Azure infrastructuur implementeren. Azure opslag bevat mogelijkheden om te vereenvoudigen, controle, diagnose en probleemoplossing van opslag problemen in de cloud-toepassingen.

> [AZURE.NOTE] Opslag-accounts met een type herhaling van Zone-redundante opslag (ZRS) beschikt niet over de aan de doelstellingen of de mogelijkheid van de logboekregistratie is ingeschakeld op dit moment. De bestanden Azure-Service biedt ook geen ondersteuning voor logboekregistratie op dit moment.

Zie voor een praktische handleiding voor end-to-end probleemoplossing in Azure Storage-toepassingen, [End-to-End problemen oplossen met Azure opslageenheden en logboekregistratie, AzCopy, en bericht Analyzer](storage-e2e-troubleshooting.md).

+ [Inleiding]
    + [Hoe deze handleiding is ingedeeld]
+ [Uw opslagservice controleren]
    + [Servicestatus bewaken]
    + [Cmdlets voor controle capaciteit]
    + [Beschikbaarheid controleren]
    + [Prestaties controleren]
+ [Diagnose van problemen opslag]
    + [Service servicestatusproblemen zijn]
    + [Prestatieproblemen]
    + [Oplossen van fouten]
    + [Opslag emulator problemen]
    + [Hulpmiddelen voor logboekregistratie van opslag]
    + [Hulpmiddelen voor logboekregistratie van netwerk gebruiken]
+ [End-to-end tracering]
    + [Logboekgegevens correleren]
    + [Verzoek om de client-ID]
    + [ID van server]
    + [Tijdstempels]
+ [Richtlijnen voor probleemoplossing]
    + [Aan de doelstellingen weergeven hoog AverageE2ELatency en lage AverageServerLatency]
    + [Aan de doelstellingen lage AverageE2ELatency en lage AverageServerLatency weergeven, maar de client voordoet hoge latentie]
    + [Aan de doelstellingen weergeven hoog AverageServerLatency]
    + [Ondervindt u onverwachte vertragingen in het bericht is bezorgd op een wachtrij]
    + [Aan de doelstellingen weergeven een toename in PercentThrottlingError]
    + [Aan de doelstellingen weergeven een toename in PercentTimeoutError]
    + [Aan de doelstellingen weergeven een toename in PercentNetworkError]
    + [De client ontvangt HTTP 403 (niet toegestaan) berichten]
    + [De client ontvangt HTTP 404 (niet gevonden) berichten]
    + [De client ontvangt HTTP 409 (Conflict) berichten]
    + [Aan de doelstellingen lage PercentSuccess weergeven of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]
    + [De doelstellingen van de capaciteit weergeven een onverwachte toename in gebruik van de capaciteit opslag]
    + [Ondervindt u onverwachte opnieuw opstarten van virtuele Machines die een groot aantal bijgevoegde VHD 's]
    + [Het probleem zich voordoet de emulator opslag voor ontwikkel- of gebruiken]
    + [Ondervindt u problemen bij het installeren van de Azure-SDK voor .NET]
    + [Hebt u een ander probleem met een storage-service]
    + [Problemen met Azure-bestanden met Windows en Linux](storage-troubleshoot-file-connection-problems.md)
+ [Bijlagen]
    + [Bijlage 1: Fiddler gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan]
    + [Bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]
    + [Bijlage 3: Microsoft bericht Analyzer gebruiken om vast te leggen netwerkverkeer]
    + [Bijlage 4: Gebruik van Excel voor het weergeven van de doelstellingen en gegevens vastleggen]
    + [Bijlage 5: Bewaken met toepassing inzichten voor Visual Studio teamservices]

## <a name="introduction"></a>Inleiding

Deze handleiding wordt uitgelegd hoe u functies zoals Azure opslag Analytics, u gerelateerde aan de clientzijde logboekregistratie in de bibliotheek van Azure opslag-Client en andere hulpprogramma's van derden identificeren en oplossen van Azure Storage diagnose stellen bij problemen.

![][1]

*Afbeelding 1 Monitoring, diagnostische gegevens en probleemoplossing*

Deze handleiding is bedoeld om te worden gelezen hoofdzakelijk door ontwikkelaars van onlineservices die gebruikmaken van Azure opslagservices en IT-professionals die verantwoordelijk is voor het beheren van deze onlineservices. De doelstellingen van deze handleiding volgen:

- Om u te helpen voor het behoud van de servicestatus en prestaties van uw Azure Storage-accounts.
- Om u te geven met de benodigde processen en hulpprogramma's kunt u bepalen als een kwestie of een probleem in een toepassing zich tot Azure Storage verhoudt.
- Zodat u sneller richtlijnen voor het oplossen van problemen met Azure Storage.

### <a name="how-this-guide-is-organized"></a>Hoe deze handleiding is ingedeeld

Het gedeelte "[uw opslagservice Monitoring]" wordt beschreven hoe de servicestatus en de prestaties van uw Azure Storage-services met Azure Analytics opslageenheden (metrische opslaggegevens) controleren.

De sectie "[diagnose opslag problemen]" beschreven hoe u een diagnose stellen bij problemen met Azure opslag Analytics logboekregistratie (opslag logboekregistratie). Hierin wordt ook het inschakelen van logboekregistratie op clients met behulp van de voorzieningen in een van de clientbibliotheken zoals de Library opslag-Client voor .NET of de SDK Azure for Java.

Het gedeelte "[End-to-end tracering]" wordt beschreven hoe u de gegevens in de verschillende logboekbestanden en gegevens van de doelstellingen kan relateren.

De sectie "[Probleemoplossing richtlijnen]" bevat voor probleemoplossing voor sommige van de algemene opslag-gerelateerde problemen die kunnen optreden.

De "[bijlagen]' bevatten informatie over het gebruik van andere hulpprogramma's zoals Wireshark en Netmon voor het pakket netwerkgegevens, Fiddler voor het analyseren van HTTP/HTTPS-berichten en Microsoft bericht Analyzer voor het correleren logboekgegevens analyseren.


## <a name="monitoring-your-storage-service"></a>Uw opslagservice controleren

Als u bekend met Windows prestatiecontroles bent, kunt u als een equivalent Azure opslag van Windows prestaties Monitor items metrische opslaggegevens zien. In metrische opslaggegevens vindt u een uitgebreide set van de doelstellingen (items in Windows prestaties Monitor terminologie) zoals beschikbaarheid van een service totale aantal aanvragen voor service of percentage voltooide aanvragen bij service. Zie voor een volledige lijst van de doelstellingen van de beschikbare [Opslagruimte Analytics aan de doelstellingen tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx). U kunt opgeven of u wilt dat de storage-service voor het verzamelen en aan de doelstellingen per uur of elke minuut aggregeren. Zie voor meer informatie over het inschakelen van de doelstellingen en controleer uw accounts opslag [inschakelen metrische opslaggegevens en weergeven van gegevens aan de doelstellingen](http://go.microsoft.com/fwlink/?LinkId=510865).

U kunt kiezen welke per uur parameters die u wilt weergeven in de [Portal van Azure](https://portal.azure.com) en configureren van regels die beheerders met een melding wanneer een per uur metrisch groter is dan een bepaalde drempel via e-mail verzenden. Zie [Meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)voor meer informatie. 

De storage-service verzamelt metingen met een aanbevolen inspanning, maar mogelijk niet opnemen voor elke opslagbewerking.

U kunt aan de doelstellingen zoals beschikbaarheid, totaal aantal aanvragen en gemiddelde latentienummers voor een account opslagruimte weergeven in de Portal Azure. Een regel melding heeft ook instellen om u te waarschuwen beheerder als beschikbaarheid onder een bepaald niveau. Deze gegevens bekijken, een mogelijke gebied voor onderzoek is de tabel service succespercentage wordt dan 100% (Zie voor meer informatie de sectie '[lage PercentSuccess zien of analytics logboekvermeldingen bewerkingen hebben met de status ClientOtherErrors]').

U moet uw Azure-toepassingen om ervoor te zorgen dat ze zijn in orde en uitvoeren zoals verwacht door continu controleren:

- Tot stand brengen van de doelstellingen van enkele volgens de basislijn voor de toepassing waarmee u kunt huidige gegevens vergelijken en alle belangrijke wijzigingen in het gedrag van Azure opslag en uw toepassing identificeren. De waarden van de doelstellingen van de basislijn, in veel gevallen worden specifieke toepassing en moet u deze instellen wanneer u prestaties van uw toepassing te testen.
- Minuut aan de doelstellingen opnemen en gebruiken van deze om te controleren actief onverwachte fouten en afwijkingen zoals pieken fout telt of tarieven aanvragen.
- Ieder uur aan de doelstellingen opnemen en gebruiken van deze bovengemiddelde waarden zoals controleren fout telt gemiddelde en tarieven aanvragen.
- Wordt onderzocht mogelijke problemen met diagnostische hulpprogramma's zoals verderop in de sectie "[diagnose opslag problemen]."

De grafieken in afbeelding 3 onderstaande illustreren hoe de gemiddelde die per uur aan de doelstellingen optreedt kunt verbergen pieken in activiteit. De doelstellingen van de per uur weergegeven om weer te geven van een constante frequentie van aanvragen, terwijl de minuut aan de doelstellingen geven de schommelingen die zijn echt plaatsvindt.

![][3]

De rest van deze sectie wordt beschreven wat u moet controleren aan de doelstellingen en waarom.

### <a name="monitoring-service-health"></a>Servicestatus bewaken

De status van de Storage-service (en andere Azure services) weergeven in alle Azure regio's overal ter wereld kunt u de [Azure-Portal](https://portal.azure.com) . Hiermee kunt u zien onmiddellijk als een probleem buiten uw besturingselement is dat dit gevolgen heeft de Storage-service in de regio die u voor uw toepassing gebruikt.

Meldingen van incidenten die betrekking hebben op de verschillende services van Azure kan ook worden verstrekt door de [Azure-Portal](https://portal.azure.com) .
Opmerking: Deze informatie eerder beschikbaar was is, samen met historische gegevens, op het [Dashboard van Azure-Service](http://status.azure.com).

Terwijl de [Portal van Azure](https://portal.azure.com) verzamelt gegevens van de servicestatus van binnen de Azure datacenters (binnenkant-out monitoring), kunt u ook overwegen heeft een benadering buitenkant in om te genereren synthetische transacties die regelmatig toegang uw Azure gehoste webtoepassing van meerdere locaties tot. De services van [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) en toepassing inzichten voor Visual Studio Team Services zijn voorbeelden van deze methode buitenkant in. Zie voor meer informatie over de toepassing inzichten voor Visual Studio Team Services, de bijlage "[bijlage 5: bewaken met toepassing inzichten voor Visual Studio teamservices](#appendix-5)."

### <a name="monitoring-capacity"></a>Cmdlets voor controle capaciteit

Metrische opslaggegevens capaciteit aan de doelstellingen voor de service blob alleen worden opgeslagen omdat BLOB's meestal verwerking van het grootste deel van de opgeslagen gegevens (op het moment van schrijven, is het niet mogelijk te metrische opslaggegevens gebruiken om te controleren van de capaciteit van tabellen en wachtrijen). U vindt deze gegevens in de tabel **$MetricsCapacityBlob** als u hebt ingeschakeld voor de service Blob cmdlets voor controle. Metrische opslaggegevens records deze gegevens in één keer per dag en u kunt de waarde van de **RowKey** gebruiken om te bepalen of de rij een entiteit die betrekking op gebruikersgegevens (waarde **gegevens**) of analytics-gegevens (waarde **analytics hebben**) bevat. Elke opgeslagen entiteit bevat informatie over de hoeveelheid opslagruimte gebruikt (**capaciteit** gemeten in bytes) en het huidige aantal containers (**ContainerCount**) en BLOB's (**ObjectCount**) wordt gebruikt in de opslagruimte-account. Zie voor meer informatie over de doelstellingen van de capaciteit die zijn opgeslagen in de tabel **$MetricsCapacityBlob** [Opslag Analytics aan de doelstellingen tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [AZURE.NOTE] U moet deze waarden voor een vroegtijdige waarschuwing dat u de grenzen van de capaciteit van uw account opslag nadert controleren. U kunt in de Portal Azure waarschuwingsregels om u te waarschuwen als statistische opslag gebruik hoger of lager onder die u opgeeft toevoegen.

Zie het blogbericht [Wat Azure opslag facturering – bandbreedte, transacties, en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)voor hulp bij het schatten van de grootte van verschillende opslagobjecten zoals BLOB's.

### <a name="monitoring-availability"></a>Beschikbaarheid controleren

Moet u de beschikbaarheid van de services opslagruimte in uw account opslag controleren door controleren of de waarde in de kolom **beschikbaar** in de tabellen per uur of minuut aan de doelstellingen, **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. De **beschikbaarheid van de** kolom bevat een percentagewaarde die aangeeft van de beschikbaarheid van de service of de bewerking API van de rij (de **RowKey** ziet als de rij aan de doelstellingen voor de service als geheel of voor een specifieke API-bewerking bevat).

Een waarde lager dan 100% wordt aangegeven dat bepaalde aanvragen opslag worden verbroken. U kunt zien waarom ze worden verbroken door te bekijken van de andere kolommen in de gegevens aan de doelstellingen die de getallen van aanvragen met verschillende fouttypen zoals **ServerTimeoutError**weergeven. U kunt verwachten om **beschikbaarheid** vallen tijdelijk dan 100% om redenen zoals tijdelijke servertime terwijl de service partities naar betere taakverdeling verzoek verplaatst; weer te geven de logica opnieuw in de clienttoepassing moet die door onregelmatige voorwaarden verwerken. [Opslag Analytics geregistreerde bewerkingen en statusberichten](http://msdn.microsoft.com/library/azure/hh343260.aspx) dit artikel worden de typen die metrische opslaggegevens in de berekening van de **beschikbaarheid van bevat** .

U kunt in de [Portal van Azure](https://portal.azure.com)waarschuwingsregels melding krijgt als de **beschikbaarheid** van een service kleiner is dan een drempelwaarde die u opgeeft toevoegen.

De sectie "[Probleemoplossing richtlijnen]" van deze handleiding worden enkele veelvoorkomende opslag serviceproblemen met beschikbaarheid.

### <a name="monitoring-performance"></a>Prestaties controleren

Als u wilt controleren van de prestaties van de storage-services, kunt u het volgende aan de doelstellingen van de tabellen per uur en minuut aan de doelstellingen.

- De waarden in de kolommen **AverageE2ELatency** en **AverageServerLatency** weergeven de gemiddelde tijd de storage-service of API bewerkingstype duurt het proces aanvragen. **AverageE2ELatency** is een maateenheid voor end-to-end-latentie waarin de tijd die het verzoek lezen en verzenden van het antwoord naast de tijd verwerking van de aanvraag (dus inclusief netwerklatentie zodra het verzoek de storage-service bereikt); **AverageServerLatency** is een maateenheid voor alleen de verwerkingstijd en kunnen daarom sluit een netwerklatentie die betrekking hebben op de client communiceert. Zie de sectie '[zien hoog AverageE2ELatency en lage AverageServerLatency]' verderop in deze handleiding voor een discussie van waarom er mogelijk een aanzienlijk verschil tussen deze twee waarden.
- De waarden in de kolommen **TotalIngress** en **TotalEgress** weergeven de totale hoeveelheid gegevens, in bytes eraan- en afmelden bij uw storage-service of via een specifiek type van de API-bewerking gaan.
- De waarden in de kolom **TotalRequests** geven het totale aantal aanvragen die de storage-service van API bewerking ontvangt. **TotalRequests** is het totale aantal aanvragen die de storage-service ontvangt.

Meestal controleert u voor onverwachte wijzigingen in een van deze waarden als een aanduiding dat er een probleem dat is vereist onderzoek.

U kunt in de [Portal van Azure](https://portal.azure.com)waarschuwingsregels om u te waarschuwen als een van de prestatiegegevens voor deze service-herfst onder toevoegen of een bepaalde drempel die u opgeeft overschrijden.

De sectie "[Probleemoplossing richtlijnen]" van deze handleiding worden enkele veelvoorkomende problemen beschreven voor opslag-service betrekking hebben op prestaties.


## <a name="diagnosing-storage-issues"></a>Diagnose van problemen opslag

Er zijn een aantal manieren dat u op de hoogte van een probleem of het probleem in uw toepassing worden kan, het gaat hierbij:

- Een storing waardoor de toepassing vastloopt of niet meer werkt.
- Belangrijke wijzigingen van de basislijnwaarden in de aan de doelstellingen u controleert zoals is beschreven in de vorige sectie "[uw opslagservice bewaken]."
- Rapporten van gebruikers van uw toepassing dat sommige bepaalde bewerking niet voltooien zoals verwacht of dat bepaalde functie niet werkt.
- Fouten die zijn gegenereerd binnen de toepassing die worden weergegeven in de logboekbestanden of via een andere methode voor de melding.

Meestal wordt met betrekking tot Azure storage-services in een van de vier algemene categorieën vallen:

- Uw toepassing heeft een prestatieprobleem, dit gemeld in voor uw gebruikers of zichtbaar gemaakt door te wijzigingen in de prestatiegegevens.
- Er is een probleem met de infrastructuur Azure-opslag in een of meer regio's.
- Uw toepassing wordt uitgevoerd als een fout, dit gemeld in voor uw gebruikers of zichtbaar worden gemaakt door een stijging van een van de fout aantal cijfers die u controleren.
- Tijdens het ontwikkelen en test u gebruikt mogelijk de emulator lokale opslag; u tegenkomen enkele zaken die verband specifiek aan gebruik van de opslagruimte emulator houden.

De volgende gedeelten worden de stappen die u moet volgen opsporen en oplossen van problemen in elk van deze vier categorieën. De sectie '[Probleemoplossing richtlijnen]' verderop in deze handleiding vindt u meer details voor enkele veelvoorkomende problemen die kunnen optreden.

### <a name="service-health-issues"></a>Service servicestatusproblemen zijn

Servicestatus serviceproblemen zijn meestal buiten het besturingselement. De [Azure-Portal](https://portal.azure.com) vindt u informatie over lopende problemen met Azure services, met inbegrip van opslag. Wanneer u niet voor leestoegang geografische-redundante opslag wanneer u uw opslagruimte-account hebt gemaakt, kan klikt u vervolgens in het geval van uw gegevens tijdelijk niet beschikbaar zijn in de primaire locatie, uw toepassing schakelen tijdelijk de alleen-lezen kopie op de secundaire locatie. Hiervoor moet uw toepassing kunnen zijn om te schakelen tussen het gebruik van de primaire en secundaire locaties en kunnen werken in een gereduceerde modus met alleen-lezen-gegevens. De Client van Azure-opslag-bibliotheken kunnen u een beleid opnieuw die kan worden gelezen van secundaire opslag voor het geval van primaire opslag is gelezen mislukt definiëren. Uw toepassing moet ook Houd er rekening mee dat de gegevens in de secundaire locatie uiteindelijk consistent is. Zie het blogbericht [Azure opslag redundantieopties en leestoegang geografische redundante opslag](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)voor meer informatie.

### <a name="performance-issues"></a>Prestatieproblemen

De prestaties van een toepassing kan zijn subjectieve, met name vanuit het perspectief van een gebruiker. Daarom belang dat u over de doelstellingen van de basislijn is beschikbaar waarmee u bepalen wanneer er mogelijk een prestatieprobleem leiden. Veel factoren kunnen van invloed zijn op de prestaties van een Azure storage-service vanuit het perspectief van de toepassing client. Deze factoren kunnen werken in de storage-service, in de client of in de netwerkinfrastructuur; het is dus belangrijk aan een strategie voor de oorsprong van het prestatieprobleem identificeren.

Nadat u hebt aangegeven dat de waarschijnlijk locatie van de oorzaak van het prestatieprobleem van de doelstellingen, kunt u de logboekbestanden vervolgens gebruiken om gedetailleerde informatie kunnen vaststellen en oplossen van het probleem verder te zoeken.

De sectie '[Probleemoplossing richtlijnen]' verderop in deze handleiding vindt u meer informatie over algemene prestaties gerelateerd u problemen ondervinden.

### <a name="diagnosing-errors"></a>Oplossen van fouten

Gebruikers van uw toepassing mogelijk een melding van fouten die worden verzonden door de clienttoepassing. Metrische opslaggegevens wordt ook verschillende fouttypen uit uw opslagruimte services zoals **NetworkError**, **ClientTimeoutError**of **AuthorizationError**aantallen records. Terwijl metrische opslaggegevens records alleen aantallen verschillende fouttypen, kunt u meer details over afzonderlijke aanvragen hand servers, aan de clientzijde en netwerk Logboeken. Meestal krijgt de HTTP-statuscode die het resultaat van de storage-service een van de waarom de aanvraag is mislukt.

> [AZURE.NOTE] Houd er rekening mee dat u om te zien sommige door onregelmatige fouten kan verwachten: bijvoorbeeld fouten vanwege tijdelijke netwerkproblemen of toepassingsfouten.

De volgende bronnen zijn handig voor informatie over de opslag-gerelateerde status en foutcodes:

- [Algemene REST API-foutcodes](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [Foutcodes BLOB-Service](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Foutcodes wachtrij-Service](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Tabel Service-foutcodes](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Foutcodes met Service-bestand](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Opslag emulator problemen

De SDK Azure bevat een opslag emulator die u op een ontwikkelwerkstation uitvoeren kunt. Deze emulator simuleert grootste deel van het gedrag van de Azure storage-services en is handig tijdens het ontwikkelen en testen, zodat u kunt de toepassingen die gebruikmaken van Azure opslagservices zonder dat u een Azure-abonnement en een account Azure opslag uitvoeren.

De sectie "[Probleemoplossing richtlijnen]" van deze handleiding worden enkele veelvoorkomende problemen aangetroffen aan de hand van de emulator opslag.

### <a name="storage-logging-tools"></a>Hulpmiddelen voor logboekregistratie van opslag

Logboekregistratie van opslagruimte biedt logboekregistratie op servers van opslagverzoeken in uw account Azure opslag. Zie voor meer informatie over het inschakelen van logboekregistratie op servers en toegang tot de logboekgegevens [vastleggen van opslag inschakelen en logboekgegevens openen](http://go.microsoft.com/fwlink/?LinkId=510867).

De bibliotheek van de Client opslag voor .NET kunt u voor het verzamelen van logboekgegevens aan de clientzijde die betrekking op opslagbewerkingen wordt uitgevoerd door uw toepassing hebben. Zie [logboekregistratie aan de clientzijde met de .NET opslag Client-bibliotheek](http://go.microsoft.com/fwlink/?LinkId=510868)voor meer informatie.

> [AZURE.NOTE] In sommige gevallen (zoals SA's autorisatie fouten), kan een gebruiker een fout waarvoor u geen aanvraaggegevens in de logboeken aan de serverzijde opslag vinden kunt melden. U kunt de mogelijkheden van de logboekregistratie van de bibliotheek van de Client opslag gebruiken om te kunnen onderzoeken als de oorzaak van het probleem op de client of network hulpprogramma's voor controle gebruiken om te kunnen onderzoeken het netwerk.

### <a name="using-network-logging-tools"></a>Hulpmiddelen voor logboekregistratie van netwerk gebruiken

U kunt het verkeer tussen de client en server gedetailleerde informatie over de gegevens die de client en server uitwisselt en de onderliggende netwerk voorwaarden opgeven vastleggen. Handige netwerkhulpprogramma logboekregistratie's bevatten:

- [Fiddler](http://www.telerik.com/fiddler) is een gratis web foutopsporing proxy waarmee u de kop- en nettolading van HTTP en HTTPS vergaderverzoeken en antwoorden berichten bekijken. Zie voor meer informatie [bijlage 1: Fiddler gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan](#appendix-1).
- [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) en [Wireshark](http://www.wireshark.org/) zijn gratis protocol netwerkanalysesystemen waarmee u kunt het pakket voor gedetailleerde informatie voor een groot aantal netwerkprotocollen weergeven. Zie voor meer informatie over Wireshark, "[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer](#appendix-2)".
- Microsoft bericht Analyzer is een functie van Microsoft die vervangt Netmon en die naast het pakket netwerkgegevens, helpt u bij het weergeven en analyseren de logboekgegevens van andere hulpprogramma's vastgelegd. Zie voor meer informatie "[bijlage 3: Microsoft bericht Analyzer gebruiken om vast te leggen netwerkverkeer](#appendix-3)".
- Als u uitvoeren van een eenvoudige connectivity-toets om te controleren wilt dat uw clientcomputer verbinding met de Azure storage-service via het netwerk maken kunt, doen u dit niet met het hulpprogramma standaard **ping** op de client. U kunt echter het [ **tcping** hulpmiddel](http://www.elifulkerson.com/projects/tcping.php) connectivity controleren.

In veel gevallen wordt de logboekgegevens van opslag registratie en de bibliotheek van de Client opslag voldoende naar een diagnose stellen bij een probleem, maar in sommige gevallen moet u mogelijk de meer gedetailleerde informatie dat deze logboekregistratie netwerkhulpprogramma's kunnen leveren. Bijvoorbeeld, kunt met Fiddler weergeven van HTTP en HTTPS-berichten u kop- en nettolading gegevens verzonden naar en vanuit de storage-services, waardoor u bekijken hoe een clienttoepassing opslagbewerkingen pogingen moeten worden weergegeven. Protocol analyzers zoals Wireshark werken op het pakketniveau van het zodat u TCP-gegevens, waarmee u problemen met verloren pakketten en verbindingsproblemen zou moeten worden weergegeven. Bericht Analyzer kunt werken met HTTP- en TCP lagen.

## <a name="end-to-end-tracing"></a>End-to-end tracering

End-to-end tracering met een verscheidenheid aan logboekbestanden is een handige techniek voor wordt onderzocht potentiële problemen. U kunt de datum/tijd-informatie uit uw gegevens aan de doelstellingen gebruiken als een opgave waar te bekijken in de logboekbestanden voor gedetailleerde informatie waarmee u kunt het probleem oplossen.

### <a name="correlating-log-data"></a>Logboekgegevens correleren

Bij het weergeven van Logboeken vanuit clienttoepassingen netwerk wordt getraceerd en aan de clientzijde opslag logboekregistratie is het belangrijk om te relateren kunnen vraagt over de verschillende logboekbestanden. De logboekbestanden zijn een aantal verschillende velden die handig als correlatie-id zijn. Het verzoek om de client-id is de nuttigste velden kunt gebruiken om te relateren van vermeldingen in de logboeken aan de andere. Maar soms kan het handig zijn de server aanvraag-id of tijdstempels gebruiken. De volgende secties vindt meer informatie over deze opties.

### <a name="client-request-id"></a>Verzoek om de client-ID

De bibliotheek van de Client opslag genereert automatisch een unieke client aanvraag-id voor elke aanvraag.

- In het logboek aan de clientzijde die de bibliotheek van de Client opslag wordt gemaakt, wordt het verzoek om de client-id weergegeven in het **Verzoek om de Client-ID** -veld van elke vermelding met betrekking tot het verzoek.
- In een netwerktracering zoals een vastgelegd door Fiddler, is de id van de aanvraag client zichtbaar in request-berichten als de waarde van de **x-ms-client-aanvraag-id** HTTP-header.
- In het logboek serverzijde opslag logboekregistratie wordt het verzoek om de client-id weergegeven in de kolom ID van Client-verzoek.

> [AZURE.NOTE]Het is mogelijk voor meerdere aanvragen voor het delen van dezelfde client aanvraag-id, omdat de client deze waarde toewijzen kan (Hoewel de bibliotheek van de Client opslag wordt automatisch een nieuwe waarde toegewezen). Alle pogingen delen als nieuwe pogingen van de client dezelfde client aanvraag-id. Wanneer een partij die wordt verzonden vanuit de client, heeft de batch id van één client.


### <a name="server-request-id"></a>ID van server

De storage-service genereert automatisch server verzoek-id's.

- De aanvraag-id weergegeven in het logboek serverzijde opslag logboekregistratie de **kop van de aanvraag-ID** -kolom.
- In een netwerktracering zoals een vastgelegd door Fiddler, wordt de aanvraag-id weergegeven in antwoordberichten als de waarde van de **x-ms-aanvraag-id** HTTP-header.
- In het logboek aan de clientzijde die de bibliotheek van de Client opslag wordt gemaakt, wordt de aanvraag-id weergegeven in de kolom **Bewerking tekst** voor de vermelding met details van de serverreactie.

> [AZURE.NOTE] De storage-service altijd een unieke server verzoek id aan toegewezen elk verzoek om die deze ontvangt, zodat elke nieuwe poging van de client en elke bewerking opgenomen in een batch een unieke server aanvraag-id heeft.

Als de bibliotheek van de Client opslag een **StorageException** in de client genereert, bevat de eigenschap **RequestInformation** een **RequestResult** -object met een eigenschap **ServiceRequestID** . U kunt ook een object **RequestResult** openen vanuit een exemplaar **OperationContext** .

In het onderstaande voorbeeld wordt gedemonstreerd hoe u een aangepaste **ClientRequestId** -waarde instellen door het koppelen van een object **OperationContext** het verzoek aan de storage-service. Ook ziet u hoe u de waarde **ServerRequestId** uit het antwoordbericht ophalen.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Tijdstempels

U kunt ook tijdstempels Zoek gerelateerde logboekvermeldingen, maar pas op dat u van de scheefheid van een klok tussen de client en server die zich kan voordoen. U moet zoeken plus of min 15 minuten voor overeenkomende serverzijde posten op basis van de tijdstempel op de client. Houd er rekening mee dat de blob-metagegevens voor de BLOB's aan de doelstellingen met het tijdsbereik de doelstellingen die zijn opgeslagen in de blob; Dit is handig als u veel BLOB's van de doelstellingen voor dezelfde minuut of uur.

## <a name="troubleshooting-guidance"></a>Richtlijnen voor probleemoplossing

In dit gedeelte vindt u informatie met de diagnose en probleemoplossing van enkele veelvoorkomende problemen uw toepassing mogelijk optreden bij het gebruik van de Azure storage-services. Gebruik de onderstaande lijst om de gegevens die relevant zijn voor uw specifieke probleem te zoeken.

**Probleemoplossing beslissingsstructuur**

----------

Het probleem zich verhouden tot de prestaties van een van de services opslagruimte?

- [Aan de doelstellingen weergeven hoog AverageE2ELatency en lage AverageServerLatency]
- [Aan de doelstellingen lage AverageE2ELatency en lage AverageServerLatency weergeven, maar de client voordoet hoge latentie]
- [Aan de doelstellingen weergeven hoog AverageServerLatency]
- [Ondervindt u onverwachte vertragingen in het bericht is bezorgd op een wachtrij]

----------

Het probleem zich verhouden tot de beschikbaarheid van een van de services opslagruimte?

- [Aan de doelstellingen weergeven een toename in PercentThrottlingError]
- [Aan de doelstellingen weergeven een toename in PercentTimeoutError]
- [Aan de doelstellingen weergeven een toename in PercentNetworkError]

----------

De clienttoepassing ontvangt een antwoord wordt verzonden HTTP 4XX (zoals 404) van een storage-service?

- [De client ontvangt HTTP 403 (niet toegestaan) berichten]
- [De client ontvangt HTTP 404 (niet gevonden) berichten]
- [De client ontvangt HTTP 409 (Conflict) berichten]

----------

[Aan de doelstellingen lage PercentSuccess weergeven of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]

----------

[De doelstellingen van de capaciteit weergeven een onverwachte toename in gebruik van de capaciteit opslag]

----------

[Ondervindt u onverwachte opnieuw opstarten van virtuele Machines die een groot aantal bijgevoegde VHD 's]

----------

[Het probleem zich voordoet de emulator opslag voor ontwikkel- of gebruiken]

----------

[Ondervindt u problemen bij het installeren van de Azure-SDK voor .NET]

----------

[Hebt u een ander probleem met een storage-service]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Aan de doelstellingen weergeven hoog AverageE2ELatency en lage AverageServerLatency

De afbeelding onder vanaf de [Portal van Azure](https://portal.azure.com) hulpmiddel cmdlets voor controle wordt een voorbeeld waar de **AverageE2ELatency** aanmerkelijk hoger zijn dan de **AverageServerLatency**is.

![][4]

Houd er rekening mee dat de storage-service alleen de meetwaarde **AverageE2ELatency** voor succesvolle aanvragen berekent en, in tegenstelling tot **AverageServerLatency**, bevat het duurt voordat de client de gegevens verzenden en ontvangen van bevestiging van de storage-service. Een verschil tussen **AverageE2ELatency** en **AverageServerLatency** kan dus vanwege de clienttoepassing wordt traag reageren of vanwege voorwaarden voldoet in het netwerk.

> [AZURE.NOTE] U kunt ook **E2ELatency** en **ServerLatency** weergeven voor afzonderlijke opslagbewerkingen in de logboekgegevens opslag vastleggen.

#### <a name="investigating-client-performance-issues"></a>Wordt onderzocht client prestatieproblemen

Mogelijke oorzaken voor de client reageert traag zijn met een beperkt aantal beschikbare verbindingen of threads of wordt weinig bronnen zoals CPU, geheugen of netwerk bandbreedte. Het is mogelijk dat u kunt het probleem oplossen door het wijzigen van de clientcode als u wilt efficiënter (bijvoorbeeld via asynchrone oproepen naar de storage-service), of door een grotere virtuele Machine (met meer cores en meer geheugen).

Voor de tabel en wachtrij-services, de algoritme van de Nagle kunt ook de oorzaak van hoge **AverageE2ELatency** ten opzichte van **AverageServerLatency**: Zie het bericht [van Nagle algoritme is niet geschikt kleine aanvragen](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx)voor meer informatie. U kunt de algoritme van de Nagle in code uitschakelen door de klas **ServicePointManager** in de naamruimte **System.Net** . U moet dit doen voordat u oproepen aan de tabel of wachtrijservices in uw toepassing aangezien heeft dit geen invloed op verbindingen die al zijn openen. Het volgende voorbeeld afkomstig zijn uit de methode **Application_Start** in een rol werknemer.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

De logboeken aan de clientzijde om te zien hoeveel aanvragen dat de clienttoepassing verzendt en controleren op algemene .NET gerelateerd prestaties problemen in uw mailclient, bijvoorbeeld CPU, .NET opschonen, netwerkgebruik of geheugen, moet u controleren. Zie als uitgangspunt voor het oplossen van .NET-clienttoepassingen, [Foutopsporing, aanwijzen, en profielen](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Wordt onderzocht latentie netwerkproblemen

Hoge end-to-end latentie die worden veroorzaakt door het netwerk is meestal vanwege tijdelijke situaties. U kunt beide tijdelijke en permanente netwerkproblemen zoals verloren pakketten onderzoek met behulp van hulpprogramma's zoals Wireshark of Microsoft bericht Analyzer.

Zie voor meer informatie over het gebruik van Wireshark problemen "[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]."

Zie voor meer informatie over het gebruik van Microsoft bericht Analyzer problemen "[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Aan de doelstellingen lage AverageE2ELatency en lage AverageServerLatency weergeven, maar de client voordoet hoge latentie

In dit scenario is dit meestal omdat een vertraging in de opslagruimte aanvragen kunnen bereiken de storage-service. U moet onderzoek waarom aanmeldingsaanvragen van de client niet stapt tot en met de blob-service.

Een mogelijke reden voor de client vertragen verzenden van aanvragen is dat er een beperkt aantal beschikbare verbindingen of threads.

U moet ook controleren of de client meerdere pogingen wordt uitgevoerd en de reden onderzoeken als dit het geval. Om te bepalen of de client meerdere pogingen wordt uitgevoerd, kunt u het volgende doen:

- Bekijk de logboekbestanden opslag Analytics. Als u meerdere pogingen zijn voordoen, ziet u meerdere bewerkingen met dezelfde client verzoek ID, maar met andere server verzoek om id's.
- Bekijk de logboekbestanden van de client. Uitgebreide logboekregistratie wordt aangegeven dat er een nieuwe poging is opgetreden.
- Fouten opsporen in uw code en controleert u de eigenschappen van het **OperationContext** -object dat is gekoppeld aan het verzoek. Als de bewerking opnieuw heeft gestart, bevat de eigenschap **RequestResults** meerdere verzoek om de unieke id's. U kunt ook de begin- en eindtijden voor elke aanvraag controleren. Zie voor meer informatie in het voorbeeld in de sectie [Server aanvraag-ID].

Als er geen problemen in de client, moet u mogelijke netwerkproblemen zoals pakketverlies onderzoek. U kunt hulpprogramma's zoals Wireshark of Microsoft bericht Analyzer gebruiken om te kunnen onderzoeken netwerkproblemen.

Zie voor meer informatie over het gebruik van Wireshark problemen "[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]."

Zie voor meer informatie over het gebruik van Microsoft bericht Analyzer problemen "[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]."

### <a name="metrics-show-high-AverageServerLatency"></a>Aan de doelstellingen weergeven hoog AverageServerLatency

In het geval van hoge **AverageServerLatency** voor aanvragen voor het downloaden van blob, moet u de logboeken opslag logboekregistratie om te kijken of er herhaalde verzoeken om de dezelfde blob (of reeks BLOB's). Voor blob uploaden aanvragen, moet u onderzoeken welke blok grootte van de client gebruikt (bijvoorbeeld geblokkeerd kleiner dan 64 kB kan resulteren in algemene tenzij gelezen, wordt ook in minder dan 64 kB stukken), en als meerdere klanten blokken naar de dezelfde blob parallel uploaden wilt. Controleer de doelstellingen van de per minuut pieken in het aantal aanvragen die in meer dan resulteren ook de per tweede schaalbaarheid doelen: Zie ook "[zien een stijging PercentTimeoutError]."

Als u hoog **AverageServerLatency** voor blob aanvragen downloaden wanneer er herhaalde aanvragen dezelfde blob of set BLOB's zijn ziet, klikt u rekening moet houden caching van deze BLOB's via Azure Cache of de Azure inhoud bezorging netwerk (CDN). Voor upload aanvragen, kunt u de doorvoer verbeteren met behulp van een groter blokkeren. Voor query's aan tabellen is het ook mogelijk om te implementeren aan de clientzijde caching op clients die de dezelfde querybewerkingen uitvoeren en waar de gegevens niet vaak.

Hoge **AverageServerLatency** waarden zijn ook een symptoom van slecht ontworpen tabellen of query's dat resultaat in scanbewerkingen of die volgt u de toevoegquery/Voeg tegen patroon. Zie '[zien een stijging PercentThrottlingError]' voor meer informatie.

> [AZURE.NOTE] U kunt een volledig controlelijst prestaties controlelijst hier vinden: [Microsoft Azure opslag prestaties en schaalbaarheid controlelijst](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Ondervindt u onverwachte vertragingen in het bericht is bezorgd op een wachtrij

Als u een vertraging op tussen de tijd waarop die een toepassing een bericht naar een wachtrij wordt toegevoegd en de tijd die deze beschikbaar te lezen uit de wachtrij ondervindt, kunt u de volgende stappen uit om de oorzaak van het probleem te moet uitvoeren:

- Controleer of dat de berichten is wel het toevoegen van de toepassing naar de wachtrij. Controleer of de toepassing niet de methode **AddMessage** enkele malen voordat het volgende probeert. De bibliotheek voor opslag-Client-logboeken bevatten eventuele herhaalde pogingen van opslagbewerkingen.
- Controleer of er is geen klok scheefheid tussen de werknemer rol die aan het bericht wordt toegevoegd aan de wachtrij en de rol van de werknemer die leest het bericht in de wachtrij waarmee worden weergegeven als wanneer er treedt een vertraging in verwerking.
- Controleer als de rol van de werknemer die de berichten uit de wachtrij leest is verbroken. Als een client wachtrij de **GetMessage** aanroept, maar niet meer reageert met een bevestiging, blijft het bericht onzichtbare in de wachtrij totdat de **invisibilityTimeout** verstreken. Het bericht wordt nu beschikbaar voor het verwerken van opnieuw.
- Controleer als lengte van de wachtrij na verloop van tijd groeit. Dit kan gebeuren als er geen voldoende werknemers die beschikbaar zijn voor het verwerken van alle berichten die door andere werknemers zijn in de wachtrij plaatsen. U moet ook controleren het aan de doelstellingen om te zien als delete-aanvragen mislukte en het aantal wachtrij halen voor berichten die worden herhaald mislukte pogingen het bericht wilt verwijderen.
- Bekijk de logboekbestanden opslag logboekregistratie voor wachtrij bewerkingen die hoger zijn dan verwacht **E2ELatency** en **ServerLatency** waarden over een langere periode dan normaal beschikken.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Aan de doelstellingen weergeven een toename in PercentThrottlingError

Bandbreedteregeling fouten optreden wanneer u de doelen schaalbaarheid van een service opslagruimte overschrijdt. De storage-service doet dit om ervoor te zorgen dat geen enkele client of tenant de service kosten van anderen kunt gebruiken. Zie [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md) voor details over schaalbaarheid doelen voor opslag-accounts en doelstellingen die voor partities binnen opslag accounts voor meer informatie.

Als een grotere de meetwaarde **PercentThrottlingError** in het percentage van aanvragen die met een bandbreedteregeling fout zijn verbroken weergeven, moet u een van de twee scenario's onderzoeken:

- [Tijdelijke stijging in PercentThrottlingError]
- [Permanente stijging in PercentThrottlingError fout]

Een stijging **PercentThrottlingError** vaak voorkomt op hetzelfde moment als een stijging van het aantal opslagruimte aanvragen of wanneer u zijn in eerste instantie laad de toepassing te testen. Dit kan ook tot uiting komen in de client als "503 Server bezet" of "500 time-out voor de bewerking" HTTP statusberichten van opslagbewerkingen.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tijdelijke stijging in PercentThrottlingError

Als u pieken in de waarde van **PercentThrottlingError** die met het aantal termijnen van hoge activiteit voor de toepassing overeenkomen ziet, moet u een exponentiële (niet-lineair) terug uitschakelen strategie voor het nieuwe pogingen implementeren in uw client: Hiermee wordt de onmiddellijke belasting op het partition verkleinen en help uw toepassing geeft pieken in het verkeer. Zie voor meer informatie over het implementeren van beleidsregels voor opnieuw met behulp van de bibliotheek van de Client opslag [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] U ziet ook pieken in de waarde van **PercentThrottlingError** die niet met het aantal termijnen van hoge activiteit voor de toepassing overeenkomen: dit meestal omdat hier is de storage-service partities die u wilt het verbeteren van taakverdeling verplaatsen.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente stijging in PercentThrottlingError fout

Als u een consistente hoge waarde voor de volgende **PercentThrottlingError** ziet een permanente stijging in uw hoeveelheden transactie, of wanneer u uw eerste laden uitvoert op uw toepassing getest en bepaal hoe uw toepassing opslag partities worden gebruikt en of deze de schaalbaarheid doelen voor een account opslagruimte bijna is bereikt. Bijvoorbeeld als u fouten in op een wachtrij (dat wordt geteld als een eenmalige partition) beperken ziet, moet klikt u vervolgens u overwegen aanvullende wachtrijen naar de transacties verdeeld over meerdere partities. Als u fouten in op een tabel beperken ziet, moet u overwegen om een andere partities kleurenschema te uw transacties verspreid over meerdere partities met behulp van een grotere groep partition sleutelwaarden. Een veel voorkomende oorzaak van dit probleem is de prepend/toevoegen tegen patroon waar u de datum als de partitiesleutel selecteert en vervolgens alle gegevens op een bepaalde dag worden geschreven naar één partition: onder laden, kan dit resulteren in een schrijven vertraging veroorzaken. U moet Houd rekening met het ontwerp van een andere partities of beoordelen of blobopslag gebruikt mogelijk een betere oplossing. U moet ook controleren of de beperken grond pieken in uw verkeer optreedt en onderzoeken manieren van de demping van het patroon van aanvragen.

Als u uw transacties over meerdere partities verdelen, moet u nog steeds op de hoogte van de schaalbaarheid grenzen voor de opslag-account zijn. Bijvoorbeeld als u tien wachtrijen elke verwerking van het maximale aantal van 2000 1KB berichten per seconde gebruikt, bent u bij de algehele limiet van 20.000 berichten per seconde voor de opslag-account. Als u meer dan 20.000 entiteiten per seconde verwerken wilt, moet u overwegen meerdere opslag-accounts. U tevens rekening mee dat de grootte van uw vergaderverzoeken en entiteiten invloed is op wanneer de storage-service uw klanten bandbreedte: als u grotere vergaderverzoeken en entiteiten hebt, u sneller kan worden vertraagd.

Niet efficiënt Queryontwerp kan ook leiden tot u te raken van de schaalbaarheid limieten voor de Tabelpartities. Bijvoorbeeld moet een query met een filter selecteert die alleen één procent van de entiteiten in een partition, maar dat alle entiteiten in een partition gescand voor toegang tot elke entiteit. Elke entiteit lezen wordt tellen richting van het totale aantal transacties in die partition; u kunt de doelen schaalbaarheid daarom eenvoudig bereiken.

> [AZURE.NOTE] Uw prestaties testen, moet elke niet efficiënt queryontwerpen in uw toepassing geven.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Aan de doelstellingen weergeven een toename in PercentTimeoutError

Uw een toename zien in **PercentTimeoutError** voor een van uw opslagservices. Tegelijkertijd ontvangt de client een groot aantal "500 time-out voor de bewerking" http-statusberichten van opslagbewerkingen.

> [AZURE.NOTE] U ziet mogelijk time-out fouten tijdelijk als de storage-service saldi aanvragen door een partition verplaatsen naar een nieuwe server te laden.

De meetwaarde **PercentTimeoutError** is een samenvoeging van de doelstellingen van de volgende: **ClientTimeoutError**, **AnonymousClientTimeoutError** **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**en **SASServerTimeoutError**.

De servertime worden veroorzaakt door een fout op de server. De client-outs optreden omdat een bewerking op de server heeft overschreden time-out van de opgegeven door de client. een client met de bibliotheek van de Client opslag kunt bijvoorbeeld een time-out voor een bewerking instellen met behulp van de eigenschap **ServerTimeout** van de klasse **QueueRequestOptions** .

Servertime geven aan een probleem met de storage-service die moeten worden verder onderzoek. U kunt aan de doelstellingen gebruiken om te zien als u de schaalbaarheid limieten voor de service zijn kunt u door en eventuele pieken in het verkeer dat dit probleem veroorzaakt identificeren. Als het probleem onderbroken wordt, kan het zijn gevolg van taakverdeling activiteit in de service. Als het probleem permanente is en niet wordt veroorzaakt door uw toepassing kunt u door de limieten schaalbaarheid van de service, moet u een Ondersteuningsprobleem met de verhogen. Voor de client-outs, moet u bepalen of de time-out is ingesteld op de gewenste waarde in de client en een van de wijzigingen de time-outwaarde in de client instellen of onderzoeken hoe u kunt verbeteren de prestaties van de bewerkingen in de storage-service, bijvoorbeeld door het optimaliseren van uw tabel query's of het verkleinen van uw berichten.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Aan de doelstellingen weergeven een toename in PercentNetworkError

Uw een toename zien in **PercentNetworkError** voor een van uw opslagservices. De meetwaarde **PercentNetworkError** is een samenvoeging van de doelstellingen van de volgende: **NetworkError**, **AnonymousNetworkError**en **SASNetworkError**. Deze optreden bij de storage-service wordt een fout gedetecteerd wanneer de client een aanvraag opslag.

De meest voorkomende oorzaak van deze fout is een client verbinding Hiermee verbreken voordat een time-out in de storage-service verloopt. U moet de code onderzoek in uw-client voor meer informatie over waarom en wanneer de client verbinding met de storage-service verbreekt. U kunt ook Wireshark, Microsoft bericht Analyzer of Tcping gebruiken om te kunnen onderzoeken connectivity netwerkproblemen vanuit de client. Deze hulpprogramma's worden beschreven in de [bijlagen].

### <a name="the-client-is-receiving-403-messages"></a>De client ontvangt HTTP 403 (niet toegestaan) berichten

Als de clienttoepassing HTTP 403 (niet toegestaan) fouten genereren is, wordt er een waarschijnlijke oorzaak is dat de client een verlopen gedeeld Access handtekening (SA's) wordt gebruikt wanneer deze een aanvraag opslag stuurt (Hoewel de andere mogelijke oorzaken zijn klok Scheefheid, ongeldige sleutels en lege kop). Als een verlopen SA's-sleutel de oorzaak is, ziet u niet alle vermeldingen in de logboekgegevens van serverzijde opslag vastleggen. De volgende tabel ziet u een steekproef uit het aan de clientzijde logboek gegenereerd door de opslagruimte Client-bibliotheek die ziet u dit probleem zich voordoet:

Bron|Gegevens opnemen|Gegevens opnemen|Verzoek om de client-id|Bewerking tekst
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Bewerking wordt gestart met locatie primair per locatie modus PrimaryOnly.
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Beginnend synchroon aanvraag om deel te https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;si = mypolicy&amp;sig OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % = 3D&amp;api-versie = 2014-02-14.
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Wachten op antwoord.
Microsoft.WindowsAzure.Storage|Waarschuwing|2|85d077ab-...|Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|Een antwoord hebt ontvangen. Statuscode = 403, ID aanvragen = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, inhoud-MD5 =, ETag =.
Microsoft.WindowsAzure.Storage|Waarschuwing|2|85d077ab-...|Een uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3 |85d077ab-...|Controleren op als de bewerking opnieuw moet worden uitgevoerd. Probeer tellen = 0, HTTP-statuscode = 403, uitzondering = van de externe server heeft een fout geretourneerd: (403)...
Microsoft.WindowsAzure.Storage|Informatie|3|85d077ab-...|De volgende locatie is ingesteld op primair, op basis van de locatie-modus.
Microsoft.WindowsAzure.Storage|Fout|1|85d077ab-...|Opnieuw beleid is niet toegestaan voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: (403).

In dit scenario, moet u achterhalen waarom het token SA's is bijna verlopen voordat de client het token naar de server verzendt:

- U moet een begintijd bij het maken van een SA's voor een client direct kunt gebruiken meestal niet instellen. Als er kleine klok verschillen tussen de host genereren van de SA's met de huidige tijd en de storage-service, dan is het mogelijk voor de storage-service voor het ontvangen van een SA's dat is nog niet geldig.
- U moet een zeer korte verlooptijd niet ingesteld voor een SA's. Klik nogmaals kunnen kleine klok verschillen tussen de host genereren van de SA's en de storage-service leiden tot een SA's gehouden bijna verlopen eerder dan verwacht.
- Bevat de parameter versie in de sleutel SA's (bijvoorbeeld **AVP = 2015-04-05**) overeenkomt met de versie van de opslagruimte Client-bibliotheek die u gebruikt? Het is raadzaam dat u altijd de nieuwste versie van de [Bibliotheek voor opslag-Client](https://www.nuget.org/packages/WindowsAzure.Storage/)gebruiken.
- Als u uw toegangstoetsen opslag, kan dit een bestaande SA's tokens ongeldig. Dit kan een probleem zijn als u de tokens SA's met een lange verlooptijd voor cache-clienttoepassingen genereren.

Als u de bibliotheek van de Client opslag te genereren SA's tokens gebruikt, is het gemakkelijk te maken van een geldige token. Als u de opslagruimte REST API gebruiken en het bouwen van de tokens SA's met de hand moet u echter zorgvuldig door het [Delegeren van Access met een Access gedeeld handtekening](http://msdn.microsoft.com/library/azure/ee395415.aspx)-onderwerp lezen.

### <a name="the-client-is-receiving-404-messages"></a>De client ontvangt HTTP 404 (niet gevonden) berichten
Als de clienttoepassing ontvangt een bericht HTTP 404 (niet gevonden) van de server, wordt dit betekent dat het object dat de client probeerde te gebruiken (zoals een entiteit, tabel, blob, container of wachtrij) niet in de storage-service bestaat. Er zijn een aantal redenen, zoals:

- [De client of een ander proces eerder verwijderd van het object]
- [Een probleem met de autorisatie gedeeld Access handtekening (SA's)]
- [Aan de clientzijde JavaScript-code is niet gemachtigd voor toegang tot het object]
- [Netwerk is mislukt]

#### <a name="client-previously-deleted-the-object"></a>De client of een ander proces eerder verwijderd van het object
In scenario's waarin de client is geprobeerd te lezen, bijwerken of verwijderen van gegevens in een storage-service is het meestal eenvoudig aan te geven in de logboeken aan de clientzijde een vorige bewerking die het desbetreffende object verwijderd uit de storage-service. De logboekgegevens vaak, ziet u dat het object door een andere gebruiker of proces worden verwijderd. In het logboek serverzijde opslag logboekregistratie weergeven het type bewerking en aangevraagd-object-toets kolommen wanneer een object in een client worden verwijderd.

In het scenario waar een client probeert een object wilt invoegen, deze mogelijk niet onmiddellijk duidelijk waarom dit in een HTTP 404 (niet gevonden) antwoord wordt verzonden, resulteert gezien het feit dat de client is er een nieuw object. Als de client is een blob deze moet kunnen vinden van de container blob als de client is maken van een bericht dat deze moet kunnen zoeken van wachtrijen maken en de client is het toevoegen van een rij moet deze wel kunt vinden in de tabel.

U kunt het logboek aan de clientzijde van de bibliotheek van de Client opslag krijgen een meer gedetailleerde begrip van wanneer de client specifieke verzoeken met de storage-service verzendt.

Het volgende aan de clientzijde logboek gegenereerd door de opslag-Client-bibliotheek ziet u het probleem wanneer de client de container voor de blob die het maakt niet vinden. Dit logboek bevat details van de volgende opslagbewerkingen:

Aanvraag-ID|Bewerking
---|---
07b26a5d-...|**DeleteIfExists** methode voor het verwijderen van de container blob. Houd er rekening mee dat deze bewerking met een verzoek om een **kop** wilt controleren op de aanwezigheid van de container.
e2d06d78...|**CreateIfNotExists** methode voor het maken van de container blob. Houd er rekening mee dat deze bewerking een **hoofd** -aanvraag die Hiermee wordt gecontroleerd op de aanwezigheid van de container bevat. Het **hoofd** geeft een 404 bericht maar blijft.
de8b1c3c-...|**UploadFromStream** methode voor het maken van de blob. Het verzoek **plaatsen** mislukt met een 404-bericht

Logboekvermeldingen:

Aanvraag-ID |  Bewerking tekst
---|---
07b26a5d-...|Synchroon aanvraag om deel te https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Wachten op antwoord.
07b26a5d-... | Een antwoord hebt ontvangen. Statuscode = 200, ID aanvragen = eeead849... Inhoud-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Antwoord kopteksten zijn verwerkt, u doorgaat met de rest van de bewerking.
07b26a5d-... | Antwoord hoofdtekst moeten worden gedownload.
07b26a5d-... | De bewerking is voltooid.
07b26a5d-... | Synchroon aanvraag om deel te https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Wachten op antwoord.
07b26a5d-... | Een antwoord hebt ontvangen. Statuscode = 202-ID aanvragen = 6ab2a4cf-..., inhoud-MD5 =, ETag =.
07b26a5d-... | Antwoord kopteksten zijn verwerkt, u doorgaat met de rest van de bewerking.
07b26a5d-... | Antwoord hoofdtekst moeten worden gedownload.
07b26a5d-... | De bewerking is voltooid.
e2d06d78-... | Asynchroon aanvraag om deel te https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Wachten op antwoord.
de8b1c3c-... | Synchroon aanvraag om deel te https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt wordt gestart.
de8b1c3c-... |  StringToSign = plaatsen... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Bij het wegschrijven verzoek gegevens voorbereiden.
e2d06d78-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (404) niet gevonden...
e2d06d78-... | Een antwoord hebt ontvangen. Statuscode = 404, ID aanvragen = 353ae3bc-..., inhoud-MD5 =, ETag =.
e2d06d78-... | Antwoord kopteksten zijn verwerkt, u doorgaat met de rest van de bewerking.
e2d06d78-... | Antwoord hoofdtekst moeten worden gedownload.
e2d06d78-... | De bewerking is voltooid.
e2d06d78-... | Asynchroon aanvraag om deel te https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.
e2d06d78-...|StringToSign = plaatsen... 0...x-MS-Client-Request-id:e2d06d78-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Wachten op antwoord.
de8b1c3c-... | Gegevens van de aanvraag schrijven.
de8b1c3c-... | Wachten op antwoord.
e2d06d78-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (409) Conflict...
e2d06d78-... | Een antwoord hebt ontvangen. Statuscode = 409, ID aanvragen = c27da20e-..., inhoud-MD5 =, ETag =.
e2d06d78-... | Fout antwoord hoofdtekst moeten worden gedownload.
de8b1c3c-... | Uitzondering opgetreden tijdens het wachten op antwoord: de externe server heeft een fout geretourneerd: (404) niet gevonden...
de8b1c3c-... | Een antwoord hebt ontvangen. Statuscode = 404, ID aanvragen = 0eaeab3e-..., inhoud-MD5 =, ETag =.
de8b1c3c-...| Een uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (404) niet gevonden...
de8b1c3c-... | Opnieuw beleid is niet toegestaan voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: (404) niet gevonden...
e2d06d78-... | Opnieuw beleid is niet toegestaan voor een nieuwe poging. Fout met de externe server heeft een fout geretourneerd: (409) Conflict...

In dit voorbeeld in het logboek toont de client aanmeldingsaanvragen van de methode **CreateIfNotExists** (aanvraag-id e2d06d78...) met het aanvragen van de methode **UploadFromStream** is interleaving (de8b1c3c-...); Dit komt omdat de clienttoepassing is aanroepen van deze methoden asynchroon. U moet de asynchrone code in de client om ervoor te zorgen dat de container wordt gemaakt voordat u probeert gegevens uploaden naar een blob in dat onderdeel aanpassen. In het ideale geval moet u alle uw containers tevoren maken.

#### <a name="SAS-authorization-issue"></a>Een probleem met de autorisatie gedeeld Access handtekening (SA's)

Als de clienttoepassing probeert te gebruiken van een sleutel SA's die de benodigde machtigingen voor de bewerking niet bevat, retourneert de storage-service een HTTP 404 (niet gevonden)-bericht naar de klant. Tegelijkertijd, ook ziet u een andere waarde dan nul voor **SASAuthorizationError** in de aan de doelstellingen.

De volgende tabel ziet u een voorbeeldbericht serverzijde log uit het logboekbestand opslag logboekregistratie:

<table>
  <tr>
    <td>Begintijd aanvragen</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Bewerkingstype</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Status aanvragen</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-statuscode</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Verificatietype</td>
    <td>SA 's</td>
  </tr>
  <tr>
    <td>Servicetype</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Aanvraag-URL</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; si = mypolicy&amp;amp; sig XXXXX =&amp;amp; api-versie = 2014-02-14&amp;amp;</td>
  </tr>
  <tr>
    <td>Koptekst-id aanvragen</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Verzoek om de client-ID</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

U moet onderzoek waarom de clienttoepassing probeert uit te voeren een bewerking die geen machtigingen voor is toegekend.

#### <a name="JavaScript-code-does-not-have-permission"></a>Aan de clientzijde JavaScript-code is niet gemachtigd voor toegang tot het object

Als u een JavaScript-client gebruikt en de storage-service retourneert HTTP 404 berichten u de volgende JavaScript-fouten in de browser controleren:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] U kunt de F12 ontwikkelaars hulpmiddelen in Internet Explorer de berichten uitwisselen tussen de browserversie en de storage-service bij het oplossen van aan de clientzijde JavaScript problemen bijhouden.

Deze fouten optreden omdat de webbrowser de beveiligingsbeperking [hetzelfde origin beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) die voorkomen dat een webpagina een API oproepen in een ander domein dan het domein dat de pagina afkomstig zijn implementeert uit.

U kunt u de JavaScript-probleem omzeilen, Cross Origin Resource delen (CORS) configureren voor de storage-service die is bij het openen van de client. Zie [Cross-Origin Resource delen (CORS)-ondersteuning voor Azure opslagservices](http://msdn.microsoft.com/library/azure/dn535601.aspx)voor meer informatie.

Het volgende voorbeeld ziet u hoe u uw service blob zodat JavaScript met in het domein Contoso voor toegang tot een blob in uw blob storage-service configureren:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Netwerk is mislukt

In sommige gevallen verloren pakketten kunnen leiden tot de storage-service HTTP 404 berichten terugkeren naar de klant. Bijvoorbeeld wanneer de clienttoepassing van een entiteit van de service tabel verwijderen ziet u de client een opslag uitzondering rapporteren genereert een ' HTTP 404 (niet gevonden) "statusbericht van de tabel-service. Wanneer u de tabel in de tabel storage-service onderzoeken, ziet u dat de service de entiteit hebt verwijderd, zoals gevraagd.

De volgende uitzondering gegevens in de client bevatten aanvraag-id (7e84f12d...) door de tabel-service voor het verzoek toegewezen: u kunt deze informatie gebruiken om te zoeken van de aanvraag details in de logboeken aan de clientzijde opslag door te zoeken in de **koptekst van een aanvraag-id** -kolom in het logboekbestand. U kunt ook de doelstellingen om te bepalen wanneer fouten zoals dit optreden en zoek naar de logboekbestanden op basis van de tijd waarop die de aan de doelstellingen deze fout opgenomen. Vermelding ziet u dat het verwijderen met een statusbericht "HTTP (404) Client andere fout" is mislukt. De dezelfde vermelding bevat ook de aanvraag-id die is gegenereerd door de klant in de kolom **klant-aanvraag-id** (813ea74f...).

Het logboek serverzijde bevat ook een andere vermelding met dezelfde **client-aanvraag-id** waarde (813ea74f...) voor een succesvolle verwijderbewerking voor dezelfde entiteit en van dezelfde client. Deze bewerking geslaagd plaatsgevonden zeer kort voordat de mislukte aanvraag verwijderen.

De meest waarschijnlijke oorzaak van dit scenario is dat de client een aanvraag verwijderen voor de entiteit verstuurd naar de tabel-service, die is geslaagd, maar geen een bevestiging hebt ontvangen van de server (bijvoorbeeld vanwege een probleem tijdelijk netwerk). De client vervolgens automatisch opnieuw geprobeerd de bewerking (via de dezelfde **client-aanvraag-id**) en deze opnieuw mislukt omdat de entiteit al is verwijderd.

Als dit probleem treedt regelmatig op, moet u onderzoek waarom de client voor het ontvangen van bevestigingen van de tabel-service is verbroken. Als het probleem onderbroken wordt, moet u de "HTTP (404) niet gevonden" fout onderscheppen en meld u aan deze de client, maar dat de client om door te gaan.

### <a name="the-client-is-receiving-409-messages"></a>De client ontvangt HTTP 409 (Conflict) berichten

De volgende tabel ziet u een uittreksel uit het logboek servers voor twee clientbewerkingen: **DeleteIfExists** onmiddellijk gevolgd door **CreateIfNotExists** dezelfde blob container naam. Opmerking dat elke client bewerking resulteert in twee aanvragen verzonden naar de server, eerst een aanvraag **GetContainerProperties** om te controleren als de container bestaat, gevolgd door het verzoek **DeleteContainer** of **CreateContainer** .

Tijdstempel|Bewerking|Resultaat|De containernaam van de|Verzoek om de client-id
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

De code in de clienttoepassing verwijderd en opnieuw een blob container met dezelfde naam vervolgens onmiddellijk gemaakt: de **CreateIfNotExists** methode (Client aanvragen ID bc881924-...) uiteindelijk mislukt met de fout HTTP 409 (Conflict). Bij een client worden verwijderd blob containers, tabellen of wachtrijen er een korte termijn voordat u de naam is weer beschikbaar.

De clienttoepassing moet unieke container namen gebruiken wanneer er nieuwe containers gemaakt als het patroon verwijderen/opnieuw gelijke waarden.

### <a name="metrics-show-low-percent-success"></a>Aan de doelstellingen lage PercentSuccess weergeven of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors

De meetwaarde **PercentSuccess** wordt het percentage van de bewerkingen die aangebracht op basis van hun HTTP-statuscode zijn vastgelegd. Bewerkingen met statuscodes van 2XX tellen als geslaagd, terwijl bewerkingen met statuscodes in 3XX, 4XX en 5XX bereiken worden geteld als mislukt of verlaag de **PercentSucess** metrische waarde. In de logboeken aan de clientzijde opslag, worden deze bewerkingen opgenomen met de transactiestatus **ClientOtherErrors**.

Het is belangrijk te weten dat deze bewerkingen zijn voltooid en kunnen daarom niet van invloed op andere criteria zoals beschikbaarheid. Enkele voorbeelden van bewerkingen die is geslaagd, maar dat kan leiden tot mislukte HTTP-statuscodes zijn:
- **ResourceNotFound** (Niet gevonden 404), bijvoorbeeld van een GET-aanvraag om een blob die niet bestaat.
- **ResouceAlreadyExists** (Conflict 409), bijvoorbeeld van een **CreateIfNotExist** bewerking waar de resource al bestaat.
- **ConditionNotMet** (Niet gewijzigd 304), bijvoorbeeld van een voorwaardelijke bewerking zoals wanneer een client stuurt een **ETag** -waarde en de kop van een HTTP- **If-None-Match** voor het aanvragen van een afbeelding alleen als deze is bijgewerkt sinds de laatste bewerking.

U vindt een lijst met algemene REST API-foutcodes die de storage-services op de pagina [Algemene REST API-foutcodes](http://msdn.microsoft.com/library/azure/dd179357.aspx)retourneren.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>De doelstellingen van de capaciteit weergeven een onverwachte toename in gebruik van de capaciteit opslag


Als u plotselinge ziet, onverwachte wijzigingen in capaciteit gebruik in uw account opslag, kunt u onderzoeken de redenen door eerst te zoeken naar uw beschikbaarheid aan de doelstellingen; bijvoorbeeld, een stijging van het aantal mislukte verwijderen aanvragen tot een verhoging van blobopslag die u gebruikt leiden kunnen als specifieke opschonen toepassing die u mogelijk hebt denkt dat worden vrijmaken van ruimte niet werkt zoals verwacht (bijvoorbeeld omdat de SA's tokens die wordt gebruikt voor het vrijmaken van ruimte zijn verlopen).

### <a name="you-are-experiencing-unexpected-reboots"></a>Ondervindt u onverwachte opnieuw opstarten van Azure virtuele Machines die een groot aantal bijgevoegde VHD 's

Als een Azure Virtual Machine (VM) een groot aantal bijgevoegde VHD's die in hetzelfde opslag-account heeft zijn, kunt u de doelen schaalbaarheid voor een individuele opslag-account veroorzaakt door de VM mislukt overschrijden. U moet de minuut aan de doelstellingen voor de opslag-account controleren (**TotalRequests**/**TotalIngress**/**TotalEgress**) voor pieken die groter is dan de doelen schaalbaarheid voor een opslag-account. Zie de sectie '[dat een stijging PercentThrottlingError zien]' voor hulp bij het bepalen van als beperken op uw account opslag is opgetreden.

In het algemeen wordt equivalent elke afzonderlijke invoer of uitvoer betrekking heeft op een VHD vanaf een virtuele Machine in **Krijgen pagina** of **Pagina plaatsen** bewerkingen in de onderliggende pagina-blob. Daarom kunt u het geschatte IO's / s voor uw omgeving om af te stellen hoeveel VHD's die u kunt hebt in een account één opslagruimte op basis van het specifieke gedrag van de toepassing. We raden niet meer dan 40 schijven in een enkel opslag-account hebt. Zie [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md) voor meer informatie over de huidige schaalbaarheid doelen voor opslag-accounts, met name de totale verzoek rente en de totale bandbreedte voor het type account dat opslagruimte u gebruikt.
Als u de schaalbaarheid doelen voor uw account opslagruimte overschrijdt, moet u uw VHD's in meerdere verschillende opslag accounts verkleinen van de activiteit in elke rekening plaatsen.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Het probleem zich voordoet de emulator opslag voor ontwikkel- of gebruiken

U meestal de emulator opslag gebruiken tijdens de ontwikkeling en testen om te voorkomen dat de vereiste voor een Azure opslag-account. Er zijn de algemene problemen die optreden kunnen wanneer u de emulator opslagruimte gebruikt:

- [Functie 'X' werkt niet in de emulator opslag]
- [Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling"bij het gebruik van de emulator opslag]
- [Het uitvoeren van de emulator opslag is vereist beheerdersbevoegdheden]

#### <a name="feature-X-is-not-working"></a>Functie 'X' werkt niet in de emulator opslag

De opslagruimte emulator ondersteunt niet alle functies van de Azure storage-services, zoals de service bestand. Zie [gebruiken de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md)voor meer informatie.

Voor deze functies waarmee de emulator opslag niet wordt ondersteund, gebruikt u de Azure storage-service in de cloud.

#### <a name="error-HTTP-header-not-correct-format"></a>Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling"bij het gebruik van de emulator opslag

U test uw toepassing die de bibliotheek van de Client opslag ten opzichte van de lokale opslag emulator gebruiken en methode roept zoals **CreateIfNotExists** mislukken met het foutbericht "de waarde voor een van de kopteksten HTTP is niet de juiste notatie." Hiermee wordt aangeduid dat de versie van de opslagruimte emulator die u gebruikt geen ondersteuning biedt voor de versie van de opslagruimte client-bibliotheek die u gebruikt. De bibliotheek van de Client opslag worden alle verzoeken die dit zorgt ervoor dat de kop **x-ms-versie** opgeteld. Als de opslagruimte emulator de waarde in de kop van de **x-ms-versie** niet wordt herkend, wordt het verzoek geweigerd.

U kunt de opslagruimte bibliotheek Client-Logboeken gebruiken om de waarde van de **kop van de x-ms-versie** , die worden er weer te geven. U kunt ook de waarde van de **kop van de x-ms-versie** bekijken als u Fiddler gebruikt om te traceren van het aanvragen van uw clienttoepassing.

Dit scenario is meestal treedt op als u installeren en gebruiken van de meest recente versie van de bibliotheek van de Client opslag zonder het bijwerken van de emulator opslag. U moet de nieuwste versie van de emulator opslag installeren of cloudopslag gebruiken in plaats van de emulator voor ontwikkeling en test.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Het uitvoeren van de emulator opslag is vereist beheerdersbevoegdheden

U wordt gevraagd om beheerdersreferenties wanneer u de emulator opslag uitvoert. Dit gebeurt alleen wanneer u de emulator opslag voor het eerst worden geïnitialiseerd. Nadat u hebt de opslagruimte emulator geïnitialiseerd, hoeft u geen beheerdersbevoegdheden opnieuw uitvoeren.

Zie [gebruiken de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md)voor meer informatie. Houd er rekening mee dat kunt u ook de emulator opslagruimte in Visual Studio, die worden ook beheerdersbevoegdheden moeten geïnitialiseerd.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Ondervindt u problemen bij het installeren van de Azure-SDK voor .NET

Wanneer u probeert de SDK installeren, mislukt het installeren van de emulator opslag op uw lokale computer. Het installatielogboek bevat een van de volgende berichten:

- CAQuietExec: Fout: geen toegang tot SQL-exemplaar
- CAQuietExec: Fout: kan database maken

De oorzaak is een probleem met bestaande LocalDB installatie. Standaard wordt in de opslagruimte emulator LocalDB gebruikt om gegevens wanneer deze de Azure storage-services simuleert. U kunt uw exemplaar LocalDB herstellen door de volgende opdrachten uit te voeren in een venster opdrachtprompt voordat u probeert de SDK installeren.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

De opdracht **verwijderen** verwijdert alle oude databasebestanden uit eerdere installaties van de emulator opslag.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Hebt u een ander probleem met een storage-service

Als het probleem met een storage-service in de vorige voor probleemoplossing secties niet opneemt, moet u de volgende methode voor de diagnose en probleemoplossing van het probleem vast.

- Controleer uw maatstelsel of er een veranderd ten opzichte van de verwachte basislijn gedrag. Vanuit de aan de doelstellingen, is het mogelijk om te bepalen of het probleem tijdelijk of permanent en welke opslagbewerkingen het probleem is dat dit gevolgen heeft.
- Kunt u de logboekgegevens van servers voor meer informatie over eventuele fouten die optreden zoeken kunt u de gegevens aan de doelstellingen. Deze informatie kunt u het probleem kunt oplossen.
- Als de informatie in de logboeken aan de clientzijde niet voldoende het probleem kunt oplossen is, kunt u de logboeken aan de clientzijde opslag clientbibliotheek om te kunnen onderzoeken het gedrag van uw clienttoepassing en hulpprogramma's zoals Fiddler, Wireshark en Microsoft bericht Analyzer naar uw netwerk onderzoeken.

Zie voor meer informatie over het gebruik van Fiddler "[bijlage 1: Fiddler gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan]."

Zie voor meer informatie over het gebruik van Wireshark "[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]."

Zie voor meer informatie over het gebruik van Microsoft bericht Analyzer "[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]."

## <a name="appendices"></a>Bijlagen

De bijlagen beschrijven verschillende hulpmiddelen die mogelijk nuttig wanneer u bent diagnose en het oplossen van problemen met Azure Storage (en andere services). Deze hulpprogramma's zijn niet deel uit van Azure Storage en andere producten van derden. Als zodanig de hulpmiddelen voor besproken in deze bijlagen vallen niet elke ondersteuningsovereenkomst met Microsoft Azure of Azure Storage wellicht en daarom als onderdeel van het evaluatieproces van in moet u de licenties en ondersteuning beschikbare opties van de providers deze tools onderzoeken.

### <a name="appendix-1"></a>Bijlage 1: Fiddler gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan

[Fiddler](http://www.telerik.com/fiddler) is een handig hulpmiddel voor het analyseren van de HTTP- en HTTPS-verkeer is toegestaan tussen uw clienttoepassing en de Azure storage-service die u gebruikt.

> [AZURE.NOTE] Fiddler kunt decoderen HTTPS-verkeer is toegestaan; u moet de documentatie Fiddler zorgvuldig kunnen begrijpen hoe dit gebeurt en voor meer informatie over de beveiligingsrisico's lezen.

Deze bijlage bevat een korte Stapsgewijze instructies voor het configureren van Fiddler vastleggen verkeer tussen de lokale computer waarop u Fiddler hebt geïnstalleerd en de Azure storage-services.

Wanneer u Fiddler hebt gestart, wordt deze beginnen met het opnemen van HTTP en HTTPS-verkeer is toegestaan op uw lokale computer. Hier volgen enkele nuttige opdrachten voor het aanpassen van Fiddler:

- Stoppen en begint met het opnemen van verkeer. In het hoofdmenu, gaat u naar **bestand** en klik vervolgens op **Vastleggen verkeer** om te schakelen vastleggen in- of uitschakelen.
- Sla de gegevens vastgelegd verkeer op. Ga op het hoofdmenu **bestand**, klikt u op **Opslaan**en klik vervolgens op **Alle sessies**: Hiermee kunt u het verkeer opslaan in een sessie archiefbestand. U kunt een archief sessie later voor analyse laden of verzenden als dit wordt gevraagd naar ondersteuning van Microsoft.

U beperkt de hoeveelheid verkeer die Fiddler wordt vastgelegd, kunt u filters die u op het tabblad **Filters** configureren. De volgende schermafbeelding ziet u een filter dat wordt vastgelegd alleen verkeer verzonden naar het eindpunt van de opslagruimte **contosoemaildist.table.core.windows.net** :

![][5]

### <a name="appendix-2"></a>Bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer

[Wireshark](http://www.wireshark.org/) is een netwerk protocol analyzer waarmee u gedetailleerde pakket informatie voor een groot aantal netwerkprotocollen kunnen bekijken.

De volgende procedure ziet u hoe om vast te leggen gedetailleerde pakket informatie voor verkeer van de lokale computer waarop u Wireshark geïnstalleerd met de service tabel in uw account Azure opslag.

1.  Start Wireshark op uw lokale computer.
2.  Selecteer in de sectie **starten** het lokale netwerk of interfaces die met internet verbonden zijn.
3.  Klik op **Opties vastleggen**.
4.  Een filter toevoegen aan het tekstvak **Filter vastleggen** . **Host contosoemaildist.table.core.windows.net** wordt bijvoorbeeld Wireshark vastleggen alleen pakketten verzonden naar of vanaf het service-eindpunt van de tabel in het **contosoemaildist** opslag-account configureren. Bekijk de [volledige lijst met Filters vastleggen](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Klik op **Start**. Wireshark vastleggen wordt nu alle de pakketten verzenden van en naar het eindpunt van de service tabel terwijl u de clienttoepassing op uw lokale computer gebruikt.
6.  Wanneer u klaar bent, klik op het hoofdmenu op **vastleggen** en **stoppen**.
7.  Als u wilt de opgenomen gegevens opslaan in een bestand voor het vastleggen van Wireshark, klik op het hoofdmenu op **bestand** en klik vervolgens op **Opslaan**.

WireShark worden eventuele fouten die zijn opgenomen in het venster **packetlist** gemarkeerd. U kunt ook het informatievenster **Expert** gebruiken (Klik op **analyseren** **Expert Info**) geeft een overzicht van fouten en waarschuwingen.

![][7]

U kunt ook hebt gekozen om weer te geven van de TCP-gegevens als de toepassingslaag ziet deze door met de rechtermuisknop op de TCP-gegevens en **Volg TCP-Stream**te selecteren. Dit is vooral handig als u uw dump zonder een filter vastleggen vastgelegd. Zie voor meer informatie, [volgen TCP-Streams] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Zie voor meer informatie over het gebruik van Wireshark [Wireshark gebruikershandleiding](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Bijlage 3: Microsoft bericht Analyzer gebruiken om vast te leggen netwerkverkeer

U kunt Microsoft bericht Analyzer gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan in een soortgelijke manier om te Fiddler en netwerkverkeer vastleggen in een soortgelijke manier om te Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Een sessie voor het traceren van web via Microsoft bericht Analyzer configureren

Als u wilt configureren een sessie voor het traceren van web voor HTTP en HTTPS-verkeer via Microsoft bericht Analyzer, voert u de toepassing Microsoft bericht Analyzer en klik op het menu **bestand** op **Vastleggen/doelcellen**. Selecteer in de lijst met beschikbare doelcellen scenario's, **Web Proxy**. Klik in het deelvenster **Doelcellen Scenario configuratie** in het tekstvak **HostnameFilter** toevoegen de namen van de eindpunten van uw opslagruimte (u kunt deze namen opzoeken in de [Portal van Azure](https://portal.azure.com)). Als de naam van uw account Azure opslag **contosodata is**, moet u bijvoorbeeld de volgende manieren te werk toevoegen aan het tekstvak **HostnameFilter** :

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Een spatie worden gescheiden door de hostnamen.

Wanneer u klaar om te beginnen met het verzamelen van gegevens van de berichttracering bent, klikt u op de knop **Start met** .

Zie voor meer informatie over de Microsoft bericht Analyzer **Web Proxy** -tracering, [Microsoft-PEF-WebProxy Provider](http://technet.microsoft.com/library/jj674814.aspx).

De ingebouwde **Web Proxy** -tracering in Microsoft bericht Analyzer is gebaseerd op Fiddler; Er kan aan de clientzijde HTTPS-verkeer vastleggen en weergeven van versleutelde HTTPS-berichten. De tracering **Web Proxy** werkt door een lokale proxy voor alle HTTP en HTTPS-verkeer die deze toegang tot versleutelde berichten geeft configureren.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnose netwerkproblemen via Microsoft bericht Analyzer

Naast de tracering Microsoft bericht Analyzer **Web Proxy** om vast te leggen details van het HTTP-/ HTTPs-verkeer tussen de clienttoepassing en de storage-service, kunt u ook de ingebouwde **Lokale Link Layer** tracering gebruiken om vast te leggen netwerkgegevens pakket. Dit kunt u de gegevens die u kunt vastleggen met Wireshark, en een diagnose stellen bij strekking opnemen problemen, zoals netwerk verloren pakketten.

De volgende schermafbeelding ziet u een voorbeeld **Lokale Link Layer** spoor met sommige **informatieve** berichten in de kolom **DiagnosisTypes** . Klikken op een pictogram in de kolom **DiagnosisTypes** , ziet u de details van het bericht. In dit voorbeeld van de server opnieuw bericht #305 verzonden, omdat er geen een bevestiging is ontvangen van de client:

![][9]

Wanneer u de sessie voor het traceren in Microsoft bericht Analyzer maakt, kunt u filters om te verkleinen van de hoeveelheid ruis in de trace opgeven. Klik op de pagina van het **vastleggen / aanwijzen** waar u de tracering definieert, klik u op de koppeling **configureren** naast **Microsoft-Windows-NDIS-PacketCapture**. De volgende schermafbeelding ziet u een configuratie die worden gefilterd TCP-verkeer is toegestaan om de IP-adressen van drie opslagservices:

![][10]

Zie voor meer informatie over de tracering Microsoft bericht Analyzer lokale koppelingslaag, [Microsoft-PEF-NDIS-PacketCapture-Provider](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Bijlage 4: Gebruik van Excel voor het weergeven van de doelstellingen en gegevens vastleggen

Veel hulpmiddelen kunt u de gegevens metrische opslaggegevens downloaden uit Azure-tabelopslag in de indeling van een scheidingstekens die kunt u heel gemakkelijk de gegevens laadt in Excel voor weergave en analyse. Opslag registreren van gegevens van Azure-blobopslag, is al in een gescheiden indeling die u in Excel laden kunt. U moet echter juiste kolomkoppen op basis van de gegevens op [Opslagindeling Analytics Log](http://msdn.microsoft.com/library/azure/hh343259.aspx) en [Opslag Analytics aan de doelstellingen tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx)toevoegen.

Uw opslag logboekregistratie om gegevens te importeren in Excel nadat u het downloaden van blobopslag:

- Klik op **Van tekst**in het menu **gegevens** .
- Blader naar het logboekbestand dat u wilt weergeven en klik op **importeren**.
- Selecteer bij stap 1 van de **Wizard Tekst importeren**, **gescheiden**.

In stap 1 van de **Wizard Tekst importeren**, selecteert u **puntkomma** als scheidingsteken alleen te gebruiken en kies dubbel-offerte als de **Tekstscheidingsteken**. Klik op **Voltooien** en kies de locatie van de gegevens in uw werkmap.

### <a name="appendix-5"></a>Bijlage 5: Bewaken met toepassing inzichten voor Visual Studio teamservices

U kunt ook de functie toepassing inzichten voor Visual Studio Team Services gebruiken als onderdeel van uw prestaties en beschikbaarheid controleren. Dit hulpmiddel kunt doen:

- Controleer of dat uw webservice is alleen beschikbaar en heeft gereageerd. Of uw app is een website of een apparaat-app waarin een webservice, kan deze uw URL testen om de paar minuten van locaties overal ter wereld en laat u weten als er een probleem is.
- Snel een prestatieproblemen of de uitzonderingen in uw webservice te analyseren. Lees meer als processor of andere resources worden uitgerekt, kunt u de stapel sporen krijgen in uitzonderingen en doorzoeken eenvoudig log sporen. Als de prestaties van de app onder aanvaardbaar limieten, kunnen we u een e-mailbericht verzenden. U kunt zowel .NET en Java-webservices controleren.

U vindt meer informatie aan [Wat is van toepassing inzichten?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Inleiding]: #introduction
[Hoe deze handleiding is ingedeeld]: #how-this-guide-is-organized

[Uw opslagservice controleren]: #monitoring-your-storage-service
[Servicestatus bewaken]: #monitoring-service-health
[Cmdlets voor controle capaciteit]: #monitoring-capacity
[Beschikbaarheid controleren]: #monitoring-availability
[Prestaties controleren]: #monitoring-performance

[Diagnose van problemen opslag]: #diagnosing-storage-issues
[Service servicestatusproblemen zijn]: #service-health-issues
[Prestatieproblemen]: #performance-issues
[Oplossen van fouten]: #diagnosing-errors
[Opslag emulator problemen]: #storage-emulator-issues
[Hulpmiddelen voor logboekregistratie van opslag]: #storage-logging-tools
[Hulpmiddelen voor logboekregistratie van netwerk gebruiken]: #using-network-logging-tools

[End-to-end tracering]: #end-to-end-tracing
[Logboekgegevens correleren]: #correlating-log-data
[Verzoek om de client-ID]: #client-request-id
[ID van server]: #server-request-id
[Tijdstempels]: #timestamps

[Richtlijnen voor probleemoplossing]: #troubleshooting-guidance
[Aan de doelstellingen weergeven hoog AverageE2ELatency en lage AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Aan de doelstellingen lage AverageE2ELatency en lage AverageServerLatency weergeven, maar de client voordoet hoge latentie]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Aan de doelstellingen weergeven hoog AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Ondervindt u onverwachte vertragingen in het bericht is bezorgd op een wachtrij]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Aan de doelstellingen weergeven een toename in PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Tijdelijke stijging in PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanente stijging in PercentThrottlingError fout]: #permanent-increase-in-PercentThrottlingError
[Aan de doelstellingen weergeven een toename in PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Aan de doelstellingen weergeven een toename in PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[De client ontvangt HTTP 403 (niet toegestaan) berichten]: #the-client-is-receiving-403-messages
[De client ontvangt HTTP 404 (niet gevonden) berichten]: #the-client-is-receiving-404-messages
[De client of een ander proces eerder verwijderd van het object]: #client-previously-deleted-the-object
[Een probleem met de autorisatie gedeeld Access handtekening (SA's)]: #SAS-authorization-issue
[Aan de clientzijde JavaScript-code is niet gemachtigd voor toegang tot het object]: #JavaScript-code-does-not-have-permission
[Netwerk is mislukt]: #network-failure
[De client ontvangt HTTP 409 (Conflict) berichten]: #the-client-is-receiving-409-messages

[Aan de doelstellingen lage PercentSuccess weergeven of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]: #metrics-show-low-percent-success
[De doelstellingen van de capaciteit weergeven een onverwachte toename in gebruik van de capaciteit opslag]: #capacity-metrics-show-an-unexpected-increase
[Ondervindt u onverwachte opnieuw opstarten van virtuele Machines die een groot aantal bijgevoegde VHD 's]: #you-are-experiencing-unexpected-reboots
[Het probleem zich voordoet de emulator opslag voor ontwikkel- of gebruiken]: #your-issue-arises-from-using-the-storage-emulator
[Functie 'X' werkt niet in de emulator opslag]: #feature-X-is-not-working
[Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling"bij het gebruik van de emulator opslag]: #error-HTTP-header-not-correct-format
[Het uitvoeren van de emulator opslag is vereist beheerdersbevoegdheden]: #storage-emulator-requires-administrative-privileges
[Ondervindt u problemen bij het installeren van de Azure-SDK voor .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Hebt u een ander probleem met een storage-service]: #you-have-a-different-issue-with-a-storage-service

[Bijlagen]: #appendices
[Bijlage 1: Fiddler gebruiken om vast te leggen HTTP en HTTPS-verkeer is toegestaan]: #appendix-1
[Bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]: #appendix-2
[Bijlage 3: Microsoft bericht Analyzer gebruiken om vast te leggen netwerkverkeer]: #appendix-3
[Bijlage 4: Gebruik van Excel voor het weergeven van de doelstellingen en gegevens vastleggen]: #appendix-4
[Bijlage 5: Bewaken met toepassing inzichten voor Visual Studio teamservices]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
