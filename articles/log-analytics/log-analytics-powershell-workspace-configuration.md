<properties
    pageTitle="PowerShell gebruiken om te maken en configureren van een logboek Analytics-werkruimte | Microsoft Azure"
    description="Meld u Analytics-gegevens voor gebruik van servers in uw on-premises of infrastructuur cloud. U kunt machine gegevens verzamelen van Azure opslagruimte wanneer gegenereerd door Azure diagnostische gegevens."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Log Analytics via PowerShell beheren

U kunt de [Log Analytics PowerShell-cmdlets] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) naar verschillende functies in Log analyses uitvoeren vanaf de opdrachtregel of als onderdeel van een script.  Voorbeelden van taken die u met PowerShell uitvoeren kunt zijn:

+ Een werkruimte maken
+ Toevoegen of verwijderen van een oplossing
+ Importeren en exporteren van opgeslagen zoekopdrachten
+ Een computergroep maken
+ Verzameling IIS-logboeken van computers met de Windows-agent is geïnstalleerd inschakelen
+ Items verzamelen van Linux en Windows-computers
+ Gebeurtenissen op syslog verzamelen op Linux-computers 
+ Gebeurtenissen op Windows-Logboeken verzamelen
+ Aangepaste gebeurtenis logboeken verzamelen
+ De log analytics-agent toevoegen aan een Azure virtuele machines
+ Log analyses met indexgegevens verzameld met behulp van Azure diagnostische gegevens configureren


Dit artikel bevat twee voorbeelden van de code die illustreren enkele van de functies die u vanuit PowerShell uitvoeren kunt.  U kunt verwijzen naar de [Log Analytics PowerShell-cmdlet verwijzing] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) voor andere functies.

> [AZURE.NOTE] Log Analytics heet eerder operationele inzichten, dat wil zeggen waarom dit de naam die in de cmdlets is.

## <a name="prerequisites"></a>Vereisten voor

Als PowerShell wilt gebruiken met uw werkruimte Log analyses, moet u het volgende hebben:

+ Een Azure-abonnement en 
+ Uw werkruimte Azure Log Analytics gekoppeld aan uw Azure-abonnement.

Als u een werkruimte OMS hebt gemaakt, maar deze is niet nog hebt gekoppeld aan een Azure-abonnement kunt u de koppeling kunt maken:

+ In de portal van Azure
+ Klik in de portal OMS of 
+ Gebruik de cmdlets Get-AzureRmOperationalInsightsLinkTargets en nieuw-AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Maken en configureren van een logboek Analytics-werkruimte

Het volgende script voorbeeld ziet u hoe u:

1.  Een werkruimte maken
2.  Lijst van de beschikbare oplossingen
3.  Oplossingen naar de werkruimte toevoegen
4.  Importeren die zijn opgeslagen zoekopdrachten
5.  Exporteren opgeslagen zoekopdrachten
6.  Een computergroep maken
7.  Verzameling IIS-logboeken van computers met de Windows-agent is geïnstalleerd inschakelen
8.  Logische schijf prestatiemeteritems verzamelen van Linux computers (% Inodes gebruikt; Gratis Megabytes; % Gebruikt ruimte; Schijf overdrachten/sec; Schijf lezen per seconde; Schijf schrijven per seconde)
9.  Syslog gebeurtenissen verzamelen van Linux-computers
10. Fout en waarschuwing gebeurtenissen uit het gebeurtenislogboek van Windows-computers verzamelen
11. Beschikbare MB geheugen prestatie-item verzamelen van Windows-computers
12. Een aangepaste logboek verzamelen 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics Azure diagnostisch hulpprogramma indexeren configureren 

Voor agentless cmdlets voor controle van Azure bronnen, moeten de resources Azure diagnostisch hulpprogramma ingeschakeld en geconfigureerd voor het schrijven met een opslag-account hebt. Log Analytics kunt kan worden geconfigureerd om de logboeken verzamelen van het account opslag. Bronnen die u moet doen van de voorgaande configuratie voor bevat:

+ Klassieke cloudservices (web en werknemer rollen)
+ Service stof clusters
+ Beveiligingsgroepen netwerk
+ Belangrijke kluizen en 
+ Toepassingsgateways

U kunt ook PowerShell gebruiken voor het configureren van een werkruimte Log Analytics in één Azure abonnement logboeken verzamelen uit verschillende Azure abonnementen.

Het volgende voorbeeld wordt getoond hoe u:

1.  De bestaande opslag-accounts en locaties die Log Analytics indexeert gegevens uit een lijst met
2.  Een configuratie te lezen van een opslag-account maken
3.  De zojuist gemaakte configuratie bijwerken om gegevens te indexeren vanaf andere locaties
4.  De zojuist gemaakte configuratie verwijderen

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Volgende stappen

- [Controleren Log Analytics PowerShell-cmdlets](http://msdn.microsoft.com/library/mt188224.aspx) voor meer informatie over het gebruik van PowerShell voor de configuratie van Log Analytics.

