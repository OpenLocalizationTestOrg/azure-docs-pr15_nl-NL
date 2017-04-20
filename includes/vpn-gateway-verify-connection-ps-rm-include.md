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