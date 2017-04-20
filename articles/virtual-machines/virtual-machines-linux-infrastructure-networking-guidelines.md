<properties
    pageTitle="Richtlijnen voor infrastructuur netwerkproblemen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van virtuele netwerken in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Netwerken infrastructuur richtlijnen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk met informatie over de vereiste planning stappen voor het virtuele netwerken binnen Azure en verbindingen tussen bestaande on-premises omgevingen.


## <a name="implementation-guidelines-for-virtual-networks"></a>Implementatie van richtlijnen voor virtuele netwerken

Beslissingen:

- Welk type virtuele netwerk moet u voor het hosten van uw IT-werkbelasting of infrastructuur (alleen de cloud gebruikt of cross-premises)?
- Voor meerdere lokale virtuele netwerken, hoeveel adresruimte moet u voor het hosten van de subnetten en VMs nu en redelijk uitbreiden in de toekomst?
- Gaat u naar gecentraliseerde virtuele netwerken maken of afzonderlijke virtuele netwerken voor elke resourcegroep maken?

Taken:

- Definieer de adresruimte voor de virtuele netwerken moet worden gemaakt.
- De set subnetten en de adresruimte voor elk definiëren.
- Cross-premises definiëren virtuele netwerken, het instellen van lokale netwerk adres spaties voor de on-premises implementatie-locaties die de VMs in het virtuele netwerk hoeft te bereiken.
- Werken met zowel on-premises netwerken team om ervoor te zorgen de juiste routering is geconfigureerd bij het maken van meerdere lokale virtuele netwerken.
- Het virtuele netwerk met uw naamgevingsconventie maken.


## <a name="virtual-networks"></a>Virtuele netwerken

Virtuele netwerken zijn nodig voor communicatie tussen virtuele machines (VMs). U kunt definiëren subnetten, aangepaste IP-adres, DNS-instellingen, beveiliging filteren en taakverdeling, net als met fysieke netwerken. U kunt via een [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of [Express Route circuitlijnen](../expressroute/expressroute-introduction.md), Azure virtuele netwerken verbinden met uw netwerken on-premises implementatie. U vindt meer informatie over [virtuele netwerken en de bijbehorende onderdelen](../virtual-network/virtual-networks-overview.md).

Met behulp van resourcegroepen, hebt u flexibiliteit bij het ontwerp van de onderdelen van uw virtual netwerken. VMs kunnen verbinding maken met virtuele netwerken buiten hun eigen resourcegroep. Een aanpak ontwerp zou gecentraliseerde resourcegroepen met uw netwerken core-infrastructuur die kan worden beheerd door een algemene team maken. VMs en hun toepassingen die zijn geïmplementeerd in afzonderlijke resourcegroepen. Deze methode kunt toepassing eigenaren toegang tot de resourcegroep die hun VMs zonder toegang tot de configuratie van de breder virtuele netwerken resources bevat.

## <a name="site-connectivity"></a>Site-connectiviteit

### <a name="cloud-only-virtual-networks"></a>Alleen de cloud virtuele netwerken
Als de on-premises gebruikers en computers niet lopende connectiviteit met VMs in een Azure virtuele netwerk hoeven, is het netwerkontwerp van uw virtuele rechtstreeks doorsturen:

![Alleen de cloud virtuele Basisnetwerkdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Deze methode werkt meestal voor internetgerichte-werkbelastingen, zoals een op Internet gebaseerde webserver. U kunt deze VMs SSH of punt-naar-site VPN-verbindingen met beheren.

Omdat ze geen verbinding met uw on-premises netwerk, kan alleen Azure virtuele netwerken een gedeelte van de privé IP-adresruimte kunnen gebruiken. De adresruimte kan de dezelfde privé ruimte in gebruik on-premises implementatie zijn.


### <a name="cross-premises-virtual-networks"></a>Cross-premises virtuele netwerken
Als on-premises gebruikers en computers lopende connectiviteit met VMs in een Azure virtuele netwerk vereisen, maakt u een virtueel cross-premises-netwerk. Het virtuele netwerk verbinden met uw on-premises netwerk met een ExpressRoute of een VPN-verbinding voor site-naar-site.

![Cross-premises virtuele netwerkdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

In deze configuratie wordt is het Azure virtuele netwerk in principe een cloudgebaseerde uitbreiding van uw on-premises netwerk.

Omdat ze verbinding met uw on-premises netwerk maken, cross-premises virtuele netwerken een gedeelte van de adresruimte die worden gebruikt door uw organisatie die uniek is moeten gebruiken. Azure wordt een andere locatie op dezelfde manier die verschillende bedrijfs locaties een specifiek IP-subnet zijn toegewezen, zoals u van uw netwerk uitbreiden.

Als u wilt toestaan pakketten verkeer tussen uw cross-premises virtuele netwerk naar uw on-premises netwerk, moet u de reeks relevante on-premises implementatie adresvoorvoegsels configureren als onderdeel van de lokale netwerk definitie voor het virtuele netwerk. Afhankelijk van de adresruimte van het virtuele netwerk en de reeks relevante on-premises implementatie locaties, kunnen er veel adres voorvoegsels voor eenheden in het lokale netwerk.

U kunt een virtueel netwerk alleen de cloud converteren naar een virtueel cross-premises-netwerk, maar waarschijnlijk moeten u naar opnieuw IP-uw virtueel netwerk-adresruimte en Azure resources. Daarom zorgvuldig als een virtueel netwerk worden verbonden met uw on-premises netwerk moet wanneer u een IP-subnetten toewijst.

## <a name="subnets"></a>Subnetten
Subnetten is toegestaan om te organiseren van resources die zijn gerelateerd, hetzij logisch (bijvoorbeeld één subnet voor die is gekoppeld aan dezelfde toepassing VMs), of fysiek (bijvoorbeeld één subnet per resourcegroep). U kunt ook gebruikmaken van subnet moeten worden geïsoleerd technieken voor extra beveiliging.

Voor meerdere lokale virtuele netwerken, moet u subnetten met dezelfde conventies die u voor lokale resources gebruikt ontwerpen. **Azure altijd de eerste drie IP-adressen van de adresruimte voor elk subnet gebruikt**. Om te bepalen het aantal adressen die u nodig hebt voor het subnet, start u door het aantal VMs die u nodig hebt, wordt nu te tellen. Schatting voor toekomstige groei en gebruik vervolgens de volgende tabel om te bepalen de grootte van het subnet.

Aantal VMs nodig | Aantal hostbits nodig | Grootte van het subnet
--- | --- | ---
1-3 | 3 | / 29
4 – 11     | 4 | / 28
12-27 | 5 | / 27
28-59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Voor lokale normale subnetten is het maximum aantal hostadressen voor een subnet met n hostbits 2<sup>n</sup> – 2. Voor een Azure subnet is het maximum aantal hostadressen voor een subnet met n hostbits 2<sup>n</sup> -5 (2 plus 3 voor de adressen die gebruikmaakt van Azure op elk subnet).

Als u de grootte van een subnet die te klein kiest, kunt u moet opnieuw IP- en implementeer deze opnieuw de VMs in het subnet.


## <a name="network-security-groups"></a>Beveiligingsgroepen netwerk
U kunt filteren regels toepassen op het verkeer die doorloopt tot en met uw netwerken virtuele met behulp van beveiligingsgroepen netwerk. U kunt gedetailleerde filteren regels voor het beveiligen van uw omgeving met virtuele netwerken maken voor het beheren van binnenkomende en uitgaande verkeer bron- en IP-bereiken, toegestane poorten, enzovoort. Netwerk beveiligingsgroepen kunnen worden toegepast op subnetten binnen een virtueel netwerk of rechtstreeks aan een bepaald virtueel netwerk-interface. Het wordt aanbevolen dat sommige niveau van de beveiligingsgroep netwerk verkeer in uw virtuele netwerken filteren. U vindt meer informatie over [Beveiligingsgroepen netwerk](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Extra netwerkonderdelen
Net als met een on-premises implementatie fysieke netwerkinfrastructuur, kan Azure virtuele netwerken meer dan alleen subnetten en IP-adressen bevatten. Bij het ontwerpen van de infrastructuur van uw toepassing, wilt u mogelijk enkele van deze extra onderdelen opnemen:

- [VPN gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) - Azure virtuele netwerken verbinden met andere Azure virtuele netwerken of verbinding maken met on-premises implementatie netwerken via een Site-naar-Site VPN-verbinding. Implementeer Express Route verbindingen voor speciale, beveiligde verbindingen. U kunt ook rechtstreeks toegang voor gebruikers met punt-naar-Site VPN-verbindingen geven.
- [De belasting voor documentconversies](../load-balancer/load-balancer-overview.md) - biedt taakverdeling verkeer voor interne en externe verkeer naar wens.
- [Toepassingsgateway](../application-gateway/application-gateway-introduction.md) - HTTP taakverdeling bij de toepassingslaag, waarin u enkele bijkomende voordelen voor webtoepassingen in plaats van de Azure implementeert de belasting voor documentconversies.
- [Verkeer Manager](../traffic-manager/traffic-manager-overview.md) - verdeling op basis van DNS-verkeer is toegestaan om te leiden van eindgebruikers tot het dichtstbijzijnde toepassingseindpunt beschikbaar, zodat u voor het hosten van uw toepassing afmelden bij Azure datacenters in verschillende regio's.


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 