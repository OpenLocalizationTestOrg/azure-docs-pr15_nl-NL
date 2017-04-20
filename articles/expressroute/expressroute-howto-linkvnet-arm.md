<properties 
   pageTitle="Een virtueel netwerk koppelen aan een circuitlijnen ExpressRoute via PowerShell | Microsoft Azure"
   description="Dit document bevat een overzicht van het virtuele netwerken (VNets) koppelen aan ExpressRoute circuits met behulp van de Resource Manager implementatiemodel and PowerShell."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
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
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Een virtueel netwerk koppelen aan een circuitlijnen ExpressRoute

> [AZURE.SELECTOR]
- [Azure-Portal - bronbeheer](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - resourcemanager](expressroute-howto-linkvnet-arm.md)
- [PowerShell - klassiek](expressroute-howto-linkvnet-classic.md)


In dit artikel kunt u virtuele netwerken (VNets) koppelen aan Azure ExpressRoute circuits met behulp van de Resource Manager implementatiemodel and PowerShell. Virtuele netwerken zijn in de hetzelfde abonnement of een deel van een ander abonnement.

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Vereisten voor de configuratie

- Moet u de nieuwste versie van de Azure PowerShell-modules (ten minste versie 1.0). Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor meer informatie over het installeren van de PowerShell-cmdlets.
- U moet de [vereisten](expressroute-prerequisites.md), [routeren vereisten](expressroute-routing.md)en [werkstromen](expressroute-workflows.md) bekijken voordat u configuratie begint.
- U moet een actieve ExpressRoute circuitlijnen hebben. 
    - Volg de instructies voor het [maken van een circuitlijnen ExpressRoute](expressroute-howto-circuit-arm.md) en de circuitlijnen ingeschakeld door uw provider connectivity hebben. 
    - Zorg ervoor dat er Azure privé peering geconfigureerd voor uw circuitlijnen. Zie het artikel [routering configureren](expressroute-howto-routing-arm.md) voor routeren instructies. 
    - Zorg ervoor dat Azure privé peering is geconfigureerd en de BGP peering tussen uw netwerk en Microsoft omhoog zodat u kunt end-to-end-connectiviteit inschakelen.
    - Zorg ervoor dat er een virtueel netwerk en een virtueel netwerkgateway gemaakt en volledig deze is ingericht. Volg de instructies voor het maken van een [VPN-gateway](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), maar moet u gebruiken `-GatewayType ExpressRoute`.

U kunt maximaal 10 virtuele netwerken koppelen aan een standaard ExpressRoute circuitlijnen. Alle virtuele netwerken moeten zich in dezelfde geopolitieke regio bij gebruik van een standaard ExpressRoute circuitlijnen. 

U kunt een virtuele netwerken buiten de geopolitieke regio van de circuitlijnen ExpressRoute koppelt of een groot aantal virtuele netwerken verbinden met uw circuitlijnen ExpressRoute als u de ExpressRoute premium-invoegtoepassing hebt ingeschakeld. Controleer de [Veelgestelde vragen](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Een virtueel netwerk in hetzelfde abonnement verbinden met een circuitlijnen

U kunt een virtueel netwerkgateway naar een circuitlijnen ExpressRoute verbinding maken met behulp van de volgende cmdlet. Zorg ervoor dat de gateway virtueel netwerk wordt gemaakt en klaar is om te worden gekoppeld voordat u de cmdlet uitvoeren:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Een virtueel netwerk in een ander abonnement van verbinden met een circuitlijnen

U kunt een circuitlijnen ExpressRoute meerdere abonnementen delen. De volgende afbeelding ziet u een eenvoudige schema van hoe delen werkt voor ExpressRoute circuits over meerdere abonnementen.

Elk van de kleinere wolken binnen de grote cloud wordt gebruikt voor abonnementen die deel uitmaakt van verschillende afdelingen binnen een organisatie. Elk van de afdelingen binnen de organisatie hun eigen abonnement kunt gebruiken voor het implementeren van hun services--, maar ze kunt delen één ExpressRoute uw on-premises netwerk verbinding te maken. Één afdeling (in dit voorbeeld: IT) kan de eigenaar bent van de circuitlijnen ExpressRoute. Overige abonnementen binnen de organisatie kan de circuitlijnen ExpressRoute kunnen gebruiken.

>[AZURE.NOTE] Connectiviteit en bandbreedte kosten voor de speciale circuitlijnen wordt toegepast op de eigenaar van de ExpressRoute circuitlijnen. Alle virtuele netwerken delen de dezelfde bandbreedte.

![Cross-abonnement connectivity](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Beheer

De *eigenaar van de circuitlijnen* is een geautoriseerde ervaren gebruiker van de ExpressRoute circuitlijnen resource. De eigenaar van de circuitlijnen kunt vergunningen die kunnen worden ingewisseld door *circuitlijnen gebruikers*maken. *Circuitlijnen gebruikers* zijn eigenaren van virtueel netwerkgateways (die zich niet in hetzelfde abonnement als de circuitlijnen ExpressRoute). *Circuitlijnen gebruikers* kunnen inwisselen vergunningen (één autorisatie per virtuele netwerk).

De *eigenaar van de circuitlijnen* heeft de kracht om te wijzigen en intrekken vergunningen op elk gewenst moment. De resultaten van een vergunning in alle koppeling verbindingen worden verwijderd uit het abonnement waarvoor toegang is ingetrokken intrekken.

### <a name="circuit-owner-operations"></a>Circuitlijnen eigenaar bewerkingen 

#### <a name="creating-an-authorization"></a>Een vergunning maken
    
De eigenaar van de circuitlijnen Hiermee maakt u een vergunning. Dit levert het maken van een sleutel autorisatie die kan worden gebruikt door een gebruiker circuitlijnen hun gateways virtueel netwerk naar de circuitlijnen ExpressRoute verbinding te maken. Een vergunning geldt voor slechts één verbinding.

Het codefragment van de volgende cmdlet ziet u hoe u een vergunning maakt:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

Het antwoord op deze bevat de autorisatie-toets en de status:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>Reviseren van vergunningen

De eigenaar van de circuitlijnen kunt bekijken van alle vergunningen die op een bepaalde circuitlijnen worden uitgegeven door de volgende cmdlet uit te voeren:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>Toe te voegen vergunningen

De eigenaar van de circuitlijnen kunt vergunningen toevoegen met behulp van de volgende cmdlet:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>Vergunningen verwijderen

De eigenaar van de circuitlijnen kunt intrekken/verwijderen vergunningen aan de gebruiker door de volgende cmdlet uit te voeren:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>Circuitlijnen gebruiker bewerkingen

De gebruiker circuitlijnen moet de peer-ID en een autorisatie-sleutel uit de eigenaar van de circuitlijnen. De toets autorisatie is een GUID.

Peer-ID is, kan worden gecontroleerd uit de volgende opdracht uit.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>Wisselen verbinding vergunningen

De volgende cmdlet om in te wisselen van een koppeling autorisatie kan worden uitgevoerd door de gebruiker circuitlijnen:

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>Verbinding vergunningen vrijgeven

U kunt een vergunning vrijgeven door te verwijderen van de verbinding die de circuitlijnen ExpressRoute bij het virtuele netwerk is gekoppeld.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over ExpressRoute.
