<properties
   pageTitle="Aan de slag voor het maken van een internetverbinding van taakverdeling in de klassieke modus via PowerShell | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding van taakverdeling in de klassieke modus via PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Maken van een internetverbinding van taakverdeling (classic) in PowerShell aan de slag

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [informatie over het maken van taakverdeling met Azure Resource Manager van een internetverbinding](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Instellen van taakverdeling via PowerShell

Als u wilt een taakverdeling via powershell hebt ingesteld, de volgende stappen uit te voeren:

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.


2. Na het maken van een virtuele machine, kunt u PowerShell-cmdlets een taakverdeling toevoegen aan een virtuele machine binnen de dezelfde cloudservice.

In het volgende voorbeeld voegt u dat een taakverdeling instellen genoemd 'webfarm"naar cloud service"mytestcloud"(of myctestcloud.cloudapp.net), de eindpunten voor de taakverdeling toevoegen aan virtuele machines met de naam"web1"en"web2". De taakverdeling ontvangt netwerkverkeer op poort 80 en laden rekeningsaldi tussen de virtuele machines gedefinieerd door het lokale eindpunt (in dit geval poort 80) met TCP.


### <a name="step-1"></a>Stap 1
Een eindpunt taakverdeling maken voor de eerste VM "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Stap 2

Een ander eindpunt maken voor de tweede VM "web2" dezelfde laden verdeling set naam

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Een virtuele machine verwijderen uit een taakverdeling

U kunt verwijderen AzureEndpoint een VM-eindpunt verwijderen uit de taakverdeling

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

U kunt ook [aan de slag voor het maken van een interne taakverdeling](load-balancer-get-started-ilb-classic-ps.md) en welk type [verdeling modus](load-balancer-distribution-mode.md) voor een especific verdeling van netwerkverkeer laadgedrag configureren.

Als uw toepassing verbindingen leven voor servers achter een taakverdeling behouden moet, kunt u meer weten over [niet-actieve TCP time-outinstellingen voor een taakverdeling](load-balancer-tcp-idle-timeout.md). Deze helpen voor meer informatie over niet-actieve Verbindingsgedrag wanneer u taakverdeling Azure gebruikt.

