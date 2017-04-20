<properties 
    pageTitle="Inleiding tot het App-omgeving" 
    description="Meer informatie over de Service-omgeving van App-functie waarmee beveiligde, VNet-die zijn gekoppeld, speciale schaaleenheden voor het uitvoeren van alle apps." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="stefsch"/>

# <a name="introduction-to-app-service-environment"></a>Inleiding tot het App-omgeving

## <a name="overview"></a>Overzicht ##
Een App-Service-omgeving is een [Premium] [ PremiumTier] service-abonnement optie van Azure-Service voor App vindt u een volledig geïsoleerd en speciale omgeving voor het uitvoeren van Azure-Service voor App-apps veilig op hoog niveau, met inbegrip van [Web Apps][WebApps], [Mobile-Apps][MobileApps], en [API Apps][APIApps].  

App Service omgevingen zijn ideaal voor het vereisen van toepassing-werkbelastingen:

- Zeer hoog schaal
- Moeten worden geïsoleerd en beveiligde netwerktoegang

Klanten kunnen meerdere App serviceomgevingen binnen een enkel Azure, en tussen meerdere Azure regio's maken.  Hierdoor App serviceomgevingen ideaal voor het horizontaal schaalbaarheid staat minder lagen ter ondersteuning van hoge RPS werkbelasting.

App Service omgevingen zijn beperkt tot slechts één klant van toepassingen en altijd zijn geïmplementeerd in een virtueel netwerk.  Klanten fijnmazige controle over beide netwerkverkeer binnenkomende en uitgaande toepassing hebt en toepassingen kunnen snelle beveiligde verbindingen maken via virtuele netwerken tot bedrijfsresources on-premises implementatie.

Alle artikelen en hoe-aan bevindt zich over App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Voor een overzicht van hoe de App serviceomgevingen hoog schaal inschakelen en secure netwerktoegang, raadpleegt u de [Uitgebreide Kennismaking AzureCon] [ AzureConDeepDive] op App-serviceomgevingen!

Voor een uitvoerige op horizontaal schalen met meerdere App serviceomgevingen Zie het artikel over het instellen van een [app geografische distributed milieu][GeodistributedAppFootprint].

Als u wilt zien hoe de beveiligingsarchitectuur weergegeven in de uitgebreide Kennismaking AzureCon is geconfigureerd, Zie het artikel over het implementeren van een [gelaagde beveiligingsarchitectuur](app-service-app-service-environment-layered-security.md) met App-serviceomgevingen.

Apps die worden uitgevoerd op App-serviceomgevingen kunnen hun geregeld door boven apparaten, zoals een web-toepassing firewalls (WAF) toegang hebben.  Het artikel over het [configureren van een WAF voor App serviceomgevingen](app-service-app-service-environment-web-application-firewall.md) behandelt dit scenario. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Speciale berekeningscluster Resources ##
Alle bronnen berekenen in een App-Service-omgeving zijn specifiek uitsluitend voor een één abonnement en een App-Service-omgeving kan worden geconfigureerd met maximaal twaalf (50) berekeningscluster resources voor exclusief gebruik één toepassing.

Een App-Service-omgeving bestaat uit een resourcegroep front berekeningscluster, evenals resourcegroepen van één tot drie werknemer berekenen. 

De front-toepassingen bevat berekeningscluster resources die verantwoordelijk is voor SSL-beëindiging als u ook automatische taakverdeling van aanvragen van de app in een App-Service-omgeving. 

Elke werknemer van toepassingen bevat berekeningscluster resources die zijn toegewezen aan de [App-Service abonnementen][AppServicePlan], die een of meer Azure-Service voor App-apps op zijn beurt bevatten.  Omdat er maximaal drie andere werknemer van toepassingen in een omgeving van de Service-App, hebt u de flexibiliteit om te kiezen verschillende berekeningscluster resources voor elke werknemer-toepassingen.  

Bijvoorbeeld, kunt dit u één werknemer van toepassingen maken met minder krachtige berekeningscluster resources voor het App-Service abonnementen is bedoeld voor ontwikkel- of -apps.  Een tweede (of zelfs derde) werknemer-toepassingen kunt krachtiger berekeningscluster resources die zijn bedoeld voor abonnementen van App-Service productie apps worden uitgevoerd.

Zie voor meer informatie over de hoeveelheid berekeningscluster resources beschikbaar zijn voor de front-end en werknemer van toepassingen, [hoe u een App-Service-omgeving configureren][HowToConfigureanAppServiceEnvironment].  

Voor meer informatie over de beschikbare resource tekengrootten worden ondersteund in een omgeving met App-Service te berekenen, raadpleegt u de [App-serviceprijzen] [ AppServicePricing] pagina en bekijk de beschikbare opties voor de Service-omgevingen App in de prijzen van laag Premium.

## <a name="virtual-network-support"></a>Virtuele netwerkondersteuning ##
Een App-Service-omgeving kan worden gemaakt in **een van beide** een virtueel resourcemanager Azure-netwerk, **of** een klassieke implementatie model virtueel netwerk ([meer informatie over virtuele netwerken][MoreInfoOnVirtualNetworks]).  Aangezien een App-Service-omgeving altijd in een virtueel netwerk en nauwkeuriger binnen een subnet van een virtueel netwerk bestaat, kunt u gebruikmaken van de beveiligingsfuncties van virtuele netwerken om te bepalen van beide voor binnenkomende en uitgaande netwerkcommunicatie.  

Een App-Service-omgeving kan een van beide Internet die tegenover elkaar liggen op een openbare IP-adres, of interne die tegenover elkaar liggen met alleen een adres Azure interne laden verdeling (ILB) zijn.

Kunt u [beveiligingsgroepen netwerk] [ NetworkSecurityGroups] beperken inkomende netwerkcommunicatie met het subnet waarin een App-Service-omgeving zich bevindt.  Hiermee kunt u apps achter boven apparaten en -services zoals web application firewalls en netwerk SaaS providers uitvoeren.

Apps moeten ook regelmatig toegang tot bedrijfsresources zoals interne databases en webservices.  Een aanpak is om deze eindpunten alleen beschikbaar voor interne netwerkverkeer die doorloopt binnen een Azure virtuele netwerk te voeren.  Als een App-Service-omgeving hetzelfde virtuele netwerk als de interne services uitmaakt, apps in de omgeving actief toegang tot deze, met inbegrip van de eindpunten bereikbaar via [Naar website] [ SiteToSite] en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

Voor meer informatie over de werking van App-serviceomgevingen met virtuele netwerken en on-premises implementatie-netwerken te gebruiken raadpleegt u de volgende artikelen op [Netwerkarchitectuur][NetworkArchitectureOverview], [Binnenkomend verkeer bepalen][ControllingInboundTraffic], en [Veilig verbinding maakt met Backends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met App-serviceomgevingen, Zie [Hoe naar maken een App-omgeving][HowToCreateAnAppServiceEnvironment]

Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

Zie [Overzicht van de architectuur netwerk] voor een overzicht van de App-omgeving netwerkarchitectuur,[ NetworkArchitectureOverview] artikel.

Zie voor meer informatie over het gebruik van een App-Service-omgeving met ExpressRoute, het volgende artikel [Express Route]en App serviceomgevingen[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
