<properties
    pageTitle="Beheren VMs in een Set van de schaal VM | Microsoft Azure"
    description="Virtuele machines in een virtuele machine schaal instellen via Azure PowerShell beheren."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Virtuele machines in een set van de schaal VM beheren

Gebruik de taken in dit artikel voor het beheren van virtuele machines in uw VM schaal instellen.

De meeste taken waarbij u gebruikmaakt van een virtuele machine in een set schaal beheren is vereist dat u weet dat de exemplaar-ID van de computer waarop u wilt beheren. [Azure Resource Explorer](https://resources.azure.com) kunt u de exemplaar-ID van een virtuele machine zoeken in een set schaal. U kunt ook Resource Explorer gebruiken om te controleren of de status van de taken die u klaar bent.

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="display-information-about-a-scale-set"></a>Informatie over het instellen van een schaal weergeven

U gaat algemene informatie over een set schaal, die ook wel de weergave exemplaar wordt genoemd. Of u vind specifiekere informatie, zoals informatie over de resources in de set schaal.

De waarden tussen aanhalingstekens vervangen door de naam of de resourcegroep en schaal instellen en vervolgens de opdracht uitvoeren:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Dit geeft ongeveer zo uitziet:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
De waarden tussen aanhalingstekens vervangen door de naam van uw resources groeperen en schaal instellen. Vervang *#* met de instantie-id van de virtuele machine die u wilt u informatie wilt over en vervolgens uit te voeren:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Dit geeft ongeveer zo in dit voorbeeld:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Een virtuele machine start in een set schaal

De waarden tussen aanhalingstekens vervangen door de naam van uw resources groeperen en schaal instellen. Vervang *#* met de id van de virtuele machine die u wilt starten en vervolgens uit te voeren:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

In Resource Explorer kunt zien we dat de status van het exemplaar **uitgevoerd is**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

U kunt de virtuele machines in de schaal instellen met behulp van de parameter - InstanceId niet starten.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Een virtuele machine in een set schaal stoppen

De waarden tussen aanhalingstekens vervangen door de naam van uw resources groeperen en schaal instellen. Vervang *#* met de id van de virtuele machine die u wilt stoppen en vervolgens uit te voeren:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

In Resource Explorer kunt zien we dat de status van het exemplaar **opgeheven is**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Als u wilt stoppen met een virtuele machine en deze niet opheffen, gebruikt u de parameter - StayProvisioned. U kunt de virtuele machines in de set niet met de parameter - InstanceId stoppen.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Een virtuele machine opnieuw in een set schaal

De waarden tussen aanhalingstekens vervangen door de naam van uw resourcegroep en de schaal is ingesteld. Vervang *#* met de id van de virtuele machine die u wilt starten en vervolgens uit te voeren:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
U kunt de virtuele machines opnieuw in de set niet met de parameter - InstanceId.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Een virtuele machine verwijderen uit een set schaal

De waarden tussen aanhalingstekens vervangen door de naam van uw resourcegroep en de schaal is ingesteld. Vervang *#* met de id van de virtuele machine die u wilt verwijderen en vervolgens uit te voeren:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

U kunt het virtuele machine schaal instellen in één keer verwijderen niet met de parameter - InstanceId.

## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een set schaal wijzigen

U kunt toevoegen of verwijderen van virtuele machines door de capaciteit van de set wijzigen. De schaal instellen die u wilt wijzigen, de capaciteit instellen op wat u nodig hebt om te worden en past u de schaal instellen met de nieuwe capaciteit krijgen. In deze opdrachten kunt u de waarden tussen aanhalingstekens vervangen door de naam van uw resourcegroep en de schaal is ingesteld.

  $vmss = get-AzureRmVmss - ResourceGroupName "groep resourcenaam" - VMScaleSetName "naam schaal instellen" $vmss.sku.capacity = 5 Update-AzureRmVmss - ResourceGroupName "groep resourcenaam"-"schalen dat de setnaam van de" naam - VirtualMachineScaleSet $vmss 

Als u virtuele machines uit de set schaal verwijdert, worden eerst de virtuele machines met de hoogste id's verwijderd.
