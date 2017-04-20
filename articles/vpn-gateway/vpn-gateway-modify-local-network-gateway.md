<properties
   pageTitle="Lokale netwerk gateway IP-adres voorvoegsels voor eenheden en IP-gateway wijzigen | Microsoft Azure"
   description="In dit artikel begeleidt u bij het wijzigen van IP-adresvoorvoegsels voor uw LAN gateway"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Lokale netwerk via PowerShell gateway-instellingen wijzigen

Soms wijzigen de instellingen voor uw LAN gateway AddressPrefix of GatewayIPAddress. De onderstaande instructies helpen u uw lokale netwerk gateway-instellingen wijzigen. U kunt ook deze instellingen in de portal van Azure wijzigen.

## <a name="before-you-begin"></a>Voordat u begint
    
U moet de meest recente versie van de Azure resourcemanager PowerShell-cmdlets installeren. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="to-modify-ip-address-prefixes"></a>IP-adresvoorvoegsels wijzigen

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Voor het wijzigen van het IP-adres van de gateway

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt de gateway-verbinding controleren. Zie [een gateway-verbinding verifiëren](vpn-gateway-verify-connection-resource-manager.md).

