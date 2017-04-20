<properties
   pageTitle="Sjablonen met Linux VM extensies Authoring | Microsoft Azure"
   description="Meer informatie over het ontwerpen van Azure resourcemanager sjablonen met uitbreidingen voor Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Azure resourcemanager sjablonen met Linux VM extensies ontwerpen

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Van Azure CLI, voert u de volgende verbindingsopdracht:

      Azure VM extension list

Deze opdracht geeft als resultaat de publisher-naam, de Extensienaam en de versie als volgt te werk:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Deze drie eigenschappen toewijzen aan "publisher", "type" en "typeHandlerVersion" respectievelijk in de bovenstaande codefragment van de sjabloon.

>[AZURE.NOTE]Het is altijd raadzaam de meest recente versie van de extensie gebruiken om de meest recente functionaliteit.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Het schema voor de extensie configuratieparameters identificeren

De volgende stap met het ontwerpen van een sjabloon extensie is de notatie voor het leveren van configuratieparameters identificeren. Een eigen set met parameters die ondersteuning biedt voor elke extensie.

Bekijk voorbeelden van configuraties voor Linux extensies, klikt u op de documentatie voor leest [Linux eExtensions voorbeelden](virtual-machines-linux-extensions-configuration-samples.md).

Raadpleeg de volgende handelingen uit om een volledig voltooid sjabloon met de VM.

[Aangepast script-extensie op een VM Linux](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Na het ontwerpen van de sjabloon, kunt u deze via de CLI Azure implementeren.
