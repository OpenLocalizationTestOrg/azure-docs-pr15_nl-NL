<properties
    pageTitle="Aan de slag met Mobile-Apps met behulp van Xamarin.Forms"
    description="Volg deze zelfstudie aan de slag met Azure Mobile-Apps voor Xamarin.Forms ontwikkeling"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Een Xamarin.Forms-app maken

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u een cloudgebaseerde backend-service toevoegt aan een mobiele app van Xamarin.Forms met een backend Azure Mobile-App. U maakt een nieuwe Mobile-App backend zowel een eenvoudige _takenlijst_ Xamarin.Forms app waarop app gegevens worden opgeslagen in Azure wordt aangegeven.

Voltooien van deze zelfstudie is vereist voor alle andere zelfstudies voor Mobile-Apps voor Xamarin.Forms.

##<a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

* Een actieve Azure-account. Als u geen account hebt, kunt u zich registreren voor een proefabonnement Azure en ontvang maximaal gratis 10 Mobile-Apps die u kunt zelfs nadat uw proefabonnement is afgelopen blijven gebruiken. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

* Visual Studio met Xamarin. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies. 

* Een Mac met Xcode v7.0 of hoger en Xamarin Studio Community is geïnstalleerd. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) en [-instelling, installeren, en controles voor Mac-gebruikers](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile), waar u direct een tijdelijk starter Mobile-App in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="create-a-new-azure-mobile-app-backend"></a>Een nieuwe backend van Azure Mobile-App maken

Volg deze stappen om een nieuwe Mobile-App backend maken.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


U hebt nu een Azure Mobile-App backend die kan worden gebruikt door uw mobiele clienttoepassingen ingericht. Vervolgens een serverproject voor eenvoudige "takenlijst" wordt gedownload backend en publiceren naar Azure.

## <a name="configure-the-server-project"></a>Configureer de serverproject

Volg de onderstaande stappen voor het configureren van het serverproject als u wilt de Node.js of .NET backend gebruiken.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Download en installeer de oplossing Xamarin.Forms

U hebt hier een paar andere opties. U kunt de oplossing naar een Mac downloaden en vervolgens opent in Xamarin Studio of u kunt downloaden van de oplossing op een Windows-computer en opent u deze in Visual Studio met een netwerk Mac voor het samenstellen van de iOS-app. Zie [installatie en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) als u meer gedetailleerde instructies voor de Xamarin setup-scenario's nodig hebt.

We gaan:

 1. Op uw Mac of op uw Windows-computer, opent u de [Azure-Portal] in een browservenster.
 2. Klik op het blad instellingen voor de Mobile-App, klik **Aan de slag** (onder mobiele) > **Xamarin.Forms**. Stap 3, klik op **een nieuwe app maken** als dit nog niet is geselecteerd.  Klik vervolgens op de knop **downloaden** .

    Dit is een project met een clienttoepassing die is verbonden met uw mobiele app gedownload. De gecomprimeerde projectbestand opslaan in uw lokale computer, en noteer waar u deze opslaat.

 3. Extraheren van het project dat u hebt gedownload en deze vervolgens in Xamarin Studio of Visual Studio openen.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Optioneel) Voer de iOS-project

In dit gedeelte is voor het uitvoeren van het project Xamarin iOS voor iOS-apparaten. U kunt dit gedeelte overslaan als u niet met iOS-apparaten werkt.

####<a name="in-xamarin-studio"></a>In Xamarin Studio

1. Met de rechtermuisknop op de iOS-project en klik vervolgens op **Instellen als opstartproject**.
2. Klik in het menu **uitvoeren** , klikt u op **Starten voor foutopsporing in** om te maken van het project en de app in de iPhone-emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Met de rechtermuisknop op de iOS-project en klik vervolgens op **instellen als opstartproject**.
2. Klik in het menu **maken** , klikt u op **Configuration Manager**.
3. Selecteer de selectievakjes **bouwen** en **implementeren** van het project iOS in het dialoogvenster **Configuration Manager** .
4. Druk op **F5 om te bouwen van het project en start de app in de iPhone-emulator** .

    >[AZURE.NOTE] Als u zich problemen voordoen tijdens het bouwen, ondersteuning NuGet uitvoeren package manager en bijwerken naar de nieuwste versie van de Xamarin pakketten. Soms mogelijk achter de projecten Quickstart vertraging in wordt bijgewerkt naar de meest recente.    

Typ in de app duidelijke tekst, zoals _Xamarin meer_ en klik op de **+** knop.

![][10]

Hiermee wordt een bericht-aanvraag verzonden naar de nieuwe mobiele app-end gehost in Azure wordt aangegeven. Gegevens uit het verzoek wordt ingevoegd in de tabel TodoItem. Items die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

>[AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw mobiele app backend in het TodoItemManager.cs C#-bestand van het project draagbare class bibliotheek van uw oplossing.

##<a name="optional-run-the-android-project"></a>(Optioneel) Het Android project uitvoeren

In dit gedeelte is voor het uitvoeren van het project Xamarin-droid voor Android. U kunt dit gedeelte overslaan als u niet met Android-apparaten werkt.

####<a name="in-xamarin-studio"></a>In Xamarin Studio

1. Met de rechtermuisknop op de Android project en klik vervolgens op **Instellen als opstartproject**.
2. Klik in het menu **uitvoeren** , klikt u op **Starten foutopsporing** om te maken van het project en de app in een Android emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Met de rechtermuisknop op het project Android (Droid) en klik vervolgens op **instellen als opstartproject**.
4. Klik in het menu **maken** , klikt u op **Configuration Manager**.
5. Selecteer de selectievakjes **bouwen** en **implementeren** van het Android project in het dialoogvenster **Configuration Manager** .
6. Druk op **F5 om te bouwen van het project en start de app in een Android emulator** .

    >[AZURE.NOTE] Als u zich problemen voordoen tijdens het bouwen, ondersteuning NuGet uitvoeren package manager en bijwerken naar de nieuwste versie van de Xamarin pakketten. Soms mogelijk achter de projecten Quickstart vertraging in wordt bijgewerkt naar de meest recente.    


Typ in de app duidelijke tekst, zoals _Xamarin meer_ en klik op de **+** knop.

![][11]

Hiermee wordt een bericht-aanvraag verzonden naar de nieuwe mobiele app-end gehost in Azure wordt aangegeven. Gegevens uit het verzoek wordt ingevoegd in de tabel TodoItem. Items die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

> [AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw mobiele app backend in het TodoItemManager.cs C#-bestand van het project draagbare class bibliotheek van uw oplossing.


##<a name="optional-run-the-windows-project"></a>(Optioneel) Het Windows-project uitvoeren


In dit gedeelte is voor het uitvoeren van het project Xamarin WinApp voor apparaten met Windows. U kunt dit gedeelte overslaan als u niet met Windows-apparaten werkt.


####<a name="in-visual-studio"></a>In Visual Studio
1. Met de rechtermuisknop op een van de Windows-projecten en klik vervolgens op **instellen als opstartproject**.
4. Klik in het menu **maken** , klikt u op **Configuration Manager**.
5. Selecteer de selectievakjes **bouwen** en **implementeren** van het Windows-project dat u hebt gekozen in het dialoogvenster **Configuration Manager** .
6. Druk op **F5 om te bouwen van het project en start de app in een Windows-emulator** .

    >[AZURE.NOTE] Als u zich problemen voordoen tijdens het bouwen, ondersteuning NuGet uitvoeren package manager en bijwerken naar de nieuwste versie van de Xamarin pakketten. Soms mogelijk achter de projecten Quickstart vertraging in wordt bijgewerkt naar de meest recente.    


Typ in de app duidelijke tekst, zoals _Xamarin meer_ en klik op de **+** knop.

Hiermee wordt een bericht-aanvraag verzonden naar de nieuwe mobiele app-end gehost in Azure wordt aangegeven. Gegevens uit het verzoek wordt ingevoegd in de tabel TodoItem. Items die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

![][12]

> [AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw mobiele app backend in het TodoItemManager.cs C#-bestand van het project draagbare class bibliotheek van uw oplossing.

##<a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
Leer hoe u gebruikers van uw app met een identiteitsprovider verifiëren.

* [Push-meldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)  
Informatie over het toevoegen van push-meldingen-ondersteuning naar uw app en uw backend Mobile-App als u wilt gebruiken van Azure melding Hubs push-meldingen configureren.

* [Offline synchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.

* [Het gebruik van de beheerde-client voor Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Informatie over het werken met de beheerde client SDK in uw app Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/

