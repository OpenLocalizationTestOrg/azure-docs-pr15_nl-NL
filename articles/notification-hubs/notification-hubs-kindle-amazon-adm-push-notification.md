<properties
    pageTitle="Aan de slag met Azure melding Hubs voor Kindle apps | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen verzenden naar een toepassing voor de Kindle."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Aan de slag met melding Hubs voor Kindle-apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie ziet u hoe u push-meldingen naar een toepassing Kindle verzenden via Azure melding Hubs.
U kunt een lege Kindle-app die push-meldingen ontvangt met behulp van Amazon-apparaat Messaging (ADM) wilt maken.

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ De Android SDK (we wordt ervan uitgegaan dat u wordt Eclips) krijgen van de <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-site</a>.
+ Volg de stappen in de <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Instelling van uw ontwikkelomgeving</a> voor het instellen van uw ontwikkelomgeving voor Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Een nieuwe app toevoegen aan de developer portal

1. Maak eerst een app in de [Amazon developer portal].

    ![][0]

2. Kopieer de **toepassingstoets**.

    ![][1]

3. Klik op de naam van de app in de portal en klik vervolgens op het tabblad **Apparaat Messaging** .

    ![][2]

4. Klik op **maken een nieuw profiel voor beveiliging**en maakt u een nieuw beveiligingsprofiel (bijvoorbeeld **TestAdm beveiligingsprofiel**). Klik vervolgens op **Opslaan**.

    ![][3]

5. Klik op **Beveiligingsprofielen** beveiligingsprofiel dat u zojuist hebt gemaakt weer te geven. Kopieer de waarden voor **Klant-ID** en **Geheim Client** voor later gebruik.

    ![][4]

## <a name="create-an-api-key"></a>Een API-sleutel maken

1. Open een opdrachtprompt met beheerdersbevoegdheden.
2. Navigeer naar de map Android SDK.
3. Voer de volgende opdracht uit:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Typ het wachtwoord **keystore** **android**.

5.  Kopieer de vingerafdruk **MD5** .
6.  Terug in de developer portal, klik op het tabblad **Messaging** op **Android/Kindle** en voer de naam van het pakket voor uw app (bijvoorbeeld **com.sample.notificationhubtest**) en de waarde **MD5** en klik vervolgens op **API-sleutel genereren**.

## <a name="add-credentials-to-the-hub"></a>Referenties toevoegen aan de hub

Klik in de portal moet u de geheim client en client-ID toevoegen aan het tabblad **configureren** van uw hub-melding.

## <a name="set-up-your-application"></a>Uw toepassing instellen

> [AZURE.NOTE] Wanneer u een toepassing maakt, gebruikt u ten minste API niveau 17.

De ADM-bibliotheken toevoegen aan uw project Eclips:

1. De bibliotheek ADM, [downloadt u de SDK]verkrijgt. Pak de SDK zip-bestand.
2. Met de rechtermuisknop op uw project in Eclips, en klik vervolgens op **Eigenschappen**. Selecteer **Java bouwen pad** aan de linkerkant en selecteer vervolgens het tabblad **bibliotheken **aan de bovenkant. Klik op **Externe Jar toevoegen**en selecteer het bestand `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` uit de map waarin u de SDK Amazon hebt uitgepakt.
3. Download de NotificationHubs Android SDK (koppeling).
4. Het pakket pak en sleep vervolgens het bestand `notification-hubs-sdk.jar` in de `libs` map in Eclips.

Uw app manifest ter ondersteuning van ADM bewerken:

1. De naamruimte Amazon in het manifest hoofdelement toevoegen:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Machtigingen toevoegen als het eerste element onder het manifest element. **[Naam van uw pakket]** vervangen door het pakket die u hebt gebruikt om de toepassing te maken.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Het volgende element als het eerste onderliggende van de toepassingselement invoegen. Houd er rekening mee te **[Naam van uw SERVICE]** vervangen door de naam van uw ADM bericht-handler die u in het volgende gedeelte maakt (inclusief het pakket) en **[Naam van uw pakket]** vervangen door de pakketnaam waarmee u uw app hebt gemaakt.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Uw ADM bericht-handler maken

1. Maak een nieuwe klasse die van overneemt `com.amazon.device.messaging.ADMMessageHandlerBase` en noem deze `MyADMMessageHandler`, zoals weergegeven in de volgende afbeelding:

    ![][6]

2. Voeg de volgende `import` instructies:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Voeg de volgende code in de klas die u hebt gemaakt. Houd er rekening mee te vervangen van de hub naam en verbinding tekenreeks (luisteren):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Voeg de volgende code aan de `OnMessage()` methode:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Voeg de volgende code aan de `OnRegistered` methode:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Voeg de volgende code aan de `OnUnregistered` methode:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. In de `MainActivity` methode toevoegen de volgende importeren-instructie:

        import com.amazon.device.messaging.ADM;

8. Voeg de volgende code toe aan het einde van de `OnCreate` methode:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Uw API-sleutel toevoegen aan uw app

1. Maak een nieuw bestand met de naam **api_key.txt** in de directory activa van uw project in Eclips.
2. Open het bestand en kopieer de API-sleutel die u in de Amazon developer portal gegenereerd.

## <a name="run-the-app"></a>De app uitvoeren

1. Start de emulator.
2. In de emulator, veegt u vanaf de bovenkant en klik op **Instellingen**, klik op **Mijn account** en registreert met een geldige Amazon-account.
3. In Eclips, moet u de app uitvoeren.

> [AZURE.NOTE] Als u een probleem optreedt, schakelt u het tijdstip waarop de emulator (of het apparaat). De tijdwaarde moet nauwkeurig. Als u wilt wijzigen van de tijd van de emulator Kindle, kunt u de volgende opdracht uitvoeren vanaf uw map met Android SDK platform-hulpprogramma's:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Een bericht verzenden

Een bericht verzenden met behulp van .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[de SDK downloaden]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
