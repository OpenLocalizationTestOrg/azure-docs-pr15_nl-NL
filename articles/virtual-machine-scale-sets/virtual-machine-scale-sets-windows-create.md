<properties
    pageTitle="Maak een virtuele Machine schaal instellen via PowerShell | Microsoft Azure"
    description="Maak een virtuele Machine schaal instellen via PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Een Windows-VM schaal instellen via Azure PowerShell maken

Deze stappen volgt een aanpak aanvullen-in-het-lege waarden voor het maken van een Azure virtuele machines schaal instellen. Zie [VM schaal Sets overzicht](virtual-machine-scale-sets-overview.md) voor meer informatie over schaal sets.

Het moet ongeveer 30 minuten duren moet de stappen in dit artikel.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="step-2-create-resources"></a>Stap 2: Resources maken

De resources die nodig zijn voor uw nieuwe schaal set maken.

### <a name="resource-group"></a>Resourcegroep

Een virtuele machine schaal set moet worden opgenomen in een resourcegroep.

1. Krijg een lijst met beschikbare locaties waar u resources kunt plaatsen:

        Get-AzureLocation | Sort Name | Select Name

2. Kies een locatie die geschikt is voor u, de waarde van **$locName** vervangen door de naam van die locatie en vervolgens de variabele te maken:

        $locName = "location name from the list, such as Central US"

3. Vervang de waarde van **$rgName** door de naam die u wilt gebruiken voor de nieuwe resourcegroep en klikt u vervolgens de variabele te maken: 

        $rgName = "resource group name"
        
4. De resourcegroep maken:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Worden er ongeveer zo in dit voorbeeld:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Opslag-account

Een opslag-account wordt gebruikt door een virtuele machine voor de opslag van het besturingssysteem schijf en diagnostische gegevens die worden gebruikt voor schaalbaarheid. Indien mogelijk, is het aanbevolen wordt om een account opslag voor elke virtuele machine die is gemaakt in een set schaal. Als dit niet mogelijk, plannen voor niet meer dan 20 VMs per opslag-account. Het voorbeeld in dit artikel ziet drie opslag accounts voor drie virtuele machines wordt gemaakt.

1. Vervang de waarde van **$saName** door een naam voor de opslag-account. Test de naam voor de uniekheid. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Als het antwoord **waar**is, is de naam van de voorgestelde uniek.

3. Vervang de waarde van **$saType** door het type account dat opslag en vervolgens de variabele te maken:  

        $saType = "storage account type"
        
    Mogelijke waarden zijn: Standard_LRS, Standard_GRS, Standard_RAGRS of Premium_LRS.
        
4. Het account maken:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Worden er ongeveer zo in dit voorbeeld:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Herhaal stap 1 tot en met 4 drie opslag accounts, bijvoorbeeld myst1, myst2 en myst3 te maken.

### <a name="virtual-network"></a>Virtuele netwerk

Een virtueel netwerk is vereist voor de virtuele machines in de set schaal.

1. Vervang de waarde van **$subnetName** door de naam die u wilt gebruiken voor het subnet in het virtuele netwerk en vervolgens de variabele te maken: 

        $subnetName = "subnet name"
        
2. De subnetconfiguratie maken:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Het adresvoorvoegsel mogelijk anders in uw virtuele netwerk.

3. Vervang de waarde van **$netName** door de naam die u wilt gebruiken voor het virtuele netwerk en vervolgens de variabele te maken: 

        $netName = "virtual network name"
        
4. Het virtuele netwerk maken:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Configuratie van de set schaal

U hebt alle bronnen die u nodig hebt voor de schaal configuratie instellen, zodat we deze hebt gemaakt.  

1. Vervang de waarde van **$ipName** door de naam die u wilt gebruiken voor de IP-configuratie en vervolgens de variabele te maken: 

        $ipName = "IP configuration name"
        
2. De IP-configuratie maken:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Vervang de waarde van **$vmssConfig** door de naam die u wilt gebruiken voor de configuratie van de set schaal en vervolgens de variabele te maken:   

        $vmssConfig = "Scale set configuration name"
        
3. De configuratie voor de set schaal maken:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    In dit voorbeeld ziet u dat een schaal instellen wordt gemaakt met de drie virtuele machines. Zie [VM schaal Sets overzicht](virtual-machine-scale-sets-overview.md) voor meer informatie over de capaciteit van schaal sets. Deze stap bevat ook de grootte (genoemd SkuName) van de virtuele machines instellen in de set. Als u wilt zoeken naar een grootte die aan uw wensen voldoet, kijkt u naar [grootten voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. De interface netwerkconfiguratie toevoegen aan de configuratie van schaal instellen:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Worden er ongeveer zo in dit voorbeeld:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Besturingssysteem profiel

1. Vervang de waarde van **$computerName** met voorvoegsel van de computer die u wilt gebruiken en vervolgens de variabele te maken: 

        $computerName = "computer name prefix"
        
2. Vervang de waarde van **$adminName** de naam van het beheerdersaccount op de virtuele machines en vervolgens de variabele te maken:

        $adminName = "administrator account name"
        
3. Vervang de waarde van **$adminPassword** door het accountwachtwoord en maak de variabele:

        $adminPassword = "password for administrator accounts"
        
4. Het besturingssysteem-profiel maken:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Opslag-profiel

1. Vervang de waarde van **$storageProfile** door de naam die u wilt gebruiken voor het profiel opslag en vervolgens de variabele te maken:  

        $storageProfile = "storage profile name"
        
2. Maak de variabelen die definiëren van de afbeelding wilt gebruiken:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Als u wilt zoeken de informatie over andere afbeeldingen die u wilt gebruiken, kijkt u naar [navigeren en selecteer Azure virtuele machines afbeeldingen met Windows PowerShell en de CLI Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Vervang de waarde van **$vhdContainers** door een lijst met de paden waarin de virtuele vaste schijven zijn opgeslagen, zoals 'https://mystorage.blob.core.windows.net/vhds', en maak de variabele:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. De opslag-profiel maken:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>VM schaal instellen

Tot slot kunt u de schaal set maken.

1. Vervang de waarde van **$vmssName** door de naam van het instellen van de schaal virtuele machine en vervolgens de variabele te maken:

        $vmssName = "scale set name"
        
2. De set schaal maken:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Worden er ongeveer zo in dit voorbeeld waarin een succesvolle implementatie:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Stap 3: Resources verkennen

Gebruik deze bronnen voor het verkennen van de VM schaal instellen die u hebt gemaakt:

- Azure-portal - een beperkte tijdsduur van gegevens is beschikbaar met behulp van de portal.
- [Azure Resource Explorer](https://resources.azure.com/) - dit hulpmiddel is het beste werkt voor het verkennen van de huidige status van een set schaal.
- Azure PowerShell - Gebruik deze opdracht om informatie te verkrijgen:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Volgende stappen

- De schaal instellen dat u zojuist hebt gemaakt met behulp van de informatie in het [beheren van virtuele machines in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-manage.md) beheren
- Houd rekening met het instellen van automatische schaalbaarheid van uw schaal instelt met behulp van de informatie in het [Automatische schaalbaarheid en VM schaal wordt ingesteld](virtual-machine-scale-sets-autoscale-overview.md)
- Meer informatie over de verticale schaling met [verticale automatisch schalen met VM schaal sets](virtual-machine-scale-sets-vertical-scale-reprovision.md) reviseren
