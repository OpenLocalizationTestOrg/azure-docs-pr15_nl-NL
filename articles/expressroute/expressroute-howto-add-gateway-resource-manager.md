<properties
   pageTitle="Het toevoegen van een gateway VNet met een virtueel netwerk voor ExpressRoute met resourcemanager en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij het toevoegen van een gateway Vnet naar een reeds gemaakte resourcemanager VNet voor ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Een gateway virtueel netwerk configureren voor ExpressRoute resourcemanager en PowerShell gebruiken


> [AZURE.SELECTOR]
- [PowerShell - resourcemanager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassiek](expressroute-howto-add-gateway-classic.md)


In dit artikel begeleidt u door de stappen voor het toevoegen, het formaat wijzigen en verwijderen van een gateway virtueel netwerk (VNet) voor een bestaande VNet. De stappen worden voor deze configuratie zijn specifiek voor VNets die zijn gemaakt met het **resourcemanager implementatiemodel** en die gebruikt in een ExpressRoute-configuratie. 

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Voordat u begint

Controleer of u de Azure PowerShell-cmdlets die nodig zijn voor deze configuratie hebt geïnstalleerd (1.0.2 of later). Als u de cmdlets nog niet hebt geïnstalleerd, moet u doen voordat u begint met de volgende configuratiestappen uit. Zie voor meer informatie over het installeren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Volgende stappen

Nadat u de gateway VNet hebt gemaakt, kunt u uw VNet koppelen aan een circuitlijnen ExpressRoute. Zie [koppeling een virtueel netwerk naar een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-arm.md).
