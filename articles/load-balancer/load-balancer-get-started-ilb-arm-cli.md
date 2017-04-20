<properties
   pageTitle="Een interne taakverdeling maken met behulp van de Azure CLI in resourcemanager | Microsoft Azure"
   description="Meer informatie over het maken van een interne taakverdeling met behulp van de Azure CLI in resourcemanager"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Een interne taakverdeling maken met behulp van de Azure CLI

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>De oplossing met behulp van de Azure CLI implementeren

De volgende stappen hoe een taakverdeling internetgerichte maken met behulp van Azure resourcemanager met CLI. Met Azure resourcemanager elke resource is gemaakt en geconfigureerd afzonderlijk en deze vervolgens samen aan een resource maakt.

U moet maken en configureren van de volgende objecten als u wilt een taakverdeling implementeren:

- **Front-end IP-configuratie**: openbare IP-adressen voor binnenkomende netwerkverkeer bevat
- **Groep met back-enddatabase adressen**: netwerkinterfaces (NIC) waarmee de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling bevat
- **Regels van taakverdeling**: regels bevat die een openbare poort op de taakverdeling aan poort in de groep back-enddatabase adressen toewijzen
- **Regels voor binnenkomende verbindingen NAT**: regels bevat die een openbare poort op de taakverdeling aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen toewijzen
- **Controleert of**: status sondes die worden gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep back-enddatabase adressen bevat

Zie [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-cli-to-use-resource-manager"></a>CLI instellen voor de Resource Manager gebruiken

1. Als u nooit Azure CLI gebruikt nog, raadpleegt u [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md). Volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. Hiermee voert u de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, als volgt:

        azure config mode arm

    Verwachte output:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Maak een interne taakverdeling stap voor stap

1. Meld u aan bij Azure.

        azure login

    Wanneer u wordt gevraagd, voert u uw Azure referenties.

2. Wijzig de opdracht hulpmiddelen voor resourcemanager Azure-modus.

        azure config mode arm

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Alle resources in Azure resourcemanager zijn gekoppeld aan een resourcegroep. Als u dit nog niet hebt gedaan nog een resourcegroep maken.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Een set van de verdeling van interne laden maken

1. Een interne taakverdeling maken

    In de volgende scenario, een resourcegroep met de naam nrprg gemaakt in Oost-Amerikaanse regio.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Alle bronnen voor een interne netwerktaakverdelers, zoals virtuele netwerken en virtueel netwerksubnetten, moet in dezelfde resourcegroep en in dezelfde regio.

2. Maak een front IP-adres van de interne taakverdeling.

    Het IP-adres dat u gebruikt, moeten in het subnetbereik van uw virtuele netwerk.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. De groep back-enddatabase adressen maken.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Nadat u een front IP-adres en een back-enddatabase adres resourcegroep definieert, kunt u laden de verdeling van regels voor binnenkomende NAT regels maken en aangepaste status controleert.

4. Een regel van de verdeling van belasting voor de interne taakverdeling maken.

    Wanneer u de vorige stappen hebt gevolgd, maakt de opdracht u een taakverdeling regel voor het luisteren poort 1433 van de front-toepassingen en poort 1433 netwerkverkeer verdeeld naar de groep back-enddatabase adressen verzendt, ook worden gebruikt.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Maak regels voor binnenkomende NAT.

    Binnenkomende NAT-regels worden gebruikt voor het maken van de eindpunten in een taakverdeling die gaat u naar een specifieke virtuele machine exemplaar. De vorige stappen gemaakt twee NAT regels voor extern bureaublad.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Servicestatus sondes voor de taakverdeling maken.

    Een test systeemstatus controles overal VM om ervoor te zorgen dat ze kunnen netwerkverkeer sturen. Het exemplaar VM met mislukte test controles is verwijderd uit de taakverdeling totdat u weer online gaat, kunt u dit en een selectievakje test bepaalt dat dit correct is.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]De Microsoft Azure-platform gebruikt een statische, openbaar geschikt IPv4-adres voor een groot aantal administratieve scenario's. Het IP-adres is 168.63.129.16. Dit IP-adres moet niet worden geblokkeerd door een firewalls, omdat dit onverwachte gevolgen dit kan.
    >Met betrekking tot Azure interne taakverdeling, wordt dit IP-adres door te controleren sondes uit de taakverdeling gebruikt om te bepalen de status voor virtuele machines in een set verdeeld. Als een netwerk-beveiligingsgroep wordt gebruikt om te verkeer beperken tot Azure virtuele machines in een set intern taakverdeling of wordt toegepast op een virtueel netwerk subnet, moet u ervoor zorgen dat een regel voor netwerk is toegevoegd aan dat verkeer is toegestaan uit 168.63.129.16.

## <a name="create-nics"></a>NIC's maken

U moet NIC maken (of bestaande wijzigen) en deze regels, regels voor de verdeling van laden en sondes NAT koppelen.

1. Maken van een NIC met de naam *kg nic1 worden*, en vervolgens met de *rdp1* NAT-regel en de groep van de back-enddatabase adressen *beilb* te koppelen.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Maken van een NIC met de naam *kg nic2 worden*, en vervolgens met de *rdp2* NAT-regel en de groep van de back-enddatabase adressen *beilb* te koppelen.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Maak een virtuele machine *BW1*met de naam en vervolgens te koppelen aan de NIC met de naam *kg nic1 worden*. Een opslag-account genoemd *web1nrp* is gemaakt voordat de volgende opdracht wordt uitgevoerd:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in een taakverdeling moeten in dezelfde beschikbaarheid set. Gebruik `azure availset create` wilt maken van een beschikbaarheid instellen.

4. Een virtuele machine (VM) met de naam *DB2*maken en vervolgens te koppelen aan de NIC met de naam *kg nic2 worden*. Een opslag-account genoemd *web1nrp* is gemaakt voordat u de volgende opdracht uit.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Een taakverdeling verwijderen

Als u wilt een taakverdeling verwijderen, gebruikt u de volgende opdracht uit:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Volgende stappen

[Een modus voor het laden gelijkmatige verdeling met behulp van de bron-IP-affiniteit configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
