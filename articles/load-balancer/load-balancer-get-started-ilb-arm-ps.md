<properties
   pageTitle="Maken van een interne taakverdeling via PowerShell in resourcemanager | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling via PowerShell in resourcemanager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Een interne taakverdeling via PowerShell maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


De volgende stappen wordt uitgelegd hoe een interne taakverdeling resourcemanager Azure gebruikt met PowerShell maken. Met Azure resourcemanager de items die u maakt een interne taakverdeling zijn geconfigureerd afzonderlijk en vervolgens worden gecombineerd om te maken van een taakverdeling.

U moet maken en configureren van de volgende objecten als u wilt een taakverdeling implementeren:

- Einde IP-configuratie front - wordt het persoonlijke IP-adres voor binnenkomende netwerkverkeer configureren
- Groep van de adressen backend - wordt het netwerkinterfaces waaraan de die afkomstig zijn uit de groep IP-front-end van taakverdeling-verkeer configureren
- Regels - bron- en configuratie van de lokale poort voor de verdeling van de belasting voor taakverdeling.
- Controleert of-de systeemstatus status-test voor de exemplaren VM configureert.
- Binnenkomende verbindingen NAT - Hiermee configureert u de poortregels rechtstreeks toegang tot een van de exemplaren VM.

U kunt meer informatie over het laden krijgen de verdeling van onderdelen met Azure resourcemanager op [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md).

De volgende stappen wordt uitgelegd hoe u een taakverdeling tussen twee virtuele machines configureert.


## <a name="setup-powershell-to-use-resource-manager"></a>Setup PowerShell resourcemanager gebruiken

Controleer of de meest recente versie van de Azure-module voor PowerShell, en dat u hebt PowerShell-instelling correct voor toegang tot uw Azure-abonnement.

### <a name="step-1"></a>Stap 1

        Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

De abonnementen voor het account controleren

        Get-AzureRmSubscription

U wordt gevraagd om te verifiëren met uw referenties.<BR>

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Resourcegroep maken voor de verdeling van belasting

### <a name="step-4"></a>Stap 4

Een nieuwe resourcegroep (overslaan deze stap als met een bestaande resourcegroep) maken

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Dit wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Controleer of alle opdrachten voor het maken van een taakverdeling dezelfde resourcegroep wordt gebruikt.

We hebben gemaakt een resourcegroep 'NRP-RG' en de locatie 'West ons' genoemd in het bovenstaande voorbeeld.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Virtuele netwerk en een persoonlijk IP-adres voor de front-end van IP-groep maken


### <a name="step-1"></a>Stap 1

Hiermee maakt u een subnet voor het virtuele netwerk en toegewezen aan variabele $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Maak een virtueel netwerk:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Hiermee maakt u het virtuele netwerk en het subnet kg subnet worden toegevoegd aan het virtuele netwerk NRPVNet wordt toegewezen aan variabele $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Front-endpool IP- en groep backend-adressen maken

Gelijkmatig verkeer een front-endpool IP-instellen voor de binnenkomende laden de verdeling van netwerk verkeer en backend-mailadres van toepassingen voor het ontvangen van het selectievakje laden.

### <a name="step-1"></a>Stap 1

Een front-end IP-toepassingen met behulp van het persoonlijke IP-adres 10.0.2.5 voor de subnet 10.0.2.0/24 die het eindpunt van het binnenkomende verkeer te maken.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>stap 2

Een back-end-adres-toepassingen die wordt gebruikt voor het ontvangen van binnenkomende verkeer uit de groep IP-front-end instellen:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>KG, NAT regels, test maken en de belasting voor documentconversies

Na het maken van de front-endpool IP- en de groep backend-adressen, moet u de regels die tot de resource van de verdeling van laden behoren maken:

### <a name="step-1"></a>Stap 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Met het bovenstaande voorbeeld wordt de volgende items maken:

- NAT-regel waarin alle inkomende verkeer naar poort 3441 worden doorgestuurd naar poort 3389.
- een tweede NAT regel waarin alle inkomende verkeer naar poort 3442 worden doorgestuurd naar poort 3389.
- een regel voor laden verdeling die wordt geladen saldo vanaf alle binnenkomende verkeer op openbare poort 80 naar lokale poort 80 in de groep back-end-adressen.
- een test-regel waarin de status voor het pad 'HealthProbe.aspx' moeten worden gecontroleerd



### <a name="step-2"></a>Stap 2

Maak de verdeling van de belasting samen alle objecten (NAT, laden de verdeling van regels, test configuraties) toe te voegen:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Netwerkinterfaces maken

Na het maken van de interne taakverdeling, moet u definiëren welke netwerkinterfaces ontvangt de binnenkomende netwerkverkeer van taakverdeling, NAT regels en test. De netwerkinterface wordt in dit geval afzonderlijk is geconfigureerd en kan worden toegewezen aan een virtuele machine later.


### <a name="step-1"></a>Stap 1


Krijg de resource virtueel netwerk en subnet, netwerkinterfaces maken:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Deze stap maakt een netwerkinterface waarmee u deel uitmaakt van de belasting voor gelijkmatige verdeling back-end-toepassingen en de eerste regel van de NAT voor RDP koppelen voor de netwerkinterface van dit:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Stap 2

Maak een tweede netwerkinterface naam kg Nic2 worden:

Deze stap maakt u een tweede netwerkinterface, toewijst aan de dezelfde laden de verdeling van back-end-toepassingen en het koppelen van de tweede NAT regel gemaakt voor RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Het eindresultaat, wordt het volgende weergegeven:

    $backendnic1

Verwachte output:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Stap 3

Gebruik de opdracht toevoegen-AzureRmVMNetworkInterface de NIC toewijzen aan een virtuele Machine.

U vindt de stapsgewijze instructies voor een virtuele machine maken en toewijzen aan een NIC volgen de documentatie: [maken een VM Azure via PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Als u al een virtuele machine hebt gemaakt, kunt u de netwerkinterface met de volgende stappen kunt toevoegen:

#### <a name="step-1"></a>Stap 1

De resource van de verdeling van laden in een variabele laden (als u die nog niet hebt gedaan). De variabele heet $lb en gebruikt u dezelfde namen van de belasting voor gelijkmatige verdeling resource hiervoor hebt gemaakt.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Stap 2

De backend-configuratie naar een variabele laden.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Stap 3

De netwerkinterface reeds gemaakte laden in een variabele. de naam van de variabele gebruikt is $NIC De naam van het netwerk-interface gebruikt is dezelfde van het bovenstaande voorbeeld.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Stap 4

Wijzig de backend-configuratie op de netwerkinterface.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Stap 5

Sla het netwerk interface-object.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Nadat een netwerkinterface wordt toegevoegd aan de belasting voor gelijkmatige verdeling backend-toepassingen, begint het netwerkverkeer op basis van de regels voor de desbetreffende resource van de verdeling van laden van taakverdeling ontvangen.

## <a name="update-an-existing-load-balancer"></a>Een bestaande taakverdeling bijwerken


### <a name="step-1"></a>Stap 1

De verdeling van de belasting van het bovenstaande voorbeeld gebruikt, laden de verdeling van object toewijzen aan variabele $slb Get-AzureRmLoadBalancer gebruiken

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Stap 2

In het volgende voorbeeld, wordt u een nieuwe regel voor NAT inkomende poort 81 in de front-end en poort 8181 gebruiken voor de back-end-toepassingen naar een bestaande taakverdeling toevoegen

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Stap 3

Sla de nieuwe configuratie Set-AzureLoadBalancer gebruiken

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Een taakverdeling verwijderen

De opdracht verwijderen-AzureRmLoadBalancer gebruiken om te verwijderen van een eerder gemaakte taakverdeling met de naam 'NRP kg"in een resourcegroep 'NRP-RG' genoemd

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] U kunt de optionele schakelen - dwingen om te voorkomen dat de prompt voor het verwijderen.



## <a name="next-steps"></a>Volgende stappen

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)