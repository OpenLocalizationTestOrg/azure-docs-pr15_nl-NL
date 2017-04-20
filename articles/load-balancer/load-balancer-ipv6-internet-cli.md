<properties
    pageTitle="Een Internet die tegenover elkaar liggen taakverdeling met IPv6 in Azure resourcemanager gebruik van de Azure CLI maken | Microsoft Azure"
    description="Informatie over het maken van een die tegenover elkaar liggen taakverdeling met IPv6 in Azure resourcemanager de CLI Azure met Internet"
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

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Een die tegenover elkaar liggen taakverdeling met IPv6 in Azure resourcemanager de CLI Azure met Internet maken

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Een Azure taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt beschikbaarheid door te distribueren binnenkomende verkeer tussen exemplaren van de orde service in de cloudservices of virtuele machines in een set van de verdeling van laden. Azure taakverdeling geeft soms ook deze services op meerdere poorten, meerdere IP-adressen of beide.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

In het volgende diagram ziet u de oplossing van taakverdeling wordt geïmplementeerd met de voorbeeldsjabloon in dit artikel beschreven.

![Scenario voor de verdeling van laden](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

In dit scenario maakt u de volgende Azure bronnen:

- twee virtuele machines (VMs)
- een virtueel netwerk-interface voor elke VM met IPv4 en IPv6-adressen die zijn toegewezen
- een taakverdeling internetgerichte met een IPv4- en een IPv6 openbare IP-adres
- een beschikbaarheid instellen die aan bevat de twee VMs
- twee laden taakverdeling regels voor de openbare VIP's toewijzen aan de privé eindpunten

## <a name="deploying-the-solution-using-the-azure-cli"></a>Het gebruik van de CLI Azure-oplossing implementeert

De volgende stappen hoe een Internet die tegenover elkaar liggen taakverdeling resourcemanager Azure gebruikt met CLI maken. Met Azure resourcemanager elke resource is gemaakt en geconfigureerd afzonderlijk en deze vervolgens samen aan een resource maakt.

Als u wilt een taakverdeling implementeert, die u kunt maken en configureren van de volgende objecten:

- Front IP-configuratie - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Groep met back-enddatabase adressen - bevat netwerkinterfaces (NIC's) voor de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling.
- Regels van taakverdeling - een openbare poort op de taakverdeling toewijzen aan poort in de groep back-enddatabase adressen regels bevat.
- Binnenkomende verbindingen NAT - een openbare poort op de taakverdeling toewijzen aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen regels bevat.
- Controleert of-status sondes gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep back-enddatabase adressen bevat.

Zie [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Uw omgeving CLI ingesteld voor gebruik van Azure resourcemanager

In dit voorbeeld zijn we de hulpmiddelen CLI uitgevoerd in een PowerShell-opdrachtvenster. We de Azure PowerShell-cmdlets niet gebruikt, maar we het uitvoeren van scripts mogelijkheden van de PowerShell gebruiken om de leesbaarheid en hergebruik te verbeteren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. De opdracht **azure config modus** overschakelen naar resourcemanager modus uitvoeren.

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

3. Meld u aan bij Azure en een lijst met abonnementen.

        azure login

    Voer uw Azure referenties wanneer u wordt gevraagd.

        azure account list

    Kies het abonnement dat u wilt gebruiken. Noteer de abonnements-Id voor de volgende stap.

4. PowerShell variabelen voor gebruik met de opdrachten CLI instellen.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Een resourcegroep, een taakverdeling, een virtueel netwerk en subnetten maken

1. Een resourcegroep maken

        azure group create $rgName $location

2. Een taakverdeling maken

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Maak een virtueel netwerk (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Maak twee subnetten in deze VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Openbare IP-adressen voor de front-groep maken

1. De PowerShell-variabelen instellen

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Maak een openbare IP-adres de front-IP-toepassingen.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Het domeinlabel van het openbare IP-verdeling van de belasting gebruikt als de FQDN. Dit een veranderd ten opzichte van klassieke implementatie, waarbij de cloudservice Geef een naam als de taakverdeling FQDN-naam.
    >In dit voorbeeld is de FQDN-naam *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Front-end en back-end-groepen maken

Dit voorbeeld wordt de front-IP-toepassingen die u ontvangt van de binnenkomende netwerkverkeer op de taakverdeling en de back-enddatabase IP-toepassingen waar de front-toepassingen het netwerkverkeer van taakverdeling stuurt.

1. De PowerShell-variabelen instellen

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Een front IP-toepassingen koppelen aan het openbare IP gemaakt in de vorige stap en de taakverdeling te maken.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>De test, NAT regels en kg regels maken

In dit voorbeeld wordt de volgende items:

- een regel test wilt controleren op connectiviteit met TCP-poort 80
- een regel NAT vertalen van alle binnenkomende verkeer op poort 3389 poort 3389 voor RDP-<sup>1</sup>
- een regel NAT vertalen van alle binnenkomende verkeer op poort 3391 poort 3389 voor RDP-<sup>1</sup>
- een regel van de verdeling van laden naar saldo vanaf alle binnenkomende verkeer op poort 80 op poort 80 van de adressen in de groep back-enddatabase.

<sup>1</sup> NAT regels zijn gekoppeld aan een specifieke virtuele machine exemplaar achter de taakverdeling. Het netwerkverkeer binnengekomen op poort 3389 wordt verzonden naar de specifieke virtuele machine en poort die is gekoppeld aan de NAT-regel. U moet een protocol (UDP of TCP) opgeven voor een NAT-regel. Beide protocollen worden niet toegewezen aan dezelfde poort.

1. De PowerShell-variabelen instellen

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. De test maken

    Het volgende voorbeeld wordt een TCP-test controleren voor connectiviteit met back-enddatabase TCP-poort 80 elke 15 seconden. Deze markeert u de back-enddatabase resource niet beschikbaar na twee opeenvolgende fouten.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Maak binnenkomende NAT-regels die RDP-verbindingen met de back-enddatabase bronnen toestaan

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Maak een taakverdeling regels die verkeer naar een andere back-enddatabase-poorten, afhankelijk van welke front-end ontvangen het verzoek verzenden

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Uw instellingen controleren

        azure network lb show --resource-group $rgName --name $lbName

    Verwachte output:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>NIC's maken

NIC's maken en deze regels, regels voor de verdeling van laden en sondes NAT koppelen.

1. De PowerShell-variabelen instellen

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Een NIC voor elke back-enddatabase maken en toevoegen van een IPv6-configuratie.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>De back-enddatabase VM resources maken en elke NIC wilt toevoegen

Als u wilt VMs hebt gemaakt, moet u een opslag-account hebt. Voor taakverdeling moeten de VMs lid zijn van een set beschikbaarheid. Zie voor meer informatie over het maken van VMs [maken een VM Azure via PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. De PowerShell-variabelen instellen

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Dit voorbeeld wordt de gebruikersnaam en wachtwoord voor de VMs als gewone tekst. Juiste Wees voorzichtig bij het gebruik van referenties in de wissen. Zie de cmdlet [Get-referentie](https://technet.microsoft.com/library/hh849815.aspx) voor een veiligere methode van het afhandelen van referenties in PowerShell.

2. De opslag-account en beschikbaarheid van de set maken

    U kunt een bestaand opslag-account wanneer u de VMs maakt. De volgende opdracht maakt een nieuw account voor de opslag.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Maak vervolgens de beschikbaarheid van de set.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. De virtuele machines maken met de bijbehorende NIC 's

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
