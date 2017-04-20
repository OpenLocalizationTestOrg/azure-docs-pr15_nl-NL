<properties
    pageTitle="Leg een afbeelding VM van generalized Azure VM vast | Microsoft Azure"
    description="Meer informatie over het vastleggen van de afbeelding van een VM uit een generalized Azure VM gemaakt in het implementatiemodel resourcemanager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Hoe u een afbeelding VM uit een generalized Azure VM vastleggen


In dit artikel leest u hoe Azure PowerShell gebruiken om te maken van een afbeelding van een generalized Azure VM. U kunt de afbeelding vervolgens gebruiken om te maken van een andere VM. De afbeelding bevat de schijf OS en de gegevensschijven die zijn gekoppeld aan de virtuele machine. De afbeelding niet de resources virtueel netwerk opgenomen, zodat u nodig hebt voor het instellen van deze resources wanneer u de nieuwe VM maakt. 


## <a name="prerequisites"></a>Vereisten voor

- U moet al zijn [de VM generalized](virtual-machines-windows-generalize-vhd.md). Een VM noemt Hiermee verwijdert u alle persoonlijke gegevens van uw account, onder andere en zorgt ervoor dat de computer moet worden gebruikt als een afbeelding.

- Moet u beschikken over Azure PowerShell versie 1.0.x of hoger zijn geïnstalleerd. Als u PowerShell al dat nog niet hebt geïnstalleerd, leest u [hoe installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor installatiestappen.


## <a name="log-in-to-azure-powershell"></a>Meld u aan bij Azure PowerShell

1. Open Azure PowerShell en meld u aan bij uw Azure-account.

    ```powershell
    Login-AzureRmAccount
    ```

    Er wordt een pop-upvenster geopend voor u uw referenties Azure-account invoeren.

2. Het abonnement id's voor uw beschikbaar abonnementen ophalen.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Stel het juiste abonnement met de abonnement-ID.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>De VM opheffen en de status naar generalized instellen       

1. De bronnen VM toewijzing.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    De *Status* voor het VM in de portal van Azure gewijzigd van **gestopt** op **gestopt (opgeheven)**.

2. De status van de virtuele machine instellen op **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Controleer de status van VM. De sectie **OSState/generalized** voor VM moet de **DisplayStatus** ingesteld op **VM generalized**hebben.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>De afbeelding maken 

1. De afbeelding VM naar de bestemming opslag container met deze opdracht kopiëren. De afbeelding is gemaakt in hetzelfde opslag-account als de oorspronkelijke virtuele machine. De `-Path` parameter een kopie van de JSON-sjabloon lokaal opslaan. De `-DestinationContainerName` parameter is de naam van de container die u wilt houdt u uw afbeeldingen. Als de container niet bestaat, is het voor u gemaakt.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    U kunt de URL van uw afbeelding krijgen van de JSON-bestandssjabloon. Ga naar de **resources** > **storageProfile** > **osDisk** > **afbeelding** > **uri** sectie voor het volledige pad van de afbeelding. De URL van de afbeelding eruit: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    U kunt ook controleren of de URI in de portal. De afbeelding wordt gekopieerd naar een container met de naam **systeem** in uw account opslag. 


## <a name="next-steps"></a>Volgende stappen

- Nu kunt u [een VM van de afbeelding te maken](virtual-machines-windows-create-vm-generalized.md).

