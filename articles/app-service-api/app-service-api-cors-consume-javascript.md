<properties
    pageTitle="CORS ondersteuning in App Service | Microsoft Azure"
    description="Informatie over het gebruik van CORS-ondersteuning in Azure Azure App-Service."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Een app API in JavaScript met CORS gebruiken

App-Service biedt ingebouwde ondersteuning voor [Meerdere Origin Resource delen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), waarmee JavaScript-clients om te maken tussen domeinen oproepen naar API's die worden gehost in API-apps. App Service kunt u CORS toegang tot de API zonder het schrijven van een code, die in uw API configureren.

In dit artikel bevat twee secties:

* De sectie [voor het configureren van CORS](#corsconfig) wordt uitgelegd in het algemeen CORS configureren voor een API-app, in de browser of mobiele app. Dit geldt gelijkmatig voor alle kaders die worden ondersteund door de Service-App, inclusief .NET, Node.js en Java. 

* Beginnen met de sectie [de zelfstudies .NET introductie doorlopende](#tutorialstart) , is het artikel een zelfstudie die laat dat CORS ondersteuning door u te maken zien op wat u [de eerste API-Apps aan](app-service-api-dotnet-get-started.md)de slag te gaan zelfstudie hebt. 

## <a id="corsconfig"></a>CORS configureren in Azure App-Service

U kunt CORS configureren in de portal van Azure of met behulp van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) hulpmiddelen.

#### <a name="configure-cors-in-the-azure-portal"></a>CORS configureren in de portal van Azure

8. In een browser gaat u naar de [Azure-portal](https://portal.azure.com/).

2. Klik op **App-Services**en klik vervolgens op de naam van uw API-app.

    ![Selecteer API-app in de portal](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Zoek de sectie **API** in het blad **Instellingen** dat wordt geopend aan de rechterkant van het blad **API-app** , en klik vervolgens op **CORS**.

    ![Selecteer CORS in instellingen blade](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Vak Voer de URL of de URL's die u toestaan dat JavaScript-oproepen wilt naar afkomstig zijn uit in de tekst.


    Als u uw JavaScript-toepassing naar een web-app met de naam todolistangular geïmplementeerd, voert u bijvoorbeeld 'https://todolistangular.azurewebsites.net'. Als alternatief, kunt u een sterretje (*) als u wilt opgeven dat alle origin domeinen worden geaccepteerd.


13. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nadat u op **Opslaan**klikt, wordt de API-app JavaScript-gesprekken van de opgegeven URL's accepteren.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>CORS configureren met behulp van Azure resourcemanager hulpmiddelen

U kunt ook CORS configureren voor een API-app met behulp van [Azure resourcemanager sjablonen](../resource-group-authoring-templates.md) in hulpmiddelen voor de opdrachtregel zoals [Azure PowerShell](../powershell-install-configure.md) en de [Azure CLI](../xplat-cli-install.md). 

Open het [bestand azuredeploy.json in de bibliotheek voor de steekproef-toepassing van deze zelfstudie](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json)voor een voorbeeld van een sjabloon Azure resourcemanager die de eigenschap CORS. Zoek de sectie van de sjabloon die op het volgende voorbeeld lijkt:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>De zelfstudie .NET introductie doorlopende

Als u de Node.js of Java introductie reeks voor API-apps volgt, kunt u de ophalen gestart reeks hebt voltooid. Ga naar het gedeelte van de [volgende stappen](#next-steps) voor suggesties voor meer informatie over API-Apps.

De rest van dit artikel is een vervolg van de reeks .NET introductie en wordt ervan uitgegaan dat u [de eerste zelfstudie](app-service-api-dotnet-get-started.md)is voltooid.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Het project ToDoListAngular implementeren naar een nieuwe WebApp

In [de eerste zelfstudie](app-service-api-dotnet-get-started.md)gemaakt u een van de middelste laag API-app en een gegevens laag API-app. In deze zelfstudie maakt u een web-app van één pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE) dat oproepen de middelste laag API-app. Voor de beveiligd-WACHTWOORDVERIFICATIE werken u hebt CORS inschakelen voor de middelste laag API-app. 

Klik in de [takenlijst van voorbeeld-toepassing](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)is het project ToDoListAngular een eenvoudige AngularJS-client die de middelste laag ToDoListAPI Web API-project-oproepen. De JavaScript-code in het bestand *app/scripts/todoListSvc.js* roept de API met behulp van de AngularJS HTTP-provider. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Een nieuwe WebApp voor het project ToDoListAngular maken

De procedure een nieuwe App Service web-app maken en implementeren van een project ernaar is vergelijkbaar met wat u voor het [maken en implementeren van een API-app in de eerste zelfstudie in deze reeks](app-service-api-dotnet-get-started.md#createapiapp)hebt gezien. Het enige verschil is dat het type app **Web App** in plaats van **API-App**.  Zie voor schermopnamen van de dialoogvensters, 

1. Met de rechtermuisknop op het project ToDoListAngular in **Solution Explorer**en klik vervolgens op **publiceren**.

3.  Klik in het tabblad **profiel** van de wizard **Publiceren** en op **Microsoft Azure App-Service**.

5. Klik in het dialoogvenster **App Service** op **Nieuw**.

3. Voer in het tabblad **hosten** van het dialoogvenster **App-Service maken** en een **Web App-naam** die in het domein *azurewebsites.net* uniek is. 

5. Kies het Azure **abonnement** dat u wilt werken.

6. Kies in de vervolgkeuzelijst **Resourcegroep** dezelfde resourcegroep die u eerder hebt gemaakt.

4. Kies in de vervolgkeuzelijst **App serviceplan** hetzelfde abonnement die u eerder hebt gemaakt. 

7. Klik op **maken**.

    Visual Studio de web-app maakt, wordt een profiel publiceren daarvoor gemaakt en wordt weergegeven de stap van de **verbinding** van de wizard **Publiceren** .

    **Publiceren** Klik nog niet op. In de volgende sectie, kunt u de nieuwe web-app om te bellen van de middelste laag API-app met App-Service configureren. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>De URL van de middelste laag instellen in web app-instellingen

1. Ga naar de [Azure-portal](https://portal.azure.com/)en ga vervolgens naar het blad **Web App** voor de web-app die u hebt gemaakt als host voor het project TodoListAngular (front-end).

2. Klik op **Instellingen > Toepassingsinstellingen**.

3. In de sectie **instellingen van de App** toevoegen de volgende sleutel en waarde:

  	|Toets|Waarde|Voorbeeld
  	|---|---|---|
  	|toDoListAPIURL|https://{Your middelste laag API app-naam} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Klik op **Opslaan**.

    Wanneer de code wordt uitgevoerd in Azure wordt aangegeven, vervangt deze waarde de localhost-URL die in het bestand *Web.config* . 

    De code die de instellingswaarde wordt is in *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    De code in *todoListSvc.js* de instelling gebruikt:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Implementeer het ToDoListAngular web-project in de nieuwe web-app

*  Klik in Visual Studio, in de stap van de **verbinding** van de wizard **Publiceren** op **publiceren**.

    Visual Studio implementeren van het project ToDoListAngular in de nieuwe web-app en wordt een browser naar de URL van de web-app geopend. 

### <a name="test-the-application-without-cors-enabled"></a>De toepassing testen zonder CORS ingeschakeld 

2. Klik in de browser speciale tools voor ontwikkelaars het Console-venster te openen.

3. In het browservenster waarin u kunt de AngularJS UI, klikt u op de koppeling van de **Takenlijst** .

    De JavaScript-code wordt geprobeerd om te bellen van de middelste laag API-app, maar de oproep mislukt omdat de front-end wordt uitgevoerd in een ander domein dan de back-end. Het browservenster ontwikkelaars hulpmiddelen voor Console ziet u een cross-origin foutbericht wordt weergegeven.

    ![Cross-origin foutbericht wordt weergegeven](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>CORS configureren voor de middelste laag API-app

In deze sectie configureert u de instelling CORS in Azure wordt aangegeven voor de middelste laag ToDoListAPI API-app. Deze instelling kan de middelste laag API-app om te worden JavaScript gebeld vanaf de web-app die u hebt gemaakt voor het project ToDoListAngular.

8. Ga naar de [Azure-portal](https://portal.azure.com/)in een browser.

2. Klik op **App-Services**en klik vervolgens op de ToDoListAPI (middelste laag) API-app.

    ![Selecteer API-app in de portal](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Zoek de sectie **API** in het blad **Instellingen** dat wordt geopend aan de rechterkant van het blad **API-app** , en klik vervolgens op **CORS**.

    ![Selecteer CORS in portal](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. In het tekstvak, voert u de URL voor de web-app ToDoListAngular (front-end). Als u het project ToDoListAngular geïmplementeerd naar een web-app met de naam todolistangular0121, kunt bijvoorbeeld oproepen van de URL `https://todolistangular0121.azurewebsites.net`.

    Als alternatief, kunt u een sterretje (*) als u wilt opgeven dat alle origin domeinen worden geaccepteerd.

13. Klik op **Opslaan**.

    ![Klik op Opslaan](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Nadat u op **Opslaan**klikt, wordt de API-app JavaScript-gesprekken van de opgegeven URL accepteren. De app ToDoListAPI0223 API accepteren in deze schermafbeelding JavaScript-client oproepen vanuit de ToDoListAngular WebApp.

### <a name="test-the-application-with-cors-enabled"></a>De toepassing testen met CORS ingeschakeld

* Open een browser naar de HTTPS-URL van de web-app. 

    Nu de toepassing, kunt u weergeven, toevoegen, bewerken en verwijderen van taakitems. 

    ![Pagina van de steekproef app takenlijst](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>App-Service CORS versus Web API CORS

In een project Web API, kunt u het pakket [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet om op te geven in code welke domeinen uw API worden geaccepteerd JavaScript gebeld vanaf installeren.
 
Web API CORS ondersteuning is flexibeler dan App Service CORS ondersteuning. Bijvoorbeeld code kunt u verschillende geaccepteerde oorsprong voor andere actie methoden, terwijl voor App-Service CORS u één set van geaccepteerde oorsprong voor alle methoden een API-app geeft.

> [AZURE.NOTE] Niet probeert zowel Web API CORS en App Service CORS gebruiken in één API-app. App-Service CORS heeft voorrang en Web API CORS heeft geen effect. Bijvoorbeeld als u één origin domein in App Service inschakelen en inschakelen van alle origin domeinen in uw Web API-code, worden uw Azure-API-app alleen geaccepteerd oproepen van het domein dat u hebt opgegeven in Azure wordt aangegeven.

### <a name="how-to-enable-cors-in-web-api-code"></a>Het inschakelen van CORS in Web API-code

De volgende stappen samenvatting maken van het proces voor het Web API CORS-ondersteuning in te schakelen. Zie voor meer informatie, [Zodat meerdere Origin verzoeken in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Klik in een project Web API door het [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-pakket te installeren.

1. Bevatten een `config.EnableCors()` regel met code in de methode **registreren** van de klasse **WebApiConfig** , zoals in het volgende voorbeeld. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. In de controller Web API toevoegen een `using` -instructie voor de `System.Web.Http.Cors` naamruimte, en toevoegen de `EnableCors` kenmerk naar de klas controller of afzonderlijke actie methoden. In het volgende voorbeeld geldt CORS ondersteuning voor de hele controller.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Azure API Management gebruikt met API-Apps

Als u Azure API Management met een API-app hebt gebruikt, configureert u CORS API-beheer in plaats van in de API-app. Zie de volgende bronnen voor meer informatie:

* [Overzicht van Azure API Management (video: CORS begint op 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [API Management cross beleid voor het domein](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Problemen oplossen

Als u een probleem ondervindt terwijl u door deze zelfstudie, hier ideeën voor probleemoplossing te volgen.

* Zorg ervoor dat u de nieuwste versie van de [Azure SDK voor .NET voor Visual Studio-2015](http://go.microsoft.com/fwlink/?linkid=518003)gebruikt.

* Zorg ervoor dat u hebt ingevoerd `https` bij de instelling CORS en zorg ervoor dat u gebruikt `https` naar de front-WebApp uitvoeren.

* Zorg ervoor dat u de instelling CORS hebt ingevoerd in de middelste laag API-app, niet in de front WebApp.

* Als u CORS in zowel de toepassingscode als de App-Azure-Service configureren bent, houd er rekening mee dat App Service CORS wordt overschreven door de instelling wat u in de toepassingscode doet. 

Meer informatie over Visual Studio-functies die vereenvoudigen probleemoplossing, raadpleegt u [problemen oplossen Azure App Service-apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Volgende stappen 

In dit artikel u hebt gezien App Service CORS ondersteuning inschakelen zodat de client JavaScript-code een API in een ander domein kunt bellen. Meer informatie over API apps Lees de [Inleiding tot verificatie in App Service](../app-service/app-service-authentication-overview.md)en ga vervolgens naar de zelfstudie [Gebruikersverificatie voor API-apps](app-service-api-dotnet-user-principal-auth.md) .
