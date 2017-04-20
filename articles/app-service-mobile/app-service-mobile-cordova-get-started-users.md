<properties
    pageTitle="Het toevoegen van verificatie op Apache Cordova met Mobile-Apps | Azure App-Service"
    description="Informatie over het gebruik van de Mobile-Apps in Azure App-Service om gebruikers van uw app Apache Cordova via allerlei identiteitsprovider, inclusief Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Verificatie toevoegen aan uw Apache Cordova-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Overzicht

In deze zelfstudie kunt u verificatie toevoegen aan de takenlijst van quickstart project op Apache Cordova met een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [aan de slag met Mobile-Apps] , waarin u eerst moet uitvoeren.

##<a name="register"></a>Registreren van uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Bekijk een video met dezelfde stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

U kunt nu verifiëren dat anonieme toegang tot uw back-end is uitgeschakeld. Visual Studio, open het project dat u hebt gemaakt wanneer u de zelfstudie [aan de slag met Mobile-Apps], voltooid en vervolgens uw toepassing uitvoeren in de **Google Android Emulator** en controleer of dat een onverwachte verbindingsfout wordt weergegeven nadat de app is gestart.

Vervolgens wordt u de app om gebruikers te verifiëren voordat de resources aanvragen van de Mobile-App-end bijwerken.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app

1. Uw project openen in **Visual Studio**en opent u de `www/index.html` bestand om te bewerken.

2. Zoek de `Content-Security-Policy` metatag in de hoofdsectie.  U moet het OAuth-host aan de lijst met toegestane bronnen toevoegen.

  	| Provider               | Naam van de Provider SDK | OAuth-Host                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | AAD               | https://login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://login.live.com      |
  	| Twitter                | Twitter           | https://API.Twitter.com     |

    Een voorbeeld van inhoud beveiligingsbeleid van (geïmplementeerd voor Azure Active Directory) is als volgt:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    U moet vervangen `https://login.windows.net` OAuth host uit de bovenstaande tabel.  Raadpleeg de [inhoud beveiligingsbeleid documentatie] voor meer informatie over deze metatag.

    Houd er rekening mee dat sommige verificatieproviders is niet dat inhoud beveiligingsbeleid verandert vereist als gebruikt op mobiele apparaten, passende.  Geen wijzigingen inhoud beveiligingsbeleid zijn bijvoorbeeld vereist als u Google-verificatie op een Android-apparaat.

3. Open de `www/js/index.js` voor het bewerken van bestanden, gaat u naar de `onDeviceReady()` methode, en klik onder het maken van de client code toevoegen de volgende handelingen uit:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Opmerking dat deze code vervangt de bestaande code die wordt gemaakt van de tabelverwijzing en de gebruikersinterface vernieuwt.

    De methode login() begint verificatie met de provider. De methode login() is een functie asynchrone die resulteert in een JavaScript-toegezegd.  In het antwoord toegezegd wordt de rest van de initialisatie geplaatst, zodat deze wordt niet uitgevoerd totdat de methode login() is voltooid.

4. Vervang in de code die u zojuist hebt toegevoegd, `SDK_Provider_Name` met de naam van uw provider login. Bijvoorbeeld voor Azure Active Directory, gebruikt u `client.login('aad')`.

4. Uw project uitvoeren.  Wanneer het project is geïnitialiseerd, wordt uw toepassing het OAuth-aanmeldingspagina weergegeven voor de door u gekozen verificatieprovider.

##<a name="next-steps"></a>Volgende stappen

* Lees meer [Over verificatie] met Azure App-Service.
* De zelfstudie u verder met het toevoegen van [Push-meldingen] aan uw Apache Cordova-app.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET-Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Aan de slag met Mobile-Apps]: app-service-mobile-cordova-get-started.md
[Inhoud beveiligingsbeleid documentatie]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-meldingen]: app-service-mobile-cordova-get-started-push.md
[Over verificatie]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET-Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
