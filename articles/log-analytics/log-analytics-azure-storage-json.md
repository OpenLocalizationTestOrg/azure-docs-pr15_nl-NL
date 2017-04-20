<properties
    pageTitle="Azure diagnostische logboeken door middel van Log analyses analyseren | Microsoft Azure"
    description="Log Analytics kan de logboekbestanden van Azure-services die Azure diagnostische logboeken om te blob-opslag in de indeling van JSON schrijven lezen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Azure diagnostische logboeken door middel van Log analyses analyseren

Log Analytics kan de logboeken voor de volgende Azure services die in de indeling van JSON-blobopslag [Azure diagnostische logboeken schrijven](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) verzamelen:

+ Automatisering (Preview)
+ Belangrijke kluis (Preview)
+ Toepassingsgateway (Preview)
+ Beveiligingsgroep netwerk (Preview)

De volgende secties begeleid u bij via PowerShell naar:

+ Log Analytics als u wilt de logboeken verzamelen van opslagruimte voor elke resource configureren  
+ De Log Analytics-oplossing voor de Azure-service inschakelen

Voordat Log Analytics van gegevens voor deze bronnen verzamelen kunt, moet Azure diagnostische hulpprogramma's zijn ingeschakeld. Gebruik de `Set-AzureRmDiagnosticSetting` te102826339-cmdlet logboekregistratie inschakelt.

Raadpleeg de volgende artikelen voor meer informatie over het inschakelen van diagnostische gegevens vastleggen:

+ [Belangrijke kluis](../key-vault/key-vault-logging.md)
+ [Toepassingsgateway](../application-gateway/application-gateway-diagnostics.md)
+ [Beveiligingsgroep netwerk](../virtual-network/virtual-network-nsg-manage-log.md)

Deze documentatie bevat ook informatie op:

+ Het oplossen van gegevens verzamelen
+ Gegevens verzamelen stoppen

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics te Azure diagnostische logboeken verzamelen configureren

Het verzamelen van Logboeken voor deze services en inschakelen van de oplossing voor het visualiseren van de logboeken wordt uitgevoerd PowerShell-scripts gebruiken.

In het onderstaande voorbeeld kunt alle ondersteunde resources aanmelden

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Voor sommige bronnen is dit mogelijk is om te de voorgaande configuratie uitvoeren vanuit het Azure-portal met behulp van de stappen in [de diagnostische logboeken overzicht van Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics te Azure diagnostische logboeken verzamelen configureren

We hebben een PowerShell-script-module die twee cmdlets om u te helpen met het configureren van Log Analytics worden geëxporteerd opgegeven:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`u wordt gevraagd om invoer en kan worden ingesteld eenvoudige configuraties is
2. `Add-AzureDiagnosticsToLogAnalytics`alle informatie om te controleren als invoer gaat en vervolgens configureert Log Analytics  

### <a name="pre-requisites"></a>Minimumvereisten

1. Azure PowerShell met versie 1.0.8 of hoger van de cmdlets operationele inzichten.
  - [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
  - Controleer of uw versie van cmdlets:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnostische gegevens vastleggen is geconfigureerd voor de Azure resource die u wilt controleren. Gebruik `Set-AzureRmDiagnosticSetting` of daarnaar verwijzen [Gebruik Log Analytics voor het verzamelen van gegevens van Azure opslag accounts](log-analytics-azure-storage.md) voor het inschakelen van diagnostische gegevens.
3. Een [Logboek Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -werkruimte  
4. De AzureDiagnosticsAndLogAnalytics PowerShell-module
  - Download de module [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) via PowerShell-galerie

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Optie 1: De Scripts interactieve configuratie uitvoeren

PowerShell openen en uitvoeren:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

U bent een lijst met beschikbare selecties te zien en gegeven een prompt om uw selectie te maken.
U wordt gevraagd naar de gewenste opties voor elk van de volgende opties:

+ Resourcetypen (Azure Services) uit-logboeken verzamelen
+ Resource-exemplaren van-Logboeken verzamelen
+ Log Analytics werkruimte om de gegevens te verzamelen

Na het uitvoeren van deze script, ziet u records in Log Analytics ongeveer 30 minuten nadat nieuwe diagnostische gegevens naar opslag is geschreven. Als records niet beschikbaar zijn nadat ditmaal verwijzen naar de sectie Probleemoplossing hieronder.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Optie 2: Een lijst met bronnen bouwen en aan de cmdlet configuratie door te geven

U kunt een lijst met bronnen die u hebt Azure diagnostische hulpprogramma's die zijn ingeschakeld en vervolgens de bronnen voor de cmdlet configuratie maken.

U kunt aanvullende informatie over de cmdlet zien door te voeren `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Meer informatie vinden in OMS [Log Analytics PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Als resource en werkruimte zich in verschillende Azure-abonnementen, ertussen schakelen naar wens gebruiken`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Na het uitvoeren van deze script, ziet u records in Log Analytics ongeveer 30 minuten nadat nieuwe diagnostische gegevens naar opslag is geschreven. Als records niet beschikbaar zijn nadat ditmaal verwijzen naar de sectie Probleemoplossing hieronder.  

>[AZURE.NOTE] De configuratie is niet zichtbaar zijn in de portal van Azure. U kunt controleren of het gebruik van de configuratie van de `Get-AzureRmOperationalInsightsStorageInsight` cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Log analyses van verzamelen Azure diagnostische logboeken stoppen

Als u wilt de Log Analytics-configuratie voor een resource verwijderen, gebruikt u de `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Probleemoplossing configuratie voor Azure diagnostische logboeken

*Probleem*

Het volgende foutbericht wordt weergegeven tijdens het configureren van een resource die is logboekregistratie met klassieke storage:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Resolutie*

Meld u aan bij de API voor het implementatiemodel klassieke met`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Het verzamelen van gegevens voor Azure diagnostische logboeken oplossen

Als u gegevens voor uw Azure resource in Log Analytics niet ziet, kunt u de volgende stappen:

+ Gegevens die doorloopt in het opslag-account controleren
+ Controleer of dat de Log Analytics-oplossing voor de service is ingeschakeld
+ Controleer of Log Analytics is geconfigureerd als u wilt lezen van opslag
+ De accountsleutel opslag bijwerken

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Controleer of gegevens is die doorloopt in het opslag-account

U kunt controleren als gegevens die is doorloopt naar het opslag-account met een hulpmiddel waarmee browsen op Azure opslag (bijvoorbeeld Visual Studio) of via PowerShell.

Ga voor het Account opslag is de resource geconfigureerd voor aanmelden als het volgende PowerShell wilt gebruiken:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

De opslagruimte container die wordt gebruikt door diagnostisch hulpprogramma Azure heeft een naam met een indeling van:  

`insights-logs-<Category>`

Raadpleeg over het [gebruik van opslag-cmdlets om te controleren van een container voor recente gegevens](../storage/storage-powershell-guide-full.md) voor meer informatie over het weergeven van de inhoud van het account opslag.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Controleer of dat de Log Analytics-oplossing voor de service is ingeschakeld

Als u `Add-AzureDiagnosticsToLogAnalyticsUI`, de juiste Log Analytics-oplossing automatisch voor u is ingeschakeld.

Als u wilt controleren of een oplossing is ingeschakeld, voert u het volgende PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Als de oplossing niet is ingeschakeld, kunt u deze via het volgende PowerShell inschakelen:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Als u wilt zoeken de naam van de oplossing moet worden ingeschakeld voor elk resourcetype, gebruikt u het volgende PowerShell (deze variabele is beschikbaar nadat u de module hebt geïmporteerd):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Controleer of Log Analytics is geconfigureerd als u wilt lezen van opslag

Als u extra Azure Resources toevoegt, moet u diagnostische gegevens vastleggen voor hen inschakelen en configureren van Log Analytics voor hen.
Als u wilt controleren welke resources en de opslag accounts Log Analytics is geconfigureerd voor het verzamelen van Logboeken voor, gebruikt u het volgende PowerShell:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Bijwerken van de opslag-toets

Als u de sleutel voor de opslag-account wijzigen, moet de Log Analytics-configuratie ook worden bijgewerkt met de nieuwe sleutel.
U kunt de Log Analytics-configuratie bijwerken met een nieuwe sleutel via het volgende PowerShell:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Als u de naam van de opslagruimte inzicht zoekt, gebruikt u de `Get-AzureRmOperationalInsightsStorageInsight` cmdlet, zoals in eerdere voorbeelden.

## <a name="next-steps"></a>Volgende stappen

- [Gebruik-blobopslag voor IIS- en -tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) lezen van de logboekbestanden van Azure-services die schrijven diagnostisch hulpprogramma tabelopslag of IIS-logboeken naar blob storage geschreven.
- [Oplossingen inschakelen](log-analytics-add-solutions.md) voor bieden inzicht in de gegevens.
- [Gebruik zoekopdrachten gaat uitvoeren](log-analytics-log-searches.md) om de gegevens te analyseren.
