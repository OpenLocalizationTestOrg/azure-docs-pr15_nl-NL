<properties
   pageTitle="Gebruik desgewenst provinciale configuratie met VM schaal Sets | Microsoft Azure"
   description="Met VM kleurenschaal Sets met de Azure DSC-extensie"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Met VM kleurenschaal Sets met de Azure DSC-extensie

[VM schaal Sets (VMSS)](virtual-machine-scale-sets-overview.md) kan worden gebruikt met de extensie-handler [Azure gewenst staat configuratie (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS biedt een manier om te implementeren en beheren van grote aantallen virtuele machines en schaal kunt elastically in-en uitfaden in antwoord op laden. DSC wordt gebruikt voor het configureren van de VMs als ze online komt zodat ze de software productie worden uitgevoerd.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Verschillen tussen het implementeren naar VM en VMSS

De structuur van de onderliggende sjabloon voor VMSS verschilt enigszins van een enkel VM. Een enkel VM implementeert name extensies onder het knooppunt "virtualMachines". Er is een vermelding van het type "extensies" waar DSC wordt toegevoegd aan de sjabloon

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Een knooppunt VMSS heeft een sectie 'Eigenschappen' met de "VirtualMachineProfile", "extensionProfile"-kenmerk. DSC wordt onder "extensies" toegevoegd

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Gedrag voor VMSS

Het gedrag voor VMSS is gelijk aan het gedrag voor een enkele VM. Wanneer u een nieuwe VM maakt, wordt deze automatisch met de extensie DSC ingericht. Als een nieuwere versie van de WMF is vereist door de extensie, de VM opnieuw wordt opgestart voordat binnenkort online. Zodra deze is online, de DSC configuratie .zip-downloads en klik op de VM inrichten. Meer informatie vindt u in [Het overzicht van Azure DSC extensie](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Volgende stappen ##
Bekijk de [resourcemanager Azure-sjabloon voor de extensie DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Leer hoe de [DSC extensie referenties veilig worden verwerkt](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Zie [Inleiding tot de configuratie van Azure staat gewenst extensie handler](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md)voor meer informatie over de Azure DSC extensie handler. 

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 


