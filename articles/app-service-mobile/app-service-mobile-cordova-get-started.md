<properties
    pageTitle="Een Cordova-app maken op de mobiele Apps Azure App Service | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met het gebruik van de mobiele app van Azure backends voor Apache Cordova ontwikkeling"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, mobiele, javascript-client" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Maken van een Apache Cordova-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u een cloudgebaseerde backend-service toevoegt aan een mobiele app van Apache Cordova met behulp van een mobiele app van Azure backend.  U maakt een nieuwe mobiele app backend zowel een eenvoudige _takenlijst_ Apache Cordova app waarop app gegevens worden opgeslagen in Azure wordt aangegeven.

Voltooien van deze zelfstudie is vereist voor alle andere Apache Cordova zelfstudies over het gebruik van de functie Mobile-Apps in Azure App-Service.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

* Een PC met [Visual Studio-Community-2015] of hoger.
* [Visual Studio Tools for Apache Cordova].
* Een [actieve Azure-account](https://azure.microsoft.com/pricing/free-trial/).

U kunt ook Visual Studio overslaan en de opdrachtregel Apache Cordova rechtstreeks gebruiken.  Dit is handig wanneer u de zelfstudie op een Mac-computer.  Compileren Apache Cordova clienttoepassingen via de opdrachtregel valt buiten door deze zelfstudie.

## <a name="create-a-new-azure-mobile-app-backend"></a>Een nieuwe Azure mobiele app backend maken

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Bekijk een video met dezelfde stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Configureer de serverproject

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Download en installeer de app Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Volgende stappen

Nu dat u deze snel aan de slag-zelfstudie hebt uitgevoerd, gaat u naar een van de volgende zelfstudies:

* [Verificatie toevoegen] aan uw Apache Cordova-app.
* [Push-meldingen toevoegen] aan uw Apache Cordova-app.

Meer informatie over belangrijke concepten met Azure App-Service.

* [Verificatie]
* [Push-meldingen]

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET-Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio-Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Verificatie toevoegen]: app-service-mobile-cordova-get-started-users.md
[Push-meldingen toevoegen]: app-service-mobile-cordova-get-started-push.md
[Verificatie]: app-service-mobile-auth.md
[Push-meldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
