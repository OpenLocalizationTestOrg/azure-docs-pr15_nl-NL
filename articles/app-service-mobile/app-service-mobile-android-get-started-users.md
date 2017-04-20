<properties
    pageTitle="Het toevoegen van verificatie op android-apparaat met Mobile-Apps | Azure App-Service"
    description="Informatie over het gebruik van de Mobile-Apps in Azure App-Service om gebruikers van uw Android-app via allerlei identiteitsprovider, inclusief Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Verificatie toevoegen aan uw Android-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Overzicht

In deze zelfstudie kunt u verificatie toevoegen aan de takenlijst van quickstart project op een Android met behulp van een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [aan de slag met Mobile-Apps] , waarin u eerst moet uitvoeren.

##<a name="register"></a>Registreren van uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ In Android Studio, opent u het project de verwachte u uitgevoerd met de zelfstudie [aan de slag met Mobile-Apps]. Klik op **app uitvoeren** vanuit het menu **uitvoeren** en controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart.

     Deze uitzondering gebeurt omdat de app probeert te krijgen tot de backend als niet-geverifieerde gebruiker, maar de tabel _TodoItem_ nu is verificatie vereist.

Vervolgens kunt u de app om gebruikers te verifiëren voordat de resources aanvragen van de Mobile-App-end bijwerken.

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Cache-verificatietokens op de client

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Volgende stappen

Nu u deze zelfstudie basisverificatie hebt uitgevoerd, kunt u doorgaat op een van de volgende zelfstudies:

+ [Push-meldingen toevoegen aan uw Android-app](app-service-mobile-android-get-started-push.md) Leer hoe u uw backend Mobile-App als u wilt gebruiken van Azure melding Hubs push-meldingen configureren.

+ [Inschakelen van offlinesynchronisatie voor uw Android-app](app-service-mobile-android-get-started-offline-data.md) Informatie over het toevoegen van Offlineondersteuning uw app met een backend Mobile-App. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Aan de slag met Mobile-Apps]: app-service-mobile-android-get-started.md
