<properties
   pageTitle="Azure Resource groep Visual Studio-projecten | Microsoft Azure"
   description="Gebruik Visual Studio een groepsproject Azure resource maken en implementeren van de bronnen aan Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Maken en implementeren van Azure resourcegroepen tot en met Visual Studio

Met Visual Studio en de [SDK van Azure](https://azure.microsoft.com/downloads/), kunt u een project dat uw infrastructuur en code in Azure implementeren maken. U kunt bijvoorbeeld de webhost, website en database definiëren voor de app en implementeren van deze infrastructuur samen met de code. Of u kunt een virtuele Machine, Virtual Network en opslag Account definiëren en implementeren van deze infrastructuur samen met een script die wordt uitgevoerd op virtuele Machine. De implementatie van **Azure Resource** groepsproject kunt u alle benodigde resources in een bewerking één, herhaald implementeren. Zie [overzicht van de Azure resourcemanager](azure-resource-manager/resource-group-overview.md)voor meer informatie over de implementatie en om uw resources te beheren.

Azure resourcegroep projecten bevatten Azure resourcemanager JSON-sjablonen, die de resources die u dashboard naar Azure implementeren definiëren. Zie voor meer informatie over de elementen van de sjabloon resourcemanager, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md). Visual Studio kunt u deze sjablonen bewerken en biedt hulpmiddelen die werken met sjablonen te vereenvoudigen.

In dit onderwerp implementeren u een WebApp en SQL-Database. De stappen zijn echter nagenoeg hetzelfde voor elk type resource. U kunt als eenvoudig implementeren een virtuele Machine en de gerelateerde bronnen. Visual Studio biedt veel verschillende starter sjablonen voor de implementatie van veelvoorkomende scenario's.

In dit artikel leest Visual Studio 2015 Update 2 en Microsoft Azure SDK voor .NET 2,9. Als u Visual Studio 2013 met Azure SDK 2,9 gebruikt, is uw ervaring grotendeels hetzelfde. U kunt versies van de Azure SDK van 2,6 of hoger. de functionaliteit van de gebruikersinterface mogelijk anders dan de gebruikersinterface in dit artikel vermelde. Het is raadzaam dat u de nieuwste versie van de [Azure SDK](https://azure.microsoft.com/downloads/) installeren voordat u begint met de stappen. 

## <a name="create-azure-resource-group-project"></a>Project Azure resourcegroep maken

In deze procedure kunt u een project Azure resourcegroep maken met een sjabloon **in de browser + SQL** .

1. Kies in Visual Studio, **bestand**, **Nieuw Project**, kiest u **C#** of **Visual Basic**. Kies **Cloud**, en kies vervolgens **De resourcegroep Azure** project.

    ![Cloud-implementatie-Project](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. De sjabloon kiezen die u wilt implementeren naar Azure resourcemanager. Zoals u ziet er zijn tal van opties op basis van het type project dat u wilt implementeren. Voor dit onderwerp, de **Web-app + SQL** -sjabloon te kiezen.

    ![Een sjabloon kiezen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    De sjabloon die u kiest is alleen een beginpunt; u kunt toevoegen en verwijderen van resources om te voldoen aan uw scenario.

    >[AZURE.NOTE] Visual Studio Hiermee haalt u een lijst met beschikbare sjablonen online. De lijst wordt gewijzigd.

    Visual Studio Hiermee maakt u een resource implementatie groepsproject voor de WebApp en SQL-database.

1. Als u wilt zien wat u hebt gemaakt, de Vouw in het implementatieproject uit te voeren.

    ![knooppunten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Omdat we hebt gekozen voor het WebApp + SQL-sjabloon voor dit voorbeeld, ziet u de volgende bestanden: 

  	|Bestandsnaam|Beschrijving|
  	|---|---|
  	|Implementeren AzureResourceGroup.ps1|Een PowerShell-script die PowerShell-opdrachten om te implementeren naar Azure resourcemanager activeert.<br />**Opmerking** Visual Studio wordt deze PowerShell-script om het implementeren van uw sjabloon. Wijzigingen die u aanbrengt in dit script van invloed zijn op implementatie in Visual Studio, zorg er dan.|
  	|WebSiteSQLDatabase.json|De sjabloon resourcemanager die de infrastructuur definieert u wilt implementeren naar Azure en de parameters die u tijdens de implementatie kunt geven. Deze definieert ook de afhankelijkheden tussen de bronnen zodat resourcemanager de resources in de juiste volgorde implementeert.|
  	|WebSiteSQLDatabase.parameters.json|Een parameterbestand die nodig zijn voor de sjabloon waarden bevat. U doorgeven parameterwaarden om aan te passen elk implementatie.|

    Alle resource implementatie groepsprojecten bevatten deze eenvoudige bestanden. Andere projecten mogelijk extra bestanden voor de andere functies bevatten.

## <a name="customize-the-resource-manager-template"></a>De sjabloon resourcemanager aanpassen

U kunt een implementatieproject doordat de JSON-sjablonen die beschrijven van de bronnen die u wilt implementeren. JSON staat voor JavaScript-Object notatie en is een serienummer gegevensindeling dat gemakkelijk is te werken met. De JSON-bestanden een schema waarnaar u boven aan elke bestand verwijst gebruiken. Als u voor meer informatie over het schema wilt, kunt u deze kunt downloaden en analyseren. Het schema wordt gedefinieerd welke elementen zijn geldig, de typen en de opmaak van velden, mogelijke waarden van opgesomde waarden, enzovoort. Zie voor meer informatie over de elementen van de sjabloon resourcemanager, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).

U op de sjabloon **WebSiteSQLDatabase.json**te openen.

De Visual Studio-editor biedt hulpmiddelen om u te helpen met het bewerken van de sjabloon resourcemanager. De **JSON** -overzichtsvenster kunt u gemakkelijk om te zien van de elementen die is gedefinieerd in de sjabloon.

![overzicht van JSON weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Een van de elementen selecteren in het overzicht, gaat u naar dit gedeelte van de sjabloon en wordt de bijbehorende JSON gemarkeerd.

![JSON navigeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

U kunt een resource toevoegen door op de knop **Resource toevoegen** boven aan het JSON overzichtsvenster selecteren of door met de rechtermuisknop op **resources** en **Nieuwe bron toevoegen**te selecteren.

![resources toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Selecteer **Opslag-Account** en geef deze een naam voor deze zelfstudie. Geef een naam die niet meer dan 11 tekens en bevat alleen getallen en kleine letters.

![opslag toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

U ziet dat niet alleen is de resource die is toegevoegd, maar ook een parameter voor het type opslag-account en een variabele voor de naam van het account opslag.

![overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

De parameter **storageType** is vooraf gedefinieerde met toegestane typen en een standaardtype. U kunt deze waarden verlaten of bewerk ze voor uw scenario. Als u niet dat anderen een **Premium_LRS** opslag-account via deze sjabloon implementeren wilt, kunt u deze uit de toegestane typen verwijderen. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio bevat ook intellisense waarmee u meer informatie over welke eigenschappen zijn beschikbaar bij het bewerken van de sjabloon. Bijvoorbeeld als u wilt bewerken in de eigenschappen voor uw App serviceplan, navigeer naar de resource **HostingPlan** en een waarde voor de **Eigenschappen**toevoegen. Zoals u ziet dat intellisense ziet u de beschikbare waarden en een beschrijving van die waarde bevat.

![intellisense weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

U kunt **numberOfWorkers** ingesteld op 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Het project resourcegroep implementeren naar Azure

U bent nu klaar om te implementeren van uw project. Wanneer u een project Azure resourcegroep implementeert, kunt u deze implementeert bij een Azure resourcegroep. De resourcegroep is een logische groepering van resources die een gemeenschappelijke levenscyclus delen.

1. Kies in het snelmenu van het project-knooppunt implementatie, **Deploy** > **Nieuwe implementatie**.

    ![Implementeert, nieuwe implementatie menu-item](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Het dialoogvenster **distribueren naar resourcegroep** wordt weergegeven.

    ![Implementeren in het dialoogvenster van een resourcegroep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Klik in de vervolgkeuzelijst **resourcegroep** kiest u een bestaande resourcegroep of een nieuw account te maken. Een resourcegroep maken, opent u het vak van de vervolgkeuzelijst **Resourcegroep** en kies **Nieuwe maken**.

    ![Implementeren in het dialoogvenster van een resourcegroep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Het dialoogvenster **Resourcegroep maken** wordt weergegeven. Geef uw groep een naam en locatie en selecteer de knop **maken** .

    ![Dialoogvenster resourcegroep maken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Bewerk de parameters voor de implementatie door de knop **Parameters bewerken** te selecteren.

    ![Knop Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Waarden voor de lege parameters bieden en selecteer de knop **Opslaan** . De lege parameters zijn **hostingPlanName**, **administratorLogin** **administratorLoginPassword**en **databasenaam**.

    **hostingPlanName** Hiermee geeft u een naam voor het [App-abonnement](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) om te maken. 
    
    **administratorLogin** Hiermee geeft u de gebruikersnaam voor de SQL Server-beheerder. Gebruik geen algemene beheerder namen zoals **sa** of **beheerder**. 
    
    De **administratorLoginPassword** Hiermee geeft u een wachtwoord voor SQL Server-beheerder. De optie voor het **opslaan van wachtwoorden als tekst zonder opmaak in het parameterbestand** is niet secure; daarom niet Selecteer deze optie. Aangezien het wachtwoord niet als tekst zonder opmaak opgeslagen wordt, moet u dit wachtwoord nogmaals tijdens de implementatie opgeven. 
    
    **databasenaam** Hiermee geeft u een naam voor de database te maken. 

    ![Het dialoogvenster Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Kies de knop **distribueren** naar het implementeren van het project naar Azure. Er wordt een PowerShell-console geopend buiten het exemplaar Visual Studio. Voer het wachtwoord van de beheerder van SQL Server in de PowerShell-console wanneer hierom wordt gevraagd. **De PowerShell-console kan worden verborgen achter andere items of geminimaliseerd in de taakbalk.** Zoekt u deze console en selecteer deze het wachtwoord in te voeren.

    >[AZURE.NOTE] Visual Studio gewoonlijk gevraagd u bij het installeren van de Azure PowerShell-cmdlets. Moet u eerst de Azure PowerShell-cmdlets voor het installeren resourcegroepen. Als u wordt gevraagd, installeert u deze.
    
1. De implementatie, kan een paar minuten duren. In de windows **uitvoer** ziet u de status van de implementatie. Als de implementatie is voltooid, geeft het laatste bericht aan een succesvolle implementatie met een ander nummer vergelijkbaar met:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. In een browser, open de [Azure-portal](https://portal.azure.com/) en meld u aan bij uw account. Als u wilt zien van het resourceveld groep, selecteert u **resourcegroepen** en de resourcegroep die u geïmplementeerd in.

    ![Selecteer groep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. U ziet alle geïmplementeerd resources. U ziet dat de naam van het account opslag niet precies wat u hebt opgegeven bij het toevoegen van deze resource. Het account opslag moet uniek zijn. De sjabloon wordt een reeks tekens automatisch toegevoegd aan de naam die u als u wilt een unieke naam opgeven. 

    ![resources weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Als u wijzigingen aanbrengt en implementeer deze opnieuw uw project wilt, kiest u de bestaande resourcegroep in het snelmenu van Azure resource groepsproject. In het snelmenu, kiest u **Deploy**en kies vervolgens de resourcegroep die u geïmplementeerd.

    ![Azure resourcegroep geïmplementeerd](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Met de infrastructuur van uw code implementeren

Nu u de infrastructuur voor de app hebt geïnstalleerd, maar er is geen werkelijke code geïmplementeerd aan het project. Dit onderwerp wordt uitgelegd hoe u een web-app en SQL-databasetabellen tijdens de implementatie. Als u een virtuele Machine in plaats van een web-app implementeert, die u wilt uitvoeren van bepaalde code op de computer als onderdeel van de implementatie. Het proces voor het implementeren van de code voor een web-app of voor het instellen van een virtuele Machine is nagenoeg hetzelfde.

1. Een project toevoegen aan uw Visual Studio-oplossing. Met de rechtermuisknop op de oplossing en selecteer **toevoegen** > **Nieuw Project**.

    ![project toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Voeg een **ASP.NET-webtoepassing**. 

    ![WebApp toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Selecteer **MVC** en schakelt u het veld voor **Host in de cloud** omdat de resource groepsproject kunt u deze taak uitvoeren.

    ![Selecteer MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Als uw web-app maakt in Visual Studio, ziet u beide projecten in de oplossing.

    ![projecten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Nu u nodig hebt om ervoor te zorgen dat uw groepsproject resource is op de hoogte van het nieuwe project. Ga terug naar uw groepsproject resource (AzureResourceGroup1). Met de rechtermuisknop op **verwijzingen** en selecteer **Toevoegen**.

    ![een verwijzing](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Selecteer het web app-project die u hebt gemaakt.

    ![een verwijzing](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Door toe te voegen een verwijzing, kunt u het web app-project koppelen aan de resource groepsproject en drie belangrijke eigenschappen automatisch ingesteld. U ziet deze eigenschappen in het venster **Eigenschappen** voor de verwijzing.

      ![Zie Naslaginformatie over](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    De eigenschappen zijn:

    - De **Aanvullende eigenschappen** bevat de web-implementatiepakket tijdelijke locatie die is verplaatst naar de opslag van Azure. Noteer de map (ExampleApp) en het bestand (package.zip). U krijgt deze waarden als parameters bij het distribueren van de app. 
    - Het **Bestandspad opnemen** bevat het pad waar het pakket wordt gemaakt. De **Doelen opnemen** bevat de opdracht die implementatie wordt uitgevoerd. 
    - De standaardwaarde van **maken; Pakket** mogelijk maakt de implementatie te maken van een webpakket voor implementatie (package.zip).  
    
    U hoeft niet een profiel publiceren als de implementatie de benodigde informatie in de eigenschappen die u wordt kunt het pakket maken.
      
1. Een resource toevoegen aan de sjabloon.

    ![resources toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Selecteer nu **Web implementeren voor Web Apps**. 

    ![web toevoegen implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Implementeer deze opnieuw in uw project van de groep resource aan de resourcegroep. Ditmaal zijn er enkele nieuwe parameters. U hoeft niet te bieden van waarden voor **_artifactsLocation** of **_artifactsLocationSasToken** omdat Visual Studio automatisch deze waarden genereert. U wel moet de map en de bestandsnaam ingesteld op het pad met het implementatiepakket (weergegeven als **ExampleAppPackageFolder** en **ExampleAppPackageFileName** in de volgende afbeelding). Geef de waarden die u eerder in de eigenschappen van de verwijzing (**ExampleApp** en **package.zip**) hebt gezien.

    ![web toevoegen implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Voor het **onderdeel opslag-account**, selecteert u het account dat is geïmplementeerd met deze resourcegroep.
    
1. Nadat de installatie is voltooid, selecteert u uw web-app in de portal. Selecteer de URL om te bladeren naar de site.

    ![Bladeren-site](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Zoals u ziet die u kunt de standaard ASP.NET-app hebt geïmplementeerd.

    ![geïmplementeerd app weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van uw bronnen via de portal, [met behulp van de Azure portal als u wilt uw Azure resources beheren](./azure-portal/resource-group-portal.md).
- Zie voor meer informatie over sjablonen, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
