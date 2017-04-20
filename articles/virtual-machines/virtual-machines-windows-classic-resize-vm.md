<properties
    pageTitle="Formaat van een klassieke Windows VM | Microsoft Azure"
    description="Het formaat van een virtuele Windows-computer die is gemaakt in het implementatiemodel klassieke, via Azure Powershell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Formaat van een Windows-VM gemaakt in het implementatiemodel klassieke

In dit artikel leest u hoe u het formaat van een Windows-VM, die is gemaakt in de klassieke implementatiemodel via Azure Powershell.

Wanneer u bepaalt de mogelijkheid om het formaat van een VM te, zijn er twee concepten die de aanbod van formaten beschikbaar om het formaat van de virtuele machine te bepalen. Het eerste concept is de regio waarin uw VM wordt geïmplementeerd. De lijst met VM-grootte die beschikbaar zijn in regio is onder het tabblad Services van de webpagina Azure regio's. Het tweede concept is de fysieke hardware die momenteel hostingprovider uw VM. De fysieke servers hostingprovider VMs worden gegroepeerd in clusters algemene fysieke hardware. De methode van het wijzigen van de grootte van een VM verschilt afhankelijk van als de gewenste nieuwe VM grootte wordt ondersteund door de hardware cluster momenteel hostingprovider de VM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [een VM gemaakt in het implementatiemodel resourcemanager formaat](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Uw account toevoegen

Azure PowerShell voor het werken met klassieke Azure resources, moet u configureren. Volg de onderstaande stappen voor het configureren van Azure PowerShell voor het beheren van klassieke resources.

1. Typ op de PowerShell-prompt `Add-AzureAccount` en klikt u op **Enter**. 
2. Typ in het e-mailadres dat is gekoppeld aan uw Azure abonnement en klik op **Doorgaan**. 
3. Typ in het wachtwoord voor uw account. 
4. Klik op **aanmelden**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Formaat in hetzelfde hardware cluster

Als u een VM beschikbaar in de hardware cluster de VM hostingprovider maken, moet u de volgende stappen uitvoeren.

1. Voer de volgende PowerShell-opdracht voor een overzicht van de beschikbaar in de hardware cluster hostingservice de cloud waarin de VM VM-grootte.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Voer de volgende opdrachten om het formaat van de VM aan.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Het formaat wijzigen op een nieuwe hardware cluster

Om het formaat van een VM maken niet beschikbaar in de hardware cluster hostingprovider de VM, de cloud te service en alle VMs in de cloudservice moeten opnieuw worden ingesteld. Elke cloudservice wordt gehost op een cluster één hardware, zodat alle VMs in de cloudservice moet een grootte die wordt ondersteund op een cluster hardware. De volgende stappen wordt beschreven hoe de grootte van een VM wijzigen door te verwijderen en klik vervolgens opnieuw te maken van de cloudservice.

1. Voer de volgende PowerShell-opdracht voor een overzicht van de grootte VM beschikbaar in de regio. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Noteer alle configuratie-instellingen voor elke VM in de cloudservice waarin de VM worden aangepast. 
3. Verwijder alle VMs in de cloudservice de optie moeten worden bewaard de schijven voor elke VM te selecteren.
4. Maak opnieuw de VM worden aangepast met de gewenste VM grootte.
5. Maak opnieuw alle andere VMs die zich in de cloudservice met een grootte VM beschikbaar in de hardware cluster nu de hostingservice van de cloud.

Een voorbeeldscript voor het verwijderen en opnieuw te maken van een cloudservice de grootte van een nieuwe VM gebruiken vindt u [hier](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Volgende stappen

- [Formaat van een VM gemaakt in het implementatiemodel resourcemanager](virtual-machines-windows-resize-vm.md).