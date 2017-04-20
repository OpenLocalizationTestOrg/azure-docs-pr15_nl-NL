### <a name="noconnection"></a>Het toevoegen of verwijderen van voorvoegsels voor eenheden - geen gateway-verbinding

- **Om toe te voegen** extra adresvoorvoegsels op een lokaal netwerk gateway dat u hebt gemaakt, maar die niet, maar ik heb een verbinding gateway, gebruikt u het volgende voorbeeld. Zorg ervoor dat de waarden aan uw eigen wijzigen.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Verwijderen** het voorvoegsel van een adres uit een lokale netwerkgateway die geen een VPN-verbinding, gebruikt u het volgende voorbeeld. Weglaten de voorvoegsels die u niet meer nodig hebt. In dit voorbeeld wordt niet meer nodig hebt aanduiding voor 20.0.0.0/24 (uit het vorige voorbeeld), zodat we zullen bijwerken de lokale netwerk gateway en uitsluiten die voorvoegsel.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Het toevoegen of verwijderen van voorvoegsels voor eenheden - gateway verbinding bestaande

Als u uw gateway-verbinding hebt gemaakt en wilt toevoegen of verwijderen van de IP-adresvoorvoegsels opgenomen in uw lokale netwerkgateway, moet u de volgende stappen in deze volgorde doen. Hierdoor wordt in sommige uitval van uw VPN-verbinding. Wanneer uw voorvoegsels voor eenheden wordt bijgewerkt, moet u eerst de verbinding verwijderen, de voorvoegsels wijzigen en vervolgens een nieuwe verbinding te maken. Zorg dat u de waarden wijzigen in uw eigen in de onderstaande voorbeelden.

>[AZURE.IMPORTANT] Verwijder de gateway VPN geen. Als u dit doet, moet u teruggaan door de stappen om opnieuw te maken, evenals uw router on-premises implementatie met de nieuwe instellingen configureren.
 
1. De verbinding verwijderen.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Wijzig de adresvoorvoegsels voor de gateway van uw lokale netwerk.

    De variabele instellen voor de LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Wijzig de voorvoegsels voor eenheden.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. De verbinding maken. In dit voorbeeld wordt een type IPSec-verbinding configureert. Wanneer u opnieuw de verbinding te maken, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) -pagina voor aanvullende verbindingstypen.

    De variabele instellen voor de VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    De verbinding maken. Houd er rekening mee dat dit voorbeeld wordt gebruikt voor de variabele $local die u in de vorige stap hebt ingesteld.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
