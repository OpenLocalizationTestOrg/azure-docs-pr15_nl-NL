<properties
   pageTitle="Controleer de verbinding van een gateway | Microsoft Azure"
   description="In dit artikel leest u hoe u de verbinding van een gateway in het implementatiemodel resourcemanager controleren"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Controleer de verbinding van een gateway

U kunt de gateway-verbinding verifiëren in verschillende manieren. In dit artikel wordt uitgelegd hoe u om te controleren of de status van een Resource Manager gateway-verbinding met behulp van de Azure-portal en via PowerShell.


## <a name="verify-using-powershell"></a>Controleer of via PowerShell

U moet de meest recente versie van de Azure resourcemanager PowerShell-cmdlets installeren. Zie voor informatie over het installeren van de PowerShell-cmdlets [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Zie [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie over het gebruik van resourcemanager cmdlets.

### <a name="step-1-log-in-to-your-azure-account"></a>Stap 1: Meld u aan bij uw Azure-account

1. Open de PowerShell-console met verhoogde bevoegdheden en verbinding maken met uw account.

        Login-AzureRmAccount

2. Controleer de abonnementen voor het account.

        Get-AzureRmSubscription 

3. Geef het abonnement waaraan u wilt gebruiken.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Stap 2: Controleer uw verbinding


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Controleer of met behulp van de Azure portal

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Volgende stappen

- U kunt virtuele machines toevoegen aan uw virtuele netwerken. Zie [maken een virtuele Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor stapsgewijze instructies.

