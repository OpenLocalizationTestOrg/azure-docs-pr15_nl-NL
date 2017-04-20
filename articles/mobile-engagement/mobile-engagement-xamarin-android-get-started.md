<properties
    pageTitle="Aan de slag met Azure mobiele betrokkenheid voor Xamarin.Android"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor Xamarin.Android Apps."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Aan de slag met Azure mobiele betrokkenheid voor Xamarin.Android-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u Azure Mobile betrokkenheid gebruiken voor meer informatie over het appgebruik van uw en hoe push-meldingen naar gebruikers kunt sturen gesegmenteerde van een toepassing Xamarin.Android.
Deze zelfstudie ziet u de eenvoudige broadcast scenario Mobile betrokkenheid gebruiken. In deze maakt u een lege Xamarin.Android-app die worden verzameld basisgegevens en Google Cloud Messaging (GCM) met push-meldingen ontvangt.

Deze zelfstudie is het volgende nodig:

+ [Xamarin Studio](http://xamarin.com/studio). U kunt ook Visual Studio gebruikt met Xamarin, maar deze zelfstudie wordt Xamarin Studio. Zie [installatie en installeer voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx)voor installatie-instructies.
+ [Mobiele betrokkenheid Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started)voor meer informatie.

##<a id="setup-azme"></a>Mobiele betrokkenheid van de instelling voor uw Android-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie", dat wil de minimale set vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. 

We gaan een eenvoudige app maken met Xamarin Studio om te laten zien van de integratie.

###<a name="create-a-new-xamarinandroid-project"></a>Een nieuw Xamarin.Android project maken

1. Starten **Xamarin Studio** gaat u naar **bestand** -> **nieuwe** -> **oplossing** 

    ![][1]

2. Selecteer **Android-App** en controleer of dat de geselecteerde taal is **C#** en klik op **volgende**.

    ![][2]

3. Vul de **App-naam** en de **organisatie-id**. Zorg ervoor dat vinkje **Afspelen Google-Services** en klik op **volgende**. 

    ![][3]
    
4. Werk de **Naam van het Project**, de **Naam van de oplossing** en de **locatie** als vereist en klik op **maken**.

    ![][4]
 
De app waarin Mobile betrokkenheid integreert maakt Xamarin Studio. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met mobiele betrokkenheid backend

1. Klik met de rechtermuisknop op de map **pakketten** in de vensters oplossing en selecteer **Pakketten toevoegen**

    ![][5]

2. Zoeken naar de **Microsoft Azure Mobile betrokkenheid Xamarin SDK** en deze toevoegen aan uw oplossing.  

    ![][6]
   
3. Open **MainActivity.cs** en voeg de volgende instructies:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. In de `OnCreate` methode, de volgende handelingen uit als u de verbinding met mobiele betrokkenheid backend geïnitialiseerd wilt toevoegen. Zorg ervoor dat uw **ConnectionString**toevoegen. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Machtigingen en de declaratie van een service toevoegen

1. Open het bestand **Manifest.xml** onder de map eigenschappen. Selecteer het tabblad bron zodat u de XML-bron rechtstreeks bijwerken.
 
2. Deze machtigingen toevoegen aan het Manifest.xml (die u kunt vinden onder de map **Eigenschappen** ) van uw project direct voor of na de `<application>` tag:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Voeg de volgende tussen de `<application>` en `</application>` labels aan de agentservice declareren:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Vervang in de code die u zojuist hebt geplakt, `"<Your application name>"` in het label. Dit wordt weergegeven in het menu **Instellingen** , waar gebruikers services worden uitgevoerd op het apparaat kunnen zien. U kunt bijvoorbeeld het woord 'Service' van dat etiket toevoegen.

###<a name="send-a-screen-to-mobile-engagement"></a>Een scherm verzenden naar mobiele betrokkenheid

Om te beginnen met het verzenden van gegevens en ervoor zorgen dat de gebruikers actief zijn, moet u ten minste één scherm naar de backend Mobile betrokkenheid sturen. Hiervoor-Zorg ervoor dat de `MainActivity` overneemt van `EngagementActivity` in plaats van `Activity`.

    public class MainActivity : EngagementActivity
    
U kunt ook als u niet kunt van overnemen `EngagementActivity` u moet toevoegen `.StartActivity` en `.EndActivity` methoden in `OnResume` en `OnPause` respectievelijk.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u werken met gegevens en uw gebruikers met push-meldingen en in-app in de context van campagnes messaging hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties stelt uw app ontvangen.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
