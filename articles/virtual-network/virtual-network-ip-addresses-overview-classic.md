<properties
   pageTitle="Openbare en persoonlijke IP-adressen van (klassieke) in Azure | Microsoft Azure"
   description="Meer informatie over openbare en persoonlijke IP-adressen in Azure wordt aangegeven"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-adressen (klassieke) in Azure wordt aangegeven
U kunt IP-adressen toewijzen aan Azure bronnen die u wilt communiceren met andere Azure resources, uw on-premises netwerk en Internet. Er zijn twee soorten IP-adressen die u in Azure wordt aangegeven gebruiken kunt: openbare en persoonlijke.

Openbare IP-adressen worden voor communicatie met Internet, met inbegrip van Azure openbare services gebruikt.

Privé IP-adressen zijn gebruikt voor communicatie binnen een Azure virtuele netwerk (VNet), een cloudservice en uw on-premises netwerk wanneer u een VPN-gateway of ExpressRoute circuitlijnen gebruikt voor het uitbreiden van uw netwerk op Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het implementatiemodel resourcemanager-](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Openbare IP-adressen
Openbare IP-adressen toestaan Azure bronnen die u wilt communiceren met Internet en Azure openbare services zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) [SQL-databases](../sql-database/sql-database-technical-overview.md)en [Azure opslag](../storage/storage-introduction.md).

Een openbare IP-adres is gekoppeld aan de volgende resourcetypen:

- Cloudservices
- IaaS virtuele Machines (VMs)
- PaaS rol exemplaren
- VPN gateways
- Toepassingsgateways

### <a name="allocation-method"></a>Toewijzingsmethode
Wanneer een openbare IP-adres worden toegewezen aan een Azure resource moet, is deze *dynamisch* toegewezen uit een groep beschikbaar openbare IP-adres binnen de locatie die de resource wordt gemaakt. Dit IP-adres wordt uitgebracht wanneer de resource is gestopt. In het geval van een cloudservice, is dit gebeurt wanneer alle exemplaren van de rol zijn gestopt, die kunnen worden voorkomen met behulp van een *statische* (gereserveerde) IP-adres (Zie [Cloud Services](#Cloud-services)).

>[AZURE.NOTE] De lijst met IP-bereiken waaruit openbare IP-adressen zijn toegewezen aan Azure resources wordt gepubliceerd op [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-hostname resolutie
Als u een cloudservice of een VM IaaS maakt, moet u de DNS-naam van een cloud-service die uniek voor alle resources in Azure is bieden. Hiermee maakt u een toewijzing in de Azure managed DNS-servers voor *DNS-naam*. cloudapp.net het openbare IP-adres van de resource. Wanneer u een cloudservice met een cloud-service DNS-naam van **contoso**maakt, worden de fully qualified domain name (FQDN) **contoso.cloudapp.net** omgezet in een openbare IP-adres (VIP) van de cloudservice opgelost. U kunt deze FQDN gebruiken om te maken van een aangepast domein CNAME-record die wijst naar het openbare IP-adres in Azure wordt aangegeven.

### <a name="cloud-services"></a>Cloudservices
Een cloudservice heeft altijd een openbare IP-adres waarnaar wordt verwezen als een virtueel IP-adres (VIP). U kunt de eindpunten maken in een cloudservice om te koppelen van verschillende poorten in de VIP aan interne poorten op VMs en exemplaren van de rol in de cloudservice. 

Een cloudservice kan bevatten meerdere IaaS VMs of PaaS rol exemplaren, alle weergegeven via de dezelfde cloud service VIP. U kunt ook [meerdere VIP's naar een cloudservice](../load-balancer/load-balancer-multivip.md), waarmee meerdere-VIP-scenario's zoals meerdere tenant-omgeving met SSL gebaseerde websites toewijzen.

U kunt ervoor zorgen dat het openbare IP-adres van een cloudservice blijft hetzelfde, zelfs als u alle exemplaren van de rol zijn gestopt, met behulp van een *statische* openbare IP-adres genoemd [Gereserveerde IP](virtual-networks-reserved-public-ip.md). U kunt een statische (gereserveerde) IP-bron maken op een specifieke locatie en toewijzen aan elke cloudservice op die locatie. U kunt het werkelijke IP-adres opgeven voor de gereserveerde IP, deze is toegewezen aan in de groep beschikbaar IP-adressen op de locatie die deze is gemaakt. Dit IP-adres wordt niet vrijgegeven totdat u deze expliciet verwijderen.

Statische (gereserveerde) openbare IP-adressen zijn vaak gebruikt in de scenario's waarin een cloudservice:

- firewallregels zijn ingesteld voor eindgebruikers vereist.
- afhankelijk van de externe DNS-naamresolutie, en een dynamische IP waarvoor een records bijwerken.
- externe webservices waarmee IP-gebaseerde beveiligingsmodel gebruikt.
- gebruikt SSL-certificaten die zijn gekoppeld aan een IP-adres.

>[AZURE.NOTE] Wanneer u een klassieke VM maakt, wordt een container *cloudservice* wordt gemaakt door Azure, met een virtueel IP-adres (VIP). Wanneer de aanmaakdatum via portal klaar is, is een standaard RDP- of SSH *eindpunt* geconfigureerd met de portal zodat u verbinding met de VM tot en met de cloud service VIP maken kunt. Deze VIP cloud-service kan worden gereserveerd, die een gereserveerd IP-adres verbinding maken met de VM effectief biedt. U kunt extra poorten openen door het configureren van meer eindpunten.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs en PaaS rol exemplaren
U kunt een openbare IP-adres rechtstreeks naar een IaaS [VM](../virtual-machines/virtual-machines-linux-about.md) of PaaS rol exemplaar in een cloudservice toewijzen. Dit is een exemplaar niveau openbare IP-adres ([ILPIP](virtual-networks-instance-level-public-ip.md)) genoemd. Dit openbare IP-adres kan alleen worden dynamische.

>[AZURE.NOTE] Dit is het verschil tussen de VIP van de cloudservice, dat wil een container voor IaaS VMs of PaaS rol exemplaren zeggen, omdat een cloudservice kan bevatten meerdere IaaS VMs of PaaS rol exemplaren, alle weergegeven via de dezelfde cloud service VIP.

### <a name="vpn-gateways"></a>VPN gateways
Een [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kan worden gebruikt om verbinding maken met een VNet Azure naar andere Azure VNets of on-premises implementatie-netwerken te gebruiken. Een VPN-gateway is een openbare IP-adres *dynamisch*, waarmee communicatie met het externe netwerk toegewezen.

### <a name="application-gateways"></a>Toepassingsgateways
Een Azure [gateway toepassing](../application-gateway/application-gateway-introduction.md) kan worden gebruikt voor Layer7 taakverdeling op route-netwerkverkeer op basis van HTTP. Toepassingsgateway is een openbare IP-adres *dynamisch*, die als de VIP taakverdeling fungeert toegewezen.

### <a name="at-a-glance"></a>In een oogopslag
De onderstaande tabel ziet u welke typen resources met de mogelijke toewijzingsmethoden (dynamic/statisch), en de mogelijkheid om toe te wijzen meerdere openbare IP-adressen.

|Resource|Dynamische|Statische|Meerdere IP-adressen|
|---|---|---|---|
|Cloudservice|Ja|Ja|Ja|
|IaaS VM of PaaS rol exemplaar|Ja|Nee|Nee|
|VPN gateway|Ja|Nee|Nee|
|Toepassingsgateway|Ja|Nee|Nee|

## <a name="private-ip-addresses"></a>Privé IP-adressen
Privé IP-adressen toestaan Azure bronnen die u wilt communiceren met andere resources in een cloudservice of een [virtueel netwerk](virtual-networks-overview.md)(VNet), of on-premises netwerk (via een VPN-gateway of ExpressRoute circuitlijnen), zonder een Internet-bereikbaar IP-adres.

In de van Azure klassieke implementatiemodel, kan een persoonlijk IP-adres naar de volgende Azure bronnen worden toegewezen:

- IaaS VMs en PaaS rol exemplaren
- Interne taakverdeling
- Toepassingsgateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs en PaaS rol exemplaren
Virtuele machines (VMs) die zijn gemaakt met het implementatiemodel klassieke worden altijd in een cloudservice, vergelijkbaar met PaaS rol exemplaren geplaatst. Het gedrag van privé IP-adressen lijken dus voor deze resources.

Het is belangrijk te weten dat een cloudservice kan worden geïmplementeerd op twee manieren:

- Als een *zelfstandig product* cloudservice, waarop de invoegtoepassing niet binnen een virtueel netwerk is.
- Als onderdeel van een virtueel netwerk.

#### <a name="allocation-method"></a>Toewijzingsmethode
Voor het geval een *zelfstandig product* -cloudservice krijgen de resources een privé IP-adres dat wordt toegewezen *dynamisch* van het datacenter van de Azure privé IP-adresbereik. Dit kan worden gebruikt alleen voor communicatie met andere VMs binnen de dezelfde cloudservice. Dit IP-adres kan wijzigen wanneer de resource wordt gestopt en gestart.

Voor het geval een cloudservice geïmplementeerd binnen een virtueel netwerk, krijgen resources privé IP-adressen uit het adresbereik van de bijbehorende subnetten (zoals opgegeven in de netwerkconfiguratie) toegewezen. Deze privé IP-adres kan worden gebruikt voor communicatie tussen alle VMs binnen de VNet.

Daarnaast kunnen voor het geval de cloudservices binnen een VNet, is een persoonlijk IP-adres toegewezen aan *dynamisch* (met DHCP) al dan niet standaard. Deze kan wijzigen wanneer de resource wordt gestopt en gestart. Om ervoor te zorgen dat het IP-adres hetzelfde blijft, moet u de toewijzingsmethode ingesteld op *statische*en geef een geldig IP-adres in het bijbehorende adresbereik.

Statische privé IP-adressen vaak worden gebruikt voor:

 - VMs die als domeincontrollers of DNS-servers fungeren.
 - VMs waarvoor firewallregels IP-adressen gebruikt.
 - VMs met services toegankelijk voor andere apps tot en met een IP-adres.

#### <a name="internal-dns-hostname-resolution"></a>Interne DNS-hostname resolutie
Alle Azure VMs en PaaS rol exemplaren zijn geconfigureerd met [Azure managed DNS-servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) al dan niet standaard, tenzij u expliciet aangepaste DNS-servers configureert. Deze DNS-servers bieden interne naamresolutie voor VMs en rol-exemplaren die zich binnen dezelfde VNet of cloud-service bevinden.

Wanneer u een VM maakt, wordt een toewijzing voor de hostnaam het privé IP-adres toegevoegd aan de Azure managed DNS-servers. Voor het geval een VM meerdere NIC's, is de hostnaam toegewezen aan het persoonlijke IP-adres van de primaire NIC Deze toewijzingsinformatie is echter beperkt tot bronnen binnen de dezelfde cloudservice of VNet.

Voor het geval een *zelfstandig product* -cloudservice, is mogelijk om op te lossen hostnamen die gebruikmaken van de overal VMs/rol binnen dezelfde cloudservice alleen. Voor het geval een cloudservice binnen een VNet, is mogelijk om op te lossen hostnamen die gebruikmaken van de VMs/rol overal binnen de VNet.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne netwerktaakverdelers (ILB) & Toepassingsgateways
U kunt een persoonlijk IP-adres toewijzen aan de **front-end van** de configuratie van een [Interne taakverdeling Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) of een [Azure toepassingsgateway](../application-gateway/application-gateway-introduction.md). Dit privé IP-adres fungeert als een interne eindpunt, toegankelijk zijn alleen voor de resources binnen het virtuele netwerk (VNet) en de externe netwerken die zijn verbonden met de VNet. U kunt u een persoonlijk IP-adres met dynamische of statische toewijzen aan de front-end van de configuratie. U kunt ook meerdere privé IP-adressen inschakelen van scenario's voor multi-vip toewijzen.

### <a name="at-a-glance"></a>In een oogopslag
De onderstaande tabel ziet u welke typen resources met de mogelijke toewijzingsmethoden (dynamic/statisch), en de mogelijkheid om toe te wijzen meerdere privé IP-adressen.

|Resource|Dynamische|Statische|Meerdere IP-adressen|
|---|---|---|---|
|VM (in een cloudservice *zelfstandige versie* )|Ja|Ja|Ja|
|PaaS rol exemplaar (in een cloudservice *zelfstandige versie* )|Ja|Nee|Ja|
|VM of PaaS rol-exemplaar (in een VNet)|Ja|Ja|Ja|
|Front-end van de verdeling van interne laden|Ja|Ja|Ja|
|Front-end van de gateway toepassing|Ja|Ja|Ja|

## <a name="limits"></a>Limieten

De volgende tabel ziet de beperkingen op IP-adressen in Azure wordt aangegeven per abonnement. U kunt [contact opnemen met ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het vergroten van de standaardlimieten snel aan de beperkingen voor maximale op basis van uw zakelijke behoeften.

||Standaardlimiet|Maximumlimiet|
|---|---|---|
|Openbare IP-adressen (dynamisch)|5|contact opnemen met ondersteuning|
|Gereserveerde openbare IP-adressen|20|contact opnemen met ondersteuning|
|Openbare VIP per distributie (cloudservice)|5|contact opnemen met ondersteuning|
|Privé VIP (ILB) per distributie (cloudservice)|1|1|

Zorg ervoor dat u de volledige set [bestandsgrootten voor netwerken](azure-subscription-service-limits.md#networking-limits) in Azure lezen.

## <a name="pricing"></a>Prijzen

Openbare IP-adressen zijn in de meeste gevallen gratis. Er zijn een nominale kosten op Extra en/of statische openbare IP-adres. Zorg ervoor dat u meer informatie over de [structuur voor openbare IP-adressen prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Verschillen tussen resourcemanager en klassieke implementaties
Hieronder ziet u een vergelijking van IP-adressering functies in resourcemanager en het implementatiemodel klassieke.

||Resource|Klassieke|Resourcemanager|
|---|---|---|---|
|**Openbare IP-adres**|VM|Waarnaar wordt verwezen als een ILPIP (alleen voor dynamisch)|Waarnaar wordt verwezen als een openbare IP-adres (dynamische of statische)|
|||Toegewezen aan een VM IaaS of een exemplaar van de rol PaaS|Dat is gekoppeld aan van de VM NIC|
||Omlaag taakverdeling Internet|Genoemd VIP (dynamische) of gereserveerde IP-(statisch)|Waarnaar wordt verwezen als een openbare IP-adres (dynamische of statische)|
|||Toegewezen aan een cloudservice|Dat is gekoppeld aan de taakverdeling front-end zoekconfiguratie|
||||
|**Privé IP-adres**|VM|Een DIP genoemd|Een persoonlijke IP-adres genoemd|
|||Toegewezen aan een VM IaaS of een exemplaar van de rol PaaS|Toegewezen aan van de VM NIC|
||Interne taakverdeling (ILB)|Toegewezen aan de ILB (dynamische of statische)|Toegewezen aan van de ILB front-end configuratie (dynamische of statische)|

## <a name="next-steps"></a>Volgende stappen
- [Deploy een VM met een statische privé IP-adres](virtual-networks-static-private-ip-classic-pportal.md) met behulp van de klassieke portal.
