<properties
    pageTitle="Algemene netwerk PowerShell-opdrachten voor VMs | Microsoft Azure"
    description="Algemene PowerShell-opdrachten om u aan de slag voor het maken van een virtueel netwerk en de bijbehorende hulpbronnen voor VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Algemene netwerk Azure PowerShell-opdrachten voor het VMs

Als u een virtuele machine maken wilt, moet u een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) maken of te weten over een bestaand virtuele netwerk waarin de VM kan worden toegevoegd. Meestal wanneer u een VM maakt, moet u ook rekening houden met het maken van de resources die in dit artikel beschreven.

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="create-network-resources"></a>Netwerk resources maken

Taak | Opdracht 
-------------- | -------------------------
Subnet configuraties maken | $subnet1 = [Nieuw AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -naam "subnet_name" - AddressPrefix xx kunt oplossen. X.X.X/XX<BR>$subnet2 = nieuw AzureRmVirtualNetworkSubnetConfig-naam "subnet_name" - AddressPrefix xx kunt oplossen. X.X.X/XX<BR><BR>Een typische netwerk mogelijk een subnet voor een [internet die tegenover elkaar liggen taakverdeling](../load-balancer/load-balancer-internet-overview.md) en een afzonderlijk subnet voor een [interne taakverdeling](../load-balancer/load-balancer-internal-overview.md). |
Een virtueel netwerk maken | $vnet = [Nieuw AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -resource_group_name"naam"virtual_network_name"- ResourceGroupName"-locatie "naam_locatie" - AddressPrefix xx kunt oplossen. X.X.X/XX-Subnet $subnet1, $subnet2
Test voor een unieke domeinnaam | [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName 'domeinnaam'-locatie "naam_locatie"<BR><BR>Een DNS-naam voor een [openbare IP-resource](../virtual-network/virtual-network-ip-addresses-overview-arm.md)wordt gemaakt van een toewijzing voor domainname.location.cloudapp.azure.com omgezet in het openbare IP-adres in de Azure managed DNS-servers, kunt u opgeven. De naam mag alleen letters, cijfers en afbreekstreepjes. De eerste en laatste teken moet een letter of nummer en de domeinnaam moet uniek zijn binnen de Azure locatie. Als **de waarde True** wordt geretourneerd, is de naam van de voorgestelde uniek.
Een openbare IP-adres maken | $pip = [Nieuw AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -naam "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel 'domeinnaam'-locatie "naam_locatie" - AllocationMethod dynamisch<BR><BR>Het openbare IP-adres wordt gebruikt voor de domeinnaam die u eerder getest en worden gebruikt door de frontend configuratie van de taakverdeling.
Een frontend IP-configuratie maken | $frontendIP = [Nieuw AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -naam "frontend_ip_name" - PublicIpAddress $pip<BR><BR>De configuratie frontend bevat het openbare IP-adres die u eerder hebt gemaakt voor binnenkomende netwerkverkeer.
Een back-end-mailadres van toepassingen maken | $beAddressPool = [Nieuw AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -naam "backend_pool_name"<BR><BR>Interne adressen biedt voor de backend van de verdeling van de belasting geopend via een netwerkinterface.
Een test maken | $healthProbe = [Nieuw AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -naam "probe_name" - RequestPath 'HealthProbe.aspx'-Protocol http-poort 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Servicestatus sondes gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep backend adressen bevat.
Een regel van taakverdeling maken | $lbRule = [Nieuw AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -naam HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe onderzoeken-Protocol Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Regels die een openbare poort op de taakverdeling aan een poort in de groep backend adressen toewijzen bevat.
Een binnenkomende NAT-regel maken | $inboundNATRule = [Nieuw AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -naam "rule_name" - FrontendIpConfiguration $frontendIP-Protocol TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Regels voor een openbare poort op de taakverdeling toewijzen aan een poort voor een specifieke virtuele machine in de groep backend adressen bevat.
Een taakverdeling maken | $loadBalancer = "resource_group_name" [Nieuw-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-naam "load_balancer_name"-locatie "naam_locatie" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-$healthProbe zoeken
Een netwerkinterface voor maken | $nic1 = "resource_group_name" [Nieuw-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-naam "network_interface_name"-locatie "naam_locatie" - PrivateIpAddress xx kunt oplossen. X.X.X-Subnet subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Maak een netwerkinterface met het openbare IP-adres en virtuele netwerk-subnetten die u eerder hebt gemaakt.
    
## <a name="get-information-about-network-resources"></a>Informatie over netwerk resources

Taak | Opdracht 
-------------- | -------------------------
Lijst met virtuele netwerken | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Bevat alle virtuele netwerken in de resourcegroep.
Informatie over een virtueel netwerk | Get-AzureRmVirtualNetwork-resource_group_name"naam"virtual_network_name"- ResourceGroupName"
Lijst-subnetten in een virtueel netwerk | Get-AzureRmVirtualNetwork-Geef de naam "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Selecteer subnetten
Informatie over een subnet | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -naam "subnet_name" - VirtualNetwork $vnet<BR><BR>Informatie over het subnet ontvangt in het opgegeven virtuele netwerk. De waarde $vnet staat voor het object van Get-AzureRmVirtualNetwork.
Lijst met IP-adressen | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Het openbare IP-adressen in de resourcegroep bevat.
Lijst netwerktaakverdelers | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Een lijst met alle netwerktaakverdelers in de resourcegroep.
Lijst netwerkinterfaces | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Dit zijn alle netwerkinterfaces in de resourcegroep.
Informatie over een netwerkinterface | Get-AzureRmNetworkInterface-resource_group_name"naam"network_interface_name"- ResourceGroupName"<BR><BR>Informatie over een bepaalde netwerkinterface krijgt.
De IP-configuratie van een netwerkinterface ophalen | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -naam "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Informatie over de IP-configuratie van de opgegeven netwerkinterface krijgt. De waarde $nic staat voor het object van Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Netwerk resources beheren

Taak | Opdracht 
-------------- | -------------------------
Een subnet toevoegen aan een virtueel netwerk | [Toevoegen AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix xx kunt oplossen. X.X.X/XX-naam "subnet_name" - VirtualNetwork $vnet<BR><BR>Hiermee voegt u een subnet toe aan een bestaand virtuele netwerk. De waarde $vnet staat voor het object van Get-AzureRmVirtualNetwork.
Een virtueel netwerk verwijderen | [Verwijderen-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -resource_group_name"naam"virtual_network_name"- ResourceGroupName"<BR><BR>Hiermee verwijdert het opgegeven virtuele netwerk uit de resourcegroep.
De netwerkinterface van een verwijderen | [Verwijderen-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -resource_group_name"naam"network_interface_name"- ResourceGroupName"<BR><BR>Hiermee verwijdert de opgegeven netwerkinterface uit de resourcegroep.
Een taakverdeling verwijderen | [Verwijderen-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -resource_group_name"naam"load_balancer_name"- ResourceGroupName"<BR><BR>Hiermee verwijdert de opgegeven taakverdeling uit de resourcegroep.
Een openbare IP-adres verwijderen | [Verwijderen-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-resource_group_name"naam"ip_address_name"- ResourceGroupName"<BR><BR>Hiermee verwijdert het opgegeven openbare IP-adres uit de resourcegroep.

## <a name="next-steps"></a>Volgende stappen

- Gebruik de netwerkinterface dat u zojuist hebt gemaakt wanneer u [een VM maken](virtual-machines-windows-ps-create.md).
- Meer informatie over hoe u [een VM met meerdere netwerkinterfaces maken kunt](../virtual-network/virtual-networks-multiple-nics.md).
