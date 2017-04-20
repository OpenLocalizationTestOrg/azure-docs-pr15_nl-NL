<properties
   pageTitle="BGP configureren op Azure VPN Gateways met Azure resourcemanager en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij het configureren van BGP met Azure VPN Gateways met Azure resourcemanager en PowerShell."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>BGP configureren op Azure VPN Gateways met Azure resourcemanager en PowerShell

In dit artikel begeleidt u bij de stappen BGP inschakelen voor een cross-premises Site-naar-Site (S2S) VPN-verbinding en een VNet-naar-VNet-verbinding met de implementatiemodel resourcemanager en PowerShell.


**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Over BGP

BGP is het standaard protocol veelgebruikte in Internet uitwisseling van gegevens voor de Routering en bereikbaarheid tussen twee of meer netwerken. BGP kunt Azure VPN Gateways en uw on-premises implementatie VPN apparaten, BGP collega's of neighbors uitwisselen "stuurt" die worden beide gateways op de beschikbaarheid en de bereikbaarheid van deze voorvoegsels doorlopen van de gateways of routers betrokken informeren genoemd. BGP kunt ook uitschakelen overdracht routering tussen meerdere netwerken doorgeven waarmee die een gateway BGP van één BGP peer aan alle andere BGP collega's hoort.

Zie [Overzicht van BGP met Azure VPN Gateways](./vpn-gateway-bgp-overview.md) voor meer discussie op voordelen van BGP en voor meer informatie over de technische vereisten en overwegingen van het gebruik van BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Aan de slag met BGP op Azure VPN gateways

In dit artikel vindt u over de stappen aan de volgende taken uitvoeren:

- [Deel 1 – inschakelen BGP op uw Azure VPN gateway](#enablebgp)

- [Deel 2 – meerdere lokale verbinding maken met BGP](#crossprembgp)

- [Deel 3 – VNet-naar-VNet verbinding maken met BGP](#v2vbgp)

Elk onderdeel van de instructies vormt een eenvoudige bouwsteen voor BGP inschakelen in de netwerkverbinding. Als u alle drie onderdelen hebt voltooid, wordt u de topologie bouwen, zoals wordt weergegeven in het volgende diagram:

![BGP topologie](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

U kunt deze samen om te maken van een meer complexe, meerdere hoop, netwerk die aan uw wensen voldoet combineren.

## <a name ="enablebgp"></a>Deel 1 – BGP configureren op de Gateway Azure VPN

De volgende configuratiestappen wordt BGP de parameters van de gateway Azure VPN instellen, zoals wordt weergegeven in het volgende diagram:

![BGP Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Voordat u begint

- Controleer of u een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
    
- U moet installeren van de Azure resourcemanager PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Stap 1: maken en configureren van VNet1 

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Voor deze oefening beginnen we door onze variabelen declareren. Het volgende voorbeeld wordt de variabelen met behulp van de waarden voor deze oefening gedeclareerd. Zorg ervoor dat de waarden vervangen door uw eigen tijdens het configureren van voor productie. U kunt deze variabelen gebruiken als u via de stappen om vertrouwd te raken met dit type configuratie worden uitgevoerd. Wijzigen van de variabelen en kopieer en plak in de PowerShell-console.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Stap 2: de Gateway VPN maken voor TestVNet1 met BGP parameters

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. het IP- en subnet configuraties maken

Een openbare IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maken wilt aanvragen. U kunt ook de subnet en IP-configuraties vereist definiëren. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. de gateway VPN maken met de AS-nummer

Maak de gateway virtueel netwerk voor TestVNet1. Houd er rekening mee dat BGP zijn uitgerust met een VPN-Route gebaseerde gateway en ook de parameter optellen, - Asn, om in te stellen de ASN (AS-nummer) TestVNet1. Maken van een gateway, kan een tijdje duren (30 minuten of langer om uit te voeren).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. het Azure BGP Peer IP-adres verkrijgen

Nadat de gateway is gemaakt, moet u eerst het BGP Peer-IP-adres op Azure VPN Gateway. Dit adres is vereist voor het configureren van de Azure VPN Gateway als een Peer BGP voor uw on-premises implementatie VPN-apparaten.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

De laatste opdracht wordt de overeenkomstige BGP configuraties weergegeven op de Azure VPN Gateway; bijvoorbeeld:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Nadat de gateway is gemaakt, kunt u deze gateway tot stand brengen van meerdere lokale verbinding of VNet-naar-VNet verbinding met het BGP. De volgende secties doorlopen de stappen voor de oefening.

## <a name ="crossprembbgp"></a>Deel 2 – meerdere lokale verbinding maken met BGP

Een cross-premises om verbinding te maken, moet u een lokale netwerkgateway om aan te geven van uw apparaat van de VPN on-premises implementatie en een verbinding met de gateway Azure VPN-verbinding met de gateway lokale netwerk maken. Het verschil tussen de instructies in dit artikel is de aanvullende eigenschappen die zijn vereist voor het opgeven van de parameters van de configuratie BGP.

![BGP Cross-premises](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Voordat u verder gaat, zorg ervoor dat u [deel 1](#enablebgp) van deze oefening hebt voltooid.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Stap 1: maken en configureren van de gateway lokale netwerk

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Deze oefening blijft de configuratie wordt weergegeven in het diagram te maken. Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Een aantal dingen te weten over de parameters van de gateway lokale netwerk:

- De gateway lokale netwerk kunt zich in de dezelfde of een andere locatie en de resourcegroep als de gateway VPN. In dit voorbeeld ziet u ze in verschillende resourcegroepen op verschillende locaties.

- De minimale voorvoegsel die u nodig hebt om aan te geven voor de gateway lokale netwerk is het adres van de host van uw BGP Peer-IP-adres op uw apparaat VPN. In dit geval is een /32 voorvoegsel van "10.52.255.254/32".

- Als een herinnering, moet u verschillende BGP ASN's tussen uw on-premises implementatie-netwerken te gebruiken en Azure VNet gebruiken. Als ze hetzelfde zijn, moet u uw VNet ASN wijzigen als uw apparaat van de VPN on-premises implementatie al met de ASN met andere neighbors BGP op hetzelfde niveau.
    
Zorg ervoor dat u nog verbonden bent met abonnement 1 voordat u verdergaat.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. het lokale netwerkgateway maken voor Site5

Zorg ervoor dat de resourcegroep maken als dit niet gemaakt is, voordat u de gateway lokale netwerk maken. Zoals u ziet de twee aanvullende parameters voor het lokale netwerkgateway: Asn en BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Stap 2: verbinding maken met de VNet gateway en het lokale netwerkgateway

#### <a name="1-get-the-two-gateways"></a>1. de twee gateways ophalen

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. de TestVNet1 naar Site5 verbinding maken

In deze stap maakt u de verbinding van TestVNet1 naar Site5. U moet opgeven "-EnableBGP $True" BGP inschakelen voor deze verbinding. Zoals eerder is beschreven, is het mogelijk dat zowel BGP als niet-BGP verbindingen voor dezelfde Azure VPN Gateway. Tenzij BGP is ingeschakeld in de eigenschap connection, Azure niet mogelijk BGP voor deze verbinding Hoewel BGP parameters al zijn geconfigureerd op beide gateways.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Het voorbeeld hieronder vindt u de parameters die u in de sectie BGP configuratie op uw apparaat van de VPN on-premises implementatie voor deze oefening treedt:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Voorvoegsels voor eenheden aan te kondigen: (bijvoorbeeld) 10.51.0.0/16 en 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Statische route: een route voor 10.12.255.30/32, met nexthop wordt de interface van de VPN tunnel op uw apparaat toevoegen
    - eBGP Multihop: zorgen dat de optie "multihop" voor eBGP is ingeschakeld op uw apparaat, indien nodig

De verbinding moet worden gemaakt na een paar minuten en de BGP peering sessie wordt gestart zodra de IPsec-verbinding tot stand is gebracht.
 
## <a name ="v2vbgp"></a>Deel 3 – VNet-naar-VNet verbinding maken met BGP

In deze sectie voegt een VNet-naar-VNet-verbinding met BGP, zoals wordt weergegeven in het onderstaande diagram. 

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

De onderstaande instructies gaat u verder van de vorige stappen hierboven wordt genoemd. U moet uitvoeren [deel I](#enablebgp) als u wilt maken en configureren van TestVNet1 en de Gateway VPN met BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Stap 1: TestVNet2 en de gateway VPN maken

Het is belangrijk om ervoor te zorgen dat de ruimte IP-adres van het nieuwe virtuele netwerk, TestVNet2, geen overlap heeft met een van uw VNet bereiken.

In dit voorbeeld wordt de virtuele netwerken behoren tot hetzelfde abonnement. U kunt instellen dat VNet-naar-VNet verbindingen tussen de verschillende abonnementen. Zie [een verbinding VNet-naar-VNet configureren](./vpn-gateway-vnet-vnet-rm-ps.md) voor meer informatie over daarna nog meer informatie. Zorg ervoor dat u hebt toegevoegd de "-EnableBgp $True" bij het maken van de verbindingen om in te schakelen BGP.

#### <a name="1-declare-your-variables"></a>1. uw variabelen declareren

Zorg ervoor dat de waarden vervangen door de bouwstenen die u wilt gebruiken voor uw configuratie.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. TestVNet2 maken in de nieuwe resourcegroep

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. het maken van de gateway VPN voor TestVNet2 met BGP parameters

Een openbare IP-adres moet worden toegewezen aan de gateway die u voor uw VNet maken wilt aanvragen. U kunt ook de subnet en IP-configuraties vereist definiëren. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

De gateway VPN maken met een getal dat als. Houd er rekening mee dat u de standaard ASN op uw Azure VPN gateways overschrijven moet. De ASN's voor het verbonden VNets moet verschillende BGP en overdracht routering kunt inschakelen.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

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

Nadat u deze stappen uitvoert, worden de verbinding in een paar minuten definiëren, en wordt de BGP peering sessie maar één keer is de VNet-naar-VNet-verbinding voltooid.

Als u alle drie onderdelen van deze oefening hebt voltooid, tot u een netwerktopologie zoals hieronder wordt weergegeven:

![BGP voor VNet-naar-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Volgende stappen

Nadat de verbinding voltooid is, kunt u virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

