<properties
   pageTitle="Problemen met Linux VM extensie oplost | Microsoft Azure"
   description="Meer informatie over het oplossen van Azure Linux VM extensie fouten"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Problemen met Azure Linux VM extensie oplost

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Status van de extensie bekijken
Azure resourcemanager sjablonen kunnen worden uitgevoerd vanuit de CLI Azure. Zodra de sjabloon wordt uitgevoerd, kan de status van de extensie van Azure Resource Explorer of de hulpmiddelen voor de opdrachtregel worden weergegeven.

Hier volgt een voorbeeld:

Azure CLI:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Probleemoplossing Extenson fouten:

### <a name="re-running-the-extension-on-the-vm"></a>De extensie opnieuw uit te voeren op de VM

Als u scripts op de VM aangepast Script extensie gebruiken uitvoert, kunt u soms uitvoeren in een fout waar VM is gemaakt, maar het script is mislukt. Klik onder deze voorwaarden is de aanbevolen wijze herstellen uit deze fout verwijderen van de extensie en opnieuw de sjabloon opnieuw uitvoeren.
Opmerking: voortaan deze functionaliteit verbeterd zouden kunnen worden als u wilt verwijderen dat u voor het verwijderen van de extensie.

#### <a name="remove-the-extension-from-azure-cli"></a>De extensie van Azure CLI verwijderen

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Waar "publsher-naam" overeenkomt met de extensietype uit de resultaten van "get-exemplaar-weergave van de azure vm" en is de naam van de resource toestelnummer van de sjabloon

Zodra de extensie is verwijderd, kan de sjabloon opnieuw worden uitgevoerd voor het uitvoeren van scripts op de VM.
