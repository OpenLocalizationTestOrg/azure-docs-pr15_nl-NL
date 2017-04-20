De stappen voor deze taak gebruikt u een VNet op basis van de volgende waarden. Aanvullende instellingen en namen kunnen u ook vinden in deze lijst. We gebruik niet deze lijst rechtstreeks in een van de stappen, hoewel we Voeg variabelen op basis van de waarden in deze lijst. U kunt de lijst wilt gebruiken als een verwijzing, de waarden vervangen door uw eigen kopiëren.

Configuratie referentielijst:
    
- Virtuele netwerknaam = "TestVNet"
- Virtuele netwerk-adresruimte 192.168.0.0/16 =
- Resourcegroep = "TestRG"
- De naam van de Subnet1 = "FrontEnd" 
- Subnet1-adresruimte = "192.168.0.0/16"
- De naam van de gateway-Subnet: 'GatewaySubnet' u moet altijd een gateway subnet *GatewaySubnet*naam.
- Gateway Subnet-adresruimte = "192.168.200.0/26"
- Regio = "Oost US"
- De gatewaynaam van de = "GW"
- De naam van de gateway-IP-= "GWIP"
- Gateway IP-configuratie naam = "gwipconf"
-  Typ = "ExpressRoute" dit type voor een ExpressRoute-configuratie is vereist.
- Openbare IP-gatewaynaam = "gwpip"


## <a name="add-a-gateway"></a>Een gateway toevoegen

1. Verbinding maken met uw Azure-abonnement. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Uw variabelen voor deze oefening declareren. In dit voorbeeld wordt het gebruik de variabelen gebruikt in het onderstaande voorbeeld. Zorg ervoor dat deze overeenkomen met de instellingen die u wilt gebruiken als u wilt bewerken. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Hiermee slaat u het object virtueel netwerk als een variabele.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Een gateway-subnet toevoegen aan uw Virtual Network. De gateway-subnet moet de naam 'GatewaySubnet'. U moet maken van een gateway waarvoor /27 is of groter (/ 26/25, enz.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. De configuratie instellen.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. De gateway-subnet opslaan als een variabele.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Een openbare IP-adres aanvragen. Het IP-adres is aangevraagd voordat het maken van de gateway. U kunt geen het IP-adres dat u gebruiken wilt. Deze wordt dynamisch toegewezen. U gebruikt dit IP-adres in het volgende configuratiegedeelte. De AllocationMethod moet dynamisch.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Maak de configuratie voor de gateway. De gatewayconfiguratie bepaalt het subnet en het openbare IP-adres gebruiken. In deze stap geeft u de configuratie die wordt gebruikt bij het maken van de gateway. Deze stap wordt het object gateway niet gemaakt. Gebruik het onderstaande voorbeeld om de gatewayconfiguratie te maken. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Maken van de gateway. In deze stap is het **-GatewayType** vooral belangrijk. U kunt de waarde **ExpressRoute**moet gebruiken. Houd er rekening mee dat na het uitvoeren van deze cmdlets, de gateway kunt 20 minuten of langer duren om te maken.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Controleren of dat de gateway is gemaakt

Gebruik de onderstaande opdracht om te bevestigen dat de gateway is gemaakt.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Formaat van een gateway

Er zijn een aantal [Gateway SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). U kunt de volgende opdracht uit de Gateway-SKU op elk gewenst moment wijzigen.

>[AZURE.IMPORTANT] Deze opdracht werkt niet voor UltraPerformance gateway. Als u wilt wijzigen van de gateway bij naar een gateway UltraPerformance, verwijdert u eerst de bestaande ExpressRoute gateway en maak een nieuwe UltraPerformance gateway. Als u wilt uw gateway van een gateway UltraPerformance downgraden, verwijdert u eerst de gateway UltraPerformance en maak een nieuwe gateway.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht verwijderen van een gateway

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
