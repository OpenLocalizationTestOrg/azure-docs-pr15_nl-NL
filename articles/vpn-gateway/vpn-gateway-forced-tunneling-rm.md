<properties 
   pageTitle="Configureren afgedwongen tunneling voor Site-naar-Site verbindingen met het implementatiemodel resourcemanager | Microsoft Azure"
   description="Hoe u omleiden of 'afdwingen' alle Internet verkeer terug naar uw on-premises locatie."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Afgedwongen tunneling met het implementatiemodel van Azure resourcemanager configureren

> [AZURE.SELECTOR]
- [PowerShell - klassiek](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - resourcemanager](vpn-gateway-forced-tunneling-rm.md)

Afgedwongen tunneling kunt u omleiden of "dwingen" alle Internet verkeer terug naar uw on-premises locatie via een VPN-Site-naar-Site tunnel voor controle en controle. Dit is een beveiligingsvereiste kritieke voor de meeste enterprise IT beleid.

Zonder afgedwongen tunneling, wordt Internet verkeer uit uw VMs in Azure wordt aangegeven altijd doorlopen van Azure netwerkinfrastructuur rechtstreeks af met Internet, zonder de optie toe te staan dat u wilt controleren of het verkeer wilt controleren. Onbevoegde toegang tot Internet potentieel kan leiden tot openbaarmaking of andere soorten inbreuk op de beveiliging

In dit artikel begeleidt u bij het configureren gedwongen tunneling voor virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel resourcemanager.

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Implementatiemodellen en hulpprogramma's voor afgedwongen tunneling**

Een afgedwongen tunneling verbinding kan worden geconfigureerd voor zowel het implementatiemodel klassieke en het implementatiemodel resourcemanager. Zie de volgende tabel voor meer informatie. We bijwerken in deze tabel zodra nieuwe artikelen, nieuwe implementatiemodellen en extra's beschikbaar zijn voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar deze uit de tabel.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Informatie over gedwongen tunneling


In het volgende diagram ziet u hoe afgedwongen tunneling werkt. 

![U wordt gedwongen Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

In het bovenstaande voorbeeld wordt tunnel de Frontend subnet is niet verplicht. De werkbelasting in het subnet Frontend kunnen blijven accepteren en reageren rechtstreeks op klantaanvragen van Internet. De middelgrote en Backend-subnetten gedwongen tunnel. Een uitgaande verbindingen vanuit deze twee subnetten met Internet wordt gedwongen of teruggeleid naar de site van een on-premises implementatie via een de tunnel S2S VPN.

Hiermee kunt u te beperken en internettoegang uit uw virtuele machines controleren of cloud services in Azure wordt aangegeven, terwijl u naar het inschakelen van uw service met meerdere niveaus architectuur die is vereist. Ook kunt u toepassen afgedwongen tunnel naar de hele virtuele netwerken als er geen werkbelasting internetgerichte in uw virtuele netwerken.

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Afgedwongen tunneling in Azure wordt aangegeven is via virtuele netwerk door de gebruiker gedefinieerde routes geconfigureerd. Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de gateway Azure VPN. Voor meer informatie over de routering van de gebruiker gedefinieerde en virtuele netwerken, Zie [door de gebruiker gedefinieerde routes en IP-doorsturen](../virtual-network/virtual-networks-udr-overview.md).

- Elk subnet virtueel netwerk heeft een ingebouwde, systeem routeren tabel. De systeem-mailroutering tabel heeft de volgende drie groepen van routes:

    - **Lokale VNet stuurt:** Rechtstreeks naar de bestemming VMs in hetzelfde virtuele netwerk
    
    - **On-premises routes:** Met de gateway Azure VPN
    
    - **Standaardroute:** Rechtstreeks met Internet. Pakketten bestemd is voor de privé IP-adressen niet de vorige twee routes vallen gaan verloren.

-  Deze procedure wordt door de gebruiker gedefinieerde routes (UDR) gebruikt om te maken van een routeren tabel als u wilt een standaard-route toevoegen en vervolgens de routering tabel koppelen aan uw subnetten VNet om in te schakelen afgedwongen tunneling op deze subnetten.

- Afgedwongen tunneling moet worden gekoppeld aan een VNet met een VPN-route gebaseerde gateway. U moet een 'standaard-site instellen "tussen de lokale sites cross lokale verbinding hebt met het virtuele netwerk.

- U wordt gedwongen tunneling ExpressRoute niet via deze methode is geconfigureerd, maar in plaats daarvan is ingeschakeld door een standaard-route via de ExpressRoute BGP peering sessies advertenties. Zie de [Documentatie van ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

## <a name="configuration-overview"></a>Configuratieoverzicht

De volgende procedure kunt u een resourcegroep en een VNet maken. U vervolgens een VPN-gateway maken en configureren afgedwongen tunneling. In deze procedure het virtuele netwerk 'MultiTier-VNet' 3 subnetten heeft: *Frontend*, *Midtier*en *Backend*, met 4 cross-premises verbindingen: *DefaultSiteHQ*en 3 *vertakkingen*.

De procedurestappen de *DefaultSiteHQ* instellen als de verbinding van de site standaard voor u wordt gedwongen tunneling en configureren van de Midtier en Backend-subnetten gebruik gedwongen tunneling.

    
## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie.

- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).

- U moet de nieuwste versie van de Azure resourcemanager PowerShell-cmdlets (1,0 of later) installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="configure-forced-tunneling"></a>Afgedwongen tunneling configureren

1. In de PowerShell-console, meld u aan bij uw Azure-account. Deze cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-Account. Na het aanmelden, downloaden het uw accountinstellingen, zodat ze beschikbaar voor Azure PowerShell zijn.

        Login-AzureRmAccount 

2. Een lijst met uw Azure abonnementen ophalen.

        Get-AzureRmSubscription

2. Geef het abonnement waaraan u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Maak een virtueel netwerk en subnetten opgeven. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Maak de gateways lokale netwerk.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. De tabel en de route regel maken.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Koppel de routetabel naar de Midtier en Backend-subnetten.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. De Gateway maken met een standaard-site. Deze stap duurt enige tijd om te voltooien, soms 45 minuten of meer, omdat u bent maken en configureren van de gateway.<br> De `-GatewayDefaultSite` de cmdlet-parameter waarmee de configuratie van de afgedwongen routering om te werken, dus kunt verrichten voor het configureren van deze instelling correct is. Deze parameter is beschikbaar in PowerShell 1.0 of hoger.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. De Site-naar-Site VPN vergaderen.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



