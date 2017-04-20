<properties 
    pageTitle="Implementeer deze opnieuw Windows virtuele machines | Microsoft Azure" 
    description="Wordt beschreven hoe implementeer deze opnieuw Windows virtuele machines om RDP verbindingsproblemen te verhelpen." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Implementeer deze opnieuw VM naar nieuwe Azure knooppunt

Als u zijn die tegenover elkaar problemen liggen hebt kunt oplossen extern bureaublad verbinding of een toepassing toegang tot Windows Azure virtuele machine (VM), opnieuw te implementeren de VM. Wanneer u een VM Implementeer deze opnieuw, wordt de VM verplaatst naar een nieuw knooppunt binnen de infrastructuur van Azure en deze vervolgens weer in, behoudt al uw configuratieopties en bijbehorende hulpbronnen bevoegdheden. Dit artikel leest u hoe u een VM met Azure PowerShell of de Azure portal implementeren.

> [AZURE.NOTE] Nadat u een VM Implementeer deze opnieuw, de tijdelijke schijf gaat verloren en dynamische IP-adressen die is gekoppeld aan een virtueel netwerkinterface worden bijgewerkt. 

## <a name="using-azure-powershell"></a>Via Azure PowerShell

Zorg ervoor dat u hebt de meest recente Azure PowerShell 1.x op uw computer zijn geïnstalleerd. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor meer informatie.

Gebruik deze Azure PowerShell-opdracht om het implementeren van uw VM:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw VM problemen ondervindt, kunt u het Help-informatie over [Probleemoplossing RDP-verbindingen](virtual-machines-windows-troubleshoot-rdp-connection.md) of [gedetailleerde RDP stappen voor probleemoplossing](virtual-machines-windows-detailed-troubleshoot-rdp.md)kunt vinden. Als u geen toegang een toepassing wordt uitgevoerd op uw VM tot, kunt u ook [het oplossen van problemen toepassing](virtual-machines-windows-troubleshoot-app-connection.md)lezen.