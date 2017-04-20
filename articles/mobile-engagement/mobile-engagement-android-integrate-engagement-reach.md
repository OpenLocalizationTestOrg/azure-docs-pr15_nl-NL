<properties
    pageTitle="Azure mobiele betrokkenheid Android SDK-integratie"
    description="Meest recente updates en procedures voor Android SDK voor Azure Mobile betrokkenheid"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Het integreren van betrokkenheid bereik op android-apparaat

> [AZURE.IMPORTANT] U moet de integratie procedure wordt beschreven in de How to betrokkenheid integreren op Android document voordat u deze handleiding volgen.

##<a name="standard-integration"></a>Standaard-integratie

De SDK hebt bereikt is vereist voor de **ondersteuning voor Android-bibliotheek (v4)**.

De snelste manier om de bibliotheek toevoegen aan uw project in **Eclips** is `Right click on your project -> Android Tools -> Add Support Library...`.

Als u niet Eclips gebruikt, kunt u de instructies lezen [hier].

Kopieer bereik resource bestanden uit de SDK in uw project:

-   Kopieer de bestanden uit de `res/layout` map bezorgd bij de SDK in de `res/layout` map van uw toepassing.
-   Kopieer de bestanden uit de `res/drawable` map bezorgd bij de SDK in de `res/drawable` map van uw toepassing.

Bewerken uw `AndroidManifest.xml` bestand:

-   De volgende sectie toevoegen (tussen de `<application>` en `</application>` labels):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Moet u deze machtiging op opnieuw afspelen systeemmeldingen die niet is geklikt tijdens het opstarten (anders ze op schijf worden bewaard, maar niet meer weergegeven, moet u echt opnemen).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Geef een pictogram voor meldingen (beide in de app en systeem bestanden) wordt gebruikt door te kopiëren en bewerken van de volgende sectie (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] In dit gedeelte is **verplicht** als u van plan systeemmeldingen gebruiken bent bij het maken van bereik campagnes. Android-apparaat kunt u voorkomen dat systeemmeldingen zonder pictogrammen die wordt weergegeven. Dus als u deze sectie weglaat, kunnen uw eindgebruikers niet ontvangen.

-   Als u campagnes met systeemmeldingen met totaalbeeld maakt, moet u de volgende machtigingen toevoegen (nadat de `</application>` tag) als ontbreken:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Klik op Android M en als Android API niveau 23 of hoger, is bedoeld voor uw toepassing ``WRITE_EXTERNAL_STORAGE`` machtiging gebruikersgoedkeuring van de is vereist. Lees [dit gedeelte](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Voor systeemmeldingen kunt u ook opgeven in het bereik campagne als het apparaat moet bellen en/of trillen. Voor deze om te werken, die u hebt om ervoor te zorgen dat u de volgende machtiging gedeclareerd (nadat de `</application>` tag):

            <uses-permission android:name="android.permission.VIBRATE" />

    Zonder deze machtiging kan Android voorkomt dat systeemmeldingen worden weergegeven als u de bellen of de optie vibrate in de campagne hebt bereikt manager gecontroleerd.

-   Als u uw toepassing met **ProGuard** maken en hebt fouten die zijn gerelateerd aan de bibliotheek Android ondersteuning of het oppervlak betrokkenheid, voegt u de volgende regels aan uw `proguard.cfg` bestand:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Systeemeigen Push

Nu u bereik module hebt geconfigureerd, moet u systeemeigen push moeten kunnen ontvangen van de campagnes op het apparaat configureren.

We ondersteund twee services op android-apparaat:

  - Google Play apparaten: Gebruik [Google Cloud Messaging] aan de hand van de handleiding voor [het GCM integreren met de betrokkenheid handleiding](mobile-engagement-android-gcm-integrate.md) .
  - Amazon-apparaten: Gebruik [Amazon apparaat Messaging] aan de hand van de handleiding voor [het ADM integreren met de betrokkenheid handleiding](mobile-engagement-android-adm-integrate.md) .

Als u zowel Amazon als Google Play apparaten, zijn mogelijk hebt alles in 1 AndroidManifest.xml/APK voor ontwikkeling wilt. Maar bij het verzenden van Amazon, kunnen ze uw toepassing negeren als ze GCM code vinden.

U moet meerdere APKs in dat geval gebruiken.

**Uw toepassing is nu klaar voor ontvangen en bereik campagnes weergeven!**

##<a name="how-to-handle-data-push"></a>Hoe u omgaat met gegevens push

### <a name="integration"></a>Integratie

Als u wilt dat uw toepassing moeten kunnen bereiken gegevens ladingen ontvangen, u moet maken van een onderliggend klasse van `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` en de verwijzing in de `AndroidManifest.xml` bestand (tussen de `<application>` en/of `</application>` labels):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Vervolgens kunt u overschrijven de `onDataPushStringReceived` en `onDataPushBase64Received` terugbellen. Hier volgt een voorbeeld:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Categorie

De categorieparameter is optioneel wanneer u een campagne Push-gegevens maken en kunt dat u gegevens filteren verplaatst. Dit is handig als u hebt verschillende broadcast ontvangers verwerking van verschillende soorten gegevens gereedschap duwt, of als u wilt verschillende typen push van `Base64` gegevens en u wilt identificeren van het type voordat ze worden geparseerd.

### <a name="callbacks-return-parameter"></a>De terugbellen geretourneerde parameter

Hier volgen enkele richtlijnen alleen goed verwerken de return-parameter van `onDataPushStringReceived` en `onDataPushBase64Received`:

-   Een broadcast ontvanger moet worden geretourneerd `null` in de terugbellen als deze niet hoe weet u omgaat met een push gegevens. U moet de categorie gebruiken om te bepalen of uw broadcast ontvanger de push gegevens of niet verwerken moet.
-   Een van de broadcast ontvanger moet worden geretourneerd `true` in de terugbellen als dit de push gegevens accepteert.
-   Een van de broadcast ontvanger moet worden geretourneerd `false` in de terugbellen als deze de push gegevens herkent, maar deze om welke reden dan ook negeert. Zo terug `false` wanneer de ontvangen gegevens zijn ongeldig.
-   Als een ontvanger resulteert uitzenden `true` terwijl een andere een geeft `false` voor de push met dezelfde gegevens, het gedrag ongedefinieerd is, moet u dit nooit doen.

Het retourtype worden alleen gebruikt voor de statistieken bereiken:

-   `Replied`Als een van de broadcast ontvangers hetzij geretourneerd wordt verhoogd `true` of `false`.
-   `Actioned`alleen als een van de broadcast ontvangers geretourneerd wordt verhoogd `true`.

##<a name="how-to-customize-campaigns"></a>Het aanpassen van campagnes

Als u wilt aanpassen campagnes, kunt u de indelingen die beschikbaar zijn in de SDK hebt bereikt.

U moet alle id's gebruikt in de indelingen bewaren en de typen van de weergaven waarmee een id, met name voor tekst en afbeelding weergaven behouden. Sommige weergaven worden alleen gebruikt voor het verbergen of weergeven gebieden zodat hun type kan worden gewijzigd. Controleer de broncode als u wilt wijzigen van het type van een weergave in de meegeleverde indelingen.

### <a name="notifications"></a>Meldingen

Er zijn twee soorten meldingen: systeem en app-meldingen die andere lay-out bestanden gebruiken.

#### <a name="system-notifications"></a>Systeemmeldingen

Als u wilt aanpassen systeemmeldingen die u wilt gebruiken de **categorieën**. U kunt naar [categorieën](#categories)gaan.

#### <a name="in-app-notifications"></a>App-meldingen

Een melding voor de app is standaard een weergave die dynamisch is toegevoegd aan de huidige activiteit gebruikersinterface waardering voor de Android-methode `addContentView()`. Dit is een melding overlay genoemd. Melding overlays zijn zeer geschikt voor een snelle integratie omdat ze niet dat u vereisen voor het wijzigen van een indeling in uw toepassing.

Als u wilt wijzigen van het uiterlijk van uw melding-overlays, kunt u gewoon het bestand wijzigen `engagement_notification_area.xml` aan uw wensen voldoet.

> [AZURE.NOTE] Het bestand `engagement_notification_overlay.xml` degene die wordt gebruikt voor het maken van een melding overlay, waaronder het bestand `engagement_notification_area.xml`. U kunt ook aanpassen aan uw behoeften (zoals voor het plaatsen van het systeemvak binnen de overlay).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Indeling voor melding als onderdeel van de indeling van een activiteit opnemen

Overlays kunnen zijn zeer geschikt voor een snelle integratie, maar onhandig of bijwerkingen in speciale gevallen hebben. De overlay-systeem kan worden aangepast op een activiteitenniveau van de, zodat u gemakkelijk kunt voorkomen dat bijwerkingen voor speciale activiteiten.

U kunt de indeling van onze melding opnemen in uw bestaande indeling met behulp van de instructie Android **opnemen** . De volgende is een voorbeeld van een gewijzigde `ListActivity` indeling die alleen bevat een `ListView`.

**Voordat u betrokkenheid integratie:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Na betrokkenheid integratie:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

In dit voorbeeld hebben we een bovenliggende container toegevoegd omdat de oorspronkelijke indeling een lijstweergave bekijken als het hoogste niveau-element gebruikt. We ook toegevoegd `android:layout_weight="1"` moeten kunnen hieronder een lijstweergave die is geconfigureerd met een weergave toevoegen `android:layout_height="fill_parent"`.

De betrokkenheid hebt bereikt SDK wordt automatisch gedetecteerd dat de melding-indeling is opgenomen in deze activiteit en een overlay voor deze activiteit geen worden toegevoegd.

> [AZURE.TIP] Als u een ListActivity in uw toepassing gebruikt, een zichtbaar bereik overlay wordt voorkomen dat u reageren op meer items in de lijstweergave geklikt. Dit is een bekend probleem. U kunt dit probleem omzeilen wordt u aangeraden u om in te sluiten van de melding-indeling in de indeling van uw eigen lijst met activiteiten zoals in het vorige voorbeeld.

##### <a name="disabling-application-notification-per-activity"></a>Melding van de toepassing per activiteit uitschakelen

Als u niet dat de overlay wilt moet worden toegevoegd aan uw activiteiten en als u niet de indeling melding in uw eigen indeling opnemen, kunt u de overlay voor deze activiteit in uitschakelen de `AndroidManifest.xml` door toe te voegen een `meta-data` sectie zoals in het volgende voorbeeld:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Categorieën

Wanneer u de meegeleverde indelingen wijzigt, kunt u het uiterlijk van alle uw meldingen wijzigen. Categorieën kunnen u verschillende gerichte weergaven (mogelijk gedrag) definiëren voor meldingen. Een categorie kan worden opgegeven bij het maken van een bereik campagne. Houd er rekening mee dat categorieën kunnen u ook aanpassen aankondigingen en polls, dat wordt beschreven verderop in dit document.

Als u wilt een categorie-handler voor uw meldingen hebt geregistreerd, moet u een oproep toevoegen wanneer de toepassing wordt geïnitialiseerd.

> [AZURE.IMPORTANT] Lees de waarschuwing over het kenmerk android: proces \<android-sdk-betrokkenheid-process\> in de How to integreren betrokkenheid op Android onderwerp voordat u verdergaat.

Het volgende voorbeeld wordt ervan uitgegaan dat u de vorige waarschuwing bevestigd en gebruiken van een onderliggend klasse van `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

De `MyNotifier` object is de implementatie van de melding categorie-handler. Dit is een van beide een implementatie van de `EngagementNotifier` interface of een subklasse van de standaardimplementatie: `EngagementDefaultNotifier`.

Houd er rekening mee dat het hetzelfde kennisgever opgedeeld in verschillende categorieën kunt afhandelen, kunt u deze registreren als volgt:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Als u wilt de uitvoering van de categorie standaard vervangen, kunt u uw implementatie zoals registreren in het volgende voorbeeld:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

De huidige categorie gebruikt in een handler wordt doorgegeven als een parameter in de meeste methoden die u kunt negeren in `EngagementDefaultNotifier`.

Wordt doorgegeven als een `String` parameter of indirect in een `EngagementReachContent` object waarvoor een `getCategory()` methode.

U kunt de grootste deel van het proces voor het maken van melding wijzigen door het opnieuw definiëren methoden op `EngagementDefaultNotifier`, je mag rustig gaat u naar de technische documentatie en klik in de broncode voor meer geavanceerde aanpassingen.

##### <a name="in-app-notifications"></a>App-meldingen

Als u alleen alternatieve indelingen gebruiken voor een bepaalde categorie wilt, kunt u dit kunt implementeren zoals in het volgende voorbeeld:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Voorbeeld van `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Zoals u ziet, wordt de id van de weergave overlay verschilt dan de standaardlocatie. Het is belangrijk dat elke indeling een unieke identificatiecode voor overlays gebruiken.

**Voorbeeld van `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Zoals u ziet, wordt de id van de melding gebied weergave verschilt dan de standaardlocatie. Het is belangrijk dat elke indeling gebruikt met een unieke identificatiecode voor melding gebieden.

Dit eenvoudige voorbeeld van categorie wordt toepassing (of app) meldingen aan de bovenkant van het scherm weergegeven. We hebt de standaard-id die wordt gebruikt in het systeemvak zelf geen gewijzigd.

Als u wijzigen wilt, moet u definiëren de `EngagementDefaultNotifier.prepareInAppArea` methode. Het is raadzaam om te zoeken op de technische documentatie en op de broncode van `EngagementNotifier` en `EngagementDefaultNotifier` desgewenst kunt u dit niveau van geavanceerde aanpassingen.

##### <a name="system-notifications"></a>Systeemmeldingen

Te verlengen `EngagementDefaultNotifier`, u kunt negeren `onNotificationPrepared` te wijzigen van de melding die door de standaardimplementatie is voorbereid.

Bijvoorbeeld:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

In dit voorbeeld wordt de Systeemmelding van een voor een inhoud wordt weergegeven als een lopend gebeurtenis wanneer de categorie "lopende" wordt gebruikt.

Als u wilt samenstellen de `Notification` object maken, kunt u terugkeren `false` naar de methode en gesprek `notify` zelf op de `NotificationManager`. In dat geval het is belangrijk dat u behouden een `contentIntent`, een `deleteIntent` en de melding-id die wordt gebruikt door `EngagementReachReceiver`.

Hier volgt een juiste voorbeeld van een dergelijke implementatie:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Melding alleen aankondigingen

Het beheer van de Klik op een melding dat alleen aankondiging kan zo worden aangepast door te overschrijven `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` voor het wijzigen van het voorbereide `Intent`. Deze methode gebruikt, kunt u de vlaggen eenvoudig af te stellen.

Bijvoorbeeld om toe te voegen de `SINGLE_TOP` vlag:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Voor gebruikers van oudere betrokkenheid, houd er rekening mee dat systeemmeldingen zonder actie URL nu de toepassing start als deze op de achtergrond, was zodat deze methode kan worden aangeroepen met een aankondiging zonder actie-URL. U overwegen dat bij het aanpassen van de bedoeling.

U kunt ook implementeren `EngagementNotifier.executeNotifAnnouncementAction` maken.

##### <a name="notification-life-cycle"></a>De levenscyclus van de melding

Wanneer u met de standaardcategorie, sommige methoden levenscyclus worden genoemd in de `EngagementReachInteractiveContent` object naar rapport statistieken en de status van de campagne bijwerken:

-   Wanneer de melding wordt weergegeven in de toepassing of deze op de statusbalk, de `displayNotification` methode (dat statistieken) wordt aangeroepen door `EngagementReachAgent` als `handleNotification` geeft als resultaat `true`.
-   Als de melding wordt geannuleerd, de `exitNotification` methode wordt aangeroepen, statistische gemeld en volgende campagnes kunnen nu worden verwerkt.
-   Als u de melding klikt, `actionNotification` wordt genoemd, statistische gemeld en de bijbehorende bedoeling wordt gestart.

Als uw implementatie van `EngagementNotifier` omzeilt het standaardgedrag, moet u deze methoden levenscyclus bellen door zelf. De volgende voorbeelden laten zien sommige gevallen waarin het standaardgedrag is overgeslagen:

-   U kunt niet uitbreiden `EngagementDefaultNotifier`, bijvoorbeeld u categorie afhandeling helemaal geïmplementeerd.
-   Voor systeemmeldingen, overrode u de `onNotificationPrepared` en u aangepast `contentIntent` of `deleteIntent` in de `Notification` object.
-   Voor app-meldingen u overrode `prepareInAppArea`, moet u ten minste toewijzen `actionNotification` naar een van uw U.I besturingselementen.

> [AZURE.NOTE] Als `handleNotification` genereert een uitzondering, de inhoud wordt verwijderd en `dropContent` wordt genoemd. Dit gemeld in de statistiek en volgende campagnes kunnen nu worden verwerkt.

### <a name="announcements-and-polls"></a>Aankondigingen en polls

#### <a name="layouts"></a>Indelingen

Kunt u de `engagement_text_announcement.xml`, `engagement_web_announcement.xml` en `engagement_poll.xml` bestanden om aan te passen tekst aankondigingen, web aankondigingen en polls.

Deze bestanden delen twee algemene indelingen voor de titel en de knop Display. De indeling voor de titel is `engagement_content_title.xml` en het eponymous drawable bestand voor de achtergrond wordt gebruikt. De indeling voor de knoppen actie en afsluiten is `engagement_button_bar.xml` en het eponymous drawable bestand voor de achtergrond wordt gebruikt.

In een peiling, de indeling van de vraag en hun keuzes worden dynamisch opgepompt enkele malen met de `engagement_question.xml` indelingsbestand voor de vragen en de `engagement_choice.xml` bestand voor de opties.

#### <a name="categories"></a>Categorieën

##### <a name="alternate-layouts"></a>Alternatieve indelingen

Zoals meldingen, kan de campagne categorie worden gebruikt om te laten alternatieve indelingen voor uw aankondigingen en polls.

Bijvoorbeeld een als categorie wilt maken voor een aankondiging tekst, kunt u uitbreiden `EngagementTextAnnouncementActivity` en deze verwijst naar de `AndroidManifest.xml` bestand:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Houd er rekening mee dat de categorie in het domeinbeheerpagina filter wordt gebruikt voor het maken van het verschil met de standaard aankondiging activiteit.

De SDK hebt bereikt beschikt over de domeinbeheerpagina van systeem voor het oplossen van de juiste activiteit voor een bepaalde categorie en deze weer op de standaardcategorie valt als de resolutie is mislukt.

Hebt u willen implementeren `MyCustomTextAnnouncementActivity`, desgewenst kunt u alleen de indeling wijzigen (maar onthoud dat de dezelfde weergave-id's) u hoeft te definiëren de klas zoals in het volgende voorbeeld:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Als u wilt vervangen de standaardcategorie van tekst aankondigingen, gewoon vervangen `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` door uw implementatie.

Web aankondigingen en polls kunnen worden aangepast op soortgelijke wijze.

Voor aankondigingen van het web kunt u uitbreiden `EngagementWebAnnouncementActivity` en declareren van uw activiteit in de `AndroidManifest.xml` zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Voor polls kunt u uitbreiden `EngagementPollActivity` en declareren uw in het `AndroidManifest.xml` zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementatie maken

U kunt ook categorieën voor uw activiteiten aankondiging (en peiling) implementeren zonder uit te breiden een van de `Engagement*Activity` klassen verstrekt door de SDK hebt bereikt. Dit is bijvoorbeeld handig als u wilt definiëren van een indeling die geen van de weergaven van dezelfde als de standaard-indelingen gebruikmaakt.

Zoals voor geavanceerde melding aanpassingen, wordt het aanbevolen om te zoeken op de broncode van de standaard-implementatie.

Hier zijn enkele dingen in gedachten moet houden: bereik de activiteit met een specifieke bedoeling (overeenkomt met het domeinbeheerpagina filter) plus een extra parameter waarin de inhoud-id wordt gestart.

Als u wilt ophalen van de inhoud object waarin de velden die u hebt opgegeven bij het maken van de campagne op de website kunt u dit doen:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Voor statistieken, moet u melden de inhoud wordt weergegeven in de `onResume` gebeurtenis:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Vervolgens vergeet niet om te bellen hetzij `actionContent(this)` of `exitContent(this)` op het inhoud object voordat de activiteit in achtergrond.

Als u niet een belt `actionContent` of `exitContent`, statistieken niet verzonden (dat wil zeggen geen analyses van de campagne) en echter de volgende campagnes worden hierover niet geïnformeerd totdat het toepassingsproces opnieuw is opgestart.

Afdrukstand of andere configuratiewijzigingen kunnen aanbrengen de code lastig zijn om te bepalen of de activiteit tot in achtergrond doorloopt al dan niet, de standaard-implementatie wordt gecontroleerd of de inhoud wordt gemeld als de gebruiker de activiteit verlaat afgesloten (hetzij door te drukken `HOME` of `BACK`), maar niet als de afdrukstand wijzigt.

Als volgt het interessante deel van de implementatie:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Zoals u ziet, als u gebeld `actionContent(this)` en klaar bent met de activiteit, `exitContent(this)` veilig kan worden aangeroepen zonder geen effect.

[Hier]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon-apparaat Messaging]:https://developer.amazon.com/sdk/adm.html
