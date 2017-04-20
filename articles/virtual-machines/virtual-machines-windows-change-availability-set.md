<properties
    pageTitle="Wijzigen van een set van de beschikbaarheid van VMs | Microsoft Azure"
    description="Informatie over het wijzigen van de beschikbaarheid instellen voor uw virtuele machines met Azure PowerShell en het implementatiemodel resourcemanager."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>De beschikbaarheid van instellen voor een Windows-VM wijzigen

De volgende stappen wordt beschreven hoe om de resultatenset beschikbaarheid van een VM via Azure PowerShell te wijzigen. Een VM kan alleen worden toegevoegd aan een beschikbaarheid instellen wanneer deze is gemaakt. Om te wijzigen van de beschikbaarheid instellen, moet u wilt verwijderen en opnieuw maken van de virtuele machine. 

## <a name="change-the-availability-set-using-powershell"></a>De beschikbaarheid van instellen via PowerShell wijzigen

1. Vastleggen van de volgende belangrijke informatie uit de VM moet worden gewijzigd.

    Naam van de VM
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    VM grootte
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Netwerk primaire netwerkinterface en hetzelfde optioneel netwerkinterfaces als ze op de VM staan
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    OS schijf profiel

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Schijf profielen voor elke gegevensschijf 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    VM extensies geïnstalleerd 
    
    ```powershell
    $vm.Extensions
    ```

2. Verwijder de VM zonder te verwijderen van een van de schijven of het netwerkinterfaces.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. De beschikbaarheid instellen als deze nog niet bestaat maken

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Maak opnieuw de VM met behulp van de nieuwe set van beschikbaarheid

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Voeg gegevensschijven en uitbreidingen. Zie [Gegevensschijf bijvoegen voor VM](virtual-machines-windows-attach-disk-portal.md) en [Extensie configuratie voorbeelden](virtual-machines-windows-extensions-configuration-samples.md)voor meer informatie. Gegevensschijven en uitbreidingen kunnen worden toegevoegd aan de VM via PowerShell of Azure CLI.

## <a name="example-script"></a>Voorbeeldscript

Het volgende script vindt u een voorbeeld van de vereiste gegevens verzamelen, de oorspronkelijke VM verwijderen en klik vervolgens opnieuw te maken tussen een set nieuwe beschikbaarheid.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Volgende stappen

Extra opslagruimte wilt toevoegen aan uw VM door het toevoegen van een extra [gegevens Schijfopruiming](virtual-machines-windows-attach-disk-portal.md).

