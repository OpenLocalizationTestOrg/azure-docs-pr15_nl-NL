<properties
    pageTitle="Azure resourcemanager gebaseerde platforms opdrachtregel hulpmiddelen voor Azure Web-App | Microsoft Azure"
    description="Informatie over het gebruik van de nieuwe Azure resourcemanager gebaseerde platforms opdrachtregel hulpmiddelen voor het beheren van uw Azure-Web-Apps."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Met behulp van Azure Resource Manager gebaseerde XPlat CLI voor Azure WebApp#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Met de versie van Microsoft Azure platforms voor hulpmiddelen voor versie 0.10.5, zijn nieuwe opdrachten toegevoegd. Deze opdrachten krijgt de gebruiker de mogelijkheid om te gebruiken op basis van Azure resourcemanager PowerShell-opdrachten voor het beheren van Web Apps.

Voor meer informatie over het beheren van resourcegroepen, raadpleegt u [de CLI Azure als u wilt beheren Azure resources en resourcegroepen gebruiken](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Abonnementen van App-Service beheren ##

### <a name="create-an-app-service-plan"></a>Een App Service maken ###
Als u wilt een abonnement dat is app maken, gebruikt u de opdracht **azure appserviceplan maken** .

Beschrijvingen van de verschillende parameters Hier volgen:

-   **--resourcegroep**: resourcegroep waarin het abonnement dat nieuwe app.
-   **--naam**: naam van de app-abonnement.
-   **--locatie**: app-service plannen locatie.
-   **--laag**: de gewenste prijzen sku (de opties zijn: F1 (gratis). D1 (gedeeld). B1 (eenvoudige klein), B2 (eenvoudige gemiddeld), en B3 (Basic grote). S1 (standaard klein), S2 (standaard gemiddeld), en S3 (standaard grote). P1 (Premium klein), P2 (Premium gemiddeld), en P3 (Premium grote).)
-   **--exemplaren**: het aantal werknemers in de app service-abonnement (standaardwaarde is 1).

Gebruik deze cmdlet voorbeeld:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Lijst met bestaande App-Service plannen ###

U kunt bestaande app service-abonnementen gebruiken de optie **azure appserviceplan lijst** .

U kunt alle app-abonnementen onder een bepaalde resourcegroep gebruiken:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Als u een bepaalde app serviceplan, gebruikt u de opdracht **azure appserviceplan weergeven** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Een bestaand App-Service plannen configureren ###

Gebruik de opdracht **azure appserviceplan config** de instellingen voor een bestaand app-abonnement wilt wijzigen. U kunt wijzigen welke sku en het aantal werknemers 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Een App-serviceplan schaalbaarheid ####

Als u een bestaand App-Service plannen wilt verkleinen, gebruiken:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>De SKU van een abonnement dat is App wijzigen ####

Als u wilt wijzigen welke sku van een bestaand App-Service plannen, gebruiken:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Verwijderen van een bestaand App-Service plannen ###

Als u wilt een bestaand app-serviceplan verwijderen, moeten alle toegewezen WebApps worden verplaatst of verwijderd eerst. Gebruik vervolgens de opdracht **verwijderen van azure webapp** dat kunt u het abonnement dat app verwijderen.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>App-Service WebApps beheren ##

### <a name="create-a-web-app"></a>Een WebApp maken ###

Als u wilt een WebApp maken, gebruikt u de opdracht **azure webapp maken** .

Beschrijvingen van de verschillende parameters Hier volgen:

- **--naam**: naam voor de web-app.
- **--abonnement**: naam voor het abonnement dat is gebruikt voor het hosten van de web-app.
- **--resourcegroep**: resourcegroep waarop het App-abonnement.
- **--locatie**: de weblocatie voor de app.

Gebruik deze cmdlet voorbeeld:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Een bestaande Web-App verwijderen ###

Verwijderen van een bestaande web-app kunt u de opdracht **azure webapp verwijderen** , moet u de naam van de web-app en de naam van de resource-groep opgeven.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Lijst met bestaande Web Apps ###

Als u de bestaande web-apps, gebruikt u de opdracht **azure webapp lijst** .

U kunt alle WebApps onder een bepaalde resourcegroep gebruiken:

    azure webapp list --resource-group ContosoAzureResourceGroup

Als u een specifieke web-app, gebruikt u de opdracht **azure webapp weergeven** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Een bestaande Web-App configureren ###

Als u wilt wijzigen de gewenste instellingen en configuraties voor een bestaande web-app, gebruikt u de opdracht **azure webapp config instellen** .

Voorbeeld (1): de php-versie van een web-app wijzigen 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Voorbeeld (2): toevoegen of wijzigen van de app-instellingen

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Weten wat andere configuratie kan worden gewijzigd, gebruikt u de opdracht **azure webapp config -h instellen** .

### <a name="change-the-state-of-an-existing-web-app"></a>De status van een bestaande Web-App wijzigen ###

#### <a name="restart-a-web-app"></a>Een web-app opnieuw te starten ####

Als wilt starten een web-app, moet u de groep en de bron van de web-app.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Stoppen met een web-app ####

Als u wilt stoppen met een web-app, moet u de groep en de bron van de web-app.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Een WebApp starten ####

Als u wilt een web-app, moet u de groep en de bron van de web-app.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>App webpublicaties profielen beheren ###

Elke WebApp heeft een publicerende profiel dat kan worden gebruikt voor het publiceren van uw apps.

#### <a name="get-publishing-profile"></a>Krijg profiel ####

Als u de publicatie-profiel voor een web-app, gebruikt u:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Deze opdracht een echo publicerende profiel-gebruikersnaam en wachtwoord voor de opdrachtregel.

### <a name="manage-web-app-hostnames"></a>Hostnamen Web App beheren ###

Als u wilt beheren hostname bindingen voor uw web-app, gebruikt u de opdracht **azure webapp config hostnamen**  

#### <a name="list-hostname-bindings"></a>Lijst hostname bindingen ####

Als u de huidige hostname bindingen voor een web-app, gebruikt u:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname bindingen toevoegen ####

Als u wilt hostname bindingen toevoegen aan een web-app, gebruiken:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname bindingen verwijderen ####

Als u wilt verwijderen hostname bindingen, gebruiken:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Volgende stappen ###
- Zie voor meer informatie over ondersteuning voor Azure resourcemanager CLI [de CLI Azure gebruiken voor het beheren van Azure resources en resourcegroepen.](../xplat-cli-azure-resource-manager.md)
- Zie voor meer informatie over het beheren van App-Service via PowerShell, [Using Azure Resource Manager-Based PowerShell voor het beheren van Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
