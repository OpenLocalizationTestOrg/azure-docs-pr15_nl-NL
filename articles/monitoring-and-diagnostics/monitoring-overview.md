<properties
    pageTitle="Overzicht van de cmdlets voor controle in Microsoft Azure | Microsoft Azure"
    description="Bovenste niveau overzicht van cmdlets voor controle en diagnostische gegevens in Microsoft Azure zoals waarschuwingen, webhooks, automatisch schalen en meer."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Overzicht van de cmdlets voor controle in Microsoft Azure wordt aangegeven

Dit artikel bevat een overzicht van Azure resources cmdlets voor controle. Deze biedt verwijzingen naar informatie aan specifieke soorten resources.  Zie [richtlijnen voor controle- en diagnostische hulpprogramma's](../best-practices-monitoring.md)voor voor uitgebreide informatie over het controleren van de toepassing niet Azure oogpunt.

Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Cmdlets voor controle, vindt u gegevens om ervoor te zorgen dat uw toepassing omhoog blijft en uitvoeren in orde zijn. Ook kunt u potentiële problemen voorkomen of oplossen voorbij die. Bovendien kunt u controlegegevens uitgebreide inzicht krijgen over het gebruik van de toepassing. Deze kennis kunt u helpen te verbeteren de prestaties van toepassingen of onderhoud of acties waarvoor anders vereist tussenkomst zou automatiseren.

In het volgende diagram ziet u een conceptuele weergave van Azure monitoring, met inbegrip van het type logboeken die u kunt verzamelen en wat u kunt doen met die gegevens.   

![Logische Model voor controle en diagnostische hulpprogramma's voor niet-berekeningscluster resources](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Afbeelding 1: Conceptueel Model voor controle en diagnostische hulpprogramma's voor niet-berekeningscluster resources

<br/>

![Logische Model voor controle en diagnostische hulpprogramma's voor berekeningscluster resources](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Afbeelding 2: Conceptueel Model voor controle en diagnostische hulpprogramma's voor berekeningscluster resources


## <a name="monitoring-sources"></a>Bronnen voor controle
### <a name="activity-logs"></a>Activiteitenlogboeken
U kunt de activiteit Log (eerder operationele of controlelogboeken genoemd) informatie zoeken over de resource zoals gezien door de Azure infrastructuur. Het logboek bevat toegangsgegevens voor de tijden wanneer resources zijn gemaakt of verwijderd.  

### <a name="host-vm"></a>Host VM
**Alleen berekenen**


Enkele resources zoals Cloud Services, virtuele Machines berekenen en Service stof een speciale Host VM die ze hebben. De Host VM is het equivalent van de hoofdsite VM in het model van Hyper-V hypervisor. In dit geval kunt u de doelstellingen verzamelen op alleen de Host VM naast het besturingssysteem Gast.  

Voor andere Azure-services is niet per se een toewijzing 1:1 tussen uw bron- en een bepaalde Host VM dus host VM doelstellingen niet beschikbaar zijn.


### <a name="resource---metrics-and-diagnostics-logs"></a>Resource - maatstaven en diagnostische logboeken
Collectable aan de doelstellingen, hangen af van het resourcetype. Virtuele Machines bevat bijvoorbeeld statistieken op de schijf IO en percentage CPU. Maar deze stat voor een wachtrij Service Bus, waarmee in plaats daarvan aan de doelstellingen zoals wachtrij grootte en een bericht doorvoer niet bestaat.

U kunt aan de doelstellingen op de modules Gast OS en diagnostische hulpprogramma's zoals diagnostisch hulpprogramma Azure aanvragen voor berekeningscluster resources. Azure diagnostische gegevens kunt verzamelen en routeren Verzamel diagnostische gegevens op andere locaties, met inbegrip van Azure opslag.

Een lijst met momenteel collectable aan de doelstellingen is beschikbaar binnen [de doelstellingen ondersteund](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Toepassing - diagnostische logboeken, toepassingen en aan de doelstellingen
**Alleen berekenen**

Toepassingen kunnen worden uitgevoerd boven aan het besturingssysteem Gast in het model berekeningscluster. Ze verzenden hun eigen set logboeken en aan de doelstellingen.

Soorten aan de doelstellingen opnemen

- Prestatie-items
- Toepassingslogboeken aan de
- Gebeurtenislogboeken van Windows
- Bron van gebeurtenis .NET
- IIS-logboeken
- Manifest op basis van ETW
- Crashdumps
- Foutenlogboeken van klant


## <a name="uses-for-monitoring-data"></a>-Gegevens bewaken kunnen worden gebruikt

### <a name="visualize"></a>Visualiseren
Zomersportevenementen van de controlegegevens in afbeeldingen en grafieken, kunt u trends uiterst sneller dan ze opzoeken via de gegevens zelf vinden.  

Er zijn een paar visualisatie methoden:

- Gebruik van de Azure-portal
- Gegevens routeren naar Azure-toepassing inzichten
- Gegevens routeren naar Microsoft PowerBI
- De gegevens doorsturen naar een derde partij visualisatie hulpmiddel met een van beide live streaming of doordat de hulpmiddel vanuit een archief in Azure opslag als gelezen

### <a name="archive"></a>Archief
-Gegevens bewaken is meestal geschreven met Azure storage en er worden bewaard totdat u deze verwijderen.

Een aantal manieren om deze gegevens te gebruiken:

- U kunt andere hulpmiddelen binnen of buiten Azure lezen en verwerkt hebben eenmaal geschreven.
- U de gegevens voor een lokale archief lokaal downloaden of uw bewaarbeleid in de cloud om de gegevens voor langere tijd wijzigen.  
- U laten de gegevens in Azure opslag staan, hoewel u moet betalen voor Azure opslag op basis van de hoeveelheid gegevens die u wilt behouden.
-

### <a name="query"></a>Query
U kunt de Azure Monitor REST API, cross platform opdrachtregel-Interface (CLI) opdrachten, PowerShell-cmdlets of de .NET SDK gebruiken voor toegang tot de gegevens in het systeem of Azure opslag

Voorbeelden hiervan zijn:

-  Gegevens ophalen voor een aangepaste controle toepassing die u hebt geschreven
-  Aangepaste query's maken en verzenden van die gegevens naar een derde partij-toepassing.

### <a name="route"></a>Doorsturen
U kunt controlegegevens op andere locaties in realtime stream.

Voorbeelden hiervan zijn:

- Verzenden naar toepassing inzichten zodat u de visualisatiehulpmiddelen er kunt gebruiken.
- Verzenden naar gebeurtenis Hubs zodat u naar hulpprogramma's van derden doorsturen kunt-realtime analyses uitvoeren.

### <a name="automate"></a>Automatiseren
U kunt controlegegevens trigger gebeurtenissen of even gehele processen voorbeelden hiervan zijn:

- Gegevens gebruiken in automatisch schalen berekeningscluster exemplaren omhoog of omlaag op basis van de toepassing laden.
- E-mailberichten verzenden wanneer een meting een vooraf ingestelde drempel kruist.
- De URL van een web (webhook) om een actie uitvoeren in een systeem buiten Azure bellen
- Een runbook starten in Azure automatisering om uit te voeren een verscheidenheid aan taken

## <a name="methods-of-use"></a>Methoden voor het gebruik
In het algemeen, kunt u gegevens bijhouden, routeren en ophalen via een van de volgende manieren bewerken. Niet alle methoden zijn beschikbaar voor alle acties of gegevenstypen.

- [Azure-portal](https://portal.azure.com)
- [PowerShell](insights-powershell-samples.md)  
- [Meerdere platforms opdrachtregelinterface)](insights-cli-samples.md)
- [REST API](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Cmdlets voor controle van Azure-aanbiedingen
Azure heeft aanbiedingen die beschikbaar zijn voor het controleren van uw services van de voorziening infrastructuur naar toepassing telemetrielogboek. De beste strategie monitoring combineert benut alle drie naar uitgebreide, gedetailleerde inzicht in de status van uw services.

- [Azure Monitor](http://aka.ms/azmondocs) – aanbiedingen visualisatie, query, routering, kennisgevingen, automatisch schalen en automatisering van gegevens zowel uit de Azure infrastructuur (activiteit logboekregistratie) en elke afzonderlijke Azure resource (diagnostische logboeken). In dit artikel maakt deel uit van de documentatie Azure Monitor. De naam van de Azure Monitor is uitgebracht 25 September bij Ignite 2016.  De naam van de vorige is "Azure inzichten."  
- [Toepassing inzichten](https://azure.microsoft.com/documentation/services/application-insights/) – biedt uitgebreide detectie en diagnostische hulpprogramma's voor problemen op de toepassingslaag van uw service, goed geïntegreerd boven aan de gegevens van Azure bewaken. Het is het platform standaard diagnostische hulpprogramma's voor App-Service Web Apps.  U kunt gegevens uit andere services om dit te routeren.  
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) -onderdeel van [Bewerkingen Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – biedt een compleet IT-beheeroplossing voor zowel on-premises en derden cloudgebaseerde infrastructuur (zoals AWS) naast het Azure resources.  Gegevens van Azure Monitor kunnen worden gerouteerd rechtstreeks naar Log analyses, zodat u aan de doelstellingen en logboeken voor uw hele omgeving op één plaats kunt zien.     


## <a name="next-steps"></a>Volgende stappen
Meer informatie over

- [Azure Monitor in een video van Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Aan de slag met Azure Monitor](monitoring-get-started.md)
- [Diagnostisch hulpprogramma azure](../azure-diagnostics.md) als u probeert diagnosticeren in uw toepassing Cloudservice binnenkomt, VM of Service stof.
- [Inzichten van toepassing](https://azure.microsoft.com/documentation/services/application-insights/) als u te diagnostische problemen in uw App Service Web-app probeert.
- [Azure-opslag problemen oplossen](../storage/storage-e2e-troubleshooting.md) bij het gebruik van opslagruimte BLOB's, tabellen of wachtrijen
- [Log analyses](https://azure.microsoft.com/documentation/services/log-analytics/) en de [bewerkingen Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
