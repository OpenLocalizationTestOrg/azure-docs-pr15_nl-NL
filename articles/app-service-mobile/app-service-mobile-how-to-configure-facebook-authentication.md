<properties
    pageTitle="Hoe u de Facebook-verificatie voor uw App Services-toepassing configureren"
    description="Leer hoe u Facebook verificatie configureren voor uw App Services-toepassing."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Het configureren van uw App-servicetoepassing Facebook login gebruiken

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe u het configureren van Azure App Service Facebook gebruik als een verificatieprovider.

Als u wilt de procedure in dit onderwerp hebt voltooid, moet u een Facebook-account waarvoor een gecontroleerde e-mailadres en een mobiel telefoonnummer hebben. Als u wilt een nieuwe Facebook-account hebt gemaakt, gaat u naar [facebook.com].

## <a name="register"> </a>Uw toepassing registreren met Facebook

1. Meld u aan bij de [Azure-portal]en Ga in uw toepassing. Kopieer de **URL**. U kunt dit wilt gebruiken voor het configureren van uw Facebook-app.

2. In een ander browservenster, Ga naar de website van de [Facebook-ontwikkelaars] en aanmelden met de referenties van uw Facebook-account.

3. (Optioneel) Als u hebt niet geregistreerd, klikt u op **Apps** > **registreren als een ontwikkelaar**hebt, klikt u vervolgens het beleid te accepteren en volg de stappen registratie.

4. Klik op **Mijn Apps** > **toevoegen van een nieuwe App** > **Website** > **overslaan en App-ID maken**. 

5. In het vak **Weergavenaam**, typ een unieke naam voor de app, typt u uw **E-mail van de contactpersoon**, kies een **categorie** voor de app, en vervolgens klikt u op **App-ID maken** en voert u het selectievakje beveiliging. Hiermee gaat u naar het dashboard voor ontwikkelaars voor de nieuwe Facebook-app.

6. Klik onder 'Facebook Login', klikt u op **Aan de slag**. Van uw toepassing **Omleiden URI** **geldige OAuth**doorsturen URI's toevoegen en klik op **Wijzigingen opslaan**. 

    > [AZURE.NOTE] Uw redirect URI is de URL van uw toepassing toegevoegd door het pad, _/.auth/login/facebook/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Zorg ervoor dat u het schema HTTPS worden gebruikt.

6. Klik op **Instellingen**in het linkernavigatievenster. Klik op het veld **App geheim** te klikken op **voorstelling**, Geef uw wachtwoord als aangevraagd, noteert u de waarden van de **App-ID** en **App geheim**. U deze later gebruiken voor het configureren van uw toepassing in Azure wordt aangegeven.

    > [AZURE.IMPORTANT] Het app-geheim is een belangrijk beveiliging. In dit geheim delen met iedereen of verspreidt binnen een clienttoepassing niet.

7. De Facebook-account dat is gebruikt om de toepassing te registreren is een beheerder van de app. Op dit moment alleen beheerders kunnen Meld u aan bij deze toepassing. Als u wilt verifiëren op andere Facebook-accounts, klikt u op **App controleren** en inschakelen van **openbaar maken < uw-app-naam >** algemene openbare toegang met Facebook-verificatie in te schakelen.

## <a name="secrets"> </a>Informatie Facebook toevoegen aan uw toepassing

1. Ga in uw toepassing terug in de [portal van Azure]. Klik op **Instellingen** > **verificatie / autorisatie**, en zorg ervoor dat de **Verificatie van de App-Service** **ingeschakeld is**.

2. Klik op **Facebook**, plak in de App-ID en App geheim waarden die u eerder hebt aangeschaft, (optioneel) een bereiken die nodig zijn voor uw toepassing inschakelen en klik op **OK**.

    ![][0]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

3. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen gebruikers die door Facebook, stelt u de **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** op **Facebook**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar Facebook voor verificatie.

4. Wanneer u klaar configureren verificatie, klikt u op **Opslaan**.

U bent nu klaar voor gebruik van Facebook voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portal]: https://portal.azure.com/
