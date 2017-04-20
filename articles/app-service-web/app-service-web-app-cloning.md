<properties
    pageTitle="Web App klonen via PowerShell"
    description="Leer hoe u uw Web-Apps naar nieuwe Web-Apps via PowerShell klonen."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service App klonen via PowerShell#

Met de versie van Microsoft Azure PowerShell versie 1.1.0 is optie om een nieuwe toegevoegd aan nieuw-AzureRMWebApp waaraan u de gebruiker de mogelijkheid om te klonen van een bestaande Web-App naar een nieuwe app in een andere regio of in dezelfde regio. Hiermee schakelt u klanten kunnen een aantal apps gebruiken tussen de verschillende regio's snel en gemakkelijk.

App klonen is momenteel alleen ondersteund voor premium laag app service-abonnementen. De nieuwe functie dezelfde beperkingen als functie van de back-up van Web Apps gebruikt, raadpleegt u [Back-up van een WebApp in Azure App-Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Meer informatie over het gebruik van Azure resourcemanager op basis van Azure PowerShell controleren cmdlets voor het beheren van uw Web-Apps [resourcemanager Azure op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Een bestaande App klonen ##

Scenario: Een bestaande web-app in de regio Zuid centraal ons, de gebruiker wil graag de inhoud naar een nieuwe web-app in de regio Noord centraal ons klonen. Dit kunt u doen met behulp van de Azure resourcemanager-versie van de PowerShell-cmdlet voor het maken van een nieuwe WebApp waarbij de optie - SourceWebApp.

U weet de naam van de resource-groep die de bron-web-app bevat, kunt we de volgende PowerShell-opdracht gebruiken om informatie van de bron web-app (in dit geval de bron-webapp naam):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Als u wilt maken op een nieuwe App-Service plannen, kunt wordt de opdracht Nieuw AzureRmAppServicePlan zoals in het volgende voorbeeld gebruiken

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Met de opdracht Nieuw AzureRmWebApp we kunnen de nieuwe web-app in de regio Noord centraal ons maken en deze koppelen aan een bestaande premium laag App-Service plannen, Daarnaast kunnen we dezelfde resourcegroep gebruiken als de bron-web-app of een nieuwe resourcegroep definiëren, het volgende voorbeeld toont die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Een bestaande web-app, inclusief alle bijbehorende implementatie klonen sleuven, de gebruiker moet de parameter IncludeSourceWebAppSlots, het gebruik van deze parameter met de opdracht Nieuw AzureRmWebApp ziet u de volgende PowerShell-opdracht:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Een bestaande web-app binnen dezelfde regio klonen, de gebruiker moet maken van een nieuwe resourcegroep en een nieuwe app-service plannen in dezelfde regio en vervolgens de volgende PowerShell-opdracht om te klonen de web-app

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Een bestaande App in een App-Service-omgeving klonen ##

Scenario: Een bestaande web-app in de regio Zuid centraal ons, de gebruiker wilt klonen van de inhoud naar een nieuwe web-app aan een bestaande App Service omgeving (ASE).

U weet de naam van de resource-groep die de bron-web-app bevat, kunt we de volgende PowerShell-opdracht gebruiken om informatie van de bron web-app (in dit geval de bron-webapp naam):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

U weet van de ASE naam en de naam van de resource-groep waartoe de ASE behoort, de gebruiker kan de opdracht Nieuw AzureRmWebApp gebruiken om te maken van de nieuwe web-app in de bestaande ASE, het volgende voorbeeld toont die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

De parameter locatie vanwege verouderde reden vereist is, maar bij het maken van een app in een ASE deze worden genegeerd. 

## <a name="cloning-an-existing-app-slot"></a>Een bestaand App-Slot klonen ##

Scenario: De gebruiker wilt klonen van een bestaande Web App Slot tot ofwel een nieuwe Web App of een nieuwe Web App-slot. De nieuwe Web-App kunt werken in hetzelfde gebied, als de oorspronkelijke Web App-slot of in een andere regio.

U weet de naam van de resource-groep die de bron-web-app bevat, kunt we de volgende PowerShell-opdracht gebruiken om de bron web app slot informatie (in dit geval de bron-webappslot naam) gekoppeld aan bron Web App-webapp te verkrijgen:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Het volgende voorbeeld toont klonen van de bron-web-app naar een nieuwe web-app:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Verkeer Manager tijdens het klonen van een App configureren ##

Meerdere landen/regio-WebApps maken en configureren van Azure verkeer Manager om verkeer te routeren naar alle deze WebApps, is een n belangrijke mogelijkheid om ervoor te zorgen dat klanten apps ten zeerste beschikbaar zijn, dat bij het klonen van een bestaande web-app hebt u de optie voor beide WebApps verbinden met een nieuw profiel van verkeer manager of een bestaande - Houd er rekening mee dat alleen Azure resourcemanager versie van verkeer Manager wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Een nieuw verkeer Manager-profiel maken tijdens een App klonen ###

Scenario: De gebruiker wil graag een web-app naar een andere regio, bij het configureren van een profiel Azure resourcemanager verkeer manager die bevatten beide WebApps klonen. Het volgende voorbeeld toont de bron-web-app naar een nieuwe web-app klonen tijdens het configureren van een nieuw verkeer Manager-profiel:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Nieuwe toevoegen in de browser gekopieerd naar een bestaand verkeer Manager-profiel ###

Scenario: De gebruiker al een manager-profiel voor Azure resourcemanager verkeer hij wilt toevoegen van beide WebApps als eindpunten. Hiervoor eerst moeten we samenstellen van de bestaande verkeer profiel-id voor manager, moeten we de abonnements-id, de naam van de resource-groep en de bestaande verkeer manager profielnaam.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Nadat ik de verkeer Manager-id, het volgende voorbeeld toont de bron-web-app naar een nieuwe web-app klonen terwijl deze toe te voegen aan een bestaand verkeer Manager-profiel:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Huidige beperkingen ##

Deze functie is momenteel in de proefversie, we werken als u wilt toevoegen van nieuwe mogelijkheden na verloop van tijd, de volgende lijst worden de bekende beperkingen voor de huidige versie van de app klonen:

- Automatisch schaalinstellingen worden niet gekopieerd
- Back-schema-instellingen worden niet gekopieerd.
- VNET instellingen worden niet gekopieerd.
- App inzichten worden niet automatisch ingesteld op de bestemming WebApp
- Eenvoudig Auth-instellingen worden niet gekopieerd.
- Kudu extensie niet worden gekopieerd
- TiP regels worden niet gekopieerd.
- Database-inhoud niet worden gekopieerd


### <a name="references"></a>Verwijzingen ###
- [Azure resourcemanager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App klonen met behulp van Azure-Portal](app-service-web-app-cloning-portal.md)
- [Back-up van een WebApp in Azure App-Service](web-sites-backup.md)
- [Azure resourcemanager ondersteuning voor Azure verkeer Manager Preview](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Inleiding tot het App-omgeving](app-service-app-service-environment-intro.md)
- [Via Azure PowerShell met Azure resourcemanager](../powershell-azure-resource-manager.md)
