<properties
    pageTitle="Het gebruik van de betrokkenheid API op android-apparaat"
    description="Meest recente Android SDK - het gebruik van de betrokkenheid API op android-apparaat"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Het gebruik van de betrokkenheid API op android-apparaat

In dit document is een invoegtoepassing voor [Geavanceerde opties voor Android mobiele betrokkenheid SDK](mobile-engagement-android-advanced-reporting.md)van het document. Deze biedt diepteas details over het gebruik van de API betrokkenheid rapporteren van de toepassing statistieken.

Houd er rekening mee dat als u alleen betrokkenheid rapporteren van uw toepassing sessies, activiteiten, loopt en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is alle uw `Activity` onderliggend klassen overnemen van de bijbehorende `EngagementActivity` class.

Als u wilt doen meer, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u moet rapporteren van uw toepassing activiteiten op een andere manier dan de geïmplementeerd in de `EngagementActivity` klassen, moet u de betrokkenheid-API gebruiken.

De API betrokkenheid is verstrekt door de `EngagementAgent` class. Een exemplaar van deze klasse kan worden opgehaald door de ondersteuning voor de `EngagementAgent.getInstance(Context)` statische methode (Houd er rekening mee dat het `EngagementAgent` object dat het resultaat is een singleton).

##<a name="engagement-concepts"></a>Betrokkenheid concepten

De volgende onderdelen Verfijn de algemene [Mobiele betrokkenheid concepten](mobile-engagement-concepts.md)voor de Android-platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Als de gebruiker meer dan een paar seconden inactief tussen twee *activiteiten blijft*, klikt u vervolgens zijn volgorde van *activiteiten* splitsen in twee afzonderlijke *sessies*. Deze enkele seconden worden de 'sessietime' genoemd.

Een *activiteit* wordt meestal veroorzaakt door één scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit het geval is, wanneer u de betrokkenheid SDK is geïntegreerd met behulp van de `EngagementActivity` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de betrokkenheid-API worden beheerd. In deze sectie geeft het splitsen van een bepaald scherm in verschillende sub delen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld tot bekende hoe vaak en hoe lang dialoogvensters worden gebruikt in dit scherm).

##<a name="reporting-activities"></a>Rapportage van activiteiten

> [AZURE.IMPORTANT] U hoeft niet te rapport activiteiten zoals beschreven in deze sectie als u de `EngagementActivity` klasse en de bijbehorende varianten zoals wordt beschreven in de How to betrokkenheid integreren op Android-document.

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

U moet bellen `startActivity()` telkens wanneer de gebruiker activiteit wijzigingen. De eerste aanroep voor deze functie wordt een nieuwe gebruikerssessie gestart.

De beste plaats om te bellen van deze functie is op elke activiteit `onResume` terugbellen.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt haar huidige activiteit

            EngagementAgent.getInstance(this).endActivity();

U moet bellen `endActivity()` ten minste eenmaal wanneer de gebruiker is voltooid zijn laatste activiteit. Hiermee wordt de betrokkenheid SDK gemeld dat de gebruiker momenteel niet actief is en dat de gebruikerssessie moet worden afgesloten eenmaal de sessietime verloopt (als u belt `startActivity()` voordat de sessietime verloopt, de sessie gewoon wordt hervat).

De beste plaats om te bellen van deze functie is op elke activiteit `onPause` terugbellen.

##<a name="reporting-events"></a>Rapportage van gebeurtenissen

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties uitgevoerd door een gebruiker tijdens zijn sessie.

**Voorbeeld zonder extra gegevens:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Voorbeeld met extra gegevens:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot sessiegebeurtenissen, kunt zelfstandige gebeurtenissen buiten de context van een sessie.

**Voorbeeld:**

Stel dat u wilt rapport-gebeurtenissen die optreden wanneer een broadcast ontvanger wordt geactiveerd:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Rapportage van fouten

### <a name="session-errors"></a>Sessie fouten

Sessie fouten worden meestal gebruikt om de fouten die invloed hebben op de gebruiker tijdens zijn sessie.

**Voorbeeld:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Zelfstandige versie van fouten

In tegenstelling tot sessie fouten, kunnen zich voordoen zelfstandige fouten buiten de context van een sessie.

**Voorbeeld:**

Het volgende voorbeeld ziet u het rapporteren van een fout wanneer het geheugen laag op de telefoon wordt, terwijl uw toepassingsproces actief is.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Taken rapporteren

### <a name="example"></a>Voorbeeld

Stel dat u wilt rapporteren van de duur van uw aanmeldingsproces:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Rapportfouten gedurende een project

Fouten kunnen zijn gerelateerd aan een lopend project in plaats van dat wordt gerelateerd aan de sessie van de huidige gebruiker.

**Voorbeeld:**

Stel dat u wilt melden een fout tijdens u zich aanmeldt proces:

[...] Aanmelding bij openbare void (Context context,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Rapportage van gebeurtenissen tijdens een taak

Gebeurtenissen kunnen zijn gerelateerd aan een lopend project in plaats van dat wordt gerelateerd aan de sessie van de huidige gebruiker.

**Voorbeeld:**

Stel dat we hebben een sociaal netwerk en we een taak voor het rapport gebruiken de totale tijd waarin de gebruiker is verbonden met de server. De gebruiker kunt houden op de achtergrond even wanneer hij een andere toepassing gebruikt of wanneer de telefoon is slapende, dus er geen sessie is.

De gebruiker van zijn vrienden kunt ontvangen, is dit een taakgebeurtenis.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Extra parameters

Willekeurige gegevens kan worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden onderverdeeld, de site gebruikmaakt van Android bundel class (werkelijk, werkt als extra parameters in Android die). Houd er rekening mee dat een bundel matrices of een ander bundel gevallen kan bevatten.

> [AZURE.IMPORTANT] Als u hebt opgeslagen in de parcelable of serialiseerbaar parameters, controleert u of hun `toString()` methode is geïmplementeerd om terug te keren een leesbare tekenreeks. Serialiseerbaar klassen die niet tijdelijke velden bevatten die niet serialiseerbaar zijn brengt Android vastlopen wanneer u belt`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Verspreid matrices in extra parameters worden niet ondersteund, dat wil zeggen won't worden serienummer als een matrix. U moet deze converteren naar gewone matrices voordat u deze in extra parameters.

### <a name="example"></a>Voorbeeld

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per gesprek (één keer gecodeerd in JSON door de service betrokkenheid).

In het vorige voorbeeld is de JSON verzonden naar de server 58 tekens bevatten:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informatie over toepassingen melden

U kunt handmatig melden voor het bijhouden van informatie (of een andere toepassing nauwkeurige gegevens) Gebruik de `sendAppInfo()` functie.

Opmerking deze informatie stapsgewijs kan worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel voor een bepaald apparaat worden bewaard.

Zoals gebeurtenis extra's, is de klas bundel gebruikt voor informatie over toepassingen abstracte, houd er rekening mee dat matrices of onderliggend pakketten wordt behandeld als platte tekenreeksen (via JSON serialisatie).

### <a name="example"></a>Voorbeeld

Hier volgt een codevoorbeeld gebruiker geslacht en geboortedatum verzenden:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Toepassingsinformatie zijn beperkt tot **1024** tekens per gesprek (één keer gecodeerd in JSON door de service betrokkenheid).

In het vorige voorbeeld is de JSON verzonden naar de server 44 tekens bevatten:

            {"expiration":"2016-12-07","status":"premium"}
