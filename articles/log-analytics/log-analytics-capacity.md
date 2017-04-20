<properties
    pageTitle="Capaciteit beheeroplossing in Log Analytics | Microsoft Azure"
    description="U kunt de oplossing voor het plannen van de capaciteit in Log Analytics uitgelegd van de capaciteit van uw Hyper-V-servers beheerd door System Center VM Manager u"
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Capaciteit beheeroplossing in Log Analytics


Kunt u de oplossing voor het plannen van capaciteit in Log Analytics aan de hand waarvan u de capaciteit van uw Hyper-V-servers beheerd door System Center VM Manager begrijpen. Deze oplossing vereist systeem Center Operations Manager en System Center VM Manager. Capaciteit Planning is niet beschikbaar als u alleen direct verbonden agenten gebruiken. U installeren de oplossing als u wilt bijwerken van de Operations Manager-agent. De oplossing prestatiemeteritems op de gecontroleerde server leest en gegevens over zoekgebruik wordt verzonden naar de OMS-service in de cloud voor die zijn verwerkt. Logica wordt toegepast op de gegevens over het gebruik en de gegevens voor de cloudservice-records. Gebruikspatronen zijn aangewezen na verloop van tijd en capaciteit geprojecteerd, op basis van de huidige verbruik.

Een raming mogelijk bijvoorbeeld worden aangegeven wanneer extra processorcores of extra geheugen nodig voor een afzonderlijke zijn. In dit voorbeeld, kan de raming aangeven dat in 30 dagen de server extra geheugen moet. Hiermee kunt u plannen voor een geheugenupgrade tijdens van de server volgende onderhoudsvenster, die eenmaal per twee weken optreden mogelijk.

>[AZURE.NOTE] De capaciteit beheeroplossing is niet beschikbaar voor worden toegevoegd aan de werkruimten. Klanten die de energiecapaciteit beheeroplossing geïnstalleerd hebben kunnen blijven gebruiken van de oplossing.  

De capaciteit oplossing voor het plannen is worden bijgewerkt om het adres van de volgende klant gerapporteerd uitdagingen:

- Vereiste VM Manager en Operations Manager gebruiken
- Niet kunnen aanpassen/filter op basis van groepen
- Ieder uur gegevens aggregatie vaker niet genoeg
- Geen VM niveau inzichten
- Betrouwbaarheid

Voordelen van de nieuwe capaciteit-oplossing:

- Ondersteuning voor gedetailleerde gegevens verzamelen met verbeterde betrouwbaarheid en nauwkeurigheid
- Ondersteuning voor Hyper-V zonder VMM
- Visualisatie van de doelstellingen in PowerBI
- Inzichten op VM niveau gebruik


## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- Operations Manager is vereist voor de capaciteit beheeroplossing.
- VM Manager is vereist voor de capaciteit beheeroplossing.
- Operations Manager connectiviteit met Virtual Machine Manager (VMM) is vereist. Zie voor meer informatie over het verbinden van de systemen, [hoe u verbinding maakt VMM met Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
- Operations Manager moet worden verbonden met Log Analytics.
- De capaciteit beheeroplossing toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.


## <a name="capacity-management-data-collection-details"></a>Capaciteit beheer van siteverzameling detailgegevens

Capaciteit Management verzamelt prestatiegegevens, metagegevens en staat gegevens met de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor capaciteit management.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Nee](./media/log-analytics-capacity/oms-bullet-red.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Nee](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)| per uur|

De volgende tabel ziet u voorbeelden van gegevenstypen die worden verzameld door de capaciteit Management:

|**Gegevenstype**|**Velden**|
|---|---|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, naam netwerk, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, weergavenaam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Prestaties|Objectnaam, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|De staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Capaciteit beheerpagina


 Nadat de oplossing capaciteitsplanning is geïnstalleerd, kunt u de capaciteit van uw gecontroleerde servers weergeven met behulp van de tegel **Capaciteitsplanning** op de pagina **Overzicht** in OMS.

![afbeelding van de tegel capaciteit plannen](./media/log-analytics-capacity/oms-capacity01.png)

De tegel wordt geopend de **Capaciteit** -Beheerdashboard waar u een overzicht van de servercapaciteit van de kunt bekijken. De volgende tegels waarop u kunt klikken op de pagina weergegeven:

- *VM tellen*: toont het aantal dagen resterende voor de capaciteit van virtuele machines
- *Berekenen*: toont processorcores en beschikbare geheugen
- *Opslag*: ziet u de beschikbare schijfruimte en gemiddelde latentie Schijfopruiming
- *Zoeken*: de Verkenner met gegevens die u om te zoeken naar gegevens in het systeem OMS gebruiken kunt

![afbeelding van de capaciteit-Beheerdashboard](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Een pagina capaciteit weergeven

- Klik op de pagina **Overzicht** **Capaciteit Management**op en klik vervolgens op **berekenen** of **opslag**.

## <a name="compute-page"></a>Pagina berekenen

U kunt het dashboard **berekenen** in Microsoft Azure OMS capaciteit informatie over het gebruik, geprojecteerd dagen van de capaciteit, en efficiëntie die betrekking hebben op de infrastructuur van uw kunnen bekijken. U kunt het gebied **Gebruik** CPU-gebruik voor core en geheugen weergeven in uw VM-hosts. U kunt de raming gebruiken voor het schatten hoeveel capaciteit kan worden gecontroleerd voor een bepaald datumbereik wordt verwacht. U kunt het gebied **Efficiency** gebruiken om te zien hoe efficiënt uw VM-hosts zijn. U kunt meer informatie over gekoppelde items weergeven door erop te klikken.

U kunt een Excel-werkmap voor de volgende categorieën genereren:

- Bovenste hosts met de hoogste core gebruik
- Bovenste hosts met de hoogste geheugengebruik
- Bovenste hosts met niet efficiënt virtuele machines
- Bovenste hosts door gebruik
- Klik onder hosts door gebruik

![afbeelding van capaciteit Management berekenen](./media/log-analytics-capacity/oms-capacity03.png)


De volgende gebieden worden weergegeven op het dashboard **berekenen** :

**Gebruik**: weergave CPU core en geheugen gebruik in uw VM-hosts.

- *Cores gebruikt*: som voor alle hosts (% van de CPU gebruikt vermenigvuldigd met het aantal fysieke cores op host).
- *Gratis Cores*: fysieke cores min gebruikte cores totaal.
- *Percentage Cores beschikbaar*: fysieke cores gedeeld door het totale aantal fysieke cores vrij te geven.
- *Virtuele kernen per VM*: virtuele cores totaal in het systeem dat wordt gedeeld door het totale aantal virtuele machines in het systeem.
- *Virtuele Cores tot fysieke Cores breedteverhouding*: de verhouding tussen de totale fysieke cores naar fysieke cores die worden gebruikt door virtuele machines in het systeem.
- *Het aantal virtuele Cores beschikbaar*: virtuele core fysieke cores verhouding vermenigvuldigd met de beschikbare fysieke cores.
- *Geheugen gebruikt*: som van geheugen dat wordt gebruikt door alle hosts.
- *Geheugen*: Total fysiek geheugen minteken geheugen gebruikt.
- *Percentage geheugen beschikbaar*: fysiek geheugen gedeeld door een totaal fysiek geheugen vrij te maken.
- *Virtueel geheugen per VM*: Totaal virtueel geheugen in het systeem gedeeld door het totale aantal virtuele machines in het systeem.
- *Virtueel geheugen fysiek geheugen verhouding*: Totaal virtueel geheugen in het systeem gedeeld door het totale fysieke geheugen in het systeem.
- *Virtueel geheugen beschikbaar*: virtueel geheugen fysiek geheugen verhouding vermenigvuldigd met de beschikbare fysieke geheugen.

**Raming hulpmiddel**

U kunt met behulp van het hulpmiddel raming historische trends weergeven voor uw Resourcegebruik. Dit geldt ook voor de trends gebruik voor virtuele machines, geheugen core en opslag. De mogelijkheid raming gebruikt een raming algoritme om te weten wanneer u af bij elk van de resources worden uitgevoerd. Hiermee kunt u de juiste capaciteit, planning, zodat u weet wanneer u moet aanschaffen van meer capaciteit (zoals geheugen, cores of opslag) wordt berekend.

**Efficiency**

- *Idle VM*: met behulp van minder dan 10% van het geheugen van processor en 10% voor de opgegeven periode.
- *VM Overutilized*: met behulp van meer dan 90% van de 90% van processor en geheugen voor de opgegeven periode.
- *Idle Host*: gebruik van minder dan 10% van het geheugen van processor en 10% voor de opgegeven periode.
- *Overutilized Host*: gebruik van meer dan 90% van de 90% van processor en geheugen voor de opgegeven periode.

### <a name="to-work-with-items-on-the-compute-page"></a>Werken met items op de pagina berekenen

1. Klik op het dashboard **berekenen** in het gebied **Gebruik** capaciteit informatie weergeven over de cores processor en geheugen in gebruik.
2. Klik op een item te openen in **de zoekpagina** en gedetailleerde informatie over deze weergeven.
3. In het hulpmiddel **raming** , verplaatst u de schuifregelaar datum om weer te geven van een raming van de capaciteit dat wordt gebruikt op de datum die u kiest.
4. Klik in het gebied **Efficiency** capaciteit efficiency informatie weergeven over virtuele machines en VM hosts.

## <a name="direct-attached-storage-page"></a>Directe bijgevoegd opslag-pagina

U kunt het dashboard **Direct bijgevoegd Storage** in OMS capaciteit informatie over het gebruik van de opslagruimte, schijfprestaties en verwachte dagen van de schijf kunnen bekijken. U kunt het gebied **Gebruik** gebruik van schijfruimte weergeven in uw VM-hosts. U kunt de **Schijf** aandachtsgebied schijfdoorvoer en latentie weergeven in uw VM-hosts. U kunt ook het hulpprogramma raming gebruiken voor het schatten hoeveel capaciteit kan worden gecontroleerd voor een bepaald datumbereik wordt verwacht. U kunt meer informatie over gekoppelde items weergeven door erop te klikken.

U kunt een Excel-werkmap genereren van capaciteit-informatie voor de volgende categorieën:

- Bovenste gebruik van schijfruimte door host
- Bovenste latentie van gemiddeld door host

De volgende gebieden worden weergegeven op de pagina **opslag** :

- *Gebruik*: gebruik van schijfruimte weergeven in uw VM-hosts.
- *Totale schijfruimte*: som (logische schijfruimte) voor alle hosts
- *Schijfruimte gebruikt*: som (gebruikte logische schijfruimte) voor alle hosts
- *Beschikbare schijfruimte*: totale schijfruimte min gebruikte schijfruimte
- *Percentage schijf gebruikt*: schijfruimte gedeeld door totale schijfruimte gebruikt
- *Percentage schijf beschikbaar*: beschikbare schijfruimte gedeeld door totale schijfruimte

![afbeelding van capaciteit Management Direct bijgevoegd Storage](./media/log-analytics-capacity/oms-capacity04.png)

**Prestaties van de schijf**

Met behulp van OMS, kunt u de trend historische gebruik van de schijfruimte weergeven. De mogelijkheid raming gebruikt een algoritme naar project toekomstig gebruik. Voor gebruik met name kunt de mogelijkheid raming u project wanneer u schijfruimte onvoldoende mogelijk. Hierdoor kunt u de juiste opslagruimte plannen en weten wanneer u moet meer opslagruimte kopen.

**Raming hulpmiddel**

U kunt met behulp van het hulpmiddel raming historische trends weergeven voor het gebruik van de schijfruimte. De mogelijkheid raming kunt u ook project wanneer u af bij schijfruimte worden uitgevoerd. Hierdoor kunt u de juiste capaciteit plannen en weten wanneer u moet meer opslagcapaciteit aanschaffen.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Werken met items op de pagina Direct bijgevoegd Storage

1. In het gebied **Gebruik** op het dashboard **Direct Storage die zijn gekoppeld** , kunt u de schijf gebruik-informatie weergeven.
2. Klik op een gekoppeld item te openen in **de zoekpagina** en gedetailleerde informatie over deze weergeven.
3. In het gebied **Schijfprestaties** , kunt u Schijfopruiming doorvoer en latentie-informatie weergeven.
4. In het **hulpmiddel raming**, verplaatst u de schuifregelaar datum om weer te geven van een raming van de capaciteit dat wordt gebruikt op de datum die u kiest.


## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde capaciteit management gegevens moeten worden weergegeven.
