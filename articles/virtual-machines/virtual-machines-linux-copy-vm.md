<properties
    pageTitle="Een kopie van uw Azure Linux VM maken | Microsoft Azure"
    description="Informatie over het maken van een kopie van uw Linux Azure virtuele machines in het implementatiemodel resourcemanager"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Een kopie van een Linux virtuele machine waarop Azure maken


In dit artikel leest u hoe u een kopie van uw Azure virtuele machine (VM) waarop Linux met het implementatiemodel resourcemanager maakt. Eerst u via het besturingssysteem en gegevensschijven kopiëren naar een nieuwe container klikt en vervolgens de resources van het netwerk instellen en de nieuwe virtuele machine maken.

U kunt ook [uploadt en maakt een VM van aangepaste schijf afbeelding](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u de volgende vereisten voldoet aan voordat u de stappen:

- U hebt de [Azure CLI] (.. / xplat-cli-install.md) gedownload en geïnstalleerd op uw computer. 

- U moet ook bepaalde gegevens over uw bestaande Azure Linux VM:

| Informatie van gegevensbronnen VM | Waar kunt u deze ophalen |
|------------|-----------------|
| VM naam | `azure vm list` |
| De naam van de resourcegroep | `azure vm list` |
| Locatie | `azure vm list` |
| Opslagaccountnaam | `azure storage account list -g <resourceGroup>` |
| De containernaam van de | `azure storage container list -a <sourcestorageaccountname>` |
| Bron VM VHD-bestandsnaam | `azure storage blob list --container <containerName>` |



- U moet bepaalde keuzen over uw nieuwe VM maken:   <br> -De naam van de container   <br> VM - naam   <br> VM - grootte   <br> de naam van de - vNet   <br> -SubNet naam   <br> De naam van de - IP   <br> NIC - naam
    

## <a name="login-and-set-your-subscription"></a>Aanmelden en instellen van uw abonnement

1. Meld u aan de CLI.
        
        azure login

2. Zorg ervoor dat in de modus van bronbeheer.
    
        azure config mode arm

3. Stel het juiste abonnement. U kunt 'azure-Accountlijst' gebruiken om al uw abonnementen weer te geven.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>De VM stoppen 

Stoppen en de bron VM toewijzing. U kunt 'azure vm lijst' gebruiken om een lijst van alle de VMs in uw abonnement en hun resource groepsnamen.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopieer de VHD


U kunt de VHD uit de bron-opslag kopiëren met de bestemming via de `azure storage blob copy start`. In dit voorbeeld gaan we de VHD naar hetzelfde opslag-account, maar een andere container kopiëren.

Als u wilt de VHD kopiëren naar een andere container in hetzelfde opslag-account, typ:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Het virtuele netwerk instellen voor uw nieuwe VM

U een virtueel netwerk- en NIC instellen voor uw nieuwe VM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>De nieuwe VM maken 

Nu kunt u een VM vanuit uw geüploade virtuele schijf [met een sjabloon van resource manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) of via de CLI maken door het opgeven van de URI naar de gekopieerde schijf door te typen:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Volgende stappen

Zie informatie over het gebruik van Azure CLI voor het beheren van uw nieuwe virtuele machine, [Azure CLI opdrachten voor het Azure Resource Manager](azure-cli-arm-commands.md).
