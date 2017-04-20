U moet een VNet en een gateway-subnet eerst maken voordat u op de volgende taken uitvoeren. Zie het artikel [een virtuele netwerk met behulp van de klassieke portal configureren](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) voor meer informatie.   

## <a name="add-a-gateway"></a>Een gateway toevoegen

Gebruik de onderstaande opdracht om te maken van een gateway. Zorg ervoor dat alle waarden voor uw eigen vervangen.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Controleren of dat de gateway is gemaakt

Gebruik de onderstaande opdracht om te bevestigen dat de gateway is gemaakt. Deze opdracht haalt ook de gateway-ID, die u nodig voor andere bewerkingen hebt.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Formaat van een gateway

Er zijn een aantal [Gateway SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). U kunt de volgende opdracht uit de Gateway-SKU op elk gewenst moment wijzigen.

>[AZURE.IMPORTANT] Deze opdracht werkt niet voor UltraPerformance gateway. Als u wilt wijzigen van de gateway bij naar een gateway UltraPerformance, verwijdert u eerst de bestaande ExpressRoute gateway en maak een nieuwe UltraPerformance gateway. Als u wilt uw gateway van een gateway UltraPerformance downgraden, verwijdert u eerst de gateway UltraPerformance en maak een nieuwe gateway. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht verwijderen van een gateway

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>