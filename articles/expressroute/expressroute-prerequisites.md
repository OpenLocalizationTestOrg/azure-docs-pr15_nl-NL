<properties
   pageTitle="Vereisten voor ExpressRoute adoptie | Microsoft Azure"
   description="Deze pagina bevat een lijst met waaraan moet worden voldaan voordat u een circuitlijnen Azure ExpressRoute kunt ordenen."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute vereisten & Controlelijst  

Als u wilt verbinden met Microsoft cloudservices ExpressRoute gebruiken, moet u controleren of dat de volgende vereisten wordt vermeld in de onderstaande secties is voldaan.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-account

- Een geldig en actieve Microsoft Azure-account. Dit is vereist voor het instellen van de circuitlijnen ExpressRoute. ExpressRoute circuits zijn bronnen binnen Azure abonnementen. Een Azure-abonnement is vereist, zelfs als er verbinding is beperkt tot Azure Microsoft cloudservices, zoals Office 365-services en CRM online.
- Een actief Office 365-abonnement (als u Office 365-services). Zie de sectie [Office 365 over de vereisten](#office-365-specific-requirements) van dit artikel voor meer informatie.

## <a name="connectivity-provider"></a>Connectivity-provider
- U kunt werken met een [ExpressRoute connectivity partner](expressroute-locations.md#partners) verbinding maken met de Microsoft-cloud. U kunt een verbinding tussen uw on-premises netwerk en Microsoft op [drie manieren](expressroute-introduction.md#howtoconnect)instellen. 
- Als uw provider een ExpressRoute connectivity-partner is, kunt u nog steeds verbinding maken met de cloud Microsoft via een [exchange-provider cloud](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Netwerkvereisten
- **Overtollige connectivity**: Er is geen redundantievereiste op fysieke verbinding tussen u en uw provider. Microsoft vereist overtollige BGP sessies worden ingesteld tussen Microsofts routers en de peering routers, zelfs wanneer er slechts [één fysieke verbinding met een exchange cloud](expressroute-faqs.md#onep2plink). 
- **Routering**: afhankelijk van hoe u verbinding maakt met de Cloud Microsoft, u of uw provider moet instellen en beheren van de BGP sessies voor het [routeren domeinen](expressroute-circuit-peerings.md). Sommige Ethernet connectivity-provider of cloud exchange provider mogelijk bieden BGP management als een waarde service toevoegen.
- **NAT**: Microsoft accepteert alleen openbare IP-adressen via Microsoft peering. Als u privé IP-adressen in uw on-premises netwerk gebruikt, adressen u of uw provider nodig om te vertalen de privé IP-adressen naar het openbare IP-voor [gebruik van de NAT](expressroute-nat.md).
- **QoS**: Skype voor bedrijven heeft verschillende services (bijvoorbeeld spraak en video, tekst) waarvoor gesplitste QoS behandeling. U en uw provider, moeten de [QoS vereisten](expressroute-qos.md)volgen.
- **Netwerkbeveiliging**: u [netwerkbeveiliging](../best-practices-network-security.md) rekening moet houden wanneer u verbinding maakt met de Cloud Microsoft via ExpressRoute.
 
## <a name="office-365"></a>Office 365

Als u van plan bent om in te schakelen van Office 365 op ExpressRoute, raadpleegt u de volgende documenten voor meer informatie over vereisten voor Office 365.


- [Overzicht van ExpressRoute voor Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routering met ExpressRoute voor Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365-URL's en IP-adresbereiken](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Netwerkplanning en prestaties optimaliseren voor Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Netwerkbandbreedteberekeningen en hulpprogramma's voor](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365-integratie met on-premises omgevingen](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Als u van plan bent om in te schakelen CRM Online op ExpressRoute, raadpleegt u de volgende documenten voor meer informatie over CRM Online

- [CRM Online-URL's](https://support.microsoft.com/kb/2655102) en [IP-adresbereiken](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over ExpressRoute.
- Een ExpressRoute connectivity-provider zoeken. Zie [ExpressRoute partners en peering locaties](expressroute-locations.md).
- Zie vereisten voor de [Routering](expressroute-routing.md), [NAT](expressroute-nat.md) en [QoS](expressroute-qos.md).
- Configureer uw verbinding ExpressRoute.
    - [ExpressRoute circuits maken](expressroute-howto-circuit-classic.md)
    - [Routering configureren](expressroute-howto-routing-classic.md)
    - [Een VNet koppelen aan een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-classic.md)

