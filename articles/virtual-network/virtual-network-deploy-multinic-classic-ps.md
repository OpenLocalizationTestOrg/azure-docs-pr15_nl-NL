<properties
   pageTitle="Meerdere NIC VMs via PowerShell in het implementatiemodel klassieke implementeren | Microsoft Azure"
   description="Informatie over het implementeren van meerdere NIC VMs via PowerShell in het implementatiemodel klassieke"
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

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Meerdere NIC VMs (klassieke) via PowerShell implementeren

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt maken van virtuele machines (VMs) in Azure wordt aangegeven en meerdere netwerkinterfaces (NIC's) toevoegen aan elk van uw VMs. Meerdere NIC inschakelen scheiding van typen verkeer via NIC's. Eén NIC misschien bijvoorbeeld communiceren met Internet, terwijl een andere alleen met interne bronnen niet verbonden met Internet communiceert. De mogelijkheid om te scheiden van netwerkverkeer over meerdere NIC is vereist voor veel virtuele netwerkapparaten, zoals de bezorging van de toepassing en WAN-optimalisatieoplossingen.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

U kunt op dit moment VMs met een enkel NIC en VMs met meerdere NIC geen in de dezelfde cloudservice. Daarom moet u de back-end-servers implementeren in een andere cloudservice dan en alle andere onderdelen in het scenario. De onderstaande stappen gebruiken een cloudservice *IaaSStory* voor de belangrijkste resources en *IaaSStory-BackEnd* benoemde voor de back-end-servers.

## <a name="prerequisites"></a>Vereisten voor

Voordat u de back-end-servers implementeren kunt, moet u de belangrijkste cloudservice met de benodigde resources voor dit scenario implementeren. Ten minste moet u een virtueel netwerk maken met een subnet voor de backend. Ga naar [een virtueel netwerk via PowerShell maken](virtual-networks-create-vnet-classic-netcfg-ps.md) om informatie over het implementeren van een virtueel netwerk.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>De back-end VMs implementeren

De backend-VMs, hangt af van het maken van de onderstaande resources.

- **Back-end subnet**. De databaseservers uitmaken deel van een afzonderlijk subnet, om te scheiden van verkeer. De onderstaande script verwacht dit subnet in een vnet met de naam *WTestVnet*voorkomt.
- **Opslag-account voor gegevensschijven**. Voor betere prestaties, wordt de gegevensschijven op de databaseservers effen staat station (SSD) technologie, die is vereist een premium-opslag-account gebruikt. Zorg ervoor dat de Azure locatie die u ter ondersteuning van premium opslag implementeren.
- **Beschikbaarheid instellen**. Op alle databaseservers worden, toegevoegd aan een enkele beschikbaarheid instellen, om ervoor te zorgen ten minste één van de VMs omhoog en uit te voeren tijdens onderhoud.

### <a name="step-1---start-your-script"></a>Stap 1: beginnen uw script

U kunt de volledige PowerShell-script gebruikt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Volg de onderstaande stappen voor het wijzigen van het script om te werken in uw omgeving.

1. Wijzig de waarden van de variabelen op basis van uw bestaande resourcegroep geïmplementeerd boven in de [vereisten](#Prerequisites).

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor uw back-end-implementatie.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: nodig resources voor uw VMs maken

Moet u een nieuwe cloudservice en een account opslag voor de gegevensschijven maken voor alle VMs. U moet ook een afbeelding en een lokale beheerdersaccount opgeven voor de VMs. Als u wilt deze resources hebt gemaakt, de volgende stappen worden uitgevoerd.

1. Maak een nieuwe cloudservice.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Maak een nieuw account van de premium-opslag.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Stel de opslag-account gemaakt boven aan de huidige opslag-account voor uw abonnement.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Selecteer een afbeelding voor de VM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. De beheerder van het lokale accountreferenties instellen.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Stap 3: VMs maken

U moet een lus gebruiken om te maken van zoveel VMs als u wilt en de benodigde NIC's en VMs binnen de lus maken. Als u wilt de NIC's en VMs hebt gemaakt, de volgende stappen worden uitgevoerd.

1. Start een `for` herhalen als u wilt herhalen de opdrachten voor het maken van een VM en twee NIC's zo vaak als nodig is, op basis van de waarde van de `$numberOfVMs` variabele.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Maak een `VMConfig` object waarbij u de afbeelding, de grootte en de beschikbaarheid van instellen voor de VM.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. De VM als een Windows VM inrichten.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. De standaardkleuren NIC instellen en hieraan een statische IP-adres.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Voeg een tweede NIC voor elke VM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Voor elke VM maken naar gegevensschijven.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Elke VM maken en de lus beëindigen.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Stap 4 - het script uitvoeren

Nu dat u hebt gedownload en gewijzigd van het script op basis van uw behoeften voldoet, runt hij scripts kunt gebruiken om de back-end database VMs met meerdere NIC's maken.

1. Sla uw script en uitvoeren vanaf de **PowerShell** -opdrachtprompt of **Filter wissen**. Ziet u de eerste uitvoer, zoals hieronder wordt weergegeven.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Vul de vereiste informatie in de prompt referenties en klik op **OK**. De uitvoer hieronder wordt weergegeven.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
