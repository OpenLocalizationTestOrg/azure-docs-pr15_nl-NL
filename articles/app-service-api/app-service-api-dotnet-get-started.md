<properties
    pageTitle="Aan de slag met de API-Apps en ASP.NET in App Service | Microsoft Azure"
    description="Informatie over het maken, implementeren en gebruiken van een ASP.NET-API-app in Azure App-Service, met behulp van Visual Studio-2015."
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
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Aan de slag met de API-Apps, ASP.NET en Swagger in Azure App-Service

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Dit is de eerste in een reeks zelfstudies met het gebruik van functies van Azure App Service die handig zijn voor het ontwikkelen en hostingprovider RESTful API's weergeven.  Deze zelfstudie behandelt ondersteuning voor metagegevens van de API in Swagger-indeling.

U leert:

* Het maken en implementeren van [API-apps](app-service-api-apps-why-best-platform.md) in Azure App-Service met behulp van hulpmiddelen voor ingebouwd in Visual Studio-2015.
* Hoe u API discovery automatiseren met behulp van het pakket Swashbuckle NuGet om dynamisch Swagger API metagegevens te genereren.
* Het gebruik van metagegevens Swagger API om automatisch te genereren clientcode voor een API-app.

## <a name="sample-application-overview"></a>Voorbeeld: overzicht

In deze zelfstudie werkt u met een eenvoudige taak steekproef-toepassing voor de lijst. De toepassing heeft een front-end van één pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE), een ASP.NET-Web-API middelste en een laag ASP.NET Web API-gegevens.

![De toepassing voorbeelddiagram API-Apps](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Hier ziet u een schermafbeelding van de front-end van de [AngularJS](https://angularjs.org/) .

![API-Apps voorbeeld van de toepassing takenlijst](./media/app-service-api-dotnet-get-started/todospa.png)

De Visual Studio-oplossing bevat drie projecten:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - front-end van de: een beveiligd-WACHTWOORDVERIFICATIE voor een AngularJS die de middelste laag belt.

* **ToDoListAPI** - de middelste laag: een ASP.NET Web API-project dat de gegevenslaag om uit te voeren CRUD-bewerkingen op taakitems belt.

* **ToDoListDataAPI** - de gegevenslaag: een ASP.NET Web API-project dat CRUD-bewerkingen op taakitems worden uitgevoerd.

De architectuur met drie lagen is een van de vele architecturen die u kunt implementeren met behulp van API-Apps en worden hier alleen gebruikt voor gedemonstreerd. De code in elke laag is zo eenvoudig mogelijk is om te laten zien API Apps functies; Zo gebruikt de gegevenslaag servergeheugen in plaats van een database als de permanente om.

Klik op voltooien van deze zelfstudie, hebt u de twee Web API-projecten omhoog en worden uitgevoerd in de cloud in App Service API-apps.

De volgende zelfstudie in de reeks implementeren de front-end van de beveiligd-WACHTWOORDVERIFICATIE in de cloud.

## <a name="prerequisites"></a>Vereisten voor

* ASP.NET Web API - de zelfstudie instructies wordt ervan uitgegaan dat u hebt een basiskennis van het werken met ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.

* Azure-account - kunt u [een Azure-account gratis opent](/pricing/free-trial/?WT.mc_id=A261C142F) of [Visual Studio activeren abonnee voordelen](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751). Er, u direct een tijdelijk starter-app kunt maken in de App Service, **geen creditcard vereist**en geen afspraken.

* Visual Studio-2015 met de [Azure SDK voor .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - de SDK automatisch worden geïnstalleerd Visual Studio-2015 als u nog geen hebt.

    * Klik op Help in Visual Studio -> over Microsoft Visual Studio en zorg ervoor dat u 'Azure App Service hulpmiddelen voor v2.9.1' of hoger is geïnstalleerd.

    ![Azure hulpmiddelen voor de App-versie](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Afhankelijk van hoeveel van de afhankelijkheden SDK al op uw computer, kan het installeren van de SDK lang duren, enkele minuten aan een half uur of meer.

## <a name="download-the-sample-application"></a>Downloaden van de steekproef-toepassing

1. Download de [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) opslagplaats.

    U kunt op de knop **Downloaden ZIP** of klonen van de bibliotheek op uw lokale computer.

2. Open de takenlijst van oplossing in Visual Studio-2015 of 2013.
   1. U moet elke oplossing vertrouwt.
        ![Beveiligingswaarschuwing](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Maak de oplossing (CTRL + SHIFT + B) de NuGet-pakketten herstellen.

    Als u zien van de toepassing betrekking heeft wilt voordat u deze implementeert, kunt u deze lokaal uitvoeren. Zorg ervoor dat ToDoListDataAPI uw opstartproject en uitvoeren van de oplossing. U moet een HTTP 403-fout in uw browser verwachten.

## <a name="use-swagger-api-metadata-and-ui"></a>API Swagger metagegevens en UI gebruiken

Ondersteuning voor metagegevens van [Swagger](http://swagger.io/) 2.0-API is ingebouwd in Azure App-Service. Elke API-app kunt een URL-eindpunt die resulteert in metagegevens voor de API in de indeling van JSON Swagger opgeven. De metagegevens geretourneerd uit dat eindpunt kan worden gebruikt voor de clientcode genereren.

Een project ASP.NET Web API kunt Swagger metagegevens dynamisch genereren met behulp van het [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-pakket. Het pakket Swashbuckle NuGet is al geïnstalleerd in de ToDoListDataAPI en ToDoListAPI projecten die u hebt gedownload.

In dit gedeelte van de zelfstudie u bekijkt de gegenereerde Swagger 2.0-metagegevens en vervolgens probeert een proef UI die is gebaseerd op de metagegevens Swagger.

1. Stel het ToDoListDataAPI-project (**niet** het project ToDoListAPI) als het opstartproject.

    ![ToDoDataAPI als opstartproject instellen](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Druk op F5 of klik op **Foutopsporing > starten foutopsporing** om uit te voeren van het project in de foutopsporingsmodus voor.

    De browser geopend en ziet u de pagina van de fout HTTP 403.

3. Toevoegen in de adresbalk van uw browser, `swagger/docs/v1` naar het einde van de regel en druk op Return. (De URL is `http://localhost:45914/swagger/docs/v1`.)

    Dit is de standaard-URL die wordt gebruikt door Swashbuckle om terug te keren Swagger 2.0 JSON-metagegevens voor de API.

    Als u Internet Explorer gebruikt, wordt de browser vraagt u om een bestand *v1.json* te downloaden.

    ![Downloaden van JSON metagegevens in Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)

    Als u Chrome, Firefox of rand gebruikt, wordt in de browser de JSON weergegeven in het browservenster. Verschillende browsers JSON anders afhandelen en het browservenster mogelijk niet precies hetzelfde uitzien als in het voorbeeld.

    ![JSON metagegevens in Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)

    In het onderstaande voorbeeld ziet u het eerste gedeelte van de metagegevens Swagger voor de API, met de definitie voor de Get-methode. Deze metagegevens is wat de Swagger gebruikersinterface die u in de volgende stappen gebruiken stations en u deze gebruikt in een latere gedeelte van de zelfstudie voor het automatisch clientcode genereren.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Sluit de browser en stop de Visual Studio foutopsporing.

5. In het project ToDoListDataAPI in **Solution Explorer**, opent u het bestand *App_Start\SwaggerConfig.cs* en schuif omlaag naar lijn 174 en verwijder de opmerkingen bij de volgende code.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Het bestand *SwaggerConfig.cs* wordt gemaakt tijdens de installatie van het pakket Swashbuckle in een project. Het bestand bevat een aantal manieren voor het configureren van Swashbuckle.

    De code die u hebt zonder opmerkingen kunt de Swagger gebruikersinterface die u in de volgende stappen gebruiken. Wanneer u een project Web API maakt met behulp van het project-sjabloon van de API-app, is deze code toegelicht al dan niet standaard wijze van beveiliging.

6. Voer nogmaals het project.

7. Toevoegen in de adresbalk van uw browser, `swagger` naar het einde van de regel en druk op Return. (De URL is `http://localhost:45914/swagger`.)

8. Wanneer de Swagger UI-pagina wordt weergegeven, klikt u op de **takenlijst van** als u wilt zien van de beschikbare methoden.

    ![Swagger UI beschikbare methoden](./media/app-service-api-dotnet-get-started/methods.png)

9. Klik op de eerste knop **krijgen** in de lijst.

10. Voer in de sectie **Parameters** een sterretje als de waarde van de `owner` parameter, en klik vervolgens op **het afwezigheidsbericht uitproberen**.

    Wanneer u verificatie in later zelfstudies toevoegt, wordt de middelste laag de werkelijke gebruikers-ID aan de gegevenslaag bieden. Nu hebben alle taken sterretje als hun eigenaar-ID, terwijl de toepassing wordt uitgevoerd zonder verificatie is ingeschakeld.

    ![Swagger UI uitproberen](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    De gebruikersinterface Swagger oproepen aan van de takenlijst van de reactiecode en de JSON-resultaten methode en wordt weergegeven.

    ![Swagger UI uitproberen resultaten](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Klik op **posten**en klik vervolgens op het vak onder **Model Schema**.

    Te klikken op het modelschema prefills het invoervak waarin u de waarde van de parameter voor de methode Post kunt opgeven. (Als dit niet in Internet Explorer werkt, een andere browser gebruikt of de parameterwaarde handmatig invoeren in de volgende stap.)  

    ![Swagger UI uitproberen bericht](./media/app-service-api-dotnet-get-started/post.png)

12. Wijzigen van de JSON in de `todo` parameter input zodanig dat deze als het volgende voorbeeld eruitziet vak of vervangt door uw eigen beschrijvingstekst:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Klik op **het afwezigheidsbericht uitproberen**.

    De takenlijst van API geeft als resultaat een HTTP 204 antwoord-code die wordt aangegeven success.

14. Klik op de eerste knop voor het **ophalen** en klik vervolgens op de knop **Probeer dit** in die sectie van de pagina.

    Het antwoord van de methode Get bevat nu het nieuwe item.

15. Optioneel: Probeer ook de opslag, verwijderen en u op ID methoden.

16. Sluit de browser en stop de Visual Studio foutopsporing.

Swashbuckle werkt met een ASP.NET Web API-project. Als u genereren van de metagegevens Swagger toevoegen aan een bestaand project wilt, hoeft u alleen het pakket Swashbuckle te installeren.

>[AZURE.NOTE] Swagger metagegevens bevat een unieke ID voor elke API-bewerking. Standaard mogelijk Swashbuckle dubbele Swagger bewerking id's voor uw Web API controller methoden genereren. Dit gebeurt als de controller heeft HTTP-methoden, zoals overbelast `Get()` en `Get(id)`. Zie voor informatie over hoe u omgaat met overbelastingen, [API aanpassen Swashbuckle gegenereerde definities](app-service-api-dotnet-swashbuckle-customize.md). Als u een project Web API in Visual Studio met behulp van de Azure API App-sjabloon maakt, is automatisch code die gegenereerd unieke bewerking id's toegevoegd aan het bestand *SwaggerConfig.cs* .  

## <a id="createapiapp"></a>Een API-app in Azure maken en implementeren van code ernaar toevoegen

In deze sectie gebruikt u Azure hulpmiddelen die zijn geïntegreerd in de wizard Visual Studio **Web publiceren** naar een nieuwe API-app maakt in Azure wordt aangegeven. Vervolgens het project ToDoListDataAPI implementeren naar de nieuwe API-app en de API bellen door de gebruikersinterface Swagger uit te voeren.

1. Met de rechtermuisknop op het project ToDoListDataAPI in **Solution Explorer**en klik vervolgens op **publiceren**.

    ![Klik op publiceren in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  In de stap van het **profiel** van de wizard **Publiceren** , klikt u op **Microsoft Azure App-Service**.

    ![Klik op Azure App-Service in Web publiceren](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Meld u aan bij uw Azure-account als u dit nog niet hebt gedaan, of uw referenties vernieuwen als ze bent verlopen.

4. Klik in het dialoogvenster App Service Kies het Azure **abonnement** dat u wilt gebruiken en klik vervolgens op **Nieuw**.

    ![Klik op Nieuw in dialoogvenster App-Service](./media/app-service-api-dotnet-get-started/clicknew.png)

    Het tabblad **hosten** in het dialoogvenster **App-Service maken** wordt weergegeven.

    Omdat u een project Web API Swashbuckle geïnstalleerd met distribueren bent, Visual Studio wordt ervan uitgegaan dat u wilt een API-App maken. Hiermee wordt aangegeven door de titel **De naam van de API-App** en het feit dat de vervolgkeuzelijst **Type wijzigen** is ingesteld op **API-App**.

    ![App-type in dialoogvenster App-Service](./media/app-service-api-dotnet-get-started/apptype.png)

5. Voer een **API App-naam** die in het domein *azurewebsites.net* uniek is. U kunt accepteer de standaardnaam die Visual Studio voorstelt.

    Als u een naam die iemand anders al gebruikt invoeren heeft, ziet u een rood uitroepteken naar rechts.

    De URL van de app API `{API app name}.azurewebsites.net`.

6. Klik op **Nieuw**en voer 'ToDoListGroup' of een andere naam als u liever in de vervolgkeuzelijst **Resourcegroep** .

    Een resourcegroep is een verzameling Azure bronnen zoals API-apps, databases, VMs en dergelijke. Voor deze zelfstudie is het beste een nieuwe resourcegroep maken omdat die kunt u heel gemakkelijk verwijderen in één stap alle Azure bronnen die u voor de zelfstudie maken.

    Dit selectievakje kunt u een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md) selecteren of een nieuw account door te typen in een naam die verschilt van een bestaande resourcegroep in uw abonnement te maken.

7. Klik op de knop **Nieuw** naast de **App-Service plannen** vervolgkeuzelijst.

    De schermafbeelding ziet voorbeeldwaarden voor **De naam van de API-App**, **abonnement**en **Resourcegroep** --uw waarden zijn verschillend.

    ![Dialoogvenster App Service maken](./media/app-service-api-dotnet-get-started/createas.png)

    In de volgende stappen maakt u een App Service-plan voor de nieuwe resourcegroep. Een App-serviceplan Hiermee geeft u de berekeningscluster bronnen die op uw API-app wordt uitgevoerd. Bijvoorbeeld als u de gratis laag kiest, uw API-app wordt uitgevoerd op gedeelde VMs, terwijl voor sommige betaalde lagen wordt uitgevoerd op speciale VMs. Zie [overzicht van de App Service-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)voor informatie over App Service-abonnementen.

8. Voer "ToDoListPlan" of een andere naam als u liever in het dialoogvenster **App-Service plannen configureren** .

9. Kies de locatie die zich het dichtst bij u in de vervolgkeuzelijst **locatie** .

    Deze instelling bepaalt welke Azure datacenter van de app wordt uitgevoerd in. Kies een locatie dicht bij u [Latentie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimaliseren.

10. Klik in de vervolgkeuzelijst **grootte** op **gratis**.

    Voor deze zelfstudie biedt de gratis prijzen laag voldoende prestaties.

11. Klik op **OK**in het dialoogvenster **App-Service plannen configureren** .

    ![Klik op OK in configureren App-abonnement](./media/app-service-api-dotnet-get-started/configasp.png)

12. Klik in het dialoogvenster **App-Service maken** op **maken**.

    ![Klik op maken in dialoogvenster App-Service maken](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio de API-app en een profiel publiceren die alle vereiste instellingen voor de app API heeft gemaakt. Vervolgens wordt de wizard **Publiceren Web** , waar u bij het implementeren van het project wilt gebruiken.

    De wizard **Publiceren** wordt geopend op het tabblad **verbinding** (Zie hieronder).

    Klik op het tabblad **verbinding** wijst u de **Server** en **de naam van de Site** -instellingen naar uw API-app. De **gebruikersnaam** en **wachtwoord** zijn implementatie-referenties die Azure voor u gemaakt. Visual Studio geopend na implementatie, een browser naar de **Doel-URL** (dit is het enige doel voor **De doel-URL**).  

13. Klik op **volgende**.

    ![Klik op volgende in het tabblad verbinding van Web publiceren](./media/app-service-api-dotnet-get-started/connnext.png)

    Het volgende tabblad is het tabblad **Instellingen** (Zie hieronder). Hier kunt u het tabblad opbouwen configuratie als u wilt implementeren van een opbouwen foutopsporing voor [Foutopsporing op afstand](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Het tabblad biedt ook verschillende **Opties voor het publiceren van bestand**:

    * Aanvullende bestanden op bestemming verwijderen
    * Voorcompileren tijdens publicatie
    * Bestanden uit de map App_Data uitsluiten

    Voor deze zelfstudie hoeft u niet elk van deze. Zie voor gedetailleerde uitleg van wat ze doen [hoe: implementeren een Web Project met één klik publiceren in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klik op **volgende**.

    ![Klik op volgende op het tabblad instellingen van Web publiceren](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Volgende is het tabblad **voorbeeld** (Zie hieronder), welke hebt u een verkoopkans om te zien welke bestanden wilt worden gekopieerd van uw project naar de API-app. Wanneer u een project naar een API-app die u al geïmplementeerd distribueren bent in eerder, wordt alleen gewijzigde bestanden worden gekopieerd. Als u een overzicht wilt van wat worden gekopieerd, kunt u de knop **Voorbeeld starten** .

15. Klik op **publiceren**.

    ![Klik op publiceren op tabblad van de Preview-versie van Web publiceren](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Het project ToDoListDataAPI implementeren Visual Studio in de nieuwe API-app. **Het uitvoervenster** logboeken geslaagde implementatie en een pagina 'gemaakt' wordt weergegeven in een browservenster geopend naar de URL van de API-app.

    ![Uitvoer venster geslaagde implementatie](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Nieuwe API app ASPX-pagina](./media/app-service-api-dotnet-get-started/appcreated.png)

16. "Swagger" toevoegen naar de URL in de adresbalk van de browser en druk op Enter. (De URL is `http://{apiappname}.azurewebsites.net/swagger`.)

    De browser weergegeven dezelfde Swagger gebruikersinterface die u eerder hebt gezien, maar wordt nu uitgevoerd in de cloud. De methode Get uitproberen en u ziet dat u terug naar de standaard 2 taakitems bent. De wijzigingen die u eerder hebt aangebracht, zijn opgeslagen in het geheugen in de lokale computer.

17. Open de [portal van Azure](https://portal.azure.com/).

    De Azure-portal is een webservice-interface voor het beheren van Azure resources zoals API-apps.

18. Klik op **meer Services > App Services**.

    ![App-Services Bladeren](./media/app-service-api-dotnet-get-started/browseas.png)

19. Zoek in het blad **App Services** , en klik op de nieuwe API-app. (Klik in de portal Azure worden windows dat wordt geopend aan de rechterkant *bladen*genoemd.)

    ![App Services blade](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Twee bladen openen. Eén blade heeft een overzicht van de API-app en hebben een lange lijst van instellingen die u kunt weergeven en wijzigen.

20. Het blad **Instellingen** , zoek de sectie **API** en klik op **API definitie**.

    ![API-definitie in instellingen blade](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Het blad **API definitie** kunt u de URL die Swagger 2.0 metagegevens in JSON-notatie retourneert opgeven. Als u Visual Studio maakt de API-app, wordt de URL van de definitie API op de standaardwaarde voor Swashbuckle gegenereerde metagegevens die u eerder hebt gezien dat wil de API-app zeggen de basis URL plus `/swagger/docs/v1`.

    ![API definitie URL](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Wanneer u een app API clientcode genereren voor deze selecteert, wordt de metagegevens uit deze URL in Visual Studio opgehaald.

## <a id="codegen"></a>Clientcode voor de gegevenslaag genereren

Een van de voordelen van de integratie van Swagger in Azure-API-apps is automatische codegeneratie. Gegenereerde client klassen gemakkelijker code schrijven die een app API-oproepen.

Het project ToDoListAPI al bevat de gegenereerde clientcode, maar in de volgende stappen wordt u deze verwijderen en opnieuw wilt genereren om te zien hoe u de code genereren.

1. Verwijder de map *ToDoListDataAPI* in Visual Studio **Solution Explorer**in het project ToDoListAPI. **Let op: Verwijder alleen de map, niet het project ToDoListDataAPI.**

    ![Gegenereerde clientcode verwijderen](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Deze map is gemaakt met behulp van het proces voor het genereren van code die u gaat wilt doorlopen.

2. Met de rechtermuisknop op het project ToDoListAPI en klik vervolgens op **toevoegen > REST API-Client**.

    ![REST API-client toevoegen in Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. In het dialoogvenster **REST API-Client toevoegen** , klikt u op **Swagger URL**en klik vervolgens op **Azure activum selecteren**.

    ![Azure activum selecteren](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. De resourcegroep die u voor deze zelfstudie gebruikt en selecteer uw app API uitvouwen in het dialoogvenster **App Service** en klik vervolgens op **OK**.

    ![Selecteer API-app voor code genereren](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Zoals u ziet dat wanneer u naar het dialoogvenster **REST API-Client toevoegen teruggaat** , het tekstvak is ingevuld de definitie API URL-waarde die u eerder in de portal hebt gezien.

    ![API definitie URL](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Een andere manier om metagegevens voor het genereren van de code is de URL rechtstreeks in plaats van via het bladerdialoogvenster invoeren. Of als u wilt clientcode genereren voordat u de implementatie in Azure, kan u het project Web API lokaal uitvoeren, gaat u naar de URL die het Swagger JSON-bestand bevat, sla het bestand en gebruik de optie voor het **selecteren van een bestaand Swagger metagegevens-bestand** .

5. Klik op **OK**in het dialoogvenster **REST API-Client toevoegen** .

    Visual Studio Hiermee maakt u een map die de naam van de API-app en genereert client klassen.

    ![Codebestanden voor gegenereerde-client](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Open in het project ToDoListAPI *Controllers\ToDoListController.cs* als u wilt zien van de code op lijn 40 die de API-oproepen via de gegenereerde-client.

    Het codefragment van de volgende ziet u hoe de code wordt de clientobject en de methode Get-oproepen.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    De parameter constructor krijgt de URL van het eindpunt van de `toDoListDataAPIURL` app-instelling. Deze waarde is ingesteld op de lokale op IIS Express URL van het project API in het bestand Web.config, zodat u de toepassing lokaal kunt uitvoeren. Als u de parameter constructor weglaat, is het standaardeindpunt de URL die u hebt de code uit gegenereerd.

7. Uw klas client wordt gegenereerd met een andere naam op basis van de naam van de API-app; Wijzig de code in *Controllers\ToDoListController.cs* zodat de typenaam overeenkomt met wat in uw project is gegenereerd. Als u de naam van uw API App ToDoListDataAPI071316, zou u bijvoorbeeld deze code wijzigen:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

als volgt:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Een API-app als host voor de middelste laag maken

Eerdere u [de gegevens laag API-app hebt gemaakt en geïmplementeerd code toe](#createapiapp).  Nu volgen u de procedure voor de middelste laag API-app.

1. Klik in **Solution Explorer**met de rechtermuisknop op de middelste laag ToDoListAPI project (niet de gegevenslaag ToDoListDataAPI), en klik vervolgens op **publiceren**.

    ![Klik op publiceren in Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Klik in het tabblad **profiel** van de wizard **Publiceren** en op **Microsoft Azure App-Service**.

3. Klik in het dialoogvenster **App Service** op **Nieuw**.

4. In het tabblad **hosten** van het dialoogvenster **App-Service maken** en accepteer de standaardwaarde **API App-naam** of voer een naam die in het domein *azurewebsites.net* uniek is.

5. Kies het Azure **abonnement** dat u hebt gebruikt.

6. Kies in de vervolgkeuzelijst **Resourcegroep** dezelfde resourcegroep die u eerder hebt gemaakt.

7. Kies in de vervolgkeuzelijst **App serviceplan** hetzelfde abonnement die u eerder hebt gemaakt. Deze waarde wordt standaard.

8. Klik op **maken**.

    Visual Studio de API-app maakt, wordt een profiel publiceren daarvoor gemaakt en wordt de stap van de **verbinding** van de wizard **Publiceren** .

9.  In de stap van de **verbinding** van de wizard **Publiceren** , klikt u op **publiceren**.

    Visual Studio implementeren van het project ToDoListAPI in de nieuwe API-app en wordt een browser naar de URL van de API-app geopend. De pagina 'is gemaakt' wordt weergegeven.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>De middelste laag om te bellen van de gegevenslaag configureren

Als u de middelste laag API-app nu gebeld, zou dit probeer aan te roepen van de gegevenslaag via de URL localhost dat zich nog in het bestand Web.config. In deze sectie kunt u de URL van de gegevens laag API app invoeren in een omgevingsinstelling in het middelste laag API-app. Als de instelling van de URL van de gegevens laag worden opgehaald door de code in de middelste laag API-app, wordt de omgevingsinstelling overschreven door wat staat er in het bestand Web.config.

1. Ga naar de [Azure-portal](https://portal.azure.com/)en ga vervolgens naar het blad **API-App** voor de API-app die u hebt gemaakt als host voor het project TodoListAPI (middelste laag).

2. Klik in de API-App **Instellingen** blade, op **Toepassingsinstellingen**.

3. In de API-App **Toepassingsinstellingen** blade, schuif omlaag naar de sectie **instellingen van de App** en voeg de volgende sleutel en waarde. De waarde is de URL van de eerste API-App die u in deze zelfstudie hebt gepubliceerd.

  	| **Toets** | toDoListDataAPIURL |
  	|---|---|
  	| **Waarde** | https://{Your gegevens laag API app-naam} .azurewebsites .net |
  	| **Voorbeeld** | https://todolistdataapi.azurewebsites.NET |

4. Klik op **Opslaan**.

    ![Klik op Opslaan voor App-instellingen](./media/app-service-api-dotnet-get-started/asinportal.png)

    Wanneer de code wordt uitgevoerd in Azure wordt aangegeven, wordt deze waarde nu overschreven door de localhost-URL die in het bestand Web.config.

## <a name="test"></a>Test

1. Blader naar de URL van de nieuwe middelste laag API-app, die u zojuist hebt gemaakt voor ToDoListAPI in een browservenster. U kunt er toegang toe krijgen door te klikken op de URL in de belangrijkste blade de API-app in de portal.

2. "Swagger" toevoegen naar de URL in de adresbalk van de browser en druk op Enter. (De URL is `http://{apiappname}.azurewebsites.net/swagger`.)

    Dezelfde Swagger gebruikersinterface die u eerder hebt gezien voor ToDoListDataAPI, maar nu weergegeven in de browser `owner` is niet een vereist veld voor de bewerking ophalen, omdat de middelste laag API-app die waarde naar de gegevens laag API-app voor u verzenden is. (Als u de verificatie-zelfstudies doet, de middelste laag werkelijke gebruikers-id's voor verzendt de `owner` parameter; voor nu deze is harde codering een sterretje.)

3. Probeer de methode Get en de andere methoden voor het valideren van de middelste laag API-app is succes bellen voor de gegevens laag API-app.

    ![Swagger UI Get-methode](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als u een probleem hebt ondervindt, terwijl u deze zelfstudie hier doorloopt zijn ideeën voor probleemoplossing:

* Zorg ervoor dat u de nieuwste versie van de [Azure SDK voor .NET](http://go.microsoft.com/fwlink/?linkid=518003)gebruikt.

* Twee van de projectnamen zijn vergelijkbare (ToDoListAPI, ToDoListDataAPI). Als u niet tevreden bent zoals beschreven in de instructies wanneer u met een project werkt, zorg er dan voor dat u het juiste project hebt geopend.

* Als u zich op een bedrijfsnetwerk en probeert te implementeren naar Azure App-Service via een firewall, zorg ervoor dat poort 443 en 8172 geopend om Web implementeren zijn. Als u deze poorten niet kunt openen, kunt u andere implementatiemethoden.  Zie [Implementeer de app Azure App-service](../app-service-web/web-sites-deploy.md).

* 'Route namen moeten uniek zijn' fouten--kan krijgt u deze als u per ongeluk het verkeerde project naar een API-app implementeren en klikt u vervolgens de juiste is ernaar later te implementeren. U kunt deze, selecteer het juiste project naar de API-app en klik op het tabblad **Instellingen** van de wizard **Publiceren** in dat geval **aanvullende bestanden op bestemming verwijderen**.

Nadat u uw ASP.NET-API-app met App-Azure-Service hebt, kunt u voor meer informatie over Visual Studio-functies die probleemoplossing vereenvoudigen. Zie [problemen met Azure App Service-apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)voor informatie over logboekregistratie, foutopsporing op afstand, en nog veel meer.

## <a name="next-steps"></a>Volgende stappen

Bovendien hebt u geleerd hoe u de bestaande Web API-projecten implementeren naar API-apps, clientcode voor API-apps genereren en API-apps van .NET-clients gebruiken. Ziet u de volgende zelfstudie in deze reeks het [gebruik van CORS API-apps van JavaScript-clients in beslag neemt](app-service-api-cors-consume-javascript.md).

Zie voor meer informatie over client code genereren, de opslagplaats [Azure/AutoRest](https://github.com/azure/autorest) op GitHub.com. Open een [probleem in de bibliotheek AutoRest](https://github.com/azure/autorest/issues)voor hulp bij het oplossen van de gegenereerde-client.

Als u maken van nieuwe API app projecten maken wilt, gebruikt u de sjabloon **Azure API-App** .

![API-App-sjabloon in Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

De sjabloon voor een project **Azure API App** is gelijk aan het kiezen van de **lege** ASP.NET 4.5.2 sjabloon, te klikken op het selectievakje in om toe te voegen Web API-ondersteuning, Swashbuckle NuGet-pakket en installeren. De sjabloon wordt ook enkele Swashbuckle configuratiecode voor uw ontworpen om te voorkomen dat het maken van dubbele Swagger bewerking id's toegevoegd. Zodra u een project API-App hebt gemaakt, u kunt het dashboard implementeren naar een API-app het op dezelfde manier u in deze zelfstudie hebt gezien.
