<properties
   pageTitle="Azure hybride gebruik voordeel voor venster Server | Microsoft Azure"
   description="Leer hoe u de voordelen van uw Windows Server Software Assurance om de on-premises implementatie-licenties aan Azure maximaliseren"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure hybride gebruik voordeel voor Windows Server

U kunt uw on-premises van Windows Server-licenties aan Azure brengen en uitvoeren van Windows Server VMs in Azure wordt aangegeven bij een lagere kosten voor klanten met behulp van Windows Server met Software Assurance. Het voordeel van Azure hybride gebruiken, kunt u Windows Server VMs in Azure uitvoert en alleen voor het tarief weer dat grondtal berekeningscluster krijgen gefactureerd. Zie de [Azure hybride gebruik voordeel licenties pagina](https://azure.microsoft.com/pricing/hybrid-use-benefit/)voor meer informatie. In dit artikel wordt uitgelegd hoe implementeren van Windows Server VMs in Azure wordt aangegeven gebruik van deze licentievoorwaarden korting.

> [AZURE.NOTE] U kunt Azure Marketplace afbeeldingen niet gebruiken om te implementeren van Windows Server VMs gebruik van de Azure-voordeel voor het gebruik van hybride. U moet uw VMs PowerShell of resourcemanager sjablonen gebruiken uw VMs correct te registreren als in aanmerking komt voor kortingen grondtal berekeningscluster implementeren.

## <a name="pre-requisites"></a>Minimumvereisten
Er zijn een paar van de vereisten om te kunnen gebruikmaken van Azure hybride gebruik voordeel voor Windows Server VMs in Azure wordt aangegeven:

- De Azure PowerShell-module geïnstalleerd
- Hebt u uw Windows Server VHD geüpload naar de opslag van Azure

### <a name="install-azure-powershell"></a>Azure PowerShell installeren
Zorg ervoor dat u hebt [installatie en configuratie van de meest recente Azure PowerShell](../powershell-install-configure.md). Zelfs als u implementeren van uw VMs resourcemanager sjablonen gebruiken wilt, moet u nog steeds Azure PowerShell geïnstalleerd om het uploaden van uw Windows Server VHD (Zie de volgende stap).

### <a name="upload-a-windows-server-vhd"></a>Een Windows-Server VHD uploaden

Als u wilt implementeren een Windows Server VM in Azure wordt aangegeven, moet u eerst een VHD met uw grondtal Windows Server opbouwen maken. Deze VHD moet worden correct voorbereid met Sysprep voordat u deze naar Azure uploaden. U kunt [meer informatie over de vereisten van VHD en proces](./virtual-machines-windows-upload-image.md) en [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Back-up van de VM voordat u Sysprep uitvoert. Als u hebt uw VHD voorbereid, uploadt u de VHD naar uw Azure Storage-account gebruikt de `Add-AzureRmVhd` cmdlet als volgt:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server en SharePoint Server Dynamics kunnen ook gebruikmaken van uw Software Assurance-licenties. Nog steeds moet u de afbeelding van de Windows Server door uw toepassingsonderdelen installeren en leveren licentie toetsen dienovereenkomstig gewijzigd en de schijf afbeelding uploaden naar Azure voorbereiden. Bekijk de juiste documentatie voor het uitvoeren van Sysprep met uw-toepassing, zoals [Aandachtspunten voor de installatie van SQL Server met Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) of [maken van een afbeelding in SharePoint Server 2016 verwijzing (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

U kunt ook meer informatie over het [uploaden van de VHD naar Azure proces](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] In dit artikel bevat informatie over het implementeren van Windows Server VMs. U kunt ook Windows-Client VMs implementeren op dezelfde manier. In de volgende voorbeelden, kunt u vervangen `Server` met `Client` correct.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Een VM via PowerShell snel begin implementeren
Wanneer uw Windows Server VM via PowerShell wordt geïmplementeerd, hebt u een extra parameter voor `-LicenseType`. Nadat u uw VHD geüpload naar Azure hebt, maakt u een nieuwe VM met `New-AzureRmVM` en het type licentieverlening als volgt opgeven:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

U kunt [meer gedetailleerde stapsgewijze instructies over het implementeren van een VM in Azure wordt aangegeven via PowerShell lezen](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) onderstaande of lezen van een meer betekenisvolle hulplijn op de verschillende stappen voor het [maken van een Windows-VM resourcemanager en PowerShell gebruiken](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Een VM via resourcemanager implementeren
Binnen de resourcemanager-sjablonen, een extra parameter voor `licenseType` kan worden opgegeven. U vindt meer informatie over [authoring resourcemanager Azure-sjablonen](../resource-group-authoring-templates.md). Nadat u uw VHD geüpload naar Azure hebt, u resourcemanager sjabloon bewerken als u wilt opnemen van het licentietype als onderdeel van de provider berekeningscluster en implementeren van de sjabloon Normaal:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Controleer of uw VM wordt gebruikt door de licentievoorwaarden voordeel
Zodra u hebt uw VM geïmplementeerd via de PowerShell of resourcemanager implementatiemethode, Controleer of het licentietype met `Get-AzureRmVM` als volgt:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

De uitvoer is vergelijkbaar met het volgende:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dit is het tegenovergestelde met de volgende VM geïmplementeerd zonder Azure hybride gebruik voordeel licenties, zoals een geïmplementeerd rechtstreeks vanuit de galerie met Azure VM:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Stapsgewijze instructies PowerShell

De volgende stappen voor gedetailleerde PowerShell weergeven een volledige implementatie van een VM. Hier vindt u meer context de werkelijke cmdlets en de verschillende onderdelen worden gemaakt in [een Windows-VM met resourcemanager en PowerShell maken](./virtual-machines-windows-ps-create.md). U stapsgewijs uw resourcegroep, opslag-account en virtuele netwerken, maken en vervolgens uw VM definiëren en ten slotte maakt uw VM.
 
Eerst veilig referenties te verkrijgen, een locatie en Resourcegroepnaam instellen:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Een openbare IP-adres maken:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Uw subnet, NIC en VNET definiëren:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Naam van uw VM en maak een config VM:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieer uw besturingssysteem:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Uw NIC toevoegen aan de VM:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

De opslag definiëren om te gebruiken:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Upload uw VHD, passende voorbereid, en als bijlage toevoegen aan uw VM voor gebruik:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Ten slotte maakt u uw VM en het type licentieverlening voor het gebruik van Azure hybride gebruik voordeel definiëren:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Volgende stappen

Lees meer over [Azure hybride gebruik voordeel licenties](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Meer informatie over het [gebruik van resourcemanager sjablonen](../azure-resource-manager/resource-group-overview.md).
