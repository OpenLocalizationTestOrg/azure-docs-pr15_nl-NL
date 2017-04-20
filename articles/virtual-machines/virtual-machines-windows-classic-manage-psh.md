<properties
   pageTitle="Uw virtuele machines beheren met behulp van Azure PowerShell | Microsoft Azure"
   description="Informatie over opdrachten die u gebruiken kunt om taken bij het beheren van uw virtuele machines te automatiseren."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Uw virtuele machines beheren met behulp van Azure PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Veel taken die u elke dag voor het beheren van uw VMs kunnen worden geautomatiseerd met Azure PowerShell-cmdlets. In dit artikel krijgt u voorbeeld opdrachten voor eenvoudigere taken en koppelingen naar artikelen met de opdrachten voor meer complexe taken weergeven.

>[AZURE.NOTE] Als u dit nog niet hebt geïnstalleerd en Azure PowerShell nog geconfigureerd, kunt u de instructies in het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)verkrijgen.

## <a name="how-to-use-the-example-commands"></a>Het gebruik van de voorbeeld-opdrachten
U moet enkele van de tekst in de opdrachten te vervangen door tekst die geschikt is voor uw omgeving. De < en > symbolen aangeven u nodig hebt voor het vervangen van tekst. Wanneer u de tekst vervangt, de symbolen verwijderen, maar de aanhalingstekens, blijft in.

## <a name="get-a-vm"></a>Een VM ophalen
Dit is een eenvoudige taak die u vaak gebruikt. Gebruik deze informatie over een VM, taken uitvoeren op een VM of krijgen weergeven in een variabele op te slaan.

Als u informatie over de VM, deze opdracht, alles in de aanhalingstekens, inclusief vervangen uitvoert de < en > tekens:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Als u wilt de uitvoer opslaan in een variabele $vm, voert u het:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Meld u aan bij een VM op basis van Windows

Deze opdrachten uitvoeren:

>[AZURE.NOTE] U kunt de naam van de virtuele machine en cloud-service krijgen van de weergave van de opdracht **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Een VM stoppen

Deze opdracht uitvoert:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Deze parameter gebruiken om de virtuele IP-(VIP) van de cloudservice geval het de laatste VM in die cloudservice is. <br><br> Als u de parameter StayProvisioned gebruikt, worden u nog steeds gefactureerd voor VM.

## <a name="start-a-vm"></a>Een VM starten

Deze opdracht uitvoert:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Deze taak is een paar stappen vereist. Gebruik eerst de *** toevoegen-cmdlet AzureDataDisk *** de schijf toevoegen aan het object $vm. Vervolgens kunt u **Update-AzureVM** cmdlet de configuratie van VM bijwerken.

U moet ook beslissen of u een nieuwe schijf of een bestand met gegevens koppelt. De opdracht Hiermee maakt u het bestand VHD en gekoppeld voor een nieuwe schijf.

Als u wilt een nieuwe schijf hebt toegevoegd, kunt u deze opdracht uitvoert:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Als u wilt een bestaande gegevensschijf hebt toegevoegd, kunt u deze opdracht uitvoert:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Als u wilt bijvoegen gegevensschijven uit een bestaande VHD-bestand in blobopslag, moet u deze opdracht uitvoert:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Maken van een VM op basis van Windows

Als u wilt een nieuwe Windows gebaseerde virtuele machine maken in Azure wordt aangegeven, volgt u de instructies in [Azure PowerShell gebruiken om te maken en vooraf configureren op basis van Windows virtuele machines](virtual-machines-windows-classic-create-powershell.md). In dit onderwerp stappen u bij het maken van een Azure PowerShell opdracht dat instellen een VM op basis van Windows die kan vooraf worden geconfigureerd wordt gemaakt:

- Met lidmaatschap van Active Directory-domein.
- Met extra schijven.
- Als lid van een bestaande taakverdeling instellen.
- Een statische IP-adres.
