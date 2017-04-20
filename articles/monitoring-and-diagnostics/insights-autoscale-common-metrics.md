<properties
    pageTitle="Azure Monitor autoscaling algemene aan de doelstellingen. | Microsoft Azure"
    description="Informatie over welke aan de doelstellingen vaak worden gebruikt voor autoscaling uw Cloudservices, virtuele Machines en Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor autoscaling algemene doelstellingen

Azure Monitor autoscaling kunt u het aantal actieve exemplaren schaal omhoog of omlaag, op basis van telemetriegegevens (aan de doelstellingen). In dit document wordt beschreven algemene parameters die u wilt gebruiken. Klik in de Portal Azure voor Cloudservices en Server-Farms kunt u de meetwaarde van de resource aan de nieuwe schaal door te kiezen. U kunt echter ook eventuele metrisch kiezen uit een andere resource aan de nieuwe schaal door.

Hier vindt u de details over het zoeken en de cijfers die u schalen wilt door weer. Het volgende geldt voor VM schaal Sets ook schaalbaarheid.

## <a name="compute-metrics"></a>Aan de doelstellingen berekenen
Standaard Azure VM v2 wordt geleverd met hulpprogramma's voor diagnose extensie geconfigureerd en de doelstellingen van de volgende ingeschakeld hebben.

- [Gast aan de doelstellingen voor Windows VM v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Gast aan de doelstellingen voor Linux VM v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

U kunt de `Get MetricDefinitions` API/PoSH/CLI naar de statistieken die beschikbaar zijn voor de resource VMSS bekijken. 

Als u VM schaal sets gebruikt en u een bepaalde waarde vermeld niet ziet, klikt u vervolgens is waarschijnlijk *uitgeschakeld* in uw extensie diagnostische gegevens.

Als een bepaalde waarde niet wordt partijen of overgebracht met de frequentie die u wilt, kunt u de configuratie van de diagnostische bijwerken.

Als beide gevallen bovenstaande waar is, raadpleegt u vervolgens [De PowerShell gebruiken om in te schakelen Azure diagnostische gegevens in een virtuele machine waarop Windows wordt uitgevoerd](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) over PowerShell configureren en bijwerken van uw extensie Azure VM diagnostische hulpprogramma's om te schakelen van de meetwaarde. Dit artikel bevat ook een voorbeeldbestand voor de configuratie van diagnostische gegevens.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Aan de doelstellingen voor Windows VM v2 als gast van een OS berekenen

Wanneer u een nieuwe VM (v2) in Azure wordt aangegeven maakt, is diagnostische gegevens met behulp van de extensie diagnostisch hulpprogramma ingeschakeld.

U kunt een lijst met de doelstellingen genereren met behulp van de volgende opdracht in PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt een waarschuwing voor de doelstellingen van de volgende maken.

|De naam van de metrische|   Eenheid|
|---|---|
|\Processor(_Total)\% processortijd    |Percentage|
|\Processor(_Total)\% tijd in beschermde modus   |Percentage|
|\Processor(_Total)\% Gebruikerstijd |Percentage|
|\Processor informatie (_Totaal) \Processor frequentie |Tellen|
|\System\Processes| Tellen|
|\Process (_Totaal) \Thread tellen| Tellen|
|\Process (_Totaal) \Handle tellen  |Tellen|
|\Memory\% toegewezen Bytes In gebruik   |Percentage|
|\Memory\Available bytes|   Bytes|
|\Memory\Committed bytes    |Bytes|
|\Memory\Commit limiet|  Bytes|
|\Memory\Pool geheugen: Bytes|  Bytes|
|\Memory\Pool geheugen: Bytes|   Bytes|
|\PhysicalDisk(_Total)\% tijd schijf| Percentage|
|\PhysicalDisk(_Total)\% schijf lezen tijd|    Percentage|
|\PhysicalDisk(_Total)\% schijf schrijven tijd|   Percentage|
|\PhysicalDisk (_Totaal) \Disk overdrachten/sec   |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk lezen per seconde   |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk per seconde  |CountPerSecond|
|\PhysicalDisk (_Totaal) \Disk bytes/sec   |BytesPerSecond|
|\PhysicalDisk (_Totaal) \Disk gelezen Bytes per seconde| BytesPerSecond|
|\PhysicalDisk (_Totaal) \Disk geschreven Bytes per seconde |BytesPerSecond|
|\PhysicalDisk (_Totaal) \Avg Lengte|  Tellen|
|\PhysicalDisk (_Totaal) \Avg Lengte lezen| Tellen|
|\PhysicalDisk (_Totaal) \Avg Lengte schrijven |Tellen|
|\LogicalDisk(_Total)\% ruimte vrij te geven| Percentage|
|\LogicalDisk (_Totaal) \Free megabytes|   Tellen|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Aan de doelstellingen voor Linux VM v2 als gast van een OS berekenen

Wanneer u een nieuwe VM (v2) in Azure wordt aangegeven maakt, is diagnostische gegevens met behulp van diagnostische gegevens extensie standaard ingeschakeld.

U kunt een lijst met de doelstellingen genereren met behulp van de volgende opdracht in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 U kunt een waarschuwing voor de doelstellingen van de volgende maken.

|De naam van de metrische                            |Eenheid|
|---|---|
|\Memory\AvailableMemory                |Bytes|
|\Memory\PercentAvailableMemory         |Percentage|
|\Memory\UsedMemory                     |Bytes|
|\Memory\PercentUsedMemory              |Percentage|
|\Memory\PercentUsedByCache             |Percentage|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bytes|
|\Memory\PercentAvailableSwap           |Percentage|
|\Memory\UsedSwap                       |Bytes|
|\Memory\PercentUsedSwap                |Percentage|
|\Processor\PercentIdleTime             |Percentage|
|\Processor\PercentUserTime             |Percentage|
|\Processor\PercentNiceTime             |Percentage|
|\Processor\PercentPrivilegedTime       |Percentage|
|\Processor\PercentInterruptTime        |Percentage|
|\Processor\PercentDPCTime              |Percentage|
|\Processor\PercentProcessorTime        |Percentage|
|\Processor\PercentIOWaitTime           |Percentage|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Seconden|
|\PhysicalDisk\AverageWriteTime         |Seconden|
|\PhysicalDisk\AverageTransferTime      |Seconden|
|\PhysicalDisk\AverageDiskQueueLength   |Tellen|
|\NetworkInterface\BytesTransmitted     |Bytes|
|\NetworkInterface\BytesReceived        |Bytes|
|\NetworkInterface\PacketsTransmitted   |Tellen|
|\NetworkInterface\PacketsReceived      |Tellen|
|\NetworkInterface\BytesTotal           |Bytes|
|\NetworkInterface\TotalRxErrors        |Tellen|
|\NetworkInterface\TotalTxErrors        |Tellen|
|\NetworkInterface\TotalCollisions      |Tellen|




## <a name="commonly-used-web-server-farm-metrics"></a>Aan de doelstellingen veelgebruikte Web (serverfarm)

U kunt ook automatisch schalen op basis van algemene web server-gegevens zoals de lengte van de Http-wachtrij uitvoeren. De naam metrische is **HttpQueueLength**.  De volgende sectie bevat de doelstellingen van de beschikbare server-farm (Web-Apps).

### <a name="web-apps-metrics"></a>De doelstellingen van de Web-Apps

U kunt een lijst met de doelstellingen van de Web-Apps met behulp van de volgende opdracht in PowerShell genereren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

U kunt op een waarschuwing of schaal door deze aan de doelstellingen.

|De naam van de metrische        |Eenheid|
|---                |---|
|CpuPercentage      |Percentage|
|MemoryPercentage   |Percentage|
|DiskQueueLength    |Tellen|
|HttpQueueLength    |Tellen|
|BytesReceived      |Bytes|
|BytesSent          |Bytes|


## <a name="commonly-used-storage-metrics"></a>Veelgebruikte opslageenheden
U kunt met de lengte wachtrij opslag, dat wil het aantal berichten in de wachtrij opslag zeggen wilt verkleinen. Lengte van de opslag is een speciale meting en wordt de drempelwaarde voor toegepast het aantal berichten per exemplaar. Dit betekent dat als er twee instanties en als de drempelwaarde is ingesteld op 100, deze aangepast wordt als het totale aantal berichten in de wachtrij 200 zijn. Bijvoorbeeld 100 berichten per exemplaar.

U kunt dit is in de Portal Azure in het blad **Instellingen** . U kunt de instelling automatisch schalen in de sjabloon ARM *metricName* gebruiken als *ApproximateMessageCount* en de ID van de wachtrij opslag doorgeeft als *metricResourceUri*bijwerken voor VM schaal sets.

Bijvoorbeeld, met een klassieke opslag-Account moet het metricTrigger van de instelling automatisch schalen zou bevatten:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Voor een account (klassieke) opslag, zou het metricTrigger opnemen:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Veelgebruikte Service Bus doelstellingen

U kunt met de lengte wachtrij Service Bus, dat wil het aantal berichten in de wachtrij Service Bus zeggen wilt verkleinen. Lengte van de service Bus is een speciale meting en wordt de opgegeven drempelwaarde toegepast het aantal berichten per exemplaar. Dit betekent dat als er twee instanties en als de drempelwaarde is ingesteld op 100, deze aangepast wordt als het totale aantal berichten in de wachtrij 200 zijn. Bijvoorbeeld 100 berichten per exemplaar.

U kunt de instelling automatisch schalen in de sjabloon ARM *metricName* gebruiken als *ApproximateMessageCount* en de ID van de wachtrij opslag doorgeeft als *metricResourceUri*bijwerken voor VM schaal sets.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Het concept van de groep resource bestaat niet voor Service Bus, maar Azure resourcemanager Hiermee maakt u een standaard-resourcegroep per regio. De resourcegroep is meestal in de notatie 'Default - ServiceBus-[regio]'. Bijvoorbeeld 'Standaard-ServiceBus-EastUS', 'Standaard-ServiceBus-WestUS', 'Standaard-ServiceBus-AustraliaEast' enzovoort.
