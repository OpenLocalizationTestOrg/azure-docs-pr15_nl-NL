<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor eenheid iOS-implementatie"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor eenheid apps implementeren naar iOS-apparaten."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Aan de slag met Azure Mobile betrokkenheid voor eenheid iOS-implementatie

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u het gebruik van Azure Mobile betrokkenheid voor meer informatie over het appgebruik van uw en het verzenden van push-meldingen aan gebruikers van een toepassing voor de eenheid gesegmenteerd wanneer u de implementatie in een iOS-apparaat.
Deze zelfstudie gebruikt de klassieke eenheid implementeren een bal zelfstudie als het beginpunt. U moet de stappen in deze [zelfstudie](mobile-engagement-unity-roll-a-ball.md) voordat u verder gaat met de betrokkenheid van de Mobile-integratie die we in de onderstaande zelfstudie laten. 

Deze zelfstudie is het volgende nodig:

+ [Eenheid-Editor](http://unity3d.com/get-unity)
+ [Mobiele betrokkenheid eenheid SDK](https://aka.ms/azmeunitysdk)
+ XCode-Editor

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobile betrokkenheid instellen voor uw iOS-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

###<a name="import-the-unity-package"></a>Het pakket eenheid importeren

1. Het [pakket Mobile betrokkenheid eenheid](https://aka.ms/azmeunitysdk) downloaden en sla deze op uw lokale computer. 

2. Ga naar **activa-pakket importeren > aangepaste-pakket ->** en selecteer het pakket dat u hebt gedownload in de vorige stap. 

    ![][70] 

3. Controleer of alle bestanden zijn geselecteerd en klik op de knop **importeren** . 

    ![][71] 

4. Nadat de Import is gelukt, ziet u de geïmporteerde SDK-bestanden in uw project.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>De EngagementConfiguration bijwerken

1. Open het **EngagementConfiguration** -scriptbestand SDK map en werk de **IOS\_verbinding\_tekenreeks** met de verbindingsreeks die u eerder hebt aangeschaft bij de portal van Azure.  

    ![][73]

2. Sla het bestand. 

###<a name="configure-the-app-for-basic-tracking"></a>De app voor eenvoudige bijhouden configureren

1. Open het script **PlayerController** is gekoppeld aan het object Player om te bewerken. 

2. Voeg de volgende instructie gebruiken:

        using Microsoft.Azure.Engagement.Unity;

3. Voeg de volgende naar de `Start()` methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementeren en de app uitvoeren

1. Een iOS-apparaat verbinden met uw computer. 

2. Open **File -> instellingen voor het maken** 

    ![][40]

3. **IOS** selecteren en klik op de **Schakeloptie Platform**

    ![][41]

    ![][42]

4. Klik op **instellingen van de speler** en geef een geldige bundel-id. 

    ![][53]

5. Ten slotte klik op **Maken en uitvoeren**

    ![][54]

6. U mogelijk gevraagd om te leveren van de naam van een map voor de opslag van de iOS-pakket. 

    ![][43]

7. Als alles fijn gaat, klikt u vervolgens het project wilt bepalen en u moet deze omhoog op uw XCode-toepassing openen. 

8. Zorg ervoor dat de **bundel-id** in het project juist is.  

    ![][75]

10. Nu de app uitvoeren in XCode, zodat het pakket is geïmplementeerd in uw verbonden apparaat en u uw spel eenheid op uw telefoon ziet! 

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u interactief werken met uw gebruikers en met push-meldingen en in-app in de context van campagnes messaging hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
U hoeft geen aanvullende instellingen in uw app-meldingen wilt ontvangen en deze al is ingesteld voor deze.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
