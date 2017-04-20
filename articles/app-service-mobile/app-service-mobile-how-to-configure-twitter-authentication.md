<properties
    pageTitle="Hoe u Twitter verificatie configureren voor uw App Services-toepassing"
    description="Leer hoe u Twitter verificatie configureren voor uw App Services-toepassing."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Het configureren van uw App-servicetoepassing Twitter login gebruiken

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe u het configureren van Azure App Service Twitter gebruik als een verificatieprovider.

Als u wilt de procedure in dit onderwerp hebt voltooid, moet u een Twitter-account met een gecontroleerde e-adres en telefoonnummer getal hebben. Om een nieuwe Twitter-account maken, gaat u naar <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Uw toepassing met Twitter registreren


1. Meld u aan bij de [Azure-portal]en Ga in uw toepassing. Kopieer de **URL**. U kunt dit wilt gebruiken voor het configureren van uw Twitter-app.

2. Navigeer naar de website [Twitter-ontwikkelaars] , meld u aan met de referenties van uw Twitter-account en klik op **Nieuwe App maken**.

3. Typ de **naam** en een **Beschrijving** voor de nieuwe app. In de **URL** voor de waarde van de **Website** van uw toepassing te plakken. Plak vervolgens voor de **Terugbellen URL**de **URL voor terugbellen** die u eerder hebt gekopieerd. Dit is de Mobile-App gateway met het pad, _/.auth/login/twitter/callback_toegevoegd. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Zorg ervoor dat u het schema HTTPS worden gebruikt.

3.  Onderaan op de pagina, lees en accepteer de gebruiksrechtovereenkomst. Klik vervolgens op **uw Twitter-toepassing maken**. Hiermee wordt de app wordt weergegeven de Toepassingdetails van de.

4. Klik op het tabblad **Instellingen** , **toe dat deze toepassing moet worden gebruikt aan te melden met Twitter**, klik op **Instellingen voor updates**.

5. Selecteer het tabblad **toetsen en Access Tokens** . Noteer de waarden van **Consumenten toets (API)** en **consumentgeheim (API geheim)**.

    > [AZURE.NOTE] Het consumentgeheim is een belangrijk beveiliging. In dit geheim delen met iedereen of verspreidt met uw app niet.


## <a name="secrets"> </a>Informatie Twitter-toevoegen aan uw toepassing

13. Ga in uw toepassing terug in de [portal van Azure]. Klik op **Instellingen**, en kiest u vervolgens **verificatie / autorisatie**.

14. Als de verificatie / autorisatie functie niet is ingeschakeld, schakelt u het overschakelen naar **op**.

15. Klik op **Twitter**. Plak in het App-ID en de App geheim waarden die u eerder hebt verkregen. Klik vervolgens op **OK**.

    ![][1]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

17. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen de gebruikers die zijn geverifieerd door Twitter, stelt u **de actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** op **Twitter**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar Twitter voor verificatie.

17. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Twitter voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
