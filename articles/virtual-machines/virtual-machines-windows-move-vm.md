<properties
    pageTitle="Een Windows VM verplaatsen | Microsoft Azure"
    description="Een Windows-VM verplaatsen naar een andere Azure abonnement of de resource groep in het implementatiemodel resourcemanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Een Windows-VM verplaatsen naar een andere Azure abonnement of de resource-groep 

In dit artikel begeleidt u bij het verplaatsen van een Windows-VM tussen resourcegroepen of abonnementen. Verplaatsen tussen abonnementen kan handig zijn als u een VM hebt gemaakt in een personal-abonnement en nu wilt verplaatsen naar het abonnement van uw bedrijf om te gaan met uw werk.

> [AZURE.NOTE] Nieuwe resource-id's wordt gemaakt als onderdeel van de verplaatsen. Zodra de VM is verplaatst, moet u bij uw hulpprogramma's en scripts die de nieuwe resource-id. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Powershell gebruiken om te verplaatsen van een VM

Als u wilt een virtuele machine verplaatsen naar een andere resourcegroep, moet u zorgen dat u ook alle van de afhankelijke bronnen verplaatsen. Als u wilt de cmdlet verplaatsen-AzureRMResource gebruiken, moet u de resourcenaam van de en het type bron. U kunt krijgen van de zoeken-AzureRMResource-cmdlet.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Als u wilt verplaatsen van een VM moeten we meerdere resources verplaatsen. We kunnen alleen afzonderlijke variabelen voor elke resource maken en somt u deze op. In dit voorbeeld bevat de meest van de eenvoudige bronnen voor een VM, maar u kunt meer naar wens toevoegen.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

De om bronnen te verplaatsen naar een ander abonnement, bevatten de parameter **- DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resources. Typ **j** om te bevestigen dat u wilt verplaatsen, de resources.

  
## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie de [bronnen die u wilt het abonnement of nieuwe resourcegroep verplaatsen](../resource-group-move-resources.md)voor meer informatie.    