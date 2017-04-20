<properties
   pageTitle="Maken en wijzigen van een circuitlijnen ExpressRoute met behulp van resourcemanager en PowerShell | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe maken, inrichten verifiëren, bijwerken, verwijderen en een circuitlijnen ExpressRoute deprovision."
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


# <a name="create-and-modify-an-expressroute-circuit"></a>Maken en wijzigen van een circuitlijnen ExpressRoute

> [AZURE.SELECTOR]
[Azure-Portal - resourcemanager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - resourcemanager](expressroute-howto-circuit-arm.md)
[PowerShell - klassiek](expressroute-howto-circuit-classic.md)


In dit artikel wordt beschreven hoe een circuitlijnen Azure ExpressRoute maken met behulp van Windows PowerShell-cmdlets en het implementatiemodel resourcemanager Azure. In dit artikel ziet u ook hoe u Controleer de status van de circuitlijnen, moet u dit, bijwerken of verwijderen en deprovision deze.

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voordat u begint


- De meest recente versie van de Azure PowerShell-modules (ten minste versie 1.0). Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor stapsgewijze instructies voor het configureren van uw computer als u wilt gebruiken de PowerShell-modules.

- Bekijk de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u configuratie begint.

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en een circuitlijnen ExpressRoute inrichten

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Meld u aan bij uw Azure-account en selecteert u uw abonnement

Als u wilt uw configuratie begint, moet u zich aanmelden bij uw Azure-account. Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie over PowerShell. Gebruik de volgende voorbeelden om te verbinden:

    Login-AzureRmAccount

Controleer de abonnementen voor het account:

    Get-AzureRmSubscription

Selecteer het abonnement waaraan u wilt maken van een circuitlijnen ExpressRoute voor:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. de lijst met ondersteunde providers, locaties en bandbreedte ophalen

Voordat u een circuitlijnen ExpressRoute hebt gemaakt, moet u de lijst met ondersteunde connectivity providers, locaties en bandbreedte opties.

De PowerShell-cmdlet `Get-AzureRmExpressRouteServiceProvider` geeft deze informatie, die u in de volgende stappen gebruikt:

    Get-AzureRmExpressRouteServiceProvider

Controleer of uw provider connectivity er wordt weergegeven. Maak een notitie van de volgende informatie omdat u deze later moet wanneer u een circuitlijnen maakt:

- Naam

- PeeringLocations

- BandwidthsOffered

U bent nu klaar om te ExpressRoute circuits maken.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute circuits maken

Als u een resourcegroep nog geen hebt, moet u een maken voordat u uw circuitlijnen ExpressRoute maakt. U kunt dit doen door de volgende opdracht uit te voeren:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Het volgende voorbeeld ziet u hoe u een 200-Mbps ExpressRoute circuitlijnen tot en met Equinix maakt in Silicon Valley. Als u verschillende instellingen en een andere provider gebruikt, vervangt u die gegevens door wanneer u uw aanvraag kunt invullen. Hier volgt een voorbeeld-verzoek voor een nieuwe servicesleutel:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Zorg ervoor dat u de juiste SKU laag en de SKU familie opgeven:

- SKU laag bepaalt of een standaard ExpressRoute of een ExpressRoute premium-invoegtoepassing is ingeschakeld. U kunt opgeven *standaard* als u van de standaard SKU of *Premium* voor de premium-invoegtoepassing.

- SKU familie bepaalt het type facturering. U kunt *Metereddata* opgeven voor een netwerk naar gebruik data-abonnement en *Unlimiteddata* voor een onbeperkte data-abonnement. Houd er rekening mee dat u het billing type van *Metereddata* in *Unlimiteddata wijzigen kunt*, maar u kunt het type niet van *Unlimiteddata* in *Metereddata wijzigen*.


>[AZURE.IMPORTANT] Uw circuitlijnen ExpressRoute wordt gefactureerd vanaf het moment dat een servicesleutel is uitgegeven. Zorg ervoor dat u deze bewerking uitvoeren wanneer de provider connectivity klaar is voor de circuitlijnen inrichten.

Het antwoord bevat de service-toets. U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. alle ExpressRoute circuits lijst

Als u een lijst met alle ExpressRoute circuits die u hebt gemaakt, worden uitgevoerd de `Get-AzureRmExpressRouteCircuit` opdracht:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Het antwoord ziet er ongeveer als het volgende voorbeeld:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

U kunt deze informatie op elk gewenst moment kunt ophalen met behulp van de `Get-AzureRmExpressRouteCircuit` cmdlet. De oproep zonder parameters worden alle circuits. Uw servicesleutel worden vermeld in het veld *ServiceKey* :


    Get-AzureRmExpressRouteCircuit


Het antwoord ziet er ongeveer zo uit:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. het verzenden van de service-toets naar uw connectivity-provider voor het inrichten

*ServiceProviderProvisioningState* vindt u informatie over de huidige status van de inrichting van aan de service-provider. Status biedt de stand aan de zijkant van Microsoft. Zie het artikel [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) voor meer informatie over circuitlijnen Staten inrichting.

Wanneer u een nieuwe ExpressRoute circuitlijnen maakt, wordt de circuitlijnen worden in de volgende staat:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



De circuitlijnen verandert naar de volgende staat wanneer de provider connectivity wordt ingeschakeld voor u:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Als u wilt mogelijk een circuitlijnen ExpressRoute gebruiken, moet zijn in de volgende staat:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. de status en de status van de toets circuitlijnen regelmatig te controleren

De status en de status van de sleutel circuitlijnen controleren, kunt u weten wanneer uw provider uw circuitlijnen heeft ingeschakeld. Na het circuitlijnen is geconfigureerd, *ServiceProviderProvisioningState* wordt weergegeven als *Provisioned*, zoals wordt weergegeven in het volgende voorbeeld:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Het antwoord ziet er ongeveer zo uit:


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

### <a name="7-create-your-routing-configuration"></a>7. de configuratie van uw routering maken

Zie het artikel [configuratie ExpressRoute circuitlijnen routering](expressroute-howto-routing-arm.md) maken en wijzigen van circuitlijnen peerings voor stapsgewijze instructies.


>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die laag 2 connectivity services bieden. Als u gebruikmaakt van een serviceprovider die beheerde layer 3 services (meestal een IP VPN, zoals MPLS), wordt uw provider connectiviteit configureren en beheren omleiding voor u.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. een virtueel netwerk koppelen aan een circuitlijnen ExpressRoute

Een virtueel netwerk vervolgens een koppeling naar uw circuitlijnen ExpressRoute. Gebruik het artikel [Linking virtuele netwerken naar ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) wanneer u met het implementatiemodel resourcemanager werkt.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>De status van een circuitlijnen ExpressRoute ophalen

U kunt deze informatie op elk gewenst moment kunt ophalen met behulp van de `Get-AzureRmExpressRouteCircuit` cmdlet. De oproep zonder parameters worden alle circuits.

    Get-AzureRmExpressRouteCircuit


Het antwoord is vergelijkbaar met het volgende voorbeeld:


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


U kunt informatie over een specifieke ExpressRoute circuitlijnen openen door het geven van de resource groepsnaam en de naam van de circuitlijnen als een parameter aan een gesprek:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Het antwoord ziet er ongeveer als het volgende voorbeeld:


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


U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Een circuitlijnen ExpressRoute wijzigen

U kunt bepaalde eigenschappen van een circuitlijnen ExpressRoute wijzigen zonder die invloed hebben op connectivity.

U kunt het volgende met geen downtime doen:

- In- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor uw circuitlijnen ExpressRoute.
- De bandbreedte van uw circuitlijnen ExpressRoute vergroten. Houd er rekening mee dat de bandbreedte van een circuitlijnen Downgrade niet wordt ondersteund.
- Het abonnement waarop meting wijzigen van gegevens met Datalimiet naar onbeperkt gegevens. Houd er rekening mee dat het abonnement waarop meting van onbeperkte gegevens wijzigen in gegevens met Datalimiet niet wordt ondersteund.
-  U kunt in- en uitschakelen *Klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over beperkingen en tekortkomingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De ExpressRoute premium-invoegtoepassing inschakelen

U kunt de ExpressRoute premium-invoegtoepassing inschakelen voor uw bestaande circuitlijnen met behulp van het volgende PowerShell-fragment:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


De circuitlijnen hebben nu de ExpressRoute premium invoegtoepassing functies die zijn ingeschakeld. Houd er rekening mee dat we u voor de mogelijkheid van de invoegtoepassing premium facturering zodra de opdracht met succes is uitgevoerd wordt gestart.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De premium-invoegtoepassing ExpressRoute wilt uitschakelen

>[AZURE.IMPORTANT] Deze bewerking kan mislukken als u resources die ouder zijn dan wat is toegestaan voor de standaard circuitlijnen gebruikt.

Houd rekening met het volgende:

- Voordat u uit premium naar standaard downgraden, moet u ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan de circuitlijnen minder dan 10 is. Als u dit niet doet, uw updateaanvraag is mislukt en we u aan de premietarieven wordt rekening.

- U kunt alle virtuele netwerken in andere geopolitieke regio's moet ontkoppelen. Als u dit niet doet, uw aanvraag bijwerken, mislukt en we u aan de premietarieven wordt rekening.

- Uw tabel moet minder dan 4000 routes voor privé peering. Als uw tabelgrootte route groter dan 4000 routes is, wordt de sessie BGP ingevoegd en won't worden ingeschakeld totdat het aantal aangekondigde voorvoegsels daaronder 4000.

U kunt de ExpressRoute premium-invoegtoepassing voor de bestaande circuitlijnen uitschakelen met behulp van de volgende PowerShell-cmdlet:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>De ExpressRoute circuitlijnen bandbreedte bijwerken

Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor ondersteunde bandbreedte opties voor uw provider. U kunt elke grootte groter is dan de grootte van uw bestaande circuitlijnen kiezen.

>[AZURE.IMPORTANT] U kunt de bandbreedte van een circuitlijnen ExpressRoute zonder verstoringen niet reduceren. Een downgrade uitvoeren bandbreedte, moet u de circuitlijnen ExpressRoute deprovision en klikt u vervolgens een nieuwe ExpressRoute circuitlijnen opnieuw wilt inrichten.

Nadat u hebt besloten welke grootte die u nodig hebt, gebruikt u de volgende opdracht uit om het formaat van uw circuitlijnen aan:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Uw circuitlijnen wordt worden formaat van de Microsoft-kant. U kunt vervolgens moet contact opnemen met uw provider connectivity als u wilt bijwerken configuraties aan hun kant zodat deze overeenkomt met deze wijziging. Nadat u deze melding maakt, begint we u voor de optie bijgewerkte bandbreedte facturering.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Als u wilt verplaatsen van de SKU uit met datalimiet naar onbeperkt

U kunt de SKU van een circuitlijnen ExpressRoute wijzigen met behulp van het volgende PowerShell-fragment:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Toegang tot het klassieke en resourcemanager omgevingen te beheren  

Lees de instructies in [ExpressRoute verplaatsen circuits uit het klassieke aan het implementatiemodel resourcemanager](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning en verwijderen van een circuitlijnen ExpressRoute

Houd rekening met het volgende:

- U kunt alle virtuele netwerken uit het circuitlijnen ExpressRoute moet ontkoppelen. Als deze bewerking mislukt, controleert u of een virtuele netwerken aan de circuitlijnen zijn gekoppeld.

- Als de ExpressRoute circuitlijnen provider inrichten servicestatus **Provisioning** of **Provisioned** moet u werken met uw provider naar de circuitlijnen aan hun kant deprovision. We blijft reserveren van resources en u rekening totdat de serviceprovider voltooit u de circuitlijnen deprovisioning en krijgt ons.

- Als de serviceprovider heeft het circuitlijnen (de status voor het inrichten van service-provider is ingesteld op **niet ingericht**) opgeheven kunt u de circuitlijnen vervolgens verwijderen. Hierdoor wordt de facturering voor de circuitlijnen gestopt

U kunt uw circuitlijnen ExpressRoute verwijderen door de volgende opdracht uit te voeren:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuitlijnen hebt gemaakt, zorg dat u het volgende doen:

- [Maken en wijzigen omleiding voor uw circuitlijnen ExpressRoute](expressroute-howto-routing-arm.md)
- [Uw virtuele netwerk koppelen aan uw circuitlijnen ExpressRoute](expressroute-howto-linkvnet-arm.md)
