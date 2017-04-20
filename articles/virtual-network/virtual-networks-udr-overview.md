<properties 
   pageTitle="Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen?"
   description="Informatie over het gebruik van gebruiker gedefinieerd Routes (UDR) en doorsturen van IP-verkeer wilt doorsturen naar het netwerk van virtuele toestellen in Azure wordt aangegeven."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen?
Wanneer u virtuele machines (VMs) aan een virtueel netwerk (VNet) in Azure wordt aangegeven toevoegen, ziet u dat de VMs met elkaar communiceren via het netwerk, automatisch zijn. U hoeft niet te geven van een gateway, hoewel de VMs in verschillende subnetten zijn. Hetzelfde geldt voor communicatie vanaf de VMs met openbare Internet en zelfs naar uw on-premises netwerk wanneer de verbinding van een hybride vanuit Azure met uw eigen datacenter aanwezig is.

Deze stroom van communicatie is mogelijk omdat Azure een reeks systeem routes gebruikt om te bepalen hoe IP-verkeer loopt. Systeem routes bepaalt de stroom van communicatie in de volgende scenario's:

- Uit in hetzelfde subnet.
- Vanuit een subnet binnen een VNet.
- Uit VMs met Internet.
- Vanuit een VNet naar een andere VNet via een VPN-gateway.
- Vanuit een VNet naar uw on-premises netwerk via een VPN-gateway.

De onderstaande afbeelding ziet u een eenvoudige installatie met een VNet, twee subnetten en een paar VMs, samen met de systeem-routes die IP-verkeer toestaan.

![Systeem routes in Azure wordt aangegeven](./media/virtual-networks-udr-overview/Figure1.png)

Hoewel het gebruik van systeem routes verkeer automatisch voor de implementatie vergemakkelijkt, zijn er omstandigheden waarin u wilt bepalen de routering van pakketten door een virtueel toestel. U kunt zo door te maken van de gebruiker gedefinieerde routes die opgeeft de volgende hop voor pakketten die doorloopt naar een specifieke subnet, gaat u naar uw virtuele toestel in plaats daarvan en het IP-doorsturen voor VM wordt uitgevoerd met het virtuele toestel inschakelen.

De onderstaande afbeelding ziet u een voorbeeld van de gebruiker gedefinieerde routes en doorsturen van IP-pakketten afdwingen verzonden naar één subnet van elkaar aftrekken om te gaan door een virtueel toestel in een derde subnet.

![Systeem routes in Azure wordt aangegeven](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Door de gebruiker gedefinieerde routes worden alleen toegepast op verkeer een subnet verlaten. u kunt geen routes om op te geven hoe verkeer komt in een subnet van Internet, bijvoorbeeld maken. Het toestel dat u verkeer naar stuurt kan ook niet in hetzelfde subnet waaruit het verkeer afkomstig is. Altijd een afzonderlijk subnet maken voor uw apparaten. 

## <a name="route-resource"></a>Route resource
Pakketten worden gerouteerd via een TCP/IP-netwerk op basis van een tabel bij elk knooppunt in een netwerk met de fysieke gedefinieerd. Een tabel is een verzameling afzonderlijke routes gebruikt om te bepalen waar pakketten op basis van het doeladres door te sturen. Er bestaat een route van de volgende opties:

|Eigenschap|Beschrijving|Beperkingen|Overwegingen|
|---|---|---|---|
| Adresvoorvoegsel | De bestemming CIDR waarop de route van toepassing, zoals 10.1.0.0/16 is.|Moet worden een geldig CIDR bereik dat staat voor adressen op de openbare Internet, Azure virtuele netwerk of on-premises implementatie datacenter.|Controleer of dat het **adresvoorvoegsel** bevat geen het adres voor het **hopadres van de volgende**, anders uw pakketten wordt ingevoerd in een lus vormen die van de bron naar de volgende hop zonder dat de bestemming ooit bereikt. |
| Volgende hop type | Het type Azure hop het pakket moet worden verzonden naar. | Moet een van de volgende waarden: <br/> **Virtual Network**. Hiermee geeft u het lokale virtuele netwerk. Als u twee subnetten, 10.1.0.0/16 en 10.2.0.0/16 in hetzelfde virtuele netwerk, wordt de route voor elk subnet in de routetabel bijvoorbeeld een volgende hop-waarde van *Virtual Network*hebt. <br/> **Virtual Network Gateway**. Hiermee geeft u een VPN-Azure S2S Gateway. <br/> **Internet**. Hiermee geeft u de standaard-internetgateway verstrekt door de infrastructuur van Azure. <br/> **Virtuele toestel**. Hiermee geeft u een virtuele toestel die u hebt toegevoegd aan uw Azure virtuele netwerk. <br/> **Geen**. Hiermee geeft u een zwart gat. Pakketten doorgestuurd naar een gat zwart wordt niet helemaal worden doorgestuurd.| Kunt u overwegen een type **geen** stoppen pakketten van die doorloopt naar een opgegeven bestemming. | 
| Adres volgende hop | Het hopadres van de volgende bevat het IP-adres pakketten moeten worden doorgestuurd. De waarden voor de volgende hop zijn alleen toegestaan in routes waar het volgende hop type *Virtual toestel is*.| Moet u een IP-adres dat is bereikbaar binnen het virtuele netwerk waar de Route door gebruiker gedefinieerde is toegepast. | Als het IP-adres een VM vertegenwoordigt, zorg er dan voor dat u [IP doorsturen](#IP-forwarding) in Azure inschakelen voor de VM. |

In Azure PowerShell hebben enkele van de waarden 'NextHopType' verschillende namen:
- Virtuele netwerk is VnetLocal
- Virtuele netwerkgateway is VirtualNetworkGateway
- Virtuele toestel is VirtualAppliance
- Internet is Internet
- Geen is geen

### <a name="system-routes"></a>Systeem Routes
Elke subnetten die is gemaakt in een virtueel netwerk wordt automatisch gekoppeld aan een tabel met de volgende regels van de systeem-route:

- **Lokale Vnet regel**: met deze regel wordt automatisch gemaakt voor elke subnet in een virtueel netwerk. Geeft aan dat er een directe koppeling tussen de VMs in de VNet is en er geen tussenliggende volgende hop is.
- **On-premises regel**: deze regel geldt voor alle verkeer naar de on-premises implementatie-adresbereiken en VPN gateway gebruikt als de bestemming van de volgende hop.
- **Regels voor Internet**: deze regel grepen al het verkeer bestemd de openbare internetverbinding (adres voorvoegsel 0.0.0.0/0) en de infrastructuur internetgateway gebruikt als de volgende hop voor al het verkeer bestemd met Internet.

### <a name="user-defined-routes"></a>Door gebruiker gedefinieerde Routes
Voor de meeste omgevingen moet u alleen de systeem-routes die al zijn gedefinieerd door Azure. Mogelijk moet u echter een routetabel maken en toevoegen van een of meer routes in bepaalde omstandigheden kunnen geven, zoals:

- Afdwingen tunnel naar Internet via uw on-premises netwerk.
- Gebruik van virtuele toestellen in uw Azure-omgeving.

U moet in de bovenstaande scenario's maken van een tabel en de gebruiker gedefinieerde routes aan toe te voegen. U kunt meerdere routetabellen hebben en dezelfde routetabel kan worden gekoppeld aan een of meer subnetten. En elk subnet kan alleen worden gekoppeld aan een tabel met één route. Alle VMs en cloudservices in een subnet gebruik de tabel die is gekoppeld aan dat subnet.

Subnetten, is afhankelijk van wat u systeem routes totdat een routetabel gekoppeld aan het subnet is. Nadat u een koppeling bestaat, is routering klaar gebaseerd op langste voorvoegsel vergelijken (LPM) tussen de gebruiker gedefinieerde routes zowel systeem routes. Als er meer dan één route met de dezelfde LPM overeenkomen is vervolgens een route geselecteerd op basis van het beginpunt in de volgende volgorde:

1. Gebruiker gedefinieerde doorsturen
1. BGP route (wanneer ExpressRoute wordt gebruikt)
1. Systeem doorsturen

Voor informatie over het maken van de gebruiker gedefinieerde routes, Zie [hoe u Routes maken en doorsturen via IP in Azure wordt aangegeven](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Door de gebruiker gedefinieerde routes gelden alleen voor Azure VMs en cloud services. Bijvoorbeeld als u toevoegen van een firewall virtuele toestel tussen uw on-premises netwerk en Azure wilt, wordt er een door de gebruiker gedefinieerde route voor uw routetabellen Azure die alle verkeer gaat u naar de on-premises implementatie-adresruimte tot het virtuele toestel doorstuurt wilt maken. U kunt ook een door de gebruiker gedefinieerde route (UDR) toevoegen aan de GatewaySubnet voor het doorsturen van al het verkeer vanuit on-premises naar Azure via de virtuele toestel. Dit is een recente toevoeging.

### <a name="bgp-routes"></a>BGP Routes
Als u een ExpressRoute verbinding tussen uw on-premises netwerk en Azure hebt, kunt u BGP doorgeven routes uit uw on-premises netwerk naar Azure inschakelen. Deze routes BGP worden gebruikt op dezelfde manier als systeem routes en door de gebruiker gedefinieerde routes in elk Azure subnet. Zie [Inleiding ExpressRoute](../expressroute/expressroute-introduction.md)voor meer informatie.

>[AZURE.IMPORTANT] U kunt uw Azure-omgeving als u wilt gebruiken dwingen tunnel via uw on-premises netwerk door te maken van een gebruiker gedefinieerde route voor subnet 0.0.0.0/0 die gebruikmaakt van de gateway VPN als de volgende hop configureren. Echter werkt dit alleen als u een VPN-gateway, niet ExpressRoute gebruikt. Via BGP is afgedwongen tunneling voor ExpressRoute geconfigureerd.

## <a name="ip-forwarding"></a>IP-doorsturen
Zoals hierboven wordt beschreven, is een van de belangrijkste redenen om te maken van een gebruiker gedefinieerde route verkeer naar een virtuele toestel doorsturen. Een virtuele toestel is niets meer dan een VM die wordt uitgevoerd als een toepassing gebruikt voor het verwerken van netwerkverkeer in sommige manier, zoals een firewall of een NAT-apparaat.

Dit virtuele toestel VM moet mogelijk binnenkomende verkeer ontvangen dat niet is geadresseerd aan zelf. Als u wilt toestaan dat een VM verkeer die zijn gericht aan andere bestemmingen ontvangen, moet u IP-doorsturen inschakelen voor VM. Dit is een Azure instellen, niet een instelling in het gastbesturingssysteem.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [maken van routes in het implementatiemodel resourcemanager-](virtual-network-create-udr-arm-template.md) en om deze te koppelen aan subnetten. 
- Informatie over het [maken van routes in het implementatiemodel klassieke](virtual-network-create-udr-classic-ps.md) en om deze te koppelen aan subnetten.
