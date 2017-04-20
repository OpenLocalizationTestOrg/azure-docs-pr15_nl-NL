<properties
    pageTitle="Azure diagnostische logboeken op gebeurtenis Hubs streamen | Microsoft Azure"
    description="Leer hoe u het streamen Azure diagnostische logboeken op gebeurtenis Hubs."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Azure diagnostische logboeken op gebeurtenis Hubs streamen

**[Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)** kunnen worden streamen nabije realtime voor alle toepassingen met de ingebouwde "Exporteren naar gebeurtenis Hubs" optie in de Portal of doordat de Service Bus regel-Id in een diagnostische instelling via de PowerShell-Cmdlets Azure of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Wat u kunt doen met diagnostische logboeken en gebeurtenis Hubs
Hier volgen een paar manieren waarop u de mogelijkheid streaming voor diagnostische logboeken kan gebruiken:

- **Stream logboeken 3e partij logboekregistratie en telemetrielogboek systemen** – na verloop van tijd gebeurtenis Hubs streaming worden de om de diagnostische logboeken pipe in derden SIEMs en aanmelden analytics-oplossingen.

- **Servicestatus weergave door 'warm pad' gegevensstromen op PowerBI** -gebeurtenis Hubs gebruiken, Stream Analytics en PowerBI, kunt u eenvoudig uw gegevens diagnostische gegevens in bijna realtime inzichten transformeren op uw Azure services. [In dit artikel documentatie biedt een uitstekende overzicht over het instellen van een gebeurtenis Hubs, gegevens met Stream Analytics, verwerken en PowerBI als een uitvoer gebruiken](../stream-analytics/stream-analytics-power-bi-dashboard.md). Hier volgt een paar tips voor het instellen met diagnostische logboeken:
    - De Hubs gebeurtenis voor een categorie van de diagnostische logboeken wordt automatisch gemaakt wanneer u de optie in de portal controleren of deze inschakelen via PowerShell, zodat u wilt de gebeurtenis Hubs selecteren in de naamruimte Service Bus met de naam begint met "inzichten-"
    - Hier volgt een voorbeeld Stream Analytics-query die kunt u gewoon parseren alle de logboekgegevens in een tabel PowerBI:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Maakt u een aangepaste telemetrielogboek en logboekregistratie platform** – als u al een platform op maat telemetrielogboek of hebben alleen nadenkt over het samenstellen van een, de ten zeerste scalable publiceren abonnementen aard van gebeurtenis Hubs kunt u flexibel nemen diagnostische logboeken. [De Zie Tjeerd Rosanova handleiding voor het gebruik van de gebeurtenis Hubs in een hier wereldwijde schaal telemetrielogboek-platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Streaming van diagnostische logboeken activeren
U kunt via programmacode, via de portal van diagnostische logboeken streaming of met behulp van de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)inschakelen. Wanneer u een Service Bus Namespace in beide gevallen kiezen en een gebeurtenis Hubs is gemaakt in de naamruimte voor elke log categorie die u hebt ingeschakeld. Een diagnostische **Log categorie** is een type logboek die een resource kan verzamelen. U kunt welke log categorieën die u wilt verzamelen voor een bepaalde bron in de Portal Azure onder het blad diagnostische gegevens selecteren.

![Log categorieën in de Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Inschakelt en streaming diagnostische logboeken uit berekeningscluster resources (bijvoorbeeld VMs of Service stof) [is vereist een andere set stappen](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Als u wilt inschakelen via de [PowerShell-Cmdlets Azure](insights-powershell-samples.md)-streaming, kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

De Service Bus regel-ID is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`, bijvoorbeeld `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Via Azure CLI
Als u wilt inschakelen via de [Azure CLI](insights-cli-samples.md)-streaming, kunt u de `insights diagnostic set` command als volgt:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Gebruik dezelfde opmaak voor Service Bus regel-ID, zoals wordt uitgelegd voor de PowerShell-Cmdlet.

###<a name="via-azure-portal"></a>Via Azure-Portal
Als u wilt inschakelen via de Portal Azure-streaming, navigeer naar de instellingen voor diagnostische gegevens van een resource en selecteer 'Exporteren naar gebeurtenis Hub.'

![Exporteren naar gebeurtenis Hubs in de Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Selecteer een bestaande Service Bus Namespace om het configureren. De naamruimte geselecteerd is waar de gebeurtenis Hubs is gemaakt (als dit uw streaming diagnostische logboeken voor eerste tijd is) of streamen naar (als er al resources die zijn die categorie log tot deze naamruimte streaming), en het beleid wordt gedefinieerd welke machtigingen van de streaming om heeft. Tegenwoordig kunnen vereist streaming met een gebeurtenis Hubs machtigingen beheren, lezen en verzenden. U kunt maken of wijzigen van het clienttoegangsbeleid Service Bus Namespace gedeeld in de klassieke portal onder het tabblad 'Configureren' voor uw Service Bus Namespace. Als u wilt bijwerken op een van deze diagnostische instellingen, moet de client de machtiging ListKey van de regel die Service Bus autorisatie.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van gebeurtenis Hubs gebruiken?
Hier volgt een voorbeeld uitvoergegevens van de gebeurtenis Hubs:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| De elementnaam van het | Beschrijving                                            |
|--------------|--------------------------------------------------------|
|records       | Een matrix van alle gebeurtenissen in deze nettolading.            |
|tijd          | De tijd waarop de gebeurtenis heeft plaatsgevonden.                      |
|categorie      | Log categorie voor deze gebeurtenis.                           |
|resourceId    | Resource-ID van de resource die deze gebeurtenis gegenereerd. |
|operationName | De naam van de bewerking.                                 |
|niveau         | Optioneel. Geeft het niveau van de gebeurtenis vastleggen.               |
|Eigenschappen    | Eigenschappen van de gebeurtenis.                               |


U kunt een overzicht van alle resource-providers die ondersteuning bieden voor streaming op gebeurtenis Hub [hier](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Volgende stappen
- [Lees meer over Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
- [Aan de slag met Hubs gebeurtenis](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
