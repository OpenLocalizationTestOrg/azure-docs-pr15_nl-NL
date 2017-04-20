<properties 
    pageTitle="Implementeer deze opnieuw Linux virtuele Machines | Microsoft Azure" 
    description="Wordt beschreven hoe implementeer deze opnieuw Linux virtuele machines om SSH verbindingsproblemen te verhelpen." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Implementeer deze opnieuw VM naar nieuwe Azure knooppunt

Als er zijn die tegenover elkaar liggen voor probleemoplossing SSH of toepassing toegang tot een Azure virtuele machine (VM), opnieuw te implementeren de VM mogelijk helpen problemen. Wanneer u een VM Implementeer deze opnieuw, wordt de VM verplaatst naar een nieuw knooppunt binnen de infrastructuur van Azure en deze vervolgens weer in, behoudt al uw configuratieopties en bijbehorende hulpbronnen bevoegdheden. Dit artikel leest u hoe u een VM met Azure CLI of de Azure portal implementeren.

> [AZURE.NOTE] Nadat u een VM Implementeer deze opnieuw, de tijdelijke schijf gaat verloren en dynamische IP-adressen die is gekoppeld aan een virtueel netwerkinterface worden bijgewerkt. 


## <a name="using-azure-cli"></a>Azure CLI gebruiken

Zorg ervoor dat u de [Meest recente Azure CLI geïnstalleerd](../xplat-cli-install.md) op uw computer hebt en u bent in de modus resourcemanager (`azure config mode arm`).

Gebruik de volgende opdracht uit Azure CLI te implementeren van uw VM:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

U kunt de status van de wijziging VM zien als deze in het proces opnieuw distribueren. De `PowerState` van VM gaat 'Uitgevoerd' naar bijwerken, klikt u vervolgens 'starten', en ten slotte '' als deze door het proces van het opnieuw te implementeren naar een nieuwe host. Controleer de status van de VMs binnen een resourcegroep met:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw VM problemen ondervindt, kunt u het Help-informatie over [Probleemoplossing SSH verbindingen](virtual-machines-linux-troubleshoot-ssh-connection.md) of [gedetailleerde SSH stappen voor probleemoplossing](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md)kunt vinden. Als u geen toegang een toepassing wordt uitgevoerd op uw VM tot, kunt u ook [het oplossen van problemen toepassing](virtual-machines-linux-troubleshoot-app-connection.md)lezen.