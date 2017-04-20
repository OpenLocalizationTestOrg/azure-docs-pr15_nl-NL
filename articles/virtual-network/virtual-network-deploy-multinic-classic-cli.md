<properties
   pageTitle="Meerdere NIC VMs de CLI Azure gebruiken in het implementatiemodel klassieke implementeren | Microsoft Azure"
   description="Informatie over het gebruik van de Azure CLI in het implementatiemodel klassieke van meerdere NIC VMs implementeren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Meerdere NIC VMs (klassieke) gebruik van de Azure CLI implementeren

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt maken van virtuele machines (VMs) in Azure wordt aangegeven en meerdere netwerkinterfaces (NIC's) toevoegen aan elk van uw VMs. Meerdere NIC inschakelen scheiding van typen verkeer via NIC's. Eén NIC misschien bijvoorbeeld communiceren met Internet, terwijl een andere alleen met interne bronnen niet verbonden met Internet communiceert. De mogelijkheid om te scheiden van netwerkverkeer over meerdere NIC is vereist voor veel virtuele netwerkapparaten, zoals de bezorging van de toepassing en WAN-optimalisatieoplossingen.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

U kunt op dit moment VMs met een enkel NIC en VMs met meerdere NIC geen in de dezelfde cloudservice. Daarom moet u de back-end-servers implementeren in een andere cloudservice dan en alle andere onderdelen in het scenario. De onderstaande stappen gebruiken een cloudservice *IaaSStory* voor de belangrijkste resources en *IaaSStory-BackEnd* benoemde voor de back-end-servers.

## <a name="prerequisites"></a>Vereisten voor

Voordat u de back-end-servers implementeren kunt, moet u de belangrijkste cloudservice met de benodigde resources voor dit scenario implementeren. Ten minste moet u een virtueel netwerk maken met een subnet voor de backend. Ga naar [een virtueel netwerk met behulp van de Azure CLI maken](virtual-networks-create-vnet-classic-cli.md) om informatie over het implementeren van een virtueel netwerk.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>De back-end VMs implementeren

De backend-VMs, hangt af van het maken van de onderstaande resources.

- **Opslag-account voor gegevensschijven**. Voor betere prestaties, wordt de gegevensschijven op de databaseservers effen staat station (SSD) technologie, die is vereist een premium-opslag-account gebruikt. Zorg ervoor dat de Azure locatie die u ter ondersteuning van premium opslag implementeren.
- **NIC's**. Elke VM heeft twee NIC's, één telefoonnummer voor toegang tot de database, en een voor projectmanagement.
- **Beschikbaarheid instellen**. Op alle databaseservers worden, toegevoegd aan een enkele beschikbaarheid instellen, om ervoor te zorgen ten minste één van de VMs omhoog en uit te voeren tijdens onderhoud.

### <a name="step-1---start-your-script"></a>Stap 1: beginnen uw script

U kunt de volledige we vaker doen script gebruikt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Volg de onderstaande stappen voor het wijzigen van het script om te werken in uw omgeving.

1. Wijzig de waarden van de variabelen op basis van uw bestaande resourcegroep geïmplementeerd boven in de [vereisten](#Prerequisites).

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor uw back-end-implementatie.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: nodig resources voor uw VMs maken

1. Maak een nieuwe cloudservice voor alle backend VMs. Let op het gebruik van de `$backendCSName` variabele voor de naam van de resource, en `$location` voor een Azure gebied.

        azure service create --serviceName $backendCSName \
            --location $location

2. Maak een account premium opslag van de schijven OS en gegevens worden gebruikt door vergeten VMs.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Stap 3: VMs met meerdere NIC's maken

1. Start een lus als u wilt maken van meerdere VMs, op basis van de `numberOfVMs` variabelen.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Voor elke VM, geef de naam en het IP-adres van elk van de twee netwerkadapters.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Maak de VM. Zoals u ziet het gebruik van de `--nic-config` parameter, met een lijst met alle NIC's met naam, subnet en IP-adres.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Maak voor elke VM, twee gegevensschijven.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Stap 4 - het script uitvoeren

Nu dat u hebt gedownload en gewijzigd van het script op basis van uw behoeften voldoet, het script uitvoeren om de back-end database VMs met meerdere NIC's maken.

1. Sla uw script en vanaf uw computer **Bash** worden uitgevoerd. Ziet u de eerste uitvoer, zoals hieronder wordt weergegeven.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Na een paar minuten de uitvoering moet worden beëindigd en u kunt de rest van de uitvoer wilt zien, zoals hieronder wordt weergegeven.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
