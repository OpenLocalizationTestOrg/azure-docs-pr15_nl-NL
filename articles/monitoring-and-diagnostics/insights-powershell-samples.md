<properties
    pageTitle="Azure Monitor PowerShell aan de slag voorbeelden. | Microsoft Azure"
    description="PowerShell gebruiken voor toegang tot Azure Monitor functies zoals automatisch schalen, waarschuwingen, webhooks en logboeken aan de activiteit te zoeken."
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Voorbeelden van Azure Monitor PowerShell aan de slag

In dit artikel wordt voorbeeld van de PowerShell-opdrachten kunt u toegang tot de functies van Azure Monitor. Azure Monitor kunt u automatisch schalen Cloud Services, virtuele Machines en Web Apps en waarschuwingen verzenden of web-URL's op basis van waarden van geconfigureerde telemetriegegevens bellen.

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor het zogenoemde "Azure inzichten" tot 25e sep 2016. Echter bevatten de naamruimten en dus de onderstaande opdrachten nog steeds de "inzichten".

## <a name="set-up-powershell"></a>PowerShell instellen
Als u nog niet is gedaan, ingesteld u PowerShell uitvoeren op uw computer. Lees [hoe u installatie en PowerShell configureren](../powershell-install-configure.md) voor meer informatie.

## <a name="examples-in-this-article"></a>Voorbeelden in dit artikel

De voorbeelden in het artikel ziet hoe u Azure Monitor cmdlets kunt gebruiken. U kunt ook de volledige lijst met Azure Monitor PowerShell-cmdlets op [Azure Monitor (inzichten) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)bekijken.


## <a name="sign-in-and-use-subscriptions"></a>Aanmelden bij en gebruiken van abonnementen

Eerst Meld u aan bij uw Azure-abonnement.

```PowerShell
Login-AzureRmAccount
```

Dit moet u aan te melden. Zodra u doet, wordt uw Account, zijn de TenantId en standaard abonnements-Id worden weergegeven. De Azure cmdlets werken in de context van uw standaardabonnement op. Als u wilt weergeven in de lijst met abonnementen die u toegang tot hebt, gebruikt u de volgende opdracht uit.

```PowerShell
Get-AzureRmSubscription
```

Gebruik de volgende opdracht uit als u wilt wijzigen uw context werken in een ander abonnement.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Controlelogboeken bijhouden voor een abonnement ophalen
Gebruik de `Get-AzureRmLog` cmdlet.  Hieronder volgen enkele veelvoorkomende voorbeelden.

Krijg logboekvermeldingen vanaf deze datum om aan te bieden:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Krijg logboekvermeldingen tussen een tijd/datumbereik:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Logboekvermeldingen ophalen uit een bepaalde resourcegroep:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Logboekvermeldingen ophalen uit een specifieke bron provider tussen een tijd/datumbereik:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Alle logboekvermeldingen met een specifieke beller ophalen:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

De volgende opdracht haalt de laatst 1000 gebeurtenissen uit het controlelogboek:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`ondersteunt veel andere parameters. Zie de `Get-AzureRmLog` verwijzing voor meer informatie.

>[AZURE.NOTE] `Get-AzureRmLog`alleen recht biedt op 15 dagen van de geschiedenis. De parameter **- MaxEvents** gebruikt, kunt u de laatste N-gebeurtenissen, dan 15 dagen query. Op access gebeurtenissen die ouder zijn dan 15 dagen, gebruikt u de REST API of SDK (C# voorbeeld met de SDK). Als u geen **Starttijd**, is de standaardwaarde **eindtijd** min één uur. Als u geen **eindtijd**, is de standaardwaarde huidige tijd. Allen tijde zijn opgeslagen in UTC.

## <a name="retrieve-alerts-history"></a>Geschiedenis meldingen ophalen
Als u wilt weergeven op alle meldingen gebeurtenissen, kunt u de resourcemanager Azure-logboeken met de volgende voorbeelden zoeken.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Als u wilt de geschiedenis voor een bepaalde regel van de waarschuwing weergeven, kunt u de `Get-AzureRmAlertHistory` cmdlet, geven in de resource-ID van de huidige regel.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

De `Get-AzureRmAlertHistory` cmdlet ondersteunt verschillende parameters. Meer informatie raadpleegt u [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Informatie over waarschuwingsregels ophalen
Alle van de volgende opdrachten worden uitgevoerd wanneer een resourcegroep met de naam 'montest'.

De eigenschappen van de huidige regel weergeven:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Alle waarschuwingen voor een resourcegroep ophalen:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Hiermee kunt u alle waarschuwingsregels instellen voor een resource doel ophalen. Bijvoorbeeld instellen alle waarschuwingsregels op een VM.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`ondersteunt andere parameters. Zie [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) voor meer informatie.

## <a name="create-alert-rules"></a>Waarschuwingsregels maken
U kunt de `Add-AlertRule` cmdlet maken, bijwerken of een waarschuwing regel uitschakelen.

U kunt gebruiken om e-mail en webhook eigenschappen maken `New-AzureRmAlertRuleEmail` en `New-AzureRmAlertRuleWebhook`respectievelijk. In de cmdlet waarschuwingsregels, moet u deze als acties aan de eigenschap **Acties** van de huidige regel toewijzen.

De volgende sectie bevat een voorbeeld u hoe ziet u een waarschuwingsregels maken met verschillende parameters.

### <a name="alert-rule-on-a-metric"></a>Waarschuwing regel op een meting
De volgende tabel beschrijft de parameters en waarden voor het maken van een melding voor het gebruik van een meting.


|parameter|waarde|
|---|---|
|Naam|  simpletestdiskwrite|
|Locatie van deze waarschuwing regel|   Oost-VS|
|ResourceGroup| montest|
|TargetResourceId|  /Subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|MetricName van de waarschuwing die is gemaakt|   \PhysicalDisk (_Totaal) \Disk schrijven/seconde. Zie de `Get-MetricDefinitions` cmdlet onder informatie over het ophalen van de exacte metrische namen|
|operator|  Groter dan|
|Drempelwaarde (aantal/sec in voor deze metrisch)|    1|
|Venstergrootte (indeling: mm: SS)|  00:05:00|
|aggregator (statistische van de meetwaarde, waarbij aantal, gemiddelde, in dit geval)|  Gemiddelde|
|aangepaste e-mailberichten (tekenreeksmatrix)|'foo@example.com','bar@example.com'|
|e-mail verzenden naar eigenaren, inzenders en lezers|    -SendToServiceOwners|

Een actie e-mailbericht maken

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Een actie Webhook maken

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De huidige regel maken op de meetwaarde CPU % op een klassieke VM

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

De huidige regel ophalen

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

De regel de waarschuwing toevoegen-cmdlet ook bijgewerkt als een waarschuwing regel al voor de opgegeven eigenschappen bestaat. Als u wilt een waarschuwing regel uitschakelt, bevatten de parameter **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Klik op controle logboekgebeurtenis Waarschuw

>[AZURE.NOTE] Deze functie is nog steeds in de Preview-versie.

In dit scenario wordt u e-mail verzenden wanneer u een website in mijn abonnement in resource groep *abhingrgtest123*is gestart.

Een e-mailregel instellen

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Een regel webhook instellen

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

De regel op de gebeurtenis maken

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

De huidige regel ophalen

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

De `Add-AlertRule` cmdlet kan diverse andere parameters. Meer informatie raadpleegt u [Toevoegen-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Een lijst met beschikbare aan de doelstellingen voor waarschuwingen ophalen
U kunt de `Get-AzureRmMetricDefinition` cmdlet om weer te geven van de lijst met alle parameters voor een specifieke resource.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Het volgende voorbeeld genereert een tabel met de naam van de meetwaarde en de eenheid voor deze.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Een volledige lijst met beschikbare opties voor `Get-AzureRmMetricDefinition` beschikbaar is binnen [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Maken en beheren van instellingen voor automatisch schalen
Een resource, zoals een Web-app, VM, Cloudservice of VM schaal instellen kan slechts één automatisch schalen instelling geconfigureerd voor deze hebben.
Elke instelling automatisch schalen kunt wel meerdere profielen. Bijvoorbeeld een profiel schaal op basis van prestaties en een tweede voor een planning op basis profiel. Elk profiel kunt meerdere regels op deze geconfigureerd hebben. Zie voor meer informatie over automatisch schalen, [hoe u automatisch schalen een toepassing](../cloud-services/cloud-services-how-to-scale.md).

Hier volgen de stappen die we gebruiken:

1. Maak (s).
2. Maak de regels die u hebt gemaakt eerder toewijzen aan de profielen profiel(en).
3. Optioneel: Maak meldingen voor automatisch schalen door webhook en e-eigenschappen te configureren.
4. Maak een instelling automatisch schalen met een naam op de doel-resource door toewijzen de profielen en de meldingen die u hebt gemaakt in de vorige stappen.

De volgende voorbeelden wordt u hoe u een instelling automatisch schalen voor een VM schaal instellen voor een Windows-besturingssysteem gebaseerd met behulp van de meetwaarde CPU-gebruik kunt maken.

Maak eerst een regel naar schalen, met een stijging van exemplaar tellen.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Maak vervolgens een regel naar schaal hebt aangemeld met een exemplaar tellen verkleinen.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Maak vervolgens een profiel voor de regels.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Maak een eigenschap webhook.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Maak de eigenschap melding voor de instelling automatisch schalen, waaronder e-mailbericht en de webhook die u eerder hebt gemaakt.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Ten slotte maakt u de instelling automatisch schalen om toe te voegen van het profiel dat u hiervoor hebt gemaakt.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Zie voor meer informatie over het beheren van instellingen voor automatisch schalen [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automatisch schalen geschiedenis
Het volgende voorbeeld ziet u hoe u recente automatisch schalen en waarschuwingen gebeurtenissen kunt bekijken. Gebruik de zoekfunctie van de log controle de geschiedenis automatisch schalen weergeven.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

U kunt de `Get-AzureRmAutoScaleHistory` cmdlet om op te halen automatisch schalen geschiedenis.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Zie [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx)voor meer informatie.

### <a name="view-details-for-an-autoscale-setting"></a>Details weergeven voor een instelling automatisch schalen
U kunt de `Get-Autoscalesetting` cmdlet om op te halen meer informatie over de instelling automatisch schalen.

Het volgende voorbeeld ziet meer informatie over alle automatisch schalen-instellingen in de resource groep 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Het volgende voorbeeld ziet u meer informatie over alle automatisch schalen-instellingen in de resource groep 'myrg1' en specifiek de automatisch schalen instelling met de naam 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Verwijderen van een instelling automatisch schalen
U kunt de `Remove-Autoscalesetting` cmdlet verwijderen van een instelling automatisch schalen.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Log profielen controlelogboeken beheren

U kunt een *logboek profiel* maken en gegevens exporteren vanuit uw controlelogboeken bijhouden met een opslag-account en kunt u Gegevensretentie configureren voor deze. Desgewenst kunt u de gegevens ook streamen op uw Hub gebeurtenis. Deze functie is momenteel in Preview en u kunt slechts één log profiel per abonnement maken. U kunt de volgende cmdlets gebruiken met uw huidige abonnement maken en beheren van log profielen. U kunt ook een bepaald abonnement kiezen. Hoewel PowerShell standaard het huidige abonnement, kunt u altijd wijzigen dat als u met `Set-AzureRmContext`. Controlelogboeken bijhouden gegevens voor het routeren naar eventuele opslag-account of gebeurtenis Hub binnen abonnement, kunt u configureren. Gegevens worden geschreven als blob-bestanden in de indeling van JSON.

### <a name="get-a-log-profile"></a>Een profiel log ophalen
Als u wilt ophalen van uw bestaande log-profielen, gebruikt u de `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Een profiel log zonder Gegevensretentie toevoegen

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een profiel log verwijderen

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Een profiel log met Gegevensretentie toevoegen

U kunt de eigenschap **- RetentionInDays** opgeven met het aantal dagen, als een positief geheel getal, waar de gegevens blijven behouden.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Log profiel met bewaren en EventHub toevoegen
Naast uw gegevens zoekresultaten omleiden naar een opslag-account, kunt u deze ook op een gebeurtenis Hub streamen. Houd er rekening mee dat in deze release preview en de opslag accountconfiguratie verplicht is, maar gebeurtenis Hub configuratie optioneel is.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Diagnostische logboeken configureren
Veel Azure services bieden extra logboeken en telemetrielogboek, inclusief beveiligingsgroepen Azure-netwerk, Software netwerktaakverdelers, sleutel kluis, Azure zoekservices, en logica Apps en deze kunnen worden geconfigureerd voor het opslaan van gegevens in uw opslagruimte van Azure-account. Deze bewerking kan alleen worden uitgevoerd op een niveau van de resource en het opslag-account aanwezig moet zijn in hetzelfde gebied, als de doel-resource waar de instelling diagnostische gegevens is geconfigureerd.

### <a name="get-diagnostic-setting"></a>Krijgen diagnostische bij het instellen

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnostische instelling uitschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnostische instelling zonder bewaarbeleid inschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnostische instelling met bewaarbeleid inschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Diagnostische instelling met bewaarbeleid voor een specifieke log categorie inschakelen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
