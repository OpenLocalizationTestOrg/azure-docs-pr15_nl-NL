<properties
   pageTitle="Uw on-premises netwerk verbinden met Azure | Microsoft Azure"
   description="Dit artikel wordt uitgelegd en verschilt van de verschillende methoden om verbinding te maken met Microsoft cloudservices zoals Azure, Office 365 en Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Uw on-premises netwerk verbinden met Azure

Microsoft biedt diverse soorten cloudservices. Terwijl u alle services via de openbare Internet verbinden kunt, kunt u ook verbinding maakt met enkele van de services die via een VPN (VPN) tunnel via Internet of via een directe, particuliere verbinding naar Microsoft. In dit artikel kunt u bepalen welke optie connectivity wordt best uw behoeften op basis van de soorten Microsoft cloudservices die u wilt gebruiken. De meeste organisaties maken gebruik van meerdere verbindingstypen hieronder beschreven.

## <a name="connecting-over-the-public-internet"></a>Verbinding maken via de openbare Internet

Dit verbindingstype biedt toegang tot Microsoft-cloudservices rechtstreeks via Internet, zoals hieronder wordt weergegeven.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Deze verbinding is meestal het eerste dat wordt gebruikt om verbinding te maken met Microsoft cloudservices. De onderstaande tabel bevat de voor- en nadelen van dit verbindingstype.



| **Voordelen**| **Overwegingen**|
|---------|---------|
|Geen wijziging van uw on-premises netwerk vereist als alle clientapparaten hebt onbeperkte toegang tot alle IP-adressen en poorten op Internet.|Hoewel verkeer is vaak versleuteld met HTTPS, kan deze worden onderschept aangezien het web openbare surft.|
|Alle Microsoft-cloudservices blootgesteld aan Internet openbare kunt verbinden.|Onverwachte latentie omdat de verbinding web surft.|
|Uw bestaande internetverbinding gebruikt.||
|Beheer van de connectivity-apparaten, geen vereist.||

Deze verbinding heeft geen connectivity of de bandbreedtekosten, omdat u uw bestaande internetverbinding gebruiken. 

## <a name="connecting-with-a-point-to-site-connection"></a>Verbinding maken met een punt-naar-site-verbinding

Dit verbindingstype biedt toegang tot sommige Microsoft cloud-services via een tunnel Tunneling Protocol SSTP (Secure Socket) via Internet, zoals hieronder wordt weergegeven.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Punt-naar-site verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar hiervoor moet u een VPN-Azure Gateway gebruiken. De onderstaande tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen**|
|---------|---------|
|Geen wijziging van uw on-premises netwerk vereist als alle clientapparaten hebt onbeperkte toegang tot alle IP-adressen en poorten op Internet.|Hoewel verkeer is versleuteld met IPSec, kan deze worden onderschept aangezien het web openbare surft.|
|Uw bestaande internetverbinding gebruikt.|Onverwachte latentie omdat de verbinding web surft.|
|Doorvoer maximaal 200 Mb/s per gateway.|Maken en beheren van afzonderlijke verbindingen tussen elk apparaat op uw on-premises netwerk en elke gateway die elk apparaat verbinding maken moet met vereist.|
|Kan worden gebruikt om te verbinden met Azure services die kunnen worden verbonden met een Azure virtuele netwerken (VNet) zoals Azure virtuele Machines en Azure-Cloudservices.|Minimale doorlopende beheer van een VPN-Azure Gateway vereist.|
||Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kunnen niet worden gebruikt.
||Verbinding maken met Azure services die niet worden verbonden met een VNet kunnen niet worden gebruikt.|

Meer informatie over de [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) -service, de [prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway)en de uitgaande gegevens doorverbinden [prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Verbinding maken met de verbinding van een site-naar-site

Dit verbindingstype biedt toegang tot enkele Microsoft cloud-services via een IPSec-tunnel via Internet, zoals hieronder wordt weergegeven.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Site-naar-site verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar hiervoor moet u gebruiken van een Gateway Azure VPN met de bijbehorende prijzen en de overdracht van uitgaande gegevens prijzen. De onderstaande tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen**|
|---------|---------|
|Alle apparaten in uw on-premises netwerk kunnen communiceren met Azure services die zijn verbonden met een VNet, dus is er niet nodig om afzonderlijke verbindingen voor elk apparaat te configureren.|Hoewel verkeer is versleuteld met IPSec, kan deze worden onderschept aangezien het web openbare surft.|
|Uw bestaande internetverbinding gebruikt.|Onverwachte latentie omdat de verbinding web surft.|
|Verbinding maken met Azure-services die kunnen worden verbonden aan een VNet zoals virtuele Machines en Cloud Services kan worden gebruikt.|Moet configureren en beheren van een gevalideerde VPN apparaat * on-premises implementatie.|
|Doorvoer maximaal 200 Mb/s per gateway.|Minimale doorlopende beheer van een VPN-Azure Gateway vereist.|
|Uitgaand verkeer gestart vanaf cloud virtuele machines via de on-premises netwerk voor controle en registratie in het gebruik van de gebruiker gedefinieerde routes of de rand Gateway Protocol (BGP) kunt afdwingen **.|Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kunnen niet worden gebruikt.|
||Verbinding maken met Azure services die niet worden verbonden met een VNet kunnen niet worden gebruikt.|
||Als u services die verbindingen terug naar de on-premises implementatie apparaten initiëren gebruikt en uw beveiligingsbeleid voor apparaten dit nodig is, moet u mogelijk een firewall tussen de on-premises netwerk en Azure.|

- * Een overzicht van de [VPN-apparaten gevalideerd](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Meer informatie over het gebruik van [de gebruiker gedefinieerde routes](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) of [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) afdwingen van Azure VNets zoekresultaten omleiden naar een on-premises implementatie-apparaat.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbinding maken met een speciale privé verbinding

Dit verbindingstype biedt toegang tot alle Microsoft-cloudservices via een speciale privé verbinding naar Microsoft die niet door Internet bladeren wordt, zoals hieronder wordt weergegeven.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute verbinding")

De verbinding is vereist voor gebruik van de service ExpressRoute en een verbinding met een provider connectivity. De onderstaande tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen**| **Overwegingen**|
|---------|---------|
|Verkeer kan niet worden onderschept tijdens overdracht via de openbare Internet, omdat een speciale verbinding via een serviceprovider wordt gebruikt.|On-premises implementatie router management vereist.|
|Bandbreedte maximaal 10 Gb/s per ExpressRoute circuitlijnen en doorvoer maximaal 2 Gb/s tot elke gateway.|Vereist een speciale verbinding met een provider connectivity.|
|Overzichtelijk latentie omdat dit is een speciale verbinding naar Microsoft die niet via Internet verloopt.|Mogelijk minimale doorlopende beheer van een of meer Azure VPN Gateways (als de circuitlijnen verbinden met VNets).|
|Niet vereist versleutelde communicatie, hoewel u kunt het verkeer, indien gewenst coderen.| Als u de cloudservices die verbindingen terug naar de on-premises implementatie apparaten initiëren gebruikt, moet u mogelijk een firewall tussen de on-premises netwerk en Azure.|
|Kan rechtstreeks verbinding maken met alle Microsoft-cloudservices, met een paar uitzonderingen *.|NAT (NAT) vereist van on-premises implementatie IP-adressen voor het invoeren van de Microsoft-datacenters voor services die niet worden verbonden met een VNet.* *|
|Kunt afdwingen uitgaand verkeer gestart vanaf cloud virtuele machines via de on-premises netwerk voor controle en registratie BGP gebruiken.|

- * Een [lijst met services](../expressroute/expressroute-faqs.md#supported-services) die niet kunnen worden gebruikt met ExpressRoute weergeven. Uw abonnement op Azure moet worden goedgekeurd als u wilt verbinden met Office 365.  Zie het artikel [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) voor meer informatie.
- ** Meer informatie over ExpressRoute [NAT](../expressroute/expressroute-nat.md) vereisten.

Meer informatie over [ExpressRoute](../expressroute/expressroute-introduction.md), de bijbehorende [prijzen](https://azure.microsoft.com/pricing/details/expressroute)en [connectivity providers](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Aanvullende overwegingen

- Meer van de bovenstaande opties hebben verschillende maximumlimieten die voor VNet verbindingen, gateway-verbindingen en andere criteria kunt ondersteunen. Het wordt aanbevolen dat u bekijken van de Azure [netwerken beperkt](../azure-subscription-service-limits.md#networking-limits) als u wilt weten als een van deze van invloed zijn op de connectivity typen die u wilt gebruiken. 
- Als u van plan bent een gateway uit een VPN-verbinding voor site-naar-site met de dezelfde VNet als een gateway ExpressRoute, moet u vertrouwd raken met belangrijke beperkingen eerst. Zie het artikel [ExpressRoute configureren en Site-naar-Site naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

De volgende informatiebronnen wordt uitgelegd hoe u het implementeren van de verbindingstypen die in dit artikel.

-   [Een verbinding punt-naar-site implementeren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [De verbinding van een site-naar-site implementeren](guidance-hybrid-network-vpn.md)
-   [Een speciale privé verbinding implementeren](guidance-hybrid-network-expressroute.md)
-   [Een speciale privé verbinding met een site op site-verbinding voor maximale beschikbaarheid implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)
