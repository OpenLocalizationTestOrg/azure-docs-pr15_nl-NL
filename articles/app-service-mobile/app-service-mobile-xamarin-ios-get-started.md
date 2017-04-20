<properties
    pageTitle="Aan de slag met Azure App Service Mobile-Apps voor Xamarin.iOS apps | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met gebruik van de Mobile-Apps voor de ontwikkeling van Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Een Xamarin.iOS-app maken

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u een cloudgebaseerde backend-service toevoegt aan een mobiele app van Xamarin.iOS met behulp van een mobiele app van Azure backend.  U maken een nieuwe mobiele app backend zowel een eenvoudige _takenlijst_ Xamarin.iOS-app die app gegevens worden opgeslagen in Azure.

Voltooien van deze zelfstudie is vereist voor alle andere Xamarin.iOS zelfstudies over het gebruik van de functie Mobile-Apps in Azure App-Service.

##<a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een actieve Azure-account. Als u geen account hebt, registreren voor een evaluatieversie van Azure en ophalen van maximaal 10 gratis mobiele apps die u kunt zelfs nadat uw proefabonnement is afgelopen blijven gebruiken. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

* Visual Studio met Xamarin. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies.

* Een Mac met Xcode v7.0 of hoger en Xamarin Studio Community is geïnstalleerd. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) en [-instelling, installeren, en controles voor Mac-gebruikers](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). U kunt een mobiele app van tijdelijk starter direct in App Service maken, geen creditcard vereist en geen afspraken.

## <a name="create-an-azure-mobile-app-backend"></a>Een backend Azure Mobile-App maken

Volg deze stappen om een backend Mobile-App maken.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Configureer de serverproject

U hebt nu een Azure Mobile-App backend die kan worden gebruikt door uw mobiele clienttoepassingen ingericht. Download vervolgens een serverproject voor eenvoudige 'takenlijst' backend en publiceren naar Azure.

De volgende stappen om te configureren van het serverproject als u wilt de Node.js of .NET backend gebruiken.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Download en installeer de app Xamarin.iOS

1. Open de [portal van Azure] in een browservenster.

2. Klik op het blad instellingen voor de Mobile-App, klikt u op **Aan de slag** > **Xamarin.iOS**. Stap 3, klik op **een nieuwe app maken** als dit nog niet is geselecteerd.  Klik vervolgens op de knop **downloaden** .

    Een clienttoepassing die is verbonden met uw mobiele backend is gedownload. De gecomprimeerde projectbestand opslaan in uw lokale computer, en noteer waar u deze opslaat.

3. Extraheren van het project dat u hebt gedownload en deze vervolgens in Xamarin Studio (of Visual Studio) openen.

    ![][9]

    ![][8]

4. Druk op F5 om te bouwen van het project en start de app in de iPhone-emulator.

5. Typ in de app duidelijke tekst, zoals _Meer Xamarin_, en klik op de **+** knop.

    ![][10]

    Gegevens uit het verzoek wordt ingevoegd in de tabel TodoItem. Items die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

>[AZURE.NOTE]U kunt de code die toegang heeft tot uw backend mobiele app voor het opvragen en voeg de gegevens in de QSTodoService.cs C#-bestand bekijken.

##<a name="next-steps"></a>Volgende stappen

* [Offline synchroniseren naar uw app toevoegen](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-ios-get-started-users.md)
* [Push-meldingen toevoegen aan uw Xamarin.Android-app](app-service-mobile-xamarin-ios-get-started-push.md)
* [Het gebruik van de beheerde-client voor Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com/
