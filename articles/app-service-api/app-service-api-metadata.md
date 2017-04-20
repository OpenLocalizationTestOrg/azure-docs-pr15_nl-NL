<properties
    pageTitle="App-Service API Apps metagegevens voor API discovery en code generatie | Microsoft Azure"
    description="Leer hoe API-Apps in Azure App Service Swagger metagegevens gebruiken om te vergemakkelijken API discovery en code generatie."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App-Service API Apps metagegevens voor API discovery en code genereren 

Ondersteuning voor [Swagger 2.0](http://swagger.io/) API metagegevens is ingebouwd in App Service API-Apps. U hoeft te Swagger gebruiken, maar als u deze gebruikt, kunt u profiteren van de Apps API-functies die discovery en verbruik gemakkelijker.   

## <a name="swagger-endpoint"></a>Eindpunt swagger

Een eindpunt waarmee Swagger 2.0 JSON-metagegevens voor een API-app in een eigenschap van de API-app, kunt u opgeven. Het eindpunt kan zijn ten opzichte van de basis-URL van de API-app of een absolute URL. Absolute URL's kunnen aanwijzen buiten de API-app. 

Veel volgende clients (voor bijvoorbeeld Visual Studio-code genereren en PowerApps 'Toevoegen API' stroom), de URL moet openbaar (niet beveiligd door de gebruiker of service verificatie). Dit betekent dat als u App Service verificatie gebruikt en u wilt laten zien de API-definitie van binnen de app zelf, u gebruik verificatieoptie waarmee anonieme verkeer moet naar uw API hebt bereikt. Zie [verificatie en autorisatie voor App Service API-Apps](app-service-api-authentication.md)voor meer informatie.

### <a name="portal-blade"></a>Portal blade

In de [portal van Azure](https://portal.azure.com/) kunt de URL van het eindpunt worden gezien en gewijzigd op het blad **API definitie** .

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure resourcemanager eigenschap

U kunt ook de URL van de definitie API voor een API-app configureren met behulp van de [Resource Explorer](https://resources.azure.com/) of [Azure resourcemanager sjablonen](../resource-group-authoring-templates.md) in hulpmiddelen voor de opdrachtregel zoals [Azure PowerShell](../powershell-install-configure.md) en de [Azure CLI](../xplat-cli-install.md). 

Ga naar in **Resource Explorer** **abonnementen > {uw abonnement} > resourceGroups > {uw resourcegroep} > providers > Microsoft.Web > sites > {uw site} > config > web**, en ziet u de `apiDefinition` eigenschap:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Voor een voorbeeld van een sjabloon Azure resourcemanager die Hiermee stelt u de `apiDefinition` eigenschap, opent u het [bestand azuredeploy.json in de toepassing van de steekproef takenlijst te klikken](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Zoek de sectie van de sjabloon die op de JSON-voorbeeld hierboven lijkt.

### <a name="default-value"></a>Standaardwaarde

Wanneer u een API-app maken met Visual Studio, het eindpunt van de definitie API automatisch is ingesteld op de basis-URL van de app API plus `/swagger/docs/v1`. Dit is de standaard-URL waarin het pakket [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet voor dynamisch gegenereerde Swagger metagegevens voor een project ASP.NET Web API dienen wordt gebruikt. 

## <a name="code-generation"></a>Code genereren

Een van de voordelen van de integratie van Swagger in Azure-API-apps is automatische code genereren. Gegenereerde client klassen gemakkelijker code schrijven die een app API-oproepen.

U kunt clientcode voor een app API genereren met behulp van Visual Studio of vanaf de opdrachtregel. Zie [aan de slag met de API-Apps en ASP.NET](app-service-api-dotnet-get-started.md#codegen)voor informatie over het genereren van client klassen in Visual Studio voor een ASP.NET Web API-project. Zie voor informatie over hoe u deze vanaf de opdrachtregel voor alle ondersteunde talen, het Leesmij-bestand van de bibliotheek [Azure/autorest](https://github.com/azure/autorest) op GitHub.com.
 
## <a name="next-steps"></a>Volgende stappen

Zie [aan de slag met API-Apps in Azure App-Service](app-service-api-dotnet-get-started.md)voor een stapsgewijze zelfstudie waarin u helpt bij het maken, implementeert en gebruik een API-app.

Als u Azure API Management met API Apps gebruikt, kunt u Swagger metagegevens importeren de API in API-Management. Zie voor meer informatie [over het importeren van de definitie van een API met bewerkingen in Azure API Management](../api-management/api-management-howto-import-api.md). 
