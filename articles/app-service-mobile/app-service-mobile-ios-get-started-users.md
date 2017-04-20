<properties
    pageTitle="Verificatie toevoegen op iOS met Azure Mobile-Apps"
    description="Leer hoe u Azure Mobile-Apps gebruiken om gebruikers van uw iOS-app via allerlei identiteitsprovider, inclusief AAD, Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Verificatie toevoegen aan uw iOS-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In deze zelfstudie kunt u verificatie toevoegen aan het project [iOS snel starten] met een ondersteunde identiteitsprovider. Deze zelfstudie is gebaseerd op de zelfstudie [iOS snel starten] , dat u eerst moet uitvoeren.

##<a name="register"></a>Registreren van uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

In Xcode, drukt u op **uitvoeren** de app te starten. Een uitzondering wordt worden verhoogd omdat de app probeert te krijgen tot de backend als niet-geverifieerde gebruiker, maar _TodoItem_ tabel nu is verificatie vereist.

##<a name="add-authentication"></a>Verificatie toevoegen aan app

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS snel aan de slag]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
