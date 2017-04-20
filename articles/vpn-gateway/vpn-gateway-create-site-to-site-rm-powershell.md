<properties
   pageTitle="Een virtueel netwerk maken met een Site op Site-VPN-verbinding met Azure resourcemanager en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij een VNet met het implementatiemodel resourcemanager en deze verbinden met uw lokale on-premises netwerk via een VPN-S2S gateway-verbinding te maken."
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Een VNet maken met de verbinding van een Site-naar-Site via PowerShell

> [AZURE.SELECTOR]
- [Resourcemanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resourcemanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassieke - klassieke Portal](vpn-gateway-site-to-site-create.md)

In dit artikel begeleidt u bij het maken van een virtueel netwerk en een Site-naar-Site VPN gateway-verbinding voor uw on-premises netwerk met het implementatiemodel van Azure resourcemanager. Site-naar-Site-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.

![Diagram van de site-naar-Site] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "site-naar-site") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Implementatiemodellen en methoden voor het Site-naar-Site-verbindingen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

De volgende tabel ziet u de momenteel beschikbaar implementatiemodellen en methoden voor het Site-naar-Site configuraties. Wanneer een artikel met configuratiestappen beschikbaar is, koppelen we rechtstreeks toe vanuit deze tabel. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Aanvullende configuraties

Als u wilt VNets met elkaar verbinden, maar niet een verbinding met een on-premises locatie maakt, raadpleegt u [een verbinding VNet-naar-VNet configureren](vpn-gateway-vnet-vnet-rm-ps.md). Als u wilt dat de verbinding van een Site-naar-Site met een VNet die beschikt over een verbinding toevoegen, raadpleegt u [toevoegen een S2S verbinding met een VNet met een VPN-verbinding voor bestaande gateway](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met de configuratie.

- Een VPN-apparaat en iemand die kan configureert. Zie [informatie over VPN-apparaten](vpn-gateway-about-vpn-devices.md). Als u niet bekend zijn met het configureren van uw apparaat VPN of niet bekend bent met het IP-adres bereiken zich in uw on-premises netwerkconfiguratie, moet u te coördineren met iemand die deze informatie voor u geven kunt.

- Een extern omlaag openbare IP-adres voor uw apparaat VPN. Dit IP-adres kan niet worden gevonden achter een NAT bevindt.
    
- Een Azure-abonnement. Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
    
- De meest recente versie van de Azure resourcemanager PowerShell-cmdlets. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="Login"></a>1. verbinding maken met uw abonnement 

Zorg ervoor dat u overschakelen naar PowerShell-modus voor het gebruik van de Resource Manager-cmdlets. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.

Open uw PowerShell-console en verbinding maken met uw account. Gebruik in het onderstaande voorbeeld om te verbinden:

    Login-AzureRmAccount

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription 

Geef het abonnement waaraan u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. een virtueel netwerk en een gateway-subnet maken

De voorbeelden gebruikt een gateway subnet van /28. Het is mogelijk te maken van een gateway-subnet kleinst /29, wordt u aangeraden die u maakt een grotere subnet waarin meer adressen door ten minste /28 of /27 te selecteren. Hierdoor wordt voor voldoende adressen zijn voor mogelijke extra configuraties kunt u in de toekomst.

Als u al een virtueel netwerk met een gateway-subnetten die is/29 of groter zijn, kunt u verder gaan naar [uw lokale netwerkgateway toevoegen](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Een virtueel netwerk en een gateway-subnet maken

In het onderstaande voorbeeld gebruiken om een virtueel netwerk en een gateway-subnet te maken. Vervangt door de waarden voor uw eigen. 

Maak eerst een resourcegroep:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Maak vervolgens uw virtuele netwerk. Controleer of dat de adres-spaties die u opgeeft elkaar niet overlappen een van de adres-spaties die op uw on-premises netwerk.

In het onderstaande voorbeeld wordt een virtueel netwerk met de naam *testvnet* en twee subnetten, één genoemd *GatewaySubnet* gemaakt en de andere *Subnet1*genoemd. Het is belangrijk één subnet benoemde specifiek *GatewaySubnet*maken. Als u noem deze iets anders, mislukt de configuratie van de verbinding. 

Stel de variabelen.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Maak de VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Een gateway-subnet toevoegen aan een virtueel netwerk dat u al hebt gemaakt

Deze stap is alleen vereist als u moet een gateway-subnet toevoegen aan een VNet die u eerder hebt gemaakt.

U kunt uw subnet, gateway maken met behulp van het onderstaande voorbeeld. Zorg ervoor dat de gateway-subnet 'GatewaySubnet' een naam. Als u noem deze iets anders, kunt u een subnet maken, maar Azure won't worden behandeld als een gateway-subnet.

Stel de variabelen.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

De gateway-subnet maken.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

De configuratie instellen. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>uw lokale netwerkgateway toevoegen

In een virtueel netwerk, worden de gateway lokale netwerk meestal verwijst naar uw on-premises locatie. U Geef de site een naam waarmee Azure kan verwijzen naar deze en ook opgeven het voorvoegsel adres ruimte voor de gateway lokale netwerk. 

Azure gebruikt het IP-adresvoorvoegsel dat u wilt aangeven welke te verzenden naar uw on-premises locatie opgeven. Dit betekent dat u moet opgeven van elk adresvoorvoegsel dat moet worden gekoppeld aan uw lokale netwerkgateway. Als uw on-premises netwerk verandert, kunt u eenvoudig deze voorvoegsels bijwerken. 

Wanneer u met de voorbeelden PowerShell, moet u het volgende:
    
- De *GatewayIPAddress* is het IP-adres van uw apparaat van de VPN on-premises implementatie. Uw apparaat VPN kan niet worden gevonden achter een NAT bevindt. 
- De *AddressPrefix* is uw on-premises implementatie-adresruimte.

Een gateway lokale netwerk met een enkel adresvoorvoegsel toevoegen:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Een gateway lokale netwerk met meerdere adresvoorvoegsels toevoegen:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>IP-adresvoorvoegsels voor uw LAN gateway wijzigen

Soms komt wijzigen uw lokale netwerk gateway voorvoegsels. De stappen voor het wijzigen van uw IP-adresvoorvoegsels, is afhankelijk van of u een gateway VPN-verbinding hebt gemaakt. Zie het gedeelte [wijzigen IP-adresvoorvoegsels voor een lokale netwerkgateway](#modify) van dit artikel.


## <a name="PublicIP"></a>4. een openbare IP-adres van de gateway VPN aanvragen

Vervolgens vraagt u een openbare IP-adres moet worden toegewezen aan uw Azure VNet VPN gateway. Dit is niet hetzelfde IP-adres dat is toegewezen aan uw apparaat VPN; in plaats daarvan deze aan de gateway Azure VPN zelf toegewezen. U kunt geen het IP-adres dat u wilt gebruiken. Dit is dynamisch toegewezen aan de gateway. Kunt u dit IP-adres tijdens het configureren van uw apparaat met on-premises implementatie VPN-verbinding maken met de gateway.

De gateway Azure VPN voor het momenteel resourcemanager implementatiemodel biedt alleen ondersteuning voor openbare IP-adressen met behulp van de methode voor dynamische toewijzing. Dit houdt echter niet in dat het IP-adres wordt gewijzigd. Alleen de wijzigingen Azure VPN gateway IP-adres is wanneer de gateway is verwijderd en opnieuw gemaakt. Het openbare IP-adres van gateway wordt niet in het formaat wijzigen, opnieuw instellen of andere interne onderhoud/upgrades van uw Azure VPN gateway gewijzigd.

Gebruik het volgende PowerShell-voorbeeld:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. de gateway IP-adressen configuratie maken

De gatewayconfiguratie bepaalt het subnet en het openbare IP-adres gebruiken. In het onderstaande voorbeeld gebruiken om de gatewayconfiguratie te maken.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. de gateway virtueel netwerk maken

In deze stap maakt u de gateway virtueel netwerk. Maken van een gateway kan een lange tijd in beslag nemen. Vaak 45 minuten of meer. 

Gebruik de volgende waarden:

- De *-GatewayType* voor de configuratie van een Site-naar-Site is *Vpn*. Het type van de gateway is altijd specifiek voor de configuratie die u implementeert. Andere gateway-configuraties kunnen bijvoorbeeld - GatewayType ExpressRoute vragen. 

- De *-VpnType* kunt zijn *RouteBased* (genoemd een dynamische Gateway in sommige documentatie) of *PolicyBased* (genoemd een statische Gateway in sommige documentatie). Zie voor meer informatie over VPN gateway typen, [Over VPN Gateways](vpn-gateway-about-vpngateways.md#vpntype).
- De *-GatewaySku* kan zijn *standaard*, *standaard*of *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. uw apparaat VPN configureren

In dit stadium moet u het openbare IP-adres van de gateway virtueel netwerk voor het configureren van uw apparaat van de VPN on-premises implementatie. Werken met de fabrikant van uw apparaat voor specifieke configuratiegegevens. U kunt ook verwijzen naar de [VPN-apparaten](vpn-gateway-about-vpn-devices.md) voor meer informatie.

Als u wilt zoeken in het openbare IP-adres van uw gateway virtueel netwerk, gebruikt u in het onderstaande voorbeeld:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. de VPN-verbinding maken

Maak vervolgens het Site-naar-Site VPN-verbinding tussen uw gateway virtueel netwerk en het apparaat VPN. Zorg ervoor dat de waarden vervangen door uw eigen. De gedeelde sleutel moet overeenkomen met de waarde die u voor de configuratie van uw VPN-apparaten gebruikt. U ziet dat de `-ConnectionType` voor Site-naar-Site *IPsec is*. 

Stel de variabelen.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

De verbinding maken.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Na een korte tijdens de verbinding wordt tot stand worden gebracht. 

## <a name="toverify"></a>Om te controleren of een VPN-verbinding

Er zijn verschillende manieren om te controleren of uw VPN-verbinding.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>IP-adresvoorvoegsels voor een lokale netwerkgateway wijzigen

Als u wijzigen van de voorvoegsels voor de gateway van uw lokale netwerk wilt, gebruikt u de volgende instructies. Twee sets met instructies worden gegeven. De instructies die u kiest, is afhankelijk van of u al uw gateway-verbinding hebt gemaakt. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Voor het wijzigen van de gateway IP-adres van een gateway lokale netwerk

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

- U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

- Voor informatie over BGP, raadpleegt u de [BGP-overzicht](vpn-gateway-bgp-overview.md) en [hoe u BGP configureert](vpn-gateway-bgp-resource-manager-ps.md).

