<properties
    pageTitle="Een Windows VHD uploaden voor resourcemanager | Microsoft Azure"
    description="Leer hoe u een virtuele Windows-computer VHD vanuit on-premises naar Azure, met het implementatiemodel resourcemanager uploaden. U kunt een schijf uit op een algemene of een gespecialiseerde VM uploaden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Een Windows VHD vanuit een on-premises VM naar Azure uploaden 


In dit artikel leest u hoe maken en uploaden van een Windows virtuele harde schijf (VHD) moet worden gebruikt bij het maken van een Vm Azure. U kunt een VHD uit een generalized VM of een gespecialiseerde VM uploaden. 

Zie voor meer informatie over schijven en VHD's in Azure wordt aangegeven [over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>De VM voorbereiden 

U kunt zowel generalized en gespecialiseerde VHD's uploaden naar Azure. Elk type is vereist dat u de VM voordat u begint voorbereiden.

- **Generalized VHD** - een generalized VHD heeft al uw persoonlijk accountgegevens verwijderd met Sysprep. Als u wilt de VHD als een afbeelding gebruiken om te maken van nieuwe VMs uit, voert u de volgende stappen uit:
    - [Een Windows-VHD uploaden naar Azure voorbereiden](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Generalize de virtuele machine met behulp van Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Gespecialiseerde VHD** - een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM. Als u wilt gebruiken de VHD als-te maken van een nieuwe VM, controleren de volgende stappen worden uitgevoerd. 
    - [Een Windows-VHD uploaden naar Azure voorbereiden](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Niet doen** generalize de VM met Sysprep.
    - Verwijder eventuele Gast virtualization hulpmiddelen en agenten die zijn geïnstalleerd op de VM (dat wil zeggen VMware hulpmiddelen voor).
    - Controleer of dat de VM is geconfigureerd voor het IP-adres en de DNS-instellingen via DHCP halen. Dit zorgt ervoor dat de server een IP-adres binnen de VNet verkrijgt bij het starten van. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure

Als u PowerShell versie 1.4 of boven de geïnstalleerde, Lees [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)nog geen hebt.

1. Open Azure PowerShell en meld u aan bij uw Azure-account. Er wordt een pop-upvenster geopend voor u uw referenties Azure-account invoeren.

    ```powershell
    Login-AzureRmAccount
    ```


2. Het abonnement id's voor uw beschikbaar abonnementen ophalen.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Stel het juiste abonnement met de abonnement-ID. Vervang `<subscriptionID>` met de ID van het juiste abonnement.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>De opslag-account aanschaffen

Moet u een account opslagruimte in Azure wordt aangegeven voor de opslag van de geüploade VM-afbeelding. U kunt een bestaand account voor de opslag gebruiken of een nieuw account te maken. 

Als u wilt weergeven van de beschikbare opslagruimte-accounts, typ:

```powershell
Get-AzureRmStorageAccount
```

Als u een bestaand opslag-account gebruikt wilt, gaat u naar de sectie [de VM afbeelding uploaden](#upload-the-vm-vhd-to-your-storage-account) .

Als u moet een opslag-account maken, als volgt te werk:

1. U moet de naam van de resourcegroep waarop de opslag-account moet worden gemaakt. Als u alle resourcegroepen die in uw abonnement wilt, typ:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Een resourcegroep maken met de naam **myResourceGroup** in de regio **West Amerikaans** , type:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Een benoemde **mystorageaccount** in deze resourcegroep met behulp van de cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) opslag-account maken:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Geldige waarden voor - SkuName zijn:

    - **Standard_LRS** - lokaal redundante opslag. 
    - **Standard_ZRS** - Zone redundante opslag.
    - **Standard_GRS** - geografische redundante opslag. 
    - **Standard_RAGRS** - leestoegang geografische redundante opslag. 
    - **Premium_LRS** - Premium lokaal redundante opslag. 



## <a name="upload-the-vhd-to-your-storage-account"></a>De VHD uploaden naar uw account opslag

Gebruik de cmdlet [Toevoegen-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) voor het uploaden van de afbeelding naar een container in uw account opslag. In dit voorbeeld uploadt u het bestand **myVHD.vhd** uit `"C:\Users\Public\Documents\Virtual hard disks\"` met een storage rekening met de naam **mystorageaccount** in de groep **myResourceGroup** resource. Het bestand in de container met de naam **mycontainer** worden geplaatst en wordt de nieuwe bestandsnaam **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Als dit lukt, krijgt u een reactie die er ongeveer als volgt:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Afhankelijk van uw netwerkverbinding en de grootte van uw VHD-bestand, deze opdracht kan enige tijd duren om te voltooien


## <a name="next-steps"></a>Volgende stappen

- [Een VM in Azure uit een generalized VHD maken](virtual-machines-windows-create-vm-generalized.md)
- [Een VM in Azure wordt aangegeven uit een gespecialiseerde VHD maken](virtual-machines-windows-create-vm-specialized.md) door te koppelen als een schijf OS wanneer u een nieuwe VM maakt.


