<properties
   pageTitle="Een Windows-VM maken met meerdere NIC | Microsoft Azure"
   description="Informatie over het maken van een Windows-VM met meerdere NIC's die zijn bijgevoegd bij deze via Azure PowerShell of resourcemanager sjablonen."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Een Windows-VM maken met meerdere NIC 's
U kunt een virtuele machine (VM) maken in Azure wordt aangegeven met meerdere virtueel netwerk-interfaces (NIC) gekoppeld. Een gebruikelijk zou zijn om met verschillende subnetten voor front-end en back-end-connectiviteit of een specifiek voor een oplossing controleren of back-netwerk. In dit artikel vindt snelle opdrachten voor het maken van een VM met meerdere NIC's die zijn gekoppeld. Voor gedetailleerde informatie, waaronder over het maken van meerdere NIC binnen uw eigen PowerShell-scripts, Zie voor meer informatie over het [implementeren van meerdere NIC's VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschillende [grootten VM](virtual-machines-windows-sizes.md) een verschillend aantal NIC wordt ondersteund, dus het formaat van uw VM dienovereenkomstig gewijzigd.

>[AZURE.WARNING] Wanneer u een VM maken - u NIC niet aan een bestaande VM toevoegen, moet u meerdere NIC's koppelen. U kunt [een VM op basis van de oorspronkelijke virtuele schijven maken](virtual-machines-windows-vhd-copy.md) en meerdere NIC maken terwijl u de VM implementeren.

## <a name="create-core-resources"></a>Belangrijkste resources maken
Zorg ervoor dat u de [meest recente Azure PowerShell geïnstalleerd en geconfigureerd hebt](../powershell-install-configure.md). Meld u aan bij uw Azure-account:

```powershell
Login-AzureRmAccount
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUs` locatie:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Maak een account opslagruimte voor uw VMs. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Virtuele netwerk en subnetten maken
Twee virtueel netwerksubnetten - definiëren één telefoonnummer voor de front-verkeer is toegestaan en één voor back-enddatabase verkeer. Het volgende voorbeeld wordt gedefinieerd twee subnetten, met de naam `mySubnetFrontEnd` en `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Maak uw virtueel netwerk en subnetten. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Meerdere NIC's maken
Maak twee NIC's, één NIC subnet, de front- en één NIC koppelen aan het subnet back-enddatabase. Het volgende voorbeeld wordt twee NIC's, met de naam `myNic1` en `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Maakt u gewoonlijk ook een [beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) of [de belasting voor documentconversies](../load-balancer/load-balancer-overview.md) om te helpen beheren en verkeer verdelen over uw VMs. Het artikel voor [meer gedetailleerde multi-NIC VM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) helpt u bij het maken van een beveiligingsgroep netwerk en NIC's toe te wijzen.


## <a name="create-the-virtual-machine"></a>De virtuele machine maken
Nu kunt u beginnen met het maken van uw configuratie VM. De grootte van elke VM geldt een limiet voor het totale aantal NIC's die u aan een VM toevoegen kunt. Meer informatie over [Windows VM grootte](virtual-machines-windows-sizes.md). 

Stel uw referenties VM eerst naar de `$cred` variabele als volgt:

```powershell
$cred = Get-Credential
```

Het volgende voorbeeld wordt een benoemde VM gedefinieerd `myVM` en gebruikt u de grootte van een VM die ondersteuning biedt voor maximaal twee NIC's (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Maak de rest van het bestand VM config. Het volgende voorbeeld wordt een Windows Server 2012 R2 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

De twee netwerkadapters die u eerder hebt gemaakt als bijlage toevoegen:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

De opslag en virtuele schijf configureren voor uw nieuwe VM:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Ten slotte maakt u een VM:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC met resourcemanager sjablonen maken
Declaratieve JSON-bestanden Azure resourcemanager sjablonen gebruiken om te definiëren van uw omgeving. Hier vindt u een [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Resourcemanager sjablonen kunnen u meerdere exemplaren van een resource tijdens de implementatie, zoals het maken van meerdere NIC's maken. U *kopiëren* gebruiken om op te geven hoe vaak maken:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Meer informatie over het [maken van meerdere exemplaren via *kopiëren*](../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` vervolgens een nummer toevoegen aan de naam van een resource, waarmee u maken `myNic1`, `MyNic2`, enz. Hier volgt een voorbeeld van de indexwaarde toe te voegen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Een volledig voorbeeld van het [maken van meerdere NIC resourcemanager sjablonen gebruiken](../virtual-network/virtual-network-deploy-multinic-arm-template.md), kunt u lezen.

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat moet worden gereviseerd [Windows VM grootte](virtual-machines-windows-sizes.md) als u probeert te maken van een VM met meerdere NIC's. Let op het maximum aantal NIC elke grootte VM ondersteunt. 

Vergeet niet dat u extra NIC niet toevoegen aan een bestaande VM, moet u de NIC's maken wanneer u de VM implementeert. Wees voorzichtig bij het plannen van uw implementaties om ervoor te zorgen dat u de vereiste netwerkverbinding vanaf het begin hebt.