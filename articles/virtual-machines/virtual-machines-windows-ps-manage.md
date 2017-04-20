<properties
    pageTitle="Beheren met resourcemanager en PowerShell VMs | Microsoft Azure"
    description="Virtuele machines met Azure resourcemanager en PowerShell beheren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Azure virtuele Machines met resourcemanager en PowerShell beheren

## <a name="install-azure-powershell"></a>Azure PowerShell installeren
 
Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="set-variables"></a>Variabelen instellen

Alle opdrachten in het artikel zijn de naam van het resourceveld groep waarin de virtuele machine zich bevindt en de naam van de virtuele machine voor het beheren van vereist. Vervang de waarde van **$rgName** door de naam van de resourcegroep die de virtuele machine bevat. Vervang de waarde van **$vmName** door de naam van de VM. Maak de variabelen.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Informatie over een VM weergeven

U kunt de gegevens VM.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Dit geeft ongeveer zo in dit voorbeeld:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Een virtuele machine stoppen

Stoppen met het actieve virtuele machine.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

U wordt gevraagd om bevestiging:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Voer **Y** als u wilt stoppen de virtuele machine.

Na een paar minuten retourneert deze ongeveer zo in dit voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Een virtuele machine start

Start de virtuele machine als deze gestopt.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Na een paar minuten retourneert deze ongeveer zo in dit voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Als u opnieuw wilt laten een virtuele machine die al wordt uitgevoerd, beschreven gebruik **Opnieuw starten-AzureRmVM** volgende.

## <a name="restart-a-virtual-machine"></a>Een virtuele machine opnieuw

Start opnieuw op de actieve virtuele machine.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Dit geeft ongeveer zo in dit voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Een virtuele machine verwijderen

Verwijder de virtuele machine.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] U kunt de **-dwingen** parameter overgeslagen bevestiging wordt gevraagd.

Als u geen gebruikt om de werking parameter-, kunt u wordt gevraagd om bevestiging:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Dit geeft ongeveer zo in dit voorbeeld:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Een virtuele machine bijwerken

In dit voorbeeld ziet hoe u de grootte van de virtuele machine bijwerken.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Dit geeft ongeveer zo in dit voorbeeld:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Zie [grootten voor virtuele machines in Azure wordt aangegeven](virtual-machines-windows-sizes.md) voor een lijst met beschikbare formaten voor een virtuele machine.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Een gegevensschijf toevoegen aan een virtuele machine

In dit voorbeeld ziet u hoe u een gegevensschijf toevoegt aan een bestaande VM.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

De schijf die u toevoegt, is niet geïnitialiseerd. Als u wilt de schijf geïnitialiseerd, kunt u Meld u aan bij deze en schijf management gebruiken. Als u geïnstalleerd WinRM en een certificaat op is geïnstalleerd wanneer u deze hebt gemaakt, kunt u externe PowerShell initialisatie van de schijf. U kunt ook de uitbreiding van een aangepast script gebruiken: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Het scriptbestand kan ongeveer zo deze code initialisatie van de schijven bevatten:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Volgende stappen

Als er problemen met een implementatie zijn, u [Probleemoplossing resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md) mogelijk bekijken
