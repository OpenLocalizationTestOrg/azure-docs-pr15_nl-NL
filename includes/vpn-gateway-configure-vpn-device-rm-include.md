
Als u wilt configureren op uw apparaat VPN, moet u het openbare IP-adres van de gateway virtueel netwerk voor het configureren van uw apparaat van de VPN on-premises implementatie. Werken met de fabrikant van uw apparaat voor specifieke configuratiegegevens en configureren van uw apparaat. Raadpleeg de [VPN-apparaten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) voor meer informatie over VPN-apparaten die ook met Azure werken.

Als u wilt zoeken in het openbare IP-adres van uw virtuele netwerkgateway via PowerShell, gebruikt u in het onderstaande voorbeeld:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

U kunt ook het openbare IP-adres voor uw gateway virtueel netwerk weergeven met behulp van de Azure-portal. Navigeer naar **virtuele netwerkgateways**en klik op de naam van de gateway.