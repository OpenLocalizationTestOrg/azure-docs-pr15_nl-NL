<properties
   pageTitle="Maak VNet Peering met Powershell-cmdlets | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk met behulp van de Azure portal in resourcemanager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>VNet Peering met Powershell-cmdlets maken

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om te maken van een VNet peering via PowerShell:

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.

> [AZURE.NOTE] PowerShell-cmdlet voor het beheren van VNet peering wordt geleverd bij [Azure PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lees virtueel netwerk-objecten:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Om te bepalen VNet peering, moet u twee koppelingen, één voor elke richting maken. De volgende stap wordt eerst een VNet peering koppeling voor VNet1 naar VNet2 maken:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Uitvoer ziet:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Deze stap maakt een VNet peering koppeling voor VNet2 naar VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Uitvoer ziet:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Nadat de VNet peering koppeling is gemaakt, kunt u de verbindingsstatus hieronder kunt zien:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Uitvoer ziet:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Er zijn een paar configureerbare eigenschappen voor VNet peering:

  	|Optie|Beschrijving|Standaard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Opgeven of adresruimte van Peer-VNet moeten deel uit van de Tag Virtual_network|Ja|
  	|AllowForwardedTraffic|Opgeven of verkeer niet die afkomstig zijn van een peered VNet wordt geaccepteerd of verwijderd|Nee|
  	|AllowGatewayTransit|Kunt u de peer VNet gebruik van de gateway VNet|Nee|
  	|UseRemoteGateways|Gebruik van de peer VNet gateway. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit geselecteerd. U kunt deze optie niet gebruiken als er een gateway geconfigureerd|Nee|

    Elke koppeling in het VNet peering heeft de set met eigenschappen hierboven. U kunt bijvoorbeeld AllowVirtualNetworkAccess ingesteld op waar voor VNet peering koppeling VNet1 naar VNet2 en stel deze in op ONWAAR voor de VNet peering koppeling in de andere richting.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    U kunt uitvoeren Get-AzureRmVirtualNetworkPeering om te controleren van dubbele waarde voor de eigenschap na de wijziging. In de uitvoer ziet u dat allowforwardedtraffic set gewijzigd in True na het uitvoeren van de bovenstaande cmdlets.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Nadat peering in dit scenario is gemaakt, moet u mogelijk zijn om te starten, de verbindingen vanaf een virtuele machine met een virtuele machine van beide VNets. Standaard AllowVirtualNetworkAccess waar is en de juiste ACL's als u wilt dat de communicatie tussen VNets VNet peering wordt inrichten. U kunt nog steeds netwerk beveiligingsregels groep (NSG) als u wilt blokkeren connectiviteit tussen specifieke subnetten of virtuele machines gedetailleerde controle over access tussen twee virtuele netwerken toepassen.  Raadpleeg voor meer informatie over het maken van regels voor NSG in dit [artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Volg de onderstaande stappen als u wilt maken VNet peering in abonnementen via PowerShell:

1. Aanmelden bij Azure met beschermde gebruiker-A de account voor abonnement-A en uitvoeren van de volgende cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Dit is niet een vereiste, peering kan worden vastgesteld evenzeer peering aanvragen gebruikers afzonderlijk voor de desbetreffende VNets verhogen zo lang maken als de aanvragen overeenkomen. Een gebruiker met bevoegdheden van de andere VNet toevoegen als een gebruiker in de lokale VNet gemakkelijker moet de configuratie.

2. Meld u aan bij Azure met bevoegdheden gebruiker-B van account voor abonnement-B en de volgende cmdlet uitvoeren:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Gebruiker-A is in login sessie, voert u de volgende cmdlet:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. In de gebruiker-B login sessie, moet u de onderstaande cmdlet uitvoeren:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nadat peering is gemaakt, is een virtuele machine in VNet3 moet kunnen communiceren met een virtuele machine in VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u de onderstaande tot stand brengen van de VNet peering PowerShell-cmdlets uitvoeren.  U moet de eigenschap AllowForwardedTraffic ingesteld op waar en VNET1 koppelen aan HubVNet, waarmee het binnenkomende verkeer van buiten de peering VNet-adresruimte.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Nadat peering is gemaakt, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) en definiëren van een gebruiker gedefinieerde route (UDR) VNet1 verkeer door een virtueel toestel gebruik van de mogelijkheden doorsturen. Als u het hopadres van de volgende in de route opgeeft, stelt u deze naar het IP-adres van het virtuele toestel in de peer VNet HubVNet. Hieronder volgt een voorbeeld:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Als u wilt een VNet peering tussen een klassieke virtuele netwerk en een resourcemanager Azure virtual netwerk in PowerShell hebt gemaakt, de volgende stappen uit te voeren:

1. Virtuele netwerkobject voor **VNET1**, het resourcemanager Azure virtuele netwerk als volgt:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Om te bepalen VNet peering in dit scenario, slechts één koppeling is vereist, specifiek een koppeling uit **VNET1** naar **VNET2**. Deze stap is vereist als u weet uw klassieke VNet resource-ID. De resource groep-ID-indeling ziet er zo:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Zorg ervoor dat SubscriptionID, ResourceGroupName en VirtualNetworkName vervangen door de juiste namen.

    Dit kunt doen door het volgende:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Wanneer de VNet peering koppeling is gemaakt, ziet u de verbindingsstatus zoals wordt weergegeven in de onderstaande uitvoer:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>VNet Peering verwijderen

1.  Als u wilt verwijderen de VNet peering, moet u de volgende cmdlet uitvoeren:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Wanneer u een koppeling in een VNET peering verwijderen, wordt de verbindingsstatus peer gaat u naar verbroken. In deze status wordt niet kunt u de koppeling opnieuw maken totdat de verbindingsstatus peer in gestart verandert. Het is raadzaam om dat u beide koppelingen verwijderen voordat u opnieuw de VNet peering maken.
