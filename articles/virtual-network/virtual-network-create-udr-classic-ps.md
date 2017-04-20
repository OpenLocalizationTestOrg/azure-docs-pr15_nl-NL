<properties 
   pageTitle="Routering bepalen en het gebruik van virtuele toestellen via PowerShell in het implementatiemodel klassieke | Microsoft Azure"
   description="Meer informatie over het om te bepalen in VNets via PowerShell in het implementatiemodel klassieke-mailroutering"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Besturingselement Routering en gebruik virtual toestellen (klassieke) via PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

De steekproef Azure PowerShell-opdrachten onderstaande een eenvoudige omgeving al hebt gemaakt verwachten op basis van het bovenstaande scenario. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in het [maken van een VNet (klassieke) via PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end van subnet maken
Als u wilt maken van de tabel en de route nodig voor de front-end van subnet op basis van de bovenstaande scenario, de onderstaande stappen uit te voeren.

3. Voer de **`New-AzureRouteTable`** cmdlet om een routetabel voor het subnet front-end te maken.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Uitvoer:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Voer de **`Set-AzureRoute`** een route maken in de routetabel-cmdlet die zijn gemaakt boven aan het verzenden van al het verkeer bestemd is voor het back-end-subnet (192.168.2.0/24) naar de **FW1** VM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Uitvoer:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Voer de **`Set-AzureSubnetRouteTable`** koppelen van de routetabel-cmdlet die hierboven zijn gemaakt met het subnet **FrontEnd** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor het back-end-subnet maken
Als u wilt maken van de tabel en de route die u nodig hebt voor het back-end-subnet op basis van de bovenstaande scenario, de onderstaande stappen uit te voeren.

3. Voer de **`New-AzureRouteTable`** cmdlet om een routetabel voor het back-end-subnet te maken.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Voer de **`Set-AzureRoute`** een route maken in de routetabel-cmdlet die zijn gemaakt boven aan het verzenden van al het verkeer bestemd de front-end van subnet (192.168.1.0/24) de **FW1** VM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Voer de **`Set-AzureSubnetRouteTable`** koppelen van de routetabel-cmdlet die hierboven zijn gemaakt met de **BackEnd** -subnet.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>IP-doorsturen op de VM FW1 inschakelen
Schakel IP doorsturen in de VM FW1 door de onderstaande stappen uit te voeren.

1. Voer de **`Get-AzureIPForwarding`** controleren de status van IP-doorsturen-cmdlet

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Uitvoer:

        Disabled

2. Voer de **`Set-AzureIPForwarding`** opdracht IP-doorsturen voor de *FW1* VM inschakelen.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
