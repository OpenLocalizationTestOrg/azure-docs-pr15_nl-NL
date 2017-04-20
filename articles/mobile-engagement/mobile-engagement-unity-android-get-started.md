<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor Android eenheid implementatie"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor eenheid apps implementeren naar iOS-apparaten."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Aan de slag met Azure Mobile betrokkenheid voor Android eenheid implementatie

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u het gebruik van Azure Mobile betrokkenheid voor meer informatie over het appgebruik van uw en het verzenden van push-meldingen aan gebruikers van een toepassing voor de eenheid gesegmenteerd wanneer u de implementatie in een Android-apparaat.
Deze zelfstudie gebruikt de klassieke eenheid implementeren een bal zelfstudie als het beginpunt. U moet de stappen in deze [zelfstudie](mobile-engagement-unity-roll-a-ball.md) voordat u verder gaat met de betrokkenheid van de Mobile-integratie die we in de onderstaande zelfstudie laten. 

Deze zelfstudie is het volgende nodig:

+ [Eenheid-Editor](http://unity3d.com/get-unity)
+ [Mobiele betrokkenheid eenheid SDK](https://aka.ms/azmeunitysdk)
+ Google-Android SDK

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobiele betrokkenheid van de instelling voor uw Android-app

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

1. Open het **EngagementConfiguration** -scriptbestand SDK map en werk de **ANDROID\_verbinding\_tekenreeks** met de verbindingsreeks die u eerder hebt aangeschaft bij de portal van Azure.  

    ![][73]

2. Sla het bestand 

3. Uitvoeren **Bestand -> betrokkenheid -> Android Manifest genereren**. Dit is de invoegtoepassing voor toegevoegd door de mobiele betrokkenheid SDK en erop te klikken, wordt de projectinstellingen van uw automatisch bijgewerkt. 

    ![][74]

> [AZURE.IMPORTANT] Zorg ervoor dat dit uitvoeren telkens wanneer u het **EngagementConfiguration** bestand anders dat uw wijzigingen niet worden doorgevoerd in de app bijwerken. 

###<a name="configure-the-app-for-basic-tracking"></a>De app voor eenvoudige bijhouden configureren

1. Open het script **PlayerController** is gekoppeld aan het object Player om te bewerken. 

2. Voeg de volgende instructie gebruiken:

        using Microsoft.Azure.Engagement.Unity;

3. Voeg de volgende naar de `Start()` methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementeren en de app uitvoeren
Zorg ervoor dat er Android SDK voordat u probeert te implementeren van deze app eenheid naar uw apparaat op uw computer is geïnstalleerd. 

1. Een Android-apparaat verbinden met uw computer. 

2. Open **File -> instellingen voor het maken** 

    ![][40]

3. Selecteer **Android** en klik op de **Schakeloptie Platform**

    ![][51]

    ![][52]

4. Klik op **instellingen van de speler** en geef een geldige bundel-id. 

    ![][53]

5. Ten slotte klik op **Maken en uitvoeren**

    ![][54]

6. U mogelijk gevraagd om te leveren van de naam van een map voor de opslag van de Android-pakket. 

7. Als alles fijn gaat, klikt u vervolgens het pakket wordt geïmplementeerd in uw verbonden apparaat en ziet u uw spel eenheid op uw telefoon. 

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>De EngagementConfiguration bijwerken

1. Open het **EngagementConfiguration** -scriptbestand SDK map en werk de **ANDROID\_GOOGLE\_getal** met de **Google-projectnummer** die u eerder hebt aangeschaft bij de Google Cloud Developer portal. Dit is een tekenreeks waarde Controleer dus of tussen dubbele aanhalingstekens is geplaatst. 

    ![][75]

2. Sla het bestand. 

3. Uitvoeren **Bestand -> betrokkenheid -> Android Manifest genereren**. Dit is de invoegtoepassing voor toegevoegd door de mobiele betrokkenheid SDK en erop te klikken, wordt de projectinstellingen van uw automatisch bijgewerkt. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>De app als u wilt ontvangen configureren

1. Open het script **PlayerController** is gekoppeld aan het object Player om te bewerken. 

2. Voeg de volgende naar de `Start()` methode

        EngagementReachAgent.Initialize();

3. Nu de app wordt bijgewerkt, implementeren en uitvoeren van de app op een apparaat per onderstaande instructies. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
