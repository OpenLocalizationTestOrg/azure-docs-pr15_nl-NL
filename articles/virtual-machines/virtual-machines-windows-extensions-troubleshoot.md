<properties
   pageTitle="Problemen met Windows VM extensie oplost | Microsoft Azure"
   description="Meer informatie over het oplossen van Azure Windows VM extensie fouten"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problemen met Azure Windows VM extensie oplost

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Status van de extensie bekijken
Azure resourcemanager sjablonen kunnen worden uitgevoerd vanuit Azure PowerShell. Zodra de sjabloon wordt uitgevoerd, kan de status van de extensie van Azure Resource Explorer of de hulpmiddelen voor de opdrachtregel worden weergegeven.

Hier volgt een voorbeeld:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Hier ziet u de voorbeelduitvoer:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Problemen met extensie oplost

### <a name="re-running-the-extension-on-the-vm"></a>De extensie opnieuw uit te voeren op de VM

Als u scripts op de VM aangepast Script extensie gebruiken uitvoert, kunt u soms uitvoeren in een fout waar VM is gemaakt, maar het script is mislukt. Klik onder deze voorwaarden is de aanbevolen wijze herstellen uit deze fout verwijderen van de extensie en opnieuw de sjabloon opnieuw uitvoeren.
Opmerking: voortaan deze functionaliteit verbeterd zouden kunnen worden als u wilt verwijderen dat u voor het verwijderen van de extensie.


#### <a name="remove-the-extension-from-azure-powershell"></a>De extensie van Azure PowerShell verwijderen

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Zodra de extensie is verwijderd, kan de sjabloon opnieuw worden uitgevoerd voor het uitvoeren van scripts op de VM.
