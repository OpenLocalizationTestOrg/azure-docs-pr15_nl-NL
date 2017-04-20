<properties
    pageTitle="Microsoft Azure-abonnement en limieten van de Service, quota en beperkingen"
    description="Overzicht van veelvoorkomende Azure-abonnement en limieten van de service, quota en beperkingen. Dit geldt ook voor meer informatie over het limieten samen met maximumwaarden vergroten."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-abonnement en limieten van de service, quota en beperkingen

## <a name="overview"></a>Overzicht

In dit document bevat enkele van de meest voorkomende Microsoft Azure-limieten. Dit omvat momenteel niet alle Azure-services. Na verloop van tijd worden deze limieten uitgevouwen en bijgewerkt meer van het platform bedekt.

Ga naar [Azure prijzen overzicht](https://azure.microsoft.com/pricing/) voor meer informatie over Azure prijzen. Er, kunt u uw kosten gebruikt de [Prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/) schatten of via de prijzen detailpagina van een service (bijvoorbeeld [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Als u het maximum boven de **Limiet standaard wilt**, kunt u [een verzoek voor ondersteuning van online-klant gratis openen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). De limieten kunnen niet worden verhoogd boven de **Maximale** waarde in de onderstaande tabellen. Als er geen **Maximumlimiet** kolom, klikt u vervolgens de opgegeven resource beschikt niet over aanpasbare limieten.

## <a name="limits-and-the-azure-resource-manager"></a>Maximale en Azure bronbeheer

Nu is het mogelijk om meerdere Azure resources in een enkel Azure-resourcegroep combineren. Wanneer u een resourcegroepen gebruikt, worden limieten die één keer globale waren beheerd op een regionale niveau met Azure bronbeheer. Zie [overzicht van de Azure resourcemanager](azure-resource-manager/resource-group-overview.md)voor meer informatie over resourcegroepen Azure.

In de onderstaande limieten, is een nieuwe tabel toegevoegd aan alle verschillen tussen de limieten bij gebruik van de Manager van de Resource Azure. Er is bijvoorbeeld een tabel **Abonnementen** en een **Abonnement limieten - resourcemanager Azure** -tabel. Wanneer een limiet is van toepassing op beide scenario's, wordt deze alleen weergegeven in de eerste tabel. Tenzij anders aangegeven, limieten gelden voor alle alle regio's.

> [AZURE.NOTE] Het is belangrijk om te benadrukken dat quota voor resources in resourcegroepen Azure per regio toegankelijk voor uw abonnement, en niet per-abonnement zijn, zoals de quota voor het beheer van service zijn. Laten we core-quota gebruiken als voorbeeld. Als u nodig hebt voor het aanvragen van een stijging van het quotum met ondersteuning voor cores, moet u bepalen hoeveel cores die u wilt gebruiken in welke regio's en breng een specifiek verzoek voor de resourcegroep Azure core quota voor de bedragen en regio's die u wilt. Daarom, als u gebruiken 30 cores in West Europa moet; de toepassing uitvoeren u moet specifiek 30 cores in West Europa aanvragen. Maar er wordt een core quotum vergroten in een andere regio geen--alleen West Europe heeft het quotum voor 30-core.
<!-- -->
Hierdoor mag handiger aandachtspunten bepalen wat uw Azure resourcegroep quota's moeten zijn voor uw werkzaamheden in een één regio, en dat bedrag in elke regio waarin bent u van plan implementatie aanvragen. Zie voor meer hulp bij uw huidige quota voor specifieke regio's ontdekken voor [het oplossen van problemen met de implementatie](./resource-manager-common-deployment-errors.md) .

## <a name="service-specific-limits"></a>Service-specifieke limieten

- [Active Directory](#active-directory-limits)
- [API-beheer](#api-management-limits)
- [App-Service](#app-service-limits)
- [Toepassingsgateway](#application-gateway-limits)
- [Toepassing inzichten](#application-insights-limits)
- [Automatisering](#automation-limits)
- [Cache Azure bestand Vgx.](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Back-up maken](#backup-limits)
- [Batch](#batch-limits)
- [BizTalk-Services](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloudservices](#cloud-services-limits)
- [Gegevens Factory](#data-factory-limits)
- [Gegevens Lake Analytics](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Gebeurtenis Hubs](#event-hubs-limits)
- [IoT Hub](#iot-hub-limits)
- [Belangrijke kluis](#key-vault-limits)
- [Mediaservices](#media-services-limits)
- [Mobiele betrokkenheid](#mobile-engagement-limits)
- [Mobiele Services](#mobile-services-limits)
- [Cmdlets voor controle](#monitoring-limits)
- [Meervoudige verificatie](#multi-factor-authentication)
- [Netwerken](#networking-limits)
- [Hub-Notification-Service](#notification-hub-service-limits)
- [Operationele inzichten](#operational-insights-limits)
- [Resourcegroep](#resource-group-limits)
- [Planner](#scheduler-limits)
- [Zoeken](#search-limits)
- [Service Bus](#service-bus-limits)
- [Herstel van de site](#site-recovery-limits)
- [SQL-Database](#sql-database-limits)
- [Opslag](#storage-limits)
- [StorSimple-systeem](#storsimple-system-limits)
- [Stream Analytics](#stream-analytics-limits)
- [Abonnement](#subscription-limits)
- [Verkeer Manager](#traffic-manager-limits)
- [Virtuele Machines](#virtual-machines-limits)
- [VM schaal Sets](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Abonnementen
#### <a name="subscription-limits"></a>Abonnementen
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonnementen - resourcemanager van Azure

De volgende beperkingen gelden bij gebruik van de Azure resourcemanager en Azure resourcegroepen. Limieten die niet met de resourcemanager van Azure hebt gewijzigd, worden niet weergegeven onder. Raadpleeg de vorige tabel voor deze limieten vermeld.

Zie voor informatie over beperkingen ten aanzien van resourcemanager aanvragen voor de verwerking, [resourcemanager beperken aanvragen](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Resourcegroep limieten

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Virtuele Machines limieten
#### <a name="virtual-machine-limits"></a>VM limieten
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limieten voor virtuele Machines - resourcemanager van Azure

De volgende beperkingen gelden bij gebruik van de Azure resourcemanager en Azure resourcegroepen. Limieten die niet met de resourcemanager van Azure hebt gewijzigd, worden niet weergegeven onder. Raadpleeg de vorige tabel voor deze limieten vermeld.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>VM schaal Sets limieten

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Limieten voor netwerken

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Limieten voor netwerken
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Toepassingsgateway beperkt

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Limieten voor verkeer Manager

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-limieten

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Opslaglimieten

Zie voor meer informatie over opslaglimieten account, [Azure opslag schaalbaarheid en prestaties doelen](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Opslaglimieten voor de Service

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>VM schijf limieten

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Zie [VM grootten](../articles/virtual-machines/virtual-machines-linux-sizes.md) voor meer informatie.

**Standaard opslag-accounts**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium opslag-accounts**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Opslaglimieten voor de Resource-Provider

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Cloud Services-limieten

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Limieten voor App-Service
De volgende limieten voor de App Service krijg ik limieten voor de Web Apps, Mobile-Apps, API-Apps en logica Apps.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler-limieten

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-limieten

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Limieten voor BizTalk-Services
De volgende tabel ziet u de limieten voor Azure BizTalk-Services.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB limieten

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Quota weergegeven met een sterretje (*) [contact opnemen met ondersteuning voor Azure kan worden aangepast](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Limieten voor Mobile betrokkenheid

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Limieten voor zoeken voor

Prijzen lagen bepalen de capaciteit en limieten van uw search-service. Lagen zijn:

- *Gratis* meerdere tenant-service, gedeeld met andere Azure abonnees, is bedoeld voor evaluatie- en kleine ontwikkeling projecten.
- *Eenvoudige* biedt speciale bronnen voor productie werkbelasting met een kleinere schaal, met maximaal drie replica's voor maximaal beschikbare query werkbelasting.
- *Standaard (S1, S2, S3, S3 hoge dichtheid)* is voor grotere productie werkbelasting. Meerdere niveaus bevat de standaard laag zodat u de configuratie van een bron voor specifieke scenario's kunt.

**Limieten per abonnement**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limieten per search-service**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Zie voor meer gedetailleerde informatie over andere limieten, met inbegrip van de grootte van het document query's per seconde, toetsen, verzoeken en antwoorden, [Service-beperkingen in Azure zoeken](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-limieten

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-limieten

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limieten voor Mobile-Services

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Limieten voor controle

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Melding Hub Service limieten

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Gebeurtenis Hubs limieten

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus limieten

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub-limieten

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Gegevens Factory-limieten

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Gegevens Lake Analytics-limieten
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics-limieten
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-limieten

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp-limieten

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Beperkt StorSimple systeem

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Operationele inzichten limieten

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Back-limieten

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Limieten voor site-herstel

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Toepassing inzichten limieten

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management limieten

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Limieten voor Azure Cache bestand Vgx.

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Toets kluis limieten

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Meervoudige verificatie
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Limieten voor automatisering
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Limieten voor SQL-Database

Zie [Beperkingen voor de Resource van de SQL Database](sql-database/sql-database-resource-limits.md)voor de grenzen van de SQL-Database.

## <a name="see-also"></a>Zie ook

[Wat zijn Azure limieten en toename](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuele Machine en Cloud Service grootten van Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Grootte voor Cloudservices](cloud-services/cloud-services-sizes-specs.md)
