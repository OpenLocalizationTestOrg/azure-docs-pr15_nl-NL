<properties 
   pageTitle="Algemene PowerShell-opdrachten voor VMs | Microsoft Azure"
   description="Algemene PowerShell-opdrachten aan u aan de slag maken en beheren van uw VMs in Azure wordt aangegeven in Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Algemene PowerShell-opdrachten voor het maken en beheren van VMs

In dit artikel komen enkele van de Azure PowerShell-opdrachten waarmee u kunt maken en beheren van virtuele machines in uw Azure-abonnement.  Voor meer gedetailleerde hulp met specifieke schakelopties en opties kunt u **Get-Help** - *opdracht*.

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="create-a-vm"></a>Een VM maken

Taak | Opdracht
-------------- | -------------------------
Een configuratie VM maken | $vm = [Nieuw AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>De configuratie VM wordt gebruikt om te definiëren of instellingen voor de VM bijwerken. De configuratie is geïnitialiseerd met de naam van de VM en de [grootte](virtual-machines-windows-sizes.md).
Configuratieinstellingen toevoegen | $vm = [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - VM $vm-Windows - computernaam "computernaam"-referenties $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Instellingen van het besturingssysteem inclusief [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan de configuratieobject dat u eerder hebt gemaakt met New-AzureRmVMConfig.
Een netwerkinterface toevoegt | $vm = [Toevoegen-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - VM $vm-Id $NIC ID<BR></BR><BR></BR>Een VM moet een [netwerkinterface](virtual-machines-windows-ps-create.md) om te communiceren in een virtueel netwerk hebben. U kunt ook [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) gebruiken om op te halen van een bestaand netwerk interface-object.
Een afbeelding platform opgeven | $vm [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - VM $vm - PublisherName "publisher_name" =-bieden "publisher_offer" - SKU's "product_sku"-"nieuwste" versie<BR></BR><BR></BR>[Informatie over de afbeelding](virtual-machines-windows-cli-ps-findimage.md) wordt toegevoegd aan de configuratieobject dat u eerder hebt gemaakt met New-AzureRmVMConfig. Het object dat het resultaat van deze opdracht wordt alleen gebruikt wanneer u de schijf OS gebruik van de afbeelding van een platform instelt.
Set OS Schijfopruiming om een afbeelding platform te gebruiken | $vm = [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - VM $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"naam"disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>De naam van de schijf besturingssysteem en de locatie in de [opslagruimte](../storage/storage-powershell-guide-full.md) wordt toegevoegd aan de configuratieobject dat u eerder hebt gemaakt.
Set OS Schijfopruiming om een algemene afbeelding te gebruiken | $vm = set-AzureRmVMOSDisk - VM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"naam"disk_name"- SourceImageUri. {guid} VHD"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>De naam van de schijf besturingssysteem, de locatie van de bronafbeelding en de locatie van de schijf in [opslag](../storage/storage-powershell-guide-full.md) wordt toegevoegd aan het configuratieobject.
OS schijf gebruik van de afbeelding van een gespecialiseerde instellen | $vm = set-AzureRmVMOSDisk - VM $vm-http://mystore1.blob.core.windows.net/vhds/"naam"name_of_disk"- VhdUri" - CreateOption bijvoegen - Windows
Een VM maken | [Nieuw-AzureRmVM]() - ResourceGroupName "resource_group_name"-locatie "naam_locatie" - VM $vm<BR></BR><BR></BR>Alle resources zijn gemaakt in een [resourcegroep](../powershell-azure-resource-manager.md). De VM moet worden gemaakt op dezelfde [locatie](https://msdn.microsoft.com/library/azure/dn495177.aspx) als de resourcegroep. Voordat u deze opdracht uitvoert, voert u nieuw AzureRmVMConfig, Set-AzureRmVMOperatingSystem Set-AzureRmVMSourceImage, toevoegen-AzureRmVMNetworkInterface en Set-AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>Informatie over VMs

Taak | Opdracht
-------------- | -------------------------
Lijst VMs in een abonnement| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Lijst VMs in een resourcegroep | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Als u een lijst met resourcegroepen in uw abonnement, gebruikt u [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Informatie over een VM | Get-AzureRmVM - ResourceGroupName "resource_group_name"-naam "vm_name"

## <a name="manage-vms"></a>VMs beheren

Taak | Opdracht
-------------- | -------------------------
Een VM starten | [Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-naam "vm_name"
Een VM stoppen | [Stop-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-naam "vm_name"
Een actieve VM opnieuw | [Opnieuw starten-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-naam "vm_name"
Een VM verwijderen | [Verwijderen-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-naam "vm_name"
Een VM generalize | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-naam "vm_name"-Generalized<BR></BR><BR></BR>Deze opdracht uitvoeren voordat u opslaan-AzureRmVMImage uitvoert.
Een VM vastleggen | [Opslaan-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-pad "C:\filepath\filename.json"<BR></BR><BR></BR>Een virtuele machine moet [afsluiten en generalized](virtual-machines-windows-generalize-vhd.md) moet worden gebruikt om een afbeelding te maken. Voordat u deze opdracht uitvoert, voert u Set-AzureRmVm.
Een VM bijwerken | [Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - VM $vm<BR></BR><BR></BR>De huidige VM-configuratie met Get-AzureRmVM ophalen, configuratie-instellingen op het object VM wijzigen en vervolgens deze opdracht uitvoert.
Een gegevensschijf toevoegen aan een VM | [Toevoegen-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - VM $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"naam"disk_name"- VhdUri" - LUN #-Caching ReadWrite - DiskSizeinGB # - CreateOption leegmaken<BR></BR><BR></BR>Gebruik Get-AzureRmVM om het object VM. Geef het aantal LUN en de grootte van de schijf. Update-AzureRmVM om de configuratiewijzigingen voor VM uitvoeren. De schijf die u toevoegt, is niet geïnitialiseerd. Zie [beheren Azure virtuele Machines resourcemanager en PowerShell gebruiken](virtual-machines-windows-ps-manage.md)voor informatie over schijven geïnitialiseerd worden toegevoegd.
Een gegevensschijf verwijderen uit een VM | [Verwijderen-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - VM $vm-naam "disk_name"<BR></BR><BR></BR>Gebruik Get-AzureRmVM om het object VM. Update-AzureRmVM om de configuratiewijzigingen voor VM uitvoeren.
Extensie toevoegen aan een VM | [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-vm_name"locatie"azure_location"- VMName"-naam "extension_name"-Publisher "publisher_name"-Type "extension_type" - TypeHandlerVersion "#. #"-instellingen $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Deze opdracht uitvoeren met de juiste [configuratiegegevens](virtual-machines-windows-extensions-configuration-samples.md) voor de extensie die u wilt installeren.
Verwijder een VM-extensie | [Verwijderen-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-vm_name"naam"extension_name"- VMName"

## <a name="next-steps"></a>Volgende stappen

- Zie de basisstappen voor het maken van een virtuele machine in het [maken van een Windows-VM resourcemanager en PowerShell gebruiken](virtual-machines-windows-ps-create.md).

