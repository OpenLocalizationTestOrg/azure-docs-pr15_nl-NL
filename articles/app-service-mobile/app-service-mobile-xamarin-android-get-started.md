<properties
    pageTitle="Aan de slag met Azure mobiele Apps voor Xamarin.Android-apps"
    description="Volg deze zelfstudie aan de slag met Azure Mobile-Apps voor Xamarin Android ontwikkeling"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Een Xamarin.Android-App maken

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u een cloudgebaseerde backend-service toevoegt aan een Xamarin.Android-app. Zie [Wat zijn Mobile-Apps](app-service-mobile-value-prop.md)voor meer informatie.

Schermafbeelding van de voltooide app is hieronder:

![][0]

Voltooien van deze zelfstudie is vereist voor alle andere zelfstudies voor Mobile-Apps voor Xamarin.Android-apps.

##<a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een actieve Azure-account. Als u geen account hebt, registreren voor een proefabonnement Azure en ophalen van maximaal 10 gratis Mobile-Apps. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

* Visual Studio met Xamarin. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies.

>[AZURE.NOTE]Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar [De App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile).  U kunt direct een tijdelijk starter Mobile-App maken in de App-Service. Geen creditcards vereist; geen verplichtingen.

## <a name="create-an-azure-mobile-app-backend"></a>Een backend Azure Mobile-App maken

Volg deze stappen om een backend Mobile-App maken.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een Azure Mobile-App backend die kan worden gebruikt door uw mobiele clienttoepassingen ingericht. Download vervolgens een serverproject voor eenvoudige 'takenlijst' backend en publiceren naar Azure.

## <a name="configure-the-server-project"></a>Configureer de serverproject

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Download en installeer de app Xamarin.Android

1. Klik onder **downloaden en uitvoeren van uw project Xamarin.Android**, op de knop **downloaden** .

    De gecomprimeerde projectbestand opslaan in uw lokale computer, en noteer waar u deze opslaat.

2. Druk op **F5 om te bouwen van het project en start de app** .

3. Typ duidelijke tekst, zoals _de zelfstudie voltooid_ in de app en klik op de knop **toevoegen** .

    ![][10]

    Gegevens uit het verzoek wordt ingevoegd in de tabel TodoItem. Items die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

    > [AZURE.NOTE] U kunt de code die toegang heeft tot uw backend mobiele app voor het opvragen en het invoegen van gegevens, u in het ToDoActivity.cs C#-bestand vindt bekijken.

##<a name="next-steps"></a>Volgende stappen

* [Offline synchroniseren naar uw app toevoegen](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-android-get-started-users.md)
* [Push-meldingen toevoegen aan uw Xamarin.Android-app](app-service-mobile-xamarin-android-get-started-push.md)
* [Het gebruik van de beheerde-client voor Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
