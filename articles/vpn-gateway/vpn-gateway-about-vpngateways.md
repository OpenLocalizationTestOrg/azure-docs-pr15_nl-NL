<properties 
   pageTitle="Over VPN Gateway | Microsoft Azure"
   description="Meer informatie over VPN Gateway verbindingen voor Azure virtuele netwerken."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Over VPN Gateway


Een gateway virtueel netwerk wordt gebruikt om netwerkverkeer tussen Azure virtuele netwerken en on-premises implementatie locaties en ook tussen virtuele netwerken binnen Azure (VNet VNet) te verzenden. Als u een VPN-gateway configureert, moet u maken en configureren van een gateway virtueel netwerk en een virtuele gateway netwerkverbinding.

In het implementatiemodel resourcemanager wanneer u een virtueel netwerk gateway resource, maakt opgeven u verschillende instellingen. Een van de vereiste instellingen is '-GatewayType'. Er zijn twee virtueel netwerk gateway typen: Vpn en ExpressRoute. 

Wanneer netwerkverkeer is een speciale privé verbinding verstuurd, gebruikt u het type gateway 'ExpressRoute'. Dit is ook een gateway ExpressRoute genoemd. Wanneer netwerkverkeer versleutelde via een openbare verbinding is verstuurd, gebruikt u het type gateway 'Vpn'. Dit is een VPN-gateway genoemd. Site-naar-Site, punt-naar-Site en VNet-naar-VNet verbindingen gebruiken een VPN-gateway.

Elke virtueel netwerk kunt slechts één virtueel netwerkgateway per gateway type hebben. U kunt bijvoorbeeld een virtueel netwerkgateway die wordt gebruikt - GatewayType ExpressRoute en een die wordt gebruikt - GatewayType Vpn hebben. In dit artikel gaat hoofdzakelijk over VPN Gateway. Zie voor meer informatie over ExpressRoute [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Prijzen

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Gateway-SKU 's

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Zie voor meer informatie over gateway SKU's, [Gateway SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Geschatte geaggregeerde doorvoer door SKU

De volgende tabel ziet u de gateway-typen en de geschatte geaggregeerde doorvoer. In deze tabel geldt voor de resourcemanager en de klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Een VPN-Gateway configureren

Als u een VPN-gateway configureert, afhankelijk van de instructies die u gebruikt de implementatiemodel dat u gebruikt om te maken van uw virtuele netwerk. Bijvoorbeeld als u uw VNet met het implementatiemodel klassieke hebt gemaakt, kunt u de richtlijnen en instructies voor het implementatiemodel klassieke maken en configureren van de VPN gateway-instellingen. Zie voor meer informatie over de implementatiemodellen, [lidmaatschap resourcemanager en klassieke implementatiemodellen](../resource-manager-deployment-model.md).

Een gateway VPN-verbinding is afhankelijk van meerdere resources die zijn geconfigureerd met bepaalde instellingen. Bijna alle bronnen kan worden geconfigureerd afzonderlijk, hoewel deze in een bepaalde volgorde in sommige gevallen moeten worden geconfigureerd. U kunt maken en configureren van bronnen met één configuratiehulpprogramma, zoals de portal van Azure starten. U kunt vervolgens later besluit om te schakelen naar een ander hulpmiddel, zoals PowerShell, voor het configureren van extra bronnen of voor het wijzigen van bestaande resources indien van toepassing. Op dit moment configureren u niet elke resource en resource-instelling in de portal van Azure. De instructies in de artikelen voor elke verbinding topologie opgeven wanneer een hulpmiddel specifieke configuratie is vereist. Zie voor informatie over de afzonderlijke resources en instellingen voor VPN Gateway [over VPN Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).

De volgende secties bevatten tabellen die vermelden:

- beschikbare implementatiemodel
- beschikbare configuratiehulpprogramma
- koppelingen waarmee u rechtstreeks naar een artikel, indien beschikbaar.

Gebruik de diagrammen en beschrijvingen om te voldoen aan vereisten voor de topologie verbinding selecteren. De diagrammen weergeven de belangrijkste basislijn topologieën, maar het is mogelijk te maken van complexere configuraties de diagrammen als uitgangspunt gebruiken.

## <a name="site-to-site-and-multi-site"></a>Site-naar-Site en multi-Site

### <a name="site-to-site"></a>Site-naar-Site

Een Site-naar-Site (S2S) VPN gateway-verbinding is een verbinding via VPN IPsec/IKE (IKEv1 of IKEv2)-tunnel. Dit type verbinding vereist een VPN-apparaat on-premises implementatie met een openbare IP-adres dat is toegewezen aan deze bevinden en zich niet achter een NAT bevindt. S2S verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.   

![S2S verbinding] (./media/vpn-gateway-about-vpngateways/demos2s.png "site-naar-site")


### <a name="multi-site"></a>Meerdere sites

U kunt maken en configureren van een gateway VPN-verbinding tussen uw VNet en meerdere on-premises implementatie-netwerken te gebruiken. Tijdens het werken met meerdere verbindingen, moet u een type RouteBased VPN (dynamische gateway voor klassieke VNets). Omdat een VNet slechts één VPN gateway hebben kan, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit is vaak de verbinding met een 'meerdere site' genoemd.
 

![Meerdere locaties verbinding] (./media/vpn-gateway-about-vpngateways/demomulti.png "meerdere sites")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Implementatiemodellen en methoden voor Site-naar-Site en meerdere sites

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet-naar-VNet

Een virtueel netwerk verbinden met een ander virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinden met een on-premises site-locatie. Beide typen connectivity gebruiken een VPN-gateway te leveren van een beveiligde tunnel met IPsec/IKE. U kunt zelfs VNet-naar-VNet communicatie met meerdere locaties verbinding configuraties combineren. Hiermee kunt u stand brengen van netwerktopologieën die cross-premises-connectiviteit met onderling virtuele netwerkconnectiviteit combineren.

De VNets die u verbinding maakt kan zijn:

- in dezelfde of een andere regio 's
- in de dezelfde of een andere abonnementen 
- in de dezelfde verschillende implementatiemodellen


![VNet naar VNet verbinding] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-naar-vnet")

#### <a name="connections-between-deployment-models"></a>Verbindingen tussen de implementatiemodellen voor

Azure momenteel heeft twee implementatiemodellen: klassieke en resourcemanager. Als u Azure hebt gebruikt voor het enige tijd, hebt u waarschijnlijk Azure VMs en exemplaar rollen in een klassieke VNet uitgevoerd. Uw nieuwere VMs en exemplaren van de rol kunnen worden uitgevoerd in een VNet in resourcemanager gemaakt. U kunt een verbinding maken tussen de VNets toe te staan dat de bronnen in één VNet om rechtstreeks met resources in een andere te communiceren.

#### <a name="vnet-peering"></a>VNet peering

U kunt mogelijk VNet peering om uw verbinding, te maken, zo lang maken als uw virtuele netwerk voldoet aan bepaalde voorwaarden gebruiken. VNet peering geen gebruikmaakt van een gateway virtueel netwerk. Zie [VNet peering](../virtual-network/virtual-network-peering-overview.md)voor meer informatie.


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Implementatiemodellen en methoden voor het VNet-naar-VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punt-naar-Site

Een punt-naar-Site (P2S) VPN gateway-verbinding kunt u een beveiligde verbinding maken met het virtuele netwerk vanaf een afzonderlijke clientcomputer. Een VPN-verbinding is P2S verstreken SSTP (Secure Socket Tunneling Protocol). P2S verbindingen hoeven niet een VPN-apparaat of een openbare IP-adres om te werken. U kunt de VPN-verbinding instellen vanaf de clientcomputer. Deze oplossing is handig als u wilt verbinden met uw VNet vanaf een externe locatie, zoals thuis of een telefonische vergadering, of als er slechts een paar clients die u moeten verbinding maken met een VNet. P2S verbindingen kunnen worden gebruikt in combinatie met S2S verbindingen tot en met dezelfde gateway VPN, mits alle configuratievereisten voor beide verbindingen compatibel zijn.


![Punt-naar-site verbinding] (./media/vpn-gateway-about-vpngateways/demop2s.png "punt-naar-site")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Implementatiemodellen en methoden voor het punt-naar-Site

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

In een ExpressRoute-verbinding, is een gateway virtueel netwerk geconfigureerd met het type gateway 'ExpressRoute', in plaats van 'Vpn'. Zie voor meer informatie over ExpressRoute [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Site-naar-Site en ExpressRoute naast elkaar bestaande verbindingen

ExpressRoute is een directe, speciale verbinding tussen uw WAN (niet via de openbare Internet) bij Microsoft-Services, met inbegrip van Azure. Site-naar-Site VPN verkeer reis versleuteld via de openbare Internet. Kunnen naar website VPN en ExpressRoute verbindingen configureren voor hetzelfde virtuele netwerk heeft verschillende voordelen.

U kunt een VPN-verbinding voor de Site-naar-Site configureren als een pad secure failover voor ExpressRoute of Site-naar-Site VPN's met verbinding maken met sites die geen deel uitmaken van uw netwerk, maar die zijn verbonden via ExpressRoute. Melding dat hiervoor is vereist twee virtueel netwerkgateways voor hetzelfde virtuele netwerk, één met - GatewayType Vpn en de andere - GatewayType ExpressRoute gebruiken.


![De verbinding gebruiken] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Implementatiemodellen en methoden voor S2S en ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Volgende stappen

De configuratie van de gateway VPN plannen. Zie [VPN Gateway Planning en ontwerp](vpn-gateway-plan-design.md) en [uw on-premises netwerk naar Azure verbinding te maken](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
