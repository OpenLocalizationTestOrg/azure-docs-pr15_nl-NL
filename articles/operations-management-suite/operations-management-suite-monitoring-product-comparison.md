<properties 
   pageTitle="Microsoft Productvergelijking monitoring | Microsoft Azure"
   description="Microsoft bewerkingen Management Suite Kantoorbeheersysteem is van Microsoft cloud op basis van IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur.  In dit artikel worden de verschillende services van opgenomen in OMS geïdentificeerd en bevat koppelingen naar hun gedetailleerde inhoud."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft controle-vergelijking

In dit artikel bevat een vergelijking tussen systeem Center Operations Manager (SCOM) en Log Analytics in bewerkingen Management Suite Kantoorbeheersysteem hun architectuur, de logica van hoe ze resources controleren en hoe ze analyse van de gegevens die ze verzamelen uitvoeren.  Dit is zodat u een fundamentele begrip van de verschillen en relatieve sterke.  

## <a name="basic-architecture"></a>Eenvoudige architectuur
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Alle SCOM onderdelen zijn geïnstalleerd op uw datacenter.  [Agenten zijn geïnstalleerd](http://technet.microsoft.com/library/hh551142.aspx) op Windows en Linux machines die worden beheerd door SCOM.  Agenten verbinden met [Servers voor het beheer](https://technet.microsoft.com/library/hh301922.aspx) die communiceren met de SCOM database en gegevens BTW-records.  Agenten, is afhankelijk van domeinverificatie verbinding maakt met management servers.  Die buiten een vertrouwd domein kunnen uitvoeren verificatie via clientcertificaat of verbinding maken met een [Gateway-Server](https://technet.microsoft.com/library/hh212823.aspx).

SCOM vereist twee SQL-databases, één voor operationele gegevens en een andere datawarehouse ter ondersteuning van rapportage en gegevensanalyse.  Een [Reporting Server](https://technet.microsoft.com/library/hh298611.aspx) wordt uitgevoerd SQL Reporting Services rapporteren van gegevens uit het datawarehouse. 

SCOM kunt cloud resources met behulp van management packs voor producten, zoals [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)en [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html)controleren.  Deze packs management voor het gebruik van een of meer lokale agenten als proxy's voor ontdekken cloud resources en actieve werkstromen meten hun prestaties en beschikbaarheid weergeven.  Proxy agenten worden ook gebruikt voor [netwerkapparaten beeldscherm](https://technet.microsoft.com/library/hh212935.aspx) en andere externe bronnen.

De bewerkingen-Console is een Windows-toepassing die is verbonden met een van de servers voor het beheer en kan de beheerder wilt weergeven en verzamelde gegevens analyseren en configureren van de SCOM-omgeving.  Een web gebaseerde console kan worden gehost op een IIS-server en biedt gegevensanalyse via een browser.

![SCOM architectuur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics

De meeste OMS onderdelen zijn in de cloud Azure zodat u kunt implementeren en met minimale kosten en administratieve moeite beheren.  Alle gegevens die worden verzameld door Log Analytics wordt opgeslagen in de bibliotheek OMS.

Log Analytics kan gegevens verzamelen van een van de drie bronnen:

- Fysieke en virtuele machines met Windows en de [Microsoft Monitoring Agent MMA ()](https://technet.microsoft.com/library/mt484108.aspx) of Linux en de [Bewerkingen Management Suite Agent voor Linux](https://technet.microsoft.com/library/mt622052.aspx).  Deze machines kunnen werken met on-premises implementatie of virtuele machines in Azure of een ander cloud.
- Een account Azure opslagruimte met [Diagnostisch hulpprogramma Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) -gegevens die worden verzameld door de rol van Azure werknemer, Webrol of VM.
- [Verbinding met een SCOM management-groep](https://technet.microsoft.com/library/mt484104.aspx).  In deze configuratie wordt de agenten communiceren met SCOM management servers die geven van de gegevens voor de database SCOM, waar deze vervolgens naar de gegevensopslag OMS wordt bezorgd.
Beheerders verzamelde gegevens analyseren en Log Analytics configureren met de portal OMS die wordt gehost in Azure wordt aangegeven en zijn toegankelijk vanaf elke gewenste browser.  Mobiele apps voor toegang tot deze gegevens zijn beschikbaar voor de standaard-platforms.

![Log Analysearchitectuur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integratie van SCOM en Log Analytics

Wanneer SCOM wordt gebruikt als gegevensbron voor Log Analytics kunt u gebruikmaken van de functies van beide producten in een hybride omgeving voor controle.  U kunt bestaande SCOM agenten via de Console bewerkingen worden beheerd door OMS, naast de verdere management packs uitvoeren vanaf SCOM configureren.  
Gegevens uit een groep voor beheer van verbonden SCOM wordt bezorgd in Log Analytics vier manieren:

- Gebeurtenissen en prestatiegegevens zijn verzameld door de agent en bezorgd bij SCOM.  Servers voor het beheer in SCOM overbrengen vervolgens de gegevens op Log Analytics.
- Sommige gebeurtenissen zoals IIS-logboeken en beveiliging gaat u verder met het rechtstreeks naar Log analyses van de agent worden bezorgd.
- Sommige oplossingen wordt extra software overbrengen op de agent of vereisen dat software zijn geïnstalleerd om extra gegevens verzamelen.  Deze gegevens worden, meestal rechtstreeks naar Log Analytics verzonden.
- Sommige oplossingen worden gegevens verzameld rechtstreeks vanuit SCOM management servers die niet afkomstig is van de agent.  De [Waarschuwing beheeroplossing](https://technet.microsoft.com/library/mt484092.aspx) verzamelt bijvoorbeeld waarschuwingen van SCOM nadat ze zijn gemaakt.

## <a name="monitoring-logic"></a>Logica bewaken
SCOM en Log Analytics werken met dezelfde gegevens die worden verzameld uit agenten maar fundamentele verschillen in hoe ze definiëren en implementeren van hun logica voor het verzamelen van gegevens en hoe ze de gegevens die ze verzamelen analyseren.

### <a name="operations-manager"></a>Operations Manager
Controle-logica voor SCOM is geïmplementeerd in [management packs](https://technet.microsoft.com/library/hh457558.aspx) waarin logica voor de onderdelen die u wilt controleren, ontdekken meten de status van deze onderdelen en voor het verzamelen van gegevens te analyseren.  -Gegevens bewaken kan net zo eenvoudig als het verzamelen van een gebeurtenis of prestaties item of deze complexe logica geïmplementeerd in een script kan gebruiken.  Management packs die volledige controle bevatten zijn beschikbaar voor een groot aantal [Microsoft en toepassingen van derden](http://go.microsoft.com/fwlink/?LinkId=82105) naast hardware en netwerk-apparaten.  U kunt [uw eigen packs voor het beheer van de auteur](http://aka.ms/mpauthor) voor aangepaste toepassingen.

Management packs bevatten meerdere werkstromen dat elk sommige distinct controleren werkt zoals meting van een prestatiemeteritem, de status van een service controleren of een script uitvoeren.  Elke werkstroom wordt uitgevoerd onafhankelijk en bepaalt een eigen resultaten zoals welke database worden geschreven naar en of een melding wordt gegenereerd. 

U kunt de details van een werkstroom zoals de frequentie die ze uitvoert, de drempelwaarde voor wanneer ze een fout van mening en de ernst van de melding dat ze genereren negeren.  U kunt ook extra functionaliteit bieden door het toevoegen van uw eigen werkstromen.

![Overschrijft](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management packs zijn geïnstalleerd in de Operations Manager-database en automatisch verdeeld over agenten door management-servers.  Elke agent automatisch management packs downloaden en laden werkstromen die relevant zijn voor de toepassingen die ze hebben geïnstalleerd.  Gegevens die worden verzameld door de-agent is bezorgd terug naar de server management voor invoegen in de SCOM database en gegevens BTW-records.  De bewerkingen-Console kunt u bekijken en analyseren van deze gegevens tot en met aangepaste weergaven, dashboards en rapporten die zijn opgenomen in het management pack.

De verdeling van management packs wordt geïllustreerd in het volgende diagram.

![Management pack stroom](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Gebeurtenis en prestaties van siteverzameling
Log Analytics verzamelt evenementen en prestatiemeteritems van agent-systemen met behulp van bronnen zoals gebeurtenissenlogboek van Windows, IIS-logboeken en Syslog.  U kunt definiëren criteria waarvoor gegevens is verzameld door de Log Analytics-portal en maak vervolgens Log query's om de verzamelde gegevens te analyseren.  Een reeks standaardcriteria wordt gedefinieerd wanneer u uw OMS-werkruimte maken en u kunt extra gegevens voor bepaalde toepassingen definiëren. 

![Gebeurtenislogboeken definiëren in Log Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Hoewel SCOM vele gedetailleerde werkstromen die meestal specifieke criteria definiëren voor gegevens en de actie die moet worden uitgevoerd in antwoord heeft, heeft Log Analytics meer algemene criteria voor het verzamelen van gegevens.  Log query's en oplossingen vindt u meer gerichte criteria voor het analyseren en stelt op specifieke gegevens in de cloud nadat deze is verzameld.

#### <a name="solutions"></a>Oplossingen
Oplossingen bieden extra logica voor het verzamelen van gegevens en analyse.  U kunt oplossingen kunt toevoegen aan uw abonnement OMS vanuit de galerie met oplossingen kunt selecteren.

![Galerie met oplossingen](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Oplossingen uitvoeren hoofdzakelijk in de cloud analyse van gebeurtenissen en van prestatiemeteritems die worden verzameld in de bibliotheek OMS leveren.  Ze kunnen ook aanvullende gegevens te verzamelen die kunnen worden geanalyseerd met Log query's of een extra gebruikersinterface van de oplossing in het dashboard OMS definiëren. 

Bijvoorbeeld de [oplossing wijzigingen bijhouden](https://technet.microsoft.com/library/mt484099.aspx) worden gedetecteerd configuratiewijzigingen op agent systemen en schrijft gebeurtenissen naar de bibliotheek OMS die kunnen worden geanalyseerd met verschillende grafische weergaven om samen te vatten wijzigingen gedetecteerd.  U kunt inzoomen vanuit de samengevatte weergave in log-query's die de gedetailleerde gegevens die worden verzameld door de oplossing worden weergegeven.


Terwijl u welke oplossingen die u aan uw abonnement toevoegt selecteren kunt, u momenteel geen de mogelijkheid om uw eigen oplossingen te maken.  U kunt de gebeurtenissen en prestatiemeteritems voor het verzamelen en het maken van aangepaste weergaven op basis van uw eigen log query's kunt selecteren.

De controle logica van analysegegevens Log wordt in het volgende diagram samengevat.

![Log Analytics-oplossing stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Statuscontrole
### <a name="operations-manager"></a>Operations Manager
SCOM kunt modelleren van de verschillende onderdelen van een toepassing en een realtime status Geef voor elk label.  Hiermee kunt u niet alleen fouten weergeven gedetecteerd en prestaties na verloop van tijd, maar ook voor het valideren van de werkelijke status van een toepassing of systeem en elk van de onderdelen op elk gewenst moment.  Omdat deze de perioden die een toepassing beschikbaar is begrijpt, ondersteunt de systeemstatus-engine in SCOM ook Service niveau overeenkomsten (SLA) waarin analyseren en de beschikbaarheid van een toepassing na verloop van tijd rapporteren.

De weergave hieronder ziet u bijvoorbeeld de realtime status van SQL database-engines gecontroleerd door SCOM.  De status van elk van de databases voor een van de database-engines wordt weergegeven in de onderste helft van de weergave.

![Status weergeven](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

De status Explorer voor een van de database-engines is hieronder wordt weergegeven met de monitoren die worden gebruikt om te bepalen de algemene status.  Deze beeldschermen zijn gedefinieerd in het SQL management pack en uitvoeren op alle SQL database-engines ontdekt door SCOM.

![Servicestatus explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Onderdelen op meerdere systemen kunnen worden gecombineerd om de status van een gedistribueerde toepassing meten.  Dit is vooral handig voor bedrijfstoepassingen die meerdere verdeelde onderdelen bevatten.  U kunt een model dat de status van elk onderdeel metingen die rollup maken in de beschikbaarheid voor de toepassing.

Active Directory is een voorbeeld van één management pack vindt u een model als u wilt analyseren de gedistribueerde onderdelen.  Het onderstaande voorbeelddiagram ziet u de status van de algehele omgeving en de relatie tussen forests, domeinen en domeincontrollers.  Elk van deze onderdelen bevat subonderdelen en meerdere monitoren vergelijkbaar met het bovenstaande voorbeeld voor SQL.

![Diagramweergave SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS niet bevatten een algemene engine naar model toepassingen of hun realtime gezondheid meten.  Afzonderlijke oplossingen kunnen de algemene status van bepaalde services op basis van verzamelde gegevens beoordelen en aangepaste logica kan worden geïnstalleerd op de agent realtime analyses uitvoeren.  Omdat oplossingen uitgevoerd in de cloud met access naar de bibliotheek OMS, kunnen ze vaak grondigere analyse dan worden gewoonlijk uitgevoerd door management packs bieden. 

Bijvoorbeeld, de [AD Assessment en SQL-Assessment oplossingen](https://technet.microsoft.com/library/mt484102.aspx) verzamelde gegevens analyseren en een geven aan andere aspecten van de omgeving.  Het bevat aanbevelingen voor verbeteringen die kunnen worden uitgevoerd om de beschikbaarheid en de prestaties van de omgeving te verbeteren.

![AD Assessment oplossing](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Gegevensanalyse
SCOM en Log Analytics bieden verschillende functies om verzamelde gegevens te analyseren.  SCOM heeft weergaven en Dashboards in de Console bewerkingen voor het analyseren van recente gegevens op verschillende notaties en rapporten voor het presenteren van gegevens uit het datawarehouse in tabelvorm.  Log Analytics biedt een interface voor volledige log querytaal en voor het analyseren van gegevens in de bibliotheek OMS.  Wanneer SCOM wordt gebruikt als gegevensbron voor Log analyses, bevat de opslagplaats gegevens die worden verzameld door SCOM, zodat de Log analysehulpmiddelen kunnen worden gebruikt om het analyseren van gegevens uit beide systemen.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Weergaven
Weergaven in de Console bewerkingen kunt u verschillende gegevenstypen verzameld door SCOM weergeven in verschillende notaties, meestal in tabelvorm voor gebeurtenissen, waarschuwingen en staat gegevens, en dit uitlijnen grafieken voor prestatiegegevens.  Weergaven starten minimale analyse of samenvoeging van de gegevens, maar kunnen u filteren op basis van bepaalde criteria. 

![Weergaven](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Management packs, meestal vindt u meerdere weergaven ondersteunende de toepassing of systeem dat wordt gecontroleerd.  Deze kan status weergaven voor de andere objecten die het management pack ontdekt, waarschuwing weergaven voor gedetecteerde problemen en van prestatieweergaven voor items bevatten.

Weergaven zijn vooral geschikt is voor het analyseren van de huidige status van de omgeving inclusief openen waarschuwingen en de status van gecontroleerde systemen en objecten.  U kunt inzoomen op gedetailleerde gebeurtenis of een melding voor een bepaalde ondersteunende als u de onderliggende oorzaak diagnose wilt prestatiegegevens. U kunt ook de prestaties en de status van de verschillende onderdelen van een toepassing te beoordelen van de huidige status weergeven.

#### <a name="dashboards"></a>Dashboards
Dashboards in de Console bewerkingen werken hoofdzakelijk met dezelfde gegevens zoals weergaven, maar meer aanpasbare zijn en rijkere visualisaties kunnen opnemen.  Een reeks standaard dashboards zijn beschikbaar van te zijn dat u eenvoudig kunt aanpassen voor uw eigen doeleinden.  U kunt ook een PowerShell-widget waarin gegevens uit een PowerShell-query geretourneerd kan worden weergegeven.

![Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Ontwikkelaars hebben de mogelijkheid aangepaste onderdelen toevoegen aan dashboards die in hun packs management opties omvatten.  Deze mogelijk zeer worden gespecialiseerd voor een bepaalde toepassing zoals het dashboard in het SQL management pack hieronder wordt weergegeven.  Dit dashboard kan ook worden gebruikt als een sjabloon voor aangepaste versies.

![SQL-Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Rapporten
Rapporten in SCOM analyseren gegevens uit het datawarehouse in tabelvorm.  Ze kunnen worden afgedrukt en gepland voor de bezorging van geautomatiseerde in verschillende bestandsindelingen zoals PDF, CSV- en Word.  Rapporten werken met gegevens uit het datawarehouse zodat zij vooral geschikt voor analyse van langdurig trends zijn.

Management packs, meestal vindt u aangepaste rapporten voor een bepaalde toepassing.  U kunt ook selecteren in een bibliotheek van algemene rapporten die u aanpassen kunt voor uw eigen toepassingen of voor de uitvoering van ad hoc analyse.

Hier volgt een voorbeeldrapport prestaties met gegevens die worden verzameld door de Active Directory Management Pack.

![Rapport](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Log Analytics heeft een [querytaal](https://technet.microsoft.com/library/mt484120.aspx) die u kunt analyses uitvoeren op gegevens uit meerdere toepassingen zonder te hoeven maken van een aangepaste weergave of rapport.  Omdat OMS is geïmplementeerd in de cloud, worden de prestaties van query's en gegevensanalyse zijn niet onderhevig aan hardwarebeperkingen en query's inclusief miljoenen records snel kunt analyseren. 

Query's in Log Analytics zijn ook de basis van andere functionaliteit gebruiken.  U kunt opslaan van een query, de resultaten exporteren naar Excel of automatisch uitvoeren met regelmatige tussenpozen en een waarschuwing wordt gegenereerd als de resultaten aan bepaalde criteria voldoen.  

![Log query stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Hieronder volgt een voorbeeld van een query Log Analytics.  In dit voorbeeld alle gebeurtenissen met "de slag' in de naam worden geretourneerd en gegroepeerd op gebeurtenis-id.  De gebruiker gewoon vindt u de query en Log Analytics genereert dynamisch de gebruikersinterface voor het uitvoeren van de analyse.  Een item selecteren in de lijst retourneert gegevens van de gedetailleerde gebeurtenis.

![Log query](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Naast ad hoc analyse, kunnen query's in Log Analytics opslaan voor toekomstig gebruik en ook toegevoegd aan uw [OMS dashboard](http://technet.microsoft.com/library/mt484090.aspx) , zoals in het volgende voorbeeld.

![OMS dashboard](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

- Implementeren [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Registreren voor [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics).  
