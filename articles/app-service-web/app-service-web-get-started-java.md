<properties 
    pageTitle="Uw eerste Java-web-app distribueren naar Azure in vijf minuten | Microsoft Azure" 
    description="Leer hoe makkelijk het is om uit te voeren WebApps in App Service door het implementeren van een steekproef-app. Start de reële ontwikkeling snel doen en direct resultaten weer te geven." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Uw eerste Java-web-app distribueren naar Azure in vijf minuten

Deze zelfstudie kunt u een eenvoudige Java-WebApp implementeren naar [Azure App-Service](../app-service/app-service-value-prop-what-is.md).
U kunt App Service WebApps, [mobiele app back-uiteinden](/documentation/learning-paths/appservice-mobileapps/)en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md)maken.

U wordt: 

- Een WebApp maakt in Azure App-Service.
- Een voorbeeld Java-app implementeren.
- Zie uw code die wordt uitgevoerd in productie wonen.

## <a name="prerequisites"></a>Vereisten voor

- Een FTP-/ TRANSFEREERT mailclient gebruikt, zoals [FileZilla](https://filezilla-project.org/)krijgen.
- Een Microsoft Azure-account aanschaffen. Als u geen account hebt, kunt u [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) of [activeren van de voordelen van uw Visual Studio-abonnee](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] U kunt [Proberen App Service](http://go.microsoft.com/fwlink/?LinkId=523751) zonder een Azure-account. Een starter-app maken en afspelen met voor een uur lang--geen creditcard vereist, niet verplichtingen.

<a name="create"></a>
## <a name="create-a-web-app"></a>Een WebApp maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met uw Azure-account.

2. Klik op **Nieuw**in het linkermenu > **Web + Mobile** > **Web App**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. In het blad app maken, gebruikt u de volgende instellingen voor de nieuwe app:

    - **App-naam**: Typ een unieke naam.
    - **Resourcegroep**: Selecteer **Nieuw** en geef een naam op voor de resourcegroep.
    - **App-Service plannen/locatie**: Klik hierop om te configureren en klik op **Nieuw** om in te stellen van de naam, locatie en prijzen laag van het App-abonnement. Je mag rustig gebruikt u de **gratis** prijzen van laag.

    Wanneer u klaar bent, worden uw app maken blade ziet er als volgt:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Klik onderaan op **maken** . U kunt klikken op het pictogram **melding** aan de bovenkant om de voortgang weer te geven.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Wanneer de implementatie is voltooid, ziet u deze melding. Klik op het bericht te openen van uw implementatie blade.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. In het blad **implementatie is voltooid** , klikt u op de koppeling van de **Resource** om te openen van uw nieuwe web-app blade.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Een app Java implementeren naar uw web-app

Nu kunnen we een Java-app implementeren naar Azure TRANSFEREERT gebruiken.

5. Schuif omlaag naar **Toepassingsinstellingen** of zoekt u het in het blad web-app, en vervolgens klikt u erop. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java-versie**, selecteer **Java 8** en klik op **Opslaan**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Als u de melding **bijgewerkt web app-instellingen**gaan, gaat u naar http://*&lt;toepassingsnaam >*. azurewebsites.net de standaard JSP servlet in actie zien.

7. Schuif omlaag naar de **implementatie-referenties** of zoekt u het terug in het blad web-app, en vervolgens klikt u erop.

8. Stel uw referenties implementatie en klik op **Opslaan**.

7. Klik in het blad web-app op **Overzicht**. Klik naast **FTP-implementatie-gebruikersnaam** en **TRANSFEREERT hostname**, op de knop **kopiëren** als u wilt kopiëren van deze waarden.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    U kunt nu naar uw app Java met TRANSFEREERT implementeren.

8. In uw FTP-/ TRANSFEREERT-client, meld u aan bij uw Azure WebApp FTP-server met de waarden die u hebt gekopieerd in de laatste stap. Gebruik de implementatie-wachtwoord die u eerder hebt gemaakt.

    De volgende schermafbeelding ziet u logboekregistratie in FileZilla gebruiken.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    U ziet mogelijk beveiligingswaarschuwingen voor de niet-herkende SSL-certificaat van Azure. Verdergaan en doorgaan.

9. Klik op [deze koppeling](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) om te downloaden van het WAR-bestand naar uw lokale computer.

9. Navigeer naar **/site/wwwroot/webapps** in de externe site in uw FTP-/ TRANSFEREERT-client en sleept u het gedownloade bestand WAR op uw lokale computer naar deze map externe.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Klik op **OK** als u wilt overschrijven van het bestand in Azure wordt aangegeven.

    >[AZURE.NOTE] Volgens het standaardgedrag van Tomcat, bestandsnaam **ROOT.war** in /site/wwwroot/webapps biedt de hoofdmap web-app (http://*&lt;toepassingsnaam >*. azurewebsites.net), en de bestandsnaam ** * &lt;anyname >*.war** biedt u een benoemde WebApp (http://*&lt;toepassingsnaam >*.azurewebsites.net/*&lt;anyname >*).

Dat is alles. Uw Java-app wordt nu uitgevoerd live in Azure wordt aangegeven. Ga in uw browser naar http://*&lt;toepassingsnaam >*. azurewebsites.net in actie zien. 

## <a name="make-updates-to-your-app"></a>Updates aanbrengen in uw app

Wanneer u een bijwerkt moet, moet u alleen het nieuwe WAR-bestand uploaden naar dezelfde externe map met de klant FTP/TRANSFEREERT.

## <a name="next-steps"></a>Volgende stappen

[Een Java-web-app van een sjabloon in de Azure Marketplace maken](web-sites-java-get-started.md#marketplace). U kunt uw eigen volledig aanpasbaar Tomcat container en krijgen van de vertrouwde Manager UI. 

Fouten opsporen in uw Azure web-app, rechtstreeks in [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) of [Eclips](app-service-web-debug-java-web-app-in-eclipse.md).

Of Doe meer met uw eerste web-app. Bijvoorbeeld:

- [Andere manieren om te implementeren van uw code wilt Azure](../app-service-web/web-sites-deploy.md)uitproberen. 
- Duren voordat u uw Azure-app naar een hoger niveau. Uw gebruikers verifiëren. De schaal die het gebaseerd op aanvraag. Sommige prestatiewaarschuwingen instellen. Alle met een paar muisklikken. Zie [functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).

