<properties
   pageTitle="Verbinding maken met het implementatiemodel resourcemanager-en de portal van Azure VNets | Microsoft Azure"
   description="Maak een gateway VPN-verbinding tussen VNets met behulp van resourcemanager en de Azure-portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Een VNet-naar-VNet-verbinding met behulp van de Azure portal configureren

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassieke - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)


In dit artikel begeleidt u bij de stappen voor het maken van een verbinding tussen VNets in het implementatiemodel resourcemanager via VPN Gateway en de Azure-portal.

Wanneer u de portal van Azure virtuele netwerken gebruikt, wordt de VNets moet in hetzelfde abonnement. Als uw virtuele netwerken in verschillende abonnementen zijn, kunt u ze nog steeds verbinden met behulp van de [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) -stappen.

![V2V-diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Implementatiemodellen en methoden voor VNet-naar-VNet verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het VNet-naar-VNet configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Over VNet-naar-VNet verbindingen

Een virtueel netwerk verbinden met een ander virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinden met een on-premises site-locatie. Een gateway Azure VPN beide typen connectivity gebruiken om een beveiligde tunnel met IPsec/IKE. De VNets die u verbinding maakt kan zijn in verschillende regio's of in de verschillende abonnementen.

U kunt zelfs VNet-naar-VNet communicatie met meerdere locaties configuraties combineren. Hiermee kunt u tot stand brengen netwerktopologieën die combineren cross-premises-connectiviteit met onderling virtuele netwerkconnectiviteit, zoals wordt weergegeven in het volgende diagram:

![Over verbindingen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Over verbindingen")

### <a name="why-connect-virtual-networks"></a>Waarom verbinding virtuele netwerken?

U kunt verbinding maken met virtuele netwerken voor de volgende oorzaken hebben:

- **Cross geografische regio-redundantie en geografische-aanwezigheid**
    - U kunt instellen uw eigen geografische herhaling of de synchronisatie met beveiligde verbindingen zonder te gaan via internetgerichte eindpunten.
    - Met Azure verkeer managervorm en taakverdeling, kunt u maximaal beschikbare werkbelasting met geografische redundantie instellen tussen meerdere Azure regio's. Een belangrijke voorbeeld is voor het instellen van SQL altijd op met de beschikbaarheid van de groepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met moeten worden geïsoleerd of begrenzing**
    - Binnen hetzelfde regio, kunt u toepassingen met meerdere niveaus instellen met meerdere virtuele netwerken verbonden samen vanwege moeten worden geïsoleerd of administratieve vereisten.

Zie de [VNet-naar-VNet Veelgestelde vragen](#faq) aan het einde van dit artikel voor meer informatie over VNet-naar-VNet verbindingen.

### <a name="values"></a>Voorbeeldinstellingen

Wanneer u deze stappen als een oefening, kunt u de configuratie voorbeeldwaarden. Voor voorbeeld doeleinden gebruiken we meerdere adres spaties voor elke VNet. VNet-naar-VNet configuraties nodig geen echter meerdere adres spaties.

**Waarden voor TestVNet1:**

- De naam van de VNet: TestVNet1
- Adresruimte: 10.11.0.0/16
    - De subnetnaam van de: FrontEnd
    - Subnet adresbereiken: 10.11.0.0/24
- Resourcegroep: TestRG1
- Locatie: Oost-Amerikaanse
- Adresruimte: 10.12.0.0/16
    - De subnetnaam van de: BackEnd
    - Subnet adresbereiken: 10.12.0.0/24
- De naam van de gateway-Subnet: GatewaySubnet (dit wordt automatisch doorvoeren in de portal)
    - Gateway Subnet adresbereiken: 10.11.255.0/27
- DNS-Server: Gebruik de IP-adres van uw DNS-Server
- Virtual Network Gateway-sleutel: TestVNet1GW
- Gateway-Type: VPN
- Type VPN: Route gebaseerde
- SKU: Selecteer de Gateway-SKU die u wilt gebruiken
- Openbare IP-adresnaam: TestVNet1GWIP
- Verbinding waarden:
    - Naam: TestVNet1toTestVNet4
    - Gedeelde sleutel: U kunt de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat wanneer u de verbinding tussen de VNets maakt, de waarde moet overeenkomen.

**Waarden voor TestVNet4:**

- De naam van de VNet: TestVNet4
- Adresruimte: 10.41.0.0/16
    - De subnetnaam van de: FrontEnd
    - Subnet adresbereiken: 10.41.0.0/24
- Resourcegroep: TestRG1
- Locatie: West VS
- Adresruimte: 10.42.0.0/16
    - De subnetnaam van de: BackEnd
    - Subnet adresbereiken: 10.42.0.0/24
- De naam van de GatewaySubnet: GatewaySubnet (dit wordt automatisch doorvoeren in de portal)
    - GatewaySubnet adresbereiken: 10.41.255.0/27
- DNS-Server: Gebruik de IP-adres van uw DNS-Server
- Virtual Network Gateway-sleutel: TestVNet4GW
- Gateway-Type: VPN
- Type VPN: Route gebaseerde
- SKU: Selecteer de Gateway-SKU die u wilt gebruiken
- Openbare IP-adresnaam: TestVNet4GWIP
- Verbinding waarden:
    - Naam: TestVNet4toTestVNet1
    - Gedeelde sleutel: U kunt de gedeelde sleutel zelf maken. In dit voorbeeld gebruiken we abc123. Het belangrijkste is dat wanneer u de verbinding tussen de VNets maakt, de waarde moet overeenkomen.


## <a name="CreatVNet"></a>1. maken en configureren van TestVNet1

Als u al een VNet hebt, controleert u of de instellingen compatibel is met het ontwerp van uw VPN gateway. Speciale aandacht besteden aan eventuele subnetten die laat deze met andere netwerken overlappen mogelijk. Als er overlappende subnetten, werkt de verbinding niet goed. Als uw VNet is geconfigureerd met de juiste instellingen, kunt u de stappen in de sectie [een DNS-server opgeven](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>Een virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Voeg extra adresruimte en subnetten maken

U kunt extra adresruimte toevoegen en subnetten maken zodra uw VNet is gemaakt.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. een subnet gateway maken

Voordat u verbinding met uw netwerk virtuele een gateway, moet u eerst de gateway subnet maken voor het virtuele netwerk waarnaar u een verbinding wilt maken. Indien mogelijk, is het raadzaam te maken van een gateway subnet met een CIDR blok /28 of /27 kan alleen worden aangeboden voldoende IP-adressen zodat aanvullende toekomstige configuratievereisten.

Als u deze configuratie als oefening maakt, raadpleegt u deze [voorbeeldinstellingen](#values) bij het maken van uw subnet, gateway.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Een gateway-subnet maken

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Geef een DNS-server (optioneel)

Als u naamresolutie voor virtuele machines die zijn geïmplementeerd in uw VNets hebt wilt, moet u een DNS-server opgeven.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. een gateway virtueel netwerk maken

In deze stap maakt u de gateway virtueel netwerk voor uw VNet. Deze stap duurt maximaal 45 minuten duren. Als u deze configuratie als oefening maakt, kunt u verwijzen naar de [voorbeeld-instellingen](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Een gateway virtueel netwerk maken

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. maken en configureren van TestVNet4

Wanneer u TestVNet1 hebt geconfigureerd, maakt u TestVNet4 herhaalt de voorgaande stappen, worden vervangen door de waarden met die van TestVNet4. U hoeft niet te wachten totdat de gateway virtueel netwerk voor TestVNet1 is voltooid voordat u configureert TestVNet4 maken. Als u uw eigen waarden gebruikt, zorg dat de adres-afstand van elkaar niet overlappen met een van de VNets waarmee u verbinding wilt maken.


## <a name="TestVNet1Connection"></a>7. de TestVNet1 verbinding configureren

Wanneer de gateways virtueel netwerk voor zowel TestVNet1 als TestVNet4 hebt voltooid, kunt u uw virtuele netwerk gateway verbindingen maken. In dit gedeelte maakt u een verbinding uit VNet1 naar VNet4.

1. In **alle resources**, navigeer naar de gateway virtueel netwerk voor uw VNet. Bijvoorbeeld: **TestVNet1GW**. Klik op **TestVNet1GW** als u wilt openen van het blad van de gateway virtueel netwerk.

    ![Verbindingen blade] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Verbindingen blade")

2. Klik op **+ toevoegen** als u wilt openen van het blad **verbinding toevoegen** .

3. Typ op het blad **verbinding toevoegen** in het naamveld een naam voor de verbinding. Bijvoorbeeld: **TestVNet1toTestVNet4**.

    ![De naam van de verbinding] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "De naam van de verbinding")

4. Voor **type verbinding**. Selecteer **VNet-naar-VNet** in de vervolgkeuzelijst.

5. De waarde van het **eerste virtueel netwerkgateway** -veld wordt automatisch ingevuld omdat u deze verbinding van de gateway opgegeven virtueel netwerk maakt.

6. Het **tweede virtueel netwerkgateway** -veld is de gateway virtueel netwerk van de VNet die u wilt een verbinding maken met. Klik op **Kies een andere gateway van virtueel netwerk** om te openen van het blad **Kies virtueel netwerkgateway** .

    ![Verbinding toevoegen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Een verbinding toevoegen")

7. Bekijk de gateways virtueel netwerk die worden weergegeven op deze blade. Zoals u ziet dat alleen virtueel netwerkgateways die in uw abonnement worden vermeld. Als u verbinden met een virtueel netwerkgateway die zich niet in uw abonnement wilt, gebruik u het [artikel PowerShell](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klik op de gateway virtueel netwerk waarmee u verbinding wilt maken.
 
9. Typ in het veld **gedeeld sleutel** een gedeelde sleutel voor de verbinding. U kunt genereren of deze toets zelf maakt. In een site op site-verbinding zou de sleutel die u precies hetzelfde is voor uw apparaat on-premises implementatie en uw virtuele gateway netwerkverbinding. Het concept lijkt hier, behalve die in plaats van verbinding met een VPN-apparaat, u verbinding maakt met een ander virtueel netwerkgateway.

    ![Gedeeld-toets] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Gedeeld-toets")

10. Klik op **OK** onderaan in het blad als uw wijzigingen wilt opslaan.

## <a name="TestVNet4Connection"></a>8. de TestVNet4 verbinding configureren

Maak vervolgens een verbinding uit TestVNet4 naar TestVNet1. Gebruik de dezelfde methode die u gebruikt voor het maken van de verbinding van TestVNet1 naar TestVNet4. Zorg ervoor dat u dezelfde gedeelde sleutel gebruiken.

## <a name="VerifyConnection"></a>9. uw verbinding controleren

Controleer de verbinding. Ga als volgt te werk voor elke gateway virtueel netwerk:

1. Zoek het blad voor de gateway virtueel netwerk. Bijvoorbeeld: **TestVNet4GW**. 
2. Klik op het blad van de gateway virtueel netwerk op **verbindingen** om het blad verbindingen voor de gateway virtueel netwerk weer te geven.

Bekijk de verbindingen en controleer de status. Nadat de verbinding is gemaakt, ziet u **is voltooid** en **verbonden** als de statuswaarden.

![Is geslaagd] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Is geslaagd")

U kunt elke verbinding afzonderlijk voor meer informatie over de verbinding dubbelklikken.

![Essentials] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentials")

## <a name="faq"></a>Veelgestelde vragen over VNet-naar-VNet

De details van de veelgestelde vragen voor meer informatie over VNet-naar-VNet verbindingen weergeven.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.
