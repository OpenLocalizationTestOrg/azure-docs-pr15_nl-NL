Als u wilt wijzigen van de gateway IP-adres, gebruikt u de `New-AzureRmVirtualNetworkGatewayConnection` cmdlet. Zo lang maken als u de naam van de gateway lokale netwerk precies hetzelfde als de bestaande naam houden, worden de instellingen worden overschreven. Op dit moment kan biedt de cmdlet 'Zet' geen ondersteuning voor het wijzigen van het IP-adres van de gateway.

### <a name="gwipnoconnection"></a>Het wijzigen van de gateway IP-adres - geen gateway-verbinding

Als u wilt bijwerken van de gateway IP-adres van uw lokale netwerkgateway die een verbinding nog niet hebt, gebruikt u het volgende voorbeeld. U kunt ook de adresvoorvoegsels op hetzelfde moment bijwerken. De instellingen die u opgeeft, worden de bestaande instellingen overschreven. Zorg ervoor dat u de bestaande naam van uw lokale netwerkgateway gebruiken. Als u niet, maakt u een nieuwe lokale netwerkgateway, het bestaande bestand niet te overschrijven.

Gebruik het volgende voorbeeld wordt de waarden voor uw eigen vervangen.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Het wijzigen van de IP-adres van de gateway - gateway verbinding bestaande

Als er al een gateway-verbinding bestaat, moet u eerst de verbinding verwijderen. Vervolgens kunt u het IP-adres van de gateway wijzigen en maak een nieuwe verbinding opnieuw. Hierdoor wordt in sommige uitval van uw VPN-verbinding.


>[AZURE.IMPORTANT] Verwijder de gateway VPN geen. Als u dit doet, moet u teruggaan door de stappen om opnieuw te maken, evenals uw router on-premises implementatie met het IP-adres dat wordt toegewezen aan de zojuist gemaakte gateway te configureren.
 

1. De verbinding verwijderen. U vindt de naam van uw verbinding met behulp van de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. De waarde GatewayIpAddress wijzigen. U kunt ook uw adresvoorvoegsels wijzigen op dit moment kan zo nodig. Houd er rekening mee dat dit de bestaande lokale netwerk gateway-instellingen worden overschreven. Gebruik de bestaande naam van uw lokale netwerkgateway wanneer u wilt wijzigen zodat de instellingen overschreven. Als u niet, maakt u een nieuwe lokale netwerkgateway, het bestaande bestand niet wordt gewijzigd.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. De verbinding maken. In dit voorbeeld wordt een type IPSec-verbinding configureert. Wanneer u opnieuw de verbinding te maken, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) -pagina voor aanvullende verbindingstypen.  U kunt uitvoeren als u de naam van de VirtualNetworkGateway, de `Get-AzureRmVirtualNetworkGateway` cmdlet.

    Stel de variabelen:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    De verbinding te maken:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

