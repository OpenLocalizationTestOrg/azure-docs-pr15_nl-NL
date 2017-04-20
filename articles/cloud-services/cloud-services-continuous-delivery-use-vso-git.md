<properties
    pageTitle="Continue bezorging met cijfer en Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informatie over het configureren van uw projecten Visual Studio Team teamservices om cijfer te automatisch bouwen en implementeren naar de Web App-functie in Azure App Service of cloud services gebruiken."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Continue bezorging bij gebruik van Visual Studio Team Services en cijfer Azure

Visual Studio Team Services teamprojecten kunt u een cijfer opslagplaats fungeren als host voor uw broncode, en automatisch bouwen en implementeren naar Azure-WebApps of cloud services wanneer u een doorvoeren push naar de bibliotheek.

U moet Visual Studio 2013 en de SDK Azure is geïnstalleerd. Als u geen Visual Studio 2013 al hebt, kunt u deze door te kiezen op [www.visualstudio.com](http://www.visualstudio.com)op de koppeling **gratis aan de slag** downloaden. Installeer de SDK Azure vanaf [hier](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]U moet een Visual Studio Team Services-account om te voltooien van deze zelfstudie: U kunt [een Visual Studio Team Services-account gratis openen](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Als u wilt instellen op een cloudservice voor automatisch bouwen en implementeren naar Azure met behulp van Visual Studio Team Services, de volgende stappen uit.

## <a name="1-create-a-git-repository"></a>1: een cijfer opslagplaats maken

1. Als u niet al een Visual Studio Team Services-account hebt, kunt u er een [hier](http://go.microsoft.com/fwlink/?LinkId=397665). Wanneer u uw teamproject maakt, kiest u cijfer als uw systeem voor bronbeheer gebruikt. Volg de instructies voor Visual Studio verbinden met uw teamproject.

2. Kies de koppeling **deze opslagplaats klonen** in **Team Explorer**.

    ![][3]

3. Geef de locatie van de lokale versie en kies vervolgens de knop **klonen** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: een project maken en deze naar de bibliotheek doorvoeren

1. Kies de **nieuwe** koppeling naar een nieuw project maken in de lokale opslagplaats in **Team Verkenner**, in de sectie **oplossingen** .

    ![][4]

2. Volgens de stappen in deze procedure kunt u een web-app of een cloudservice (Azure-toepassing) implementeren. Maak een nieuw project van Azure-Cloudservice, of een nieuw MVC ASP.NET-project. Zorg ervoor dat het project is bedoeld voor de .NET Framework-4 of hoger. Als u een project van de service cloud maakt, een ASP.NET-MVC Webrol en een rol werknemer toevoegen.
Als u een WebApp maken wilt, de **ASP.NET-webtoepassing** project-sjabloon kiezen en kies vervolgens **MVC**. Zie [een ASP.NET-web-app in Azure App Service maken](../app-service-web/web-sites-dotnet-get-started.md) voor meer informatie.

3. Open het snelmenu voor de oplossing en kies **doorvoeren**.

    ![][7]

4. Als dit de eerste keer dat u hebt cijfer in Visual Studio Team Services gebruikt, moet u bepaalde gegevens om uzelf te identificeren in cijfer opgeven. Voer uw gebruikersnaam en e-mailadres in het gebied **Wachtende wijzigingen** van **Team Explorer**. Voer in een opmerking voor de gegevens zijn doorgevoerd en kies vervolgens de knop **vastleggen** .

    ![][8]

5. Houd rekening met de opties voor het opnemen of uitsluiten van specifieke wijzigingen wanneer u inchecken. Als u de gewenste wijzigingen niet zijn opgenomen, kies **Alle opnemen**.

6. U hebt nu de wijzigingen doorgevoerd in uw lokale kopie van de bibliotheek. Vervolgens deze wijzigingen met de server te synchroniseren door te kiezen van de **synchronisatie** -koppeling.

## <a name="3-connect-the-project-to-azure"></a>3: het project verbinden met Azure

1. Nu dat u een cijfer opslagplaats in Visual Studio Team Services met enkele broncode erin hebt, kunt u bent uw bibliotheek cijfer verbinden met Azure.  Selecteer uw cloud-service of web-app in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)of maak een nieuwe record door te kiezen de + pictogram links onderaan en ** **Cloudservice** of Web App** en klik vervolgens **Snelle maken**te kiezen.

    ![][9]

3. Voor cloudservices, kiest u de koppeling **publiceren met Visual Studio Team Services instellen** . Kies web-apps voor de koppeling voor het **instellen van de implementatie van een besturingselement voor gegevensbronnen** .

    ![][10]

2. Typ de naam van uw Visual Studio Team Services-account in het tekstvak en kies de koppeling **Autoriseer nu** in de wizard. U mogelijk gevraagd aan te melden.

    ![][11]

3. Kies in het pop-upvenster **Verbinding aanvragen** **accepteren** Azure voor het configureren van uw teamproject in Visual Studio Team Services toe te staan.

    ![][12]

4. Als autorisatie is geslaagd, ziet u een vervolgkeuzelijst met uw team-projecten van Visual Studio Team Services.  Selecteer de naam van de teamproject dat u hebt gemaakt in de vorige stappen en klikt u op van de wizard vinkje knop.

    ![][13]

    De volgende keer dat u het doorvoeren van gegevens naar uw bibliotheek, push wordt Visual Studio Team Services bouwen en implementeren van uw project naar Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: de tabel opnieuw activeren en implementeren van uw project

1. Visual Studio, opent u een bestand en wijzig deze. Het bestand bijvoorbeeld wijzigen `_Layout.cshtml` onder de weergaven\\gedeelde map in een MVC web beheerdersrol.

    ![][17]

2. Bewerk de tekst van de voettekst voor de site en sla het bestand.

    ![][18]

3. Open het snelmenu voor het knooppunt oplossing, projectknooppunt of het bestand dat u hebt gewijzigd in **Solution Explorer**en kies **doorvoeren**.

4. Typ in een opmerking en kies **doorvoeren**.

    ![][20]

5. Kies de **synchronisatie** -koppeling.

    ![][38]

6. Kies de **Push** -koppeling naar uw doorvoeren push naar de bibliotheek in Visual Studio Team Services. (U kunt ook gebruiken de knop **synchroniseren** naar uw doorvoeracties kopiëren naar de bibliotheek. Het verschil is dat **synchronisatie** ook Hiermee worden de meest recente wijzigingen van de bibliotheek.)

    ![][39]

7. Kies de knop **Start** om terug te keren naar de startpagina van het **Team Explorer** .

    ![][21]

8. Kies **genereert** om weer te geven van de versies bezig.

    ![][22]

    **Team Explorer** ziet u dat een opbouwen geactiveerd is voor uw inchecken.

    ![][23]

9. Dubbelklik op de naam van de build bezig om weer te geven een gedetailleerd logboek in de loop van het opbouwen.

10. Terwijl de build in uitvoering is, raadpleegt u de definitie opbouwen die is gemaakt wanneer u de wizard gebruikt om te koppelen aan Azure.  Open het snelmenu voor de definitie opbouwen en kies **Bouwen definitie bewerken**.

    ![][25]

11. In het tabblad **Trigger** ziet u dat de definitie opbouwen om aan te vullen elke inchecken, al dan niet standaard is ingesteld. (Een cloudservice, Visual Studio Team Services genereert en voor de basispagina tak automatisch naar de testomgeving implementeert. U hebt nog steeds moet een handmatige stap om te implementeren naar de persoonlijke site. Voor een web-app die geen tijdelijke omgeving, implementeert deze de basispagina tak rechtstreeks naar de persoonlijke site.

    ![][26]

1. In het tabblad **proces** ziet u dat de implementatieomgeving is ingesteld op de naam van uw service of web-app van cloud.

    ![][27]

1. Geef de waarden voor de eigenschappen als u verschillende waarden dan de standaardwaarden wilt. De eigenschappen voor de publicatie van Azure zijn in de sectie **implementatie** en moet u mogelijk ook MSBuild parameters instellen. Bijvoorbeeld in een wolk service project om op te geven van een service te configureren dan 'Cloud', kiest u de parameters MSbuild `/p:TargetProfile=[YourProfile]` waar *[YourProfile]* overeenkomt met een configuratiebestand service met een naam zoals ServiceConfiguration. *YourProfile*.cscfg.

    De volgende tabel ziet u de beschikbare eigenschappen in het gedeelte **implementatie** :

  	|Eigenschap|Standaardwaarde|
  	|---|---|
  	|Niet-vertrouwde certificaten toestaan|Als de waarde false, moeten de SSL-certificaten worden ondertekend door een basisinstantie.|
  	|Upgrade toestaan|Hiermee kunt de implementatie bijwerken van een bestaande implementatie in plaats van een nieuwe id maken. Behoudt het IP-adres.|
  	|Niet verwijderen|Als de waarde true, niet de implementatie van een bestaande niet-gerelateerde overschrijven (upgrade is toegestaan).|
  	|Pad naar de implementatie-instellingen|Het pad naar het bestand .pubxml voor een web-app, ten opzichte van de hoofdmap van de cessies‑retrocessies. Voor cloudservices genegeerd.|
  	|SharePoint-implementatie-omgeving|Hetzelfde als de servicenaam van de.|
  	|Azure-implementatie-omgeving|De web-app of cloud servicenaam.|

1. Op dat moment moet uw opbouwen worden voltooid.

    ![][28]

1. Als u dubbelklikt op de naam van de build, bevat Visual Studio een **Overzicht maken**, zoals testresultaten uit de bijbehorende eenheid testprojecten.

    ![][29]

1. In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), kunt u de bijbehorende implementatie weergeven op het tabblad **installaties** wanneer de testomgeving wordt geselecteerd.

    ![][30]

1.  Blader naar de URL van uw site. Kies de knop **Bladeren** op in de portal voor een web-app. Voor een cloudservice, kiest u de URL in de sectie **Snelle bekijken** van **de dashboardpagina waarin de tijdelijke-omgeving** .

    Implementaties van continue integratie voor cloudservices zijn gepubliceerd naar de omgeving tijdelijke al dan niet standaard. U kunt dit wijzigen door de eigenschap **Alternatieve Cloud Service omgeving** naar **productie**. Hier ziet u waar de URL van de site op de pagina dashboard van de cloudservice is.

    ![][31]

    Een nieuw browsertabblad wordt geopend om uw actieve site weer te geven.

    ![][32]

1.  Als u andere wijzigingen aan uw project maakt, u trigger meer maakt en wordt u meerdere implementaties verzamelt. De meest recente taak is gemarkeerd als actief.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: implementeer deze opnieuw een eerdere versie

Deze stap is optioneel. In de klassieke Azure portal, kiest u een eerdere implementatie en kies **Implementeer deze opnieuw** aan uw site om een eerdere inchecken terugspoelen. Houd er rekening mee dat Hiermee wordt een nieuwe build in TFS activeren en maak een nieuwe vermelding in de geschiedenis van uw implementatie.

![][34]

## <a name="6-change-the-production-deployment"></a>6: de productie-implementatie wijzigen

Wanneer u klaar bent, kunt u de tijdelijke-omgeving naar de productieomgeving promoveren door te **wisselen** in de portal van Azure klassieke te kiezen. De zojuist geïmplementeerd tijdelijke-omgeving is gepromoveerd tot productie en de vorige productieomgeving, indien van toepassing, wordt een tijdelijke-omgeving. Het is mogelijk dat de actieve implementatie verschillende voor de productie en tijdelijke omgevingen, maar de geschiedenis van de implementatie van recente versies is hetzelfde, ongeacht de omgeving.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: implementeren van een tak werken.

Wanneer u cijfer gebruikt, meestal wijzigingen aanbrengen in een tak werken en integreren in de basispagina tak wanneer uw ontwikkeling een klaar status bereikt. Tijdens de ontwikkelingsfase van een project wilt u bouw en implementeer de tak werken in Azure.

1. In **Team Explorer**, klikt u op de knop **Start** en kies vervolgens de knop **vertakkingen** .

    ![][40]

2. Kies de koppeling **Nieuwe tak** .

    ![][41]

3. Voer de naam van de onderliggende items, zoals "werkt," en kies **Tak maken**. Hiermee maakt u een nieuwe lokale tak.

    ![][42]

4. Publiceer de tak. Kies de naam van de tak in **niet-gepubliceerde vertakkingen**en kies **publiceren**.

    ![][44]

6. Alleen wijzigingen in de basispagina tak activeren standaard een doorlopend opbouwen. Als u wilt instellen continue opbouwen voor een tak werken, kiest u de pagina **genereert** in **Team Explorer**en kies **Bouwen definitie bewerken**.

7. Open het tabblad **Instellingen voor gegevensbron** . Kies onder **gecontroleerde vertakkingen voor doorlopende integratie en opbouwen**, de optie **Klik hier om een nieuwe rij toevoegen**.

    ![][47]

8. Geef de tak die u hebt gemaakt, zoals referenties/koppen/werken.

    ![][48]

9. Een wijziging aanbrengt in de code, opent u het snelmenu voor het gewijzigde bestand en kies vervolgens **doorvoeren**.

    ![][43]

10. Kies de koppeling **Niet-gesynchroniseerde doorvoeracties** en kies de knop **synchroniseren** of op de koppeling **Push** kunt u de gewijzigde kopiëren naar de kopie van de tak werken in Visual Studio Team Services.

    ![][45]

11. Ga naar de weergave **genereert** en zoek de build die u zojuist hebt geactiveerd voor de tak werken.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer tips voor het gebruik van cijfer in Visual Studio Team Services meer [ontwikkelen en delen van uw code in Visual Studio met cijfer](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) en Zie voor informatie over het gebruik van een cijfer opslagplaats waarin niet wordt beheerd door Visual Studio Team Services publiceren naar Azure [Continue implementatie naar Azure App-Service](../app-service-web/app-service-continuous-deployment.md). Zie voor meer informatie over Visual Studio Team Services, [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
