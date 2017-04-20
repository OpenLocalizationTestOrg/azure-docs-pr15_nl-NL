<properties
    pageTitle="PowerShell gebruiken voor setup-toepassing inzichten in een Azure | Microsoft Azure"
    description="Automatiseren diagnostisch hulpprogramma Azure naar pijp inzicht krijgen in toepassing configureren."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>PowerShell gebruiken voor het instellen van toepassing inzichten voor een Azure web-app

[Microsoft Azure](https://azure.com) kan zijn [geconfigureerd voor het verzenden van Azure diagnostisch hulpprogramma](app-insights-azure-diagnostics.md) inzicht krijgen in [Visual Studio toepassing](app-insights-overview.md). De diagnostische hulpprogramma's koppelen aan Azure-Cloudservices en Azure VMs. Ze aanvullen het telemetrielogboek die u verzendt vanuit de app met de toepassing inzichten SDK. Als onderdeel van het proces van het maken van nieuwe resources in Azure automatiseren, kunt u diagnostische gegevens via PowerShell.

## <a name="azure-template"></a>Azure-sjabloon

Als de WebApp wordt uitgevoerd in Azure wordt aangegeven en u uw resources met een sjabloon van Azure resourcemanager maakt, kunt u de toepassing inzichten configureren door deze naar het knooppunt resources toe te voegen:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-een naam voor de resource van toepassing inzichten
* `myWebAppName`-de-id van de web-app


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Diagnostisch hulpprogramma extensie als onderdeel van het implementeren van een Cloudservice inschakelen

De `New-AzureDeployment` cmdlet heeft een parameter `ExtensionConfiguration`, die een matrix met hulpprogramma's voor diagnose configuraties duurt. Deze kunnen worden gemaakt met de `New-AzureServiceDiagnosticsExtensionConfig` cmdlet. Bijvoorbeeld:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnostisch hulpprogramma extensie op een bestaande Cloudservice inschakelen

Gebruik op een bestaande service, `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Configuratie van de extensie huidige diagnostische ophalen

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Diagnostisch hulpprogramma extensie verwijderen

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Als u de hulpprogramma's voor diagnose extensie gebruikmaakt van een ingeschakeld `Set-AzureServiceDiagnosticsExtension` of `New-AzureServiceDiagnosticsExtensionConfig` zonder de parameter rol, klikt u vervolgens kunt u het gebruik van de extensie `Remove-AzureServiceDiagnosticsExtension` zonder de rol-parameter. Als de parameter rol is gebruikt bij het inschakelen van de extensie moet vervolgens deze ook worden gebruikt bij het verwijderen van de extensie.

De extensie diagnostische gegevens van elke afzonderlijke rol verwijderen:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Zie ook

* [Azure bewaken Cloud Services-apps gebruiken met de toepassing inzichten](app-insights-cloudservices.md)
* [Azure diagnostisch hulpprogramma verzenden naar toepassing inzichten](app-insights-azure-diagnostics.md)
* [Automatiseren waarschuwingen configureren](app-insights-powershell-alerts.md)

