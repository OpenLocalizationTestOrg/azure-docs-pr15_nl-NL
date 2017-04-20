<properties 
    pageTitle="Uw eerste Python web-app distribueren naar Azure in vijf minuten | Microsoft Azure" 
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
    
# <a name="deploy-your-first-python-web-app-to-azure-in-five-minutes"></a>Uw eerste Python web-app distribueren naar Azure in vijf minuten

Deze zelfstudie kunt u uw eerste Python web-app implementeren naar [Azure App-Service](../app-service/app-service-value-prop-what-is.md).
U kunt App Service WebApps, [mobiele app back-uiteinden](/documentation/learning-paths/appservice-mobileapps/)en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md)maken.

U wordt: 

- Een WebApp maakt in Azure App-Service.
- Voorbeeld Python code implementeren.
- Zie uw code die wordt uitgevoerd in productie wonen.
- Werk uw web-app u net als [push die cijfer doorgevoerd](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Vereisten voor

- [Cijfer](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- Een Microsoft Azure-account. Als u geen account hebt, kunt u [registreren voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) of [activeren van de voordelen van uw Visual Studio-abonnee](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] U kunt [Proberen App Service](http://go.microsoft.com/fwlink/?LinkId=523751) zonder een Azure-account. Een starter-app maken en afspelen met voor een uur lang--geen creditcard vereist, niet verplichtingen.

## <a name="deploy-a-python-web-app"></a>Een Python web-app implementeren

1. Open een nieuwe opdrachtprompt van Windows PowerShell-venster, Linux shell of terminal OS X. Uitvoeren `git --version` en `azure --version` om te bevestigen dat cijfer en Azure CLI zijn geïnstalleerd op uw computer.

    ![Installatie van CLI hulpprogramma's voor uw eerste WebApp testen in Azure wordt aangegeven](./media/app-service-web-get-started/1-test-tools.png)

    Als u de hulpmiddelen voor dit nog niet hebt geïnstalleerd, raadpleegt u de [vereisten](#Prerequisites) voor het van downloadkoppelingen.

3. Meld u aan bij Azure als volgt:

        azure login

    Het helpbericht kunt doorgaan met de aanmelding te volgen.

    ![Meld u aan bij Azure om uw eerste WebApp te maken](./media/app-service-web-get-started/3-azure-login.png)

4. Azure CLI wijzigen in de modus ASM en stelt u de implementatie-gebruiker voor App-Service. Implementeert u code met de referenties later.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Wijzigen in een werkmap (`CD`) en klonen van de steekproef-app als volgt:

        git clone https://github.com/Azure-Samples/app-service-web-python-get-started.git

2. Wijzigen naar de bibliotheek van de steekproef-app. Bijvoorbeeld:

        cd app-service-web-python-get-started

4. Maak de App Service app resource in Azure wordt aangegeven met de naam van een unieke app en de implementatie-gebruiker die u eerder hebt geconfigureerd. Wanneer u wordt gevraagd, geeft u het aantal het gewenste gebied.

        azure site create <app_name> --git --gitusername <username>

    ![Maken van de Azure resource voor de eerste WebApp in Azure wordt aangegeven](./media/app-service-web-get-started-languages/python-site-create.png)

    Uw app is nu gemaakt in Azure wordt aangegeven. De huidige map is ook cijfer-geïnitialiseerd en onderhouden met de nieuwe App Service-app als een externe cijfer.
    U kunt bladeren naar de app-URL (http://&lt;Naam_toepassing >. azurewebsites.net) de mooie standaard HTML-pagina zien, maar we toegang krijgen uw code er nu.

4. Uw voorbeeldcode dashboard implementeren naar uw Azure-app zoals u zou push-code met cijfer. Wanneer u wordt gevraagd, gebruikt u het wachtwoord dat u eerder hebt geconfigureerd.

        git push azure master

    ![Push-code bij uw eerste web-app in Azure wordt aangegeven](./media/app-service-web-get-started-languages/python-git-push.png)

    `git push`voegt het code in Azure wordt aangegeven, niet alleen, maar activeert ook implementatietaken in de implementatie-engine. 
    Als u alle bestanden requirements.txt (Python) in de hoofdmap van uw project (opslagplaats) hebt, herstelt u de vereiste pakketten met het implementatiescript voor u. 

Gefeliciteerd, u hebt uw app geïmplementeerd in Azure App-Service.

## <a name="see-your-app-running-live"></a>Zie uw app actief live

Als u wilt zien van uw app actief live in Azure wordt aangegeven, moet u deze opdracht uitvoert vanuit een andere map in uw bibliotheek:

    azure site browse

## <a name="make-updates-to-your-app"></a>Updates aanbrengen in uw app

U kunt nu cijfer gebruiken om vanuit de hoofdsite van uw project (bibliotheek) op elk gewenst moment een update aanbrengen in de persoonlijke site. U doen dit dezelfde manier als wanneer u de eerste keer uw code geïmplementeerd. Bijvoorbeeld, elke keer dat u wilt een nieuwe wijziging die u hebt getest lokaal push, net Voer de volgende opdrachten vanuit de hoofdsite van uw project (opslagplaats):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Volgende stappen

[Maken, configureert, en een Django web-app naar Azure in Visual Studio implementeren](web-sites-python-ptvs-django-mysql.md). Door deze zelfstudie te volgen, leert u de eenvoudige vaardigheden voor het uitvoeren van een WebApp Python in Azure wordt aangegeven, waaronder:

- Maak en implementeer een Python-app met een sjabloon.
- Stel Python versie.
- Virtuele omgevingen maken.
- Verbinding maken met een database.

Of Doe meer met uw eerste web-app. Bijvoorbeeld:

- [Andere manieren om te implementeren van uw code wilt Azure](../app-service-web/web-sites-deploy.md)uitproberen. Bijvoorbeeld als u wilt implementeren vanaf een van uw opslagplaatsen GitHub, selecteert u **GitHub** in plaats van **Lokale cijfer opslagplaats** in **distributieopties**.
- Duren voordat u uw Azure-app naar een hoger niveau. Uw gebruikers verifiëren. De schaal die het gebaseerd op aanvraag. Sommige prestatiewaarschuwingen instellen. Alle met een paar muisklikken. Zie [functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).

