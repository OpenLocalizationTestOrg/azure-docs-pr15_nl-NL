<properties
    pageTitle="Push-meldingen toevoegen aan uw Xamarin.Forms-app | Microsoft Azure"
    description="Leer hoe u meerdere platforms push-meldingen naar uw apps Xamarin.Forms verzenden via Azure services."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Push-meldingen toevoegen aan uw Xamarin.Forms-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie voegt u push-meldingen voor alle projecten het resultaat is van de [Xamarin.Forms snel starten](app-service-mobile-xamarin-forms-get-started.md) zodat een push-bericht wordt verzonden naar alle platforms clients telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de push notification-extensie-pakket. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten voor

* Voor iOS, moet u een [Apple ontwikkelaars programma lidmaatschap](https://developer.apple.com/programs/ios/) en een fysieke iOS-apparaat omdat de [push-meldingen biedt geen ondersteuning voor iOS simulator](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Een melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject als u wilt verzenden push-meldingen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Optioneel) Configureren en uitvoeren van het project Android

Hiermee voert u deze sectie om in te schakelen push-meldingen voor het project Xamarin.Forms Droid voor Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging (FCM) inschakelen

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>De Mobile-App-end om te verzenden push-aanvragen via van FCM configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Push-meldingen toevoegen aan het project Android

Met de end geconfigureerd met FCM, kunnen we onderdelen en codes naar de klant registreren bij FCM, registreren voor push-meldingen met Azure melding Hubs tot en met de mobiele app-end en meldingen ontvangen toevoegen.

1. In het project **Droid** , met de rechtermuisknop op de map **Components** op **Krijgen meer onderdelen...**, zoek naar het onderdeel **Google Cloud Messaging-Client** en toe te voegen aan het project. Dit onderdeel ondersteunt push-meldingen voor een project Xamarin Android.


2. Open het projectbestand MainActivity.cs en voeg de volgende met de instructie boven aan het bestand:

        using Gcm.Client;

3. De volgende code toevoegen aan de methode **OnCreate** na de oproep door naar **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Een nieuwe **CreateAndShowDialog** helpmethode, als volgt toevoegen:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. De volgende code hebt toegevoegd aan de klasse **MainActivity** :

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Dit beschrijft het huidige exemplaar van de **MainActivity** zodat we de belangrijkste UI-thread kunt uitvoeren.

6. Geïnitialiseerd de `instance`, variabele aan het begin van de methode **OnCreate** , als volgt.

        // Set the current instance of MainActivity.
        instance = this;

2. Een nieuw klassebestand toevoegen aan het **Droid** project met de naam `GcmService.cs`, en zorg ervoor dat de volgende instructies voor het **gebruik van** aan de bovenkant van het bestand aanwezig zijn:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Na de instructies **gebruiken** en voordat u de declaratie van de **naamruimte** , moet u de volgende toestemmingsaanvragen toevoegen aan de bovenkant van het bestand.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. De definitie van de volgende klasse toevoegen aan de naamruimte. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]**< PROJECT_NUMBER >** vervangen door uw projectnummer dat u eerder hebt genoteerd.   

11. De lege **GcmService** klas vervangen door de volgende code die de nieuwe broadcast ontvanger gebruikt:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. De volgende code hebt toegevoegd aan de **GcmService** -klasse die wordt de gebeurtenis-handler **OnRegistered** genegeerd en wordt een methode **registreren** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. De volgende code die wordt geïmplementeerd **OnMessage**toevoegen: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Hiermee omgaat met binnenkomende meldingen en stuurt u naar de notification manager moet worden weergegeven.

14. **GcmServiceBase** moet u implementeren van de **OnUnRegistered** **BijFout** -handler methoden en, die u kunt als volgt doen:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

U bent nu klaar test push-meldingen in de app op een Android-apparaat of de emulator uitgevoerd.

###<a name="test-push-notifications-in-your-android-app"></a>Test push-meldingen in uw Android-app

De eerste twee stappen zijn vereist alleen wanneer testen op een emulator.

1. Zorg ervoor dat u naar implementeren of foutopsporing op een virtueel apparaat waarop Google APIs instellen als het doel, zoals hieronder wordt weergegeven in de manager Android virtuele apparaat (AVD).

2. Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**en volg de aanwijzingen voor het gebruik van een bestaande Google-account toevoegen aan het apparaat naar een nieuwe record maken.

1. Klik met de rechtermuisknop op het project **Droid** in Visual Studio of Xamarin Studio, en klik op **instellen als opstartproject**.

2. Druk op de knop **uitvoeren** voor het bouwen van het project en start de app op uw Android-apparaat of emulator.

3. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.

4. Controleren of een melding is ontvangen wanneer een item wordt toegevoegd.


##<a name="optional-configure-and-run-the-ios-project"></a>(Optioneel) Configureren en uitvoeren van het project iOS

In dit gedeelte is voor het uitvoeren van het project Xamarin iOS voor iOS-apparaten. U kunt dit gedeelte overslaan als u niet met iOS-apparaten werkt.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>De melding hub voor APNS configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Volgende configureert u de instelling van de iOS-project in Xamarin Studio of Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Push-meldingen toevoegen aan uw iOS-app

1. Open in het project **iOS** AppDelegate.cs de volgende instructie met **behulp van** toevoegen aan het begin van de codebestand.

        using Newtonsoft.Json.Linq;

4. In de klas **AppDelegate** toevoegen een overschrijven voor de gebeurtenis **RegisteredForRemoteNotifications** registreren voor meldingen:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. In **AppDelegate**, moet u ook de volgende overschrijven voor de gebeurtenis-handler **DidReceivedRemoteNotification** toevoegen:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Deze methode omgaat met binnenkomende meldingen terwijl de app wordt uitgevoerd.

2. In de klas **AppDelegate** de volgende code hebt toegevoegd aan de methode **FinishedLaunching** : 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Hierdoor kunnen ondersteuning voor externe meldingen en aanvragen push registratie.

Uw app wordt nu bijgewerkt ter ondersteuning van push-meldingen.

####<a name="test-push-notifications-in-your-ios-app"></a>Test push-meldingen in uw iOS-app

1. Klik met de rechtermuisknop op de iOS-project en klik op **instellen als StartPp voor een Project**.

2. Druk op de knop **uitvoeren** of **F5** in Visual Studio om het project bouwen en start de app op een iOS-apparaat en klik op **OK** om pushmeldingen te accepteren.

    > [AZURE.NOTE] U moet expliciet push-meldingen uit uw app accepteren. Deze aanvraag vindt alleen plaats voor de eerste keer is dat de app wordt uitgevoerd.

3. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.

4. Controleer of dat een melding wordt ontvangen en klik op **OK** om te sluiten van de melding.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Optioneel) Configureren en uitvoeren van de Windows-projecten

In dit gedeelte is voor het uitvoeren van de Xamarin.Forms WinApp en WinPhone81 projecten voor apparaten met Windows. Deze stappen ook ondersteuning voor universele Windows-Platform (UWP) projecten. U kunt dit gedeelte overslaan als u niet met Windows-apparaten werkt.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Uw Windows-app voor push-meldingen hebt geregistreerd met WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>De melding hub voor WNS configureren

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Push-meldingen toevoegen aan uw Windows-app

1. In Visual Studio, **App.xaml.cs** openen in een Windows-project en voeg de volgende instructies voor het **gebruik van** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Vervang `<your_TodoItemManager_portable_class_namespace>` met naamruimte van uw draagbare project met de `TodoItemManager` class.
 

2. In App.xaml.cs toevoegen de volgende **InitNotificationsAsync** methode: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Deze methode wordt het kanaal voor push-meldingen en registreert een sjabloon om de sjabloon berichten van uw hub melding wilt ontvangen. De melding van een sjabloon die ondersteuning biedt voor *messageParam* worden bezorgd bij deze client.

3. Bijwerken in App.xaml.cs, de **OnLaunched** gebeurtenis-handler methodedefinitie door toe te voegen de `async` wijzigingstoets, voegt u de volgende regel met code aan het einde van de methode: 

        await InitNotificationsAsync();

    Dit zorgt ervoor dat de push notification-registratie wordt gemaakt of vernieuwd telkens wanneer de app wordt gestart. Het is belangrijk hiervoor om te garanderen dat het WNS push-kanaal altijd actief is.  

4. In Solution Explorer voor Visual Studio, opent u het bestand **Package.appxmanifest** en **Mailpop kunnen** worden ingesteld op **Ja** onder **meldingen**.

5. De app maken en controleer of er geen fouten.  U client-app moet nu registreren voor de Sjabloonmeldingen van de Mobile-App-end. Herhaal deze sectie voor elk project Windows in uw oplossing.


####<a name="test-push-notifications-in-your-windows-app"></a>Test push-meldingen in uw Windows-app

1. Klik met de rechtermuisknop op een Windows-project Visual Studio, en klikt u op **instellen als opstartproject**.

2. Druk op de knop **uitvoeren** voor het bouwen van het project en start de app.

3. Typ een naam voor een nieuwe todoitem in de app en klik vervolgens op het plusteken (**+**) pictogram toe te voegen.

4. Controleren of een melding is ontvangen wanneer het item wordt toegevoegd.

##<a name="next-steps"></a>Volgende stappen

Meer informatie over push-meldingen:

* [Een diagnose stellen bij problemen van push-meldingen](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Er zijn verschillende redenen waarom meldingen mogelijk krijgen verbroken of naam niet eindigt op apparaten. In dit onderwerp ziet u hoe u analyseren en duidelijk de oorzaak van push-meldingen fouten. 

Houd rekening met doorlopende op een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
Leer hoe u gebruikers van uw app met een identiteitsprovider verifiëren.

* [Offline synchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

