<properties
   pageTitle="Het configureren van omleiding voor een circuitlijnen ExpressRoute | Microsoft Azure"
   description="In dit artikel begeleidt u bij de stappen voor het maken en de inrichting van de privé, openbare en Microsoft peering van een circuitlijnen ExpressRoute. In dit artikel leest u ook het controleren van de status, bijwerken of verwijderen van peerings voor uw circuitlijnen."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Maken en wijzigen voor een circuitlijnen ExpressRoute-mailroutering


> [AZURE.SELECTOR]
[Azure-Portal - resourcemanager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - resourcemanager](expressroute-howto-routing-arm.md)
[PowerShell - klassiek](expressroute-howto-routing-classic.md)



In dit artikel begeleidt u bij de stappen voor het maken en beheren van de configuratie van de routering voor een ExpressRoute circuitlijnen met PowerShell en het implementatiemodel resourcemanager Azure.  De onderstaande stappen ziet u ook het controleren van de status, bijwerken of verwijderen en peerings voor een circuitlijnen ExpressRoute deprovision. 


**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Vereisten voor de configuratie

- Moet u de nieuwste versie van de Azure PowerShell-modules, versie 1.0 of hoger. 
- Zorg ervoor dat u de pagina [vereisten voor](expressroute-prerequisites.md) de pagina [Routering vereisten](expressroute-routing.md) en de pagina [werkstromen](expressroute-workflows.md) hebt bekeken voordat u configuratie begint.
- U moet een actieve ExpressRoute circuitlijnen hebben. Volg de instructies te [maken een circuitlijnen ExpressRoute](expressroute-howto-circuit-arm.md) en hebben de circuitlijnen ingeschakeld door uw provider connectivity voordat u verdergaat. De circuitlijnen ExpressRoute moet zich in een staat ingerichte en is ingeschakeld voor u kunnen de cmdlets hieronder beschreven.

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders aanbod Layer 2 connectivity-services. Als u een serviceprovider aanbod beheerde Layer 3-services (meestal een IPVPN, zoals MPLS) gebruikt, wordt uw provider connectiviteit configureren en beheren omleiding voor u.

>[AZURE.IMPORTANT] We doen momenteel niet geconfigureerd door serviceproviders tot en met de service-beheerportal peerings aangekondigd. We werken over het inschakelen van deze mogelijkheid binnenkort. Neem contact op met uw provider voordat u configureert BGP peerings.

U kunt een, twee of alle drie peerings (Azure privé, Azure openbaar en Microsoft) voor een circuitlijnen ExpressRoute configureren. U kunt peerings configureren in elke gewenste volgorde. Echter, moet u ervoor zorgen dat u de configuratie van elk peering tegelijk worden voltooid. 

## <a name="azure-private-peering"></a>Azure privé peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure privé peering configuratie voor een circuitlijnen ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Azure privé peering maken

1. Importeer de PowerShell-module voor ExpressRoute.
    
    U moet het installatieprogramma van de meest recente PowerShell installeren vanuit [De galerie PowerShell](http://www.powershellgallery.com/) en de resourcemanager Azure-modules importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. U moet PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle AzureRM.* modules in het versiebereik bekend semantic importeren

        Import-AzureRM

    U kunt ook gewoon een select module in het versiebereik bekend semantic importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken van ExpressRoute circuitlijnen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-arm.md) en hebt deze is ingericht door de provider connectivity. 

    Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure privé peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies. 

3. Controleer de circuitlijnen ExpressRoute om ervoor te zorgen dat deze is ingericht.

    U moet eerst controleren of de circuitlijnen ExpressRoute is ingericht en ook ingeschakeld. Zie het volgende voorbeeld.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is een vergelijkbare in het voorbeeld hieronder:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Configureer Azure privé peering voor de circuitlijnen.

    Zorg ervoor dat u de volgende items hebt voordat u met de volgende stappen verdergaat:

    - Een /30 subnet voor de primaire koppeling. Dit mag geen onderdeel van een adresruimte gereserveerd voor virtuele netwerken.
    - Een /30 subnet voor de secundaire koppeling. Dit mag geen onderdeel van een adresruimte gereserveerd voor virtuele netwerken.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen. U kunt een privé als getal voor deze peering. Zorg ervoor dat u 65515 niet worden gebruikt.
    - Een MD5-hash als u ervoor kiest om een te gebruiken. **Dit is optioneel**.
    
    U kunt de volgende cmdlet om te configureren Azure privé peering voor uw circuitlijnen uitvoeren.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Als u wilt een MD5-hash gebruiken, kunt u de onderstaande cmdlet gebruiken.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN, niet klant ASN opgeeft.

### <a name="to-view-azure-private-peering-details"></a>Azure peering privégegevens weergeven

U kunt met de volgende cmdlet configuratiegegevens ophalen

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Azure privé peering configuratie bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken. De VLAN-ID van de circuitlijnen wordt in het onderstaande voorbeeld bijgewerkt tussen 100 en 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Azure privé peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren.

>[AZURE.WARNING] Moet u ervoor zorgen dat alle virtuele netwerken niet-gekoppelde uit de circuitlijnen ExpressRoute zijn voordat u deze cmdlet. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure openbare peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure openbare peering configuratie voor een circuitlijnen ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Azure openbare peering maken

1. Importeer de PowerShell-module voor ExpressRoute.
    
    U moet de meest recente versie PowerShell-installer installeren vanuit [De galerie PowerShell](http://www.powershellgallery.com/) en de resourcemanager Azure-modules importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. U moet PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle AzureRM.* modules in het versiebereik bekend semantic importeren

        Import-AzureRM

    U kunt ook gewoon een select module in het versiebereik bekend semantic importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken van ExpressRoute circuitlijnen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-arm.md) en hebt deze is ingericht door de provider connectivity. 

    Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure openbare peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies.

3. Controleer ExpressRoute circuitlijnen om ervoor te zorgen dat deze is ingericht.

    U moet eerst controleren of de circuitlijnen ExpressRoute is ingericht en ook ingeschakeld. Zie het volgende voorbeeld.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is een vergelijkbare in het voorbeeld hieronder:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Configureer Azure openbare peering voor de circuitlijnen.

    Zorgt ervoor dat u de volgende informatie voordat u verder verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel.
    - Een /30 subnet voor de secundaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen.
    - Een MD5-hash als u ervoor kiest om een te gebruiken. **Dit is optioneel**.
    
    U kunt de volgende cmdlet om te configureren Azure openbare peering voor uw circuitlijnen uitvoeren

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    U kunt de volgende cmdlet gebruiken als u wilt een MD5-hash gebruiken

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN en niet klant ASN opgeeft.

### <a name="to-view-azure-public-peering-details"></a>Azure openbare peering details weergeven

U kunt met de volgende cmdlet configuratiegegevens ophalen

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Azure openbare peering configuratie bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

De VLAN-ID van de circuitlijnen wordt bijgewerkt van 200 op 600 in het voorbeeld hierboven.

### <a name="to-delete-azure-public-peering"></a>Azure openbare peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de peering configuratie van Microsoft voor een circuitlijnen ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering maken

1. Importeer de PowerShell-module voor ExpressRoute.
    
    U moet het installatieprogramma van de meest recente PowerShell installeren vanuit [De galerie PowerShell](http://www.powershellgallery.com/) en de resourcemanager Azure-modules importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. U moet PowerShell uitvoeren als beheerder.

        Install-Module AzureRM

        Install-AzureRM

    Alle AzureRM.* modules in het versiebereik bekend semantic importeren

        Import-AzureRM

    U kunt ook gewoon een select module in het versiebereik bekend semantic importeren 
        
        Import-Module AzureRM.Network 

    Aanmelden bij uw account

        Login-AzureRmAccount

    Selecteer het abonnement dat u wilt maken van ExpressRoute circuitlijnen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. ExpressRoute circuits maken.
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-arm.md) en hebt deze is ingericht door de provider connectivity. 

    Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure privé peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies.

3. Controleer ExpressRoute circuitlijnen om ervoor te zorgen dat deze is ingericht.

    U moet eerst controleren of de circuitlijnen ExpressRoute is ingericht en ook ingeschakeld. Zie het volgende voorbeeld.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Het antwoord is een vergelijkbare in het voorbeeld hieronder:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Microsoft peering voor de circuitlijnen configureren.

    Zorg ervoor dat u hebt de volgende informatie voordat u verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4 voorvoegsel waarvan u de eigenaar en geregistreerd in een RIR / IR.
    - Een /30 subnet voor de secundaire koppeling. Dit moet een geldige openbare IPv4 voorvoegsel waarvan u de eigenaar en geregistreerd in een RIR / IR.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen.
    - Aangekondigd voorvoegsels voor eenheden: U moet ondersteuning bieden voor een lijst met alle voorvoegsels u van plan bent om via de sessie BGP bekend te maken. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. U kunt een door komma's gescheiden lijst verzenden als u van plan bent om te verzenden van een reeks voorvoegsels voor eenheden. Deze voorvoegsels moeten aan u zijn geregistreerd in een RIR / IR.
    - Klant ASN: Als u reclame voorvoegsels die niet zijn geregistreerd op de peering als getal, kunt u de AS-nummer waaraan ze zijn geregistreerd. **Dit is optioneel**.
    - De registernaam van het routeren: Kunt u de RIR / IR waarmee de AS getal en voorvoegsels voor eenheden zijn geregistreerd.
    - Een hash MD5, als u ervoor kiest om een te gebruiken. **Dit is optioneel.**
    
    U kunt de volgende cmdlet om te configureren voor uw circuitlijnen peering Microsoft uitvoeren

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Voor Microsoft peering informatie

U kunt met de volgende cmdlet configuratiegegevens krijgen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Peering configuratie van Microsoft bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Microsoft peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Volgende stappen

Volgende stap, [koppeling een VNet naar een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-arm.md).

-  Zie voor meer informatie over werkstromen voor het ExpressRoute [ExpressRoute werkstromen](expressroute-workflows.md).

-  Zie voor meer informatie over circuitlijnen peering, [ExpressRoute circuits en routeren domeinen](expressroute-circuit-peerings.md).

-  Zie voor meer informatie over het werken met virtuele netwerken [virtuele netwerk overzicht](../virtual-network/virtual-networks-overview.md).

