<properties
   pageTitle="Overzicht van de Azure Virtual Network (VNet)"
   description="Meer informatie over virtuele netwerken (VNets) in Azure wordt aangegeven."
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

# <a name="virtual-network-overview"></a>Overzicht van Virtual Network

Een Azure virtuele netwerk (VNet) is een weergave van uw eigen netwerk in de cloud.  Het is een logische moeten worden geïsoleerd van de Azure cloud toegewezen aan uw abonnement. U kunt volledig bepalen het IP-adresblokken, DNS-instellingen beveiligingsbeleid voor apparaten en routetabellen in dit netwerk. U kunt ook verder uw VNet segmenteren in subnetten en IaaS Azure virtuele machines (VMs) starten en/of [cloudservices (PaaS rol exemplaren)](../cloud-services/cloud-services-choose-me.md). Bovendien kunt u het virtuele netwerk koppelen aan uw on-premises netwerk met een van de beschikbare in Azure [connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) . U kunt uw netwerk in wezen uitvouwen tot Azure, met volledige controle op IP-adresblokken met het voordeel van enterprise schaal die Azure biedt.

Voor een beter begrip VNets, raadpleegt u de onderstaande afbeelding waarin een eenvoudigere on-premises netwerk.

![On-premises netwerk](./media/virtual-networks-overview/figure01.png)

De bovenstaande afbeelding ziet u een on-premises netwerk dat is verbonden met openbare Internet via een router. U kunt ook een firewall tussen de router en een DMZ hostingprovider een DNS-server en een serverfarm web bekijken. De web-serverfarm is verdeeld met een taakverdeling hardware die wordt blootgesteld aan Internet, en gebruikt resources uit het interne subnet. Het interne subnet is van de DMZ gescheiden door een andere firewall, en hosts Active Directory-domeincontroller, databaseservers en toepassingsservers.

Hetzelfde netwerk kan worden gehost in Azure wordt aangegeven, zoals wordt weergegeven in de onderstaande afbeelding.

![Azure virtuele netwerk](./media/virtual-networks-overview/figure02.png)

Zoals u ziet hoe de Azure infrastructuur krijgt de rol van de router, zodat u toegang van uw VNet met openbare Internet zonder dat u een van de configuratie. Firewalls kunnen worden vervangen door netwerk-beveiligingsgroepen (NSGs) toegepast op elke afzonderlijke subnet. En fysieke netwerktaakverdelers worden vervangen door internet interne en externe omlaag netwerktaakverdelers in Azure wordt aangegeven.

>[AZURE.NOTE] Er zijn twee implementatiemodi in Azure wordt aangegeven: klassieke (ook wel bekend als servicebeheer) en Azure Resource Manager (ARM). Klassieke VNets kan worden toegevoegd aan een groep affiniteit of als een regionale VNet gemaakt. Als u een VNet in een groep affiniteit hebt, wordt het wordt aanbevolen om te [migreren deze naar een regionale VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Voordelen van Virtual Network

- **Moeten worden geïsoleerd**. VNets zijn volledig van elkaar zijn gescheiden. Waarmee u niet-aaneengesloten netwerken voor de ontwikkeling, testen en productie die met de dezelfde CIDR adres bouwstenen maken.

- **Toegang tot openbare Internet**. Alle IaaS VMs en PaaS rol exemplaren in een VNet hebben toegang tot openbare Internet al dan niet standaard. U kunt toegang beheren met behulp van netwerk beveiligingsgroepen (NSGs).

- **Toegang tot VMs binnen de VNet**. PaaS rol exemplaren en IaaS VMs kan worden gestart in hetzelfde virtuele netwerk en ze kunnen verbinden met elkaar met behulp van privé IP-adressen, zelfs als ze in verschillende subnetten hoeft zijn te configureren van een gateway of openbare IP-adressen gebruiken.

- **Naamresolutie**. Azure bevat de naam van de interne oplossing voor IaaS VMs en PaaS rol exemplaren geïmplementeerd in uw VNet. U kunt ook uw eigen DNS-servers implementeren en configureren van de VNet als u wilt gebruiken.

- **Beveiliging**. Verkeer invoeren en afsluiten het virtuele machines en PaaS rol exemplaren in een VNet kan worden beheerd met netwerk beveiligingsgroepen.

- **Connectivity**. VNets kunnen worden verbonden met elkaar met behulp van netwerkgateways of VNet peering. VNets kan niet worden verbonden met on-premises implementatie-datacenters tot en met site-naar-site VPN netwerken of Azure ExpressRoute. Meer informatie over VPN-verbinding voor site-naar-site, gaat u naar [informatie over VPN gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Meer informatie over ExpressRoute, gaat u naar [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md). Meer informatie over het VNet peering, gaat u naar [VNet peering](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Zorg ervoor dat u een VNet maken voordat u een IaaS VMs of PaaS rol exemplaren implementeert bij uw Azure-omgeving. Op ARM gebaseerde VMs vereisen een VNet en als u een bestaande VNet niet opgeeft, Azure Hiermee maakt u een standaard VNet die mogelijk een conflict CIDR Adresblok met uw on-premises netwerk. Zodat u niet kunt u uw VNet verbinden met uw on-premises netwerk.

## <a name="subnets"></a>Subnetten

Subnet is een bereik van IP-adressen in de VNet, kunt u een VNet verdelen over meerdere subnetten voor organisatie en de beveiliging. VMs en PaaS rol exemplaren geïmplementeerd in subnetten (dezelfde of een andere) binnen een VNet kunnen communiceren met elkaar zonder extra configuratie. U kunt ook routetabellen en NSGs aan een subnet configureren.

## <a name="ip-addresses"></a>IP-adressen


Er zijn twee soorten IP-adressen die zijn toegewezen aan resources in Azure wordt aangegeven: *openbare* en *persoonlijke*. Openbare IP-adressen toestaan Azure bronnen die u wilt communiceren met Internet- en andere Azure openbare services zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Privé IP-adressen kunt communicatie tussen resources in een virtueel netwerk, samen met de verbonden via een VPN-verbinding, zonder een Internet-geschikt IP-adressen.

Meer informatie over IP-adressen in Azure wordt aangegeven, gaat u naar [IP-adressen in virtuele netwerk](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure netwerktaakverdelers

Virtuele machines en cloudservices in een netwerk met een virtuele kunnen worden blootgesteld aan Internet via Azure netwerktaakverdelers. Bedrijfstoepassingen die interne omlaag zijn kan alleen worden verdeeld met interne taakverdeling.

- **Externe taakverdeling**. U kunt een externe taakverdeling beschikbaarheid bieden voor IaaS VMs en PaaS rol exemplaren geopend via de openbare Internet.

- **Interne belasting voor documentconversies**. U kunt een interne taakverdeling beschikbaarheid bieden voor IaaS VMs en PaaS rol exemplaren toegankelijk vanaf andere services in uw VNet.

Meer informatie over taakverdeling in Azure wordt aangegeven, gaat u naar [de verdeling van overzicht laden](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Beveiligingsgroep netwerk (NSG)

NSGs om binnenkomende en uitgaande toegang tot netwerkinterfaces (NIC's), kunt u VMs en subnetten. Elke NSG bevat een of meer regels opgeven of verkeer is goedgekeurd of geweigerd op basis van de bron-IP-adres, bronpoort IP-adres en doelpoort. Meer informatie over NSGs, gaat u naar [Wat is een beveiligingsgroep netwerk](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuele toestellen

Een virtuele toestel is alleen een andere VM in uw VNet die wordt uitgevoerd als een toestel-functie van software die zijn gebaseerd, zoals firewall, WAN optimaliseren of detectie van een geopende. U kunt een route in Azure om te leiden van uw VNet-verkeer door een virtueel toestel gebruik van de mogelijkheden maken.

NSGs kan bijvoorbeeld worden gebruikt voor beveiliging op uw VNet. NSGs echter laag 4 Access (Toegangsbeheerlijst) voor binnenkomende en uitgaande pakketten. Als u een laag 7 beveiligingsmodel gebruiken wilt, moet u een toestel firewall gebruiken.

Virtuele toestellen hangt af van de [gebruiker gedefinieerde routes en IP-doorsturen](virtual-networks-udr-overview.md).

## <a name="limits"></a>Limieten
Er zijn beperkingen ten aanzien van het aantal virtuele netwerken die zijn toegestaan in een abonnement, Zie [Azure toegang limieten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
Er is geen extra kosten voor het gebruik van virtuele netwerken in Azure wordt aangegeven. De berekeningscluster exemplaren gestart binnen de Vnet brengt de standaardtarieven zoals is beschreven in [Azure VM prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/). De [VPN Gateways](https://azure.microsoft.com/pricing/details/vpn-gateway/) en [openbare IP-adressen] (https://azure.microsoft.com/pricing/details/ip-addresses/) gebruikt in de VNet worden ook opgeladen standaardtarieven.

## <a name="next-steps"></a>Volgende stappen

- [Een VNet maken](virtual-networks-create-vnet-arm-pportal.md) en subnetten.
- [Een VM in een VNet maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Meer informatie over [NSGs](virtual-networks-nsg.md).
- Meer informatie over het [door de gebruiker gedefinieerde routes en IP-doorsturen](virtual-networks-udr-overview.md).
