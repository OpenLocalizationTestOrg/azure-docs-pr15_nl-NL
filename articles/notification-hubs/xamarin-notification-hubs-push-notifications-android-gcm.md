<properties
    pageTitle="Aan de slag met melding Hubs voor Xamarin.Android apps | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen verzenden naar een toepassing voor de Xamarin Android."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Aan de slag met melding Hubs met Xamarin voor Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Deze zelfstudie wordt getoond hoe u met Azure melding Hubs push-meldingen verzenden naar een Xamarin.Android-toepassing.
U kunt een lege Xamarin.Android-app die push-meldingen ontvangt met behulp van Google Cloud Messaging (GCM) wilt maken. Wanneer u klaar bent, kunt u zult gebruikmaken van uw hub melding uitzenden van push-meldingen naar alle apparaten die uw app uitvoeren. De code die klaar is beschikbaar in de [app NotificationHubs] [ GitHub] voorbeeld.

Deze zelfstudie ziet u de eenvoudige broadcast scenario in de melding Hubs gebruiken.


## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kunt u vinden op GitHub [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ Visual Studio met Xamarin in Windows of Xamarin Studio op Mac OS x voltooid installatie-instructies zijn in de [installatie en installeer voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Actieve Google-account
+ [Azure onderdeel Messaging]
+ [Onderdeel van Google Cloud Messaging-Client]

Voltooien van deze zelfstudie is vereist voor alle andere meldingen Hubs zelfstudies voor Xamarin.Android-apps.

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F)voor meer informatie.

##<a name="enable-google-cloud-messaging"></a>Google Cloud Messaging inschakelen

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>De melding hub configureren

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Klik op het tabblad <b>configureren</b> aan de bovenkant, voert u de <b>API-sleutel</b> waarde die u in de vorige sectie hebt aangeschaft en klik vervolgens op <b>Opslaan</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Uw hub melding is nu geconfigureerd voor het werken met GCM en u de verbindingstekenreeksen tot beide registreren van uw app-meldingen wilt ontvangen en kunt sturen push-meldingen hebt.

##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de melding hub

###<a name="create-a-new-project"></a>Een nieuw project maken

1. Xamarin Studio, klikt u op **Nieuwe oplossing**, klikt u op **Android-App**en klik op **volgende**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Voer uw **App-naam** en **id**. Klik op het **Doel Plaforms** moet worden ondersteund en klik vervolgens op **volgende** en **maken**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Hiermee maakt u een nieuw project voor Android.

2. Open de Projecteigenschappen van het door met de rechtermuisknop op het nieuwe project in de weergave van de oplossing en **Opties**te kiezen. Selecteer het item **Android toepassing** in de sectie **maken** .

    Zorg ervoor dat de eerste letter van uw **pakketnaam** kleine letters is.

    > [AZURE.IMPORTANT] De eerste letter van de pakketnaam moet kleine letters. U ontvangt manifest toepassingsfouten anders wanneer u uw **BroadcastReceiver** en **IntentFilter** hebt geregistreerd voor push-meldingen hieronder.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Stel desgewenst de **Minimum Android versie** naar een andere API niveau.

4. Stel desgewenst de **doeltoepassing Android versie** naar de andere API-versie die u wilt de doelgroep (moet API niveau 8 of hoger).

Klik op **OK** en sluit het dialoogvenster Projectopties.


###<a name="add-the-required-components-to-your-project"></a>De vereiste onderdelen toevoegen aan uw project

De Google Cloud Messaging-Client beschikbaar op de Xamarin onderdeel Store eenvoudiger ondersteunende push-meldingen in Xamarin.Android.

1. Met de rechtermuisknop op de map Components in Xamarin.Android-app en kies **Meer onderdelen ophalen**.

2. Zoek het onderdeel **Azure Messaging** en toe te voegen aan het project.

3. Zoek het onderdeel **Google Cloud Messaging-Client** en toe te voegen aan het project.


###<a name="set-up-notification-hubs-in-your-project"></a>Melding hubs in uw project instellen

1. Verzamel de volgende informatie voor uw Android-app en melding hub:

    - **GoogleProjectNumber**: deze waarde projectnummer ophalen uit het overzicht van uw app op de Google Developer Portal. U hebt een notitie van deze waarde eerder wanneer u de app op de portal gemaakt.
    - **Luister verbindingsreeks**: klik op het dashboard in de [Klassieke Azure-Portal], op **weergave verbindingstekenreeksen**. Kopieer de verbindingsreeks *DefaultListenSharedAccessSignature* voor deze waarde.
    - **De naam van de hub**: dit is de naam van uw hub in de [Klassieke Azure-Portal]. Bijvoorbeeld: *mynotificationhub2*.

    Maak een klasse **Constants.cs** voor uw project Xamarin en definiëren van de volgende constante waarden in de klas. De tijdelijke aanduidingen vervangen door de waarden.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Voeg de volgende beweringen naar **MainActivity.cs**gebruiken:

        using Android.Util;
        using Gcm.Client;

3. Een variabele van het exemplaar toevoegen de `MainActivity` klasse die wordt gebruikt voor het weergeven van een dialoogvenster Waarschuwing wanneer de app wordt uitgevoerd:

        public static MainActivity instance;


3. De volgende methode in de klas **MainActivity** maken:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. In de `OnCreate` methode van **MainActivity.cs**, geïnitialiseerd de `instance` variabele en een oproep toevoegen aan `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Maak een nieuwe klasse, **MyBroadcastReceiver**.

    > [AZURE.NOTE] We doorlopen die een klasse **BroadcastReceiver** helemaal onderstaande maken. Een snelle alternatief voor het maken van handmatig **MyBroadcastReceiver.cs** is echter om te verwijzen naar het bestand **GcmService.cs** in het voorbeeld Xamarin.Android project wordt geleverd bij de [NotificationHubs voorbeelden][GitHub]. Dupliceren **GcmService.cs** en het wijzigen van klassenamen is een prima plek om te beginnen als u ook.

5. Voeg de volgende instructies naar **MyBroadcastReceiver.cs** (verwijzen naar het onderdeel en constructie die u eerder hebt toegevoegd):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. In **MyBroadcastReceiver.cs**, de volgende toestemmingsaanvragen tussen het **gebruik van** -instructies en declaratie van de **naamruimte** items toevoegen:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. In **MyBroadcastReceiver.cs**, wijzigt u de klas **MyBroadcastReceiver** zodat deze overeenkomen met het volgende:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Een andere klasse toevoegen aan **MyBroadcastReceiver.cs** **PushHandlerService**, dat is afgeleid van **GcmServiceBase**met de naam. Controleer of **het kenmerk** toepassen op de klas:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** implementeert methoden **OnRegistered()**, **OnUnRegistered()** **OnMessage()**, **OnRecoverableError()**en **OnError()**. Onze **PushHandlerService** implementatieklasse deze methoden moet overschrijven en deze methoden in antwoord op de interactie met de hub melding wordt geactiveerd.


9. De methode **OnRegistered()** in **PushHandlerService** overschrijven met behulp van de volgende code:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] In de **OnRegistered()** -code hierboven, moet u de mogelijkheid om op te geven van tags om u te registreren voor specifieke SMS kanalen opmerking.

10. De methode **OnMessage** in **PushHandlerService** overschrijven met behulp van de volgende code:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. De volgende methoden voor **createNotification** en **dialogNotify** toevoegen aan **PushHandlerService** voor gebruikers hoogte te stellen wanneer er wordt een melding ontvangen.

    >[AZURE.NOTE] Melding ontwerpen in Android versie 5.0 en hoger vertegenwoordigt een aanzienlijk vertrek uit van eerdere versies. Als u dit op Android 5.0 of hoger test, moet de app worden uitgevoerd als de melding wilt ontvangen. Zie [Android meldingen](http://go.microsoft.com/fwlink/?LinkId=615880)voor meer informatie.

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Abstracte leden **OnUnRegistered()**, **OnRecoverableError()**en **OnError()** overschrijven zodat uw code wordt gecompileerd:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Uw app uitvoeren in de emulator

Als u deze app in de emulator uitvoert, zorg dat u gebruikt een Android virtuele apparaat (AVD) die ondersteuning biedt voor Google APIs.

> [AZURE.IMPORTANT] Push-meldingen ontvangen, moet u een Google-account instellen op uw apparaat met Android virtuele. (In de emulator, Ga naar **Instellingen** en klik op **Account toevoegen**.) Zorg ook dat de emulator is verbonden met Internet.

>[AZURE.NOTE] Melding ontwerpen in Android versie 5.0 en hoger vertegenwoordigt een aanzienlijk vertrek uit van eerdere versies. Zie [Android meldingen](http://go.microsoft.com/fwlink/?LinkId=615880)voor meer informatie.


1. In **Hulpmiddelen voor**, klikt u op **Open Android Emulator Manager**, selecteer uw apparaat en klik vervolgens op **bewerken**.

    ![][18]

2. **Google-API's** in **doel**selecteren en klik vervolgens op **OK**.

    ![][19]

3. Klik op de bovenste werkbalk op **uitvoeren**en selecteer vervolgens de app. Hiermee wordt de emulator gestart en wordt de app wordt uitgevoerd.

  De app de *registratie-id* uit GCM zijn opgehaald en registreert met de hub melding.

##<a name="send-notifications-from-your-backend"></a>Meldingen verzenden vanuit uw back-end


U kunt testen meldingen ontvangen in uw app door te sturen van meldingen in de [Klassieke Azure-Portal] via het tabblad foutopsporing op de hub melding, zoals wordt weergegeven in het volgende scherm.

![][30]


Push-meldingen worden meestal verzonden in een back-end-service zoals Mobile Services of ASP.NET via een compatibele bibliotheek. U kunt ook de REST API rechtstreeks naar de meldingsberichten verzenden als een bibliotheek niet beschikbaar voor uw back-end is.

Hier volgt een lijst met enkele andere zelfstudies die u bekijken wilt mogelijk voor het verzenden van meldingen:

- ASP.NET: Zie [Gebruik melding Hubs push-meldingen aan gebruikers].
- Azure melding Hubs Java SDK: Zie [How to melding Hubs uit Java gebruiken](notification-hubs-java-push-notification-tutorial.md) voor het verzenden van meldingen van Java. Dit is getest in Eclips voor Android ontwikkeling.
- PHP: Zie [het gebruik van de melding Hubs van PHP](notification-hubs-php-push-notification-tutorial.md).


In de volgende subsecties van de zelfstudie verzenden u meldingen via een .NET-console-app en met behulp van een mobiele service via een script knooppunt.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Optioneel) Meldingen verzenden via een .NET-app

In dit gedeelte worden we meldingen verzonden via een .NET-console-app

1. Maak een nieuwe Visual C# consoletoepassing:

    ![][20]

2. Klik op **Hulpmiddelen voor**Visual Studio, op **NuGet Package Manager**en klik vervolgens op **Package Manager-Console**.

    Hiermee worden de Package Manager-Console in Visual Studio weergegeven.

3. Klik in het venster Package Manager Console instellen met de **project standaard** naar uw nieuwe console-toepassingsproject en klik vervolgens in het consolevenster Voer de volgende opdracht uit:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de Azure melding Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Open het bestand Program.cs en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

5. In uw `Program` klasse, voegt u de volgende methode. Werk de tijdelijke aanduiding voor tekst met uw *DefaultFullSharedAccessSignature* tekenreeks en hub verbindingsnaam in de [Klassieke Azure-Portal].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Voeg de volgende regels in uw methode **Main** :

         SendNotificationAsync();
         Console.ReadLine();

7. Druk op F5 om uit te voeren van de app. U kunt een bericht moet krijgen in de app.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Optioneel) Meldingen verzenden via een mobiele service

1. Ga als volgt [aan de slag met mobiele Services].

1. Meld u aan bij de [Klassieke Azure-Portal]en selecteer uw telefoonprovider.

2. Selecteer het tabblad **Scheduler** op de voorgrond.

    ![][22]

3. Een nieuwe geplande taak maakt, Voeg een naam en selecteer **op aanvraag**.

    ![][23]

4. Wanneer de taak is gemaakt, klikt u op de taaknaam. Klik vervolgens op het tabblad **Script** op de bovenste balk.

5. Voeg in het volgende script binnen uw functie scheduler. Zorg ervoor dat de tijdelijke aanduidingen vervangen door de naam van de hub van de melding en de verbindingsreeks, voor *DefaultFullSharedAccessSignature* die u eerder hebt gekregen. Klik op **Opslaan**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klik **Eenmaal uitvoeren** op de onderste balk. U kunt een mailpop-upmelding moet ontvangen.

##<a name="next-steps"></a>Volgende stappen

In dit voorbeeld eenvoudige uitgezonden u meldingen voor al uw Android-apparaten. Om u te richten op specifieke gebruikers, raadpleegt u de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers]. Als u wilt uw gebruikers door rente groepen segmenten, vindt u [Gebruik melding Hubs naar het laatste nieuws verzenden]. Meer informatie over het gebruik van de melding Hubs in de [Melding Hubs richtlijnen] en in de [Melding Hubs stapsgewijze procedures voor Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Aan de slag met mobiele Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure klassieke Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Melding Hubs richtlijnen]: http://msdn.microsoft.com/library/jj927170.aspx
[Melding Hubs procedures voor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Gebruik van de melding Hubs om push-meldingen aan gebruikers]: /manage/services/notification-hubs/notify-users-aspnet
[Laatste nieuws verzenden via melding Hubs]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Onderdeel van Google Cloud Messaging-Client]: http://components.xamarin.com/view/GCMClient/
[Azure onderdeel Messaging]: http://components.xamarin.com/view/azure-messaging
