<properties
    pageTitle="Een Internet die tegenover elkaar liggen taakverdeling met IPv6 via PowerShell voor resourcemanager maken | Microsoft Azure"
    description="Informatie over het maken van een internetverbinding van taakverdeling met IPv6 via PowerShell voor resourcemanager"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure taakverdeling, dubbele stapel, openbare IP-, systeemeigen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Maken van een internetverbinding van taakverdeling met IPv6 via PowerShell voor resourcemanager aan de slag

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Een Azure taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt beschikbaarheid door te distribueren binnenkomende verkeer tussen exemplaren van de orde service in de cloudservices of virtuele machines in een set van de verdeling van laden. Azure taakverdeling geeft soms ook deze services op meerdere poorten, meerdere IP-adressen of beide.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

In het volgende diagram ziet u de oplossing in dit artikel wordt geïmplementeerd van taakverdeling.

![Scenario voor de verdeling van laden](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende Azure bronnen:

- een taakverdeling internetgerichte met een IPv4- en een IPv6 openbare IP-adres
- twee laden taakverdeling regels voor de openbare VIP's toewijzen aan de privé eindpunten
- een beschikbaarheid instellen die aan bevat de twee VMs
- twee virtuele machines (VMs)
- een virtueel netwerk-interface voor elke VM met IPv4 en IPv6-adressen die zijn toegewezen

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Het gebruik van de Azure PowerShell-oplossing implementeert

De volgende stappen hoe een Internet die tegenover elkaar liggen taakverdeling resourcemanager Azure gebruikt met PowerShell maken. Met Azure resourcemanager elke resource wordt gemaakt en geconfigureerd afzonderlijk, vervolgens opslag samen om een resource.

Als u wilt een taakverdeling implementeert, die u kunt maken en configureren van de volgende objecten:

- Front IP-configuratie - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Groep met back-enddatabase adressen - bevat netwerkinterfaces (NIC's) voor de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling.
- Regels van taakverdeling - een openbare poort op de taakverdeling toewijzen aan poort in de groep back-enddatabase adressen regels bevat.
- Binnenkomende verbindingen NAT - een openbare poort op de taakverdeling toewijzen aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen regels bevat.
- Controleert of-status sondes gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep back-enddatabase adressen bevat.

Zie [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor de Resource Manager gebruiken

Zorg ervoor dat u hebt de meest recente versie van de Azure resourcemanager-module voor PowerShell.

1. Meld u aan bij Azure

        Login-AzureRmAccount

    Voer uw referenties in wanneer hierom wordt gevraagd.

2. De abonnementen voor het account controleren

        Get-AzureRmSubscription

3. Kies welke van uw Azure-abonnementen te gebruiken.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Een resourcegroep (overslaan deze stap als met een bestaande resourcegroep) maken

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbare IP-adres van de front-IP-groep maken

1. Maak een virtueel netwerk met een subnet.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Azure openbare IP-adres (PIP) resources voor de front-IP-adres van toepassingen maken.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Verdeling van de belasting wordt het domeinlabel van het openbare IP-als voorvoegsel voor de FQDN gebruikt. In dit voorbeeld zijn de FQDN-namen *lbnrpipv4.westus.cloudapp.azure.com* en *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Een Front-End IP-configuraties en een Back-End-mailadres-groep maken

1. Maak van de front-adresconfiguratie met de openbare IP-adressen die u hebt gemaakt.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Het maken van back-enddatabase adres pools.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>KG regels, NAT regels een test en een taakverdeling maken

In dit voorbeeld wordt de volgende items:

- een regel NAT alle binnenkomende verkeer op poort 443 met poort 4443 vertalen
- een regel van de verdeling van laden naar saldo vanaf alle binnenkomende verkeer op poort 80 op poort 80 van de adressen in de groep back-enddatabase.
- een regel van de verdeling van laden toe te staan dat RDP-verbinding met de VMs op poort 3389.
- een regel test om te controleren van de status op een pagina met de naam *HealthProbe.aspx* of een service op poort 8080
- een taakverdeling waarin alle deze objecten

1. Maak de NAT-regels.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Maak een systeemstatus-test. Er zijn twee manieren voor het configureren van een test:

    HTTP-test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    of TCP-test

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    In dit voorbeeld gaat we gebruiken de TCP-testpakketten.

3. Maak een regel van de verdeling van laden.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Maak de verdeling van de belasting met de eerder gemaakte objecten.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>NIC's maken voor de back-enddatabase VMs

1. Lees het virtuele netwerk en virtuele netwerk Subnet, waar de NIC's moeten worden gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. IP-configuraties en NIC's maken voor de VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Virtuele machines maken en toewijzen van de zojuist gemaakte NIC 's

Zie voor meer informatie over het maken van een VM [maken en u een virtuele Windows-computer met resourcemanager en Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Maak een account beschikbaarheid instellen en opslag

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Elke VM maken en toewijzen via de vorige NIC's die zijn gemaakt

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
