<properties
    pageTitle="Melding Hubs belangrijk nieuws Zelfstudievideo - Android"
    description="Informatie over het gebruik van Azure Service Bus melding Hubs recente nieuws meldingen naar Android-apparaten stuurt."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Laatste nieuws verzenden via melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u het gebruiken van Azure melding Hubs uitzenden recente nieuws meldingen aan een Android-app. Als alles compleet is, wordt u mogelijk te registreren voor verbreken nieuwscategorieën waarin u geïnteresseerd bent en u ontvangt alleen push-meldingen voor deze categorieën. Dit scenario is een algemene patroon voor veel apps waar meldingen moeten worden verzonden naar groepen gebruikers die eerder rente in deze, bijvoorbeeld RSS-lezer, apps voor muziek ventilatoren, enzovoort zijn gedeclareerd.

Uitzenden scenario's zijn ingeschakeld door een of meer _tags_ op te nemen bij het maken van een registratie in de hub melding. Wanneer meldingen worden verzonden naar een tag, worden alle apparaten die zijn geregistreerd voor de tag de melding ontvangt. Omdat tags gewoon tekenreeksen zijn, hebben ze geen vooraf worden ingericht. Voor meer informatie over de labels, raadpleegt u [de melding Hubs Routering en Tag expressies](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Vereisten voor

In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met melding Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebben uitgevoerd [aan de slag met melding Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande belangrijkste activiteiten waarmee de gebruiker om te selecteren van categorieën om u te registreren. De categorieën die door een gebruiker is geselecteerd zijn op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in de hub melding met de geselecteerde categorieën gemaakt als labels.

1. Opent u het bestand res/layout/activity_main.xml te vervangen door de inhoud met de volgende items:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Open het bestand res/values/strings.xml en voeg de volgende regels:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Uw main_activity.xml grafische indeling ziet er nu als volgt:

    ![][A1]

3. Maak nu een klasse **meldingen** in hetzelfde pakket als uw klas **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Deze klasse gebruikt de lokale opslag voor de opslag van de categorieën van nieuws die dit apparaat heeft ontvangen. De presentatie bevat ook methoden voor het registreren voor deze categorieën.


4. Uw persoonlijke velden voor **NotificationHub** en **GoogleCloudMessaging**verwijderen en een veld toevoegen voor **meldingen**in uw klas **MainActivity** :

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Klik in de methode **onCreate** , verwijdert u de initialisatie van de **hub** -veld en de methode **registerWithNotificationHubs** . De volgende regels die een exemplaar van de klas **meldingen** geïnitialiseerd vervolgens toevoegen. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`en `HubListenConnectionString` al moeten worden ingesteld met de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van de melding en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

    > [AZURE.NOTE] Omdat referenties die worden geleverd met een client-app niet in het algemeen veilige zijn, moet u alleen de sleutel voor beluisteren van access met uw client-app distribueren. Access wordt ingeschakeld uw app om u te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd beluisteren en meldingen kunnen niet worden verzonden. De volledige toegang-toets wordt gebruikt in een beveiligde backend-service voor het verzenden van meldingen en bestaande registraties wijzigen.


6. Vervolgens voegt u de volgende invoer en `subscribe` methode u omgaat met de knop abonneren Klik op gebeurtenis:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Deze methode maakt van een lijst met categorieën en gebruikt de klas **meldingen** aan de lijst opslaan in de lokale opslag en de bijbehorende labels met de melding hub registreren. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in een lokale opslag op het apparaat en registreert met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Register voor meldingen

Volgende stappen uit registreren met de hub melding bij het opstarten gebruik van de categorieën die zijn opgeslagen in de lokale opslag.

> [AZURE.NOTE] Omdat de registratie-id toegewezen door Google Cloud Messaging (GCM) op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat melding fouten. In dit voorbeeld wordt geregistreerd voor melding elke keer dat de app wordt gestart. Apps die vaak worden uitgevoerd voor kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie wilt behouden bandbreedte wanneer minder dan een dag is verstreken sinds de vorige registratie.


1. Voeg de volgende code toe aan het einde van de methode **onCreate** in de klas **MainActivity** :

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Dit zorgt ervoor dat telkens wanneer de app wordt gestart opgehaald van de categorieën uit lokale opslag en daarom een registeration voor deze categorieën. 

2. Werkt u vervolgens de `onStart()` methode van de `MainActivity` klasse als volgt:

    @Overridebeveiligde void onStart() {super.onStart();      isVisible = ONWAAR zijn.

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Hiermee vernieuwt u de belangrijkste activiteit op basis van de status van eerder opgeslagen categorieën.

De app is voltooid en een set met categorieën kunt opslaan in de lokale opslag van apparaat wordt gebruikt om te registreren met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt. Vervolgens definiëren we een backend die categorie meldingen voor deze app kunt verzenden.

##<a name="sending-tagged-notifications"></a>Gelabelde meldingen verzenden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>De app uitvoeren en meldingen genereren

1. Android Studio, de app maken en deze op een apparaat of emulator starten.

    Opmerking dat de UI-app biedt een aantal Hiermee schakelt u waarmee u Kies de categorieën die zich kunnen abonneren op.

2. Hiermee schakelt u een of meer categorieën inschakelen en klik op **abonneren**.

    De app zet om de geselecteerde categorieën in labels en vraagt een nieuwe apparaatregistratie voor de geselecteerde codes van de hub melding. De geregistreerde categorieën worden geretourneerd en in een mailpop-upmelding weergegeven.

4. Een nieuw bericht verzenden door de app .NET-Console uit te voeren.  U kunt ook gelabelde Sjabloonmeldingen op het tabblad foutopsporing van uw hub-melding in de [Klassieke Azure-Portal]verzenden.

    Meldingen voor de geselecteerde categorieën worden weergegeven als mailpop meldingen.

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u het laatste nieuws uitzenden per categorie. Houd rekening met voltooiing van een van de volgende zelfstudies die markeren van andere geavanceerde melding Hubs-scenario's:

+ [Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]

    Leer hoe u de app recente nieuws om in te schakelen verzendende gelokaliseerde meldingen uitvouwen.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Gebruik melding Hubs gelokaliseerde laatste nieuws uitzenden]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klassieke Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
