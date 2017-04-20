<properties
    pageTitle="Diagnostische gegevens in Azure-Cloudservices via PowerShell inschakelen | Microsoft Azure"
    description="Informatie over het inschakelen van diagnostische hulpprogramma's voor cloudservices via PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Diagnostische gegevens in Azure-Cloudservices via PowerShell inschakelen

U kunt diagnostische gegevens zoals toepassingslogboeken verzamelen prestatie-item enzovoort vanuit een Cloudservice met de extensie diagnostisch hulpprogramma Azure. In dit artikel wordt beschreven hoe de extensie diagnostisch hulpprogramma Azure inschakelen voor een Cloudservice via PowerShell.  Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die u nodig hebt voor dit artikel.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Diagnostisch hulpprogramma extensie als onderdeel van het implementeren van een Cloudservice inschakelen

Deze benadering van goede voor doorlopende Integratietype scenario's waarin de extensie diagnostische hulpprogramma's kan worden ingeschakeld als onderdeel van het implementeren van de cloudservice. Bij het maken van een nieuwe Cloudservice-implementatie kunt u de extensie diagnostische hulpprogramma's inschakelen door doorgeven in de *ExtensionConfiguration* -parameter voor de cmdlet [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) . De parameter *ExtensionConfiguration* neemt een matrix van diagnostische gegevens configuraties die kan worden gemaakt met de cmdlet [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

Het volgende voorbeeld ziet hoe u kunt inschakelen diagnostische hulpprogramma's voor een cloudservice met een WebRole en WorkerRole elk met de configuratie van een andere diagnostische.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Als het configuratiebestand diagnostisch hulpprogramma Hiermee geeft u een element StorageAccount met een opslagaccountnaam, klikt u vervolgens in de cmdlet New-AzureServiceDiagnosticsExtensionConfig automatisch dat opslag-account gebruikt. Dit werkt, moet het account opslagruimte in hetzelfde abonnement als de Cloudservice die wordt geïmplementeerd.

Van Azure SDK 2,6 bewerken de extensie configuratiebestanden gegenereerd door het MSBuild publiceren bevat Doeluitvoer de naam van het opslag-account op basis van het hulpprogramma's voor diagnose configuratietekenreeks die is opgegeven in het configuratiebestand service (.cscfg). Het script hieronder ziet u hoe u parseren van de extensie configuratiebestanden uit de Doeluitvoer publiceren en configureren van de extensie diagnostische hulpprogramma's voor elke rol bij het distribueren van de cloudservice.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online is, wordt een soortgelijke benadering voor geautomatiseerde deploymnts van Cloudservices met de extensie diagnostische hulpprogramma's gebruikt. Zie [Publiceren-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) voor een volledig voorbeeld.

Als er geen StorageAccount is opgegeven in de configuratie van de diagnostische, moet u in de parameter StorageAccountName om aan te geven de cmdlet. Als de parameter StorageAccountName is opgegeven, klikt u vervolgens in de cmdlet altijd het opslag-account dat is opgegeven in de parameter en niet de een die is opgegeven in het configuratiebestand diagnostische hulpprogramma's gebruiken.

Als het account van de opslagruimte diagnostische gegevens in een ander abonnement van de Cloudservice, moet u expliciet in de StorageAccountName en StorageAccountKey parameters om aan te geven de cmdlet. De parameter StorageAccountKey is niet nodig wanneer het hulpprogramma's voor diagnose opslag-account in hetzelfde abonnement, zoals de cmdlet kan automatisch uitvoeren en de sleutelwaarde instellen wanneer de extensie diagnostische hulpprogramma's inschakelen. Echter als het account van de opslagruimte diagnostische gegevens in een ander abonnement, klikt u vervolgens de cmdlet mogelijk niet automatisch ophalen van de sleutel en moet u de sleutel tot en met de parameter StorageAccountKey expliciet opgeven.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnostisch hulpprogramma extensie op een bestaande Cloudservice inschakelen

U kunt de cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) inschakelen of werk deze bij de configuratie van de diagnostische op een Cloudservice die al wordt uitgevoerd.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Configuratie van de extensie huidige diagnostische ophalen
Gebruik de cmdlet [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) om de configuratie van de huidige diagnostische hulpprogramma's voor een cloudservice.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Diagnostisch hulpprogramma extensie verwijderen
Als u wilt uitschakelen diagnostische gegevens in een cloudservice kunt u de cmdlet [Verwijderen-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Als u de hulpprogramma's voor diagnose extensie met *Set-AzureServiceDiagnosticsExtension* of de *Nieuw AzureServiceDiagnosticsExtensionConfig* zonder de parameter *rol* ingeschakeld kunt u de extensie *Verwijderen-AzureServiceDiagnosticsExtension* gebruiken zonder de parameter *rol* verwijderen. Als de parameter *rol* is gebruikt bij het inschakelen van de extensie moet vervolgens deze ook worden gebruikt bij het verwijderen van de extensie.

De extensie diagnostische gegevens van elke afzonderlijke rol verwijderen:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Volgende stappen

- Zie [Diagnostisch hulpprogramma inschakelen in Azure Cloud Services en virtuele Machines](cloud-services-dotnet-diagnostics.md)voor aanvullende richtlijnen over het gebruik van Azure diagnostisch hulpprogramma en andere technieken voor het oplossen van problemen.
- Het [Hulpprogramma's voor diagnose configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) beschrijft de verschillende XML-configuraties opties voor de extensie diagnostische gegevens.
- Meer informatie over het inschakelen van de extensie diagnostische hulpprogramma's voor virtuele Machines, raadpleegt u [een Windows virtuele machine controle en diagnostische gegevens met behulp van Azure resourcemanager sjabloon maken](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
