<properties
   pageTitle="Sjablonen met Windows VM extensies Authoring | Microsoft Azure"
   description="Meer informatie over het ontwerpen van Azure resourcemanager sjablonen met uitbreidingen voor Windows VMs"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Azure resourcemanager sjablonen met Windows VM extensies ontwerpen

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Van Azure PowerShell, moet u de volgende Azure PowerShell-cmdlet uitvoeren:

      Get-AzureVMAvailableExtension


Deze cmdlet retourneert de publisher-naam, de Extensienaam van de en versie als volgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Deze drie eigenschappen toewijzen aan "publisher", "type" en "typeHandlerVersion" respectievelijk in de bovenstaande codefragment van de sjabloon.

>[AZURE.NOTE]Het is altijd raadzaam de meest recente versie van de extensie gebruiken om de meest recente functionaliteit.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Het schema voor de extensie configuratieparameters identificeren

De volgende stap met het ontwerpen van een sjabloon extensie is de notatie voor het leveren van configuratieparameters identificeren. Een eigen set met parameters die ondersteuning biedt voor elke extensie.

Bekijk voorbeelden van configuraties voor uitbreidingen voor Windows, raadpleegt u [Windows extensies voorbeelden](virtual-machines-windows-extensions-configuration-samples.md).


Raadpleeg de volgende handelingen uit om een volledig voltooid sjabloon met VM uitbreidingen.

[Aangepast Script extensie op een Windows-VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Na het ontwerpen van de sjabloon, kunt u deze via Azure PowerShell implementeren.
