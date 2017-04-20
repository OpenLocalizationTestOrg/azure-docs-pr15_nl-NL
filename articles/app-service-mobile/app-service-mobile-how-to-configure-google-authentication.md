<properties
    pageTitle="Hoe u Google verificatie configureren voor uw App Services-toepassing"
    description="Leer hoe u Google verificatie configureren voor uw App Services-toepassing."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Het configureren van uw App-servicetoepassing Google login gebruiken

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe u het configureren van Azure App Service Google gebruik als een verificatieprovider.

Als u wilt de procedure in dit onderwerp hebt voltooid, moet u een Google-account waarvoor een gecontroleerde e-mailadres hebben. Als u wilt een nieuwe Google-account hebt gemaakt, gaat u naar [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Uw toepassing met Google registreren

1. Meld u aan bij de [Azure-portal]en Ga in uw toepassing. Kopieer de **URL**die u later gebruiken voor het configureren van uw Google-app.

2. Navigeer naar de website van [Google-API's](http://go.microsoft.com/fwlink/p/?LinkId=268303) , meld u aan met de referenties van uw Google-account, klik op **Project maken**, Geef een **naam van het Project**en klik op **maken**.

3. Klik onder **Sociale API's** op **Google + API** en klik vervolgens op **inschakelen**.

4. Klik in het linker navigatiegedeelte op **referenties** > **OAuth toestemming scherm**, selecteer uw **e-mailadres**, voer een **Productnaam**en klik op **Opslaan**.

5. Klik in het tabblad **referenties** op **referenties maken** > **OAuth cliënt-ID**en klik op Selecteer **webtoepassing**.

6. Plak de App Service **URL** die u eerder hebt gekopieerd naar **Geautoriseerd JavaScript oorsprong**en plak uw redirect URI in **Omleiden URI geautoriseerd**. De omleiding URI is de URL van uw toepassing toegevoegd door het pad, _/.auth/login/google/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/google/callback`. Zorg ervoor dat u het schema HTTPS worden gebruikt. Klik vervolgens op **maken**.

7. Maak een notitie van de waarden van de client-ID en geheim client op het volgende scherm.


    > [AZURE.IMPORTANT]
    De geheim client is een belangrijk beveiliging. In dit geheim delen met iedereen of verspreidt binnen een clienttoepassing niet.


## <a name="secrets"> </a>Informatie Google toevoegen aan uw toepassing

8. Ga in uw toepassing terug in de [portal van Azure]. Klik op **Instellingen**, en kiest u vervolgens **verificatie / autorisatie**.

9. Als de verificatie / autorisatie functie niet is ingeschakeld, schakelt u het overschakelen naar **op**.

10. Klik op **Google**. Plak in de App-ID en App geheim waarden die u eerder hebt aangeschaft en inschakelen (optioneel) een bereiken die uw toepassing is vereist. Klik vervolgens op **OK**.

    ![][1]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

17. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen de gebruikers die zijn geverifieerd door Google, stelt u de **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** op **Google**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar Google voor verificatie.

12. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Google voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portal]: https://portal.azure.com/

