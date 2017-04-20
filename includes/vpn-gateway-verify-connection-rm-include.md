### <a name="to-verify-your-connection-by-using-powershell"></a>Voor de verificatie van uw verbinding via PowerShell

U kunt controleren of dat de verbinding is voltooid met behulp van de `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet, met of zonder `-Debug`. 

1. Gebruik het volgende voorbeeld cmdlet voor het configureren van de waarden die overeenkomen met uw eigen. Als u wordt gevraagd, selecteert u 'A' om te kunnen uitvoeren 'All'. In het voorbeeld `-Name` verwijst naar de naam van de verbinding die u hebt gemaakt en wilt testen.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Nadat u de cmdlet is voltooid, kunt u de waarden weergeven. In het onderstaande voorbeeld ziet u de verbindingsstatus als 'Connected' en ziet u ingress- en egress bytes.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Voor de verificatie van uw verbinding met behulp van de Azure-portal

Klik in de portal Azure kunt u de verbindingsstatus weergeven door te gaan naar de verbinding. Er zijn verschillende manieren u dit wilt doen. De volgende stappen uit weergeven een manier om te navigeren naar uw verbinding en controleer of.

1. Klik op **alle resources** in de [portal van Azure](http://portal.azure.com)en navigeer naar de gateway virtueel netwerk.
2. Klik op het blad voor de gateway virtueel netwerk, klikt u op **verbindingen**. Hier ziet u de status van elke verbinding.
3. Klik op de naam van de verbinding die u wilt controleren om te **Essentials**openen. In Essentials, kunt u meer informatie over de mobiele verbinding voor weergeven. De **Status** ervan is 'Geslaagd' en 'Verbonden' wanneer u een verbinding hebt gemaakt.

    ![Verbinding controleren](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)