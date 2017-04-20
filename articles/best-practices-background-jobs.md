<properties
   pageTitle="Achtergrond taken richtlijnen | Microsoft Azure"
   description="Richtlijnen voor achtergrond taken die klaar onafhankelijk van de gebruikersinterface."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Achtergrond taken richtlijnen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Overzicht

Verschillende soorten toepassingen vereisen achtergrondtaken die afzonderlijk van de gebruikersinterface (UI) worden uitgevoerd. Voorbeelden hiervan zijn taken, intensief verwerkingstaken en langdurige processen zoals werkstromen. Achtergrondtaken kunnen worden uitgevoerd zonder tussenkomst van gebruiker--de toepassing kunt start de taak en gaat u verder met het verwerken van interactieve aanmeldingsaanvragen van gebruikers. Dit kan bijdragen aan de belasting van de toepassing UI, die u kunt de beschikbaarheid van verbeteren en interactieve antwoord verkorten minimaliseren.

Bijvoorbeeld als een toepassing is vereist voor het genereren van miniaturen van afbeeldingen die door gebruikers zijn geüpload, kan dit als volgt als een achtergrondtaak en sla de miniatuur op opslag wanneer deze voltooid--zonder dat de gebruiker hoeft is te wachten om door het proces moet zijn voltooid. Op dezelfde manier, kan een gebruiker een bestelling een achtergrond-werkstroom die de volgorde verwerkt, terwijl de gebruikersinterface kan de gebruiker die u kunt doorgaan met het browsen op het WebApp starten. Wanneer de achtergrond voltooid is, kunt deze tabel bijwerken met de gegevens opgeslagen orders en een e-mailbericht verzenden naar de gebruiker die de volgorde bevestigt.

Wanneer u rekening houden of een taak als een achtergrondtaak implementeren, de belangrijkste criteria is of de taak kan worden uitgevoerd zonder tussenkomst van de gebruiker en zonder de gebruikersinterface hoeft te wachten om door de taak moet zijn voltooid. Taken waarvoor de gebruiker of de gebruikersinterface wachten terwijl ze zijn voltooid is mogelijk niet geschikt als achtergrondtaken.

## <a name="types-of-background-jobs"></a>Soorten achtergrondtaken

Achtergrondtaken omvatten een of meer van de volgende typen taken:

- CPU-veel-taken, zoals wiskundige berekeningen of structurele model analyse.
- Ik/O-veel-taken, zoals het uitvoeren van een reeks opslag transacties of bestanden indexeren.
- Batchtaken, zoals elke Gegevensupdates of geplande verwerking.
- Langdurige werkstromen, zoals orderafhandeling of inrichting services en -systemen.
- Verwerking van vertrouwelijke gegevens waar de taak wordt doorgegeven aan een veiliger locatie voor de verwerking. Zoals u mogelijk niet wilt verwerken gevoelige gegevens vanuit een web-app. In plaats daarvan kunt u een patroon zoals [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) wilt overbrengen van de gegevens naar een geïsoleerd achtergrondproces dat toegang tot beveiligde opslag heeft.

## <a name="triggers"></a>Triggers

Achtergrondtaken kunnen worden gestart op verschillende manieren. Worden ingedeeld in een van de volgende categorieën:

- [**Gebeurtenisgestuurde triggers**](#event-driven-triggers). De taak wordt in antwoord op een gebeurtenis, meestal een actie die u hebt gemaakt door een gebruiker of een stap in een werkstroom gestart.
- [**Triggers planning op basis van hoeveelheid werk**](#schedule-driven-triggers). De taak wordt aangeroepen volgens een schema op basis van een timer. Dit is mogelijk een terugkerende planning of een eenmalige aanroep die is opgegeven voor een later tijdstip.

### <a name="event-driven-triggers"></a>Gebeurtenisgestuurde triggers

Gebeurtenisgestuurde aanroep wordt een trigger gebruikt om te beginnen taak op de achtergrond. Voorbeelden van het gebruik van gebeurtenisgestuurde triggers zijn:

- De gebruikersinterface of een andere taak geplaatst een bericht in een wachtrij. Het bericht bevat gegevens over een actie die heeft plaatsgevonden, zoals de gebruiker die een bestelling plaatsen. Taak op de achtergrond op deze wachtrij luistert en vastgesteld de ontvangst van een nieuw bericht. Er leest het bericht en gebruikt de gegevens erin als invoer voor de achtergrondtaak.
- De gebruikersinterface of een andere taak wordt opgeslagen of een waarde in de opslagruimte wordt bijgewerkt. Taak op de achtergrond bewaakt de opslag en wijzigingen vastgesteld. Deze de gegevens leest en wordt deze gebruikt als invoer voor de achtergrondtaak.
- De gebruikersinterface of een andere taak, wordt een aanvraag voor eindpunt, zoals een HTTPS URI of een API dat wordt weergegeven als een webservice. De gegevens die is vereist voor het voltooien van de achtergrond als onderdeel van de aanvraag wordt doorgegeven. De service-eindpunt of web roept taak op de achtergrond, die de gegevens worden gebruikt als invoer.

Normale voorbeelden van taken die zijn geschikt voor gebeurtenisgestuurde aanroep zijn verwerking, werkstromen, verzenden naar externe services, e-mailberichten verzenden en inrichten van nieuwe gebruikers in multitenant toepassingen van afbeeldingen.

### <a name="schedule-driven-triggers"></a>Triggers planning op basis van hoeveelheid werk

Een timer aanroep planning op basis van hoeveelheid werk gebruikt om te beginnen taak op de achtergrond. Voorbeelden van het gebruik van planning op basis van hoeveelheid werk triggers zijn:

- Een timer die lokaal wordt uitgevoerd in de toepassing of als onderdeel van het besturingssysteem van de toepassing roept een achtergrondtaak regelmatig.
- Een timer die wordt uitgevoerd in een andere toepassing of een timerservice zoals Azure Scheduler, wordt een aanvraag verzonden naar een webservice API regelmatig. De service API of web roept taak op de achtergrond.
- Een afzonderlijk proces of de toepassing wordt gestart een timer die ervoor zorgt de taak dat moet worden aangeroepen eenmaal na een bepaald tijdsinterval vertraging of op een bepaalde tijd op de achtergrond.

Typische voorbeelden van taken die zijn geschikt voor planning op basis van hoeveelheid werk aanroep zijn batchbestand routines (zoals bijwerken gerelateerde producten lijsten voor gebruikers die op basis van hun recente gedrag), dagelijks gegevensverwerking taken (zoals indexen bijwerken of genereren van de samengevoegde resultaten), gegevensanalyse voor dagelijkse rapporten, gegevens bewaarbeleid opschonen en consistentiecontrole gegevens.

Als u een taak voor planning op basis van hoeveelheid werk die als één exemplaar uitvoeren moet, moet u rekening houden met de volgende:

- Als het exemplaar berekeningscluster waarop de planner (zoals een virtuele machine met Windows geplande taken) wordt aangepast, hebt u meerdere exemplaren van de planner uitgevoerd. Deze kunnen meerdere exemplaren van de taak starten.
- Als de taken uitvoeren voor meer dan de periode tussen scheduler gebeurtenissen, is het mogelijk dat de planner, een ander exemplaar van de taak starten terwijl de vorige taak nog actief is.

## <a name="returning-results"></a>Resultaten retourneren

Achtergrondtaken uitvoeren asynchroon in een afzonderlijk proces, of zelfs in een andere locatie, dan de gebruikersinterface of het proces dat taak op de achtergrond aangeroepen. In het ideale geval achtergrondtaken zijn 'fire en vergeet' bewerkingen en hun uitvoering heeft geen invloed op de gebruikersinterface of de bellen proces. Dit betekent dat het bellen proces niet de taken zijn voltooid wacht. Dit kunt u geen daarom automatisch opsporen wanneer de taak eindigt.

Als u een achtergrondtaak om te communiceren met de bellen taak om aan te geven van de voortgang of voltooiing vereist, moet u een om hiervoor implementeren. Er zijn enkele voorbeelden:

- Schrijf een waarde statusindicator met storage die toegankelijk is voor de taak UI of beller waarvoor kunt controleren of deze waarde als vereist controleren. Andere gegevens die taak op de achtergrond naar de beller terugkeren moet kunnen worden geplaatst in dezelfde opslag.
- Een antwoordenwachtrij waarop de gebruikersinterface of de beller luistert definiëren. Taak op de achtergrond kunt berichten verzenden naar de wachtrij die aangeven status en voltooid. Gegevens die taak op de achtergrond naar de beller terugkeren moet kunnen in de berichten worden geplaatst. Als u Azure Service Bus gebruikt, kunt u de eigenschappen **ReplyTo** en **CorrelationId** gebruiken voor de uitvoering van deze mogelijkheid. Zie [correlatie in Service Bus Brokered Messaging](http://www.cloudcasts.net/devguide/Default.aspx?id=13029)voor meer informatie.
- Laten zien een API of het eindpunt van de taak op de achtergrond waarop de gebruikersinterface of de beller kan statusinformatie opvragen. Gegevens die taak op de achtergrond naar de beller terugkeren moet kunnen worden opgenomen in de reactie.
- Taak op de achtergrond belt terug naar de gebruikersinterface of de beller via een API om aan te geven van de status op vooraf gedefinieerde punten of op voltooiing hebben. Dit kan zijn via gebeurtenissen lokaal of via een publiceren en abonnementen. Gegevens die taak op de achtergrond naar de beller terugkeren moet kunnen worden opgenomen in de nettolading verzoek of gebeurtenis.

## <a name="hosting-environment"></a>Hostomgeving

U kunt achtergrondtaken hosten met behulp van een reeks verschillende Azure platform services:

- [**Azure WebApps en WebJobs**](#azure-web-apps-and-webjobs). U kunt WebJobs aangepaste taken die zijn gebaseerd op een bereik van verschillende soorten scripts of uitvoerbare programma's binnen de context van een web-app uitvoeren.
- [**Rollen voor het web en werknemer van azure-Cloudservices**](#azure-cloud-services-web-and-worker-roles). U kunt code binnen een rol die wordt uitgevoerd als een achtergrondtaak schrijven.
- [**Azure virtuele Machines**](#azure-virtual-machines). Als u een Windows-service hebt of wilt gebruiken de Taakplanner van Windows, is het gebruikelijk voor het hosten van uw achtergrondtaken binnen een speciale virtuele machine.
- [**Azure Batch**](./batch/batch-technical-overview.md). Het is een platform-service die computerintensieve werk uit te voeren op een beheerde verzameling virtuele machines gepland en kan automatisch schaal berekenen resources aan de behoeften van uw taken.

De volgende secties wordt elk van deze opties uitgebreider beschreven en overwegingen waarmee u de gewenste optie kiezen opnemen.

## <a name="azure-web-apps-and-webjobs"></a>Azure WebApps en WebJobs

Azure WebJobs kunt u aangepaste taken als achtergrondtaken binnen een Azure-Web-App uitvoeren. Als een continu proces worden uitgevoerd binnen de context van uw web-app WebJobs. WebJobs ook uitvoeren in antwoord op een triggergebeurtenis van Azure Scheduler of externe factoren, zoals wijzigingen in de opslagruimte BLOB's en berichtenwachtrijen. Taken kunnen worden gestart en gestopt op aanvraag en afsluiten. Als een continu actieve WebJob mislukt, wordt deze automatisch gestart. Opnieuw en fout acties worden geconfigureerd.

Wanneer u een WebJob configureren:

- Als u wilt dat de taak om te reageren op een gebeurtenisgestuurde-trigger, moet u deze configureren als **doorlopend uitvoeren**. Het script of programma wordt opgeslagen in de map site/wwwroot/app_data/taken/continue.
- Als u wilt dat de taak om te reageren op een trigger planning op basis van hoeveelheid werk, moet u deze uitvoert **op een planning**configureren. Het script of programma wordt opgeslagen in de map met de naam site/wwwroot/app_data/taken/geactiveerd.
- Als u de optie **uitvoeren op aanvraag** selecteren wanneer u een taak configureren, worden deze dezelfde code als de optie **uitvoeren volgens een schema** uitgevoerd wanneer u het programma start.

Azure WebJobs uitgevoerd binnen de sandbox van de web-app. Dit betekent dat kunnen ze toegang omgevingsvariabelen tot en delen van gegevens, zoals verbindingsreeks met de web-app. De taak heeft toegang tot de unieke id van de computer waarop de taak wordt uitgevoerd. De verbindingsreeks met de naam **AzureWebJobsStorage** biedt toegang tot Azure opslag wachtrijen, BLOB's en tabellen voor de toepassingsgegevens en toegang tot de Service Bus voor messaging en communicatie. De verbindingsreeks met de naam **AzureWebJobsDashboard** biedt toegang tot de logboekbestanden van de taak actie.

Azure WebJobs bestaan uit de volgende kenmerken:

- **Beveiliging**: WebJobs zijn beveiligd met de referenties van de implementatie van de web-app.
- **Ondersteunde bestandstypen**: U kunt WebJobs definiëren met behulp van de opdrachtscripts (cmd), de batch-bestanden (type), de PowerShell-scripts (.ps1), bash shell-scripts (.sh), PHP scripts (.php), Python scripts (.py), JavaScript-code (js) en uitvoerbare programma's (.exe, .jar en meer).
- **Implementatie**: U kunt de scripts en uitvoerbare bestanden implementeren met behulp van de Azure-portal via de invoegtoepassing [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) voor Visual Studio of [Visual Studio 2013 Update 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (u kunt maken en implementeren met deze optie), met behulp van de [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)of door deze te kopiëren rechtstreeks naar de volgende locaties:
  - Geactiveerd voor uitvoering: site/wwwroot/app_data/taken/geactiveerd / {taak naam}
  - Voor doorlopende uitvoering: site/wwwroot/app_data/taken/continue / {taak naam}
- **Logboekregistratie**: Console.Out (gemarkeerde) wordt behandeld als INFO. Console.Error wordt behandeld als fout. U kunt de cmdlets voor controle en diagnostische gegevens weer met behulp van de Azure-portal. U kunt de logboekbestanden downloaden rechtstreeks vanaf de site. Ze zijn opgeslagen in de volgende locaties:
  - Geactiveerd voor uitvoering: Vfs/gegevens/taken/geactiveerd/taaknaam
  - Voor doorlopende uitvoering: Vfs/gegevens/taken/continue/taaknaam
- **Configuratie**: U kunt WebJobs configureren met behulp van de portal, de REST API en PowerShell. U kunt een configuratiebestand met de naam settings.job in dezelfde map als de taak-script hoofdsite configuratie-informatie voor een taak te verstrekken. Bijvoorbeeld:
  - {"stopping_wait_time": 60}
  - {"is_singleton": waar}

### <a name="considerations"></a>Overwegingen

- Standaard WebJobs schaal met de web-app. U kunt echter taken uit te voeren op één exemplaar met de eigenschap van de configuratie **is_singleton** op **true**. Één exemplaar WebJobs zijn handig voor taken die u niet wilt schalen of uitvoeren als tegelijk meerdere exemplaren, zoals opnieuw indexeren, gegevensanalyse en vergelijkbare taken.
- Als u wilt minimaliseren wat de invloed van taken op de prestaties van de web-app, door een lege Azure Web App-exemplaar maken in een nieuwe App-serviceplan naar host WebJobs die mogelijk lang uitgevoerd of resource intensief te overwegen.

### <a name="more-information"></a>Meer informatie

- [Azure WebJobs aanbevolen resources](./app-service-web/websites-webjobs-resources.md) bevat de veel nuttige informatiebronnen, downloads en voorbeelden voor WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloud Services-rollen voor web en werknemer

U kunt achtergrondtaken binnen een Webrol of in een afzonderlijk werknemer rol uitvoeren. Wanneer u zijn bepalen of u kunt een rol werknemer gebruiken, kunt u schaalbaarheid en elasticiteit vereisten, levensduur van de taak, laat u cadence, beveiliging, fouttolerantie, conflict, complexiteit en de logische architectuur. Zie voor meer informatie [Berekenen Resource samenvoeging patroon](http://msdn.microsoft.com/library/dn589778.aspx).

Er zijn verschillende manieren willen implementeren achtergrondtaken binnen een rol Cloud Services:

- Een implementatie van de klas **RoleEntryPoint** maken in de rol en de methoden gebruiken om uit te voeren achtergrondtaken. De taken uitgevoerd in de context van WaIISHost.exe. Zij kunnen de methode **GetSetting** van de klasse **CloudConfigurationManager** gebruiken voor het laden van configuratie-instellingen. Zie [levenscyclus (Cloudservices)](#lifecycle-cloud-services)voor meer informatie.
- Gebruik opstarttaken achtergrondtaken uitvoeren wanneer de toepassing wordt gestart. Als u wilt dat de taken te voeren op de achtergrond, stelt u de eigenschap **taskType** in op **achtergrond** (als u niet doet dit, het proces van toepassing opstarten wordt stoppen en wacht totdat de taak eindigt). Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.
- Gebruik de WebJobs SDK willen implementeren achtergrondtaken zoals WebJobs die worden gestart als een taak opstarten. Zie [aan de slag met de SDK van Azure WebJobs](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)voor meer informatie.
- Een taak opstarten gebruiken voor het installeren van een Windows-service die wordt uitgevoerd van een of meer achtergrondtaken. Zodat de service wordt uitgevoerd op de achtergrond, moet u de eigenschap **taskType** als **achtergrond** instellen. Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.

### <a name="running-background-tasks-in-the-web-role"></a>Actieve achtergrondtaken in de Webrol

Het belangrijkste voordeel van het uitvoeren van achtergrondtaken in de Webrol is het opslaan in de kosten hostingprovider omdat er geen vereiste extra rollen implementeren.

### <a name="running-background-tasks-in-a-worker-role"></a>Achtergrondtaken uitgevoerd in een rol werknemer

Achtergrondtaken uitgevoerd in een rol werknemer kent diverse voordelen:

- Dit kunt u voor het beheren van schaalbaarheid afzonderlijk voor elk type rol. Bijvoorbeeld, moet u mogelijk meer exemplaren van een Webrol ter ondersteuning van de huidige belasting, maar minder exemplaren van de rol van de werknemer die wordt uitgevoerd achtergrondtaken. Schaalbaarheid achtergrond taak berekeningscluster exemplaren afzonderlijk van de rollen UI, kunt u hostingprovider kosten, behoud acceptabel beperken.
- Deze compatibele de verwerking van belasting voor achtergrondtaken uit de Webrol. De Webrol waarmee de gebruikersinterface kunt blijven heeft gereageerd en deze kan betekenen dat minder exemplaren vereist zijn ter ondersteuning van een bepaalde hoeveelheid aanmeldingsaanvragen van gebruikers.
- U kunt implementeren scheiding van problemen. Elk Roltype kan een specifieke reeks duidelijk gedefinieerd en verwante taken kunt implementeren. Hierdoor ontwerpen en onderhouden van de code eenvoudiger omdat er minder onderlinge afhankelijkheid van code en functionaliteit tussen elke rol.
- Kan helpen deze isoleren gevoelige processen en gegevens. Web rollen die implementeren van de gebruikersinterface hoeft bijvoorbeeld niet toegang heeft tot gegevens dat wordt beheerd en beheerd door een rol werknemer. Dit is handig in betere beveiliging, met name als u een patroon zoals het [Gatekeeper patroon](http://msdn.microsoft.com/library/dn589793.aspx).  

### <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het kiezen van hoe en waar achtergrondtaken implementeren wanneer u rollen voor het web en werknemer van Cloud Services gebruikt:

- Achtergrondtaken in een bestaande Webrol hostingprovider kan de kosten van het uitvoeren van een rol afzonderlijke werknemer alleen betrekking heeft op deze taken worden opgeslagen. Het is echter waarschijnlijk van invloed zijn op de prestaties en beschikbaarheid van de toepassing als er conflicten voor verwerking en andere resources. Via een rol afzonderlijke werknemer beschermen de Webrol tegen het effect van langdurige of systeembronnen achtergrondtaken.
- Als u achtergrondtaken met behulp van de **RoleEntryPoint** klasse host, kunt u dit eenvoudig verplaatsen naar een andere rol. Als u de klas in een Webrol maken en later besluit dat u moet de taken worden uitgevoerd in een rol werknemer, kunt u bijvoorbeeld de implementatie van de klasse **RoleEntryPoint** verplaatsen naar de rol werknemer.
- Opstarttaken zijn ontworpen om een programma of een script uitvoeren. Een achtergrondtaak als een uitvoerbaar programma implementeert mogelijk moeilijker, vooral als er ook implementatie van afhankelijke stroombaan moeten zijn. Het is mogelijk beter te implementeren en gebruiken van een script definiëren van een achtergrondtaak wanneer u opstarttaken gebruikt.
- Uitzonderingen die ertoe leiden dat een achtergrondtaak mislukt bestaan uit een ander effect, afhankelijk van de manier waarop deze worden gehost:
  - Als u de **RoleEntryPoint** klasse-methode gebruikt, treedt een mislukte taak de rol opnieuw opstarten zodat de taak wordt automatisch gestart. Dit kan van invloed zijn op beschikbaarheid van de toepassing. Zorg ervoor dat u in de klas **RoleEntryPoint** en de achtergrondtaken de verwerking van krachtige uitzonderingen opnemen om te voorkomen dat dit. Code gebruiken om taken die waar dit nodig is mislukt opnieuw te starten en de uitzondering om de rol opnieuw alleen als u niet zonder problemen van de fout bij van uw code herstellen te genereren.
  - Als u opstarttaken gebruikt, bent u verantwoordelijk voor het beheren van de uitvoering van de taken en in te schakelen als dat niet werkt.
- Beheren en controleren van opstarttaken is moeilijker dan het gebruik van de **RoleEntryPoint** klasse-methode. De SDK van Azure WebJobs bevat echter een dashboard voor het beheren van WebJobs dat u begint uw tot en met opstarttaken te vereenvoudigen.

### <a name="more-information"></a>Meer informatie

- [Resource samenvoeging patroon berekenen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Aan de slag met de SDK van Azure WebJobs](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure virtuele Machines

Achtergrondtaken mogelijk worden geïmplementeerd op een manier die voorkomt dat deze wordt geïmplementeerd op Azure Web Apps of Cloudservices of deze opties mogelijk niet gemakkelijk. Typische voorbeelden zijn Windows-services, en hulpprogramma's van derden en uitvoerbare programma's. Een ander voorbeeld mogelijk programma's voor een omgeving die is anders dan die als host voor de toepassing zijn geschreven. Het kan bijvoorbeeld een Unix- of Linux-programma dat u wilt uitvoeren vanuit een Windows- of .NET-toepassing zijn. U kunt kiezen uit een bereik van besturingssystemen voor een Azure virtuele machine en uw service of het uitvoerbare bestand worden uitgevoerd op die virtuele machine.

Zie [Azure App Services, Cloudservices en virtuele Machines vergelijking](./app-service-web/choose-web-site-cloud-service-vm.md)om te kiezen wanneer u het gebruik van virtuele Machines. Zie voor informatie over de opties voor virtuele Machines [virtuele Machine en Cloudservice grootten voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Zie voor meer informatie over de besturingssystemen en de vooraf gedefinieerde afbeeldingen die beschikbaar voor virtuele Machines zijn, [Azure virtuele Machines Marketplace](https://azure.microsoft.com/gallery/virtual-machines/).

Als u wilt starten taak op de achtergrond in een afzonderlijk virtuele machine, hebt u een reeks opties:

- U kunt de taak uitvoeren op aanvraag rechtstreeks vanuit uw toepassing door een aanvraag verzonden naar een eindpunt dat toegang biedt tot de taak. Hiermee wordt doorgegeven in alle gegevens die de taak is vereist. Dit eindpunt Hiermee wordt de taak.
- U kunt de taak uit te voeren op een planning met behulp van een scheduler of timer die beschikbaar is in het door u gekozen besturingssysteem configureren. Bijvoorbeeld op Windows kunt u Windows Taakplanner uitvoeren van scripts en taken. Of, als u SQL Server geïnstalleerd op de virtuele machine hebt, kunt u de SQL Server-Agent uitvoeren van scripts en taken.
- U kunt Azure Scheduler gebruiken om de taak door een bericht toevoegen aan een wachtrij waarop de taak luistert of door een nieuw vergaderverzoek verzenden naar een API dat toegang biedt tot de taak.

Zie het gedeelte [Triggers](#triggers) voor meer informatie over hoe u achtergrondtaken kunt starten.  

### <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten wanneer u of u wilt implementeren achtergrondtaken in een Azure virtuele machines zijn bepalen:

- Achtergrondtaken in een afzonderlijk Azure virtuele machine hostingprovider biedt flexibiliteit en controle over de beginfase, worden uitgevoerd, planning en resourcetoewijzing toestaat. Deze worden echter runtime kosten verhogen als een virtuele machine moeten worden geïmplementeerd NET om uit te voeren achtergrondtaken.
- Er is geen faciliteit om de taken in de portal van Azure en geen geautomatiseerd opnieuw starten de mogelijkheid voor mislukte taken--de Hoewel u kunt de eenvoudige status van de virtuele machine controleren en deze te beheren met behulp van de [Cmdlets voor gebruikersbeheer van Azure-Service](http://msdn.microsoft.com/library/azure/dn495240.aspx)te houden. Er zijn echter geen faciliteiten om te bepalen processen en threads in berekeningscluster knooppunten. Gebruik van een virtuele machine moeten meestal extra hoeveelheid willen implementeren van een om waarmee gegevens worden verzameld uit instrumentation in de taak en uit het besturingssysteem in de virtuele machine. Een oplossing die thuishoren is via het [Systeem Center Management Pack voor Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Kunt u overwegen controleren sondes die worden weergegeven via HTTP-eindpunten maken. De code voor deze sondes kan systeemstatus controles uitvoeren, verzamelen gebruiksgegevens en statistieken worden aangepast-- of foutgegevens sorteren en een toepassing herstellen. Zie voor meer informatie [Systeemstatus eindpunt Monitoring patroon](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Meer informatie

- [Virtuele Machines](https://azure.microsoft.com/services/virtual-machines/) op Azure
- [Veelgestelde vragen over Azure virtuele Machines](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Ontwerpoverwegingen

Er zijn verschillende fundamentele factoren u rekening moet houden bij het ontwerpen van achtergrondtaken. De volgende secties worden partitioneren, conflicten en afhankelijk.

## <a name="partitioning"></a>Partitioneren

Als u besluit om op te nemen achtergrondtaken binnen een bestaand berekeningscluster-exemplaar (zoals een WebApp, Webrol, bestaande werknemer rol of VM), moet u bepalen hoe dit heeft invloed op de kenmerken van de kwaliteit van het exemplaar berekenen en de achtergrondtaak zelf. Deze factoren kunt u bepalen of de taken met de bestaande berekeningscluster instantie colocate of Scheid deze in een afzonderlijk berekeningscluster exemplaar:

- **Beschikbaarheid**: achtergrondtaken mogelijk niet moeten zijn hetzelfde niveau van beschikbaarheid als andere delen van de toepassing, met name de gebruikersinterface en andere onderdelen rechtstreeks aanstaande interactie van de gebruiker. Achtergrondtaken mogelijk handiger van latentie, opnieuw geprobeerde verbindingsfouten, en andere factoren die van invloed zijn op beschikbaarheid omdat de bewerkingen kunnen wordt geplaatst. Echter moet er voldoende capaciteit om te voorkomen dat de back-up van aanvragen die kan wachtrijen blokkeren en invloed op de toepassing als geheel.
- **Schaalbaarheid**: achtergrondtaken kunnen hebben een vereiste verschillende schaalbaarheid dan de gebruikersinterface en de interactieve onderdelen van de toepassing zijn. Schaalbaarheid van de gebruikersinterface kan het nodig zijn om te voldoen aan de pieken in de vraag, terwijl uitstaande achtergrondtaken tijdens minder beschikbaarheidsinfo mogelijk worden uitgevoerd door een kleiner aantal exemplaren van berekeningscluster.
- **Tolerantie**: mislukken van een berekeningscluster exemplaar waarop alleen achtergrondtaken kan niet fatally invloed zijn op de toepassing als geheel als de verzoeken om deze taken kunnen worden in de wachtrij of uitgesteld totdat de taak weer beschikbaar is. Als de berekeningscluster exemplaar en/of de taken kunnen opnieuw worden gestart binnen een juiste interval, kunnen gebruikers van de toepassing niet worden beïnvloed.
- **Beveiliging**: achtergrondtaken mogelijk verschillende beveiligingsvereisten die zijn of beperkingen dan de gebruikersinterface of andere delen van de toepassing. Met behulp van een exemplaar afzonderlijk berekenen, kunt u een ander beveiligingsomgeving voor de taken opgeven. U kunt ook patronen zoals Gatekeeper te isoleren van de achtergrond berekeningscluster exemplaren van de gebruikersinterface om te maximaliseren beveiligings- en scheiding.
- **Prestaties**: U kunt het type van berekeningscluster exemplaar voor achtergrond taken die kunnen worden specifiek overeenkomst de vereisten van de prestaties van de taken. Dit kan betekenen met behulp van de optie van een minder dure berekenen als de taken niet dezelfde verwerking voorzieningen als de gebruikersinterface of een grotere exemplaar hoeven als ze extra capaciteit en resources vereisen.
- **Beheerbaarheid**: achtergrondtaken mogelijk een ander ontwikkeling en implementatie ritme vanuit het hoofdvenster van de toepassing-code of de gebruikersinterface. Deze op een aparte berekeningscluster exemplaar implementeert, kunt u updates en versiebeheer vereenvoudigen.
- **Kosten**: exemplaren uitvoeren achtergrond taken toeneemt hostingprovider kosten toe te voegen berekenen. Overweeg de verhouding tussen extra capaciteit en deze extra kosten.

Zie [Opvulteken Verkiezingsuitslagen patroon](http://msdn.microsoft.com/library/dn568104.aspx) en [Consumenten patroon dat](http://msdn.microsoft.com/library/dn568101.aspx)voor meer informatie.

## <a name="conflicts"></a>Conflicten

Als u meerdere exemplaren van een achtergrondtaak hebt, is het mogelijk dat ze worden geconfigureerd voor toegang tot bronnen en -services, zoals databases en opslag. Dit kan resulteren in bronnen wordt geminimaliseerd, waardoor conflicten in de beschikbaarheid van de services en in de integriteit van gegevens in opslag. U kunt bronnen wordt geminimaliseerd oplossen met behulp van een benadering van pessimistische vergrendelen. Hiermee voorkomt u dat dat exemplaren van een taak van gelijktijdig toegang krijgen tot een service of beschadigen van gegevens.

Een andere methode voor het oplossen van conflicten is achtergrondtaken instelt als een singleton, zodat er ooit slechts één exemplaar uitgevoerd. Hierdoor wordt echter de betrouwbaarheid en prestaties voordelen dat een veelvoud-exemplaar configuratie kan leveren. Dit geldt met name als de gebruikersinterface voldoende werk als u wilt meer dan één achtergrondtaak bezet houden kunt opgeven.

Het is belangrijk om ervoor te zorgen dat de achtergrondtaak automatisch opnieuw starten kunt en dat er voldoende capaciteit omgaan met pieken in de vraag. Dit doet u door het toewijzen van een berekeningscluster exemplaar met voldoende resources, door te implementeren wachtrij functie waarmee verzoeken om later kunt uitvoeren als de aanvraag afneemt kunt opslaan, of met behulp van een combinatie van de volgende manieren.

## <a name="coordination"></a>Afhankelijk

De achtergrondtaken mogelijk complexe en kunnen het nodig zijn meerdere individuele taken uitvoeren om een resultaat te bereiken of om te voldoen aan alle vereisten. In deze scenario's aan de taak splitsen in kleinere onopvallende stappen of subtaken die kunnen worden uitgevoerd door meerdere consumenten gelijke waarden is. Groot aantal stappen taken kan efficiënter en flexibeler zijn, omdat het is mogelijk dat afzonderlijke stappen herbruikbare in meerdere taken. Het is ook eenvoudig toevoegen, verwijderen of de volgorde van de stappen wijzigen.

Meerdere taken en stappen coördineren kan lastig zijn, maar er zijn drie algemene patronen die u gebruiken kunt om te helpen uw implementatie van een oplossing:

- **Een taak in meerdere herbruikbare stappen ontleedt**. Een toepassing nodig zijn om het uitvoeren van tal van taken van verschillende complexiteit van de informatie die wordt verwerkt. Een eenvoudige, maar inflexibele aanpak voor de uitvoering van deze toepassing mogelijk deze processen als een monolithisch module uitvoeren. Deze benadering is echter waarschijnlijk het verkleinen van de mogelijkheden voor de code refactoring, zodat deze wordt geoptimaliseerd of deze hergebruiken als u onderdelen van de verwerking van dezelfde nodig ergens anders in de toepassing. Zie [Pipes en Filters patroon](http://msdn.microsoft.com/library/dn568100.aspx)voor meer informatie.
- **Beheer van de uitvoering van de stappen voor een taak**. Een toepassing mogelijk taken uitvoeren die bestaan uit een aantal stappen (sommige mogelijk aanroepen van externe services of gebruikmaken van externe bronnen). Het is mogelijk dat de afzonderlijke stappen onafhankelijk van elkaar, maar ze zijn ingedeeld door de toepassingslogica waarmee de taak. Zie voor meer informatie [Scheduler Agent toezichthouder patroon](http://msdn.microsoft.com/library/dn589780.aspx).
- **Herstel voor taakstappen die mislukt beheren**. Een toepassing moet mogelijk ongedaan maken van het werk dat wordt uitgevoerd door een reeks stappen (die samen een uiteindelijk consistente bewerking) als er een of meer van de stappen mislukt. Zie voor meer informatie [Verwacht transactie patroon](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Levenscyclus (Cloudservices)

 Als u besluit willen implementeren achtergrondtaken voor Cloud Services-toepassingen die gebruikmaken van web en werknemer rollen met behulp van de klas **RoleEntryPoint** , is het is belangrijk is voor meer informatie over de levenscyclus van deze klasse pas deze correct gebruiken.

Web en werknemer rollen leest u over een reeks verschillende fasen terwijl ze begindatum, uitvoeren en stoppen. De klasse **RoleEntryPoint** wordt een reeks gebeurtenissen die aangeven wanneer deze fasen zijn optreedt. U gebruikt deze voor geïnitialiseerd, uitvoeren, en uw aangepaste achtergrondtaken stoppen. De volledige cyclus luidt als volgt:

- Azure de rol constructie geladen en zoekt u deze naar een klasse dat is afgeleid van **RoleEntryPoint**.
- Als deze klasse worden gevonden, wordt aangeroepen **RoleEntryPoint.OnStart()**. U negeren deze methode als u wilt uw achtergrondtaken geïnitialiseerd.
- Nadat de methode **OnStart** is voltooid, presenteren Azure oproepen **Application_Start()** in van de toepassing globaal bestand als dit is (bijvoorbeeld Global.asax in een Webrol waarop ASP.NET wordt uitgevoerd).
- Azure- **RoleEntryPoint.Run()** oproepen op een nieuwe voorgrond thread die wordt uitgevoerd in combinatie met **OnStart()**. U negeren deze methode voor het starten van uw achtergrondtaken.
- Wanneer de methode Run eindigt, roept Azure eerst **Application_End()** in van de toepassing globaal bestand als dit aanwezig is, en wordt vervolgens **RoleEntryPoint.OnStop()**. U Negeer de methode **OnStop** uw achtergrondtaken stoppen, resources opschonen buitengebruikstelling van objecten en sluit verbindingen die de taken hebt opgegeven.
- Hostproces voor de rol van de Azure werknemer is gestopt. Op dit moment wordt de rol gerecycled en opnieuw wordt gestart.

Zie voor meer informatie en een voorbeeld van het gebruik van de methoden van de klasse **RoleEntryPoint** , [Berekenen Resource samenvoeging patroon](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten wanneer u van plan bent hoe u achtergrondtaken wordt uitgevoerd in een rol web of werknemer:

- De standaard **uitvoeren** methode-implementatie in de klas **RoleEntryPoint** bevat een oproep naar **Thread.Sleep(Timeout.Infinite)** die de rol voor onbepaalde tijd leven bewaart. Als u de methode **Run** (dit is meestal nodig achtergrondtaken uitvoeren) negeren, moet u uw code om af te sluiten van de methode, tenzij u wilt het exemplaar van de rol Prullenbak niet toestaan.
- Een typische implementatie van de methode **Run** bevat starten elk van de achtergrondtaken en een lus constructie die geregeld gecontroleerd of de status van alle achtergrondtaken-code. Dit kunt die mislukt of annulering tokens die aangeven dat taken hebt voltooid controleren opnieuw starten.
- Als een achtergrondtaak een onverwerkte uitzondering genereert, moet deze taak zijn gerecycled terwijl andere achtergrondtaken in de rol worden uitgevoerd. Echter de uitzondering wordt veroorzaakt door beschadigde bestanden van objecten buiten de taak, zoals gedeelde opslag, de uitzondering moet worden verwerkt door uw klas **RoleEntryPoint** , alle taken moeten worden geannuleerd als de methode **Run** moet kunnen beëindigen. Azure wordt Start de rol.
- Gebruik de methode **OnStop** onderbreken of achtergrondtaken beëindigen en opschonen van resources. Dit mogelijk gebruikmaakt van langdurige of dergelijk complex taken stoppen. Houd rekening met hoe u dit kunt doen om te voorkomen dat gegevensinconsistenties noodzakelijk is. Als een exemplaar van de rol om welke reden dan een door de gebruiker geactiveerde afsluiten stopt, moet de code die wordt uitgevoerd in de methode **OnStop** worden uitgevoerd binnen vijf minuten voordat deze is beëindigd. Zorg ervoor dat uw code in die tijd kan worden voltooid of niet uitgevoerd tot voltooiing mogelijk zonder.  
- Verdeling van de Azure belasting wordt gestart waarmee verkeer in het exemplaar van de rol wanneer de methode **RoleEntryPoint.OnStart** , de waarde **true retourneert**. Overweeg daarom onderbrengen van alle initialisatiecode in de methode **OnStart** zodat rol-exemplaren die is niet geïnitialiseerd geen al het verkeer ontvangt.
- U kunt opstarttaken naast de methoden van de klasse **RoleEntryPoint** gebruiken. U kunt opstarttaken geïnitialiseerd instellingen die u wijzigen in de Azure taakverdeling wilt omdat deze taken worden uitgevoerd voordat de rol aanvragen ontvangt. Zie [opstarttaken in Azure uitvoeren](./cloud-services/cloud-services-startup-tasks.md)voor meer informatie.
- Als er een fout in een taak opstarten, kan deze de rol continu opnieuw afdwingen. Dit kan voorkomen dat u uit de uitvoering van een virtuele IP-(VIP) adres omwisselen terug naar een eerder gefaseerde versie, omdat het omwisselen exclusieve toegang tot de rol vereist. Dit kan niet worden verkregen tijdens het opstarten van de rol. Voor het oplossen van dit:
    -  De volgende code toevoegen aan het begin van de methoden **OnStart** en **uitvoeren** in uw rol:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - De definitie van de instelling **blokkeren** toevoegen als een Booleaanse waarde aan de ServiceDefinition.csdef en ServiceConfiguration. *.cscfg bestanden voor de rol en stel deze in op* *Onwaar* *. Als de rol in de modus van een herhaalde opnieuw starten gaat, kunt u de instelling om te wijzigen * *waar** blokkeert rol kan worden uitgevoerd en kunnen worden verwijderd met een eerdere versie.

## <a name="resiliency-considerations"></a>Aandachtspunten voor de tolerantie

Achtergrondtaken moet robuuste kan alleen worden aangeboden betrouwbare services met de toepassing. Wanneer u plannen en ontwerpen van achtergrondtaken, kunt u overwegen de volgende punten:

- Achtergrondtaken moet kunnen worden verwerkt rol of service opnieuw is opgestart zonder gegevens te beschadigen of inleiding tot inconsistenties in de toepassing. Voor langdurige of dergelijk complex taken, kunt u overwegen _wijzen controleren_ door op te slaan de status van taken in permanente opslag, of als berichten in een wachtrij als dit nodig is. U kunt bijvoorbeeld staat informatie in een bericht in een wachtrij behouden en stapsgewijs deze informatie over de status bijwerken met de voortgang van taken, zodat de taak kan worden verwerkt in het laatste bekende goede controlepunt--in plaats van moet opnieuw worden gestart vanaf het begin. Wanneer u een Azure Service Bus wachtrijen gebruikt, kunt u bericht sessies hetzelfde scenario inschakelen. Sessies kunnen u opslaan en de status van de verwerking van toepassing op te halen met behulp van de methoden [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) en [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Zie voor meer informatie over het ontwerpen van betrouwbare dergelijk complex processen en werkstromen, [Scheduler Agent toezichthouder patroon](http://msdn.microsoft.com/library/dn589780.aspx).
- Als u Internet of een werknemer rollen gebruikt voor het hosten van meerdere achtergrondtaken, ontwerp uw negeren van de methode **Run** mislukte of vastgelopen taken controleren en deze opnieuw te starten. Waar dit niet praktisch haalbaar is en u een rol werknemer gebruikt, ervoor zorgen dat de rol werknemer opnieuw starten van de methode **Run** af te sluiten.
- Wanneer u wachtrijen gebruiken om te communiceren met achtergrondtaken, de wachtrijen kunnen fungeert als buffer voor de opslag van aanvragen die zijn verzonden naar de taken die tijdens de toepassing is onder hoger zijn dan gebruikelijke laden. Hiermee kunt de taken achterhalen de gebruikersinterface minder druk bezet perioden. Ook betekent dat de rol hergebruik niet de gebruikersinterface blokkeert. Zie voor meer informatie, [op basis van wachtrijen laden herverdelen patroon](http://msdn.microsoft.com/library/dn589783.aspx). Als bepaalde taken belangrijker is dan andere zijn, kunt u de [Prioriteit wachtrij patroon](http://msdn.microsoft.com/library/dn589794.aspx) om ervoor te zorgen dat deze taken worden uitgevoerd voordat u minder belangrijk implementeren.
- Achtergrondtaken die zijn gestart door berichten of berichten van proces moeten zijn ontworpen worden afgehandeld inconsistenties, zoals berichten binnengekomen volgorde, berichten die net zo vaak ertoe leiden dat een fout (vaak _poison berichten_genoemd) en berichten die meer dan één keer worden bezorgd. Het volgende overwegen:
  - Berichten die moeten worden verwerkt in een specifieke volgorde, zoals modellen die gegevens op basis van de bestaande gegevenswaarde (bijvoorbeeld een waarde toevoegen aan een bestaande waarde) wijzigen, mogelijk niet komen terecht in de oorspronkelijke volgorde waarin ze zijn verzonden. Ze kunnen ook worden verwerkt door verschillende exemplaren van een achtergrondtaak in een andere volgorde vanwege verschillende laadtijd op elk exemplaar. Berichten die moeten worden verwerkt in een specifieke volgorde moeten bevatten een volgnummer, sleutel of enkele andere indicator die achtergrondtaken gebruiken kunnen om ervoor te zorgen dat ze worden verwerkt in de juiste volgorde. Als u Azure Service Bus gebruikt, kunt u bericht sessies gebruiken om te garanderen de volgorde van bezorging. Het is echter meestal handiger, indien mogelijk, voor het ontwerpen van het proces, zodat de berichtvolgorde niet belangrijk is.
  - Een achtergrondtaak wordt meestal weergeven van alle berichten in de wachtrij tijdelijk te van andere consumenten bericht verbergen. Vervolgens celbewerkingsmodus verwijdert u hiermee de berichten nadat ze met succes zijn verwerkt. Als een achtergrondtaak mislukt tijdens het verwerken van een bericht, verschijnt dat bericht opnieuw in de wachtrij als de korte weergave time-out is verlopen. Wordt verwerkt door een ander exemplaar van de taak of tijdens de volgende verwerking cyclus van dit exemplaar. Als het bericht consistente treedt er een fout in de gebruiker, wordt deze de taak, de wachtrij en uiteindelijk de toepassing zelf geblokkeerd als de wachtrij vol raakt. Daarom essentieel detecteren en poison berichten verwijderen uit de wachtrij. Als u van Azure Service Bus gebruikmaakt, kunnen berichten die ertoe leiden dat een fout worden verplaatst automatisch of handmatig aan een wachtrij gekoppeld onbestelbare.
  - Wachtrijen gegarandeerd bij _minste eenmaal_ bezorging regelingen, maar ze meer dan één keer, hetzelfde bericht kunnen leiden. Bovendien als een achtergrondtaak mislukt na het verwerken van een bericht, maar voordat deze wordt verwijderd uit de wachtrij, binnenkort het bericht beschikbaar voor het verwerken van opnieuw. Achtergrondtaken moet idempotency is ingeschakeld, wat betekent dat het verwerken van hetzelfde bericht meerdere malen geen een fout of inconsistenties in de gegevens van de toepassing veroorzaakt. Sommige bewerkingen zijn natuurlijk idempotency is ingeschakeld, zoals een opgeslagen waarde instellen op een specifieke nieuwe waarde. Bewerkingen uitvoeren zoals een waarde toevoegen aan een bestaande opgeslagen waarde zonder het controleren van de opgeslagen waarde is nog steeds hetzelfde als wanneer het bericht oorspronkelijk is verzonden treedt echter inconsistenties. Azure-Service Bus wachtrijen kunnen worden geconfigureerd individuele berichten automatisch te verwijderen.
  - Een eigenschap voor het tellen van de-queue waarmee wordt aangegeven hoe vaak die een bericht is gelezen uit de wachtrij wordt ondersteund door sommige SMS systemen, zoals Azure opslag wachtrijen en Azure Service Bus wachtrijen. Dit is handig in de verwerking van herhaalde en poison berichten. Zie [Asynchrone Messaging handleiding](http://msdn.microsoft.com/library/dn589781.aspx) en [Idempotency patronen](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/)voor meer informatie.

## <a name="scaling-and-performance-considerations"></a>Aandachtspunten voor de schaalbaarheid en prestaties

Achtergrondtaken moeten bieden voldoende prestaties om ervoor te zorgen ze niet de toepassing blokkeren of inconsistent vanwege vertraagde bewerking wanneer het systeem onder laden is. Meestal is verbeterd door schaalbaarheid van de berekeningscluster-exemplaren die als de achtergrondtaken host. Wanneer u plannen en ontwerpen van achtergrondtaken, kunt u overwegen de volgende punten rond schaalbaarheid en prestaties:

- Azure ondersteunt autoscaling (schalen en schaling terug in) op basis van huidige aanvraag en laden-- of op een vooraf gedefinieerde planning, Web-Apps voor Cloudservices web en werknemer rollen en virtuele Machines die worden gehost implementaties. Deze functie gebruiken om ervoor te zorgen dat de toepassing als geheel beschikt over prestatiemogelijkheden voor voldoende minimum aan runtime kosten.
- Wanneer achtergrondtaken een verschillende prestaties van de andere gedeelten van een Cloud Services-toepassing (bijvoorbeeld de UI of onderdelen zoals de data access-laag), kan de achtergrondtaken samen in een rol afzonderlijke werknemer hostingprovider de gebruikersinterface en achtergrond taak rollen wilt schalen onafhankelijk voor het beheren van het selectievakje laden. Als meerdere achtergrondtaken sterk afwijkt prestatiemogelijkheden van elkaar verschillen hebt, kunt u ze verdelen in afzonderlijke werknemer rollen en onafhankelijk schaalbaarheid van elk Roltype. Bedenk wel dat dit runtime kosten vergeleken met het combineren van alle taken in minder rollen verhogen.
- Gewoon schaalbaarheid van de rollen mogelijk niet voldoende om te voorkomen dat verlies van de prestaties met laden. Mogelijk moet u ook schalen wachtrijen voor opslagruimte en andere resources om te voorkomen dat een potentieel in het totale processing ketting niet langer een vertraging veroorzaken. U kunt wellicht ook andere tekortkomingen, zoals de maximumdoorvoer van opslag en andere services waarvoor de aanvraag en de achtergrondtaken.
- Achtergrondtaken moeten zijn ontworpen voor schaalbaarheid. Bijvoorbeeld moet ze kunnen dynamisch het aantal opslag wachtrijen wordt gebruikt om te trekken of berichten verzenden naar de juiste wachtrij te detecteren.
- Standaard WebJobs schalen met hun bijbehorende Azure Web Apps-exemplaar. Als u een WebJob om uit te voeren als slechts één exemplaar wilt, kunt u echter een Settings.job-bestand dat de JSON-gegevens bevat maken **{"is_singleton": waar}**. Hiermee dwingt Azure pas uit te voeren één exemplaar van de WebJob, zelfs als er meerdere exemplaren van de bijbehorende WebApp. Dit is een handige techniek voor geplande taken die u als slechts één exemplaar uitvoeren moet.

## <a name="related-patterns"></a>Gerelateerde patronen

- [Asynchroon SMS handleiding](http://msdn.microsoft.com/library/dn589781.aspx)
- [AutoScaling richtlijnen](http://msdn.microsoft.com/library/dn589774.aspx)
- [Transactie patroon verwacht](http://msdn.microsoft.com/library/dn589804.aspx)
- [Concurrerende consumenten patroon](http://msdn.microsoft.com/library/dn568101.aspx)
- [Richtlijnen partitioneren berekenen](http://msdn.microsoft.com/library/dn589773.aspx)
- [Resource samenvoeging patroon berekenen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeper patroon](http://msdn.microsoft.com/library/dn589793.aspx)
- [Opvulteken Verkiezingsuitslagen patroon](http://msdn.microsoft.com/library/dn568104.aspx)
- [Pipes en Filters patroon](http://msdn.microsoft.com/library/dn568100.aspx)
- [Prioriteit wachtrij patroon](http://msdn.microsoft.com/library/dn589794.aspx)
- [Wachtrij gebaseerde laden patroon effenen](http://msdn.microsoft.com/library/dn589783.aspx)
- [Scheduler Agent toezichthouder patroon](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Meer informatie

- [Schaalbaarheid van Azure toepassingen met de rol van werknemer](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Achtergrondtaken uitvoeren](http://msdn.microsoft.com/library/ff803365.aspx)
- [De levenscyclus van Azure rol opstarten](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (blogbericht)
- [Azure Cloud Services rol Lifecycle](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (video)
- [Aan de slag met de Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure wachtrijen en Service Bus wachtrijen - vergeleken en in afgezet tegen](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Het inschakelen van diagnostische gegevens in een Cloudservice](./cloud-services/cloud-services-dotnet-diagnostics.md)
