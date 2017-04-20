<properties
   pageTitle="Aangepast Script-extensie op een Windows-VM | Microsoft Azure"
   description="Azure VM configuratietaken uit te voeren met behulp van de extensie aangepast Script uitvoeren PowerShell-scripts op een externe Windows VM automatiseren"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Aangepast Script-extensie voor Windows virtuele machines

In dit artikel geeft een overzicht van het gebruik van de extensie aangepast Script op Windows VMs via Azure PowerShell-cmdlets met Azure-Service API's.

VM (VM) extensies zijn gebouwd door Microsoft en vertrouwde uitgevers van derden voor het uitbreiden van de functionaliteit van de VM. Zie voor een overzicht van VM extensies, [Azure VM extensions en functies](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met behulp van het model resourcemanager](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Overzicht van de extensie aangepast Script

U kunt met de extensie aangepast Script voor Windows PowerShell-scripts uitvoeren op een externe VM zonder het aanmelden bij deze. U kunt de scripts na de inrichting van de VM of op elk gewenst moment tijdens de levenscyclus van VM uitvoeren zonder eventuele extra poorten te openen. De meest voorkomende gevallen gebruiken voor het uitvoeren van aangepast Script extensie zijn uitgevoerd, installeren en configureren van extra software op de VM nadat deze ingericht.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Vereisten voor het uitvoeren van de extensie aangepast Script

1. <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-cmdlets</a> versie 0.8.0 installeren of hoger.
2. Als u wilt dat de scripts worden uitgevoerd op een bestaande VM, zorg er dan voor dat op de VM VM-Agent is ingeschakeld. Als dit niet is geïnstalleerd, volgt u deze [stappen](virtual-machines-windows-classic-agents-and-extensions.md). Als de VM vanuit de Azure-portal is gemaakt, is de VM Agent al dan niet standaard geïnstalleerd.
3. Upload de scripts die u wilt uitvoeren op de VM met Azure Storage. De scripts kunnen afkomstig zijn uit een container één of meerdere Opslagcontainers.
4. Het script moet worden geschreven, zodat het script invoer, dat wordt gestart met de bestandsextensie, andere scripts wordt gestart.

## <a name="custom-script-extension-scenarios"></a>Aangepast Script extensie-scenario 's

### <a name="upload-files-to-the-default-container"></a>Bestanden uploaden naar de standaardcontainer

Het volgende voorbeeld ziet hoe u kunt uw scripts uitvoeren op de VM als ze zich in de container opslag van het standaardaccount van uw abonnement. U kunt uw scripts uploaden naar ContainerName. U kunt controleren of het standaardaccount voor de opslag met behulp van de **Get-AzureSubscription – standaard** opdracht.

Het volgende voorbeeld wordt een VM, maar u kunt ook hetzelfde scenario uitvoeren op een bestaande VM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Bestanden uploaden naar een niet-standaard opslag container

Dit scenario ziet u hoe u een niet-standaard opslag container binnen hetzelfde abonnement of in een ander abonnement voor scripts en bestanden uploaden. In dit voorbeeld ziet u een bestaande VM, maar bewerkingen kunnen worden uitgevoerd wanneer u een VM maakt.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Scripts uploaden naar meerdere containers verschillende opslag-accounts

  Als de scriptbestanden zijn opgeslagen in meerdere containers, hebt u de URL van de volledige toegang tot de gedeelde-handtekeningen (SA's) voor de bestanden als u wilt uitvoeren van scripts bieden.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>De extensie aangepast Script van de Azure portal toevoegen

Ga naar de VM in de <a href="https://portal.azure.com/ " target="_blank">portal van Azure</a> en de extensie toevoegen door te geven van de script-bestand om uit te voeren.

  ![Geef het scriptbestand][5]


### <a name="uninstall-the-custom-script-extension"></a>Verwijderen van de extensie aangepast Script

U kunt de extensie aangepast Script van de VM verwijderen met behulp van de volgende opdracht uit.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Gebruik de extensie aangepast Script met sjablonen

Zie voor meer informatie over het gebruik van de extensie aangepast Script met Azure resourcemanager sjablonen, [met de extensie aangepast Script voor Windows VMs met Azure resourcemanager sjablonen](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
