<properties
   pageTitle="Azure Virtual Network (VNet) plannen en ontwerphandleiding | Microsoft Azure"
   description="Informatie over het plannen en ontwerpen van virtuele netwerken in Azure op basis van uw vereisten voor moeten worden geïsoleerd, connectiviteit en locatie."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Plannen en ontwerpen van Azure virtuele netwerken

Maken van een VNet om te experimenteren met is gemakkelijk genoeg is, maar waarschijnlijk wilt, implementeert u meerdere VNets na verloop van tijd voor de ondersteuning van uw organisatie productie. Met enkele plannen en ontwerpen, is mogelijk om te implementeren VNets en verbindt u de gewenste efficiënter resources. Als u niet bekend met VNets bent, het is raadzaam in die u [meer informatie over VNets](virtual-networks-overview.md) en [het implementeren van](virtual-networks-create-vnet-arm-pportal.md) een voordat u verdergaat. 

## <a name="plan"></a>Plannen

Er is een goed begrip van Azure abonnementen, regio's en netwerk resources kritieke voor succes. U kunt de lijst met overwegingen onder als uitgangspunt gebruiken. Nadat u deze overwegingen kennen, kunt u de vereisten voor het netwerkontwerp van uw definiëren.

### <a name="considerations"></a>Overwegingen

Houd bij het beantwoorden van de planning vragen onder, rekening met het volgende:

- Alles wat die u in Azure maakt bestaat uit een of meer resources. Een virtuele machine (VM) is een resource, de netwerkadapterinterface (NIC) die wordt gebruikt door een VM is een resource, het openbare IP-adres dat is gebruikt door een NIC is een resource, de VNet de NIC is verbonden met een resource is.
- U maken bronnen binnen een [Azure regio](https://azure.microsoft.com/regions/#services) en abonnementen. En resources kunnen alleen worden verbonden met een VNet die zich in dezelfde regio en abonnement in. 
- U kunt VNets met elkaar verbinden met behulp van een Azure [VPN Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). U kunt ook verbinding maakt VNets in regio's van abonnementen op deze manier.
- U kunt VNets verbinding maken met uw on-premises netwerk via een van de [connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) beschikbaar in Azure wordt aangegeven. 
- Verschillende resources kunnen worden gegroepeerd in [resourcegroepen](../azure-resource-manager/resource-group-overview.md#resource-groups), wordt eenvoudiger voor het beheren van de resource als een eenheid. Een resourcegroep kan resources uit meerdere regio's bevatten, zolang de resources deel uitmaakt van hetzelfde abonnement.

### <a name="define-requirements"></a>Vereisten definiëren

Gebruik de vragen onder als uitgangspunt voor het netwerkontwerp van uw Azure.  

1. Welke Azure locaties wordt u gebruiken voor host VNets?
2. Moet u de communicatie tussen deze Azure locaties?
3. Moet u de communicatie tussen uw VNet(s) Azure en uw on-premises implementatie datacenter(s)?
4. Hoeveel infrastructuur als een VMs Service (IaaS) cloud services rollen en web-apps wilt die u nodig hebt voor uw oplossing?
5. Moet u isoleren verkeer op basis van groepen van VMs (dat wil zeggen front-end-endwebservers en back-end databaseservers)?
6. Moet u bepalen netwerkverkeer virtuele toestellen gebruiken?
7. Hebben gebruikers verschillende sets met machtigingen voor verschillende Azure resources nodig?

### <a name="understand-vnet-and-subnet-properties"></a>Meer informatie over de eigenschappen VNet en subnet

VNet en subnetten informatiebronnen definiëren van de rand van een waardepapier voor werkbelasting uitgevoerd in Azure wordt aangegeven. Een VNet wordt gekenmerkt door een verzameling adres spaties, gedefinieerd als CIDR blokken. 

>[AZURE.NOTE] Netwerkbeheerders bent bekend met CIDR-notatie. Als u niet bekend met CIDR, [meer informatie over deze bent](http://whatismyipaddress.com/cidr).

VNets bevatten de volgende eigenschappen.

|Eigenschap|Beschrijving|Beperkingen|
|---|---|---|
|**naam**|De naam van de VNet|Tekenreeks van maximaal 80 tekens. Mogen letters, cijfers, onderstrepingsteken, punten of streepjes. Moet beginnen met een letter of een getal. Moet eindigen met een letter, getal of onderstrepingsteken. Kan bevat hoofdletters of kleine letters.|  
|**locatie**|Azure locatie (ook wel regio genoemd).|Moet een van de geldige Azure locaties.|
|**addressSpace**|Verzameling adresvoorvoegsels waaruit de VNet in CIDR-notatie.|Moet u een matrix van geldige CIDR adresblokken, met inbegrip van openbare IP-adresbereiken.|
|**subnetten**|Verzameling subnetten waaruit de VNet|Zie de onderstaande tabel met subnet eigenschappen.||
|**dhcpOptions**|Object met één vereiste eigenschap met de naam **dnsServers**.||
|**dnsServers**|Matrix van DNS-servers op de VNet. Als er geen server is opgegeven, wordt Azure interne naamresolutie gebruikt.|Moet u een matrix van maximaal 10 DNS-servers op volgorde van IP-adres.| 

Een subnet is een resource onderliggende van een VNet en helpt bij het definiëren segmenten van adres spaties binnen een tekstblok CIDR, met IP-adres voorvoegsels voor eenheden. NIC's kunnen worden toegevoegd aan subnetten en verbonden met VMs, een connectiviteit voor verschillende werkbelasting.

Subnetten bevatten de volgende eigenschappen. 

|Eigenschap|Beschrijving|Beperkingen|
|---|---|---|
|**naam**|De subnetnaam van de|Tekenreeks van maximaal 80 tekens. Mogen letters, cijfers, onderstrepingsteken, punten of streepjes. Moet beginnen met een letter of een getal. Moet eindigen met een letter, getal of onderstrepingsteken. Kan bevat hoofdletters of kleine letters.|
|**locatie**|Azure locatie (ook wel regio genoemd).|Moet een van de geldige Azure locaties.|
|**addressPrefix**|Één adresvoorvoegsel dat het subnet in CIDR notatie|Moet u één CIDR blok die deel uitmaakt van een van de de VNet adres spaties.|
|**networkSecurityGroup**|NSG toegepast op het subnet|Zie [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Routetabel die zijn toegepast op het subnet|Zie [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Verzameling IP-configuratieobjecten die worden gebruikt door NIC's die zijn verbonden met het subnet|Zie [IP-configuratie](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Mailnamen omzetten

Uw VNet standaard [geleverde Azure naamresolutie.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) voor het oplossen van namen binnen de VNet, en klik op de openbare Internet. Als u uw VNets verbinden met uw on-premises implementatie-datacenters, moet u op te geven van [uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) om op te lossen namen tussen uw netwerken.  

### <a name="limits"></a>Limieten

Bekijk de netwerken limieten in het artikel [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) om ervoor te zorgen dat het ontwerp van uw met een van de limieten niet conflicteren. Enkele beperkingen kunnen met het openen van een ondersteuningsticket toenemen.

### <a name="role-based-access-control-rbac"></a>Rolgebaseerd toegangsbeheer RBAC)

U kunt [Azure RBAC](../active-directory/role-based-access-built-in-roles.md) gebruiken om te bepalen het toegangsniveau verschillende gebruikers u verschillende bronnen in Azure wordt aangegeven moet mogelijk. Op deze manier kunt u het werk dat is uitgevoerd door uw team op basis van hun behoeften scheiden. 

Wat betreft virtuele netwerken zijn, hebben gebruikers in de rol **Inzender netwerk** volledige controle over resourcemanager Azure virtuele netwerk resources. Daarnaast hebben gebruikers in de **Klassieke netwerk Inzender** rol volledige controle over klassieke virtueel netwerk resources.

>[AZURE.NOTE] U kunt ook [uw eigen rollen maken](../active-directory/role-based-access-control-configure.md) om te scheiden van uw administratieve behoeften.

## <a name="design"></a>Ontwerp

Zodra u weet dat de antwoorden op de vragen in de sectie [plannen](#Plan) , bekijk het volgende voordat u uw VNets definieert.

### <a name="number-of-subscriptions-and-vnets"></a>Aantal abonnementen en VNets

U kunt meerdere VNets maken in de volgende scenario's:

- **VMs die moeten worden geplaatst in verschillende Azure locaties**. VNets in Azure zijn regionale. Ze kunnen niet locaties beslaan. Daarom moet u ten minste één VNet voor elke Azure locatie die u wilt host VMs in.
- **Werkbelasting die moeten worden volledig van elkaar zijn gescheiden**. U kunt afzonderlijke VNets, waarmee de dezelfde IP-adres spaties, zelfs kunt isoleren verschillende werkbelastingen van elkaar op te maken. 

Houd er rekening mee dat de limieten bovenstaand per regio, per abonnement zijn. Dat betekent dat u kunt meerdere abonnementen gebruiken om uit te breiden de limiet voor bronnen die u in Azure wordt aangegeven onderhouden kunt. U kunt een VPN-verbinding voor de site-naar-site of een circuitlijnen ExpressRoute VNets verbinding in de verschillende abonnementen.

### <a name="subscription-and-vnet-design-patterns"></a>VNet ontwerppatronen en -abonnement

De volgende tabel ziet enkele algemene ontwerppatronen voor het gebruik van abonnementen en VNets.

|Scenario|Diagram|Professionals|Nadelen|
|---|---|---|---|
|Één abonnement, twee VNets per app|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Slechts één abonnement om toegang te beheren.|Maximum aantal VNets per Azure regio. U moet meer abonnementen daarna. Lees het artikel [Azure bestandsgrootten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.|
|Met één abonnement per twee VNets per app-app|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Slechts twee VNets per abonnement gebruikt.|Moeilijker te beheren wanneer er te veel apps.|
|Een abonnement per eenheid voor bedrijven, twee VNets per app.|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Verhouding tussen aantal abonnementen en VNets.|Maximum aantal VNets per eenheid voor bedrijven (abonnement). Lees het artikel [Azure bestandsgrootten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.|
|Een abonnement per eenheid voor bedrijven, twee VNets per groep van apps.|![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Verhouding tussen aantal abonnementen en VNets.|Apps moeten worden geïsoleerd via subnetten en NSGs.|


### <a name="number-of-subnets"></a>Aantal subnetten

Meerdere subnetten in een VNet in de volgende scenario's, moet u overwegen:

- **Onvoldoende privé IP-adressen voor alle NIC's in een subnet**. Als uw subnet-adresruimte bevat geen voldoende IP-adressen voor het aantal NIC's in het subnet, moet u meerdere subnetten maken. Houd er rekening mee dat Azure behoudt 5 privé IP-adressen van elk subnet die niet kunnen worden gebruikt: de eerste en laatste adressen van de adresruimte (voor het subnetadres, en multicast) en 3 adressen naar intern (voor DHCP en DNS doeleinden) worden gebruikt. 
- **Beveiliging**. U kunt subnetten gebruiken om groepen van VMs van elkaar te scheiden voor werkbelasting waarvoor een gelaagde structuur en verschillende [beveiligingsgroepen netwerk (NSGs)](virtual-networks-nsg.md#subnets) voor deze subnetten toepassen.
- **Hybride connectivity**. U kunt VPN gateways en ExpressRoute circuits om [verbinding](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) te gebruiken uw VNets onderling en naar uw lokale gegevens center(s). VPN gateways en ExpressRoute circuits vragen om een subnet van hun eigen moet worden gemaakt.
- **Virtuele toestellen**. U kunt een virtuele toestel, zoals een firewall, WAN accelerator of VPN gateway gebruiken in een VNet Azure. Als u dit doet, kunt u moet [route-verkeer is toegestaan](virtual-networks-udr-overview.md) op deze apparaten en deze isoleren in hun eigen subnet.

### <a name="subnet-and-nsg-design-patterns"></a>Subnet en patronen voor NSG ontwerpen

De volgende tabel ziet enkele algemene ontwerppatronen voor het gebruik van subnetten.

|Scenario|Diagram|Professionals|Nadelen|
|---|---|---|---|
|Één subnet, NSGs per toepassingslaag, per app|![Één subnet](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Slechts één subnet om te beheren.|Meerdere NSGs nodig elke toepassing isoleren.|
|Één subnet per NSGs per toepassingslaag-app|![Subnet per app](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Minder NSGs om te beheren.|Meerdere subnetten om te beheren.|
|Één subnet per toepassingslaag, NSGs per app.|![Subnet per laag](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Verhouding tussen aantal subnetten en NSGs.|Maximum aantal NSGs per abonnement. Lees het artikel [Azure bestandsgrootten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.|
|Één subnet per toepassingslaag, per NSGs per subnet-app|![Subnet per laag per app](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Mogelijk kleinere aantal NSGs.|Meerdere subnetten om te beheren.|

## <a name="sample-design"></a>Ontwerp van de steekproef

Neem de volgende scenario om te illustreren de toepassing van de informatie in dit artikel.

U werkt naar een bedrijf dat 2 datacenters in Noord-Amerika en twee datacenters Europe heeft. U omlaag toepassingen van 6 andere klant onderhouden met 2 verschillende geïdentificeerd bedrijfseenheden die u wilt migreren naar Azure als een pilot. De eenvoudige architectuur voor de toepassingen zijn er als volgt uit:

- 1, 2, App3 en App4 worden gehost op Linux-servers Ubuntu webtoepassingen. Elke toepassing maakt verbinding met een afzonderlijke toepassingsserver die als host fungeert voor RESTful services op Linux-servers. De RESTful services verbinden met een back-end MySQL-database.
- App5 en App6 zijn webtoepassingen die worden gehost op Windows-servers met Windows Server 2012 R2. Elke toepassing maakt verbinding met een back-end SQL Server-database.
- Alle apps zijn momenteel wordt gehost op een van de datacenters van het bedrijf in Noord-Amerika.
- De on-premises implementatie-datacenters gebruiken de 10.0.0.0/8-adresruimte.

U moet een virtueel netwerkoplossing ontwerpen die voldoet aan de volgende vereisten:

- Elke eenheid bedrijven moet niet worden beïnvloed door verbruik van andere bedrijfseenheden bronnen.
- U moet de hoeveelheid VNets en subnetten naar eenvoudiger kunnen beheren minimaliseren.
- Elke business-eenheid moet een enkel test/ontwikkeling VNet gebruikt voor alle toepassingen hebben.
- Elke toepassing wordt gehost in 2 verschillende Azure-datacenters per continent (Noord-Amerika en Europa).
- Elke toepassing wordt volledig geïsoleerd van elkaar verschillen.
- Elke toepassing kan worden gebruikt door klanten via Internet met behulp van HTTP.
- Elke toepassing kan worden gebruikt door gebruikers die zijn verbonden met de on-premises implementatie-datacenters door middel van een versleutelde tunnel.
- Verbinding met datacenters on-premises implementatie moet bestaande VPN apparaten gebruiken.
- Netwerken groep van het bedrijf moet volledige controle over de configuratie VNet hebben.
- Ontwikkelaars in elke business-eenheid moet alleen kunnen VMs implementeren naar bestaande subnetten.
- Alle toepassingen worden gemigreerd als ze te Azure (lift-en-shift zijn).
- De databases op elke locatie moeten worden gerepliceerd naar andere Azure locaties één keer per dag.
- Elke toepassing moet gebruiken 5 front-end-endwebservers, 2 toepassingsservers (indien nodig) en 2 databaseservers.

### <a name="plan"></a>Plannen

U kunt het ontwerp van uw planning door het beantwoorden van de vraag in het gedeelte [vereisten definiëren](#Define-requirements) , zoals hieronder moet beginnen.

1. Welke Azure locaties wordt u gebruiken voor host VNets?

    2 locaties in Noord-Amerika en 2 locaties in Europa. U moet kiezen die zijn gebaseerd op de plaats van uw bestaande on-premises implementatie-datacenters. Op deze manier de verbinding van uw fysieke locaties met Azure heeft een betere latentie.

2. Moet u de communicatie tussen deze Azure locaties?

    Ja. Aangezien de databases moeten worden gerepliceerd naar alle locaties.

3. Moet u de communicatie tussen uw VNet(s) Azure en uw on-premises gegevens center(s) bieden?

    Ja. Aangezien gebruikers die zijn verbonden met moeten de datacenters on-premises implementatie kunnen toegang tot de toepassingen via een versleutelde tunnel.
 
4. Hoeveel IaaS VMs moet u voor uw oplossing?

    200 IaaS VMs. 1, 2 en App3 vereisen 5-endwebservers elke, 2 toepassingsservers elke en 2 databaseservers elke. Dit is een totaal van 9 IaaS VMs per toepassing of 36 IaaS VMs. App5 en App6 vereisen endwebservers 5 en 2 databaseservers. Dit is een totaal van 7 IaaS VMs per toepassing of 14 IaaS VMs. U moet dus 50 IaaS VMs voor alle toepassingen in elke Azure regio. Aangezien we 4 regio's gebruiken moeten, wordt er 200 IaaS VMs.

    U moet ook op te geven van de DNS-servers in elke VNet of in uw on-premises implementatie-datacenters voor het oplossen van naam tussen uw Azure IaaS VMs en uw on-premises netwerk. 

5. Moet u isoleren verkeer op basis van groepen van VMs (dat wil zeggen front-end-endwebservers en back-end databaseservers)?

    Ja. Elke toepassing moet worden volledig geïsoleerd van elkaar en elke toepassingslaag ook moet worden geïsoleerd. 

6. Moet u bepalen netwerkverkeer virtuele toestellen gebruiken?

    Nee. Virtuele toestellen kunnen worden gebruikt om aan te bieden meer controle over netwerkverkeer, waaronder meer gedetailleerde gegevens vlak logboekregistratie. 

7. Hebben gebruikers verschillende sets met machtigingen voor verschillende Azure resources nodig?

    Ja. De netwerken team moet volledig beheer hebben voor de virtuele netwerkinstellingen, terwijl ontwikkelaars alleen moet kunnen hun VMs implementeren naar bestaande subnetten. 

### <a name="design"></a>Ontwerp

U kunt het ontwerp van de precisie van abonnementen, VNets, subnetten en NSGs moet volgen. Bespreken we NSGs hier, maar u meer informatie over [NSGs](virtual-networks-nsg.md) moet voordat u het ontwerp van uw voltooit.

**Aantal abonnementen en VNets**

De volgende vereisten hebben betrekking op abonnementen en VNets:

- Elke eenheid bedrijven moet niet worden beïnvloed door verbruik van andere bedrijfseenheden bronnen.
- U moet de hoeveelheid VNets en subnetten minimaliseren.
- Elke business-eenheid moet een enkel test/ontwikkeling VNet gebruikt voor alle toepassingen hebben.
- Elke toepassing wordt gehost in 2 verschillende Azure-datacenters per continent (Noord-Amerika en Europa).

Op basis van die vereisten, moet u een abonnement voor elke eenheid bedrijven. Op deze manier verbruik van resources uit een eenheid voor bedrijven worden niet meegeteld richting limieten voor de eenheden van andere bedrijven. En aangezien u minimaliseren van het aantal VNets wilt, moet u overwegen het patroon dat in **één abonnement per eenheid voor bedrijven, twee VNets per groep van apps** zoals hieronder wordt weergegeven.

![Één abonnement](./media/virtual-network-vnet-plan-design-arm/figure9.png)

U moet ook de adresruimte opgeven voor elke VNet. Aangezien u nodig hebt connectiviteit tussen de on-premises gegevens gecentreerd en de Azure regio's, de adresruimte die wordt gebruikt voor Azure VNets kan geen conflicteren met de on-premises netwerk en de adresruimte die worden gebruikt door elke VNet moet niet conflicteren met andere bestaande VNets. U kunt de adres-spaties in de onderstaande tabel om te voldoen aan deze vereisten is voldaan.  

|**Abonnement**|**VNet**|**Azure regio**|**-Adresruimte**|
|---|---|---|---|
|BU1|ProdBU1US1|West VS|172.16.0.0/16|
|BU1|ProdBU1US2|Oost-VS|172.17.0.0/16|
|BU1|ProdBU1EU1|Noord-Europa|172.18.0.0/16|
|BU1|ProdBU1EU2|West Europa|172.19.0.0/16|
|BU1|TestDevBU1|West VS|172.20.0.0/16|
|BU2|TestDevBU2|West VS|172.21.0.0/16|
|BU2|ProdBU2US1|West VS|172.22.0.0/16|
|BU2|ProdBU2US2|Oost-VS|172.23.0.0/16|
|BU2|ProdBU2EU1|Noord-Europa|172.24.0.0/16|
|BU2|ProdBU2EU2|West Europa|172.25.0.0/16|

**Aantal subnetten en NSGs**

De volgende vereisten hebben betrekking op subnetten en NSGs:

- U moet de hoeveelheid VNets en subnetten minimaliseren.
- Elke toepassing wordt volledig geïsoleerd van elkaar verschillen.
- Elke toepassing kan worden gebruikt door klanten via Internet met behulp van HTTP.
- Elke toepassing kan worden gebruikt door gebruikers die zijn verbonden met de on-premises implementatie-datacenters door middel van een versleutelde tunnel.
- Verbinding met datacenters on-premises implementatie moet bestaande VPN apparaten gebruiken.
- De databases op elke locatie moeten worden gerepliceerd naar andere Azure locaties één keer per dag.

Op basis van die vereisten, kan u gebruik één subnet per toepassingslaag, en gebruik NSGs om verkeer filteren per toepassing. Op deze manier alleen er 3 subnetten in elke VNet (front-end, toepassingslaag en gegevens layer) en één NSG per toepassing per subnet. In dit geval moet u overwegen het patroon van de ontwerpen **één subnet per toepassingslaag, NSGs per app** . De onderstaande afbeelding ziet u het gebruik van de ontwerppatroon dat staat voor het VNet **ProdBU1US1** .

![Één subnet per laag, één NSG per toepassing per laag](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Echter ook moet u een extra subnet maken voor de VPN-verbinding tussen de VNets en uw on-premises implementatie-datacenters. En moet u de adresruimte voor elk subnet opgeven. De onderstaande afbeelding ziet u een oplossing van een steekproef voor **ProdBU1US1** VNet. U zou dit scenario repliceren voor elke VNet. Elke kleur vertegenwoordigt een andere toepassing.

![Voorbeeld VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Toegangsbeheer**

De volgende vereisten hebben betrekking op toegangsbeheer:

- Netwerken groep van het bedrijf moet volledige controle over de configuratie VNet hebben.
- Ontwikkelaars in elke business-eenheid moet alleen kunnen VMs implementeren naar bestaande subnetten.

Op basis van die vereisten, kunt u gebruikers uit het team netwerken toevoegen aan de rol van de ingebouwde **Netwerk Inzender** in elk abonnement; en maakt u een aangepaste rol voor ontwikkelaars van de toepassing in elk abonnement dat door ze te machtigen voor het toevoegen van VMs aan bestaande subnetten.

## <a name="next-steps"></a>Volgende stappen

- [Deploy een virtueel netwerk](virtual-networks-create-vnet-arm-template-click.md) op basis van een scenario.
- Meer informatie over hoe u IaaS VMs [taakverdeling](../load-balancer/load-balancer-overview.md) en [Routering via meerdere Azure regio's beheren](../traffic-manager/traffic-manager-overview.md).
- Meer informatie over [NSGs en hoe u plannen en ontwerpen van](virtual-networks-nsg.md) een oplossing NSG.
- Meer informatie over uw [cross-premises en VNet connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
