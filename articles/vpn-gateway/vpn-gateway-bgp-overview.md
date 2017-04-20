<properties
   pageTitle="Overzicht van BGP met Azure VPN Gateways | Microsoft Azure"
   description="Dit artikel bevat een overzicht van BGP met Azure VPN Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Overzicht van BGP met Azure VPN Gateways

Dit artikel bevat een overzicht van ondersteuning voor BGP (Border Gateway Protocol) in Azure VPN Gateways.

## <a name="about-bgp"></a>Over BGP

BGP is het standaard protocol veelgebruikte in Internet uitwisseling van gegevens voor de Routering en bereikbaarheid tussen twee of meer netwerken. Als u dit gebruikt in de context van Azure virtuele netwerken, BGP Azure VPN Gateways kunt en uw on-premises implementatie VPN apparaten, genaamd BGP collega's of neighbors uitwisselen "stuurt" die worden beide gateways op de beschikbaarheid en de bereikbaarheid van deze voorvoegsels doorlopen van de gateways of routers informeren betrokken. BGP kunt ook uitschakelen overdracht routering tussen meerdere netwerken doorgeven waarmee die een gateway BGP van één BGP peer aan alle andere BGP collega's hoort.
 
### <a name="why-use-bgp"></a>Waarom BGP gebruiken?

BGP is een optionele functie die u met Azure Route gebaseerde VPN gateways gebruiken kunt. U moet er ook voor zorgen dat uw on-premises implementatie VPN apparaten ondersteunen BGP voordat u de functie inschakelen. U kunt blijven gebruiken Azure VPN gateways en uw on-premises implementatie VPN apparaten zonder BGP. Dit is het equivalent van het gebruik van statische routes (zonder BGP) *versus* met dynamische routering met BGP tussen uw netwerken en Azure.

Er zijn verschillende voordelen en nieuwe mogelijkheden met BGP:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Ondersteuning voor automatische en flexibele voorvoegsel updates

Met BGP hoeft u alleen een minimale voorvoegsel naar een specifieke BGP peer via de tunnel IPsec S2S VPN declareren. Het kan zijn kleinst een voorvoegsel host (/ 32) van het BGP peer IP-adres van uw apparaat van de VPN on-premises implementatie. U kunt bepalen welke lokale netwerk voorvoegsels die reclame maken voor aan Azure toe te staan dat uw Azure Virtual Network voor toegang tot de gewenste.
    
U kunt ook de reclame maken voor een grotere voorvoegsels die mogelijk enkele van uw voorvoegsels VNet adres, zoals een grote privé IP-adres spatie (bijvoorbeeld 10.0.0.0/8) bevatten. Houd er rekening mee Hoewel de voorvoegsels mag niet identiek zijn met een van uw VNet voorvoegsels voor eenheden. Deze gelijk aan uw voorvoegsels VNet routes gaan verloren.

>[AZURE.IMPORTANT] Op dit moment worden de standaard-route (0.0.0.0/0) naar Azure VPN gateways advertenties geblokkeerd. Verdere update krijgt wanneer deze mogelijkheid is ingeschakeld.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Ondersteuning voor meerdere tunnels tussen een VNet en een on-premises site met automatische failover op basis van BGP

U kunt meerdere verbindingen tot stand brengen tussen uw VNet Azure en on-premises implementatie VPN apparaten op dezelfde locatie. Deze functionaliteit biedt meerdere tunnels (paden) tussen de twee netwerken in een actieve configuratie. Als een de tunnel nadat de verbinding is verbroken, wordt de bijbehorende routes vervalt via BGP en wordt het verkeer worden automatisch verplaatst naar de resterende tunnels.
    
In het volgende diagram ziet u een eenvoudig voorbeeld van deze instelling zeer beschikbaar:
    
![Meerdere actieve paden](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Ondersteuning voor overdracht routering tussen uw on-premises implementatie-netwerken en meerdere Azure VNets

BGP kan meerdere gateways te leren en doorgeven voorvoegsels uit verschillende netwerken, ongeacht of ze rechtstreeks of onrechtstreeks verbonden bent. Dit kunt inschakelen overdracht routering met Azure VPN gateways tussen uw on-premises implementatie-sites of tussen meerdere Azure virtuele netwerken.
    
In het volgende diagram ziet u een voorbeeld van de topologie van een meerdere hop met meerdere paden die verkeer tussen de twee on-premises implementatie-netwerken via Azure VPN gateways binnen de Microsoft-Networks kunt douanevervoer:

![Meerdere hop overdracht](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP Veelgestelde vragen


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Volgende stappen

Zie [aan de slag met BGP op Azure VPN gateways](./vpn-gateway-bgp-resource-manager-ps.md) voor stapsgewijze instructies voor het configureren van BGP voor uw cross-premises en VNet-naar-VNet verbindingen.

