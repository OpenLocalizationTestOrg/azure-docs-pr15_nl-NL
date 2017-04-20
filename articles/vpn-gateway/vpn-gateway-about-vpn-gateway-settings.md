<properties 
   pageTitle="Over VPN Gateway-instellingen voor virtueel netwerkgateways | Microsoft Azure"
   description="Meer informatie over VPN Gateway-instellingen voor Azure Virtual Network."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Over VPN Gateway-instellingen

Een VPN-gateway verbinding oplossing is afhankelijk van de configuratie van meerdere resources om te kunnen verzenden netwerkverkeer tussen virtuele netwerken en on-premises locaties. Elke resource bevat configureerbare instellingen. De combinatie van de resources en instellingen bepaalt de uitkomst van de verbinding.

De secties in dit artikel worden de instellingen die zijn gerelateerd aan een VPN-gateway in het model van de implementatie **Resourcemanager** en resources. Mogelijk nuttig zijn om weer te geven van de beschikbare configuraties met behulp van de verbinding topologie diagrammen. U vindt de beschrijvingen en de topologie diagrammen voor elke verbinding-oplossing in het artikel [Over VPN Gateway](vpn-gateway-about-vpngateways.md) . 

## <a name="gwtype"></a>Gateway-typen

Elke virtueel netwerk kan slechts één virtueel netwerkgateway van elk type hebben. Wanneer u een virtueel netwerkgateway maakt, moet u ervoor zorgen dat de gateway-type voor uw configuratie in orde is.

De beschikbare waarden voor - GatewayType zijn: 

- VPN
- ExpressRoute

Een VPN-gateway is vereist de `-GatewayType` *Vpn*.  

Voorbeeld:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Gateway-SKU 's


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>De gateway SKU configureren

**De gateway SKU geven in de portal van Azure**

Als u de Azure-portal maken van een Resource Manager virtueel netwerkgateway gebruikt, kunt u de gateway SKU met behulp van de vervolgkeuzelijst. De opties die wordt weergegeven, komen overeen met de Gateway type en een VPN-type die u selecteert.

Bijvoorbeeld als u het type gateway 'VPN' en de VPN type '-beleid' selecteert, ziet u alleen de 'Eenvoudige' SKU omdat dit de enige SKU die beschikbaar zijn voor PolicyBased VPN's. Als u de 'Route-programma' selecteert, kunt u selecteren in Basic, Standard en HighPerformance SKU's. 


**De gateway SKU opgeven via PowerShell**


Hiermee geeft u het volgende PowerShell-voorbeeld de `-GatewaySku` als *standaard*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Wijzigen van een gateway SKU**

Als u wilt uw gateway SKU upgraden naar een krachtiger SKU (vanuit Basic/standaard naar HighPerformance) kunt u de `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. U kunt ook de gateway SKU grootte Gebruik deze cmdlet downgraden.

Het volgende PowerShell-voorbeeld ziet u een gateway SKU naar HighPerformance wordt gewijzigd.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Geschatte geaggregeerde doorvoer door gateway SKU en type

De volgende tabel ziet u de gateway-typen en de geschatte geaggregeerde doorvoer. In deze tabel geldt voor de resourcemanager en de klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Verbindingstypen

In het implementatiemodel resourcemanager, is elke configuratie vereist een type gateway specifieke virtuele netwerkverbinding. De beschikbare resourcemanager PowerShell waarden voor `-ConnectionType` zijn:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

In het volgende PowerShell-voorbeeld maken we een S2S-verbinding waarvoor het verbindingstype *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN typen

Wanneer u de gateway virtueel netwerk voor een VPN-gateway-configuratie maakt, moet u een VPN-type. Het type VPN die u kiest, is afhankelijk van de topologie van de verbinding die u wilt maken. Een verbinding P2S is bijvoorbeeld een type RouteBased VPN vereist. Een VPN-type kan ook zijn afhankelijk van de hardware die u wilt gebruiken. S2S configuraties moeten een VPN-apparaat. Een bepaald type van de VPN wordt alleen ondersteund door sommige VPN-apparaten.

Het type VPN dat u selecteert moet voldoen aan alle de verbinding voor de oplossing die dat u wilt maken. Bijvoorbeeld als u een S2S VPN gateway-verbinding en een P2S VPN gateway-verbinding voor hetzelfde virtuele netwerk maken wilt, gebruikt u VPN type *RouteBased* omdat P2S is een type RouteBased VPN vereist. U moet ook controleert u of uw apparaat VPN een RouteBased VPN-verbinding ondersteund. 

Wanneer u een virtueel netwerkgateway hebt gemaakt, kunt u het type VPN niet wijzigen. U moet verwijderen van de gateway virtueel netwerk en een nieuwe record maken. Er zijn twee VPN typen:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Hiermee geeft u het volgende PowerShell-voorbeeld de `-VpnType` als *RouteBased*. Wanneer u een gateway maakt, moet u ervoor zorgen dat de VpnType - voor uw configuratie klopt. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Gateway-vereisten

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Gateway-subnet

Als u wilt een virtueel netwerkgateway configureert, moet u eerst een subnet gateway maken voor uw VNet. De gateway-subnet moet de naam *GatewaySubnet* werkt alleen naar behoren. Deze naam kunt Azure weten dat dit subnet moet worden gebruikt voor de gateway.

De minimale grootte van uw subnet, gateway is volledig afhankelijk van de configuratie die u wilt maken. Hoewel het mogelijk maken van een gateway-subnet /29 zo klein is, wordt aangeraden dat u een gateway-subnet van /28 of groter maken (/ 28, /27, /26, enz.). 

Gateway groter maken, voorkomt u omhoog op groottebeperkingen gateway wordt uitgevoerd. U hebt, bijvoorbeeld een virtueel netwerkgateway gemaakt met een gateway subnet grootte /29 van een verbinding S2S. U nu wilt configureren van een S2S/ExpressRoute naast configuratie. Deze configuratie is vereist een gateway subnet minimale grootte /28. Als u wilt maken van uw configuratie, moet u het subnet gateway zodat de minimumvereisten voor de verbinding, dat wil /28 zeggen wijzigen.

De volgende resourcemanager PowerShell-voorbeeld ziet u een gateway subnet GatewaySubnet met de naam. De notatie CIDR Hiermee geeft u een /27, waardoor voldoende IP-adressen voor de meeste configuraties die momenteel aanwezig zijn, kunt u zien.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Lokale netwerkgateways

Wanneer u maakt een VPN-gateway-configuratie, staat de gateway lokale netwerk vaak voor uw on-premises locatie. In het implementatiemodel klassieke, worden de gateway lokale netwerk is een lokale Site genoemd. 

U geeft de gateway lokale netwerk een naam, het openbare IP-adres van het apparaat van de VPN on-premises implementatie en geef de adresvoorvoegsels die op de locatie van de on-premises implementatie bevinden zich. Azure kijkt naar de bestemming adresvoorvoegsels voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor de gateway van uw lokale netwerk en routeert pakketten dienovereenkomstig gewijzigd. U opgeven lokale netwerkgateways voor VNet-naar-VNet configuraties die gebruikmaken van een gateway VPN-verbinding ook.

Het volgende PowerShell-voorbeeld wordt een nieuwe lokale netwerkgateway:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Soms moet u het lokale netwerk gateway-instellingen wijzigen. Bijvoorbeeld wanneer u toevoegen aan of het adresbereik wijzigen, of als het IP-adres van het apparaat VPN gewijzigd. Voor een klassiek VNet, kunt u deze instellingen in de klassieke-portal op de pagina lokale netwerken. Voor resourcemanager, Zie [Modify gateway-instellingen voor lokale netwerk via PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API's en PowerShell-cmdlets

Zie de volgende pagina's voor aanvullende technische informatie en specifieke syntaxisvereisten bij het gebruik van de REST API's en PowerShell-cmdlets voor VPN Gateway configuraties:

|**Klassieke** | **Resourcemanager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Volgende stappen

Zie [Over VPN Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over de beschikbare verbinding configuraties. 







 
