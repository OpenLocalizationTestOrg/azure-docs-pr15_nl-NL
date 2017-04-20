<properties
   pageTitle="Het configureren van actieve S2S VPN-verbindingen met Azure VPN Gateways met Azure resourcemanager en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij het configureren van actieve verbindingen met Azure VPN Gateways met Azure resourcemanager en PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Actieve S2S VPN-verbindingen met Azure VPN Gateways met Azure resourcemanager en PowerShell configureren

In dit artikel begeleidt u bij de stappen voor het maken van actieve cross-premises en VNet-naar-VNet verbindingen met de implementatiemodel resourcemanager en PowerShell.


**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Over maximaal beschikbare Cross-Premises-verbindingen

Als u wilt bereiken beschikbaarheid voor cross-premises en VNet-naar-VNet-connectiviteit, moet u meerdere VPN gateways implementeren en meerdere parallelle verbindingen maken tussen uw netwerken en Azure. Zie [ten zeerste beschikbaar Cross-Premises en VNet-naar-VNet-connectiviteit](./vpn-gateway-highlyavailable.md) voor een overzicht van connectiviteitsopties en topologie.

In dit artikel vindt de instructies voor het instellen van een VPN-verbinding actieve cross-premises en actieve verbinding tussen twee virtuele netwerken:

- [Deel 1 – maken en configureren van de gateway Azure VPN in actieve modus](#aagateway)

- [Deel 2 – actieve cross-premises verbindingen tot stand brengen](#aacrossprem)

- [Deel 3 – actieve VNet-naar-VNet verbindingen tot stand brengen](#aav2v)

- [Deel 4 - Update bestaande gateway tussen actieve en stand-by actief-](#aaupdate)

U kunt deze samen om te maken van een meer complexe, maximaal beschikbare netwerktopologie die aan uw wensen voldoet combineren.

>[AZURE.IMPORTANT] Houd er rekening mee dat de actieve alleen in HighPerformance SKU werkt


## <a name ="aagateway"></a>Deel 1 – maken en configureren van actieve VPN gateways

De volgende stappen wordt de gateway Azure VPN configureren in actieve modi. De belangrijkste verschillen tussen de actieve en stand-by actief-gateways:

- U moet twee Gateway IP-configuraties met twee openbare IP-adressen maken
- U moet de vlag EnableActiveActiveFeature instellen
- De gateway SKU moet HighPerformance

De andere eigenschappen zijn hetzelfde als de niet-actieve gateways. 

### <a name="before-you-begin"></a>Voordat u begint

- Controleer of u een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
    
- U moet installeren van de Azure resourcemanager PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1: maken en configureren van VNet1

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Voor deze oefening beginnen we door onze variabelen declareren. Het volgende voorbeeld wordt de variabelen met behulp van de waarden voor deze oefening gedeclareerd. Zorg ervoor dat de waarden vervangen door uw eigen tijdens het configureren van voor productie. U kunt deze variabelen gebruiken als u via de stappen om vertrouwd te raken met dit type configuratie worden uitgevoerd. Wijzigen van de variabelen en kopieer en plak in de PowerShell-console.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. verbinding maken met uw abonnement en maak een nieuwe resourcegroep

Zorg ervoor dat u overschakelen naar PowerShell-modus voor het gebruik van de Resource Manager-cmdlets. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

Open uw PowerShell-console en verbinding maken met uw account. Gebruik in het onderstaande voorbeeld om te verbinden:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 maken

Het onderstaande voorbeeld wordt een virtueel netwerk met de naam TestVNet1 en drie subnetten, één genoemd GatewaySubnet één genoemd FrontEnd en één genoemd Backend gemaakt. Wanneer wordt vervangen door waarden, is het belangrijk dat u altijd naam toewijzen aan uw subnet, gateway specifiek GatewaySubnet. Als u noem deze iets anders, mislukt het maken van de gateway. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Stap 2: de gateway VPN voor TestVNet1 maken met de actieve modus

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. het openbare IP-adressen en gateway IP-configuraties maken

Twee openbare IP-adressen moet worden toegewezen aan de gateway die u voor uw VNet maken wilt aanvragen. U kunt ook de subnet en IP-configuraties vereist definiëren. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. maken van de gateway VPN met actieve configuratie

Maak de gateway virtueel netwerk voor TestVNet1. Houd er rekening mee dat er twee GatewayIpConfig vermeldingen zijn en de vlag EnableActiveActiveFeature is ingesteld. Actieve modus is een gateway Route gebaseerde VPN van HighPerformance SKU vereist. Maken van een gateway, kan een tijdje duren (30 minuten of langer om uit te voeren).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. het verkrijgen van de gateway openbare IP-adressen en het BGP Peer-IP-adres

Nadat de gateway is gemaakt, moet u eerst het BGP Peer-IP-adres op Azure VPN Gateway. Dit adres is vereist voor het configureren van de Azure VPN Gateway als een Peer BGP voor uw on-premises implementatie VPN-apparaten.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Gebruik de volgende cmdlets om weer te geven van de twee openbare IP-adressen voor de gateway VPN en hun bijbehorende BGP Peer-IP-adressen voor elk exemplaar van de gateway is toegewezen:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

De volgorde van de openbare IP-adressen voor de gateway-exemplaren en de bijbehorende BGP Peering-adressen zijn hetzelfde. In dit voorbeeld de gateway VM met openbare IP van 40.112.190.5 10.12.255.4 wordt gebruikt als het adres BGP Peering en de gateway de beschikking 138.91.156.129 10.12.255.5 wordt gebruikt. Deze informatie is nodig bij het instellen van uw aan lokale VPN apparaten verbinding maakt met de actieve gateway. De gateway wordt weergegeven in het diagram onder met alle adressen:

![actieve gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Nadat de gateway is gemaakt, kunt u deze gateway tot stand brengen van de actieve cross-premises of VNet-naar-VNet verbinding. De volgende secties doorlopen de stappen voor de oefening.


## <a name ="aacrossprem"></a>Deel 2: een actieve cross-premises-verbinding tot stand brengen

Een cross-premises om verbinding te maken, moet u een lokale netwerkgateway om aan te geven van uw apparaat van de VPN on-premises implementatie en een verbinding met de gateway Azure VPN-verbinding met de gateway lokale netwerk maken. In dit voorbeeld wordt de gateway Azure VPN is in de actieve modus. Hierdoor zelfs als er slechts één on-premises implementatie VPN-apparaat (lokale netwerkgateway) en één verbinding resource, worden beide Azure VPN gateway-exemplaren S2S VPN tunnels maken met het on-premises implementatie-apparaat.

Voordat u verder gaat, zorg ervoor dat u [deel 1](#aagateway) van deze oefening hebt voltooid.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1: maken en configureren van de gateway lokale netwerk

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Deze oefening blijft de configuratie wordt weergegeven in het diagram te maken. Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Een aantal dingen te weten over de parameters van de gateway lokale netwerk:

- De gateway lokale netwerk kunt zich in de dezelfde of een andere locatie en de resourcegroep als de gateway VPN. In dit voorbeeld worden aangegeven in verschillende resourcegroepen, maar in dezelfde Azure locatie.

- Als er slechts één on-premises implementatie VPN apparaat zoals hierboven, wordt de verbinding actieve kunt werken met of zonder BGP-protocol. In dit voorbeeld BGP voor de cross lokale verbinding wordt gebruikt.

- Als BGP is ingeschakeld, is het voorvoegsel die u nodig hebt om aan te geven voor de gateway lokale netwerk het adres van de host van uw BGP Peer-IP-adres op uw apparaat VPN. In dit geval is een /32 voorvoegsel van "10.52.255.253/32".

- Als een herinnering, moet u verschillende BGP ASN's tussen uw on-premises implementatie-netwerken te gebruiken en Azure VNet gebruiken. Als ze hetzelfde zijn, moet u uw VNet ASN wijzigen als uw apparaat van de VPN on-premises implementatie de ASN wordt al wordt gebruikt om te peer met andere neighbors BGP.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. het lokale netwerkgateway maken voor Site5
    
Zorg ervoor dat u nog verbonden bent met abonnement 1 voordat u verdergaat. Maak de resourcegroep als deze nog niet is gemaakt.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet gateway en het lokale netwerkgateway

#### <a name="1-get-the-two-gateways"></a>1. de twee gateways ophalen

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. de TestVNet1 naar Site5 verbinding maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5_1 met 'EnableBGP' is ingesteld op $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN en BGP parameters voor uw on-premises implementatie VPN-apparaat

Het voorbeeld hieronder vindt u de parameters die u in de sectie BGP configuratie op uw apparaat van de VPN on-premises implementatie voor deze oefening treedt:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Voorvoegsels voor eenheden aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 voor tunnel naar 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 voor tunnel naar 138.91.156.129
    - Statische routes: bestemming 10.12.255.4/32, nexthop de VPN-tunnel in de interface 40.112.190.5 bestemming 10.12.255.5/32, nexthop de VPN-tunnel in de interface 138.91.156.129
    - eBGP Multihop: zorgen dat de optie "multihop" voor eBGP is ingeschakeld op uw apparaat, indien nodig

De verbinding moet worden gemaakt na een paar minuten en de BGP peering sessie wordt gestart zodra de IPsec-verbinding tot stand is gebracht. In dit voorbeeld heeft dusverre geconfigureerd slechts één on-premises implementatie VPN-apparaat, resulteert in het diagram hieronder wordt weergegeven:

![actief-actief-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Stap 3: twee on-premises implementatie VPN apparaten verbinden met de actieve VPN gateway

Als u twee VPN apparaten op de dezelfde on-premises netwerk hebt, kunt u dubbele redundantie door de gateway bij Azure VPN naar het tweede apparaat van de VPN-verbinding te bereiken.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. het tweede lokale netwerkgateway maken voor Site5

Houd er rekening mee dat de gateway IP-adres, adresvoorvoegsel en BGP peering adres voor de tweede lokale netwerkgateway elkaar niet met de vorige lokale netwerkgateway voor de dezelfde on-premises netwerk overlappen moeten. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Verbind de gateway VNet en de tweede lokale netwerkgateway

Maken van de verbinding van TestVNet1 naar Site5_2 met 'EnableBGP' is ingesteld op $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN en BGP parameters voor het tweede on-premises implementatie VPN-apparaat

Op dezelfde manier onder lijsten de parameters wordt u in het tweede VPN apparaat:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Voorvoegsels voor eenheden aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 voor tunnel naar 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 voor tunnel naar 138.91.156.129
    - Statische routes: bestemming 10.12.255.4/32, nexthop de VPN-tunnel in de interface 40.112.190.5 bestemming 10.12.255.5/32, nexthop de VPN-tunnel in de interface 138.91.156.129
    - eBGP Multihop: zorgen dat de optie "multihop" voor eBGP is ingeschakeld op uw apparaat, indien nodig

Nadat de verbinding (tunnels) zijn gemaakt, hebt u twee overtollige VPN-apparaten en tunnels verbinden van uw on-premises netwerk en Azure:

![twee-redundantie-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Deel 3 – een actieve VNet-naar-VNet-verbinding tot stand brengen

In dit gedeelte maakt een actieve VNet-naar-VNet-verbinding met BGP. 

De onderstaande instructies gaat u verder van de vorige stappen hierboven wordt genoemd. [Deel 1](#aagateway) als u wilt maken en configureren van TestVNet1 en de Gateway VPN met BGP, moet u uitvoeren. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de gateway VPN maken

Het is belangrijk om ervoor te zorgen dat de ruimte IP-adres van het nieuwe virtuele netwerk, TestVNet2, geen overlap heeft met een van uw VNet bereiken.

In dit voorbeeld wordt de virtuele netwerken behoren tot hetzelfde abonnement. U kunt instellen met VNet-naar-VNet verbindingen tussen de verschillende abonnementen. Zie [een verbinding VNet-naar-VNet configureren](./vpn-gateway-vnet-vnet-rm-ps.md) voor meer informatie over daarna nog meer informatie. Zorg ervoor dat u hebt toegevoegd de "-EnableBgp $True" bij het maken van de verbindingen om in te schakelen BGP.

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 maken in de nieuwe resourcegroep

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. de actieve VPN gateway maken voor TestVNet2

Twee openbare IP-adressen moet worden toegewezen aan de gateway die u voor uw VNet maken wilt aanvragen. U kunt ook de subnet en IP-configuraties vereist definiëren. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

De gateway VPN maken met de AS-nummer en de vlag 'EnableActiveActiveFeature'. Houd er rekening mee dat u de standaard ASN op uw Azure VPN gateways overschrijven moet. De ASN's voor het verbonden VNets moet verschillende BGP en overdracht routering kunt inschakelen.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Stap 2: verbinding maken met de TestVNet1 en TestVNet2 gateways

In dit voorbeeld zijn beide gateways in hetzelfde abonnement. U kunt deze stap uitvoeren in de dezelfde PowerShell-sessie.

#### <a name="1-get-both-gateways"></a>1. beide gateways ophalen

Controleer of u zich aanmeldt en verbinding maken met abonnement 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. beide verbindingen maken

In deze stap maakt u de verbinding tussen TestVNet1 en TestVNet2 en de verbinding van TestVNet2 naar TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Zorg ervoor dat u BGP inschakelen voor beide verbindingen.

Na het voltooien van deze stappen, de verbinding stelt u in een paar minuten en de BGP worden peering sessie omhoog wanneer de verbinding VNet-naar-VNet met dubbele redundantie is voltooid:

![actief-actief-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Deel 4 - Update bestaande gateway tussen actieve en stand-by actief-

De laatste sectie wordt beschreven hoe u een bestaande Azure VPN gateway uit actief-stand-by kunt configureren in actieve modus of vice versa.

>[AZURE.IMPORTANT] Houd er rekening mee dat de actieve alleen in HighPerformance SKU werkt

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Een gateway actief stand-by-tot actieve gateway configureren

#### <a name="1-gateway-parameters"></a>1. parameters van de gateway

Het volgende voorbeeld wordt een gateway actief stand-by-converteert naar een actieve gateway. U moet maken van een andere openbare IP-adres en vervolgens een tweede Gateway IP-configuratie toevoegen. Hieronder ziet u de parameters die worden gebruikt:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. het openbare IP-adres maken en vervolgens de tweede gateway IP-configuratie toevoegen

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. actieve-modus inschakelen en werk de gateway

In PowerShell voor het activeren van de werkelijke update, moet u het object gateway instellen. De SKU van de gateway-object moet ook worden gewijzigd in HighPerformance sinds deze eerder standaard is gemaakt.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Deze update kan 30 tot 45 minuten duren.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Een gateway actieve tot stand-by actief-gateway configureren

#### <a name="1-gateway-parameters"></a>1. parameters van de gateway

Gebruik dezelfde parameters als hierboven, krijgen de naam van de IP-configuratie die u wilt verwijderen.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. de gateway IP-configuratie verwijderen en de actieve-modus uitschakelen

Daarnaast moet u het object gateway instellen in PowerShell voor het activeren van de werkelijke update.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Deze update duurt maximaal 30 45 minuten.


## <a name="next-steps"></a>Volgende stappen

Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

