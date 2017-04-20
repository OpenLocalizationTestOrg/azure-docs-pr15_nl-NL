<properties
    pageTitle="Maken van een Azure VM via PowerShell | Microsoft Azure"
    description="Gebruik Azure PowerShell en Azure resourcemanager eenvoudig maken een nieuwe VM met Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Maken van een Windows-VM resourcemanager en PowerShell gebruiken

In dit artikel leest u hoe u snel een Azure virtuele machines met Windows Server en de resources die nodig zijn met [Resourcemanager](../azure-resource-manager/resource-group-overview.md) en PowerShell maakt. 

De stappen in dit artikel vereist voor het maken van een virtuele machine en moet duurt het ongeveer 30 minuten Voer de stappen uit. Voorbeeld parameterwaarden in de opdrachten vervangen door namen die geschikt is voor uw omgeving.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.
        
## <a name="step-2-create-a-resource-group"></a>Stap 2: Een resourcegroep maken

Alle resources moeten worden opgenomen in een resourcegroep, dus kunt dat eerst te maken.  

1. Haal een lijst met beschikbare locaties waar resources kunnen worden gemaakt.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Stel de locatie voor de resources. Deze opdracht Hiermee stelt u de locatie op **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Een resourcegroep maken. Deze opdracht maakt de resourcegroep **myResourceGroup** op de locatie die u hebt ingesteld met de naam.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Stap 3: Een opslag-account maken

Een [opslag-account](../storage/storage-introduction.md) nodig is voor de opslag van de virtuele harde schijf die wordt gebruikt door de virtuele machine die u maakt. Opslag accountnamen moeten tussen 3 en 24 tekens lang en kunnen bevatten getallen en alleen van toepassing zijn op kleine letters.

1. Test de accountnaam opslag voor uniekheid. Deze opdracht Hiermee test u de naam **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Als deze opdracht **True**retourneert, is de naam van de voorgestelde uniek zijn binnen Azure. 
    
2. Maak nu de opslag-account.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Stap 4: Een virtueel netwerk maken

Alle virtuele machines uitmaken deel van een [virtueel netwerk](../virtual-network/virtual-networks-overview.md).

1. Een subnet maken voor het virtuele netwerk. Deze opdracht Hiermee maakt u een benoemde **mySubnet** met het voorvoegsel van een adres van 10.0.0.0/24 subnet.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Maak nu het virtuele netwerk. Deze opdracht Hiermee maakt u een virtueel netwerk met de naam **myVnet** met het subnet die u hebt gemaakt en de voorvoegsel van een adres van **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Stap 5: Een openbare IP-adres en netwerk interface maken

Als u wilt kunnen communiceren met de virtuele machine in het virtuele netwerk, moet u een [openbare IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface.

1. Maak het openbare IP-adres. Deze opdracht Hiermee maakt u een openbare IP-adres met de naam **myPublicIp** met de methode voor een toewijzing van **dynamische**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. De netwerkinterface maken. Deze opdracht maakt u een netwerkinterface met de naam **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Stap 6: Een virtuele machine maken

Nu dat u alle delen op hun plaats staan hebt, is het tijd om te maken van de virtuele machine.

1. Deze opdracht om in te stellen van de naam van de beheerdersaccount en het wachtwoord voor de virtuele machine uitvoeren.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Het wachtwoord moet 12-123 tekens lang zijn en teken ten minste één kleine letters, hoofdletters teken, een getal en een speciaal teken hebben. 
        
2. Het configuratieobject voor de virtuele machine maken. Deze opdracht maakt een configuratieobject met de naam **myVmConfig** die de naam van de VM en de grootte van de VM definieert.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Zie [grootten voor virtuele machines in Azure wordt aangegeven](virtual-machines-windows-sizes.md) voor een lijst met beschikbare formaten voor een virtuele machine.
    
3. Besturingssysteem-instellingen voor de VM configureren. Deze opdracht stelt de computernaam, besturingssysteemtype en referenties voor de VM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. De afbeelding wilt gebruiken voor het inrichten van de VM definiëren. Deze opdracht Hiermee definieert u de Windows Server-afbeelding wilt gebruiken voor de VM. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Zie [navigeren en selecteer Windows VM afbeeldingen in Azure wordt aangegeven met PowerShell of de CLI](virtual-machines-windows-cli-ps-findimage.md)voor meer informatie over het selecteren van afbeeldingen te gebruiken.
        
5. De netwerkinterface die u hebt gemaakt in de configuratie toevoegen.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. De naam en locatie van de harde schijf van VM definiëren. Het virtuele harde schijf-bestand is opgeslagen in een container. Deze opdracht maakt de schijf in een container met de naam **vhds/WindowsVMosDisk.vhd** in het opslag-account dat u hebt gemaakt.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Voeg de informatie van de schijf besturingssysteem in de configuratie VM. Vervang de waarde van **$diskName** door een naam voor de schijf besturingssysteem gebruikt. Maak de variabele en de gegevens over toevoegen aan de configuratie.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Ten slotte de virtuele machine maken.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie zijn, zou een volgende stap nagaan [Probleemoplossing resource groep implementaties met Azure-portal](../resource-manager-troubleshoot-deployments-portal.md)
- Informatie over het beheren van de virtuele machine die u aan de hand van [beheren virtuele machines met Azure resourcemanager en PowerShell](virtual-machines-windows-ps-manage.md)hebt gemaakt.
- Profiteren van het gebruik van een sjabloon voor het maken van een virtuele machine met behulp van de gegevens in [een virtuele Windows-computer met een resourcemanager-sjabloon maken](virtual-machines-windows-ps-template.md)
