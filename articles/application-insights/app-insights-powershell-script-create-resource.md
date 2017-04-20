<properties 
    pageTitle="PowerShell-script om de bron van een toepassing inzichten te maken" 
    description="Het maken van toepassing inzichten resources automatiseren." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell-script om de bron van een toepassing inzichten te maken

*Er is een toepassing inzichten in de proefversie.*

Als u een nieuwe toepassing- of een nieuwe versie van een toepassing - met [Visual Studio toepassing inzichten](https://azure.microsoft.com/services/application-insights/)controleren wilt, stelt u een nieuwe resource in Microsoft Azure. Deze resource is waarop de telemetriegegevens uit uw app is geanalyseerd en weergegeven. 

U kunt het maken van een nieuwe resource automatiseren via PowerShell.

Bijvoorbeeld als u een mobiel apparaat-app ontwikkelt, er waarschijnlijk dat, op elk gewenst moment, er verschillende gepubliceerde versies van uw app door uw klanten wordt gebruikt worden. U wilt niet dat de resultaten telemetrielogboek ophalen uit verschillende versies verward. U krijgt zodat uw opbouwen proces voor het maken van een nieuwe resource voor elke opbouwen.

## <a name="script-to-create-an-application-insights-resource"></a>Script om de bron van een toepassing inzichten te maken

Zie de relevante cmdlet-specificaties:

* [Nieuwe AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Nieuwe AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell-Script*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Wat moet u doen met de iKey

Elke resource wordt aangegeven door de sleutel instrumentation (iKey). De iKey is een uitvoer van een script voor het maken van de resource. Het opbouwscript moet geven dat de iKey naar de toepassing inzichten SDK ingesloten in uw app.

Er zijn twee manieren om de iKey beschikbaar te maken de SDK:
  
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Of in [initialisatiecode](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Zie ook

* [Toepassing inzichten en web test resources maken van sjablonen](app-insights-powershell.md)
* [Cmdlets voor controle van Azure diagnostische gegevens met PowerShell instellen](app-insights-powershell-azure-diagnostics.md) 
* [Waarschuwingen instellen via PowerShell](app-insights-powershell-alerts.md)

 