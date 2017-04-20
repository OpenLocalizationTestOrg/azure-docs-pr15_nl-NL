<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor Windows Phone Silverlight-apps"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en push-meldingen voor Windows Phone Silverlight-apps."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Aan de slag met Azure Mobile betrokkenheid voor Windows Phone Silverlight-apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u het gebruiken van Azure Mobile betrokkenheid om meer informatie over het appgebruik van uw en push-meldingen verzenden naar gesegmenteerde gebruikers van een Windows Phone Silverlight-toepassing.
Deze zelfstudie ziet u de eenvoudige broadcast scenario Mobile betrokkenheid gebruiken. In deze maakt u een lege app voor Windows Phone Silverlight die worden verzameld basisgegevens en met Microsoft Push Notification-Service (MPNS) push-meldingen ontvangt.

> [AZURE.NOTE] Als u Windows Phone 8.1 (niet-Silverlight) hebt samengesteld, raadpleegt u de [Windows universele zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md).

Deze zelfstudie is het volgende nodig:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget pakket

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started)voor meer informatie.

##<a id="setup-azme"></a>Instellen van mobiele betrokkenheid voor uw Windows Phone-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie", dat wil de minimale set vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. De volledige integratie documentatie vindt u in de [Mobile betrokkenheid Windows Phone SDK-integratie](mobile-engagement-windows-phone-sdk-overview.md)

We gaan een eenvoudige app maken met Visual Studio om te laten zien van de integratie.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Een nieuw project in de Windows Phone Silverlight maken

De volgende stappen wordt ervan uitgegaan dat het gebruik van Visual Studio-2015 Hoewel de stappen identiek in eerdere versies van Visual Studio zijn. 

1. Visual Studio Start en selecteer in **het beginscherm** **Nieuw Project**.

2. Selecteer in het pop-upvenster **Windows 8** -> **Windows Phone** -> **Lege App (Windows Phone Silverlight)**. Geef de app **naam** en **de naam van de oplossing**en klik vervolgens op **OK**.

    ![][1]

3. U kunt afstemmen op **Windows Phone 8.0** of **Windows Phone 8.1**.

U hebt nu een nieuwe Windows Phone Silverlight-toepassing waarin de SDK van Azure Mobile betrokkenheid integreert gemaakt.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

1. Installeer het [MicrosoftAzure.MobileEngagement] nuget-pakket in uw project.

2. Open `WMAppManifest.xml` (onder de map Eigenschappen) en zorg ervoor dat de volgende zijn gedeclareerd (ze toevoegt als deze geen deel uitmaakt) in de `<Capabilities />` tag:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Nu de verbindingsreeks die u eerder hebt gekopieerd voor uw app Mobile betrokkenheid plakken en plak deze in de `Resources\EngagementConfiguration.xml` bestand, tussen de `<connectionString>` en `</connectionString>` labels:

    ![][3]

4. In de `App.xaml.cs` bestand:

    een. Toevoegen de `using` instructie:

            using Microsoft.Azure.Engagement;

    b. Initialisatie van de SDK in de `Application_Launching` methode:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Invoegen van de volgende handelingen uit in de `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Realtime controle inschakelen

Om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actieve, moet u ten minste één scherm (activiteit) op de backend Mobile betrokkenheid verzenden.

1. Voeg in het MainPage.xaml.cs, de `using` instructie:

        using Microsoft.Azure.Engagement;

2. Vervang de klasse base van **MainPage**, dat wil voordat **PhoneApplicationPage zeggen**, door **EngagementPage**.

        class MainPage : EngagementPage 
    
3. In uw `MainPage.xml` bestand:

    een. Toevoegen aan de declaraties naamruimten:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Vervang `phone:PhoneApplicationPage` in de naam van de XML-code met `engagement:EngagementPage`.

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u kunt werken en uw gebruikers met Push-meldingen en Messaging in-app in de context van campagnes hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties uw app instellen voor het ontvangen van deze.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Uw app MPNS Push-meldingen wilt ontvangen inschakelen

Nieuwe mogelijkheden aan toevoegen uw `WMAppManifest.xml` bestand:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Initialisatie van de SDK bereik

1. In `App.xaml.cs`, bellen `EngagementReach.Instance.Init();` rechts in de functie **Application_Launching** na de initialisatie agent:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. In `App.xaml.cs`, bellen `EngagementReach.Instance.OnActivated(e);` rechts in de functie **Application_Activated** na de initialisatie agent:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

U bent klaar. Nu verifiëren we dat u correct hebt riep uit deze eenvoudige integratie.

##<a id="send"></a>Een bericht verzenden naar uw app

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

U ziet nu een melding die wordt weergegeven op het apparaat als een melding in-app als de app is geopend anders als een mailpop-upmelding als volgt uit: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
