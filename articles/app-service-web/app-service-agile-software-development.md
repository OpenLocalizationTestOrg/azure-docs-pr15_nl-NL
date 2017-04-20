<properties
    pageTitle="Agile software development met Azure App-Service"
    description="Informatie over het maken van hoge schaalbaarheid complexe toepassingen met Azure App-Service op een manier die ondersteuning biedt voor agile software development."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Agile software development met Azure App-Service #

In deze zelfstudie leert u hoe hoge schaalbaarheid complexe toepassingen met [Azure App-Service](/services/app-service/) op een manier die ondersteuning biedt voor [agile software development](https://en.wikipedia.org/wiki/Agile_software_development)maakt. Dit wordt ervan uitgegaan dat u al weet hoe u het [implementeren van complexe toepassingen volgens plan in Azure wordt aangegeven](app-service-deploy-complex-application-predictably.md).

Beperkingen van technische processen kunnen de geslaagde implementatie van agile methoden vaak verhinderen. Azure App-Service met functies zoals [doorlopend publiceren](app-service-continuous-deployment.md), [tijdelijke omgevingen](web-sites-staged-publishing.md) (sleuven) en [monitoring](web-sites-monitor.md), wanneer zorgvuldig in combinatie met de integratie en beheer van de implementatie van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)kan deel uitmaken van een goede oplossing voor ontwikkelaars die Profiteer van agile software development.

De volgende tabel wordt een korte lijst met vereisten die zijn gekoppeld aan agile ontwikkeling, en hoe Azure kunt elk van deze services.

| Vereiste | Hoe Azure kunt |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Bouwen met elke doorvoeren<br>-Automatisch bouwen en snel | Wanneer geconfigureerd met continue implementatie, werken Azure App Service als builds van live uitvoeren op basis van een ontwikkelaar tak. Telkens wanneer de code is verplaatst naar de tak, is het automatisch-en-klare en lopende live in Azure wordt aangegeven.|
| -Controleer genereert zelf testen | Laden tests, web tests, enzovoort, met de sjabloon Azure ResourceManager kan worden geïmplementeerd.|
| -Tests in een klonen van productieomgeving uitvoeren | Azure resourcemanager sjablonen kunnen worden gebruikt om te klonen zijn van de Azure productieomgeving (inclusief de instellingen van de app, verbinding tekenreeks sjablonen, schaalbaarheid, enzovoort) voor het testen van snel en beter te maken.|
| -Weergeven resultaat van de meest recente versie eenvoudig | Continue implementatie van Azure uit een bibliotheek betekent dat u nieuwe code in een live-toepassing testen kunt direct nadat u uw wijzigingen doorvoeren. |
| -Doorvoeren in de belangrijkste tak elke dag<br>-Implementatie automatiseren | Elke doorvoeren/samenvoegen implementeren continue integratie van een productietoepassing met de belangrijkste vertakking van een bibliotheek automatisch in de belangrijkste tak aan productie. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Wat u doet ##

U doorlopen een doorsnee ontwikkelaar-test-fase-productieve werkstroom nieuwe wijzigingen in de toepassing van de steekproef [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , die bestaat uit twee [WebApps](/services/app-service/web/), namelijk een frontend (ve) en de andere wordt een Web API-backend (BE) en een [SQL-database](/services/sql-database/)publiceren. U werkt met de implementatiearchitectuur hieronder wordt weergegeven:

![](./media/app-service-agile-software-development/what-1-architecture.png)

De afbeelding om in te zetten woorden:

-   De implementatiearchitectuur onderverdeeld in drie verschillende omgevingen (of [resourcegroepen](../azure-resource-manager/resource-group-overview.md) in Azure), elk voorzien van een eigen [App serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [schaalbaarheid](web-sites-scale.md) -instellingen en SQL-database. 
-   Elke omgeving kan afzonderlijk worden beheerd. Ze kunnen zelfs bestaat niet in de verschillende abonnementen.
-   Ontwikkel- en worden geïmplementeerd als twee sleuven van de dezelfde App Service-app. De basispagina tak is ingesteld voor doorlopende integratie met het tijdelijk opslaan boven.
-   Wanneer een basispagina tak gegevens zijn doorgevoerd op het tijdelijk opslaan slot (met productiegegevens) is geverifieerd, wordt de gecontroleerde tijdelijk opslaan app omgezet in de productie slot [met geen downtime](web-sites-staged-publishing.md).

De omgeving productie en tijdelijke is gedefinieerd door de sjabloon [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

De ontwikkelaar en test omgevingen zijn gedefinieerd door de sjabloon [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

U wilt de normale vertakkingslogica strategie, ook gebruiken met code vanuit de ontwikkelaar tak snel aan de sectie testen, klikt u vervolgens verplaatsen naar de basispagina tak (zwevend omhoog in de kwaliteit, dus tot speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Wat u nodig hebt ##

-   Een Azure-account
-   Een [GitHub](https://github.com/) -account
-   Cijfer Shell (geïnstalleerd met [GitHub voor Windows](https://windows.github.com/)) - Hiermee kunt u zowel het cijfer en de PowerShell-opdrachten kunt uitvoeren in dezelfde sessie 
-   Meest recente [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) -bits
-   Basiskennis van de volgende handelingen uit:
    -   Implementatie van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) -sjabloon (ook Zie [Deploy een complexe toepassing volgens plan in Azure wordt aangegeven](app-service-deploy-complex-application-predictably.md))
    -   [Cijfer](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] U hebt een Azure-account om te voltooien van deze zelfstudie nodig:
> + U kunt [een Azure-account gratis openen](/pricing/free-trial/) - u tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze omhoog gebruikt kunt u het account en gebruik vrij te geven Azure services, zoals Web Apps.
> + U kunt [Visual Studio abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/) : uw Visual Studio abonnement geeft u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.
>
> Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="set-up-your-production-environment"></a>Uw productieomgeving instellen ##

>[AZURE.NOTE] Het script in deze zelfstudie gebruikt configureert automatisch continue publiceren vanaf uw GitHub-bibliotheek. Hiervoor is vereist dat uw referenties GitHub al zijn opgeslagen in Azure wordt aangegeven, anders kan de implementatie met scripts, mislukt tijdens een poging om het configureren van instellingen voor gegevensbron voor de Webapps. 
>
>Voor de opslag uw referenties GitHub in Azure wordt aangegeven, maakt u een web-app in de [Portal van Azure](https://portal.azure.com/) en [GitHub implementatie configureren](app-service-continuous-deployment.md). Alleen moet u dit één keer te doen. 

In een normale DevOps scenario u een toepassing die wordt uitgevoerd in Azure leven hebt en u wilt wijzigen door continue publiceren. In dit scenario hebt u een sjabloon die u ontwikkeld, getest en implementeren van de productieomgeving. U wordt instellen in deze sectie.

1.  Maak uw eigen zich splitsen van de bibliotheek [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Zie voor informatie over het maken van uw zich splitsen [zich een cessies‑retrocessies splitsen](https://help.github.com/articles/fork-a-repo/). Nadat uw zich splitsen is gemaakt, kunt u deze zien in uw browser.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Open een cijfer Shell-sessie. Als u nog geen cijfer Shell hebt, nu [GitHub voor Windows](https://windows.github.com/) installeren.

3.  Maak een lokale kopie van uw zich splitsen door de volgende opdracht uit te voeren:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Nadat u uw lokale klonen hebt, navigeer naar * &lt;repository_root >*\ARMTemplates en de deploy.ps1 als volgt een script uitvoeren:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Wanneer u wordt gevraagd, typt u in de gewenste gebruikersnaam en wachtwoord voor toegang tot de database.

    Hier ziet u de inrichten voortgang van verschillende Azure resources. Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Kijk eens naar * &lt;repository_root >*\ARMTemplates\Deploy.ps1, om te zien hoe resources met unieke id's wordt gegenereerd. U kunt dezelfde aanpak gebruiken om te klonen zijn van de implementatie van dezelfde maken zonder bang conflicterende resourcenamen.
 
6.  Terug in uw cijfer Shell-sessie, uitgevoerd:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Wanneer het script is voltooid, gaat u terug om naar het adres van de frontend (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) om de toepassing uitvoert in productie weer te geven.
 
5.  Meld u aan bij de [Portal van Azure](https://portal.azure.com/) en eens kijken wat wordt gemaakt.

    U moet kunnen Zie twee web-apps in dezelfde resourcegroep, met een de `Api` achtervoegsel in de naam. Als u de weergave van de groep resources bekijkt, worden ook ziet u de SQL-Database en server, het App-abonnement en voor de WebApps het tijdelijk opslaan elementen. Blader door de verschillende bronnen en vergelijk deze met * &lt;repository_root >*\ARMTemplates\ProdAndStage.json om te zien hoe deze zijn geconfigureerd in de sjabloon.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

U hebt nu de productieomgeving ingesteld. Vervolgens wordt u een nieuwe update om de toepassing te starten.

## <a name="create-dev-and-test-branches"></a>Ontwikkelaar maken en testen van vertakkingen ##

Nu dat u een complexe toepassing uitgevoerd in productie in Azure wordt aangegeven hebt, brengt u een update aan uw toepassing overeenkomstig agile methodologie. In dit gedeelte maakt u de ontwikkelaar en vertakkingen die u nodig hebt om de vereiste updates te testen.

1.  De testomgeving eerst maken. In uw cijfer Shell-sessie, voer de volgende opdrachten voor het maken van de omgeving voor een nieuwe tak **NewUpdate**genoemd. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Wanneer u wordt gevraagd, typt u in de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. 

    Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon. En net zoals dat u hebt nu een nieuwe tak met een eigen testomgeving. Neem even de tijd moet worden gereviseerd een paar dingen over deze testomgeving:

    -   U kunt deze hebt gemaakt in een Azure abonnement. Dat betekent dat de productieomgeving van uw testomgeving afzonderlijk kan worden beheerd.
    -   Uw testomgeving wordt uitgevoerd live in Azure wordt aangegeven.
    -   Uw testomgeving is gelijk aan de productieomgeving, behalve voor het tijdelijk opslaan sleuven en de schaal instellingen. U kunt dit weet, omdat dit de enige verschillen tussen ProdandStage.json en Dev.json zijn.
    -   U kunt uw testomgeving beheren in een eigen App Service-abonnement met een andere prijs laag (zoals **gratis**).
    -   Deze testomgeving verwijderen is net zo eenvoudig als het verwijderen van het resourceveld groep. Leert u hoe u deze [later](#delete).

2.  Ga door met het maken van een tak ontwikkelaar door de volgende opdrachten uit te voeren:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Wanneer u wordt gevraagd, typt u in de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. 

    Neem even de tijd moet worden gereviseerd een paar dingen over deze ontwikkelaar-omgeving: 

    -   Uw omgeving ontwikkelaar heeft een gelijk aan de testomgeving configuratie omdat deze is geïmplementeerd met dezelfde sjabloon.
    -   Elke ontwikkelaar-omgeving kan worden gemaakt in de ontwikkelaars eigen Azure abonnement, blijft de testomgeving afzonderlijk worden beheerd.
    -   Uw omgeving ontwikkelaar wordt uitgevoerd live in Azure wordt aangegeven.
    -   Verwijderen van de ontwikkelaar-omgeving is net zo eenvoudig als het verwijderen van het resourceveld groep. Leert u hoe u deze [later](#delete).

>[AZURE.NOTE] Als er meerdere ontwikkelaars werken op de nieuwe update, kan elk van deze eenvoudig maken een tak en speciale ontwikkelaar-omgeving als volgt:
>
>1. Maken van hun eigen zich splitsen van de bibliotheek in GitHub (Zie [zich splitsen een cessies‑retrocessies](https://help.github.com/articles/fork-a-repo/)).
>2. De zich splitsen op hun lokale computer klonen
>3. Voer de dezelfde opdrachten om hun eigen ontwikkelaar tak en omgeving te maken.

Wanneer u klaar bent, moet uw GitHub zich splitsen, drie vertakkingen hebben:

![](./media/app-service-agile-software-development/test-1-github-view.png)

En u zes WebApps (drie sets met twee) in drie aparte resourcegroepen nodig hebt:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Houd er rekening mee dat ProdandStage.json Hiermee geeft u de productieomgeving als u wilt gebruiken de **standaard** laag, die geschikt voor schaalbaarheid van de productietoepassing is prijzen.

## <a name="build-and-test-every-commit"></a>Bouwen en testen van elke doorvoeren ##

De sjabloonbestanden ProdAndStage.json en Dev.json opgeven al parameters voor de bron, die standaard ingesteld continue publiceren voor de web-app. Daarom activeert elke doorvoeren naar de tak GitHub een automatische implementatie naar Azure van die vertakking. Laten we eens kijken hoe de configuratie nu werkt.

1.  Zorg ervoor dat u aan de tak ontwikkelaar van de lokale opslagplaats deelneemt. Voer de volgende opdracht in cijfer Shell hiervoor:

        git checkout Dev

2.  Een eenvoudige wijziging aanbrengen aan van de app UI laag door te wijzigen van de code als u wilt gebruiken [Bootstrap](http://getbootstrap.com/components/) lijsten. Open * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml en om de gemarkeerde wijziging hieronder:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Als u niet kunt in de bovenstaande afbeelding vinden: 
    >
    >- Wijzig in regel 18 `check-list` naar `list-group`.
    >- Wijzig in regel 19 `class="check-list-item"` naar `class="list-group-item"`.

3.  Sla de wijziging. Terug in de schaal van de cijfer, voert u de volgende opdrachten:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Deze opdrachten cijfer zijn vergelijkbaar met "inchecken van uw code' in een andere bron besturingselement systeem zoals TFS. Wanneer u uitvoert `git push`, de nieuwe doorvoeren activeert een automatische code push naar Azure, waarin de toepassing op de wijziging weergegeven in de omgeving ontwikkelaar opnieuw ingesteld.

4.  Om te bevestigen dat deze push code in uw omgeving ontwikkelaar is opgetreden, gaat u naar uw ontwikkelaar-omgeving web app blade en kijkt u naar het deel achter de **implementatie** . U moet mogelijk om de meest recente doorvoeren-bericht weer te geven.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Klik op **Bladeren** om de nieuwe wijziging in het live van Azure daar.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Dit is een heel kleine wijziging aan de toepassing. Veel tijden nieuwe wijzigingen in een complexe webtoepassing heeft echter onbedoelde en ongewenste bijwerkingen. Wordt kunnen eenvoudig testen elke doorvoeren in live builds, kunt u deze problemen onderschept voordat ze uw klanten zien.

Nu zijn u weet hoe u de realisatie dat, als ontwikkelaar van het project **NewUpdate** u eenvoudig een ontwikkelaar-omgeving voor uzelf maken kunnen, en vervolgens elke doorvoeren bouwen en testen van elke opbouwen.

## <a name="merge-code-into-test-environment"></a>Code samenvoegen in testomgeving ##

Wanneer u klaar bent om te push van uw code uit ontwikkelaar tak maximaal NewUpdate tak, is het proces standaard cijfer:

1.  Alle nieuwe samenvoegen worden doorgevoerd in NewUpdate in de ontwikkelaar tak in GitHub, zoals doorvoeracties gemaakt door andere ontwikkelaars. Een nieuwe doorvoeren op GitHub wordt een push code activeren en maken in de ontwikkelaar-omgeving. U kunt Controleer of dat uw code in ontwikkelaar tak werkt nog steeds met de meest recente bits NewUpdate vertakken.

2.  Uw nieuwe doorvoeracties ontwikkelaar vertakken samenvoegen met NewUpdate tak op GitHub. Deze actie in de gebeurtenis een code push en opbouwen in de testomgeving. 

Notitie opnieuw dat omdat continue implementatie al ingesteld met deze vertakkingen cijfer is, u hoeft te maken van een andere actie zoals integratie met genereert. U hoeft alleen om uit te voeren standaard bron besturingselement procedures met cijfer en Azure alle opbouwen processen wordt uitgevoerd voor u.

Nu kunnen we uw code push naar **NewUpdate** tak. Voer de volgende opdrachten in cijfer Shell:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Dat is alles. 

Ga naar het blad web app voor uw testomgeving om uw nieuwe doorvoeren (samengevoegd NewUpdate tak) nu verplaatst naar de testomgeving weer te geven. Klik vervolgens op **Bladeren** om te zien dat de stijlwijziging wordt nu uitgevoerd live in Azure wordt aangegeven.

## <a name="deploy-update-to-production"></a>Update op productie implementeren ##

Zet nieuwe code voor het milieu ontwikkel- en het is belangrijk niet afwijken van wat u hebt al gereed wanneer u code naar de testomgeving verplaatst. Het is heel eenvoudig. 

Voer de volgende opdrachten in cijfer Shell:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Vergeet niet dat op basis van de manier waarop die de ontwikkel- en -omgeving ingesteld in ProdandStage.json is, uw nieuwe code naar de slot **tijdelijke** wordt gedistribueerd en er wordt uitgevoerd. Dus als u naar de URL voor het tijdelijk opslaan slot navigeert, u de nieuwe code die er wordt uitgevoerd ziet. Klik hiertoe uitvoeren de `Show-AzureWebsite` cmdlet in cijfer Shell.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
En nu, nadat u de update in het tijdelijk opslaan slot hebt geverifieerd, het enige wat u nog moet gebeuren vervangt in bedrijf. In de schaal van de cijfer, voert u de volgende opdrachten:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Gefeliciteerd! U hebt een nieuwe update naar uw webtoepassing productie gepubliceerd. Sterker nog is dat u net hebt uitgevoerd deze door te eenvoudig maken ontwikkelaar en testen omgevingen, en bij het maken en testen van elke doorvoeren. Dit zijn essentieel bouwstenen voor agile software development.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Verwijder ontwikkelaar en enviroments testen ##

Omdat u uw omgevingen ontwikkelaar en testen om te worden zelfstandig resourcegroepen opzettelijk ontworpen hebt, is het is heel eenvoudig om ze te verwijderen. Als u wilt verwijderen die u hebt gemaakt in deze zelfstudie GitHub vertakkingen zowel Azure onderdelen, alleen de volgende opdrachten worden uitgevoerd in cijfer Shell:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Overzicht ##

Agile software development is een verplichte nodig hebt voor veel bedrijven die bij het gebruik van Azure als hun toepassingsplatform. In deze zelfstudie hebt u geleerd hoe maken en naar beneden exacte replica's of nabij kopieën van de productieomgeving trek met gemak, zelfs voor complexe toepassingen. U hebt geleerd hoe om te profiteren van deze mogelijkheid voor het maken van een ontwikkelingsproces waarmee u kunt maken en testen van elke één doorvoeren in Azure wordt aangegeven. Deze zelfstudie Hopelijk leert u hoe u kunt best Azure App-Service en Azure resourcemanager samen gebruiken om te maken van een DevOps-oplossing die caters naar agile methoden. U kunt vervolgens op dit scenario maken door geavanceerde DevOps technieken zoals [testen in productie](app-service-web-test-in-production-get-start.md)te voeren. Zie voor een algemene testen-in-productieschema, [Flighting-implementatie (bètaversie testen) in Azure App-Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Meer informatiebronnen ##

-   [Een complexe toepassing volgens plan in Azure implementeren](app-service-deploy-complex-application-predictably.md)
-   [Agile ontwikkeling in de praktijk: Tips en trucs voor gemoderniseerde ontwikkelingscyclus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Geavanceerde implementatiestrategieën voor het Azure-WebApps resourcemanager sjablonen gebruiken](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Een presentatie Azure resourcemanager-sjablonen](../resource-group-authoring-templates.md)
-   [JSONLint - de JSON-validatie](http://jsonlint.com/)
-   [ARMClient – GitHub publiceren naar de site instellen](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Cijfer takken – basisfilters takken en samenvoegen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo van Blog](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Azure platforms opdrachtregel hulpmiddelen](../xplat-cli-install.md)
-   [Gebruikers maken of bewerken in Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Project Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
