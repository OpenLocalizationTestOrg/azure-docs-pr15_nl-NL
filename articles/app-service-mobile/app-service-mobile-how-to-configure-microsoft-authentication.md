<properties
    pageTitle="Hoe u de verificatie van Microsoft-Account voor uw App Services-toepassing configureren"
    description="Informatie over het Microsoft-Account verificatie configureren voor uw App Services-toepassing."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Het configureren van uw App-servicetoepassing gebruik van Microsoft-Account aanmelden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe u het configureren van Azure App Service gebruik van Microsoft-Account als een verificatieprovider. 

## <a name="register-microsoft-account"> </a>Registreren van uw app bij Microsoft-Account

1. Meld u aan bij de [Azure-portal]en Ga in uw toepassing. Kopieer de **URL**, waarmee u kunt later uw app configureren met Microsoft-Account.

2. Navigeer naar de pagina [Mijn toepassingen] in het Microsoft-Account Developer Center en meld u aan met uw Microsoft-account, indien nodig.

3. Klik op **een app toevoegen**, en vervolgens typt u de toepassingsnaam van een en klik op **de toepassing maken**.

4. Maak een notitie van de **Toepassings-ID**, zoals u deze later moet. 

5. Onder 'Platforms', klikt u op **Platform toevoegen** en selecteer "Web".

6. Klik onder "URI's omleiden" opgeven van het eindpunt voor uw toepassing, klik vervolgens op **Opslaan**. 
 
    >[AZURE.NOTE]Uw redirect URI is de URL van uw toepassing toegevoegd door het pad, _/.auth/login/microsoftaccount/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Zorg ervoor dat u het schema HTTPS worden gebruikt.

7. Klik in het vak "Toepassing geheimen," **Nieuw wachtwoord genereren**. Noteer de waarde die wordt weergegeven. Nadat u de pagina verlaat, wordt deze niet opnieuw worden weergegeven.


    > [AZURE.IMPORTANT] Het wachtwoord is een belangrijk beveiliging. Het wachtwoord delen met iedereen of verspreidt binnen een clienttoepassing niet.

## <a name="secrets"> </a>Informatie van de Microsoft-Account toevoegen aan uw App Service-toepassing

1. Terug in de [Azure-portal], gaat u naar uw toepassing, klik op **Instellingen** > **verificatie / autorisatie**.

2. Als de verificatie / autorisatie functie niet is ingeschakeld, **Schakel deze**.

3. Klik op **Microsoft-Account**. Plak in de toepassing-ID en wachtwoord waarden die u eerder hebt aangeschaft en inschakelen (optioneel) een bereiken die uw toepassing is vereist. Klik vervolgens op **OK**.

    ![][1]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

4. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen de gebruikers die zijn geverifieerd door Microsoft-account, stel **actie moet worden uitgevoerd wanneer de aanvraag is niet geverifieerd** bij **Microsoft-Account**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar de Microsoft-account voor verificatie.

5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Microsoft-Account voor verificatie in uw app.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Mijn toepassingen]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portal]: https://portal.azure.com/
