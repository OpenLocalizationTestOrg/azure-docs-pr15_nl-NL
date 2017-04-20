<properties
   pageTitle="Een taakverdeling internetgerichte in resourcemanager maken via PowerShell | Microsoft Azure"
   description="Informatie over het maken van een verdeling van de belasting internetgerichte in resourcemanager via PowerShell"
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

# <a name="get-started"></a>Maken van een verdeling van de belasting internetgerichte in resourcemanager via PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook [informatie over het maken van een verdeling van de belasting internetgerichte met behulp van het implementatiemodel klassieke](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>De oplossing implementeert met behulp van Azure PowerShell

De volgende procedures wordt uitgelegd hoe u een internetverbinding taakverdeling maken met behulp van Azure resourcemanager met PowerShell. Met Azure resourcemanager elke resource is gemaakt en geconfigureerd afzonderlijk en deze vervolgens samen aan een taakverdeling maken.

U moet maken en configureren van de volgende objecten als u wilt een taakverdeling implementeren:

- Front IP-configuratie: openbare IP-(PIP)-adressen voor binnenkomende netwerkverkeer bevat.
- Groep met back-enddatabase adressen: netwerkinterfaces (NIC's) voor de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling bevat.
- Regels van taakverdeling: regels bevat die een openbare poort op de taakverdeling aan een poort in de groep back-enddatabase adressen toewijzen.
- Binnenkomende NAT regels: regels bevat die een openbare poort op de taakverdeling aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen toewijzen.
- Sondes: status sondes gebruikt om te controleren van de beschikbaarheid van VM exemplaren in de groep back-enddatabase adressen bevat.

Zie [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor de Resource Manager gebruiken

Controleer of dat u hebt de meest recente versie van de Azure resourcemanager-module voor PowerShell:

1. Meld u aan bij Azure.

        Login-AzureRmAccount

    Voer uw referenties in wanneer hierom wordt gevraagd.

2. Controleer de abonnementen voor het account.

        Get-AzureRmSubscription

3. Kies welke van uw Azure-abonnementen te gebruiken.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Een resourcegroep maken. (Deze stap overslaan als u een bestaande resourcegroep gebruikt).

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbare IP-adres van de front-IP-groep maken

1. Een subnet en een virtueel netwerk maken.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Maak een Azure openbare IP-adres resource, met de naam **PublicIP**, moet worden gebruikt door een front IP-toepassingen met de DNS-naam **loadbalancernrp.westus.cloudapp.azure.com**. De volgende opdracht wordt gebruikt voor het type statische toewijzing.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Verdeling van de belasting wordt het domeinlabel van het openbare IP-als een voorvoegsel voor de FQDN gebruikt. Dit verschilt van het implementatiemodel klassieke, die de cloudservice als de taakverdeling FQDN-naam gebruikt.
    >In dit voorbeeld is de FQDN-naam **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Een front IP-toepassingen en een back-enddatabase adresgroep maken

1. Maak een front IP-toepassingen met de naam **Kg-Frontend** die gebruikmaakt van de resource **PublicIp** .

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Maak een back-enddatabase adres-toepassingen met de naam **kg backend**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT regels, een regel van de verdeling van laden, een test en een taakverdeling maken

In dit voorbeeld wordt de volgende items:

- Een regel NAT alle binnenkomende verkeer op poort 3441 poort 3389 vertalen
- Een regel NAT alle binnenkomende verkeer op poort 3442 poort 3389 vertalen
- Een regel test om te controleren van de status op een pagina met de naam **HealthProbe.aspx**
- Een regel van de verdeling van laden naar saldo vanaf alle binnenkomende verkeer op poort 80 op poort 80 van de adressen in de groep back-enddatabase
- Een taakverdeling waarin alle deze objecten

Voer deze stappen uit:

1. Maak de NAT-regels.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Maak een systeemstatus-test. Er zijn twee manieren voor het configureren van een test:

    HTTP-test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP-test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Maak een regel van de verdeling van laden.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Maak de taakverdeling met behulp van de eerder gemaakte objecten.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>NIC's maken

Netwerkinterfaces maken (of bestaande wijzigen) en deze vervolgens NAT regels, regels voor de verdeling van laden en sondes te koppelen:

1. Krijg het virtuele netwerk en een virtueel netwerk subnet, waar de NIC's moeten worden gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Maken van een NIC met de naam **kg nic1 worden**, en koppelen aan de groep eerste (en alleen) back-enddatabase-adressen en de eerste NAT-regel.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Maken van een NIC met de naam **kg nic2 worden**, en koppelen aan de tweede NAT regel en de eerste (en alleen) back-enddatabase-adres van toepassingen.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Controleer de NIC's.

        $backendnic1

    Verwachte output:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Gebruik de `Add-AzureRmVMNetworkInterface` cmdlet de NIC's toewijzen aan andere VMs.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Voor hulp bij het maken van een virtuele machine en het toewijzen van een NIC, Zie [maken een VM Azure via PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>De netwerkinterface toevoegen aan de taakverdeling

1. Verdeling van de belasting van Azure ophalen.

    De resource van de verdeling van laden in een variabele laden (als u die nog niet hebt gedaan). De variabele heet **$lb**. Gebruik de dezelfde de namen van de belasting voor gelijkmatige verdeling resource die u eerder hebt gemaakt.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. De configuratie van de back-enddatabase naar een variabele laden.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. De netwerkinterface reeds gemaakte laden in een variabele. Naam van de variabele is **$nic**. De naam van het netwerk-interface is de dezelfde uit het eerdere voorbeeld.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. De configuratie van de back-enddatabase op het netwerkinterface wijzigen.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Sla het netwerk interface-object.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Nadat een netwerkinterface wordt toegevoegd aan de groep laden de verdeling van back-enddatabase, begint ontvangen netwerkverkeer op basis van de regels taakverdeling voor de desbetreffende resource van de verdeling van laden.

## <a name="update-an-existing-load-balancer"></a>Een bestaande taakverdeling bijwerken

1. Met behulp van de verdeling van de belasting van het eerdere voorbeeld, een laden de verdeling van object toewijzen aan de variabele **$slb** met behulp van `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. In het volgende voorbeeld, kunt u een binnenkomende NAT-regel--toevoegen met behulp van poort 81 in de front-toepassingen en poort 8181 voor de back-enddatabase van toepassingen--aan een bestaande taakverdeling.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. De nieuwe configuratie opslaan met behulp van `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Een taakverdeling verwijderen

Gebruik de opdracht `Remove-AzureLoadBalancer` verwijderen van een eerder gemaakte taakverdeling met de naam **NRP kg** in een resourcegroep genoemd **NRP-RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] U kunt de optionele schakeloptie **-dwingen** om te voorkomen dat de prompt voor het verwijderen.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
