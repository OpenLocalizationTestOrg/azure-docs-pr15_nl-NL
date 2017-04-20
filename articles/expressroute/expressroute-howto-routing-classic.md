<properties
   pageTitle="Het configureren van omleiding voor een circuitlijnen ExpressRoute voor het klassieke implementatiemodel via PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij de stappen voor het maken en de inrichting van de privé, openbare en Microsoft peering van een circuitlijnen ExpressRoute. In dit artikel leest u ook het controleren van de status, bijwerken of verwijderen van peerings voor uw circuitlijnen."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Maken en wijzigen voor een circuitlijnen ExpressRoute-mailroutering

> [AZURE.SELECTOR]
[Azure-Portal - resourcemanager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - resourcemanager](expressroute-howto-routing-arm.md)
[PowerShell - klassiek](expressroute-howto-routing-classic.md)



In dit artikel begeleidt u bij de stappen voor het maken en beheren van de configuratie van de routering voor een ExpressRoute circuitlijnen met PowerShell en het implementatiemodel klassieke. De onderstaande stappen ziet u ook het controleren van de status, bijwerken of verwijderen en peerings voor een circuitlijnen ExpressRoute deprovision.


**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Vereisten voor de configuratie

- Moet u de nieuwste versie van de Azure PowerShell-modules. U kunt de meest recente PowerShell-module downloaden van de PowerShell-gedeelte van de [pagina Downloads van Azure](https://azure.microsoft.com/downloads/). Volg de instructies op de pagina [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor stapsgewijze instructies voor het configureren van uw computer als u wilt gebruiken van de Azure PowerShell-modules. 
- Zorg ervoor dat u de pagina [vereisten voor](expressroute-prerequisites.md) de pagina [Routering vereisten](expressroute-routing.md) en de pagina [werkstromen](expressroute-workflows.md) hebt bekeken voordat u configuratie begint.
- U moet een actieve ExpressRoute circuitlijnen hebben. Volg de instructies voor het [maken van een circuitlijnen ExpressRoute](expressroute-howto-circuit-classic.md) en hebben de circuitlijnen ingeschakeld door uw provider connectivity voordat u verdergaat. De circuitlijnen ExpressRoute moet zich in een staat ingerichte en is ingeschakeld voor u kunnen de cmdlets hieronder beschreven.

>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders aanbod Layer 2 connectivity-services. Als u een serviceprovider aanbod beheerde Layer 3-services (meestal een IPVPN, zoals MPLS) gebruikt, wordt uw provider connectiviteit configureren en beheren omleiding voor u.

U kunt een, twee of alle drie peerings (Azure privé, Azure openbaar en Microsoft) voor een circuitlijnen ExpressRoute configureren. U kunt peerings configureren in elke gewenste volgorde. Echter, moet u ervoor zorgen dat u de configuratie van elk peering tegelijk worden voltooid. 

## <a name="azure-private-peering"></a>Azure privé peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure privé peering configuratie voor een circuitlijnen ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Azure privé peering maken

1. **Importeer de PowerShell-module voor ExpressRoute.**
    
    U moet de modules Azure en ExpressRoute importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. Voer de volgende opdrachten de modules Azure en ExpressRoute importeren in de PowerShell-sessie.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken.**
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-classic.md) en hebt deze is ingericht door de provider connectivity. Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure privé peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies. 

3. **Controleer de circuitlijnen ExpressRoute om ervoor te zorgen dat deze is ingericht.**

    U moet eerst controleren of de circuitlijnen ExpressRoute is ingericht en ook ingeschakeld. Zie het volgende voorbeeld.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat de circuitlijnen Provisioned en ingeschakeld bevat. Als dit niet het geval is, kunt u werken met uw provider connectivity om uw circuitlijnen de vereiste staat en de status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Configureer Azure privé peering voor de circuitlijnen.**

    Zorg ervoor dat u de volgende items hebt voordat u met de volgende stappen verdergaat:

    - Een /30 subnet voor de primaire koppeling. Dit mag geen onderdeel van een adresruimte gereserveerd voor virtuele netwerken.
    - Een /30 subnet voor de secundaire koppeling. Dit mag geen onderdeel van een adresruimte gereserveerd voor virtuele netwerken.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen. U kunt een privé als getal voor deze peering. Zorg ervoor dat u 65515 niet worden gebruikt.
    - Een MD5-hash als u ervoor kiest om een te gebruiken. **Dit is optioneel**.
    
    U kunt de volgende cmdlet om te configureren Azure privé peering voor uw circuitlijnen uitvoeren.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Als u wilt een MD5-hash gebruiken, kunt u de onderstaande cmdlet gebruiken.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN, niet klant ASN opgeeft.

### <a name="to-view-azure-private-peering-details"></a>Azure peering privégegevens weergeven

U kunt met de volgende cmdlet configuratiegegevens ophalen

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure privé peering configuratie bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken. De VLAN-ID van de circuitlijnen wordt in het onderstaande voorbeeld bijgewerkt tussen 100 en 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Azure privé peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren.

>[AZURE.WARNING] Moet u ervoor zorgen dat alle virtuele netwerken niet-gekoppelde uit de circuitlijnen ExpressRoute zijn voordat u deze cmdlet. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure openbare peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de Azure openbare peering configuratie voor een circuitlijnen ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Azure openbare peering maken

1. **Importeer de PowerShell-module voor ExpressRoute.**
    
    U moet de modules Azure en ExpressRoute importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. Voer de volgende opdrachten de modules Azure en ExpressRoute importeren in de PowerShell-sessie. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken**
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-classic.md) en hebt deze is ingericht door de provider connectivity. Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure openbare peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies.

3. **ExpressRoute circuitlijnen om ervoor te zorgen dat deze is ingericht controleren**

    U moet eerst controleren of de circuitlijnen ExpressRoute is ingericht en ook ingeschakeld. Zie het volgende voorbeeld.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat de circuitlijnen Provisioned en ingeschakeld bevat. Als dit niet het geval is, kunt u werken met uw provider connectivity om uw circuitlijnen de vereiste staat en de status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Azure openbare peering voor de circuitlijnen configureren**

    Zorgt ervoor dat u de volgende informatie voordat u verder verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel.
    - Een /30 subnet voor de secundaire koppeling. Dit moet een geldige openbare IPv4-voorvoegsel.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen.
    - Een MD5-hash als u ervoor kiest om een te gebruiken. **Dit is optioneel**.
    
    U kunt de volgende cmdlet om te configureren Azure openbare peering voor uw circuitlijnen uitvoeren

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    U kunt de volgende cmdlet gebruiken als u wilt een MD5-hash gebruiken

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Zorg ervoor dat u uw AS-nummer als peering ASN en niet klant ASN opgeeft.

### <a name="to-view-azure-public-peering-details"></a>Azure openbare peering details weergeven

U kunt met de volgende cmdlet configuratiegegevens ophalen

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure openbare peering configuratie bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

De VLAN-ID van de circuitlijnen wordt bijgewerkt van 200 op 600 in het voorbeeld hierboven.

### <a name="to-delete-azure-public-peering"></a>Azure openbare peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

In deze sectie bevat instructies voor het maken, ophalen, bijwerken en verwijderen van de peering configuratie van Microsoft voor een circuitlijnen ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering maken

1. **Importeer de PowerShell-module voor ExpressRoute.**
    
    U moet de modules Azure en ExpressRoute importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. Voer de volgende opdrachten de modules Azure en ExpressRoute importeren in de PowerShell-sessie.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **ExpressRoute circuits maken**
    
    Volg de instructies voor het maken van een [ExpressRoute circuitlijnen](expressroute-howto-circuit-classic.md) en hebt deze is ingericht door de provider connectivity. Als uw provider connectivity beheerde Layer 3-services biedt, kunt u uw provider connectivity om in te schakelen Azure privé peering voor u aanvragen. In dat geval u de instructies die worden vermeld in de volgende secties niet nodig. Als uw provider connectivity niet wordt beheerd omleiding voor u, na het maken van uw circuitlijnen, volgt u de onderstaande instructies.

3. **ExpressRoute circuitlijnen om ervoor te zorgen dat deze is ingericht controleren**

    Eerst moet u controleren of de circuitlijnen ExpressRoute Provisioned en ingeschakeld status is.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Zorg ervoor dat de circuitlijnen Provisioned en ingeschakeld bevat. Als dit niet het geval is, kunt u werken met uw provider connectivity om uw circuitlijnen de vereiste staat en de status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Microsoft peering voor de circuitlijnen configureren**

    Zorg ervoor dat u hebt de volgende informatie voordat u verdergaat.

    - Een /30 subnet voor de primaire koppeling. Dit moet een geldige openbare IPv4 voorvoegsel waarvan u de eigenaar en geregistreerd in een RIR / IR.
    - Een /30 subnet voor de secundaire koppeling. Dit moet een geldige openbare IPv4 voorvoegsel waarvan u de eigenaar en geregistreerd in een RIR / IR.
    - Een geldige VLAN-ID tot stand brengen van deze peering op. Zorg ervoor dat er geen bij andere peering in het circuitlijnen gebruikmaakt van dezelfde VLAN-ID.
    - Als getal voor peering. U kunt zowel 2-byte of 4-byte als getallen.
    - Aangekondigd voorvoegsels voor eenheden: U moet ondersteuning bieden voor een lijst met alle voorvoegsels u van plan bent om via de sessie BGP bekend te maken. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. U kunt een door komma's gescheiden lijst verzenden als u van plan bent om te verzenden van een reeks voorvoegsels voor eenheden. Deze voorvoegsels moeten aan u zijn geregistreerd in een RIR / IR.
    - Klant ASN: Als u reclame voorvoegsels die niet zijn geregistreerd op de peering als getal, kunt u de AS-nummer waaraan ze zijn geregistreerd. **Dit is optioneel**.
    - De registernaam van het routeren: Kunt u de RIR / IR waarmee de AS getal en voorvoegsels voor eenheden zijn geregistreerd.
    - Een MD5-hash, als u ervoor kiest om een te gebruiken. **Dit is optioneel.**
    
    U kunt de volgende cmdlet om te configureren van Microsoft pering voor uw circuitlijnen uitvoeren

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Microsoft peering details weergeven

U kunt met de volgende cmdlet configuratiegegevens krijgen.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Peering configuratie van Microsoft bij te werken

U kunt een willekeurig deel van de configuratie met behulp van de volgende cmdlet bijwerken.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft peering verwijderen

U kunt uw peering configuratie verwijderen door de volgende cmdlet uit te voeren.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Volgende stappen

Volgende, [koppeling een VNet naar een circuitlijnen ExpressRoute](expressroute-howto-linkvnet-classic.md).


-  Zie voor meer informatie over werkstromen voor het [ExpressRoute werkstromen](expressroute-workflows.md).
-  Zie voor meer informatie over circuitlijnen peering, [ExpressRoute circuits en routeren domeinen](expressroute-circuit-peerings.md).

