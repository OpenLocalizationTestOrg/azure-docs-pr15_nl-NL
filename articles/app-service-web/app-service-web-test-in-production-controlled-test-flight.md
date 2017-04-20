<properties
    pageTitle="Flighting implementatie (bètaversie testen) in Azure App-Service"
    description="Leer hoe u nieuwe functies in uw app flight of uw updates in deze zelfstudie end-to-end bètaversie testen. Deze worden samengebracht App Service-functies zoals doorlopend publiceren, sleuven, verkeer routeren en integratie van de toepassing inzichten."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting implementatie (bètaversie testen) in Azure App-Service

Deze zelfstudie wordt getoond hoe u *flighting implementaties* door integratie van de verschillende mogelijkheden van [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714) en [Azure toepassing inzichten](/services/application-insights/). 

*Flighting* is een implementatieproces dat is gevalideerd met een nieuwe functie of wijzigen met een beperkt aantal reële klanten en is een primaire testen in productieschema. Er lijkt op het testen van beta en soms "gecontroleerde test flight" wordt genoemd. Veel grote ondernemingen met de aanwezigheid van een web gebruik deze methode vroege validatie krijgt op de app-updates in hun gewoonte van [agile ontwikkeling](https://en.wikipedia.org/wiki/Agile_software_development). Azure App-Service kunt u test in productie integreren met continu publiceren en toepassing inzichten willen implementeren hetzelfde DevOps scenario. Voordelen van deze methode zijn:

- **Winst reële feedback _voordat_ updates zijn vrijgegeven voor productie** - het enige wat beter dan feedback krijgen zodra u loslaat is feedback krijgen voordat u loslaat. U kunt zo snel als in de levenscyclus van de producten gewenst wordt bijgewerkt met de werkelijke gebruikersverkeer en gedrag van testen.
- **Verbeteren [continue ontwikkeling testen op basis van hoeveelheid werk (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - door te testen in productie integreren met continue integratie en instrumentation met toepassing inzichten, gebruikersvalidatie gebeurt vroeg en automatisch in de levenscyclus van uw product. Dit bespaart u tijd investeringen in handmatige testen.
- **Optimaliseren testen werkstroom** - door te automatiseren testen in productie met continue controle instrumentation, kunt u mogelijk uitvoeren de doelstellingen van verschillende soorten tests in een afzonderlijk proces, zoals [integratie](https://en.wikipedia.org/wiki/Integration_testing), [regressie](https://en.wikipedia.org/wiki/Regression_testing) [bruikbaarheid](https://en.wikipedia.org/wiki/Usability_testing), toegankelijkheid, lokalisatie, [prestaties](https://en.wikipedia.org/wiki/Software_performance_testing), [beveiliging](https://en.wikipedia.org/wiki/Security_testing)en [bevestigen](https://en.wikipedia.org/wiki/Acceptance_testing).

Een flighting implementatie routing vrijwel geen live verkeer. In een dergelijk systeem gewenste om inzicht te krijgen zo snel mogelijk, of dit zijn een onverwachte fout, vertragingen, gebruikerservaring problemen. Onthoud dat u werkt met reële klanten. Dus ervan rechts, u moet ervoor zorgen dat u hebt ingesteld uw flighting implementatie Verzamel alle gegevens die u nodig hebt bij een weloverwogen beslissing nemen voor uw volgende stap. Deze zelfstudie leert u hoe u het verzamelen van gegevens met de toepassing inzichten, maar u kunt nieuwe Relic of andere technologieën die past bij uw scenario. 

## <a name="what-you-will-do"></a>Wat u doet

In deze zelfstudie leert u hoe u om de volgende scenario's samen om te testen van uw App Service-app in productie weer te geven:

- [Route productie verkeer](app-service-web-test-in-production-get-start.md) naar uw beta-app
- [Instrument uw app](../application-insights/app-insights-web-track-usage.md) handig aan de doelstellingen verkrijgen
- Continu Implementeer de bèta-app en live app volgen
- Aan de doelstellingen tussen de productie-app en de bèta-app om te zien hoe codewijzigingen vertalen om resultaten weer te vergelijken

## <a name="what-you-will-need"></a>Wat u nodig hebt

-   Een Azure-account
-   Een [GitHub](https://github.com/) -account
- Visual Studio-2015 - kunt u de [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx)downloaden.
-   Cijfer Shell (geïnstalleerd met [GitHub voor Windows](https://windows.github.com/)) - Hiermee kunt u zowel het cijfer en de PowerShell-opdrachten kunt uitvoeren in dezelfde sessie
-   Meest recente [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) -bits
-   Basiskennis van de volgende handelingen uit:
    -   Implementatie van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) -sjabloon (Zie [Deploy een complexe toepassing volgens plan in Azure wordt aangegeven](app-service-deploy-complex-application-predictably.md))
    -   [Cijfer](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] U hebt een Azure-account om te voltooien van deze zelfstudie nodig:
> + U kunt [een Azure-account gratis openen](/pricing/free-trial/) - u tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze omhoog gebruikt kunt u het account en gebruik vrij te geven Azure services, zoals Web Apps.
> + U kunt [Visual Studio abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/) : uw Visual Studio abonnement geeft u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.
>
> Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="set-up-your-production-web-app"></a>Uw productie WebApp instellen

>[AZURE.NOTE] Het script in deze zelfstudie gebruikt configureert automatisch continue publiceren vanaf uw GitHub-bibliotheek. Hiervoor is vereist dat uw referenties GitHub al zijn opgeslagen in Azure wordt aangegeven, anders kan de implementatie met scripts, mislukt tijdens een poging om het configureren van instellingen voor gegevensbron voor de Webapps.
>
>Voor de opslag uw referenties GitHub in Azure wordt aangegeven, maakt u een web-app in de [Portal van Azure](https://portal.azure.com/) en [GitHub implementatie configureren](app-service-continuous-deployment.md#Step7). Alleen moet u dit één keer te doen.

In een normale DevOps scenario u een toepassing die wordt uitgevoerd in Azure leven hebt en u wilt wijzigen door continue publiceren. In dit scenario wordt u naar productie een sjabloon die u hebt ontwikkeld en getest implementeren.

1.  Maak uw eigen zich splitsen van de bibliotheek [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Zie voor informatie over het maken van uw zich splitsen [zich een cessies‑retrocessies splitsen](https://help.github.com/articles/fork-a-repo/). Nadat uw zich splitsen is gemaakt, kunt u deze zien in uw browser.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Open een cijfer Shell-sessie. Als u nog geen cijfer Shell hebt, nu [GitHub voor Windows](https://windows.github.com/) installeren.
3.  Maak een lokale kopie van uw zich splitsen door de volgende opdracht uit te voeren:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Nadat u uw lokale klonen hebt, navigeer naar * &lt;repository_root >*\ARMTemplates en uitvoeren de deploy.ps1 script met een uniek achtervoegsel, zoals hieronder wordt weergegeven:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Wanneer u wordt gevraagd, typt u in de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. Onthoud dat u de databasereferenties van uw, omdat u ze opnieuw opgeven moet bij het bijwerken van de resourcegroep.

    Hier ziet u de inrichten voortgang van verschillende Azure resources. Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Terug in uw cijfer Shell-sessie, uitgevoerd:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Wanneer het script is voltooid, gaat u terug om naar het adres van de frontend (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) om de toepassing uitvoert in productie weer te geven.
5.  Meld u aan bij de [Portal van Azure](https://portal.azure.com/) en eens kijken wat wordt gemaakt.

    U moet kunnen Zie twee web-apps in dezelfde resourcegroep, met een de `Api` achtervoegsel in de naam. Als u de weergave van de groep resources bekijkt, worden ook ziet u de SQL-Database en server, het App-abonnement en voor de WebApps het tijdelijk opslaan elementen. Blader door de verschillende bronnen en vergelijk deze met * &lt;repository_root >*\ARMTemplates\ProdAndStage.json om te zien hoe deze zijn geconfigureerd in de sjabloon.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

U kunt de app productie hebt ingesteld.  Nu kunnen we Stel dat u feedback ontvangt dat bruikbaarheid slechte voor de app is. Zodat u besluit om te kunnen onderzoeken. U gaat instrument van uw app u om feedback te geven.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Onderzoeken: Uw app client monitoring/doelstellingen Instrument

5. Open * &lt;repository_root >*\src\MultiChannelToDo.sln in Visual Studio.
6. Alle Nuget-pakketten herstellen via het contextmenu van de oplossing > **NuGet-pakketten beheren voor oplossing** > **herstellen**.
6. Met de rechtermuisknop op **MultiChannelToDo.Web** > **Toevoegen toepassing inzichten Telemetrielogboek** > **-Instellingen configureren** > resourcegroep wijzigen naar ToDoApp*&lt;your_suffix >* > **Toepassing inzichten toevoegen aan Project**.
7. Open het blad voor de resource **MultiChannelToDo.Web** -toepassing inzicht in de Portal Azure. Klik op **meer informatie over het browser pagina laden gegevens verzamelen** in het onderdeel **toepassing servicestatus** > code kopiëren.
7. De gekopieerde JS instrumentation code wilt toevoegen * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, vlak voor de afsluiting `<heading>` tag. De unieke instrumentation-toets van uw toepassing inzicht resource moet bevatten.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Aangepaste gebeurtenissen inzicht krijgen in toepassing voor verzenden muis muisklikken door de volgende code toe te voegen aan de onderkant van de hoofdtekst:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Deze JavaScript-fragment verzendt een aangepaste gebeurtenis inzicht krijgen in toepassing telkens wanneer een gebruiker klikt op een willekeurige plaats in de WebApp.

12. In cijfer Shell selectiebeslissingen en uw wijzigingen aan uw zich splitsen in GitHub push. Wacht tot clients vernieuwen van de browser.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  De wijzigingen geïmplementeerd app in productie uitwisselen:

        .\swap –Name ToDoApp<your_suffix>

13. Blader naar de resource van toepassing inzichten die u hebt geconfigureerd. Klik op aangepaste gebeurtenissen.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Als u aan de doelstellingen voor aangepaste gebeurtenissen niet ziet, wacht enkele minuten en klik op **vernieuwen**.

Stel dat u ziet als een grafiek hieronder:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

En het raster gebeurtenis eronder:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

De **knop** gebeurtenis overeenkomt met de verzendknop op basis van de code van uw ToDoApp-toepassing, en de gebeurtenis **invoer** komt overeen met het tekstvak. Dusverre logisch dingen. Het lijkt echter erop dat er een goede bedrag van muisklikken en weinig klikken op de taakitems (de **LI** gebeurtenissen is).

Op basis van dit doet u formulier uw hypothese dat sommige gebruikers kunnen nog niet helemaal duidelijk welk deel van de gebruikersinterface kan worden geklikt en is omdat de cursor voor de geselecteerde tekst is opgemaakt als u dit op items in de lijst en de bijbehorende pictogrammen.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Dit kan een gekunstelde voorbeeld zijn. U gaat echter een verbetering naar uw app maken en voer daarna een flighting implementatie om bruikbaarheid feedback van klanten live.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Uw app server monitoring/doelstellingen instrument
Dit is een tangens aangezien de client-app het scenario gedemonstreerd in deze zelfstudie alleen behandelt. Echter voor volledigheid u wordt de serverzijde app instellen.

6. Met de rechtermuisknop op **MultiChannelToDo** > **Toevoegen toepassing inzichten Telemetrielogboek** > **-Instellingen configureren** > resourcegroep wijzigen naar ToDoApp*&lt;your_suffix >* > **Toepassing inzichten toevoegen aan Project**.
12. In cijfer Shell selectiebeslissingen en uw wijzigingen aan uw zich splitsen in GitHub push. Wacht tot clients vernieuwen van de browser.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  De wijzigingen geïmplementeerd app in productie uitwisselen:

        .\swap –Name ToDoApp<your_suffix>

Dat is alles.

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Onderzoeken: Slot / regiospecifieke labels toevoegen aan de doelstellingen van de client-app

In deze sectie configureert u de verschillende implementatie sleuven slot / regiospecifieke telemetrielogboek om naar te verzenden dezelfde toepassing inzichten resource. Op deze manier kunt u telemetriegegevens tussen verkeer van verschillende sleuven (implementatieomgevingen) eenvoudig het effect te bekijken van uw app wijzigingen vergelijken. Tegelijkertijd, kunt u het verkeer productie scheiden van de rest, zodat u kunt doorgaan met het controleren van uw app productie naar wens.

Aangezien u van gegevens over de werking van de client verzamelen bent, wordt u [toevoegen aan uw JavaScript-code initialisatiefunctie van een telemetrielogboek gebruikt](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) in index.cshtml. Als u testen serverzijde prestaties wilt, bijvoorbeeld, kunt u ook doen op dezelfde manier in uw servercode (Zie de [Toepassing inzichten API voor aangepaste gebeurtenissen en aan de doelstellingen](../application-insights/app-insights-api-custom-events-metrics.md).

1. Eerst toevoegen de code bewteen de twee `//` opmerkingen onder in het JavaScript blokkeren die u hebt toegevoegd aan de `<heading>` eerder markeren.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Deze code initialisatiefunctie zorgt ervoor dat de `appInsights` object om toe te voegen de aangepaste eigenschap van een genoemd `Environment` aan elke stukje telemetrielogboek verzendt.

2. Voeg nu deze aangepaste eigenschap als een [slot instelling](web-sites-staged-publishing.md#AboutConfiguration) voor uw web-app in Azure wordt aangegeven. Voer de volgende opdrachten in uw cijfer Shell-sessie hiervoor.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    De Web.config in uw project al definieert de `environment` app-instelling. Met deze instelling, wanneer u de app lokaal, test uw doelstellingen worden zijn gemarkeerd met `VS Debugger`. Echter wanneer u uw wijzigingen aan Azure push, Azure wordt zoeken en gebruiken de `environment` app instellen in de web-app configuratie in plaats daarvan en uw doelstellingen zijn gemarkeerd met `Production`.

3. Doorvoeren en uw codewijzigingen push naar uw zich splitsen op GitHub en wacht voor uw gebruikers gebruik van de nieuwe app (nodig Vernieuw de browser). Het duurt ongeveer 15 minuten voor de nieuwe eigenschap weergeven in uw toepassing inzichten `MultiChannelToDo.Web` resource.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Nu gaat u naar het blad **aangepaste gebeurtenissen** opnieuw en filteren van de doelstellingen op `Environment=Production`. U moet nu mogelijk om alle nieuwe aangepaste gebeurtenissen in de slot productie met dit filter weer te geven.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Klik op de knop **Favorieten** de huidige aan de doelstellingen Explorer-instellingen om op te slaan ongeveer zo **aangepaste gebeurtenissen: productie**. U kunt eenvoudig overschakelen tussen deze weergave en de weergave van een implementatie slot later.

    > [AZURE.TIP] Voor nog meer krachtige analyses, kunt u bovendien [integratie van de resource van toepassing inzichten met Power BI](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Slot / regiospecifieke markeringen toevoegen aan de doelstellingen van uw server-app
Klik nogmaals voor volledigheid u wordt de serverzijde app instellen. In tegenstelling tot de client-app dat is geïmplementeerd in JavaScript, slot / regiospecifieke labels voor de server-app is geïmplementeerd met .NET-code.

1. Open * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. De blokkering van de code onder, vlak voor de afsluiting toevoegen naamruimte accolade.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. De naam resolutie fouten corrigeren door toe te voegen de `using` instructies onder aan het begin van het bestand:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. De volgende code toevoegen aan het begin van de `Application_Start()` methode:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Doorvoeren en uw codewijzigingen push naar uw zich splitsen op GitHub en wacht voor uw gebruikers gebruik van de nieuwe app (nodig Vernieuw de browser). Het duurt ongeveer 15 minuten voor de nieuwe eigenschap weergeven in uw toepassing inzichten `MultiChannelToDo` resource.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Update: Uw beta-tak instellen

2. Open * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json en zoek de `appsettings` resources (zoekt `"name": "appsettings"`). Zijn er 4 één voor elke slot. 

2. Voor elk `appsettings` resource toevoegen een `"environment": "[parameters('slotName')]"` instelling van de app naar het einde van de `properties` matrix. Vergeet niet tot het einde van de vorige regel door een komma.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    U zojuist hebt toegevoegd de `environment` instelling van de app aan alle sleuven in de sjabloon.
    
2. Zoek in hetzelfde bestand, de `slotconfignames` resources (zoekt `"name": "slotconfignames"`). Zijn er 2 één voor elke app.

2. Voor elk `slotconfignames` resource toevoegen `"environment"` naar het einde van de `appSettingNames` matrix. Vergeet niet tot het einde van de vorige regel door een komma.

    U zojuist hebt gemaakt de `environment` app golfclub instellen voor de desbetreffende implementatie slot voor beide apps.  

3. Voer de volgende opdrachten op het achtervoegsel met dezelfde resource groep die u eerder hebt gebruikt in uw cijfer Shell-sessie.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Wanneer u wordt gevraagd, geeft u dezelfde SQL-databasereferenties als voordat. Wanneer u wordt gevraagd de resourcegroep bijwerken, typ `Y`, klikt u vervolgens `ENTER`.

    Nadat het script is voltooid, alle resources in de oorspronkelijke resourcegroep blijven behouden, maar een nieuwe slot met de naam "beta" is gemaakt in deze met dezelfde configuratie als de "Tijdelijke" slot dat is gemaakt in het begin.

    >[AZURE.NOTE] Deze methode voor het maken van verschillende omgevingen verschilt van de methode in [Agile software development met Azure App-Service](app-service-agile-software-development.md). U kunt hier implementatieomgevingen maken met implementatie sleuven, waar als er u implementatieomgevingen met resourcegroepen maken. Implementatieomgevingen beheren met resourcegroepen, kunt u de productieomgeving off-limits behouden voor ontwikkelaars, maar het is niet gemakkelijk te testen in productie, waarin u eenvoudig met sleuven doen kunt.

Als u wilt, kunt u ook een alfa-app maken door te voeren

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Voor deze zelfstudie wilt u gewoon blijven gebruiken uw beta-app.

## <a name="update-push-your-updates-to-the-beta-app"></a>Update: Push uw updates naar de bèta-app

Terug naar de app die u wilt verbeteren.

1. Zorg ervoor dat u zich nu in uw beta-vertakking

        git checkout beta

2. In * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, zoek de `<li>` markeren en toevoegen de `style="cursor:pointer"` kenmerk, zoals hieronder wordt weergegeven.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. doorvoeren en push naar Azure.

4. Controleer of dat de wijziging wordt nu weergegeven in de bèta-slot door te schuiven naar http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Als u de wijziging nog niet ziet, vernieuwt u de browser om de nieuwe javascript-code.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nu dat u uw wijziging uitgevoerd in de bèta-slot hebt, bent u klaar om uit te voeren een flighting implementatie.

## <a name="validate-route-traffic-to-the-beta-app"></a>Valideren: Route-verkeer door naar de bèta-app

In deze sectie, wordt u verkeer doorsturen naar de bèta-app. Ter overzichtelijkheid van demonstratie gaat u naar een groot deel van de gebruikersverkeer doorsturen naar deze. In feite afhankelijk de hoeveelheid gegevens die u wilt doorsturen van uw specifieke situatie. Bijvoorbeeld, als uw site ingesteld op de schaal van microsoft.com is, mogelijk moet u minder dan één procent van de totale verkeer om te kunnen handig gegevens krijgen.

1. Voer de volgende opdrachten voor het routeren helft van de productie-verkeer naar de bèta-slot in uw cijfer Shell-sessie:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  De `ReroutePercentage=50` eigenschap geeft aan dat 50% van het verkeer productie worden doorgestuurd naar de URL van de bèta-app (opgegeven door de `ActionHostName` eigenschap).

2. Ga naar http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50% van het verkeer moet nu worden omgeleid naar de bèta-slot.

3. Filteren in uw toepassing inzichten resource, de doelstellingen omgeving = "beta".

    > [AZURE.NOTE] Als u deze gefilterde weergave als een andere favoriet opslaat, kunt u gemakkelijk de weergaven metrische explorer tussen productie en bèta-weergaven spiegelen.

Stel dat in de toepassing inzichten ziet u een ander nummer ongeveer als volgt uit:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Niet alleen wordt dit weergegeven dat er nog veel meer klikken op de `<li>` labels, maar er lijkt moet zijn van een stijging van klikken op `<li>` tags. U kunt vervolgens sluiten dat personen de nieuwe hebt ontdekt `<li>` labels zijn waarop kan worden geklikt en alle bijbehorende eerder-voltooide taken die in de app wordt nu worden gewist.

Op basis van de gegevens van uw flighting implementatie, besluit u dat de nieuwe gebruikersinterface gereed voor productie is.

## <a name="go-live-move-your-new-code-into-production"></a>Ga live: uw nieuwe code in bedrijf verplaatsen

U bent nu klaar om te verplaatsen van de update naar productie. Wat is het handig is dat nu u weet dat de update is al gevalideerde _voordat_ deze wordt verplaatst naar productie. Nu u kunt vertrouwen het dashboard implementeren. Aangezien u een update naar de AngularJS client-app hebt aangebracht, worden alleen de code aan de clientzijde gevalideerd. Als u wijzigingen aanbrengen in de back-enddatabase Web API-app, kunt u uw wijzigingen kan valideren op dezelfde manier en gemakkelijk.

1. In cijfer Shell, verwijdert u de regel voor het doorsturen van verkeer door de volgende opdracht uit te voeren:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Voer de opdrachten cijfer:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Wacht enkele minuten duren voordat de nieuwe code op het tijdelijk opslaan slot kunnen worden geïmplementeerd en klik starten http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net om te bevestigen dat de nieuwe update is temperatuur in het tijdelijk opslaan slot. Vergeet niet dat de basispagina tak van uw zich splitsen is gekoppeld aan de slot tijdelijk opslaan van uw app.

3. Nu het tijdelijk opslaan slot uitwisselen in bedrijf

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Overzicht ##

Azure App-Service kunt u gemakkelijk voor kleine-middelgrote bedrijven om te testen van hun apps klant-omlaag in de productie iets die traditioneel heeft plaatsgevonden in grote ondernemingen. Hopelijk, deze zelfstudie hebt gekregen de kennis die u nodig hebt om aan te brengen samen App Service en toepassing inzichten om mogelijke flighting implementatie, en zelfs andere test-in-productieve-scenario's in uw wereld DevOps. 

## <a name="more-resources"></a>Meer informatiebronnen ##

-   [Agile software development met Azure App-Service](app-service-agile-software-development.md)
-   [Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen](web-sites-staged-publishing.md)
-   [Een complexe toepassing volgens plan in Azure implementeren](app-service-deploy-complex-application-predictably.md)
-   [Een presentatie Azure resourcemanager-sjablonen](../resource-group-authoring-templates.md)
-   [JSONLint - de JSON-validatie](http://jsonlint.com/)
-   [Cijfer takken – basisfilters takken en samenvoegen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Project Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
