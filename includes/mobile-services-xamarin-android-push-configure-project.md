
1. In de oplossing weergave (of **Solution Explorer** in Visual Studio), met de rechtermuisknop op de map **Components** op **Krijgen meer onderdelen...**, zoek naar het onderdeel **Google Cloud Messaging-Client** en toe te voegen aan het project.

2. Open het projectbestand ToDoActivity.cs en voeg de volgende instructie aan de klasse gebruiken:

        using Gcm.Client;

3. Voeg de volgende nieuwe code in de klas **ToDoActivity** : 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Hiermee kunt u toegang tot het mobiele client-exemplaar van het proces van push-handler service.

4.  Nadat de **MobileServiceClient** is gemaakt, kunt u de volgende code toevoegen met de methode **OnCreate** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Uw **ToDoActivity** is nu voorbereid voor het toevoegen van push-meldingen.