<properties
   pageTitle="ExpressRoute circuits verplaatsen van klassiek naar resourcemanager | Microsoft Azure"
   description="Deze pagina wordt beschreven hoe verplaats ik een klassieke circuitlijnen aan het implementatiemodel resourcemanager."
   documentationCenter="na"
   services="expressroute"
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
   ms.author="ganesr"/>


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>ExpressRoute circuits verplaatsen van het klassieke aan het implementatiemodel resourcemanager

## <a name="configuration-prerequisites"></a>Vereisten voor de configuratie

- Moet u de nieuwste versie van de Azure PowerShell-modules (ten minste versie 1.0).
- Zorg ervoor dat u de [vereisten](expressroute-prerequisites.md), [routeren vereisten](expressroute-routing.md)en [werkstromen](expressroute-workflows.md) hebt bekeken voordat u configuratie begint.
- Voordat u verder voorgaande en controleer de gegevens die is verstrekt onder [een circuitlijnen ExpressRoute uit klassieke naar resourcemanager verplaatsen](expressroute-move.md). Zorg ervoor dat u hebt vastgelegd de grenzen en beperkingen van de mogelijkheden.
- Als u verplaatsen van een circuitlijnen Azure ExpressRoute uit het implementatiemodel klassieke aan het implementatiemodel van Azure resourcemanager wilt, moet u de circuitlijnen volledig geconfigureerd en operationele in het implementatiemodel klassieke hebben.
- Zorg ervoor dat er een resourcegroep die is gemaakt in het implementatiemodel resourcemanager.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>De circuitlijnen ExpressRoute verplaatsen naar het implementatiemodel resourcemanager

Zodat u deze tijdens het klassieke zowel de resourcemanager implementatiemodellen gebruiken kunt, moet u een circuitlijnen ExpressRoute aan het implementatiemodel resourcemanager verplaatsen. U kunt dit doen door de volgende PowerShell-opdrachten uit te voeren.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Stap 1: Circuitlijnen details van het implementatiemodel klassieke verzamelen

Moet u eerst verzamelen van informatie over uw circuitlijnen ExpressRoute.

Meld u aan bij de Azure klassieke omgeving en de service-toets te verzamelen. U kunt het volgende PowerShell-fragment gebruiken om de informatie verzamelen die:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopieer de **service-toets** van de circuitlijnen die u wilt overstappen naar het implementatiemodel resourcemanager.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Stap 2: Meld u aan bij de Resource Manager-omgeving en een nieuwe resourcegroep maken

U kunt een nieuwe resourcegroep maken met behulp van de volgende fragment:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

U kunt ook een bestaande resourcegroep gebruiken als u dat gedaan hebt.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Stap 3: De circuitlijnen ExpressRoute verplaatsen naar het implementatiemodel resourcemanager

U bent nu klaar om te Beweeg over uw circuitlijnen ExpressRoute uit het klassieke aan het implementatiemodel resourcemanager. Bekijk de informatie die onder [een circuitlijnen ExpressRoute uit het klassieke aan het implementatiemodel resourcemanager verplaatsen](expressroute-move.md) voordat u doorgaat.

U kunt dit doen door het volgende fragment uit te voeren:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Nadat de overstap is voltooid, wordt de nieuwe naam die wordt weergegeven in de vorige cmdlet worden gebruikt om de resource. De circuitlijnen in principe gewijzigd.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Een circuitlijnen ExpressRoute voor beide implementatiemodellen inschakelen

Voordat u toegang tot het implementatiemodel beheren, moet u uw circuitlijnen ExpressRoute verplaatsen naar het implementatiemodel resourcemanager.

De volgende cmdlet voor toegang tot beide implementatiemodellen uitvoeren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nadat u deze bewerking is geslaagd, is mogelijk om weer te geven van de circuitlijnen in het implementatiemodel klassieke.

Voer de volgende als u de details van de circuitlijnen ExpressRoute:

    get-azurededicatedcircuit

U moet zien van de service-toets wordt vermeld. Nu kunt u koppelingen naar de ExpressRoute circuitlijnen met uw standaard klassieke implementatie model opdrachten voor klassieke VNets en uw standaard ARM-opdrachten voor ARM VNETs beheren. De volgende artikelen wordt u begeleid bij het beheren van koppelingen naar de circuitlijnen ExpressRoute:

- [Het virtuele netwerk koppelen aan uw circuitlijnen ExpressRoute in het implementatiemodel resourcemanager](expressroute-howto-linkvnet-arm.md)
- [Het virtuele netwerk koppelen aan uw circuitlijnen ExpressRoute in het implementatiemodel klassieke](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>De circuitlijnen ExpressRoute aan het implementatiemodel klassieke uitschakelen

De volgende cmdlet om uit te schakelen toegang tot het objectmodel klassieke implementatie uitvoeren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nadat u deze bewerking is geslaagd, is het niet mogelijk om weer te geven van de circuitlijnen in het implementatiemodel klassieke.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuitlijnen hebt gemaakt, zorg dat u het volgende doen:

- [Maken en wijzigen omleiding voor uw circuitlijnen ExpressRoute](expressroute-howto-routing-arm.md)
- [Het virtuele netwerk koppelen aan uw circuitlijnen ExpressRoute](expressroute-howto-linkvnet-arm.md)
