<properties
    pageTitle="Azure resourcemanager gebaseerde PowerShell-opdrachten voor Azure Web-App | Microsoft Azure"
    description="Informatie over het gebruik van de nieuwe Azure resourcemanager gebaseerde PowerShell-opdrachten voor het beheren van uw Azure-Web-Apps."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure Resource Manager gebaseerde PowerShell gebruiken voor het beheren van Azure WebApps#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Met Microsoft Azure PowerShell versie 1.0.0 zijn nieuwe opdrachten toegevoegd, die de gebruiker de mogelijkheid om te gebruiken op basis van Azure resourcemanager PowerShell-opdrachten voor het beheren van Web Apps geven.

Zie voor meer informatie over het beheren van resourcegroepen, [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md). 

Meer informatie over de volledige lijst met opties voor de PowerShell-cmdlets en parameters, raadpleegt u de [Volledige Cmdlet verwijzing van Web App Azure resourcemanager gebaseerde PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Abonnementen van App-Service beheren ##

### <a name="create-an-app-service-plan"></a>Een App Service maken ###
Als u wilt een abonnement dat is app maken, gebruikt u de cmdlet **New-AzureRmAppServicePlan** .

Beschrijvingen van de verschillende parameters Hier volgen:

-   **Naam**: naam van de app-abonnement.
-   **Locatie**: service-abonnement locatie.
-   **ResourceGroupName**: resourcegroep waarin het abonnement dat nieuwe app.
-   **Laag**: de gewenste prijzen laag (standaard is gratis, andere opties zijn gedeeld, Basic, Standard en Premium)
-   **WorkerSize**: de grootte van werknemers (standaard is kleine als de parameter laag is opgegeven als Basic, Standard of Premium. Andere opties zijn normaal en groot.)
-   **NumberofWorkers**: het aantal werknemers in de app service-abonnement (standaardwaarde is 1). 

Gebruik deze cmdlet voorbeeld:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Een App Service maken in een App Service-omgeving ###
Als u wilt maken van een app-abonnement in een omgeving met app-service, gebruikt u de opdracht voor **Nieuw-AzureRmAppServicePlan** van dezelfde opdracht met extra parameters van de ASE naam en de naam van de groep resource van ASE te geven.

Gebruik deze cmdlet voorbeeld:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Meer informatie over het app-omgeving controleren [Inleiding tot het App-omgeving](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Lijst met bestaande App-Service plannen ###

Als u de bestaande app service-abonnementen, gebruikt u de cmdlet **Get-AzureRmAppServicePlan** .

U alle app-abonnementen onder uw abonnement te gebruiken: 

    Get-AzureRmAppServicePlan

U kunt alle app-abonnementen onder een bepaalde resourcegroep gebruiken:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Als u een bepaalde app serviceplan, gebruikt u:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Een bestaand App-Service plannen configureren ###

Gebruik de cmdlet **Set-AzureRmAppServicePlan** de instellingen voor een bestaand app-abonnement wilt wijzigen. U kunt de laag, de grootte van de werknemer en het aantal werknemers wijzigen 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Een App-serviceplan schaalbaarheid ####

Als u een bestaand App-Service plannen wilt verkleinen, gebruiken:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Wijzigen van de werknemer-grootte van een App-Service plannen ####

Als u wilt wijzigen van de grootte van werknemers in een bestaand App-Service plannen, gebruiken:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>De laag van een abonnement dat is App wijzigen ####

Als u wilt wijzigen van de laag van een bestaand App-Service plannen, gebruiken:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Verwijderen van een bestaand App-Service plannen ###

Als u wilt een bestaand app-serviceplan verwijderen, moeten alle toegewezen WebApps worden verplaatst of verwijderd eerst. Gebruik vervolgens de **Verwijderen-AzureRmAppServicePlan** -cmdlet kunt u het abonnement dat app verwijderen.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>App-Service WebApps beheren ##

### <a name="create-a-web-app"></a>Een WebApp maken ###

Als u wilt een WebApp maken, gebruikt u de cmdlet **New-AzureRmWebApp** .

Beschrijvingen van de verschillende parameters Hier volgen:

- **Naam**: naam voor de web-app.
- **AppServicePlan**: naam voor het abonnement dat is gebruikt voor het hosten van de web-app.
- **ResourceGroupName**: resourcegroep waarop het App-abonnement.
- **Locatie**: de weblocatie voor de app.

Gebruik deze cmdlet voorbeeld:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Een WebApp maakt in een App Service-omgeving ###

U maakt een web-app in een App Service-omgeving (ASE). De dezelfde **Nieuw AzureRmWebApp** -opdracht met extra parameters gebruiken om op te geven van de naam van de ASE en de naam van de resource-groep waartoe de ASE behoort.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Meer informatie over het app-omgeving controleren [Inleiding tot het App-omgeving](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Een bestaande Web-App verwijderen ###

Verwijderen van een bestaande web-app kunt u de cmdlet **Verwijderen-AzureRmWebApp** , moet u de naam van de web-app en de naam van de resource-groep opgeven.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Lijst met bestaande Web Apps ###

Als u de bestaande web-apps, gebruikt u de cmdlet **Get-AzureRmWebApp** .

U alle WebApps onder uw abonnement te gebruiken:

    Get-AzureRmWebApp

U kunt alle WebApps onder een bepaalde resourcegroep gebruiken:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Als u een specifieke web-app, gebruikt u:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Een bestaande Web-App configureren ###

Als u wilt wijzigen de gewenste instellingen en configuraties voor een bestaande web-app, gebruikt u de cmdlet **Set-AzureRmWebApp** . Voor een volledige lijst met parameters, raadpleegt u de [Cmdlet-koppeling](https://msdn.microsoft.com/library/mt652487.aspx)

Voorbeeld (1): Gebruik deze cmdlet verbindingstekenreeksen met de wijzigen

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Voorbeeld (2): toevoegen of wijzigen van de app-instellingen

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Voorbeeld (3): de web-app om uit te voeren in de 64-bits modus instellen

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>De status van een bestaande Web-App wijzigen ###

#### <a name="restart-a-web-app"></a>Een web-app opnieuw te starten ####

Als wilt starten een web-app, moet u de groep en de bron van de web-app.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Stoppen met een web-app ####

Als u wilt stoppen met een web-app, moet u de groep en de bron van de web-app.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Een WebApp starten ####

Als u wilt een web-app, moet u de groep en de bron van de web-app.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>App webpublicaties profielen beheren ###

Elke WebApp is een publicatie profiel dat kan worden gebruikt voor het publiceren van uw apps, meerdere bewerkingen kunnen worden uitgevoerd op het publiceren van profielen.

#### <a name="get-publishing-profile"></a>Krijg profiel ####

Als u de publicatie-profiel voor een web-app, gebruikt u:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Deze opdracht een echo het publicerende profiel aan de opdrachtregel als u ook uitvoerbereik het profiel voor publiceren naar een tekstbestand.

#### <a name="reset-publishing-profile"></a>Opnieuw publiceren profiel ####

Als u beide opnieuw wilt implementeren de publicerende wachtwoord voor FTP- en web voor een web-app, gebruiken:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Certificaten van Web App beheren ###

Zie voor meer informatie over het beheren van web-app certificaten, [SSL-certificaten binding via PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Volgende stappen ###
- Zie voor meer informatie over ondersteuning voor Azure resourcemanager PowerShell [Azure PowerShell gebruiken met Azure resourcemanager.](../powershell-azure-resource-manager.md)
- Zie meer informatie over App serviceomgevingen, [Inleiding tot het App-omgeving.](app-service-app-service-environment-intro.md)
- Zie voor meer informatie over het beheren van App Service SSL-certificaten via PowerShell, [SSL-certificaten binding via PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Zie meer informatie over de volledige lijst met Azure resourcemanager gebaseerde PowerShell-cmdlets voor Azure Web Apps, [verwijzing naar Azure-cmdlets van Web Apps Azure resourcemanager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Zie voor meer informatie over het beheren van App-Service met CLI, [Using Azure Resource Manager-Based XPlat CLI voor Azure Web App](app-service-web-app-azure-resource-manager-xplat-cli.md)
