<properties
   pageTitle="Openbare en persoonlijke IP-adressen in Azure resourcemanager | Microsoft Azure"
   description="Meer informatie over openbare en persoonlijke IP-adressen in Azure resourcemanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP-adressen in Azure wordt aangegeven
U kunt IP-adressen toewijzen aan Azure bronnen die u wilt communiceren met andere Azure resources, uw on-premises netwerk en Internet. Er zijn twee soorten IP-adressen die u in Azure wordt aangegeven gebruiken kunt:

- **Openbare IP-adressen**: gebruikt voor communicatie met Internet, met inbegrip van Azure openbare diensten
- **Privé IP-adressen**: gebruikt voor communicatie binnen een Azure virtuele netwerk (VNet) en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute circuitlijnen gebruikt voor het uitbreiden van uw netwerk op Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](virtual-network-ip-addresses-overview-classic.md).

Als u bekend met het implementatiemodel klassieke bent, controleert u de [verschillen tussen de IP-adressen tussen klassieke en resourcemanager](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen toestaan Azure bronnen die u wilt communiceren met Internet en Azure openbare services zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) [SQL-databases](../sql-database/sql-database-technical-overview.md)en [Azure opslag](../storage/storage-introduction.md).

In Azure resourcemanager, een [openbare IP-](resource-groups-networking.md#public-ip-address) adres is een resource die de eigen eigenschappen. U kunt een openbare IP-adres resource koppelen aan een van de volgende bronnen:

- Virtuele machines (VM)
- Internetgerichte netwerktaakverdelers
- VPN gateways
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Er zijn twee manieren waarin een IP-adres is toegewezen aan een *openbare* IP-resource - *dynamische* of *statische*. De toegewezen standaardmethode wordt *dynamische*, waarbij een IP-adres **niet** op het moment van de creatie is toegewezen. In plaats daarvan is het openbare IP-adres toegewezen wanneer u start (of maken) de bijbehorende resource (zoals de verdeling van een VM of laden). Het IP-adres is uitgebracht wanneer u stoppen (of verwijdert) de resource. Hierdoor wordt het IP-adres wijzigen wanneer u stopt en start een resource.

Om ervoor te zorgen dat het IP-adres voor de bijbehorende resource hetzelfde blijft, kunt u de toewijzingsmethode expliciet ingesteld op *statische*. In dit geval wordt een IP-adres onmiddellijk toegewezen. Deze is alleen wanneer u de resource verwijderen of wijzigen van de methode voor de toewijzing naar een *dynamische*uitgebracht.

>[AZURE.NOTE] Zelfs wanneer u de toewijzingsmethode op *statische instelt*, niet kunt u het werkelijke IP-adres dat is toegewezen aan de *openbare IP-resource*opgeven. In plaats daarvan wordt deze uit een groep van beschikbare IP-adressen in de Azure locatie die de resource wordt gemaakt in toegewezen.

Statische openbare IP-adressen zijn vaak gebruikt in de volgende scenario's:

- Eindgebruikers moeten firewallregels om te communiceren met uw Azure resources bijwerken.
- DNS-naamresolutie, waar een wijziging in het IP-adres moet bijwerken van een records.
- Uw Azure resources communiceren met andere apps of de services met een IP-adres gebaseerde beveiliging-model.
- U SSL-certificaten die zijn gekoppeld aan een IP-adres gebruiken.

>[AZURE.NOTE] De lijst met IP-bereiken waaruit openbare IP-adressen (dynamic/statisch) worden toegewezen aan Azure resources wordt gepubliceerd op [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-hostname resolutie
Een DNS-domein naamlabel voor een openbare IP-resource wordt gemaakt van een toewijzing voor *domainnamelabel*, kunt u opgeven. *locatie*. cloudapp.azure.com het openbare IP-adres in de Azure managed DNS-servers. Bijvoorbeeld als u een openbare IP-resource met **contoso** als een *domainnamelabel* in de **West Amerikaans maakt** Azure *locatie*, de fully qualified domain name (FQDN) **contoso.westus.cloudapp.azure.com** wordt omgezet in het openbare IP-adres van de resource. U kunt deze FQDN gebruiken om te maken van een aangepast domein CNAME-record die wijst naar het openbare IP-adres in Azure wordt aangegeven.

>[AZURE.IMPORTANT] Elk domein naam etiket gemaakt moet uniek zijn binnen de Azure locatie.  

### <a name="virtual-machines"></a>Virtuele machines
U kunt een openbare IP-adres koppelen aan een [Windows](../virtual-machines/virtual-machines-windows-about.md) - of [Linux](../virtual-machines/virtual-machines-linux-about.md) VM toewijzen aan de **netwerkinterface**. In het geval van een interface netwerkcapaciteit VM, kunt u dit toewijzen aan de *primaire* netwerk-interface alleen. U kunt een dynamische of een statische openbare IP-adres toewijzen aan een VM.

### <a name="internet-facing-load-balancers"></a>Internetgerichte netwerktaakverdelers
U kunt een openbare IP-adres koppelen aan een [Azure taakverdeling](../load-balancer/load-balancer-overview.md), toewijzen aan de belasting voor gelijkmatige verdeling **frontend** configuratie. Dit openbare IP-adres dient als een taakverdeling virtuele IP-adres (VIP). U kunt een dynamische of een statische openbare IP-adres toewijzen aan een front taakverdeling. U kunt ook meerdere openbare IP-adressen toewijzen aan een taakverdeling front-end, waarmee [multi-VIP](../load-balancer/load-balancer-multivip.md) -scenario's, zoals een omgeving met meerdere tenant met op basis van een SSL-websites.

### <a name="vpn-gateways"></a>VPN gateways
[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) wordt gebruikt om een Azure virtuele netwerk (VNet) verbinding te maken met andere VNets Azure of een on-premises netwerk. Moet u een openbare IP-adres toewijzen aan de **IP-configuratie** om te communiceren met het externe netwerk. U kunt een *dynamische* openbare IP-adres op dit moment alleen toewijzen aan een VPN-gateway.

### <a name="application-gateways"></a>Toepassingsgateways
U kunt een openbare IP-adres koppelen aan een Azure- [Toepassingsgateway](../application-gateway/application-gateway-introduction.md)toewijzen aan van de gateway **frontend** configuratie. Dit openbare IP-adres dient als een VIP verdeeld. U kunt een *dynamische* openbare IP-adres op dit moment alleen toewijzen aan een toepassing gateway frontend configuratie.

### <a name="at-a-glance"></a>Aan-overzicht
De volgende tabel ziet de specifieke eigenschap waarmee een openbare IP-adres kan worden gekoppeld aan een resource op het hoogste niveau en de methoden voor eventuele toewijzing (dynamische of statische) die kunnen worden gebruikt.

|Op het hoogste niveau resource|IP-adres koppeling|Dynamische|Statische|
|---|---|---|---|
|VM|Netwerkinterface|Ja|Ja|
|De belasting voor documentconversies|Configuratie van de front-end van|Ja|Ja|
|VPN gateway|Gateway IP-configuratie|Ja|Nee|
|Toepassingsgateway|Configuratie van de front-end van|Ja|Nee|

## <a name="private-ip-addresses"></a>Privé IP-adressen
Privé IP-adressen toestaan Azure bronnen die u wilt communiceren met andere resources in een [virtueel netwerk](virtual-networks-overview.md) of een on-premises netwerk via een VPN-gateway of ExpressRoute circuitlijnen, zonder een Internet-bereikbaar IP-adres.

In het implementatiemodel resourcemanager Azure-zijn een persoonlijk IP-adres is gekoppeld aan de volgende soorten Azure informatiebronnen:

- VMs
- Interne netwerktaakverdelers (ILBs)
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Een persoonlijk IP-adres is toegewezen uit het adresbereik van het subnet waaraan de resource is gekoppeld. Het adresbereik van het subnet zelf is een onderdeel van de adresbereiken van de VNet.

Er zijn twee manieren waarin een persoonlijk IP-adres is toegewezen aan: *dynamische* of *statische*. De toegewezen standaardmethode is *dynamische*, waar het IP-adres van de resource subnet (met DHCP) automatisch wordt toegewezen. Dit IP-adres kan wijzigen wanneer u stopt en start de resource.

U kunt de toewijzingsmethode instellen op *statische* om ervoor te zorgen dat het IP-adres hetzelfde blijft. In dit geval moet u ook een geldige IP-adres die deel uitmaakt van de resource subnet opgeven.

Statische privé IP-adressen vaak worden gebruikt voor:

- VMs die als domeincontrollers of DNS-servers fungeren.
- Resources waarvoor firewallregels IP-adressen gebruikt.
- Resources toegankelijk voor andere apps/bronnen via een IP-adres.

### <a name="virtual-machines"></a>Virtuele machines
Een persoonlijk IP-adres is toegewezen aan het **netwerkinterface** van een [Windows](../virtual-machines/virtual-machines-windows-about.md) - of [Linux](../virtual-machines/virtual-machines-linux-about.md) VM. Voor het geval een interface netwerkcapaciteit VM krijgt elke interface een persoonlijk IP-adres dat is toegewezen. Als u dynamische of statische voor de netwerkinterface van een kunt u de toewijzingsmethode.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interne DNS-hostname resolutie (voor VMs)
Alle Azure VMs zijn geconfigureerd met [Azure managed DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) al dan niet standaard, tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden interne naamresolutie voor VMs die zich binnen de dezelfde VNet bevinden.

Wanneer u een VM maakt, wordt een toewijzing voor de hostnaam het privé IP-adres toegevoegd aan de Azure managed DNS-servers. De hostnaam is voor het geval een interface netwerkcapaciteit VM toegewezen aan het persoonlijke IP-adres van de primaire netwerk-interface.

VMs geconfigureerd met Azure managed DNS-servers kunnen de hostnamen van alle VMs binnen hun VNet omzetten in hun persoonlijke IP-adressen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne netwerktaakverdelers (ILB) & Toepassingsgateways
U kunt een persoonlijk IP-adres toewijzen aan de **front-end van** de configuratie van een [Interne taakverdeling Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Azure toepassingsgateway](../application-gateway/application-gateway-introduction.md). Dit privé IP-adres fungeert als een interne eindpunt, toegankelijk zijn alleen voor de resources binnen het virtuele netwerk (VNet) en de externe netwerken die zijn verbonden met de VNet. U kunt u een persoonlijk IP-adres met dynamische of statische toewijzen aan de front-end van de configuratie.

### <a name="at-a-glance"></a>Aan-overzicht
De volgende tabel ziet de specifieke eigenschap waarmee een persoonlijk IP-adres kan worden gekoppeld aan een resource op het hoogste niveau en de methoden voor eventuele toewijzing (dynamische of statische) die kunnen worden gebruikt.

|Op het hoogste niveau resource|IP-adres koppeling|Dynamische|Statische|
|---|---|---|---|
|VM|Netwerkinterface|Ja|Ja|
|De belasting voor documentconversies|Configuratie van de front-end van|Ja|Ja|
|Toepassingsgateway|Configuratie van de front-end van|Ja|Ja|

## <a name="limits"></a>Limieten

De beperkingen van het IP-adressen worden aangegeven in de volledige set [bestandsgrootten voor netwerken](azure-subscription-service-limits.md#networking-limits) in Azure wordt aangegeven. Deze limieten zijn per regio en per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het vergroten van de standaardlimieten snel aan de beperkingen voor maximale op basis van uw zakelijke behoeften.

## <a name="pricing"></a>Prijzen

Openbare IP-adressen wellicht een nominale boete. Meer informatie over het IP-adres prijzen in Azure wordt aangegeven, bekijk de pagina met het [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) .

## <a name="next-steps"></a>Volgende stappen
- [Een VM met een statische openbare IP-adres met behulp van de Azure portal implementeren](virtual-network-deploy-static-pip-arm-portal.md)
- [Een VM met een statische openbare IP-adres met een sjabloon implementeren](virtual-network-deploy-static-pip-arm-template.md)
- [Een VM met een statische privé IP-adres met behulp van de Azure portal implementeren](virtual-networks-static-private-ip-arm-pportal.md)
