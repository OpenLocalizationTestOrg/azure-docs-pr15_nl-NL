<properties
    pageTitle="Een VM Linux verplaatsen | Microsoft Azure"
    description="Een VM Linux verplaatsen naar een andere Azure abonnement of de resource groep in het implementatiemodel resourcemanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Een VM Linux naar een ander abonnement of de resource groep verplaatsen

In dit artikel begeleidt u bij het verplaatsen van een Linux VM tussen resourcegroepen of abonnementen. Een VM verplaatsen tussen abonnementen kan handig zijn als u een VM in een personal-abonnement hebt gemaakt en nu wilt verplaatsen naar het abonnement van uw bedrijf.

> [AZURE.NOTE] Nieuwe resource-id's worden gemaakt als onderdeel van de verplaatsen. Zodra de VM is verplaatst, moet u uw hulpprogramma's en scripts die de nieuwe resource-id's bijwerken. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Gebruik de CLI Azure een VM verplaatsen 

Als u wilt verplaatsen met succes een VM, moet u de VM en alle bijbehorende ondersteunende resources verplaatsen. Gebruik de opdracht **azure groep weergeven** voor een overzicht van alle resources in een resourcegroep en hun id's. Naar de uitvoer van deze opdracht pipe naar een bestand, zodat u kunt kopiëren en plak de id's in nieuwere opdrachten is het handig.

    azure group show <resourceGroupName>

Als u wilt een VM en de bijbehorende bronnen verplaatsen naar een andere resourcegroep, gebruikt u de opdracht CLI **azure resource verplaatsen** . Het volgende voorbeeld ziet u hoe u een VM en de meest voorkomende bronnen die er moeten verplaatsen. We gebruiken de parameter **-i** en geven in een lijst door komma's gescheiden (zonder spaties) van id's voor de resources om te verplaatsen.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Als u de VM en de bijbehorende bronnen verplaatsen naar een ander abonnement wilt, voegt u de **--bestemming-subscriptionId & #60; destinationSubscriptionID & #62;** -parameter voor het opgeven van het abonnement bestemming.

Als u vanuit de opdrachtprompt op een Windows-computer werkt, u moet toevoegen een **$** vóór de namen van variabelen wanneer u ze declareren. Dit is niet nodig in Linux.

U wordt gevraagd om te bevestigen dat u wilt verplaatsen van de opgegeven resource. Typ **j** om te bevestigen dat u wilt verplaatsen, de resources.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten resources verplaatsen tussen resourcegroepen en abonnementen. Zie de [bronnen die u wilt het abonnement of nieuwe resourcegroep verplaatsen](../resource-group-move-resources.md)voor meer informatie.    