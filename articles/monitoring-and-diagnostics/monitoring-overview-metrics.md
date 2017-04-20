<properties
    pageTitle="Overzicht van de doelstellingen in Microsoft Azure | Microsoft Azure"
    description="Overzicht van de doelstellingen en hoe deze worden gebruikt in Microsoft Azure"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van de doelstellingen in Microsoft Azure 

In dit artikel wordt beschreven wat de doelstellingen zijn in Microsoft Azure hun voordelen en hoe u aan de slag met deze.  

## <a name="what-are-metrics"></a>Wat zijn de doelstellingen?

Azure Monitor kunt u telemetrielogboek om te krijgen inzicht in de prestaties en de status van uw werkbelasting op Azure in beslag neemt. Het belangrijkste type Azure telemetriegegevens zijn de cijfers (ook wel prestatiemeteritems genoemd) dat door de meest Azure resources. Azure Monitor biedt verschillende manieren om te configureren en gebruiken van deze gegevens voor het controleren en problemen oplossen.


## <a name="what-can-you-do-with-metrics"></a>Wat kunt u doen met de doelstellingen?

Aan de doelstellingen zijn van een zeer handige bron van telemetrielogboek en kunt u als volgt te werk:

- **Bijhouden de prestaties** van de bron (zoals een VM, Website of logica App) door uitzetten van de doelstellingen op een grafiek in de portal en deze grafiek aan een dashboard vast.
- **Een melding ontvangen van een probleem** die invloed hebben op de prestaties van uw resource wanneer een meting een bepaalde drempel kruist.
- **Configureren automatische acties**, zoals autoscaling een resource of een runbook gebruikt wanneer een meting een bepaalde drempel kruist.
- **Geavanceerde analyses uitvoeren** of gerapporteerd op prestaties of gebruik trends van uw resources.
- **Archief** de prestaties of statusproblemen geschiedenis van de resource **voor naleving/controle** doelstellingen.

##  <a name="metric-characteristics"></a>Metrische kenmerken
Aan de doelstellingen bestaan uit de volgende kenmerken:

- Alle doelstellingen hebben **1 minuten frequentie**. U ontvangt een waarde elke minuut van de resource, zodat u in de buurt van realtime inzicht in de status en de status van de resource.
- Aan de doelstellingen zijn **beschikbaar kant-en-klare zonder dat u nodig hebt om deel te nemen** of bij het instellen van extra diagnostische gegevens.
- U kunt voor elke metrisch **30 dagen na geschiedenis** openen. U kunt snel de recente en maandelijkse trends in de prestaties of de status van uw resource bekijken.

U kunt:

- Eenvoudig ontdekken, access en **alle statistieken bekijken** via de Azure-portal als u een resource selecteert en ze in een grafiek uitzetten. 
- Een metrische **waarschuwingsregels die wordt een melding gestuurd of geautomatiseerde actie duurt** configureren wanneer de meetwaarde kruist de drempelwaarde die u hebt ingesteld. Automatisch schalen is een speciale geautomatiseerde actie waarmee u de schaal van de resource om te voldoen aan de verzoeken voor oproepen of laden op uw website of het berekenen van resources uit aanpassen. U kunt de schaal van out/in een instelling automatisch schalen regel configureren op basis van een meting een drempelwaarde kruist.
- **Archief** aan de doelstellingen voor meer of ze gebruiken voor het melden van offline. U kunt uw maatstaven voor blob storage wanneer u diagnostische instellingen voor de resource configureren routeert.
- **Stream** Maatstelsel van een gebeurtenis-Hub, zodat u kunt ze vervolgens doorsturen naar Azure Stream Analytics of aangepaste apps voor dicht bij realtime analyse. U kunt doen met de diagnostische instellingen.
- **Route** alle maatstelsel OMS Log Analytics te ontgrendelen direct analyses, zoeken en aangepaste waarschuwen bij gegevens aan de doelstellingen van uw resources.
- **Verbruiken** de doelstellingen via nieuwe Azure Monitor REST API's.
- De doelstellingen van de **query** is via de PowerShell-Cmdlets of meerdere platforms REST API.

 ![Routering van de doelstellingen in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Aan de doelstellingen toegang via de portal
Hier volgt een snelle stapsgewijze instructies voor het maken van een metrische grafiek via Azure-portal

### <a name="view-metrics-after-creating-a-resource"></a>Weergave aan de doelstellingen na het maken van een resource
1. Azure-portal openen
2. Maak een nieuwe App-Service - website.
3. Nadat u een website hebt gemaakt, gaat u naar het blad Overzicht van de website.
4. U kunt de doelstellingen van de nieuwe weergeven als een tegel 'Monitoring'. U kunt de tegel bewerken en selecteer de optie meer metrisch

 ![Klik op een resource in Azure Monitor aan de doelstellingen](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Toegang tot alle doelstellingen op één locatie
1. Open de Azure-portal 
2. Ga naar het nieuwe tabblad beeldscherm en selecteer de optie maatstelsel eronder 
3. U kunt uw abonnement, resourcegroep en de naam van de resource selecteren in de vervolgkeuzelijst. 
4. Nu kunt u de lijst beschikbare maatstelsel weergeven. 
5. Selecteer de meetwaarde u bent geïnteresseerd en uitzetten deze. 
6. U kunt het vastmaken aan het dashboard door te klikken op de pincode in de rechterbovenhoek.

 ![Toegang tot alle doelstellingen op één locatie in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] U kunt toegang tot host niveau aan de doelstellingen van VMs (Azure resourcemanager gebaseerd) en VM schaal Sets zonder eventuele aanvullende diagnostische instellingen. Deze nieuwe host niveau gegevens zijn beschikbaar voor Windows en Linux exemplaren. Deze gegevens worden niet verwarren met de Gast-OS niveau criteria die u toegang tot hebt wanneer u Diagnostisch hulpprogramma Azure op uw VMs of VMSS inschakelen. Meer informatie over het configureren van Azure diagnostische hulpprogramma's, Zie [Wat is Microsoft Azure diagnostische gegevens](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Aan de doelstellingen toegang via REST API
Azure aan de doelstellingen kunnen worden geraadpleegd via Azure Monitor-API's. Er zijn twee API's die u helpen detecteren en toegang tot aan de doelstellingen. Gebruik de: 

- [Azure Monitor metrisch definities REST API](https://msdn.microsoft.com/library/mt743621.aspx) voor toegang tot de lijst met parameters die beschikbaar zijn voor een service.
- [Azure Monitor aan de doelstellingen REST API](https://msdn.microsoft.com/library/mt743622.aspx) voor toegang tot de gegevens van de doelstellingen van de werkelijke

>[AZURE.NOTE] In dit artikel worden de doelstellingen via de [nieuwe API aan de doelstellingen](https://msdn.microsoft.com/library/dn931930.aspx) voor Azure resources. De API-versie voor de nieuwe metrische definities API is 03-01-2016 en de versie aan de doelstellingen API 2016-01-09 is. De oudere metrische definities en aan de doelstellingen kunnen worden geopend met de api-versie 04-01-2014.

Zie voor meer gedetailleerde stapsgewijze instructies voor het gebruik van de Azure Monitor REST API's, [Azure Monitor REST API Stapsgewijze instructies](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Opties voor de doelstellingen exporteren
U kunt gaat u naar het blad naar diagnostische logboeken onder het tabblad beeldscherm en de opties exporteren doelstellingen weergeven. U kunt aan de doelstellingen (en diagnostische logboeken) moet worden doorgestuurd naar Blob Storage, gebeurtenis Hubs of OMS Log Analytics selecteren voor gebruik-situaties eerder in dit artikel is vermeld. 

 ![Opties voor de doelstellingen in Azure Monitor exporteren](./media/monitoring-overview-metrics/MetricsOverview3.png)   

U kunt dit configureren via resourcemanager sjablonen [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) of [REST API's](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Aan de doelstellingen van actie ondernemen
U kunt meldingen ontvangen of automatische acties uitvoeren op metrische gegevens. U kunt configureren waarschuwingsregels of de instellingen automatisch schalen dat te doen.

### <a name="alert-rules"></a>Waarschuwingsregels
U kunt waarschuwingsregels configureren voor de doelstellingen. Deze waarschuwingsregels kunnen controleren als een meting heeft een bepaalde drempel Gekruiste en via e-mail een melding of fire een webhook die vervolgens kan worden gebruikt om een aangepast script uitvoeren. U kunt ook de webhook gebruiken voor het configureren van 3e product-integraties.

 ![Aan de doelstellingen en waarschuwingsregels in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatisch schalen
Sommige Azure resources ondersteuning voor meerdere exemplaren aan de nieuwe schaal of u omgaat met uw werkbelasting. Automatisch schalen is van toepassing op App-Services (Web-apps), VM schaal Sets (VMSS) en klassieke Cloudservices. U kunt automatisch schalen regels aan de nieuwe schaal of wanneer een bepaalde meting die invloed hebben op uw werkzaamheden kruist een drempel die u opgeeft. Zie [overzicht van autoscaling](monitoring-overview-autoscale.md)voor meer informatie.

 ![Aan de doelstellingen en automatisch schalen in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Ondersteunde Services en de doelstellingen
Azure Monitor is een nieuwe infrastructuur van de doelstellingen. Biedt ondersteuning voor de volgende Azure services in de portal van Azure en de nieuwe versie van de Azure Monitor API:

- VMs (Azure resourcemanager op basis)
- VM schaal Sets
- Batch
- Gebeurtenis-Hub naamruimte 
- Service Bus naamruimte (alleen premium SKU)
- SQL (versie 12)
- Elastische SQL-toepassingen
- Websites
- Webserver-Farms
- Logica Apps
- IoT Hubs
- Bestand Vgx Cache.
- Netwerken: Toepassingsgateways
- Zoeken

U kunt weergeven een een gedetailleerde lijst van alle ondersteunde services en hun doelstellingen aan [de doelstellingen van de Azure Monitor - ondersteunde aan de doelstellingen per resourcetype](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de koppelingen in dit artikel. Daarnaast kunnen meer:  

- over [algemene aan de doelstellingen voor autoscaling](insights-autoscale-common-metrics.md)
- het [maken van waarschuwingsregels](insights-alerts-portal.md)




