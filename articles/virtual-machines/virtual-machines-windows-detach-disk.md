<properties
    pageTitle="Een gegevensschijf uit een Windows-VM loskoppelen | Microsoft Azure"
    description="Leer hoe u een gegevensschijf uit een virtuele machine in Azure wordt aangegeven met het implementatiemodel resourcemanager loskoppelen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Het ontkoppelen van een gegevensschijf uit een virtuele Windows-computer


Wanneer u een schijf met gegevens die gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Dit de schijf verwijdert uit de virtuele machine, maar deze niet verwijderen uit de opslag. 

> [AZURE.WARNING] Als u een schijf loskoppelen deze niet automatisch verwijderd. Als u zich hebt geabonneerd op Premium opslag, blijft u gelden de opslagruimte tarieven voor de schijf. Raadpleeg [prijzen en facturering wanneer u een Premium-opslag gebruikt](../storage/storage-premium-storage.md#pricing-and-billing)voor meer informatie. 

Als u de bestaande gegevens opnieuw op de schijf gebruiken wilt, kunt u deze opnieuw bij dezelfde virtuele machine, of een ander abonnement.  


## <a name="detach-a-data-disk-using-the-portal"></a>Een schijf met gegevens met behulp van de portal loskoppelen

1. Selecteer in de portal hub **virtuele Machines**.

2. Selecteer de virtuele machine met de gegevensschijf die u wilt loskoppelen en klik vervolgens op **alle instellingen**.

3. Selecteer in het blad **Instellingen** **schijven**.

4. Selecteer de gegevensschijf die u wilt loskoppelen in het blad **schijven** .

5. Klik in het blad voor de gegevensschijf, op **ontkoppelen**.


    ![Schermafbeelding van de knop ontkoppelen.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

De schijf blijft in opslag, maar niet meer wordt gekoppeld aan een virtuele machine.


## <a name="detach-a-data-disk-using-powershell"></a>Een data-schijf via PowerShell loskoppelen

In dit voorbeeld wordt de eerste opdracht de virtuele machine met de naam **MyVM07** in met de cmdlet Get-AzureRmVM **RG11** resourcegroep. De opdracht opgeslagen de virtuele machine in de variabele **$VirtualMachine** . 

De tweede opdracht Hiermee verwijdert u de gegevensschijf met DataDisk3 de naam van de virtuele machine. 

De laatste opdracht werkt de status van de virtuele machine het voltooien van de gegevensschijf verwijderen.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Zie voor meer informatie [Verwijderen AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Volgende stappen

Als u de gegevensschijf hergebruiken wilt, kunt u alleen [toevoegen aan een andere VM](virtual-machines-windows-attach-disk-portal.md)
