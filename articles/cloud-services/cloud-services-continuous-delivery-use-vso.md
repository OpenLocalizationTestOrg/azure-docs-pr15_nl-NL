<properties
    pageTitle="Continue bezorging met Visual Studio Team-Services in Azure | Microsoft Azure"
    description="Informatie over het configureren van uw projecten Visual Studio Team teamservices automatisch bouwen en implementeren naar de Web App-functie in Azure App Service of cloud services."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Continue bezorging bij Azure met behulp van Visual Studio Team Services

U kunt uw projecten Visual Studio Team teamservices automatisch bouwen en implementeren naar Azure-WebApps of cloud services configureren.  (Zie [Continue bezorging voor Cloud Services in Azure wordt aangegeven](cloud-services-dotnet-continuous-delivery.md)voor informatie over het instellen van een doorlopend opbouwen en systeem met behulp van een *on-premises implementatie* Team Foundation Server implementeren.)

Deze zelfstudie wordt ervan uitgegaan dat u hebt Visual Studio 2013 en de SDK Azure is geïnstalleerd. Als u geen Visual Studio 2013 al hebt, kunt u deze door te kiezen op [www.visualstudio.com](http://www.visualstudio.com)op de koppeling **gratis aan de slag** downloaden. Installeer de SDK Azure vanaf [hier](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]U moet een Visual Studio Team Services-account om te voltooien van deze zelfstudie: U kunt [een Visual Studio Team Services-account gratis openen](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Als u wilt instellen op een cloudservice voor automatisch bouwen en implementeren naar Azure met behulp van Visual Studio Team Services, de volgende stappen uit.

## <a name="1-create-a-team-project"></a>1: een teamproject maken

Volg de instructies [hier](http://go.microsoft.com/fwlink/?LinkId=512980) maken van uw teamproject en dit koppelen aan Visual Studio. In dit scenario wordt ervan uitgegaan dat u Team Foundation versie besturingselement (TFVC) gebruikt als uw oplossing bron. Als u gebruiken cijfer voor versiebeheer wilt, raadpleegt u [de versie van het cijfer van deze procedure](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: een project voor het besturingselement voor gegevensbronnen inchecken

1. Openen van de oplossing die u wilt implementeren in Visual Studio, of een nieuw account te maken.
Volgens de stappen in deze procedure kunt u een web-app of een cloudservice (Azure-toepassing) implementeren.
Als u een nieuwe oplossing maakt wilt, maak een nieuw project van Azure-Cloudservice, of een nieuw MVC ASP.NET-project. Zorg ervoor dat het project is bedoeld voor .NET Framework-4 of 4.5 en als u een project van de service cloud maakt, een ASP.NET-MVC Webrol en een rol werknemer toevoegen en kies Internet-toepassing voor de Webrol. Wanneer u wordt gevraagd, kies **Internet-toepassing**.
Als u een WebApp maken wilt, de ASP.NET-webtoepassing project-sjabloon kiezen en kies vervolgens MVC. Zie [maken een ASP.NET-web-app in Azure App-Service](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] CI implementaties van Visual Studio-webtoepassingen ondersteund Visual Studio Team Services alleen op dit moment. Website projecten zijn buiten het bereik.

1. Open het contextmenu voor de oplossing en kies **Oplossing toevoegen aan een besturingselement voor gegevensbronnen**.

    ![][5]

1. Accepteer of de standaardwaarden wijzigen en kies de knop **OK** . Zodra het proces is voltooid, bron besturingselement pictogrammen worden weergegeven in **Solution Explorer**.

    ![][6]

1. Open het snelmenu voor de oplossing en kies **Inchecken**.

    ![][7]

1. Typ een beschrijving voor het inchecken in het gebied **Wachtende wijzigingen** van **Team Explorer**en klikt u op de knop **Inchecken** .

    ![][8]

    Houd rekening met de opties voor het opnemen of uitsluiten van specifieke wijzigingen wanneer u inchecken. Als de gewenste wijzigingen niet worden opgenomen, kiest u de koppeling **Alle opnemen** .

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: het project verbinden met Azure

1. Nu u een project voor het team van VS Team Services met enkele broncode erin hebt, u bent klaar om te verbinden met uw teamproject Azure.  Selecteer uw cloud-service of web-app in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)of maak een nieuwe record door te kiezen de **+** pictogram links onderaan en ** **Cloudservice** of Web App** en klik vervolgens **Snelle maken**te kiezen. Kies de koppeling **publiceren met Visual Studio Team Services instellen** .

    ![][10]

1. Typ de naam van uw Visual Studio Team Services-account in het tekstvak en klik op de koppeling **Autoriseer nu** in de wizard. U mogelijk gevraagd aan te melden.

    ![][11]

1. Kies in het pop-upvenster voor **Verbinding aanvragen** , de knop **accepteren** Azure voor het configureren van uw teamproject in de VS Team Services toe te staan.

    ![][12]

1. Wanneer autorisatie is geslaagd, ziet u een vervolgkeuzelijst met een lijst met uw team-projecten van Visual Studio Team Services. Kies de naam van de teamproject dat u hebt gemaakt in de vorige stappen en kies vervolgens van de wizard vinkje knop.

    ![][13]

1. Nadat u uw project is gekoppeld, ziet u enkele instructies voor de wijzigingen aan uw project Visual Studio Team Services-team zijn ingecheckt.  Klik op uw volgende inchecken, wordt Visual Studio Team Services bouwen en implementeren van uw project naar Azure.  Probeer dit nu door te klikken op de koppeling **Inchecken in Visual Studio** , en klikt u vervolgens de koppeling **Visual Studio starten** (of de overeenkomstige **Visual Studio** knop onderaan in de portal scherm).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: de tabel opnieuw activeren en implementeren van uw project

1. Kies de koppeling **Source Control Explorer** in Visual Studio **Team Explorer**.

    ![][15]

1. Ga naar het oplossingsbestand en deze openen.

    ![][16]

1. Klik in **Solution Explorer**openen van een bestand en wijzig deze. Het bestand bijvoorbeeld wijzigen `_Layout.cshtml` onder de weergaven\\gedeelde map in een MVC web beheerdersrol.

    ![][17]

1. Het logo voor de site bewerken en kies het bestand wilt opslaan, **Ctrl + S** .

    ![][18]

1. Kies de koppeling **Wachtende wijzigingen** in **Team Explorer**.

    ![][19]

1. Voer een opmerking in en kies vervolgens de knop **Inchecken** .

    ![][20]

1. Kies de knop **Start** om terug te keren naar de startpagina van het **Team Explorer** .

    ![][21]

1. Kies de koppeling **genereert** om weer te geven van de versies bezig.

    ![][22]

    **Team Explorer** ziet u dat een opbouwen geactiveerd is voor uw inchecken.

    ![][23]

1. Dubbelklik op de naam van de build bezig om weer te geven een gedetailleerd logboek naarmate het opbouwen vordert.

    ![][24]

1. Terwijl de build in uitvoering is, raadpleegt u de definitie opbouwen die is gemaakt wanneer u TFS gekoppeld aan Azure met behulp van de wizard.  Open het snelmenu voor de definitie opbouwen en kies **Bouwen definitie bewerken**.

    ![][25]

    In het tabblad **Trigger** ziet u dat de definitie opbouwen al dan niet standaard is ingesteld om aan te vullen elke inchecken.

    ![][26]

    In het tabblad **proces** ziet u dat de implementatieomgeving is ingesteld op de naam van uw service of web-app van cloud. Als u met WebApps werkt, wordt de eigenschappen die u ziet afwijken van de hier wordt getoond.

    ![][27]

1. Geef de waarden voor de eigenschappen als u verschillende waarden dan de standaardwaarden wilt. De eigenschappen voor de publicatie van Azure zijn in de sectie **implementatie** .

    De volgende tabel ziet u de beschikbare eigenschappen in het gedeelte **implementatie** :

  	|Eigenschap|Standaardwaarde|
  	|---|---|
  	|Niet-vertrouwde certificaten toestaan|Als de waarde false, moeten de SSL-certificaten worden ondertekend door een basisinstantie.|
  	|Upgrade toestaan|Hiermee kunt de implementatie bijwerken van een bestaande implementatie in plaats van een nieuwe id maken. Behoudt het IP-adres.|
  	|Niet verwijderen|Als de waarde true, niet de implementatie van een bestaande niet-gerelateerde overschrijven (upgrade is toegestaan).|
  	|Pad naar de implementatie-instellingen|Het pad naar het bestand .pubxml voor een web-app, ten opzichte van de hoofdmap van de cessies‑retrocessies. Voor cloudservices genegeerd.|
  	|SharePoint-implementatie-omgeving|Hetzelfde als de servicenaam van de.|
  	|Azure-implementatie-omgeving|De web-app of cloud servicenaam.|

1. Als u meerdere configuraties (.cscfg-bestanden) gebruikt, kunt u de gewenste service-configuratie opgeven bij de instelling **samenstelt, Geavanceerd MSBuild argumenten** . Bijvoorbeeld als u wilt gebruiken ServiceConfiguration.Test.cscfg, stel MSBuild argumenten lijn optie `/p:TargetProfile=Test`.

    ![][38]

    Op dat moment moet uw opbouwen worden voltooid.

    ![][28]

1. Als u dubbelklikt op de naam van de build, bevat Visual Studio een **Overzicht maken**, zoals testresultaten uit de bijbehorende eenheid testprojecten.

    ![][29]

1. In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), kunt u de bijbehorende implementatie weergeven op het tabblad **installaties** wanneer de testomgeving is geselecteerd.

    ![][30]

1.  Blader naar de URL van uw site. Voor een web-app, klikt u op de knop **Bladeren** op de opdrachtbalk. Voor een cloudservice, kiest u de URL in de sectie **Snelle bekijken** van **de dashboardpagina waarin de tijdelijke-omgeving voor een cloudservice** . Implementaties van continue integratie voor cloudservices zijn gepubliceerd naar de omgeving tijdelijke al dan niet standaard. U kunt dit wijzigen door de eigenschap **Alternatieve Cloud Service omgeving** naar **productie**. Deze schermafbeelding ziet waar de URL van de site op de pagina dashboard van de cloudservice is.

    ![][31]

    Een nieuw browsertabblad wordt geopend om uw actieve site weer te geven.

    ![][32]

    Voor cloudservices, als u andere wijzigingen in uw project aanbrengen en wordt u meerdere implementaties verzamelt u trigger meer genereert. De meest recente taak is gemarkeerd als actief.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: implementeer deze opnieuw een eerdere versie

Deze stap is van toepassing op cloudservices en is optioneel. Klik in de klassieke Azure portal kiest u een eerdere implementatie en kies vervolgens de knop **Implementeer deze opnieuw** aan uw site om een eerdere inchecken terugspoelen.  Houd er rekening mee dat Hiermee wordt een nieuwe build in TFS activeren en maak een nieuwe vermelding in de geschiedenis van uw implementatie.

![][34]

## <a name="6-change-the-production-deployment"></a>6: de productie-implementatie wijzigen

Deze stap geldt alleen voor cloudservices, niet-Webapps. Wanneer u klaar bent, kunt u de tijdelijke-omgeving naar de productieomgeving promoveren met de knop **wisselen** in de portal van Azure klassieke. De zojuist geïmplementeerd tijdelijke-omgeving is gepromoveerd tot productie en de vorige productieomgeving, indien van toepassing, wordt een tijdelijke-omgeving. Het is mogelijk dat de actieve implementatie verschillende voor de productie en tijdelijke omgevingen, maar de geschiedenis van de implementatie van recente versies is hetzelfde, ongeacht de omgeving.

![][35]

## <a name="7-run-unit-tests"></a>7: eenheidstests uitvoeren

Deze stap geldt alleen voor web apps, niet cloud services. Als u wilt een kwaliteit heeft bereikt hebt opgeslagen in de implementatie, kunt u eenheidstests uitvoeren en als ze mislukt, kunt u voorkomen dat de implementatie.

1.  Visual Studio, moet u een eenheid test-project toevoegen.

    ![][39]

1.  Projectverwijzingen toevoegen aan het project dat u wilt testen.

    ![][40]

1.  Voeg enkele eenheidstests per. Als u wilt beginnen, kunt u een pop-toets die altijd wordt doorgegeven.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  De definitie van de build bewerken, kiest u het tabblad **proces** , en de **Test** knooppunt.

1.  De **Fail opbouwen op test mislukt** ingesteld op waar. Dit betekent dat de implementatie zich niet tenzij de tests doorgeeft.

    ![][41]

1.  Een nieuwe build wachtrij.

    ![][42]

    ![][43]

1. Terwijl de build nadert, op de voortgang controleren.

    ![][44]

    ![][45]

1. Wanneer het bouwen klaar is, controleert u of de testresultaten.

    ![][46]

    ![][47]

1.  Maak een toets die, mislukt. Een nieuwe test toevoegen door te kopiëren van de eerste fase, wijzig de naam en de regel met code met de mededeling NotImplementedException een verwachte uitzondering.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Schakel in de wachtrij een nieuwe build wijzigen.

    ![][48]

1. Bekijk de testresultaten voor meer informatie over de fout.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Volgende stappen
Zie [eenheidstests in uw opbouwen uitvoeren](http://go.microsoft.com/fwlink/p/?LinkId=510474)voor meer informatie over testen in Visual Studio Team Services van de eenheden. Als u cijfer gebruikt, raadpleegt u [uw code in cijfer delen](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) en de [continue implementatie tot Azure App-Service](../app-service-web/app-service-continuous-deployment.md).  Zie voor meer informatie over Visual Studio Team Services, [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
