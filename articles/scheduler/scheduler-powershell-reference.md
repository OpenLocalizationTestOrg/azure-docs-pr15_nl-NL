<properties
 pageTitle="Scheduler PowerShell-Cmdlets verwijzing"
 description="Scheduler PowerShell-Cmdlets verwijzing"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Scheduler PowerShell-Cmdlets verwijzing

De volgende tabel wordt beschreven en koppelingen naar de pagina overzicht van elk van de belangrijkste cmdlets in Azure Scheduler.

Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement: Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). 

Zie voor meer informatie over [cmdlets Azure resourcemanager](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx) [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

|Cmdlet|Beschrijving van de cmdlet|
|---|---|
[Uitschakelen-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Schakelt een verzameling taak. 
[Inschakelen AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Schakelt een verzameling taak.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Krijgt geplande taken.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Haalt vervullen van siteverzamelingen.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Haalt taak geschiedenis.
[Nieuwe AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Hiermee maakt u een HTTP-taak.
[Nieuwe AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Hiermee maakt u een taak-siteverzameling.
[Nieuwe AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Hiermee maakt u een taak bus wachtrij.
[Nieuwe AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Hiermee maakt u een taak bus onderwerp.
[Nieuwe AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Hiermee maakt u een taak van de wachtrij opslag. 
[Verwijderen AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Hiermee verwijdert u een geplande taak.  
[Verwijderen AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Hiermee verwijdert u een taak-siteverzameling. 
[Set-AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Een taak Scheduler HTTP wijzigt.
[Set-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Hiermee wijzigt u de siteverzameling van een taak. 
[Set-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Een taak bus wachtrij wijzigt.  
[Set-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Hiermee wijzigt u een taak bus onderwerp. 
[Set-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Hiermee wijzigt u een taak van de wachtrij opslag.   

U kunt een van de volgende cmdlets uitvoeren voor meer informatie: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Zie ook


 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)
