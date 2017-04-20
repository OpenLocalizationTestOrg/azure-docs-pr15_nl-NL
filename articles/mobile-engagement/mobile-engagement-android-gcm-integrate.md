<properties
    pageTitle="Azure mobiele betrokkenheid Android SDK-integratie"
    description="Meest recente updates en procedures voor Android SDK voor Azure Mobile betrokkenheid"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>GCM integreren met mobiele betrokkenheid

> [AZURE.IMPORTANT] U moet de integratie procedure wordt beschreven in de How to betrokkenheid integreren op Android document voordat u deze handleiding volgen.
>
> Dit document is alleen nuttig als u al en zijn geïntegreerd met het bereik module naar de Google Play apparaten push-abonnement. Als u wilt bereiken campagnes in uw toepassing integreren, lees eerst integreren betrokkenheid bereiken op android-apparaat.

##<a name="introduction"></a>Inleiding

Integratie van GCM kan de toepassing moet worden geplaatst.

GCM nettoladingen verplaatst naar de SDK altijd bevatten de `azme` key in het gegevensobject. Dus als u in uw toepassing GCM voor andere doeleinden gebruikt, kunt u dit gereedschap duwt op basis van die toets filteren.

> [AZURE.IMPORTANT] Alleen apparaten met Android 2.2 of bovenaan ondervindt Google Play geïnstalleerd en problemen Google achtergrond verbinding ingeschakeld kunt verplaatst met GCM; u kunt echter integreren dat deze code veilig op niet-ondersteunde apparaten (alleen die wordt gebruikt).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Een project Google Cloud Messaging maken met API-sleutel

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integratie

### <a name="managing-device-registrations"></a>Apparaat registraties beheren

Elk apparaat moet een opdracht registratiegegevens verzenden naar de Google-servers, anders kan ze kunnen niet worden bereikt.

Een apparaat kan ook unregister GCM-mailmeldingen (het apparaat is automatisch gemaakt wanneer u de toepassing is).

Als u [Google Play SDK] niet gebruikt of u niet al de bedoeling registratie uzelf verzendt, kunt u betrokkenheid registreren van het apparaat automatisch voor u kunt aanbrengen.

Om dit mogelijk toevoegen de volgende uw `AndroidManifest.xml` bestand, binnen de `<application/>` tag:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id om de betrokkenheid Push-service te communiceren en meldingen ontvangen

Toevoegen de volgende handelingen uit om te voldoen om te communiceren van de registratie-id van het apparaat dat bij de betrokkenheid Push-service en de meldingen ontvangen, uw `AndroidManifest.xml` bestand, binnen de `<application/>` markeren (ook als u apparaat registraties zelf beheren):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Controleer of u de volgende machtigingen hebben uw `AndroidManifest.xml` (nadat de `</application>` tag).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Mobiele betrokkenheid toegang verlenen aan de GCM API-sleutel

[Deze handleiding](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile betrokkenheid om toegang te verlenen aan de GCM API-sleutel volgen.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
