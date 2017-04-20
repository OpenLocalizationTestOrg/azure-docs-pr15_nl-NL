<properties
    pageTitle="Toets kluis instellen voor virtuele machines in Azure resourcemanager | Microsoft Azure"
    description="Hoe u de toets kluis instellen voor gebruik met een resourcemanager Azure virtuele machines."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Toets kluis voor virtuele machines in Azure resourcemanager instellen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel

Geheimen/certificaten zijn Azure resourcemanager gestapelde gezien als resources die worden verstrekt door de provider van de resource van toets kluis. Zie voor meer informatie over sleutel kluis [Wat Azure-toets kluis is?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. In de volgorde voor sleutel kluis moet worden gebruikt met resourcemanager Azure virtuele machines, moet de eigenschap **EnabledForDeployment** op de toets kluis zijn ingesteld op waar. U kunt dit doen in verschillende-clients.
>
>2. De toets kluis moet worden gemaakt in dezelfde abonnement en locatie als de virtuele Machine.

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShell gebruiken voor het instellen van toets kluis
Zie voor informatie over het maken van een belangrijke kluis via PowerShell [aan de slag met Azure toets kluis](../key-vault/key-vault-get-started.md#vault).

Voor nieuwe belangrijke kluizen, kunt u deze PowerShell-cmdlet gebruiken:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Voor bestaande belangrijke kluizen, kunt u deze PowerShell-cmdlet gebruiken:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Ons CLI toets kluis instellen
Zie een belangrijke kluis maken met behulp van een interface voor de opdrachtregel (CLI) [beheren toets kluis CLI gebruiken](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Voor CLI moet u de belangrijkste kluis maken voordat u de implementatie-beleid toewijzen. U kunt dit doen met behulp van de volgende opdracht uit:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Sjablonen gebruiken om de toets kluis instellen
Terwijl u een sjabloon gebruikt, moet u de `enabledForDeployment` eigenschap `true` voor de resource toets kluis.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Zie voor andere opties die u configureren kunt wanneer u een belangrijke kluis maken met behulp van sjablonen [maken een belangrijke kluis](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
