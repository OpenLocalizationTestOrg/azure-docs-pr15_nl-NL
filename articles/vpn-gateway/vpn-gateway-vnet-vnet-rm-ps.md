<properties
   pageTitle="Verbinding maken met Azure VNets met VPN Gateway en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij het virtuele netwerken aan elkaar koppelen met behulp van Azure resourcemanager en PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Een verbinding VNet-naar-VNet configureren voor resourcemanager via PowerShell

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassieke - klassieke Portal](virtual-networks-configure-vnet-to-vnet-connection.md)

In dit artikel begeleidt u bij de stappen voor het maken van een verbinding tussen VNets in het implementatiemodel resourcemanager via een VPN-Gateway. De virtuele netwerken kunnen zijn in dezelfde of een andere regio's en uit de dezelfde of een andere abonnementen.


![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Implementatiemodellen en methoden voor VNet-naar-VNet verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het VNet-naar-VNet configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Over VNet-naar-VNet verbindingen

Een virtueel netwerk verbinden met een ander virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinden met een on-premises site-locatie. Een gateway Azure VPN beide typen connectivity gebruiken om een beveiligde tunnel met IPsec/IKE. De VNets die u verbinding maakt, kan zijn in verschillende regio's. Of in de verschillende abonnementen. U kunt zelfs VNet-naar-VNet communicatie met meerdere locaties configuraties combineren. Hiermee kunt u tot stand brengen netwerktopologieën die combineren cross-premises-connectiviteit met onderling virtuele netwerkconnectiviteit, zoals wordt weergegeven in het volgende diagram:


![Over verbindingen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Waarom verbinding virtuele netwerken?

U kunt verbinding maken met virtuele netwerken voor de volgende oorzaken hebben:

- **Cross geografische regio-redundantie en geografische-aanwezigheid**
    - U kunt instellen uw eigen geografische herhaling of de synchronisatie met beveiligde verbindingen zonder te gaan via internetgerichte eindpunten.
    - Met Azure verkeer managervorm en taakverdeling, kunt u maximaal beschikbare werkbelasting met geografische redundantie instellen tussen meerdere Azure regio's. Een belangrijke voorbeeld is voor het instellen van SQL altijd op met de beschikbaarheid van de groepen spreiden over meerdere Azure regio's.

- **Regionale meerlagige toepassingen met moeten worden geïsoleerd of begrenzing**
    - Binnen hetzelfde regio, kunt u toepassingen met meerdere niveaus instellen met meerdere virtuele netwerken verbonden samen vanwege moeten worden geïsoleerd of administratieve vereisten.


### <a name="vnet-to-vnet-faq"></a>Veelgestelde vragen over VNet-naar-VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Welke set stappen moet ik gebruiken?

In dit artikel ziet u twee verschillende groepen stappen. Een reeks stappen voor [VNets die zich in hetzelfde abonnement bevinden](#samesub), en een andere voor [VNets die zich in de verschillende abonnementen bevinden](#difsub). Het belangrijkste verschil tussen de paren is of u kunt maken en configureren van alle virtuele netwerk- en gateway resources binnen de dezelfde PowerShell-sessie.

De stappen in dit artikel met de variabelen die aan het begin van elke sectie zijn gedeclareerd. Als u al met bestaande VNets werkt, wijzigt u de variabelen zodat de instellingen in uw eigen omgeving. 

![Beide verbindingen](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Hoe u verbinding maakt VNets die in hetzelfde abonnement zijn

![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Voordat u begint
    
Voordat u begint moet u de Azure resourcemanager PowerShell-cmdlets installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="Step1"></a>Stap 1: uw IP-adresbereiken plannen


Maak twee virtuele netwerken samen met de bijbehorende gateway subnetten en configuraties in de volgende stappen. We maken een VPN-verbinding tussen de twee VNets vervolgens. Het is belangrijk is voor het plannen van het IP-adresbereiken voor configureren van uw netwerk. Houd er rekening mee dat moet u ervoor zorgen dat geen van uw VNet bereiken of lokale netwerkbereiken overlap op geen enkele manier.

We gebruiken de volgende waarden in de voorbeelden:

**Waarden voor TestVNet1:**

- De naam van de VNet: TestVNet1
- Resourcegroep: TestRG1
- Locatie: Oost-Amerikaanse
- TestVNet1: de 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Back-end: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet1GW
- Openbare IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Waarden voor TestVNet4:**

- De naam van de VNet: TestVNet4
- TestVNet2: de 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Back-end: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Resourcegroep: TestRG4
- Locatie: West VS
- DNS-Server: 8.8.8.8
- GatewayName: VNet4GW
- Openbare IP: VNet4GWIP
- VPNType: RouteBased
- Verbinding: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Stap 2: maken en configureren van TestVNet1

1. Uw variabelen declareren

    Begin met het variabelen declareren. In dit voorbeeld wordt de variabelen met behulp van de waarden voor deze oefening gedeclareerd. In de meeste gevallen moet u de waarden vervangen door uw eigen. U kunt echter deze variabelen gebruiken als u via de stappen om vertrouwd te raken met dit type configuratie uitvoert. De variabelen indien nodig, wijzigen en vervolgens kopiëren en plak ze in de PowerShell-console.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Verbinding maken met uw abonnement

    Overschakelen naar de PowerShell-modus voor het gebruik van de Resource Manager-cmdlets. Open uw PowerShell-console en verbinding maken met uw account. Met het volgende voorbeeld kunt u verbinding maken:

        Login-AzureRmAccount

    Controleer de abonnementen voor het account.

        Get-AzureRmSubscription 

    Geef het abonnement waaraan u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. De subnet configuraties voor TestVNet1 maken

    In dit voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één genoemd GatewaySubnet één genoemd FrontEnd en één genoemd Backend gemaakt. Wanneer wordt vervangen door waarden, is het belangrijk dat u altijd naam toewijzen aan uw subnet, gateway specifiek GatewaySubnet. Als u noem deze iets anders, mislukt het maken van de gateway. 

    Het volgende voorbeeld wordt de variabelen die u eerder hebt ingesteld. In dit voorbeeld is het subnet gateway een /27 gebruiken. Het is mogelijk te maken van een gateway-subnet kleinst /29, wordt u aangeraden die u maakt een grotere subnet waarin meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen zijn voor mogelijke extra configuraties kunt u in de toekomst. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. TestVNet1 maken

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Een openbare IP-adres aanvragen

    Een openbare IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maken wilt aanvragen. U ziet dat de AllocationMethod dynamisch. U kunt geen het IP-adres dat u wilt gebruiken. Dit dynamisch toegewezen aan de gateway. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. De gatewayconfiguratie maken

    De gatewayconfiguratie bepaalt het subnet en het openbare IP-adres gebruiken. Het voorbeeld gebruiken om de gatewayconfiguratie te maken. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. De gateway maken voor TestVNet1

    In deze stap maakt u de gateway virtueel netwerk voor uw TestVNet1. VNet-naar-VNet configuraties moeten een RouteBased VpnType. Maken van een gateway, kan een tijdje duren (45 minuten of meer om uit te voeren).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Stap 3: maken en configureren van TestVNet4

Wanneer u TestVNet1 hebt geconfigureerd, maak TestVNet4. Volg de stappen onder de waarden vervangen door uw eigen als dat nodig is. Deze stap kan worden uitgevoerd binnen de dezelfde PowerShell-sessie, omdat deze al in hetzelfde abonnement.

1. Uw variabelen declareren

    Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Controleer voordat u verder gaat, of dat u nog verbonden bent met abonnement 1.

2. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. De subnet configuraties voor TestVNet4 maken

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. TestVNet4 maken

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Een openbare IP-adres aanvragen

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. De gatewayconfiguratie maken

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. De gateway TestVNet4 maken

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Stap 4: verbinding maken met de gateways

1. Beide gateways virtueel netwerk ophalen

    In dit voorbeeld omdat beide gateways in hetzelfde abonnement, kan deze stap worden voltooid in de dezelfde PowerShell-sessie.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. De TestVNet1 naar TestVNet4 verbinding maken

    In deze stap maakt u de verbinding van TestVNet1 naar TestVNet4. Hier ziet u een gedeelde sleutel waarnaar wordt verwezen in de voorbeelden. U kunt uw eigen waarden voor de gedeelde sleutel. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen met. Maakt een verbinding kan een korte tijdje duren om te voltooien.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. De TestVNet4 naar TestVNet1 verbinding maken

    Deze stap is vergelijkbaar is met de bovenstaande, behalve u de verbinding van TestVNet4 naar TestVNet1 maakt. Controleer of de gedeelde toetsen overeenkomen met.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    De verbinding moet worden gemaakt na een paar minuten.

4. Controleer uw verbinding. Zie de sectie [uw verbinding controleren](#verify).


## <a name="difsub"></a>Hoe u verbinding maakt VNets die in verschillende-abonnementen zijn


![V2V-diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In dit scenario verbinding we TestVNet1 en TestVNet5 maken. TestVNet1 en TestVNet5 bevinden zich in een ander abonnement. De stappen voor deze configuratie toevoegen om extra VNet-naar-VNet verbinding te TestVNet1 verbinden met TestVNet5. 

Het verschil hier is dat enkele van de volgende configuratiestappen uit moeten worden uitgevoerd in een aparte PowerShell-sessie in de context van de tweede abonnement. Met name wanneer de twee abonnementen deel uitmaken van andere organisaties. 

De instructies gaat u verder van de vorige stappen hierboven wordt genoemd. U moet [stap 1](#Step1) en [stap 2](#Step2) als u wilt maken en configureren van TestVNet1 en de Gateway VPN voor TestVNet1 uitvoeren. Nadat u stap 1 en stap 2 hebt voltooid, gaat u verder met stap 5 TestVNet5 maken.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Stap 5: Controleer of de extra IP-adresbereiken

Het is belangrijk om ervoor te zorgen dat de ruimte IP-adres van het nieuwe virtuele netwerk, TestVNet5, geen overlap heeft met een van uw VNet bereiken of lokale netwerk gateway bereiken. 

In dit voorbeeld de virtuele netwerken mogelijk deel uitmaken van andere organisaties. Voor deze oefening kunt u de volgende waarden voor de TestVNet5:

**Waarden voor TestVNet5:**

- De naam van de VNet: TestVNet5
- Resourcegroep: TestRG5
- Locatie: Japan Oost
- TestVNet5: de 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Back-end: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet5GW
- Openbare IP: VNet5GWIP
- VPNType: RouteBased
- Verbinding: VNet5toVNet1
- ConnectionType: VNet2VNet

**Extra waarden voor TestVNet1:**

- Verbinding: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Stap 6 - maken en configureren van TestVNet5

Deze stap moet worden uitgevoerd in de context van het nieuwe abonnement. In dit onderdeel kan worden uitgevoerd door de beheerder in een andere organisatie die eigenaar is van het abonnement.

1. Uw variabelen declareren

    Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Verbinding maken met abonnement 5

    Open uw PowerShell-console en verbinding maken met uw account. Gebruik in het onderstaande voorbeeld om te verbinden:

        Login-AzureRmAccount

    Controleer de abonnementen voor het account.

        Get-AzureRmSubscription 

    Geef het abonnement waaraan u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Een nieuwe resourcegroep maken

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. De subnet configuraties voor TestVNet4 maken
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. TestVNet5 maken

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Een openbare IP-adres aanvragen

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. De gatewayconfiguratie maken

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. De gateway TestVNet5 maken

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Stap 7 - verbinding van de gateways

In dit voorbeeld omdat de gateways in de verschillende abonnementen, hebt we gesplitst deze stap twee PowerShell-sessies dat is gemarkeerd als [abonnement 1] en [abonnement 5].

1. **[Abonnement 1]** De gateway virtueel netwerk krijgen voor abonnement 1

    Controleer of u zich aanmeldt en verbinding maken met abonnement 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Kopieer de uitvoer van de volgende elementen en verzend deze naar de beheerder van abonnement 5 via e-mail of een andere methode.

        $vnet1gw.Name
        $vnet1gw.Id

    Deze twee elementen krijgen waarden die vergelijkbaar is met het volgende voorbeeld-uitvoer:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Abonnement 5]** De gateway virtueel netwerk voor abonnement 5 ophalen

    Controleer of u aanmelden en verbinding maken met abonnement 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Kopieer de uitvoer van de volgende elementen en verzend deze naar de beheerder van abonnement 1 via e-mail of een andere methode.

        $vnet5gw.Name
        $vnet5gw.Id

    Deze twee elementen krijgen waarden die vergelijkbaar is met het volgende voorbeeld-uitvoer:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Abonnement 1]** De TestVNet1 naar TestVNet5 verbinding maken

    In deze stap maakt u de verbinding van TestVNet1 naar TestVNet5. Het verschil hier is dat vnet5gw $ rechtstreeks kan niet worden verkregen omdat deze al in een ander abonnement. U moet een nieuw PowerShell-object maken met de waarden die meegedeeld van abonnement 1 in de bovenstaande stappen. Gebruik het volgende voorbeeld. Vervang de naam, -Id en gedeelde sleutel door uw eigen waarden. Het belangrijkste is dat de gedeelde sleutel voor beide verbindingen moet overeenkomen met. Maakt een verbinding kan een korte tijdje duren om te voltooien.

    Zorg ervoor dat u verbinding maakt met abonnement 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Abonnement 5]** De TestVNet5 naar TestVNet1 verbinding maken

    Deze stap is vergelijkbaar is met de bovenstaande, behalve u de verbinding van TestVNet5 naar TestVNet1 maakt. Hetzelfde proces van het maken van een PowerShell-object op basis van de waarden opgehaald van abonnement 1 geldt ook hier. Zorg dat de gedeelde toetsen overeenkomen met in deze stap.

    Zorg ervoor dat u verbinding maakt met abonnement 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Een verbinding controleren


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Volgende stappen

- Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.
- Voor informatie over BGP, raadpleegt u de [BGP-overzicht](vpn-gateway-bgp-overview.md) en [hoe u BGP configureert](vpn-gateway-bgp-resource-manager-ps.md). 

