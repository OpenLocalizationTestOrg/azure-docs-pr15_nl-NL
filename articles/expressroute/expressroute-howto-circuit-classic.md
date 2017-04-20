<properties
   pageTitle="Maken en wijzigen van een circuitlijnen ExpressRoute met behulp van klassieke implementatiemodel en PowerShell | Microsoft Azure"
   description="In dit artikel begeleidt u bij de stappen voor het maken en de inrichting van een circuitlijnen ExpressRoute. In dit artikel leest u ook het controleren van de status, bijwerken of verwijderen en uw circuitlijnen deprovision."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Maken en wijzigen van een circuitlijnen ExpressRoute

> [AZURE.SELECTOR]
[Azure-Portal - resourcemanager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - resourcemanager](expressroute-howto-circuit-arm.md)
[PowerShell - klassiek](expressroute-howto-circuit-classic.md)

In dit artikel begeleidt u bij de stappen voor het maken van een circuitlijnen Azure ExpressRoute via PowerShell-cmdlets en het implementatiemodel klassieke. In dit artikel ziet u ook het controleren van de status, bijwerken of verwijderen en een circuitlijnen ExpressRoute deprovision.

**Over Azure-implementatie-modellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Voordat u begint

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Controleer de vereisten en de werkstroom artikelen

Zorg ervoor dat u de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) hebt bekeken voordat u configuratie begint.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Installeer de meest recente versies van de Azure PowerShell-modules 

Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor stapsgewijze instructies voor het configureren van uw computer als u wilt gebruiken van de Azure PowerShell-modules.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Meld u aan bij uw Azure-account en selecteer een abonnement

1. De volgende cmdlet met een verhoogde Windows PowerShell-prompt uitvoeren:

        Add-AzureAccount
2. In het aanmeldingsscherm dat wordt weergegeven, moet u zich aanmelden bij uw account.

3. Een lijst met uw abonnementen ophalen.

        Get-AzureSubscription
4. Selecteer het abonnement dat u wilt gebruiken.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Maken en een circuitlijnen ExpressRoute inrichten

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. de PowerShell-modules voor ExpressRoute importeren

 Als u dit nog niet hebt gedaan, moet u de modules Azure en ExpressRoute importeren in de PowerShell-sessie om te beginnen met het gebruik van de cmdlets ExpressRoute. U kunt de modules importeren van de locatie waar ze zijn geïnstalleerd op uw lokale computer. Afhankelijk van de manier waarop u voor het installeren van de modules, de locatie mogelijk anders dan het volgende voorbeeld wordt getoond. Het voorbeeld desgewenst wijzigen.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. de lijst met ondersteunde providers, locaties en bandbreedte ophalen

Voordat u een circuitlijnen ExpressRoute hebt gemaakt, moet u de lijst met ondersteunde connectivity providers, locaties en bandbreedte opties.

De PowerShell-cmdlet `Get-AzureDedicatedCircuitServiceProvider` geeft deze informatie, die u in de volgende stappen gebruikt:

    Get-AzureDedicatedCircuitServiceProvider

Controleer of uw provider connectivity er wordt weergegeven. Maak een notitie van de volgende informatie omdat u deze later moet wanneer u een circuitlijnen maakt:

- Naam

- PeeringLocations

- BandwidthsOffered

U bent nu klaar om te ExpressRoute circuits maken.         

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute circuits maken

Het volgende voorbeeld ziet u hoe u een 200-Mbps ExpressRoute circuitlijnen tot en met Equinix maakt in Silicon Valley. Als u verschillende instellingen en een andere provider gebruikt, vervangt u die gegevens door wanneer u uw aanvraag kunt invullen.

>[AZURE.IMPORTANT] Uw circuitlijnen ExpressRoute wordt gefactureerd vanaf het moment dat een servicesleutel is uitgegeven. Zorg ervoor dat u deze bewerking uitvoeren wanneer de provider connectivity klaar is voor het inrichten van de circuitlijnen.


Hier volgt een voorbeeld-verzoek voor een nieuwe servicesleutel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Of, als u ExpressRoute circuits maken met de premium-invoegtoepassing wilt, gebruikt u het volgende voorbeeld. Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Het antwoord bevat de service-toets. U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. alle ExpressRoute circuits lijst

U kunt uitvoeren de `Get-AzureDedicatedCircuit` opdracht om een lijst met alle ExpressRoute circuits die u hebt gemaakt:


    Get-AzureDedicatedCircuit

Het antwoord worden iets vergelijkbaar met het volgende voorbeeld:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt deze informatie op elk gewenst moment kunt ophalen met behulp van de `Get-AzureDedicatedCircuit` cmdlet. De oproep zonder parameters worden alle circuits. Uw service-code worden, vermeld in het veld *ServiceKey* .

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. het verzenden van de service-toets naar uw connectivity-provider voor het inrichten van


*ServiceProviderProvisioningState* wordt aandacht besteed aan de huidige status van de inrichting van aan de service-provider. *Status* biedt de stand aan de zijkant van Microsoft. Zie het artikel [werkstromen](expressroute-workflows.md#expressroute-circuit-provisioning-states) voor meer informatie over circuitlijnen Staten inrichting.

Wanneer u een nieuwe ExpressRoute circuitlijnen maakt, wordt de circuitlijnen worden in de volgende staat:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


De circuitlijnen gaat naar de volgende status zodra de provider connectivity wordt ingeschakeld voor u:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Een circuitlijnen ExpressRoute moet in de volgende stand voor u kunnen gebruiken:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. de status en de status van de toets circuitlijnen regelmatig te controleren

Hiermee kunt u weten wanneer uw provider uw circuitlijnen heeft ingeschakeld. Nadat de circuitlijnen is geconfigureerd, *ServiceProviderProvisioningState* wordt weergegeven als *Provisioned* zoals wordt weergegeven in het volgende voorbeeld:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. de configuratie van uw routering maken

Verwijzen naar de [ExpressRoute circuit configuratie routering (maken en wijzigen van circuitlijnen peerings)](expressroute-howto-routing-classic.md) artikel voor stapsgewijze instructies.

>[AZURE.IMPORTANT] Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die laag 2 connectivity services bieden. Als u gebruikmaakt van een serviceprovider die beheerde layer 3 services (meestal een IP VPN, zoals MPLS), wordt uw provider connectiviteit configureren en beheren omleiding voor u.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. een virtueel netwerk koppelen aan een circuitlijnen ExpressRoute

Een virtueel netwerk vervolgens een koppeling naar uw circuitlijnen ExpressRoute. Raadpleeg [ExpressRoute koppelen circuits met virtuele netwerken](expressroute-howto-linkvnet-classic.md) voor stapsgewijze instructies. Als u maken van een virtueel netwerk wilt met behulp van het implementatiemodel klassieke voor ExpressRoute, raadpleegt u [maken een virtueel netwerk voor ExpressRoute](expressroute-howto-vnet-portal-classic.md) voor instructies.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>De status van een circuitlijnen ExpressRoute ophalen

U kunt deze informatie op elk gewenst moment kunt ophalen met behulp van de `Get-AzureCircuit` cmdlet. De oproep zonder parameters worden alle circuits.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

U kunt informatie over een specifieke ExpressRoute circuitlijnen openen door de service-toets als een parameter doorgeven aan het gesprek:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


U gaat gedetailleerde beschrijvingen van alle parameters door het volgende uit te voeren:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Een circuitlijnen ExpressRoute wijzigen

U kunt bepaalde eigenschappen van een circuitlijnen ExpressRoute wijzigen zonder die invloed hebben op connectivity.

U kunt het volgende met geen downtime doen:

- In- of uitschakelen van een ExpressRoute premium-invoegtoepassing voor uw circuitlijnen ExpressRoute.
- De bandbreedte van uw circuitlijnen ExpressRoute vergroten. Houd er rekening mee dat de bandbreedte van een circuitlijnen Downgrade niet wordt ondersteund.
- De meting abonnement wijzigen van gegevens met Datalimiet naar onbeperkt gegevens. Houd er rekening mee dat het abonnement waarop meting van onbeperkte gegevens wijzigen in gegevens met Datalimiet niet wordt ondersteund.
- U kunt in- en uitschakelen *Klassieke bewerkingen toestaan*.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie over beperkingen en tekortkomingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De ExpressRoute premium-invoegtoepassing inschakelen

U kunt de ExpressRoute premium-invoegtoepassing inschakelen voor uw bestaande circuitlijnen met behulp van de volgende PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Uw circuitlijnen hebben nu de ExpressRoute premium invoegtoepassing functies die zijn ingeschakeld. Houd er rekening mee dat we u voor de mogelijkheid van de invoegtoepassing premium facturering zodra de opdracht met succes is uitgevoerd wordt gestart.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De premium-invoegtoepassing ExpressRoute wilt uitschakelen

>[AZURE.IMPORTANT] Deze bewerking kan mislukken als u resources die ouder zijn dan wat is toegestaan voor de standaard circuitlijnen gebruikt.

Houd rekening met het volgende:

- Moet u ervoor zorgen dat het aantal virtuele netwerken die zijn gekoppeld aan de circuitlijnen minder dan 10 is voordat u uit premium naar standaard downgraden. Als u dit niet doet en u uw aanvraag bijwerken, mislukt de premietarieven gefactureerd.

- U kunt alle virtuele netwerken in andere geopolitieke regio's moet ontkoppelen. Als u dit niet doet en u uw aanvraag bijwerken, mislukt de premietarieven gefactureerd.

- Uw tabel moet minder dan 4000 routes voor privé peering. Als uw route tabel groter dan 4000 routes is, wordt de sessie BGP wordt neerzetten en won't worden ingeschakeld totdat het aantal aangekondigde voorvoegsels daaronder 4000.

U kunt de premium-invoegtoepassing ExpressRoute voor uw bestaande circuitlijnen uitschakelen met behulp van de volgende PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>De ExpressRoute circuitlijnen bandbreedte bijwerken

Controleer de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor ondersteunde bandbreedte opties voor uw provider. U kunt elke grootte die groter is dan de grootte van uw bestaande circuitlijnen zo lang maken als de fysieke poort (waarop uw circuitlijnen is gemaakt), kunt kiezen.

>[AZURE.IMPORTANT] U kunt de bandbreedte van een circuitlijnen ExpressRoute zonder verstoringen niet reduceren. Een downgrade uitvoeren bandbreedte, moet u de circuitlijnen ExpressRoute deprovision en klikt u vervolgens een nieuwe ExpressRoute circuitlijnen opnieuw wilt inrichten.

Nadat u hebt besloten welke grootte die u nodig hebt, kunt u de volgende opdracht uit om de grootte van uw circuitlijnen te gebruiken:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Uw circuitlijnen wordt is grootte omhoog bij Microsoft. U moet contact opnemen met uw provider connectivity als u wilt bijwerken configuraties aan hun kant zodat deze overeenkomt met deze wijziging. Houd er rekening mee dat we u facturering voor de optie bijgewerkte bandbreedte vanaf dit punt op wordt gestart.

Als u het volgende foutbericht weergegeven wanneer de bandbreedte circuitlijnen vergroten, betekent dit dat er is geen voldoende bandbreedte links op de fysieke poort waar uw bestaande circuitlijnen wordt gemaakt. U moet deze circuitlijnen verwijderen en maak een nieuwe circuitlijnen van de grootte die u nodig hebt. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning en verwijderen van een circuitlijnen ExpressRoute

Houd rekening met het volgende:

- U kunt alle virtuele netwerken uit de circuitlijnen ExpressRoute voor deze bewerking moet ontkoppelen. Controleer of er een virtuele netwerken die zijn gekoppeld aan de circuitlijnen als deze bewerking is mislukt.

- Als de ExpressRoute circuitlijnen provider inrichten servicestatus **Provisioning** of **Provisioned** moet u werken met uw provider naar de circuitlijnen aan hun kant deprovision. We blijft reserveren van resources en u rekening totdat de serviceprovider voltooit u de circuitlijnen deprovisioning en krijgt ons.

- Als de serviceprovider heeft de circuitlijnen (de status voor het inrichten van service-provider is ingesteld op **niet ingericht**) opgeheven kunt u de circuitlijnen vervolgens verwijderen. Hierdoor wordt de facturering voor de circuitlijnen gestopt.

U kunt uw circuitlijnen ExpressRoute verwijderen door de volgende opdracht uit te voeren:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuitlijnen hebt gemaakt, zorg dat u het volgende doen:

- [Maken en wijzigen omleiding voor uw circuitlijnen ExpressRoute](expressroute-howto-routing-classic.md)
- [Het virtuele netwerk koppelen aan uw circuitlijnen ExpressRoute](expressroute-howto-linkvnet-classic.md)
