<properties 
    pageTitle="App-omgeving | Microsoft Azure" 
    description="Wat is een Azure App Service-omgeving? Een introductie over het App-omgeving." 
    keywords="Azure app service-omgeving, virtueel netwerk, secure netwerken"
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

# <a name="app-service-environment-documentation"></a>App-omgeving servicedocumentatie

Een App-Service-omgeving is een [Premium] [ PremiumTier] service-abonnement optie van Azure-Service voor App vindt u een volledig geïsoleerd en speciale omgeving voor het uitvoeren van Azure-Service voor App-apps veilig op hoog niveau, met inbegrip van [Web Apps][WebApps], [Mobile-Apps][MobileApps], en [API-Apps][APIApps].  

App Service omgevingen zijn ideaal voor het vereisen van toepassing-werkbelastingen:

- Zeer hoog schaal
- Moeten worden geïsoleerd en beveiligde netwerktoegang

Klanten kunnen meerdere App serviceomgevingen binnen een enkel Azure, en tussen meerdere Azure regio's maken.  Hierdoor App serviceomgevingen ideaal voor het horizontaal schaalbaarheid staat minder lagen ter ondersteuning van hoge RPS werkbelasting.

App Service omgevingen zijn beperkt tot slechts één klant van toepassingen en altijd zijn geïmplementeerd in een virtueel netwerk.  Klanten fijnmazige controle over beide binnenkomende en uitgaande toepassing netwerkverkeer met [netwerk beveiligingsgroepen]hebt[NetworkSecurityGroups].  Toepassingen kunnen ook snelle beveiligde verbindingen maken via virtuele netwerken tot bedrijfsresources on-premises implementatie.

Apps moeten regelmatig toegang tot bedrijfsresources zoals interne databases en webservices.  Apps App serviceomgevingen waarop toegang tot bronnen bereikbaar via [Naar website] [ SiteToSite] VPN en [Azure ExpressRoute] [ ExpressRoute] verbindingen.

* [Wat is een App-Service-omgeving?](../app-service-web/app-service-app-service-environment-intro.md)
* [Maken van een App Service-omgeving](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Apps maken in een App Service-omgeving](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Maken en gebruiken van een interne taakverdeling met App Service omgevingen](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Een App Service-omgeving configureren](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Schaalbaarheid van Apps in een App Service-omgeving](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Netwerk beveiligings- en architectuur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Hoe u de

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Video 's
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
