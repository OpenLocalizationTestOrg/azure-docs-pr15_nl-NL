<properties
    pageTitle="Overzicht van Azure diagnostische logboeken | Microsoft Azure"
    description="Leer wat Azure diagnostische logboeken zijn en hoe u deze kunt gebruiken voor meer informatie over gebeurtenissen binnen een Azure resource."
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
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Overzicht van Azure diagnostische logboeken
**Diagnostische logboeken Azure** zijn logboeken dat door een resource die volwaardige, veelgebruikte gegevens over de werking van deze resource bevatten. De inhoud van deze logboeken is afhankelijk van het resourcetype (bijvoorbeeld Windows-systeem van gebeurtenislogboeken zijn één categorie van de diagnostische logboeken voor VMs en blob, tabel en wachtrij logboeken zijn categorieën van diagnostische logboeken voor opslag-accounts) en verschillen van de [activiteit Log (voorheen bekend als controlelogboek of operationele Log)](monitoring-overview-activity-logs.md), waarmee u inzicht in de bewerkingen die zijn uitgevoerd voor bronnen in uw abonnement. Niet alle resources ondersteuning voor het nieuwe type diagnostische logboeken die hier worden beschreven. De lijst met Services ondersteund hieronder ziet u welke resourcetypen bieden ondersteuning voor de nieuwe diagnostische logboeken.

![Logische plaatsing van de diagnostische logboeken](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Wat u kunt doen met diagnostische logboeken
Hier volgen enkele dingen die u met diagnostische logboeken doen kunt:

- Sla deze op een **Account van de opslagruimte** voor controle- of handmatige inspectie. U kunt het bewaarbeleid tijd (in dagen) met de **Diagnostische instellingen**opgeven.
- [Streamen laten **Hubs gebeurtenis** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname door een derde partij service of aangepaste analytics-oplossing zoals PowerBI.
- Ze met [OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md) analyseren

## <a name="diagnostic-settings"></a>Diagnostische instellingen
Diagnostische logboeken voor niet-berekeningscluster resources zijn geconfigureerd met diagnostische instellingen. **Diagnostische instellingen** voor een besturingselement voor een resource:

- Diagnostische logboeken worden waar verzonden (opslag-Account, gebeurtenis Hubs en/of OMS Log Analytics).
- Welke categorieën Log worden verzonden.
- Hoe lang elke categorie log moet worden bewaard in een opslag-Account: een bewaarbeleid van nul dagen betekent dat de logboeken eeuwen worden gehouden. Anders tussen deze waarde 1 en 2147483647. Als bewaarbeleid zijn ingesteld, maar Logboeken opslaan in een opslag-Account is uitgeschakeld (bijvoorbeeld als er slechts gebeurtenis Hubs of OMS opties zijn geselecteerd), het bewaarbeleid geen gevolgen hebben.

Deze instellingen zijn eenvoudig geconfigureerd via het blad diagnostische hulpprogramma's voor een resource in de portal van Azure, via Azure PowerShell en CLI opdrachten of via de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] Diagnostische logboeken en aan de doelstellingen voor berekeningscluster resources (bijvoorbeeld VMs of Service stof) gebruikt [een aparte om de configuratie en de selectie van uitvoer](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Het inschakelen van verzameling diagnostische logboeken
Verzameling diagnostische logboeken kan worden ingeschakeld als onderdeel van het maken van een resource of nadat een resource is gemaakt via een van de resource blade in de Portal. U kunt ook diagnostische logboeken op elk gewenst moment Azure PowerShell of CLI opdrachten, of gebruiken van de Azure Monitor REST API inschakelen.

> [AZURE.TIP] Deze instructies geldt niet rechtstreeks aan elke resource. Zie de koppelingen schema onder aan deze pagina voor meer informatie over speciale stappen die u mogelijk op bepaalde resourcetypen van toepassing.

[In dit artikel leest hoe u een resource-sjabloon kunt gebruiken om in te schakelen diagnostische instellingen bij het maken van een resource](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Diagnostische logboeken inschakelen in de portal
Wanneer u berekeningscluster resourcetypen doordat de extensie Windows of Linux Azure diagnostische hulpprogramma's maakt, kunt u de diagnostische logboeken inschakelen in de portal van Azure:

1.  Ga naar **Nieuw** en kiest u de resource waarin u geïnteresseerd bent.
2.  Na de basisinstellingen configureren en een grootte, te selecteren in het blad **Instellingen** , onder **controle**, selecteer **ingeschakeld** en kiest u een opslag-account waar u wilt de diagnostische logboeken opslaan. Normale gegevens eenheidstarieven voor opslagruimte en transacties wordt geheven wanneer u diagnostische gegevens naar een opslag-account verzendt.

    ![Diagnostische logboeken tijdens het maken van de resource inschakelen](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Klik op **OK** en maken van de resource.

Voor niet-berekeningscluster resources, kunt u diagnostische logboeken inschakelen in de portal van Azure nadat een resource is gemaakt door het volgende te doen:

1.  Ga naar het blad voor de resource en opent u het blad **Diagnostische gegevens** .
2.  Klik **op** en kies een opslag-Account en/of de Hub van de gebeurtenis.

    ![Diagnostische logboeken inschakelen nadat de resource is gemaakt](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  Selecteer onder **Logboeken**, welke **Log categorieën** die u wilt verzamelen of streamen.
4.  Klik op **Opslaan**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Diagnostische logboeken via PowerShell inschakelen
Als u wilt inschakelen diagnostische logboeken via de PowerShell-Cmdlets van Azure, moet u de volgende opdrachten gebruiken.

Gebruik deze opdracht om te schakelen opslag van de diagnostische logboeken in een opslag-Account:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

De opslagruimte Account-ID is de resource-id voor de opslag-account waarnaar u wilt de logboeken verzenden.

Gebruik deze opdracht om te schakelen streaming van diagnostische logboeken op een gebeurtenis-Hub:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

De Service Bus regel-ID is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`.

Gebruik deze opdracht om te schakelen van de diagnostische logboeken verzenden naar een werkruimte Log analyses:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] De parameter WorkspaceId is niet beschikbaar in de oktober-versie. Deze worden beschikbaar in de November-versie.

U kunt uw Log Analytics werkruimte-ID in de portal van Azure aanvragen.

U kunt deze parameters als u meerdere uitvoeropties wilt combineren.

### <a name="enable-diagnostic-logs-via-cli"></a>Diagnostische logboeken via CLI inschakelen
Als u wilt inschakelen diagnostische logboeken via de CLI Azure, moet u de volgende opdrachten gebruiken:

Gebruik deze opdracht om te schakelen opslag van de diagnostische logboeken in een opslag-Account:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

De opslagruimte Account-ID is de resource-id voor de opslag-account waarnaar u wilt de logboeken verzenden.

Gebruik deze opdracht om te schakelen streaming van diagnostische logboeken op een gebeurtenis-Hub:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

De Service Bus regel-ID is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`.

Gebruik deze opdracht om te schakelen van de diagnostische logboeken verzenden naar een werkruimte Log analyses:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] De parameter workspaceId is niet beschikbaar in de oktober-versie. Deze worden beschikbaar in de November-versie.

U kunt uw Log Analytics werkruimte-ID in de portal van Azure aanvragen.

U kunt deze parameters als u meerdere uitvoeropties wilt combineren.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Diagnostische logboeken via REST API inschakelen
Diagnostische instellingen voor het gebruik van de Azure Monitor REST API wilt wijzigen door [het document](https://msdn.microsoft.com/library/azure/dn931931.aspx)te zien.

## <a name="manage-diagnostic-settings-in-the-portal"></a>Diagnostische instellingen in de portal beheren

Om ervoor te zorgen dat alle resources zich correct zijn ingesteld met diagnostische instellingen, kunt u navigeren naar het blad **controle** in de portal en vervolgens opent het blad **Diagnostische logboeken** .

![Diagnostische logboeken blade in de portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Mogelijk moet u "Meer services" klikt u op het blad Monitoring zoeken.

U kunt in deze blade, weergeven en filteren van alle resources die ondersteuning bieden voor diagnostische logboeken om te zien als ze hebben diagnostische gegevens die zijn ingeschakeld en welke opslag-account, gebeurtenis hub en/of Log Analytics-werkruimte deze logboeken die zijn doorloopt naar.

![Diagnostische logboeken blade resultaten in de portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Klikken op een resource, toont alle logboeken die zijn opgeslagen in de opslagruimte-account en geeft u de optie voor het uitschakelen of de diagnostische instellingen wijzigen. Klik op het downloadpictogram om het downloaden van Logboeken voor een bepaalde periode.

![Diagnostische logboeken blade één resource](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Diagnostische logboeken wordt alleen weergegeven in deze weergave en worden gedownload als u diagnostische instellingen deze op te slaan met een opslag-account hebt geconfigureerd.

Het blad diagnostische instellingen, waar u kunt inschakelen, uitschakelen of wijzigen van uw diagnostische instellingen voor de geselecteerde resource krijgt te klikken op de koppeling voor **Diagnostische instellingen** .

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Ondersteunde services en schema voor diagnostische logboeken
Het schema voor diagnostische logboeken varieert afhankelijk van de resource en log categorie. Hieronder vindt u de ondersteunde services en hun schema.

| Service                       | Schema en documenten                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Software-taakverdeling     |    [Log analytics voor Azure taakverdeling (Preview)](../load-balancer/load-balancer-monitor-log.md)             |
|    Beveiligingsgroepen netwerk    |    [Log analytics voor netwerk-beveiligingsgroepen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Toepassingsgateways       |    [Diagnostisch hulpprogramma logboekregistratie voor de toepassingsgateway](../application-gateway/application-gateway-diagnostics.md)     |
|    Belangrijke kluis                  |    [Azure belangrijke kluis logboekregistratie](../key-vault/key-vault-logging.md)                                                 |
|    Azure zoeken               |    [Schakelen en te zoeken verkeer Analytics gebruiken](../search/search-traffic-analytics.md)                         |
|    Lake gegevensopslag            |    [Toegang krijgen tot de diagnostische logboeken voor Azure Lake gegevensopslag](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Gegevens Lake Analytics        |    [Diagnostische logboeken openen van Azure gegevens Lake analysegegevens](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logica Apps                 |    Geen schema beschikbaar.                                                                                         |
|    Azure Batch                |    [Azure Batch diagnostische gegevens vastleggen](../batch/batch-diagnostics.md)                                              |
|    Azure automatisering           |    [Log analytics voor het automatiseren van Azure](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Gebeurtenis-Hub                  |    Geen schema beschikbaar.                                                                                         |
|    Service Bus                |    Geen schema beschikbaar.                                                                                         |
|    Stream Analytics           |    Geen schema beschikbaar.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Log categorieën per resourcetype wordt ondersteund

|Resourcetype|Categorie|De naam van categorie weergeven|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Logboeken aan de taak|
|Microsoft.Automation/automationAccounts|JobStreams|Taak-Streams|
|Microsoft.Batch/batchAccounts|ServiceLog|Service-Logboeken|
|Microsoft.DataLakeAnalytics/accounts|Controlelogboek|Controlelogboeken bijhouden|
|Microsoft.DataLakeAnalytics/accounts|Aanvragen|Logboeken aanvragen|
|Microsoft.DataLakeStore/accounts|Controlelogboek|Controlelogboeken bijhouden|
|Microsoft.DataLakeStore/accounts|Aanvragen|Logboeken aanvragen|
|Microsoft.EventHub/namespaces|ArchiveLogs|Logboeken archiveren|
|Microsoft.EventHub/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.KeyVault/vaults|AuditEvent|Controlelogboeken bijhouden|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnostische werkstroomgebeurtenissen runtime|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Beveiligingsgroep netwerk gebeurtenis|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Beveiligingsgroep netwerk regel item|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Beveiligingsgroep netwerk regel stroom gebeurtenis|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|De verdeling van waarschuwing gebeurtenissen laden|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Status van de verdeling van test systeemstatus laden|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Toepassing Gateway Access Log|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Toepassing Gateway prestaties Log|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Logboek van toepassing Gateway Firewall|
|Microsoft.Search/searchServices|OperationLogs|Logboeken aan de bewerking|
|Microsoft.ServerManagement/nodes|RequestLogs|Logboeken aanvragen|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.StreamAnalytics/streamingjobs|Kan worden uitgevoerd|Kan worden uitgevoerd|
|Microsoft.StreamAnalytics/streamingjobs|Ontwerpen|Ontwerpen|

## <a name="next-steps"></a>Volgende stappen
- [Stream diagnostische logboeken op **gebeurtenis Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Diagnostische instellingen wijzigen met de Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [De logboeken met OMS Log Analytics analyseren](../log-analytics/log-analytics-azure-storage-json.md)
