<properties
    pageTitle="Een universele Windows Platform (UWP) die wordt gebruikt in de Mobile-Apps maken | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met het gebruik van de mobiele app van Azure backends voor het ontwikkelen van apps universele Windows-Platform (UWP) in C#, Visual Basic of JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Een Windows-app maken

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u een cloudgebaseerde backend-service toevoegt aan een app universele Windows-Platform (UWP). Zie [Wat zijn Mobile-Apps](app-service-mobile-value-prop.md)voor meer informatie. Hier volgen volgende schermopnamen vanuit de voltooide app:

![Voltooide bureaublad-app](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Klik op een bureaublad wordt uitgevoerd. 

![Voltooide telefoon-app](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Uitgevoerd op een telefoon

Voltooien van deze zelfstudie is vereist voor alle andere zelfstudies voor Mobile-App voor UWP-apps. 

##<a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

* Een actieve Azure-account. Als u geen account hebt, kunt u zich registreren voor een evaluatieversie van Azure en ophalen van maximaal 10 gratis mobiele apps die u kunt zelfs nadat uw proefabonnement is afgelopen blijven gebruiken. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie.

* [Visual Studio-Community-2015] of een latere versie.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). Hiervoor kunt u direct maken een mobiele app van tijdelijk starter in App Service, geen creditcard vereist en geen afspraken.

##<a name="create-a-new-azure-mobile-app-backend"></a>Een nieuwe backend van Azure Mobile-App maken

Volg deze stappen om een nieuwe Mobile-App backend maken.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een Azure Mobile-App backend die kan worden gebruikt door uw mobiele clienttoepassingen ingericht. Vervolgens een serverproject voor eenvoudige "takenlijst" wordt gedownload backend en publiceren naar Azure.

## <a name="configure-the-server-project"></a>Configureer de serverproject

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Download en installeer het client-project

Wanneer u uw backend Mobile-App hebt geconfigureerd, kunt u een nieuwe client-app maakt of een bestaande app verbinding maken met Azure wijzigen. In deze sectie, kunt u een UWP app sjabloonproject dat is aangepast als u wilt verbinden met uw backend Mobile-App downloaden.

1. Klik in het blad **snel aan de slag** voor uw backend Mobile-App, op **een nieuwe app maken op** > **downloaden**en klik uitpakken de gecomprimeerde project-bestanden naar uw lokale computer.

    ![Windows quickstart project downloaden](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Optioneel) Het project UWP-app toevoegen aan dezelfde oplossing als het serverproject. Dit eenvoudiger fouten opsporen in en test de app en de backend in dezelfde oplossing Visual Studio, als u wilt doen. Als u wilt een UWP app-project toevoegen aan de oplossing, moet u Visual Studio-2015 of een latere versie gebruiken.

4. Druk op F5 om te implementeren en uitvoeren van de app met de app UWP als het opstartproject.

5. Duidelijke tekst, zoals *de zelfstudie voltooid*, typ in het tekstvak **een TodoItem invoegen** in de app en klik vervolgens op **Opslaan**.

    ![Volledige bureaublad van Windows quickstart](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Hiermee wordt een POST-aanvraag verzonden naar de nieuwe mobiele app-end die wordt gehost in Azure wordt aangegeven.

6. (Optioneel) De app stoppen en opnieuw op een ander apparaat of mobiele emulator.

    ![Windows quickstart volledige telefoonnummer](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Zoals u ziet dat er gegevens uit de vorige stap hebt opgeslagen van Azure is geladen nadat de app UWP is gestart. 

##<a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Leer hoe u gebruikers van uw app met een identiteitsprovider verifiëren.

* [Push-meldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen-ondersteuning naar uw app en uw backend Mobile-App als u wilt gebruiken van Azure melding Hubs push-meldingen configureren.

* [Offline synchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio-Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
