<properties
    pageTitle="Formaat van een Windows VM | Microsoft Azure"
    description="Het formaat van een virtuele Windows-computer die is gemaakt in het implementatiemodel resourcemanager, via Azure Powershell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>De grootte van een Windows VM wijzigen

In dit artikel leest u hoe u het formaat van een Windows-VM, die is gemaakt in het resourcemanager implementatiemodel via Azure Powershell.

Nadat u een virtuele machine (VM) hebt gemaakt, kunt u de VM schalen omhoog of omlaag door de VM tekengrootte te wijzigen. In sommige gevallen, moet u eerst de VM toewijzing. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op het cluster hardware waarop de VM momenteel.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Formaat van een Windows-VM niet in een set beschikbaarheid

1. Lijst met de VM-grootte die beschikbaar zijn op het hardware cluster waar de VM wordt gehost. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Als de gewenste grootte wordt weergegeven, voert u de volgende opdrachten om het formaat van de VM aan. Als de gewenste grootte niet wordt weergegeven, gaat u naar stap 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Als de gewenste grootte niet wordt weergegeven, voer de volgende opdrachten toewijzing van de VM, het formaat en de VM start.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] De VM vrijgeven loslaat dynamische IP-adressen die zijn toegewezen aan de VM. De schijven OS en gegevens worden niet beïnvloed. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Formaat van een Windows-VM in een set beschikbaarheid

Als de nieuwe grootte voor een VM in een set beschikbaarheid niet beschikbaar op het hardware cluster momenteel hostingprovider de VM is, klikt u vervolgens moet alle VMs in de set beschikbaarheid om de grootte van de VM te worden opgeheven. Ook mogelijk moet u de grootte van andere VMs-update in de beschikbaarheid instellen nadat een VM formaat is gewijzigd. Als u een VM in een set beschikbaarheid, moet u de volgende stappen uitvoeren.

1. Lijst met de VM-grootte die beschikbaar zijn op het hardware cluster waar de VM wordt gehost.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Als de gewenste grootte wordt weergegeven, voert u de volgende opdrachten om het formaat van de VM aan. Als deze niet wordt weergegeven, gaat u naar stap 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Als de gewenste grootte niet wordt weergegeven, gaat u verder met de volgende stappen uit op alle VMs in de set beschikbaarheid van de toewijzing VMs formaat en het opnieuw.

4.  Alle VMs in de set beschikbaarheid stoppen.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Het formaat wijzigen en opnieuw beginnen de VMs in de beschikbaarheid van de set.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Volgende stappen

- Voor extra schaalbaarheid, meerdere exemplaren van VM uitvoeren en schaal af. Zie [automatisch schalen Windows-computers in een virtuele Machine schaal instellen](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)voor meer informatie.



