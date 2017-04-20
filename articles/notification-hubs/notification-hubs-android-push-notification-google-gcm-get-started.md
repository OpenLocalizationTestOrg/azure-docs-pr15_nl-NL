<properties
    pageTitle="Push-meldingen verzenden naar android-apparaat met Azure melding Hubs | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen voor Android-apparaten."
    services="notification-hubs"
    documentationCenter="android"
    keywords="push-meldingen, push-bericht, android push-bericht"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Push-meldingen verzenden naar android-apparaat met Azure melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] In dit onderwerp ziet u push-meldingen met Google Cloud Messaging (GCM). Als u van Google Firebase Cloud Messaging (FCM) gebruikt, raadpleegt u [de push-meldingen verzenden naar android-apparaat met Azure melding Hubs en FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

Deze zelfstudie wordt getoond hoe u met Azure melding Hubs push-meldingen verzenden naar een Android-toepassing.
U kunt een lege Android-app die push-meldingen ontvangt met behulp van Google Cloud Messaging (GCM) wilt maken.

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kan worden gedownload van GitHub [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Vereisten voor

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started)voor meer informatie.

Naast een actieve Azure account hierboven genoemde, is deze zelfstudie alleen de meest recente versie van [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797)vereist.

Voltooien van deze zelfstudie is vereist voor alle andere meldingen Hubs zelfstudies voor Android-apps.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Een project te maken die ondersteuning biedt voor Google Cloud Messaging

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Een nieuwe melding hub configureren


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. Selecteer in het blad **Instellingen** **Notification Services** en **Google (GCM)**. De API-sleutel en klik op **Opslaan**.

&emsp;&emsp;![Azure melding Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Uw hub melding is nu geconfigureerd voor het werken met GCM en u de verbindingstekenreeksen tot beide registreren van uw app om te ontvangen en versturen push-meldingen hebt.

##<a id="connecting-app"></a>Uw app verbinden met de melding hub

###<a name="create-a-new-android-project"></a>Maak een nieuw project voor Android

1. Android Studio, start u een nieuwe Android Studio-project.

    ![Android Studio - nieuw project][13]

2. Kies de vormgeving **telefoons en tablets** en de **Minimum SDK** die u wilt ondersteunen. Klik vervolgens op **volgende**.

    ![Android Studio - werkstroom voor het maken van project][14]

3. Kies **Lege activiteit** voor de belangrijkste activiteit, klik op **volgende**en klik vervolgens op **Voltooien**.

###<a name="add-google-play-services-to-the-project"></a>Google Play services toevoegen aan het project

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure melding Hubs bibliotheken toevoegen


1. In de `Build.Gradle` opbergen voor de **app**, voegt u de volgende regels in de sectie **afhankelijkheden** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Hiermee voegt u de volgende opslagplaats na de sectie **afhankelijkheden** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>De AndroidManifest.xml bijwerken.


1. Ter ondersteuning van GCM, moeten we een service van de luisteraar ervan af exemplaar-ID implementeren in onze code die wordt gebruikt voor het [verkrijgen van registratie tokens](https://developers.google.com/cloud-messaging/android/client#sample-register) [Van Google exemplaar ID-API](https://developers.google.com/instance-id/)gebruiken. In deze zelfstudie we de klas naam `MyInstanceIDService`. 
 
    Voor het toevoegen van de volgende servicedefinitie naar het bestand AndroidManifest.xml, is het binnen de `<application>` tag. Vervang de `<your package>` tijdelijke aanduiding met het pakketnaam van de werkelijke wordt weergegeven boven aan de `AndroidManifest.xml` bestand.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Als we onze GCM registratietoken van de exemplaar-ID-API hebt ontvangen, we gebruiken deze registreren [bij de Hub van Azure melding](notification-hubs-push-notification-registration-management.md). We biedt ondersteuning voor deze registratie in het achtergrond met een `IntentService` met de naam `RegistrationIntentService`. Deze service ook is verantwoordelijk voor het [vernieuwen van onze GCM registratietoken](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
 
    Voor het toevoegen van de volgende servicedefinitie naar het bestand AndroidManifest.xml, is het binnen de `<application>` tag. Vervang de `<your package>` tijdelijke aanduiding met het pakketnaam van de werkelijke wordt weergegeven boven aan de `AndroidManifest.xml` bestand. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. We wordt ook een ontvanger als u wilt ontvangen definiëren. Voor het toevoegen van de volgende definitie van de ontvanger aan het bestand AndroidManifest.xml, is het binnen de `<application>` tag. Vervang de `<your package>` tijdelijke aanduiding met het pakketnaam van de werkelijke wordt weergegeven boven aan de `AndroidManifest.xml` bestand.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Voeg het volgende nodig GCM gerelateerd machtigingen hieronder de `</application>` tag. Zorg ervoor dat voor het vervangen van `<your package>` met de pakketnaam wordt weergegeven boven aan de `AndroidManifest.xml` bestand.

    Zie [een GCM Client-app voor Android instellen](https://developers.google.com/cloud-messaging/android/client#manifest)voor meer informatie over deze machtigingen.

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Code toe te voegen


1. Vouw in de projectweergave **app** > **src** > **belangrijkste** > **java**. Met de rechtermuisknop op de pakketmap onder **java**, klikt u op **Nieuw**en klik vervolgens op **Java-klasse**. Een nieuwe klasse met de naam toevoegen `NotificationSettings`. 

    ![Android Studio - nieuwe Java-klasse][6]

    Zorg ervoor dat bij de volgende drie tijdelijke aanduidingen in de volgende code voor het `NotificationSettings` class:
    * **SenderId**: het project dat u eerder in de [Google Cloud-Console](http://cloud.google.com/console).
    * **HubListenConnectionString**: de verbindingsreeks **DefaultListenAccessSignature** voor uw hub. U kunt deze verbindingsreeks kopiëren door te klikken **-Beleid** op het blad **Instellingen** van de hub van de [Azure-Portal].
    * **HubName**: Gebruik de naam van uw hub-melding die wordt weergegeven in het blad hub in de [Portal van Azure].

    `NotificationSettings`code:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Een andere nieuwe klasse met de naam met de bovenstaande stappen toevoegen `MyInstanceIDService`. Dit is onze exemplaar-ID luisteraar ervan af service-implementatie.

    De code voor deze klasse telefonisch onze `IntentService` [het token GCM vernieuwen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) op de achtergrond.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Een andere nieuwe klasse toevoegen aan uw project met de naam, `RegistrationIntentService`. Dit is de uitvoering voor onze `IntentService` die [het token GCM vernieuwen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) en [registreren met de melding hub](notification-hubs-push-notification-registration-management.md)worden verwerkt.

    Gebruik de volgende code voor deze klasse.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. In uw `MainActivity` klasse, voegt u het volgende `import` instructies boven de declaratie class.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. De volgende privé-leden toevoegen aan de bovenkant van de klas. We gebruiken deze [de beschikbaarheid van Google Play Services zoals aanbevolen door Google gecontroleerd](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. In uw `MainActivity` klasse, het toevoegen van de volgende methode voor de beschikbaarheid van Google-Services afspelen. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. In uw `MainActivity` klasse, voegt u de volgende code die moeten worden gecontroleerd voor Google-Services afspelen belt uw `IntentService` uw registratie GCM token ophalen en registreert met uw hub melding.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. In de `OnCreate` methode van de `MainActivity` klasse, voegt u de volgende code om te starten van het registratieproces wanneer activiteit wordt gemaakt.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Deze aanvullende methoden voor het toevoegen de `MainActivity` om te controleren of app staat en rapport status in uw app.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. De `ToastNotify` methode gebruikt *"Hallo wereld"* `TextView` besturingselement rapportstatus en meldingen permanent in de app. In de indeling activity_main.xml toevoegen de volgende id voor dat besturingselement.

        android:id="@+id/text_hello"

11. We vervolgens toevoegen een subcategorie voor onze ontvanger die zoals gedefinieerd in de AndroidManifest.xml. Een andere nieuwe klasse toevoegen aan uw project met de naam `MyHandler`.

12. De volgende importinstructies toevoegen aan de bovenkant van `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Voeg de volgende code voor het `MyHandler` class zodat u een subcategorie van `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Deze code overschrijft de `OnReceive` methode, zodat de handler wordt rapporteren meldingen die zijn ontvangen. De push-meldingen voor de Android melding manager met behulp van de handler ook stuurt de `sendNotification()` methode. De `sendNotification()` methode moet worden uitgevoerd wanneer de app niet wordt uitgevoerd en er wordt een melding ontvangen.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Android Studio op de menubalk, op de knop **Opbouwen** > **Bouw die tabellen opnieuw Project** om ervoor te zorgen dat er geen fouten aanwezig in uw code zijn.

##<a name="sending-push-notifications"></a>Push-meldingen verzenden

U kunt testen push-meldingen ontvangen in uw app door deze te verzenden via de [Portal van Azure] - zoek naar de sectie **Probleemoplossing** in het blad hub, zoals hieronder wordt weergegeven.

![Azure melding Hubs - Test verzenden](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Optioneel) Push-meldingen rechtstreeks vanuit de app verzenden

Normaal gesproken zou u meldingen met een back-endserver verzenden. Voor sommige gevallen wilt u mogelijk kunnen push-meldingen rechtstreeks vanuit de clienttoepassing verzenden. In deze sectie wordt uitgelegd hoe u meldingen wilt verzenden vanuit de client de [Azure melding Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx)gebruiken.

1. Vouw in de weergave van Project voor Android Studio, **App** > **src** > **belangrijkste** > **resolutie** > **indeling**. Open de `activity_main.xml` indelingsbestand en klik op de **tekst** van de tab-toets om de tekstinhoud van het bestand te werken. Werken met de onderstaande code die wordt toegevoegd nieuwe `Button` en `EditText` besturingselementen voor het verzenden van push notification-berichten op de melding-hub. Alleen voordat deze code onderaan toevoegen `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Vouw in de weergave van Project voor Android Studio, **App** > **src** > **belangrijkste** > **resolutie** > **waarden**. Open de `strings.xml` bestand en de tekenreekswaarden waarnaar wordt verwezen door de nieuwe `Button` en `EditText` besturingselementen. Volgende onderaan in het bestand, net voordat toevoegen `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. In uw `NotificationSetting.java` bestand, voegt u de volgende instelling naar de `NotificationSettings` class.

    Update `HubFullAccess` met de verbindingsreeks **DefaultFullSharedAccessSignature** voor uw hub. Deze verbindingsreeks kan worden gekopieerd van de [Azure-Portal] door te klikken **-Beleid** op het blad **Instellingen** voor uw hub melding.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. In uw `MainActivity.java` bestand, voegt u het volgende `import` bovenstaande beweringen de `MainActivity` class.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. In uw `MainActivity.java` bestand, de volgende leden toevoegen aan de bovenkant van de `MainActivity` class.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. U moet een token Software Access handtekening (SA's) om te verifiëren van een POST-aanvraag om berichten te verzenden naar uw hub melding maken. Hiermee wordt uitgevoerd door parseren van de belangrijkste gegevens uit de verbindingsreeks en vervolgens het token SA's te maken zoals in de [Algemene concepten](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-verwijzing is vermeld. De volgende code is een voorbeeldimplementatie.

    In `MainActivity.java`, voegt u de volgende methode naar de `MainActivity` class parseren van de verbindingsreeks.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. In `MainActivity.java`, voegt u de volgende methode naar de `MainActivity` class een token SA's-verificatie.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. In `MainActivity.java`, voegt u de volgende methode naar de `MainActivity` class afhandelen, klik vervolgens op de knop **Bericht verzenden** en het meldingsbericht push verzenden naar de hub met behulp van de ingebouwde REST API.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Uw app testen

####<a name="push-notifications-in-the-emulator"></a>Push-meldingen in de emulator

Als u testen push-meldingen binnen een emulator wilt, zorg dat uw afbeelding emulator het Google-API niveau dat u hebt gekozen voor uw app ondersteunt. Als de afbeelding niet systeemeigen Google APIs ondersteunt, worden er met de **SERVICE\_niet\_beschikbaar** uitzondering.

Zorg ervoor dat u uw Google-account hebt toegevoegd aan uw actieve emulator onder **Instellingen**naast het bovenstaande, > **Accounts**. Anders uw pogingen registreren bij GCM kunnen dit leiden tot de **verificatie\_FAILED** uitzondering.

####<a name="running-the-application"></a>De toepassing wordt uitgevoerd

1. De app uitgevoerd en u ziet dat de registratie-ID voor de registratie van een voltooid wordt gerapporteerd.

    ![Testen op Android - kanaal registratie][18]

2. Voer een waarschuwingsbericht worden verzonden naar alle Android-apparaten die bij de hub hebben geregistreerd.

    ![Testen op Android - u een bericht verzendt][19]

3. Druk op **melding verzenden**. Alle apparaten die heb de app actief ziet een `AlertDialog` exemplaar met het meldingsbericht push. Apparaten die geen de app uitgevoerd, maar eerder zijn geregistreerd voor push-meldingen ontvangt een melding in Android Notification Manager. Die kunnen worden bekeken door te vegen omlaag in de linkerbovenhoek.

    ![Testen op Android - meldingen][21]

##<a name="next-steps"></a>Volgende stappen

Het is raadzaam de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers] als de volgende stap. Dit wordt uitgelegd hoe u meldingen wilt verzenden van een ASP.NET-end markeringen gebruiken om u te richten op specifieke gebruikers.

Als u wilt dat uw gebruikers door groepen rente in segmenten, raadpleegt u de zelfstudie [Gebruik melding Hubs naar het laatste nieuws verzenden] .

Zie onze [Melding Hubs richtlijnen]voor meer meer algemene informatie over melding Hubs.

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Melding Hubs richtlijnen]: http://msdn.microsoft.com/library/jj927170.aspx
[Gebruik van de melding Hubs om push-meldingen aan gebruikers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Laatste nieuws verzenden via melding Hubs]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com
