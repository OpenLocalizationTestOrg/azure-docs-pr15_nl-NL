<properties 
   pageTitle="Hybride verbinding met 2 niveaus toepassing | Microsoft Azure"
   description="Informatie over het implementeren van virtuele toestellen en UDR maken van een omgeving met meerdere niveaus in Azure wordt aangegeven"
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
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuele toestel scenario

Een gebruikelijk tussen groter Azure klant is dat u een toepassing twee lagen blootgesteld aan Internet, terwijl toegang tot de vorige laag vanuit een on-premises implementatie-datacenter. In dit document wordt u begeleid bij een scenario waarbij de gebruiker gedefinieerde Routes (UDR), een VPN-Gateway en virtuele netwerkapparaten om te implementeren van een twee niveaus-omgeving die voldoet aan de volgende vereisten:

- Webtoepassing moet toegankelijk zijn vanuit de openbare Internet alleen.
- Webserver hostingprovider van de toepassing moet een back-end-toepassingsserver toegang tot.
- Al het verkeer van Internet naar de webtoepassing moet gaan via een firewall virtuele toestel. Dit virtuele toestel worden gebruikt voor alleen internetverkeer.
- Al het verkeer gaat u naar de toepassingsserver moet gaan via een firewall virtuele toestel. Dit virtuele toestel worden gebruikt voor toegang tot de backend-endserver, en die afkomstig zijn uit de on-premises netwerk via een VPN-Gateway.
- Beheerders moeten kunnen de firewall virtuele toestellen beheren van hun lokale computers, met behulp van een externe firewall virtuele toestel alleen voor management gebruikt.

Dit is een standaard DMZ-scenario met een DMZ en een beveiligde netwerk. Deze scenario kan worden gemaakt in Azure met behulp van NSGs, virtuele toestellen firewall of een combinatie van beide. De onderstaande tabel worden enkele van de voor- en nadelen tussen NSGs en firewall virtuele toestellen getoond.

||Professionals|Nadelen|
|---|---|---|
|NSG|Gratis. <br/>Geïntegreerd in Azure RBAC. <br/>Regels kunnen worden gemaakt in ARM sjablonen.|Complexiteit kan variëren in grotere omgevingen. |
|Firewall|Volledige controle over gegevens vlak. <br/>Centraal beheer via firewall console.|Kosten van de firewall toestel. <br/>Niet geïntegreerd met Azure RBAC.|

De onderstaande oplossing hand firewall virtuele toestellen een netwerk DMZ/beveiligde scenario.

## <a name="considerations"></a>Overwegingen

U kunt de omgeving uitleg hierboven in Azure wordt aangegeven met behulp van verschillende functies vandaag, als volgt implementeren.

- **Virtuele netwerk (VNet)**. Een VNet Azure worden verwerkt in soortgelijke wijze als een on-premises netwerk en kan worden opgesplitst in een of meer subnetten verkeer moeten worden geïsoleerd en scheiding van problemen op te geven.
- **Virtuele toestel**. Verschillende partners bieden virtuele toestellen in de Azure Marketplace die kunnen worden gebruikt voor de drie firewalls hierboven is beschreven. 
- **Door gebruiker gedefinieerde Routes (UDR)**. Routetabellen kunnen UDRs door Azure netwerken gebruikt om te bepalen de stroom van pakketten binnen een VNet bevatten. Deze routetabellen kunnen worden toegepast op subnetten. Een van de nieuwste functies in Azure is de mogelijkheid een routetabel toepassen op de GatewaySubnet, de mogelijkheid om door te sturen van al het verkeer die afkomstig zijn in de VNet Azure uit een hybride verbinding op een virtuele toestel.
- **IP-doorsturen**. Standaard doorsturen van de Azure netwerken engine pakketten naar virtueel netwerk interface kaarten (NIC) alleen als het pakket doeladres overeenkomt met het NIC IP-adres. Als een UDR definieert dat een pakket moet worden verstuurd naar een bepaalde virtuele toestel, zou de Azure netwerken engine daarom pakket neerzetten. Om ervoor te zorgen dat het pakket wordt bezorgd in een VM (in dit geval een virtuele toestel) die niet de werkelijke doel voor het pakket, moet u IP-doorsturen inschakelen voor het virtuele toestel.
- **Beveiligingsgroepen netwerk (NSGs)**. In het onderstaande voorbeeld maakt geen gebruik van NSGs, maar u kan NSGs toegepast op de subnetten en/of NIC's in deze oplossing gebruiken om het verkeer en afmelden bij deze subnetten en NIC verder te filteren.


![IPv6-connectiviteit](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- 2 resourcegroepen, niet wordt weergegeven in het diagram. 
    - **ONPREMRG**. Bevat alle resources die nodig is om te simuleren van een on-premises netwerk.
    - **AZURERG**. Bevat alle resources die nodig zijn voor de Azure virtuele netwerkomgeving. 
- Een VNet naam **onpremvnet** gebruikt nabootsen een on-premises implementatie-datacenter gesegmenteerd zoals hieronder wordt weergegeven.
    - **onpremsn1**. Subnet met een virtuele machine (VM) Ubuntu nabootsen een on-premises server wordt uitgevoerd.
    - **onpremsn2**. Subnet met een VM Ubuntu nabootsen een lokale computer wordt gebruikt door een beheerder uitgevoerd.
- Er is virtual toestel van een firewall met de naam **OPFW** op **onpremvnet** een tunnel naar **azurevnet**beheren.
- Een benoemde **azurevnet** VNet gesegmenteerd zoals hieronder wordt weergegeven.
    - **azsn1**. Externe firewall subnet uitsluitend voor de externe firewall gebruikt. Alle internetverkeer komen via dit subnet. Dit subnet bevat alleen een NIC die is gekoppeld aan de externe firewall.
    - **azsn2**. Front-end subnet hostingprovider een VM wordt uitgevoerd met een webserver die van Internet worden gebruikt.
    - **azsn3**. Back-end subnet hostingprovider een VM een back-end application server die worden gebruikt door de front-end van web-server wordt uitgevoerd.
    - **azsn4**. Management subnet uitsluitend voor management toegang tot alle firewall virtuele toestellen gebruikt. Dit subnet bevat alleen een NIC voor elk firewall virtuele toestel gebruikt in de oplossing.
    - **GatewaySubnet**. Azure hybride verbinding subnet is vereist voor ExpressRoute en VPN Gateway te leveren connectiviteit tussen Azure VNets en andere netwerken. 
- Er zijn 3 firewall virtuele toestellen in het netwerk **azurevnet** . 
    - **AZF1**. Externe firewall blootgesteld aan Internet openbare, met behulp van een openbare IP-adres resource in Azure wordt aangegeven. Moet u zorgen dat u hebt een sjabloon van Marketplace, of rechtstreeks vanuit de leverancier van uw apparaat, dat bepalingen een 3-NIC virtuele toestel.
    - **AZF2**. Interne firewall die worden gebruikt om te bepalen verkeer tussen **azsn2** en **azsn3**. Dit is ook een 3-NIC virtuele toestel.
    - **AZF3**. Management firewall toegankelijk zijn voor beheerders van het datacenter van de on-premises implementatie en onderhouden met een management subnet gebruikt om alle firewall toestellen te beheren. U kunt 2-NIC virtuele toestel sjablonen vinden in de Marketplace, of vraag een rechtstreeks vanuit de leverancier van uw toestel.

## <a name="user-defined-routing-udr"></a>Door gebruiker gedefinieerde routeren (UDR)

Elk subnet in Azure kan worden gekoppeld aan een tabel UDR is gebruikt om te bepalen hoe verkeer gestart in dat subnet is doorgestuurd. Als er geen UDRs zijn gedefinieerd, wordt standaardroutes in Azure gebruikt om te verkeer uit één subnet naar een andere toestaan. Meer informatie over UDRs, gaat u naar [Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen](./virtual-networks-udr-overview.md#ip-forwarding).

Om ervoor te zorgen communicatie vindt plaats via het juiste firewall toestel, op basis van de laatste vereiste bovenstaande, moet u de volgende routetabel met UDRs in **azurevnet**maken.

### <a name="azgwudr"></a>azgwudr

In dit scenario wordt alleen verkeer die doorloopt vanuit on-premises naar Azure gebruikt voor het beheren van de firewalls door te verbinden met **AZF3**en dat verkeer via de interne firewall, **AZF2**moet gaan. Slechts één route dus nodig in de **GatewaySubnet** zoals hieronder wordt weergegeven.

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Staat toe dat on-premises implementatie verkeer management firewall **AZF3** hebt bereikt|

### <a name="azsn2udr"></a>azsn2udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Staat toe dat de backend-subnet waarop de toepassingsserver via **AZF2** verkeer|
|0.0.0.0/0|10.0.2.10|Staat toe dat alle andere verkeer worden gerouteerd via **AZF1**|

### <a name="azsn3udr"></a>azsn3udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Staat toe dat verkeer **azsn2** laten lopen van app-server naar de webserver tot en met **AZF2**|

Moet u ook om routetabellen te maken voor de subnetten **onpremvnet** nabootsen van het datacenter van de on-premises implementatie.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Staat toe dat verkeer **onpremsn2** tot en met **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Bestemming|Volgende hop|Uitleg|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Staat toe dat de back-ups subnet verkeer in Azure wordt aangegeven tot en met **OPFW**|
|192.168.1.0/24|192.168.2.4|Staat toe dat verkeer **onpremsn1** tot en met **OPFW**|

## <a name="ip-forwarding"></a>IP-doorsturen 

UDR en IP-doorsturen zijn functies die u kunt gebruiken in combinatie toe te staan dat virtuele toestellen moet worden gebruikt om te bepalen netwerkverkeer in een VNet Azure.  Een virtuele toestel is niets meer dan een VM die wordt uitgevoerd als een toepassing gebruikt voor het verwerken van netwerkverkeer in sommige manier, zoals een firewall of een NAT-apparaat.

Dit virtuele toestel VM moet mogelijk binnenkomende verkeer ontvangen dat niet is geadresseerd aan zelf. Als u wilt toestaan dat een VM verkeer die zijn gericht aan andere bestemmingen ontvangen, moet u IP-doorsturen inschakelen voor VM. Dit is een Azure instellen, niet een instelling in het gastbesturingssysteem. Uw virtuele toestel nog steeds moet een type van toepassing op de binnenkomende verkeer verwerken en deze correct doorsturen.

Meer informatie over het IP-doorsturen, gaat u naar [Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen](./virtual-networks-udr-overview.md#ip-forwarding).

Stel dat u hebt de volgende instellingen in een Azure vnet bijvoorbeeld:

- Subnet **onpremsn1** bevat een VM **onpremvm1**met de naam.
- Subnet **onpremsn2** bevat een VM **onpremvm2**met de naam.
- Een virtuele toestel **OPFW** met de naam is verbonden met **onpremsn1** en **onpremsn2**.
- De route van een door de gebruiker gedefinieerde gekoppeld aan **onpremsn1** geeft aan dat al het verkeer naar **onpremsn2** naar **OPFW**moet worden verzonden.

In dit stadium **onpremvm1** probeert een verbinding maken met **onpremvm2**, de UDR worden gebruikt als verkeer worden verzonden naar **OPFW** als de volgende hop. Houd er rekening mee dat de bestemming van de werkelijke pakket niet wordt gewijzigd, wordt er nog steeds aangegeven dat **onpremvm2** is de bestemming. 

Zonder het IP-doorsturen ingeschakeld voor **OPFW**, wordt de pakketten, zet u de Azure virtuele netwerken logica aangezien deze alleen pakketten worden verzonden naar een VM als het IP-adres van de VM de bestemming voor het pakket is.

Met het IP-doorsturen stuurt de logica Azure virtuele netwerk de pakketten naar OPFW, zonder te wijzigen van de oorspronkelijke doele-mailadres. **OPFW** moet de pakketten verwerken en bepalen wat u moet doen met hen.

Voor de bovenstaande scenario als u wilt werken, moet u inschakelen IP-doorschakelen op de NIC's voor **OPFW**, **AZF1**, **AZF2**en **AZF3** die worden gebruikt voor de routering (alle NIC's, behalve die gekoppeld aan het subnet management). 

## <a name="firewall-rules"></a>Firewallregels

Zoals hierboven is beschreven, IP-alleen doorschakelen zorgt ervoor pakketten zijn verzonden naar de virtuele toestellen. Uw toestel nog moet bepalen wat u moet doen met deze pakketten. U moet maken van de volgende regels in uw apparaten in de bovenstaande scenario:

### <a name="opfw"></a>OPFW

OPFW vertegenwoordigt een on-premises implementatie-apparaat met de volgende regels:

- **Route**: al het verkeer naar 10.0.0.0/16 (**azurevnet**) moet worden verzonden via tunnel **ONPREMAZURE**.
- **Beleid**: alle bidirectionele verkeer tussen **port2** en **ONPREMAZURE**.
 
### <a name="azf1"></a>AZF1

AZF1 vertegenwoordigt een Azure virtuele toestel met de volgende regels:

- **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

### <a name="azf2"></a>AZF2

AZF2 vertegenwoordigt een Azure virtuele toestel met de volgende regels:

- **Route**: al het verkeer naar 10.0.0.0/16 (**onpremvnet**) moet worden verzonden naar het Azure gateway IP-adres (dat wil zeggen 10.0.0.1) tot en met **port1**.
- **Beleid**: alle bidirectionele verkeer tussen **port1** en **port2**.

## <a name="network-security-groups-nsgs"></a>Beveiligingsgroepen netwerk (NSGs)

In dit scenario worden NSGs niet gebruikt. U kunt echter NSGs toepassen op elk subnet inkomende en uitgaande verkeer beperken. U kan bijvoorbeeld de volgende NSG regels toepassen op de externe FW-subnet.

**Inkomende**

- Alle TCP-verkeer van Internet mogen wijzigen poort 80 op een VM in het subnet.
- Alle andere verkeer vanaf Internet weigeren.

**Uitgaande**
- Weigeren al het verkeer met Internet.

## <a name="high-level-steps"></a>Hoog niveau stappen

Als u wilt implementeren in dit scenario, volgt u de hoog niveau onderstaande stappen.

1.  Meld u aan uw Azure-abonnement.
2.  Als u een VNet wilt om de on-premises netwerk nabootsen implementeren, inrichten van de resources die deel uitmaken van **ONPREMRG**.
3.  De resources die deel uitmaken van **AZURERG**inrichten.
4.  De tunnel uit **onpremvnet** naar **azurevnet**inrichten.
5.  Wanneer alle resources is ingericht, meld u aan bij **onpremvm2** en ping 10.0.3.101 als verbindingen tussen **onpremsn2** en **azsn3**wilt testen.
