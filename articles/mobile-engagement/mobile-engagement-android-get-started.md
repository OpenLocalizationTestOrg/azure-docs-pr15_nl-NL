<properties
    pageTitle="Aan de slag met Android Apps Azure Mobile betrokkenheid"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en push-meldingen voor Android-apps."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Aan de slag met Azure Mobile betrokkenheid voor Android-apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u Azure Mobile betrokkenheid gebruiken voor meer informatie over het appgebruik van uw en hoe push-meldingen naar gebruikers kunt sturen gesegmenteerde van een Android-toepassing.
Deze zelfstudie ziet u de eenvoudige broadcast scenario Mobile betrokkenheid gebruiken. In deze maakt u een lege Android-app die worden verzameld basisgegevens en Google Cloud Messaging (GCM) met push-meldingen ontvangt.

## <a name="prerequisites"></a>Vereisten voor

Voltooien van deze zelfstudie, is de [Android Tools voor ontwikkelaars](https://developer.android.com/sdk/index.html), waaronder de geïntegreerde ontwikkelomgeving Android Studio en de meest recente Android platform vereist.

Er moeten ook de [Mobiele betrokkenheid Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started)voor meer informatie.

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Mobile betrokkenheid instellen voor uw Android-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Deze zelfstudie biedt een 'eenvoudige integratie", dat wil de minimale set vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. U kunt een eenvoudige app maken met Android Studio om te laten zien van de integratie.

De volledige integratie documentatie vindt u in de [Mobile betrokkenheid Android SDK-integratie](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Een Android-project maken

1. **Android Studio**Start en selecteer **een nieuwe Android Studio project starten**in het pop-upvenster.

    ![][1]

2. Geef een app-naam en het bedrijf domein. Maak een notitie van wat u invullen, omdat u deze later nodig hebt. Klik op **volgende**.

    ![][2]

3. Selecteer de doel-vormgeving en API niveau en klik op **volgende**.

    >[AZURE.NOTE] Mobile betrokkenheid vereist API niveau 10 minimale (Android 2.3.3).

    ![][3]

4. Selecteer **Lege activiteit** hier, dat wil het scherm alleen voor deze app zeggen en klik op **volgende**.

    ![][4]

5. Ten slotte, laat u de standaardwaarden is en klik op **Voltooien**.

    ![][5]

Android Studio maakt nu de demo-app waarin we Mobile betrokkenheid integreren.

### <a name="include-the-sdk-library-in-your-project"></a>De bibliotheek SDK opnemen in uw project

1. De [mobiele betrokkenheid Android SDK](https://aka.ms/vq9mfn)downloaden.
2. Pak de archiefbestanden naar een map op uw computer.
3. De bibliotheek .jar voor de huidige versie van deze SDK identificeren en naar het Klembord kopiëren.

      ![][6]

4. Ga naar de sectie **Project** (1) en de JAR plakken in de map libs (2).

      ![][7]

5. Als u wilt de bibliotheek hebt geladen, het project te synchroniseren.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Uw app verbinden met mobiele betrokkenheid backend met de verbindingsreeks

1. Kopieer de volgende programmacoderegels naar de activiteiten maken (moeten worden uitgevoerd alleen op één plaats van de toepassing, meestal de belangrijkste activiteit). Voor dit voorbeeld-app, u kunt snel de MainActivity onder src openen -> Hoofdgegeven -> java Mapopties en toevoegen van de volgende handelingen uit:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. De verwijzingen oplossen door op Alt + Enter te drukken of de volgende importinstructies toe te voegen:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Ga terug naar de klassieke Azure-Portal in de pagina **Verbindingsgegevens** van uw app en kopieer de **Verbindingsreeks**.

      ![][9]

4. Plak deze in de `setConnectionString` parameter, worden vervangen door de hele tekenreeks wordt weergegeven in de volgende code:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Machtigingen en de declaratie van een service toevoegen

1. Deze machtigingen toevoegen aan het Manifest.xml van uw project direct voor of na de `<application>` tag:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Toevoegen als u wilt declareren de agent-service, kunt u deze code tussen de `<application>` en `</application>` labels:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Vervang in de code die u hebt geplakt, `"<Your application name>"` op het etiket, die wordt weergegeven in het menu **Instellingen** is waar u de services worden uitgevoerd op het apparaat kunt zien. U kunt bijvoorbeeld het woord 'Service' van dat etiket toevoegen.

### <a name="send-a-screen-to-mobile-engagement"></a>Een scherm verzenden naar mobiele betrokkenheid

Als u wilt beginnen met het verzenden van gegevens en zorg ervoor dat de gebruikers actief zijn, moet u ten minste één scherm (activiteit) op de backend Mobile betrokkenheid verzenden.

Ga naar **MainActivity.java** en voeg de volgende handelingen uit als u wilt de klasse base van **MainActivity** naar **EngagementActivity**vervangen:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Als uw grondtal klasse niet *activiteit is*, raadpleegt u [Android rapportage geavanceerde](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) voor informatie over het overnemen van verschillende klassen.


Opmerking van de volgende regel in dit scenario's met een eenvoudig voorbeeld:

    // setSupportActionBar(toolbar);

Als u wilt behouden de `ActionBar` in uw app, gaat u naar [Geavanceerde Android rapportage](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Push-meldingen en app messaging inschakelen

Tijdens een campagne kunt Mobile betrokkenheid u werken met gegevens en uw gebruikers met push-meldingen en app messaging hebt bereikt. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende sectie stelt uw app ontvangen.

### <a name="copy-sdk-resources-in-your-project"></a>SDK bronnen in uw project kopiëren

1. Ga terug naar de inhoud van uw SDK downloaden en kopieer de map met de **resolutie** .

    ![][10]

2. Ga terug naar Android Studio, selecteert u **de hoofdmap van uw projectbestanden** en plakt u deze als u wilt de resources toevoegen aan uw project.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Ga naar [Android SDK](mobile-engagement-android-sdk-overview.md) om gedetailleerde informatie over de integratie SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
