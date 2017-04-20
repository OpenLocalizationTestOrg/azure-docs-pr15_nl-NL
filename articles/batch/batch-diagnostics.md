<properties
   pageTitle="Azure Batch diagnostische gegevens vastleggen | Microsoft Azure"
   description="Opnemen en diagnostische logboekgebeurtenissen voor Azure Batch account resources zoals pools en taken te analyseren."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure Batch diagnostische gegevens vastleggen

De Batch-service Hiermee net als met veel Azure services, plaatst u de gebeurtenissen voor bepaalde resources tijdens de levensduur van de resource. U kunt de diagnostische logboeken Azure Batch record gebeurtenissen voor resources zoals pools en taken wilt inschakelen en gebruik vervolgens de logboeken voor diagnostische evaluatie en controle. Gebeurtenissen zoals groep maken, groep verwijderen, begin, de taak is voltooid en anderen zijn opgenomen in de diagnostische logboeken Batch.

>[AZURE.NOTE] In dit artikel wordt beschreven hoe de registratie van gebeurtenissen voor Batch account resources zelf, niet vervullen en uitvoergegevens van taak. Zie [het project en uitvoer persistent Azure Batch](batch-task-output.md)voor meer informatie over het opslaan van de uitvoergegevens van uw taken en taken.

## <a name="prerequisites"></a>Vereisten voor

* [Azure Batch-account](batch-account-create-portal.md)

* [Azure opslag-account](../storage/storage-create-storage-account.md#create-a-storage-account)

  Als u wilt de diagnostische logboeken Batch persistent, moet u een opslag van Azure-account waar Azure de logboeken wilt opslaan maken. U dit account opslag opgeven wanneer u [Diagnostische gegevens vastleggen inschakelen](#enable-diagnostic-logging) voor uw account Batch. Het opslag-account dat u opgeven wanneer u log-siteverzamelingen inschakelen is niet hetzelfde als een gekoppelde opslag-account waarnaar wordt verwezen in de [toepassingspakketten](batch-application-packages.md) en [taak uitvoer permanente](batch-task-output.md) artikelen.

  >[AZURE.WARNING] U bent **BTW geheven** voor de gegevens die zijn opgeslagen in uw opslagruimte van Azure-account. Dit geldt ook voor de diagnostische logboeken in dit artikel beschreven. Houd hier rekening mee bij het ontwerpen van uw [bewaarbeleid](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Diagnostische gegevens vastleggen

Diagnostische gegevens vastleggen is niet ingeschakeld voor uw account Batch standaard. Diagnostische gegevens vastleggen voor elke Batch-account dat u wilt controleren, moet u expliciet inschakelen:

[Het inschakelen van verzameling diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Het is aan te raden wij u het volledige artikel [overzicht van Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) beter inzicht niet alleen hoe het inschakelen van logboekregistratie, maar de categorieën log worden ondersteund door de verschillende services van Azure lezen. Azure Batch ondersteunt bijvoorbeeld momenteel één log categorie: **Logboeken aan de Service**.

## <a name="service-logs"></a>Service-Logboeken

Azure Batch Service logboeken bevatten gebeurtenissen die zijn gegenereerd door de service Azure Batch gedurende de levensduur van een Batch-bron, zoals een groep of een taak. Elke gebeurtenis die is verstrekt door de Batch is opgeslagen in de opgegeven opslag-account in de indeling van JSON. Dit is bijvoorbeeld de hoofdtekst van een steekproef **van toepassingen gebeurtenis maken**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

De hoofdtekst van elke gebeurtenis bevindt zich in een bestand .json in de opgegeven opslag van Azure-account. Als u direct toegang krijgen tot de logboeken wilt, kunt u het [schema van diagnostische logboeken in de opslagruimte-account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)controleren.

## <a name="service-log-events"></a>Service gebeurtenissen

De Batch-service Hiermee momenteel plaatst u de volgende gebeurtenissen van Service. Deze lijst zijn mogelijk niet volledig, aangezien extra gebeurtenissen wellicht zijn toegevoegd omdat in dit artikel voor het laatst werd bijgewerkt.

| **Service gebeurtenissen** |
| ------------------ |
| [Groep maken][pool_create] |
| [Groep verwijderen starten][pool_delete_start] |
| [Groep verwijderen is voltooid][pool_delete_complete] |
| [Groep formaat wijzigen starten][pool_resize_start] |
| [Het formaat van toepassingen wijzigen is voltooid][pool_resize_complete] |
| [Taak starten][task_start] |
| [Taak voltooid][task_complete] |
| [Taak fail][task_fail] |

## <a name="next-steps"></a>Volgende stappen

Naast het opslaan van diagnostische gegevens vastleggen in een opslag van Azure-account, kunt u ook streamen Batch Service gebeurtenissen naar een [Azure gebeurtenis Hub](../event-hubs/event-hubs-what-is-event-hubs.md)en stuurt u hen naar [Azure Log Analytics](../log-analytics/log-analytics-overview.md).

* [Azure diagnostische logboeken op gebeurtenis Hubs streamen](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Stream Batch diagnostische gebeurtenissen de ten zeerste scalable gegevens ingress service, gebeurtenis Hubs. Gebeurtenis Hubs kunnen nemen miljoenen gebeurtenissen per seconde, die u kunt vervolgens transformeren en opslaan met een realtime analytics-provider.

* [Azure diagnostische logboeken door middel van Log analyses analyseren](../log-analytics/log-analytics-azure-storage-json.md)

  Stuur uw diagnostische logboeken naar Log Analytics kunt u deze in de portal bewerkingen Management Suite Kantoorbeheersysteem analyseren, of ze exporteren voor analyse in Power BI of Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
