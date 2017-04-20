<properties 
   pageTitle="Over ExpressRoute virtueel netwerkgateways | Microsoft Azure"
   description="Meer informatie over virtueel netwerkgateways voor ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Over virtueel netwerkgateways voor ExpressRoute


Een gateway virtueel netwerk wordt gebruikt voor het verzenden van netwerkverkeer tussen Azure virtuele netwerken en on-premises locaties. Wanneer u een verbinding ExpressRoute configureren, moet u maken en configureren van een gateway virtueel netwerk en een virtuele gateway netwerkverbinding.

Wanneer u een virtueel netwerkgateway maakt, kunt u verschillende instellingen opgeven. Een van de vereiste instellingen geeft of de gateway wordt gebruikt voor ExpressRoute of Site-naar-Site VPN-verkeer is toegestaan. In het implementatiemodel resourcemanager, de instelling is '-GatewayType'.

Wanneer netwerkverkeer is een speciale privé verbinding verstuurd, gebruikt u het type gateway 'ExpressRoute'. Dit is ook een gateway ExpressRoute genoemd. Wanneer netwerkverkeer versleutelde op openbare Internet is verstuurd, gebruikt u het type gateway 'Vpn'. Dit is een VPN-gateway genoemd. Site-naar-Site, punt-naar-Site en VNet-naar-VNet verbindingen gebruiken een VPN-gateway. 

Elke virtueel netwerk kunt slechts één virtueel netwerkgateway per gateway type hebben. U kunt bijvoorbeeld een virtueel netwerkgateway die wordt gebruikt - GatewayType Vpn en een die wordt gebruikt - GatewayType ExpressRoute hebben. In dit artikel ligt de nadruk op ExpressRoute virtueel netwerkgateway.

## <a name="gwsku"></a>Gateway-SKU 's

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Als u upgraden van de gateway bij naar een krachtiger gateway SKU wilt, in de meeste gevallen kunt u de 'Formaat wijzigen-AzureRmVirtualNetworkGateway' PowerShell-cmdlet. Dit werkt voor upgrades naar de standaardweergave en de HighPerformance SKU's. Echter als u wilt upgraden naar de SKU UltraPerformance, moet u om de gateway opnieuw te maken.

###  <a name="aggthroughput"></a>Geschatte geaggregeerde doorvoer door gateway SKU


De volgende tabel ziet u de gateway-typen en de geschatte geaggregeerde doorvoer. In deze tabel geldt voor de resourcemanager en de klassieke implementatiemodellen.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST API's en PowerShell-cmdlets

Zie de volgende pagina's voor aanvullende technische informatie en specifieke syntaxisvereisten bij het gebruik van de REST API's en PowerShell-cmdlets voor virtueel netwerk gateway configuraties:

|**Klassieke** | **Resourcemanager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Volgende stappen

Zie [Overzicht van de ExpressRoute](expressroute-introduction.md) voor meer informatie over de beschikbare verbinding configuraties. 







 
