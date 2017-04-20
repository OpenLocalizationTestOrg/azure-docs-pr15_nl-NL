<properties
    pageTitle="PowerShell gebruiken in te schakelen diagnostisch hulpprogramma Azure virtuele machines waarop Windows wordt uitgevoerd | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Informatie over het gebruik van PowerShell in te schakelen diagnostisch hulpprogramma Azure virtuele machines waarop Windows wordt uitgevoerd"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>PowerShell gebruiken in te schakelen diagnostisch hulpprogramma Azure virtuele machines waarop Windows wordt uitgevoerd

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure diagnostische gegevens is de mogelijkheid binnen Azure waarmee het vastleggen van diagnostische gegevens op een gedistribueerde toepassing. U kunt de extensie diagnostische hulpprogramma's voor het verzamelen van diagnostische gegevens zoals toepassingslogboeken aan de of items uit een Azure virtuele machine (VM) waarop Windows wordt uitgevoerd. In dit artikel wordt beschreven hoe Windows PowerShell gebruiken om te schakelen van de extensie diagnostische hulpprogramma's voor een VM. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die u nodig hebt voor dit artikel.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>De extensie diagnostische hulpprogramma's inschakelen als u het implementatiemodel resourcemanager gebruiken

Terwijl u een Windows-VM via het objectmodel van de implementatie van Azure resourcemanager maken door de configuratie van de extensie toe te voegen aan de sjabloon resourcemanager, kunt u de extensie diagnostische hulpprogramma's inschakelen. Zie [een virtuele Windows-computer met cmdlets voor controle en diagnostische gegevens met behulp van de Azure resourcemanager-sjabloon maken](virtual-machines-windows-extensions-diagnostics-template.md).

Als u wilt de extensie diagnostische gegevens op een bestaande VM die is gemaakt via het implementatiemodel resourcemanager inschakelt, kunt u de cmdlet [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell zoals hieronder wordt weergegeven.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* is het pad naar het bestand met de configuratie van de diagnostische in XML-bestand, zoals wordt beschreven in het onderstaande [voorbeeld](#sample-diagnostics-configuration) .  

Als het configuratiebestand diagnostisch hulpprogramma Hiermee geeft u een element **StorageAccount** met een opslagaccountnaam, klikt u vervolgens in het script *Set-AzureRMVMDiagnosticsExtension* automatisch de extensie diagnostisch hulpprogramma Diagnostische gegevens te verzenden naar dat opslag-account instellen. Dit werkt, moet het account opslagruimte in hetzelfde als de VM-abonnement.

Als er geen **StorageAccount** is opgegeven in de configuratie van de diagnostische, moet u in de parameter *StorageAccountName* om aan te geven de cmdlet. Als de parameter *StorageAccountName* is opgegeven, wordt klikt u vervolgens de cmdlet altijd gebruiken het opslag-account dat is opgegeven in de parameter en niet de presentatie die is opgegeven in het configuratiebestand diagnostische gegevens.

Als het account van de opslagruimte diagnostische gegevens zich in een ander abonnement van de VM, moet u expliciet in de *StorageAccountName* en *StorageAccountKey* parameters om aan te geven de cmdlet. De parameter *StorageAccountKey* is niet nodig wanneer het hulpprogramma's voor diagnose opslag-account in hetzelfde abonnement, zoals de cmdlet kan automatisch uitvoeren en de sleutelwaarde instellen wanneer de extensie diagnostische hulpprogramma's inschakelen. Echter als het account van de opslagruimte diagnostische gegevens in een ander abonnement, klikt u vervolgens de cmdlet mogelijk niet automatisch ophalen van de sleutel en moet u de sleutel tot en met de parameter *StorageAccountKey* expliciet opgeven.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Zodra de extensie diagnostische gegevens op een VM is ingeschakeld, krijgt u de huidige instellingen via de cmdlet [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) .

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

De cmdlet retourneert *PublicSettings*, waarin de XML-configuratie in een indeling Base64-codering. Als u wilt lezen de XML, moet u dit decoderen.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

De cmdlet [Verwijderen-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) kan worden gebruikt voor de extensie diagnostische gegevens verwijderen uit de VM.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>De extensie diagnostische hulpprogramma's inschakelen als u het implementatiemodel klassieke gebruiken

U kunt de cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) inschakelen extensie diagnostische gegevens op een VM die u hebt gemaakt via het objectmodel klassieke implementatie. Het volgende voorbeeld ziet u hoe u een nieuwe VM via het objectmodel klassieke implementatie maakt met de extensie diagnostische gegevens is ingeschakeld.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Als u wilt dat de extensie diagnostische gegevens op een bestaande VM die is gemaakt via het implementatiemodel klassieke, gebruikt u eerst de cmdlet [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) om de configuratie VM. Vervolgens de VM-configuratie als u wilt opnemen van de extensie diagnostische gegevens met behulp van de cmdlet [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) bijwerken. Ten slotte de bijgewerkte configuratie voor VM toepassen met behulp van de [Update-AzureVM](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Configuratie van de steekproef diagnostische

De volgende XML kan worden gebruikt voor de openbare configuratie van diagnostische gegevens met de bovenstaande scripts. Deze configuratie van de steekproef worden diverse prestatietellers overbrengen naar het hulpprogramma's voor diagnose opslag-account, samen met fouten uit de toepassing, beveiliging en systeemkanalen in de Windows-Logboeken en eventuele fouten uit de logboeken aan de infrastructuur diagnostische gegevens.

De configuratie moet worden bijgewerkt met het volgende:

- Het kenmerk *resourceID* van het element dat **aan de doelstellingen** moet worden bijgewerkt met de resource-ID voor de VM.
    - De resource-ID kan worden samengesteld met behulp van de volgende patroon: "/ abonnementen / {*abonnements-ID voor het abonnement met de VM*} /resourceGroups/ {*de naam van de resourcegroup voor VM*} / providers/Microsoft.Compute/virtualMachines/ {*de naam van de VM*}".
    - Bijvoorbeeld als de abonnements-ID voor het abonnement waarop de VM wordt uitgevoerd **11111111-1111-1111-1111-111111111111 is**, de naam van de resource-groep voor de resourcegroep **MyResourceGroup is**en de naam van de VM **MyWindowsVM**is, zou klikt u vervolgens de waarde voor *resourceID* :

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Voor meer informatie over het aan de doelstellingen zijn gegenereerd op basis van de configuratie van items en aan de doelstellingen prestaties, Zie [Diagnostisch hulpprogramma Azure aan de doelstellingen tabel in opslag](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- Het element **StorageAccount** moet worden bijgewerkt met de naam van het hulpprogramma's voor diagnose opslag-account.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Volgende stappen
- Zie [Diagnostisch hulpprogramma inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md)voor aanvullende richtlijnen over het gebruik van de mogelijkheid Azure diagnostische hulpprogramma's en andere technieken voor het oplossen van problemen.
- [Diagnostisch hulpprogramma configuraties schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) beschrijft de verschillende XML-configuraties opties voor de extensie diagnostische gegevens.
