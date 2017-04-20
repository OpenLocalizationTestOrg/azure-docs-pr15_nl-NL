<properties
   pageTitle="Een Internet die tegenover elkaar liggen taakverdeling in resourcemanager gebruik van de Azure CLI maken | Microsoft Azure"
   description="Informatie over het maken van een die tegenover elkaar liggen taakverdeling in resourcemanager de CLI Azure met Internet"
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

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Een interne taakverdeling met de CLI Azure maken

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook [informatie over het maken van een die tegenover elkaar liggen taakverdeling klassieke implementatie met Internet](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Het gebruik van de CLI Azure-oplossing implementeert

De volgende stappen hoe een Internet die tegenover elkaar liggen taakverdeling resourcemanager Azure gebruikt met CLI maken. Met Azure resourcemanager elke resource wordt gemaakt en geconfigureerd afzonderlijk, vervolgens opslag samen om een resource.

U moet maken en configureren van de volgende objecten als u wilt een taakverdeling implementeren:

- Front IP-configuratie - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Groep met back-enddatabase adressen - bevat netwerkinterfaces (NIC's) voor de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling.
- Regels van taakverdeling - een openbare poort op de taakverdeling toewijzen aan poort in de groep back-enddatabase adressen regels bevat.
- Binnenkomende verbindingen NAT - een openbare poort op de taakverdeling toewijzen aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen regels bevat.
- Controleert of-status sondes gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep back-enddatabase adressen bevat.

Zie [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-cli-to-use-resource-manager"></a>CLI instellen voor de Resource Manager gebruiken

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. Voer de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbare IP-adres van de front-IP-groep maken

1. Maak een virtueel netwerk (VNet) met de naam *NRPVnet* in de Oost Amerikaans locatie met een resourcegroep met de naam *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Een benoemde *NRPVnetSubnet* met een tekstblok CIDR 10.0.0.0/24 in *NRPVnet*subnet maken.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Een openbare IP-adres met de naam *NRPPublicIP* moet worden gebruikt door een front IP-toepassingen met de DNS-naam *loadbalancernrp.eastus.cloudapp.azure.com*maken. De opdracht hieronder wordt gebruikt voor het type van de statische toewijzing en inactiviteit van 4 minuten.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Verdeling van de belasting, het domeinlabel van het openbare IP-gebruikt als de FQDN. Dit een veranderd ten opzichte van klassieke implementatie, waarbij de cloud-service als de taakverdeling volledig domein naam (Fully Qualified Domain Name).
    >In dit voorbeeld is de FQDN-naam *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Een taakverdeling maken

De volgende opdracht maakt een taakverdeling met de naam *NRPlb* in de resourcegroep *NRPRG* in de *Oost Amerikaans* Azure locatie.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Een front IP-toepassingen en een back-end-mailadres-groep maken

Dit voorbeeld wordt gedemonstreerd hoe u de front-IP-toepassingen die u ontvangt van de binnenkomende netwerkverkeer op de taakverdeling en de backend IP-toepassingen waar de front-toepassingen het netwerkverkeer van taakverdeling stuurt maakt.

1. Een front IP-toepassingen koppelen aan het openbare IP gemaakt in de vorige stap en de taakverdeling te maken.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Het instellen van een back-enddatabase adres-toepassingen die wordt gebruikt voor het ontvangen van binnenkomende verkeer uit de front-IP-groep.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Regels voor kg, NAT regels en test maken

In dit voorbeeld wordt de volgende items gemaakt.

- een regel NAT vertalen van alle binnenkomende verkeer op poort 21 op poort 22<sup>1</sup>
- een regel NAT vertalen van alle binnenkomende verkeer op poort 23 met poort 22
- een regel van de verdeling van laden naar saldo vanaf alle binnenkomende verkeer op poort 80 op poort 80 van de adressen in de groep back-enddatabase.
- een regel test om te controleren van de status op een pagina met de naam *HealthProbe.aspx*.

<sup>1</sup> NAT regels zijn gekoppeld aan een specifieke virtuele machine exemplaar achter de taakverdeling. Het netwerkverkeer binnengekomen op poort 21 wordt verzonden naar een specifieke virtuele machine op poort 22 die is gekoppeld aan deze NAT-regel. U moet een protocol (UDP of TCP) opgeven voor een NAT-regel. Beide protocollen worden niet toegewezen aan dezelfde poort.

1. Maak de NAT-regels.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Maak een regel van de verdeling van laden.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Maak een systeemstatus-test.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Controleer de instellingen.

        azure network lb show nrprg nrplb

    Verwachte output:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>NIC's maken

U moet NIC maken (of bestaande wijzigen) en deze regels, regels voor de verdeling van laden en sondes NAT koppelen.

1. Maken van een NIC met de naam *kg nic1 worden*, en koppelen aan de *rdp1* NAT-regel en de groep van de back-enddatabase adressen *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Verwachte output:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Maken van een NIC met de naam *kg nic2 worden*, en koppelen aan de *rdp2* NAT-regel en de groep van de back-enddatabase adressen *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Een virtuele machine (VM) met de naam *web1*maken en koppelen aan de NIC met de naam *kg nic1 worden*. Een opslag-account genoemd *web1nrp* is gemaakt voordat u de opdracht hieronder.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in een taakverdeling moeten in dezelfde beschikbaarheid set. Gebruik `azure availset create` wilt maken van een beschikbaarheid instellen.

    De uitvoer moet er ongeveer als volgt te werk:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Het bericht dat **Dit is een NIC zonder publicIP geconfigureerd** naar verwachting sinds de NIC gemaakt voor de verdeling van de belasting verbinding maakt met Internet via het laden de verdeling van openbare IP-adres.

    Aangezien de *kg nic1 worden* NIC is gekoppeld aan de *rdp1* NAT-regel, u kunt verbinding maken met *web1* met RDP via poort 3441 van de taakverdeling.

4. Een virtuele machine (VM) met de naam *web2*maken en koppelen aan de NIC met de naam *kg nic2 worden*. Een opslag-account genoemd *web1nrp* is gemaakt voordat u de opdracht hieronder.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Een bestaande taakverdeling bijwerken

U kunt regels die verwijst naar een bestaande taakverdeling toevoegen. In het volgende voorbeeld wordt wordt een nieuwe regel voor laden-verdeling toegevoegd aan een bestaande taakverdeling **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Een taakverdeling verwijderen

Gebruik de volgende opdracht uit om te verwijderen van een taakverdeling:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
