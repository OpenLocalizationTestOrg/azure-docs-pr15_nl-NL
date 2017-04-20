<properties
    pageTitle="Continue implementatie Azure App service | Microsoft Azure"
    description="Informatie over het inschakelen van continue implementatie naar Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Continue implementatie Azure App-service

Deze zelfstudie ziet u hoe u een werkstroom continue implementatie voor uw app [Azure App-Service] configureren. App Service-integratie met BitBucket, GitHub en Visual Studio Team Services (VSTS) kunnen een doorlopend implementatie-werkstroom waar Azure in de meest recente updates van uw project gepubliceerd naar een van deze services ophaalt. Continue implementatie is een handige optie voor projecten waarin meerdere en veelgebruikte bijdragen aan goede doelen worden geïntegreerd.

## <a name="overview"></a>Continue implementatie inschakelen

Continue implementatie inschakelen 

1. De inhoud van uw app publiceren naar de bibliotheek die wordt gebruikt voor doorlopende implementatie.  
    Zie voor meer informatie over uw project publiceren naar deze services, [maken een cessies‑retrocessies (GitHub)], [een cessies‑retrocessies (BitBucket) maken]en [aan de slag met VSTS].

2. Klik in het menu-blade van uw app in de [portal van Azure], op **APP-implementatie > distributieopties**. Klik op **Bron kiezen**en selecteer vervolgens de implementatie-bron.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Als u wilt configureren van een VSTS account voor App Service implementatie raadpleegt u deze [zelfstudie](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Hiermee voert u de werkstroom autorisatie. 

4. Kies het project en tak om te implementeren uit in het blad **implementatie bron** . Wanneer u klaar bent, klikt u op **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Als u doorlopend-implementatie waarbij GitHub of BitBucket inschakelt, wordt de openbare en persoonlijke projecten worden weergegeven.

    App-Service Hiermee maakt u een koppeling met de geselecteerde bibliotheek, worden de bestanden uit de opgegeven tak en onderhoudt een klonen van uw bibliotheek voor de App Service-app. Als u VSTS continue implementatie van de Azure-portal configureert, de integratie wordt gebruikt voor de App Service [Kudu implementatie-engine](https://github.com/projectkudu/kudu/wiki)die al opbouwen en implementatie taken met automatiseert elke `git push`. U hoeft niet afzonderlijk instellen van continue implementatie in VSTS. Wanneer dit proces is voltooid, kan het blad **distributieopties** -app wordt weergegeven een actieve implementatie waarmee wordt aangegeven implementatie is voltooid.

5. Als u wilt controleren of dat de app is geïnstalleerd, klikt u op de **URL** van de app blade in de portal van Azure Klik boven aan. 

6. Push om te bevestigen dat continue implementatie uit de bibliotheek van uw keuze optreedt, een wijziging naar de bibliotheek. Uw app moet bijwerken met de wijzigingen kort nadat de push naar de bibliotheek is voltooid. U kunt controleren dat dit heeft binnengehaald in de update in het blad **distributieopties** van uw app.

## <a name="VSsolution"></a>Continue implementatie van een Visual Studio-oplossing 

Zet een Visual Studio-oplossing nieuwe naar Azure App-Service is net zo eenvoudig als het om een eenvoudige index.html-bestand. Het proces van de implementatie App Service stroomlijnt alle de details, inclusief NuGet afhankelijkheden herstellen en het bouwen van de toepassing binaire bestanden. U kunt Volg de aanbevolen procedures uit de bron-besturingselement van onderhouden code alleen in uw bibliotheek cijfer en laat de rest kunt verrichten App Service-implementatie.

De stappen voor uw Visual Studio-oplossing met App-Service drukken zijn hetzelfde als in de [vorige sectie](#overview), mits u als volgt uw oplossing en bibliotheek configureren:

-   De optie beheer van Visual Studio-bron gebruiken om te genereren een `.gitignore` bestand, zoals de afbeelding hieronder of handmatig toevoegen een `.gitignore` bestand in de hoofdmap van uw bibliotheek met de inhoud is vergelijkbaar met deze [.gitignore voorbeeld](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore). 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Mapstructuur van de volledige oplossing toevoegen aan uw bibliotheek, met het bestand .sln in de hoofdmap van de bibliotheek.

Zodra u hebt uw bibliotheek zoals is beschreven instellen en uw app in Azure wordt aangegeven voor continue publicatie vanaf een van de online cijfer opslagplaatsen geconfigureerd, kunt u uw ASP.NET-toepassing lokaal in Visual Studio ontwikkelen en implementeren continu uw code gewoon door te drukken van uw wijzigingen naar uw online cijfer-bibliotheek.

## <a name="disableCD"></a>Continue implementatie uitschakelen

Voor het uitschakelen van continue implementatie 

1. Klik in het menu-blade van uw app in de [portal van Azure], op **APP-implementatie > distributieopties**. Klik vervolgens op **verbinding verbreken** in het blad **distributieopties** .

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Na het beantwoorden van **Ja** naar het bevestigingsbericht, kunt u terug naar van uw app blade en klikt u op **APP-implementatie > distributieopties** als u wilt publiceren vanuit een andere bron instellen.

## <a name="additional-resources"></a>Aanvullende informatie

* [Hoe u veelvoorkomende problemen met continue implementatie onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [PowerShell gebruiken voor Azure]
* [Het gebruik van de Azure opdrachtregel hulpprogramma's voor Mac en Linux]
* [Cijfer documentatie]
* [Project-Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

[Azure App-Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure-portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[PowerShell gebruiken voor Azure]: ../articles/powershell-install-configure.md
[Het gebruik van de Azure opdrachtregel hulpprogramma's voor Mac en Linux]: ../articles/xplat-cli-install.md
[Cijfer documentatie]: http://git-scm.com/documentation

[Een cessies‑retrocessies (GitHub) maken]: https://help.github.com/articles/create-a-repo
[Een cessies‑retrocessies (BitBucket) maken]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Aan de slag met VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
