<properties 
   pageTitle="VPN Gateway planning en ontwerp | Microsoft Azure"
   description="Meer informatie over het plannen van de VPN Gateway en een ontwerp voor cross-premises en hybride VNet-naar-VNet verbindingen"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planning en ontwerp voor VPN Gateway

Plannen en ontwerpen van uw cross-premises en VNet-naar-VNet configuraties kunnen zijn eenvoudige of complexe, afhankelijk van uw behoeften netwerken. In dit artikel begeleidt u bij eenvoudige aandachtspunten voor de planning en ontwerp.

## <a name="planning"></a>Planning


### <a name="compare"></a>Cross-premises connectiviteitsopties

Als u verbinden met uw on-premises implementatie-sites veilig een virtueel netwerk wilt, beschikt u over drie manieren doen: Site-naar-Site, punt-naar-Site en ExpressRoute. Vergelijk de verschillende cross-premises-verbindingen die beschikbaar zijn. De optie die u kiest kan afhangen van verschillende overwegingen, zoals:


- Welk soort doorvoer vereist uw oplossing?
- Wilt u communiceren via de openbare Internet via veilig VPN, of een privé verbinding?
- Hebt u een openbare IP-adres beschikbaar voor gebruik?
- Bent u van plan een VPN-apparaat gebruiken? Als dat het geval is, is het compatibel?
- U verbinding maakt slechts een paar computers, of wilt u een permanente verbinding voor uw site?
- Welk type VPN gateway is vereist voor de oplossing die u wilt maken?
- Welke gateway SKU moet u gebruiken?


Met behulp van de volgende tabel kunt u bepalen van de beste optie connectivity voor uw oplossing.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Gateway-vereisten door VPN-type en SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Zie voor meer informatie over gateway SKU's, [VPN Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Geaggregeerde doorvoer per SKU en VPN

De volgende tabel ziet u de gateway-typen en de geschatte geaggregeerde doorvoer. De geschatte geaggregeerde doorvoer mogelijk een bepalende correctiefactor voor het ontwerp van uw.
Prijzen verschillen tussen gateway SKU's. Zie [VPN Gateway prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/)voor informatie over prijzen. In deze tabel geldt voor de resourcemanager en de klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Ondersteunde configuraties per SKU en VPN

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Werkstroom

De volgende lijst bevat een overzicht van de algemene werkstroom voor het cloud connectivity:

1.  Ontwerp en plannen van de topologie van uw connectiviteit en lijst de spaties adres voor alle netwerken die u verbinding wilt maken.
2.  Een Azure virtuele netwerk maken. 
3.  Een gateway VPN voor het virtuele netwerk maken.
4.  Maken en verbindingen met on-premises implementatie-netwerken te gebruiken of andere virtuele netwerken configureren (indien nodig).
5.  Maken en configureren van een punt-naar-Site-verbinding voor uw Azure VPN gateway (indien nodig).
 

## <a name="design"></a>Ontwerp

### <a name="topologies"></a>Verbinding topologieën

Bekijk eerst de diagrammen in het artikel [Over VPN Gateway](vpn-gateway-about-vpngateways.md) . Het artikel bevat eenvoudige diagrammen, de implementatiemodellen voor elke topologie (resourcemanager of klassieke) en welke implementatie hulpmiddelen u kunt gebruiken om te implementeren van uw configuratie.   

### <a name="designbasics"></a>Databaseontwerp

De volgende secties worden de basisbeginselen van de gateway VPN. U kunt wellicht ook [netwerken services beperkingen](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Over subnetten

Wanneer u verbindingen maakt, moet u rekening houden met uw subnet bereiken. U kunt geen overlappende subnet-adresbereiken. Een overlappende subnet is wanneer u een virtueel netwerk of on-premises locatie bevat dezelfde adresruimte die de andere locatie bevat. Dit betekent dat u moet uw engineers netwerk voor uw lokale on-premises implementatie-netwerken te gebruiken om te kunnen fungeren uit een bereik kunt gebruiken voor uw IP Azure adressering van ruimte/subnetten. U moet adresruimte die niet wordt gebruikt op de lokale on-premises netwerk. 

Voorkomen overlappende subnetten is ook belangrijk wanneer u met VNet-naar-VNet verbindingen werkt. Als uw subnetten overlappen en een IP-adres in zowel het verzenden en de bestemming VNets bestaat, mislukt VNet-naar-VNet verbindingen. Azure niet kunt de gegevens doorsturen naar de andere VNet omdat het doeladres deel uit van het verzendende VNet maakt. 

VPN Gateways vereist een specifiek subnet, een gateway-subnet genoemd. Alle gateway-subnetten moeten de naam GatewaySubnet werkt alleen naar behoren. Zorg ervoor dat u niet een andere naam een naam geven uw subnet, gateway en VMs of iets anders op het subnet gateway niet implementeren. Zie de [Gateway-subnetten](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Over het lokale netwerkgateways

De gateway lokale netwerk wordt meestal verwijst naar uw on-premises locatie. In het implementatiemodel klassieke, worden de gateway lokale netwerk is een lokale netwerksite genoemd. Als u een lokale netwerkgateway configureert, u een naam geven, geef het openbare IP-adres van het apparaat van de VPN on-premises implementatie en de adresvoorvoegsels die in de on-premises locatie opgeven. Azure kijkt naar de bestemming adresvoorvoegsels voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor de gateway lokale netwerk en routeert pakketten dienovereenkomstig gewijzigd. U kunt deze adresvoorvoegsels naar wens aanpassen. Zie [lokale netwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng)voor meer informatie.


#### <a name="gwtype"></a>Over gateway typen

De juiste gateway tekst selecteren voor uw topologie is kritieke. Als u het verkeerde type selecteert, werkt de gateway niet goed. De gateway-type geeft aan hoe de gateway zelf verbinding maakt en is een vereiste configuratie-instelling voor het implementatiemodel resourcemanager.

De gateway-typen zijn:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Over verbindingstypen

Elke configuratie is vereist een specifiek verbindingstype. De verbindingstypen zijn:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Over VPN typen

Elke configuratie is vereist een specifiek type van de VPN. Als u twee configuraties combineren wilt, zoals het maken van de verbinding van een Site-naar-Site en de verbinding van een punt-naar-Site met de dezelfde VNet, moet u een VPN-type dat voldoet aan de eisen van beide verbinding gebruiken.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

De volgende tabellen bevatten het type VPN zoals deze wordt toegewezen aan elke verbindingsconfiguratie. Zorg ervoor dat het type VPN voor de gateway overeenkomt met de configuratie die u wilt maken. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>VPN-apparaten voor Site-naar-Site-verbindingen

Configureren van de verbinding van een Site-naar-Site, ongeacht de implementatiemodel, moet u de volgende items:

- Een VPN-apparaat die compatibel is met Azure VPN gateways
- Een openbare IPv4-IP-adres zijn dat zich niet achter een NAT

Moet u ervaring configureren van uw apparaat VPN hebben, of hebben iemand die het apparaat dat u kunt configureren. Zie voor meer informatie over VPN-apparaten, [over VPN-apparaten](vpn-gateway-about-vpn-devices.md). De VPN apparaten artikel bevat informatie over gevalideerde apparaten, vereisten voor apparaten die niet zijn gevalideerd en koppelingen naar apparaat configuratiedocumenten indien beschikbaar.

### <a name="forcedtunnel"></a>Houd rekening met het afgedwongen tunnel routering

U kunt voor de meeste configuraties configureren afgedwongen tunneling. Afgedwongen tunneling kunt u omleiden of "dwingen" alle Internet verkeer terug naar uw on-premises locatie via een VPN-Site-naar-Site tunnel voor controle en controle. Dit is een beveiligingsvereiste kritieke voor de meeste enterprise IT beleid. 

Zonder afgedwongen tunneling, wordt Internet verkeer uit uw VMs in Azure wordt aangegeven altijd doorlopen van Azure netwerkinfrastructuur rechtstreeks af met Internet, zonder de optie toe te staan dat u wilt controleren of het verkeer wilt controleren. Onbevoegde toegang tot Internet kan mogelijk leiden tot openbaarmaking of andere soorten inbreuk op beveiliging.

**U wordt gedwongen tunneling diagram**

![U wordt gedwongen Tunneling verbinding] (./media/vpn-gateway-plan-design/forced-tunnel.png "u wordt gedwongen tunneling")

Een afgedwongen tunneling verbinding kan worden geconfigureerd in beide implementatiemodellen en met behulp van verschillende hulpmiddelen. Zie de volgende tabel voor meer informatie. We bijwerken in deze tabel zodra nieuwe artikelen, nieuwe implementatiemodellen en extra's beschikbaar zijn voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar deze uit de tabel.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Volgende stappen

Zie de artikelen [VPN Gateway-Veelgestelde vragen](vpn-gateway-vpn-faq.md) en [Over VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie om u te helpen met het ontwerp van uw.

Zie voor meer informatie over specifieke gateway-instellingen, [Over VPN Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).




