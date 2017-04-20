<properties
    pageTitle="Provision en implementeren van microservices volgens plan in Azure wordt aangegeven"
    description="Leer hoe u een toepassing op basis van microservices in Azure App Service als één eenheid en praat met JSON resource groep sjablonen en PowerShell-scripts implementeren."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Provision en implementeren van microservices volgens plan in Azure wordt aangegeven #

Deze zelfstudie leert hoe u inrichten en implementeren van een toepassing op basis van [microservices](https://en.wikipedia.org/wiki/Microservices) in [Azure App Service](/services/app-service/) als één eenheid en praat JSON resource groep sjablonen en PowerShell-scripts gebruiken. 

Bij inrichting en implementeren van hoge schaalbaarheid toepassingen die bestaan uit ten zeerste ontkoppelde zijn microservices, herhaalbaarheid en voorspelbaarheid essentieel voor succes. [Azure-Service voor App](/services/app-service/) kunt u microservices die WebApps, mobiele apps API-apps en logica apps bevatten maken. [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) kunt u alle microservices beheren als een eenheid, samen met resource afhankelijkheden zoals database en instellingen voor gegevensbron. U kunt nu ook dergelijke JSON sjablonen en het uitvoeren van de eenvoudige PowerShell-scripts gebruiken toepassing implementeren. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Wat u doet ##

In deze zelfstudie implementeert u een toepassing die bevat:

-   Twee web-apps (dat wil zeggen twee microservices)
-   Een back-end SQL-Database
-   App-instellingen, verbindingstekenreeksen en besturingselement voor gegevensbronnen
-   Toepassing inzichten, meldingen, autoscaling instellingen

## <a name="tools-you-will-use"></a>Hulpprogramma's die u wilt gebruiken ##

In deze zelfstudie gebruikt u de volgende hulpprogramma's. Aangezien het niet volledig discussie op Extra, ik ga naar het end-to-end-scenario blijven en alleen geeft u een korte inleiding aan elke wordt en waar u meer informatie over deze kunt vinden. 

### <a name="azure-resource-manager-templates-json"></a>Azure resourcemanager sjablonen (JSON) ###
 
Telkens wanneer u een web-app in Azure App-Service maakt, bijvoorbeeld een JSON-sjabloon door resourcemanager Azure gebruikt de hele resourcegroep maken met de component resources. Een complexe sjabloon van [Azure Marketplace](/marketplace) zoals de app [Scalable WordPress](/marketplace/partners/wordpress/scalablewordpress/) de MySQL-database, opslag-accounts, de App serviceplan, de WebApp zelf opnemen kunt, waarschuwingsregels, app-instellingen automatisch schalen instellingen en meer, en alle deze sjablonen zijn beschikbaar voor u via PowerShell. Zie voor informatie over het downloaden en deze sjablonen gebruiken, [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

Zie voor meer informatie over de resourcemanager Azure-sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 voor Visual Studio ###

De nieuwste SDK bevat verbeteringen in de ondersteuning voor de Resource Manager van een sjabloon in de JSON-editor. U kunt hiermee snel een sjabloon maken op resource groep maken of open een bestaande JSON-sjabloon (zoals een sjabloon gedownloade galerie) voor het wijzigen van, het parameterbestand vullen en zelfs de resourcegroep rechtstreeks vanuit een resourcegroep Azure-oplossing implementeert.

Zie [Azure SDK 2.6 voor Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/)voor meer informatie.

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 of hoger ###

Begin in versie 0.8.0, bevat de installatie van Azure PowerShell de module Azure resourcemanager naast de Azure-module. Deze nieuwe module kunt u de implementatie van resourcegroepen scripts.

Zie voor meer informatie [Azure PowerShell gebruiken met Azure resourcemanager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer ###

Dit [voorbeeldfunctie](https://resources.azure.com) kunt u de JSON-definities van alle resourcegroepen in uw abonnement en de afzonderlijke resources verkennen. In het hulpmiddel kunt u de JSON-definities van een resource bewerken, verwijderen van een gehele hiërarchie van resources en maken van nieuwe resources.  De informatie die direct beschikbaar zijn in dit hulpprogramma is handig voor het ontwerpen van een sjabloon omdat deze u welke eigenschappen die u nodig hebt ziet om te zetten voor een bepaald type resource, de juiste waarden, enzovoort. U kunt zelfs uw resourcegroep maken in de [Portal van Azure](https://portal.azure.com/)en vervolgens de JSON-definities in het hulpmiddel explorer kunt u de resourcegroep templatize controleren.

### <a name="deploy-to-azure-button"></a>Implementeren naar Azure knop ###

Als u GitHub voor het besturingselement gebruikt, kunt u een [Deploy aan Azure knop](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) in uw Leesmij plaatsen. MD, waarmee een inschakelen-toets implementatie interface om te Azure. Terwijl u dit voor een eenvoudige web-app doen kunt, kunt u hierop als u een hele resourcegroep implementeren door een azuredeploy.json-bestand in de hoofdmap opslagplaats plaatsen uitbreiden. Dit bestand JSON, waarin u de resource groep sjabloon, worden gebruikt door de distribueren naar Azure knop resourcegroep maken. Zie voor een voorbeeld, voorbeeld van de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , die u in deze zelfstudie gebruiken wilt.

## <a name="get-the-sample-resource-group-template"></a>De voorbeeldsjabloon van de resource-groep ophalen ##

Nu we gaan rechts toe.

1.  Navigeer naar de steekproef van de App Service [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) .

2.   Klik op **Deploy naar Azure**in readme.md.
 
3.  U bent die u hebt gemaakt naar de site [implementeren naar azure](https://deploy.azure.com) en gevraagd implementatie invoerparameters uit te voeren. Zoals u ziet dat de meeste van de velden zijn gevuld met de naam van de bibliotheek en sommige tekenreeksen willekeurig voor u. Als u wilt, maar alleen wat die u moet invoeren zijn de administratieve login van SQL Server en het wachtwoord en klik op **volgende**, kunt u alle velden wijzigen.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Klik vervolgens op **Deploy** om de implementatieproces te starten. Wanneer het proces wordt uitgevoerd tot voltooiing, klikt u op de http://todoapp*XXXX*. azurewebsites.net-koppeling naar de gedistribueerde toepassing bladeren. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    De gebruikersinterface zou iets traag worden als u eerst naar deze omdat de apps alleen worden gestart, maar overtuigen uzelf dat dit een volledig functioneel toepassing is.

5.  Klik op de koppeling **beheren** om te zien van de nieuwe toepassing in de Portal Azure weer aan de pagina Deploy.

6.  Klik op de koppeling van de groep resource in de vervolgkeuzelijst **Essentials** . Houd er ook rekening mee dat de web-app al is verbonden met de bibliotheek GitHub onder **Externe Project**. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Klik in het blad van de groep resource Houd er rekening mee dat er al twee web-apps en één SQL-Database in de resourcegroep.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Alles die u zojuist hebt gezien in een paar korte minuten is een volledig geïmplementeerd twee-microservice-toepassing, met alle onderdelen, afhankelijkheden, instellingen, database, en continue publiceren, ingesteld door een geautomatiseerde situatie in Azure resourcemanager. Dit alles is uitgevoerd door twee dingen:

-   Het distribueren naar Azure knop
-   azuredeploy.JSON in de hoofdmap cessies‑retrocessies

U kunt deze dezelfde toepassing implementeren tientallen, honderden of duizenden malen en de precieze dezelfde configuratie elke keer hebt. De herhaalbaarheid en voorzienbaarheid van deze methode kunt u hoog-toepassingen met toegankelijkheid en betrouwbaarheid implementeren.

## <a name="examine-or-edit-azuredeployjson"></a>Bekijk AZUREDEPLOY (of bewerken). JSON ##

Nu eens kijken hoe de opslagplaats GitHub is gepland naar. U met de JSON-editor in de SDK van Azure .NET, dus als u nog niet al geïnstalleerd [Azure .NET SDK 2.6](/downloads/), doet u dit nu.

1.  De [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) -bibliotheek met de functie van uw favoriete cijfer klonen. In de onderstaande schermafbeelding gebeurt dit in de Verkenner Team in Visual Studio-2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Open vanuit de hoofdsite van de bibliotheek en azuredeploy.json in Visual Studio. Als u het deelvenster Overzicht van JSON niet ziet, moet u Azure .NET SDK installeren.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ik ben niet worden verzonden om te beschrijven van elk detail van de indeling van JSON, maar de sectie [Meer bronnen](#resources) bevat koppelingen voor zal het leren van de taal van de resource groep-sjabloon. Hier Ga alleen ik om aan te geven de interessante functies waarmee u kunt aan de slag bij het maken van uw eigen aangepaste sjabloon voor app-implementatie.

### <a name="parameters"></a>Parameters ###

Bekijk de sectie parameters om te zien dat de meeste van deze parameters zijn wat de knop **distribueren naar Azure** wordt u gevraagd om in te voeren. De site achter de knop **distribueren naar Azure** wordt gevuld met de parameters die zijn gedefinieerd in azuredeploy.json gebruikersinterface van de invoer. Deze parameters worden gebruikt in de definities van de resource, zoals resourcenamen, eigenschapswaarden, enzovoort.

### <a name="resources"></a>Resources ###

Klik in het knooppunt resources kunt u zien dat 4 op het hoogste niveau resources zijn gedefinieerd, zoals een SQL Server-instantie, een App-serviceplan en twee Webapps. 

#### <a name="app-service-plan"></a>App-abonnement ####

Laten we beginnen met een eenvoudige hoofdniveau bron in de JSON. Klik op het App-Service-abonnement met de naam **[hostingPlanName]** om te markeren, de bijbehorende JSON-code in het overzicht JSON. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Houd er rekening mee dat het `type` element geeft de tekenreeks voor een App serviceplan (deze heet een serverfarm een lang, lange tijd geleden), en andere elementen en eigenschappen zijn ingevuld met de parameters die zijn gedefinieerd in het bestand JSON, en deze resource geen geneste resources.

>[AZURE.NOTE] Noteer ook die de waarde van `apiVersion` wordt uitgelegd Azure welke versie van de REST API voor het gebruik van de definitie van de resource JSON met, waarna ze invloed kan zijn op hoe de resource moet worden weergegeven in de `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Klik op de SQL Server-bron met **SQL Server** in de JSON-overzicht de naam.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Houd rekening met de volgende punten over de gemarkeerde JSON-code:

-   Het gebruik van parameters zorgt ervoor dat de gemaakte resources zijn met de naam en op een manier die wordt omgezet in consistent zijn met elkaar geconfigureerd.
-   De SQL Server-resource heeft twee geneste resources, elk een andere waarde voor `type`.
-   Geneste bronnen binnen `“resources”: […]`, waarbij de database en de firewallregels zijn gedefinieerd, hebben een `dependsOn` -element waarin de resource-ID van de resource van de SQL Server hoofdniveau. Hiermee wordt aangegeven Azure resourcemanager, "voordat u deze resource maakt, wordt die andere resource moet al bestaan; en als die andere resource is gedefinieerd in de sjabloon, maakt u deze record eerst ".

    >[AZURE.NOTE] Voor gedetailleerde informatie over het gebruik van de `resourceId()` , functie, Zie [Azure resourcemanager sjabloon functies](../resource-group-template-functions.md).

-   Het effect van de `dependsOn` element is dat Azure resourcemanager weten kunt welke resources kunnen worden gemaakt parallel en welke resources opeenvolging moeten worden gemaakt. 

#### <a name="web-app"></a>In de browser ####

Nu kunnen we doorgaan naar de werkelijke web-apps zelf, welke ingewikkelder. Klik op de web-app [variables('apiSiteName')] in het overzicht JSON de JSON-code markeren. U ziet dat het dingen veel interessanter punt. Daartoe hebt ik overleggen over de functies één voor één:

##### <a name="root-resource"></a>Hoofdmap resource #####

De web-app, is afhankelijk van twee verschillende bronnen. Dit betekent dat Azure resourcemanager de web-app maakt alleen nadat de App-serviceplan zowel de SQL Server-instantie zijn gemaakt.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App-instellingen #####

Instellingen voor de app zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

In de `properties` element voor `config/appsettings`, er twee app-instellingen in de indeling `“<name>” : “<value>”`.

-   `PROJECT`is een [KUDU instelling](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) waarin staat Azure-implementatie welke project dat wilt gebruiken in een multi-project Visual Studio-oplossing. Ik kunt u zien hoe het besturingselement voor gegevensbronnen is geconfigureerd, maar omdat de code ToDoApp zich in een oplossing van meerdere project Visual Studio, moeten we deze instelling.
-   `clientUrl`is gewoon een app instellen dat de toepassingscode wordt gebruikt.

##### <a name="connection-strings"></a>Verbindingstekenreeksen met de #####

De verbindingstekenreeksen zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

In de `properties` element voor `config/connectionstrings`, elke verbindingsreeks ook is gedefinieerd als een combinatie van naam: waarde, met de specifieke opmaak van `“<name>” : {“value”: “…”, “type”: “…”}`. Voor de `type` element, mogelijke waarden zijn `MySql`, `SQLServer`, `SQLAzure`, en `Custom`.

>[AZURE.TIP] Voor een definitieve lijst van de tekenreeks verbindingstypen, voert u de volgende opdracht in Azure PowerShell: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Besturingselement voor gegevensbronnen #####

De instellingen voor bronbeheer zijn ook gedefinieerd als een geneste resource. Azure resourcemanager gebruikmaakt van deze resource voor het configureren van continue publiceren (Zie voorbehoud op `IsManualIntegration` later) en ook op gang de implementatie van toepassingscode automatisch tijdens het verwerken van de JSON-bestand.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`en `branch` moet heel intuïtieve en moet verwijzen naar de bibliotheek cijfer en de naam van de tak om uit te publiceren. Klik nogmaals worden deze gedefinieerd door invoerparameters weergegeven. 

Houd er rekening mee de `dependsOn` element die, naast de web-app resource zelf, `sourcecontrols/web` ook is afhankelijk van `config/appsettings` en `config/connectionstrings`. Dit komt doordat zodra `sourcecontrols/web` is geconfigureerd, het proces Azure-implementatie automatisch probeert te implementeren, maken en de toepassingscode te starten. Daarom kunt u ervoor zorgen dat de toepassing toegang tot de verbindingstekenreeksen met de vereiste app-instellingen en heeft voordat de toepassingscode wordt uitgevoerd deze afhankelijkheid invoegen. 

>[AZURE.NOTE] Zoals u ziet ook `IsManualIntegration` is ingesteld op `true`. Deze eigenschap wordt in deze zelfstudie nodig omdat u niet uw eigendom de opslagplaats GitHub en dus niet kunt daadwerkelijk toestemming verlenen Azure continue publiceren vanaf [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (dat wil zeggen configureren Automatische opslagplaats updates push naar Azure). U kunt de standaardwaarde `false` voor de opgegeven bibliotheek alleen als u de eigenaar GitHub referenties hebt geconfigureerd in de [portal van Azure](https://portal.azure.com/) voordat. Met andere woorden, als u eerder een besturingselement voor gegevensbronnen GitHub of BitBucket voor een app in de [Portal van Azure](https://portal.azure.com/) hebt ingesteld, wordt met de referenties van de gebruiker, vervolgens Azure de aanmeldingsreferenties onthouden en deze gebruiken wanneer u een app uit GitHub of BitBucket in de toekomst implementeren. Echter als u dit nog niet hebt gedaan, implementatie van de JSON-sjabloon, mislukt tijdens het configureren van instellingen voor de web-app bronbeheer omdat deze niet kunt bij GitHub of BitBucket met de referenties van de eigenaar van de bibliotheek aanmelden resourcemanager Azure.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>De JSON-sjabloon met geïmplementeerd resourcegroep vergelijken ##

Hier kunt u alle de web-app van bladen in de [Portal van Azure](https://portal.azure.com/)doorlopen, maar er is een ander hulpmiddel die is net zo handig als niet meer. Ga naar de [Azure Resource Explorer](https://resources.azure.com) voorbeeldfunctie, waarmee u een JSON-weergave van alle resourcegroepen in uw abonnementen, terwijl ze daadwerkelijk bestaat niet in de Azure-end. U kunt ook zien hoe de resourcegroep JSON-hiërarchie in Azure overeenkomt met de hiërarchie in het sjabloonbestand die wordt gebruikt om deze te maken.

Bijvoorbeeld, als ik ga naar het hulpprogramma [Azure Resource Explorer](https://resources.azure.com) en vouw in de Verkenner de, ziet ik de resourcegroep en het hoofdniveau resources die zijn verzameld onder het type desbetreffende resource.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Als u op een web-app inzoomen, kunt u moeten zien web app-configuratiegegevens dat vergelijkbaar is met de onderstaande schermafbeelding:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Klik nogmaals de geneste bronnen een hiërarchie die vergelijkbaar zijn met die in uw JSON-sjabloonbestand nodig hebt en ziet u de instellingen van de app, verbindingstekenreeksen met de, enzovoort, correct worden doorgevoerd in het deelvenster JSON. Het ontbreken van deze instellingen kan duiden op een probleem met de JSON-bestand en kunt u problemen met het sjabloonbestand JSON.

## <a name="deploy-the-resource-group-template-yourself"></a>Implementatie van de resource groep-sjabloon ##

De knop **distribueren naar Azure** is handig, maar u kunt het implementeren van de resource groep sjabloon in azuredeploy.json alleen als u al azuredeploy.json naar GitHub hebt verplaatst. De SDK van Azure .NET biedt ook de hulpmiddelen voor het implementeren van een sjabloonbestand JSON rechtstreeks vanuit uw lokale computer. U doet dit door de volgende stappen uit te voeren:

1.  Klik op **bestand**in Visual Studio > **Nieuw** > **Project**.

2.  Klik op **Visual C#** > **Cloud** > **Azure resourcegroep**, klikt u op **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Azure-sjabloon selecteren**, selecteer **Lege sjabloon** en klik op **OK**.

4.  Sleep azuredeploy.json naar de map met **sjablonen** van het nieuwe project.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Open de gekopieerde azuredeploy.json in Solution Explorer.

6.  Alleen voor het tekstvenster, laten we eens enkele standaard toepassing inzicht resources toevoegen aan onze JSON-bestand, door te klikken op **Resource toevoegen**. Als u alleen geïnteresseerd bent in het bestand JSON implementeert, gaat u verder met de stappen deploy.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Selecteer **Toepassing inzichten voor Web-Apps**, Controleer of een bestaand App-Service plannen en WebApp is geselecteerd en klik vervolgens op **toevoegen**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    U kunt nu wel om diverse nieuwe bronnen die, afhankelijk van de resource en wat het doet, afhankelijkheden hebben op het App-abonnement of de WebApp. Deze resources niet zijn ingeschakeld in hun bestaande definitie en u gaat u dit kunt wijzigen.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Klik in het overzicht JSON op **appInsights automatisch schalen** als u wilt markeren, de JSON-code. Dit is de schaal instelling voor uw App serviceplan.

9.  Zoek in de gemarkeerde JSON-code, de `location` en `enabled` eigenschappen en stel deze zoals hieronder wordt weergegeven.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. Klik in het overzicht JSON op **CPUHigh appInsights** als u wilt markeren, de JSON-code. Dit is een melding.

11. Zoek de `location` en `isEnabled` eigenschappen en stel deze zoals hieronder wordt weergegeven. Doe hetzelfde voor de andere drie waarschuwingen (paarse bollen).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. U bent nu klaar om te implementeren. Met de rechtermuisknop op het project en selecteer **Deploy** > **Nieuwe implementatie**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Meld u aan bij uw Azure-account als u dat nog niet had gedaan.

14. Selecteert u een bestaande resourcegroep in uw abonnement of een nieuw account te maken, selecteer **azuredeploy.json**en klik vervolgens op **Bewerken van Parameters**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    U kunt nu mogelijk om alle parameters die zijn gedefinieerd in het sjabloonbestand in een nette tabel te bewerken. Parameters die standaardwaarden definiëren beschikt al over de standaardwaarden en parameters die definiëren van een lijst met toegestane waarden wordt weergegeven als vervolgkeuzelijsten.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Vul de lege parameters en gebruikt u het [adres van de GitHub cessies‑retrocessies voor ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) in **repoUrl**. Klik vervolgens op **Opslaan**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] AutoScaling is een functie die wordt aangeboden in **Standard** laag of hoger en waarschuwingen voor abonnement op gebruikersniveau zijn functies die worden aangeboden in **eenvoudige** laag of hoger, moet u de parameter **sku** ingesteld op **Standard** of **Premium** om te zien van alle uw nieuwe App inzichten resources omhoog licht.
    
16. Klik op **implementeren**. Als u het **opslaan van wachtwoorden**hebt geselecteerd, kan het wachtwoord, worden opgeslagen in de parameter bestand **als tekst zonder opmaak**. Anders wordt u gevraagd het wachtwoord invoeren tijdens de implementatie.

Dat is alles. Nu u hoeft u alleen om te gaan naar de [Azure-Portal](https://portal.azure.com/) en het hulpprogramma [Azure Resource Explorer](https://resources.azure.com) om de meldingen voor nieuwe weer te geven en automatisch schalen instellingen toegevoegd aan uw JSON geïmplementeerd toepassing.

De stappen in deze sectie voornamelijk uitgevoerd als volgt te werk:

1.  Bereid het sjabloonbestand
2.  Een parameterbestand om te gaan met het sjabloonbestand is gemaakt
3.  Het sjabloonbestand met de parameterbestand geïmplementeerd

De laatste stap wordt eenvoudig uitgevoerd door een PowerShell-cmdlet. Als u wilt zien wat Visual Studio toen het de toepassing gedistribueerd, open Scripts\Deploy-AzureResourceGroup.ps1. Er is een groot aantal er code, maar ik Ga alleen de relevante code moet u het sjabloonbestand met de parameterbestand implementeren markeren.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

De laatste cmdlet `New-AzureResourceGroup`, is de fase die daadwerkelijk de actie uitvoeren. Dit alles moet aan u demonstreren dat, met behulp van tooling, relatief eenvoudig is naar de cloud-toepassing volgens plan implementeren. Telkens wanneer u de cmdlet op dezelfde sjabloon met hetzelfde parameterbestand uitvoeren, gaat u hetzelfde resultaat te krijgen.

## <a name="summary"></a>Overzicht ##

In DevOps zijn herhaalbaarheid en voorspelbaarheid sleutels voor een succesvolle implementatie van een hoge schaalbaarheid-toepassing op basis van microservices. In deze zelfstudie hebt u een toepassing twee microservice Azure als één resourcegroep met de sjabloon Azure resourcemanager geïmplementeerd. Het opgegeven Hopelijk, heeft de kennis die u nodig hebt bij beginnen met het converteren van uw toepassing in Azure wordt aangegeven in een sjabloon en kunt inrichten en deze volgens plan implementeren. 

## <a name="next-steps"></a>Volgende stappen ##

Informatie over het [toepassen van agile methoden en continu publiceren uw toepassing microservices met eenvoudige](app-service-agile-software-development.md) en geavanceerde implementatie technieken zoals [flighting implementatie](app-service-web-test-in-production-controlled-test-flight.md) snel vinden.

<a name="resources"></a>
## <a name="more-resources"></a>Meer informatiebronnen ##

-   [Azure resourcemanager sjabloon taal](../resource-group-authoring-templates.md)
-   [Een presentatie Azure resourcemanager-sjablonen](../resource-group-authoring-templates.md)
-   [Azure resourcemanager sjabloon functies](../resource-group-template-functions.md)
-   [Een toepassing met Azure resourcemanager sjabloon implementeren](../resource-group-template-deploy.md)
-   [Via Azure PowerShell met Azure resourcemanager](../powershell-azure-resource-manager.md)
-   [Probleemoplossing resourcegroep implementaties in Azure wordt aangegeven](../resource-manager-troubleshoot-deployments-portal.md)




 
