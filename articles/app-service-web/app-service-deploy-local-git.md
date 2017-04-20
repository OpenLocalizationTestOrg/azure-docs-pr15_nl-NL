<properties
    pageTitle="Lokale cijfer implementatie Azure App-service"
    description="Informatie over het inschakelen van lokale cijfer implementatie naar Azure App-Service."
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
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale cijfer implementatie Azure App-service

Deze zelfstudie ziet u hoe u uw app implementeren naar [Azure App-Service] uit een bibliotheek cijfer op uw lokale computer. App-Service ondersteunt deze methode met de optie **Lokale cijfer** implementatie in de [Portal van Azure].  
Veel van de opdrachten cijfer is beschreven in dit artikel worden automatisch uitgevoerd bij het maken van een App Service-app de [Interface van Azure-opdrachtregel] gebruiken als beschreven [hier](app-service-web-get-started.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

- Cijfer. U kunt de installatie binaire [hier](http://www.git-scm.com/downloads)downloaden.  
- Basiskennis van cijfer.
- Een Microsoft Azure-account. Als u geen account hebt, kunt u [registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial) of [activeren van de voordelen van uw Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter-app in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.  

## <a name="Step1"></a>Stap 1: Maak een lokale opslagplaats

De volgende taken uitvoeren om te maken van een nieuwe cijfer opslagplaats.

1. Start een hulpprogramma voor de opdrachtregel, zoals **GitBash** (Windows) of **Bash** (Unix-Shell). Op OS X-systemen u kunt toegang tot de opdrachtregel via de toepassing **Terminal** .

2. Navigeer naar de map waarin de inhoud te implementeren zich bevindt.

3. Gebruik de volgende opdracht uit een nieuwe cijfer opslagplaats geïnitialiseerd:

        git init

## <a name="Step2"></a>Stap 2: Uw inhoud doorvoeren

App-Service ondersteunt toepassingen die zijn gemaakt in diverse talen. 

1. Als uw bibliotheek bevat inhoud overslaan Hiermee wijst u en verplaatsen naar het punt 2 hieronder. Als uw bibliotheek niet inhoud gewoon bevat nog te vullen met een statische HTML-bestand als volgt: 

    - Met een teksteditor, maakt u een nieuw bestand met de naam **index.html** in de hoofdmap van de bibliotheek cijfer
    - De volgende tekst toevoegen terwijl de inhoud voor de index.html archiveren en sla deze: *Hallo cijfer!*
        
2. Vanaf de opdrachtregel, controleert u of u onder de hoofdsite van uw bibliotheek cijfer. Gebruik de volgende opdracht uit om te bestanden toevoegen aan uw bibliotheek:

        git add -A 

4. Doorvoeren van de wijzigingen vervolgens naar de bibliotheek met behulp van de volgende opdracht uit:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Stap 3: De App Service app-bibliotheek inschakelen

De volgende stappen om in te schakelen een cijfer opslagplaats voor uw App Service-app uitvoeren.

1. Meld u aan bij de [Portal van Azure].

2. Klik in uw App Service-app blade, op **Instellingen > implementatie bron**. Klik op **bron kiezen**en vervolgens op **Lokale cijfer opslagplaats**en klik vervolgens op **OK**.  

    ![Lokale cijfer opslagplaats](./media/app-service-deploy-local-git/local_git_selection.png)

3. Als dit de eerste keer bij het instellen van een bibliotheek in Azure wordt aangegeven, moet u aanmeldingsreferenties voor het maken. U kunt ze Meld u aan bij de Azure wijzigingen in de bibliotheek en push vanuit uw lokale cijfer opslagplaats. Van uw app blade, klik op **Instellingen > implementatie referenties**, configureer uw implementatie-gebruikersnaam en wachtwoord. Wanneer u klaar bent, klikt u op **Opslaan**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Stap 4: Implementeren in uw project

Gebruik de volgende stappen uw app publiceren naar App-Service met lokale cijfer.

1. Klik in de blade van uw app in de Portal Azure, op **Instellingen > Eigenschappen** voor het **Cijfer URL**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Cijfer URL** is de externe verwijzing naar implementeren naar vanuit uw lokale opslagplaats. U gebruikt deze URL in de volgende stappen uit.

2. Met de opdrachtregel, Controleer of u in de hoofdmap van uw lokale cijfer opslagplaats.

3. Gebruik `git remote` om toe te voegen van de externe verwijzing vermeld in **Cijfer URL** uit stap 1. De opdracht ziet er ongeveer als volgt uit:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] De **externe** opdracht een benoemde verwijzing naar een externe bibliotheek toegevoegd. In dit voorbeeld wordt een overzicht met de naam 'azure' voor uw web-app opslagplaats gemaakt.

4. Push uw inhoud naar de App-Service met de nieuwe **azure** externe die u zojuist hebt gemaakt.

        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt wanneer u uw referenties implementatie in de Portal Azure opnieuw instellen. Voer het wachtwoord (Let erop dat Gitbash niet sterretjes aan de console wordt echo terwijl u uw wachtwoord typt). 
       
5. Ga terug naar de app in de Portal Azure. Een vermelding van de meest recente push moet worden weergegeven in het blad **implementaties** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Klik op de knop **Bladeren** boven aan van de app blade om te controleren of dat de inhoud is geïmplementeerd. 
    
## <a name="Step5"></a>Problemen oplossen

Hierna ziet u fouten of problemen vaak optreden bij het gebruik van cijfer publiceren naar een App Service-app in Azure wordt aangegeven:

****

**Probleem**: kan niet naar access [siteURL]: kan geen verbinding met [scmAddress]

**Oorzaak**: deze fout kan optreden als de app niet actief is.

**Resolutie**: Start de app in de Portal van Azure. Cijfer implementatie werkt niet, tenzij de app wordt uitgevoerd. 


****

**Probleem**: kan niet host 'naam' oplossen

**Oorzaak**: deze fout kan optreden als de gegevens voor adres ingevoerd bij het maken van de 'azure' externe onjuist is.

**Oplossing**: Gebruik de `git remote -v` opdracht voor een overzicht van alle afstandsbedieningen, samen met de bijbehorende URL. Controleer of de URL voor de externe 'azure' klopt. Indien nodig, verwijderen en opnieuw maken van deze remote met de juiste URL.

****

**Symptoom**: geen referenties in gemeenschappelijke en geen opgegeven; niets te doen. Misschien moet u een tak zoals 'model'.

**Oorzaak**: deze fout kan optreden als u niet een tak opgeeft wanneer het uitvoeren van een cijfer push-bewerking en de push.default-waarde die wordt gebruikt door cijfer niet hebt ingesteld.

**Resolutie**: de push-bewerking opnieuw, geven de basispagina tak uitvoeren. Bijvoorbeeld:

    git push azure master

****

**Symptoom**: src refspec [branchname] niet overeenkomt.

**Oorzaak**: deze fout kan optreden als u probeert te voeren naar een tak dan basispagina op de 'azure' externe.

**Resolutie**: de push-bewerking opnieuw, geven de basispagina tak uitvoeren. Bijvoorbeeld:

    git push azure master

****

**Symptoom**: fout: wijzigingen die zijn doorgevoerd naar externe bibliotheek, maar uw web-app niet bijgewerkt.

**Oorzaak**: deze fout kan optreden als u een Node.js-app met een package.json-bestand waarmee wordt opgegeven aanvullende vereiste modules implementeert.

**Resolutie**: extra berichten met 'npm fout!' vastgelegde vóór deze fout moeten worden en bieden extra context bieden over de fout. Hieronder volgen enkele oorzaken van deze fout en de bijbehorende 'npm fout!' bekende Bericht:

* **Onjuiste package.json bestand**: npm fout! Afhankelijkheden kan niet worden gelezen.

* **Systeemeigen module die geen een binaire verdeling voor Windows**:

    * npm fout! \`cmd "/ c" "knooppunt-gyp opnieuw opbouwen"\` is mislukt 1

        OF-BEWERKING

    * npm fout! [modulename@version]vooraf: \`Zorg || gmake\`


## <a name="additional-resources"></a>Aanvullende informatie

* [Cijfer documentatie](http://git-scm.com/documentation)
* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Continu implementatie Azure App-service](app-service-continuous-deployment.md)
* [PowerShell gebruiken voor Azure](../powershell-install-configure.md)
* [Het gebruik van de opdrachtregel van Azure](../xplat-cli-install.md)

[Azure App-Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure-Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure opdrachtregel-Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
