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

#<a name="how-to-integrate-engagement-on-android"></a>Het integreren van betrokkenheid op android-apparaat

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md)

Deze procedure worden de eenvoudigste manier om te activeren betrokkenheid van analyses en functies in uw Android-toepassing voor controle.

> [AZURE.IMPORTANT] De minimale Android SDK-API-niveau moet 10 of hoger (Android 2.3.3 of hoger).

De volgende stappen zijn selecteren om het rapport van logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, loopt en Technicals activeert. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken worden aangepast, zoals gebeurtenissen, fouten en taken moet worden uitgevoerd handmatig de betrokkenheid-API gebruiken (Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw android-apparaat](mobile-engagement-android-use-engagement-api.md) omdat deze statistieken afhankelijk van de toepassing zijn.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>De betrokkenheid SDK en service insluiten in uw project Android

De Android SDK downloaden van [hier](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` en plaatst u deze naar de `libs` map van uw Android-project (de map libs maken als u nog niet bestaat).

> [AZURE.IMPORTANT]
> Als u uw toepassingspakket met ProGuard samenstelt, moet u sommige klassen behouden. U kunt het volgende configuratie-fragment gebruiken:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Geef uw betrokkenheid verbindingsreeks door te bellen van de volgende methode in de activiteit van het startprogramma voor apps:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven op Azure-Portal.

-   Als ontbreekt, de volgende Android machtigingen toevoegen (vóór de `<application>` tag):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   De volgende sectie toevoegen (tussen de `<application>` en `</application>` labels):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Wijziging `<Your application name>` door de naam van uw toepassing.

> [AZURE.TIP] De `android:label` kenmerk kunt u de naam van de service betrokkenheid kiezen, zoals deze wordt weergegeven voor de eindgebruikers in het scherm 'Services uitgevoerd' van hun telefoon. Het wordt aanbevolen om dit kenmerk in te stellen `"<Your application name>Service"` (bijvoorbeeld `"AcmeFunGameService"`).

Geven de `android:process` kenmerk zorgt ervoor dat de betrokkenheid-service wordt uitgevoerd in een eigen proces (betrokkenheid uitgevoerd in hetzelfde proces terwijl uw toepassing dat uw Hoofdgegeven/UI-thread potentieel minder heeft gereageerd zorgt ervoor).

> [AZURE.NOTE] Een code, die u in plaatsen `Application.onCreate()` en andere terugbellen toepassing wordt uitgevoerd voor alle aan uw toepassing processen, inclusief de betrokkenheid-service. Deze mogelijk ongewenste kant-effecten (zoals overbodige geheugentoewijzingen en threads in van de betrokkenheid proces, dubbele broadcast ontvangers of services).

Als u negeren `Application.onCreate()`, het is raadzaam in het volgende codefragment toevoegen aan het begin van uw `Application.onCreate()` functie:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()` en `Application.onConfigurationChanged(...)`.

U kunt ook uitbreiden `EngagementApplication` in plaats van uitbreiden `Application`: de terugbellen `Application.onCreate()` heeft het proces selectievakje en oproepen `Application.onApplicationProcessCreate()` alleen als het huidige proces degene die de betrokkenheid hostingservice is, dezelfde regels voor de andere terugbellen gelden.

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-activity-classes"></a>Aanbevolen methode: overbelastingen uw `Activity` klassen

Om te activeren in het rapport van alle de logboeken aan de betrokkenheid te berekenen van gebruikers, sessies, activiteiten, loopt en technische statistieken vereist, u net hebt om alle uw `*Activity` onderliggend klassen overnemen van de bijbehorende `Engagement*Activity` klassen (bijvoorbeeld als uw oudere activiteiten uitgebreid `ListActivity`, maak deze breidt `EngagementListActivity`).

**Zonder betrokkenheid:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Met betrokkenheid:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Bij gebruik van `EngagementListActivity` of `EngagementExpandableListActivity`, moet u ervoor zorgen dat een bellen naar `requestWindowFeature(...);` bestaat uit de voordat u de oproep door naar `super.onCreate(...);`, anders kan vastlopen zal plaatsvinden.

U vindt deze klassen in de `src` map, en ze in uw project kunt kopiëren. De klassen zijn ook in de **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: bellen `startActivity()` en `endActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `Activity` klassen, u kunt in plaats daarvan starten en te beëindigen van uw activiteiten door te bellen `EngagementAgent`van rechtstreeks methoden.

> [AZURE.IMPORTANT] De Android SDK belt nooit de `endActivity()` methode, zelfs wanneer de toepassing is gesloten (op android-apparaat, toepassingen zijn daadwerkelijk nooit heb afgesloten). Het is dus *ten zeerste* aanbevolen om te bellen de `startActivity()` methode in de `onResume` terugbellen van *alle* uw activiteiten en de `endActivity()` methode in de `onPause()` terugbellen van *alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet worden meer. Als u een sessie is meer, wordt betrokkenheid nooit verbroken door de service van de betrokkenheid-end (sinds de service verbonden blijft zo lang maken als een sessie in behandeling is).

Hier volgt een voorbeeld:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

In dit voorbeeld vergelijkbaar met de `EngagementActivity` klasse en de bijbehorende varianten, waarvan de broncode is opgegeven in de `src` map.

##<a name="test"></a>Test

Controleer of nu integratie met de door de mobiele app in een emulator of ander apparaat en bevestigt dat Hiermee markeert u een sessie op het tabblad beeldscherm.

De volgende secties zijn optioneel.

##<a name="location-reporting"></a>Locatie-rapportage

Als u locaties om te worden gerapporteerd wilt, moet u een paar regels configuratie toevoegen (tussen de `<application>` en `</application>` labels).

### <a name="lazy-area-location-reporting"></a>Rapportage van fikse gebied locatie

Rapportage van fikse gebied locatie kunt rapporteren van het land, regio en plaats die is gekoppeld aan apparaten. Dit type locatie rapportage wordt alleen gebruikt voor netwerklocaties (gebaseerd op de cel-ID of WIFI). Het gebied van het apparaat wordt gerapporteerd maximaal eenmaal per sessie. De GPS is nooit gebruikt en kan dus dit type locatie rapport erg weinig (niet te zeggen Nee) heeft invloed op de kleine.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in een bereik campagnes.

Om te schakelen fikse gebied locatie rapportage, kunt u dit doen met de configuratie die eerder in deze procedure wordt vermeld:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtiging toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

### <a name="real-time-location-reporting"></a>Rapportage van realtime-locatie

Rapportage van realtime-locatie kan rapporteren van de breedte en hoogte die is gekoppeld aan apparaten. Standaard dit type locatie rapportage wordt alleen gebruikt voor netwerklocaties (gebaseerd op de cel-ID of WIFI) en de rapportage is alleen beschikbaar wanneer de toepassing wordt uitgevoerd op voorgrond (dat wil zeggen tijdens een sessie).

Realtime locaties zijn *niet* gebruikt om te berekenen van statistieken. De enige doel is toe te staan dat het gebruik van realtime geografische-fencing \<bereik-publiek-geofencing\> criterium in een bereik campagnes.

Om te schakelen realtime locatie rapportage, kunt u dit doen met de configuratie die eerder in deze procedure wordt vermeld:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtiging toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

#### <a name="gps-based-reporting"></a>GPS gebaseerd rapportage

Realtime locatie rapportage alleen standaard gebaseerd netwerklocaties. Schakel het gebruik van GPS op basis van locaties (dit zijn uiterst nauwkeuriger) door de configuratieobject te gebruiken:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtiging toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Rapportage-achtergrond

Standaard is realtime locatie rapportage alleen actief wanneer de toepassing wordt uitgevoerd op voorgrond (dat wil zeggen tijdens een sessie). Als u wilt de melden ook op de achtergrond inschakelen, gebruikt u het configuratieobject:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wanneer de toepassing wordt uitgevoerd op achtergrond, alleen locaties op basis van een netwerk worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Het rapport van de locatie achtergrond wordt beëindigd als de gebruiker het apparaat opnieuw is opgestart, kunt u deze automatisch opnieuw starten bij het opstarten manier toevoegen:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

U moet ook de volgende machtiging toevoegen als ontbreken:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M-machtigingen

Beginnen met Android M, sommige machtigingen worden beheerd gedurende runtime en goedkeuring van een gebruiker moet.

De machtigingen runtime uitgeschakeld al dan niet standaard voor nieuwe app-installaties als u snel Android API niveau 23. Anders wordt deze ingeschakeld al dan niet standaard.

De gebruiker kan-/ uitschakelen die machtigingen via het menu apparaat-instellingen. Het uitschakelen van machtigingen ten opzicht van het systeemmenu achtergrondprocessen van de toepassing beëindigd, dit is een probleem systeem en heeft geen invloed op de mogelijkheid voor het ontvangen van push op achtergrond.

In de context van Mobile betrokkenheid worden de machtigingen die moeten worden goedgekeurd gedurende runtime:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(alleen wanneer doelgroepen Android API niveau 23 voor dit item)

De externe opslag worden alleen gebruikt voor bereik totaalbeeld functie. Als u vindt deze machtiging storend voor gebruikers vragen, kunt u deze verwijderen als u deze gebruikt alleen voor Mobile betrokkenheid maar maar oplevert totaalbeeld functie uitschakelen.

Voor de functies van de locatie, moet u machtigingen voor een gebruiker met een standaard-dialoogvenster aanvragen. Als de gebruiker goedkeurt, moet u vertellen ``EngagementAgent`` maken van deze wijziging in aanmerking in realtime (anders de wijziging wordt verwerkt de volgende keer dat de gebruiker de toepassing start).

Hier volgt een codevoorbeeld in een activiteit van uw toepassing met machtigingen aanvragen en het resultaat doorsturen als positieve naar ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Geavanceerde rapportage

(Optioneel) als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de betrokkenheid-API gebruiken via de methoden van de `EngagementAgent` class. Een object van deze klasse is opgehaald door de ondersteuning voor de `EngagementAgent.getInstance()` statische methode.

De API betrokkenheid stelt al betrokkenheid van geavanceerde mogelijkheden en wordt beschreven in het gebruik van de API betrokkenheid op android-apparaat (en van de technische documentatie van de `EngagementAgent` class).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Geavanceerde configuratie (in AndroidManifest.xml)

### <a name="wake-locks"></a>Vergrendelingen slaapstand

Als u zorgen wilt dat de statistieken worden verzonden in realtime bij gebruik van Wi-Fi of als het scherm uitgeschakeld is, voegt u de volgende optionele machtiging:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Rapport vastlopen

Als u uitschakelen foutenrapporten wilt, dit toevoegen (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Drempelwaarde voor burst

Standaard logboeken op de betrokkenheid service-rapporten in realtime. Als uw toepassing Logboeken zeer vaak rapporten, is het beter in de buffer de logboeken en om deze te melden in één keer op een gewone tijd grondtal (dit is de modus' burst' genoemd). Hiervoor toevoegen (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

De modus burst iets langere levensduur maar meteen invloed heeft op de Monitor betrokkenheid: alle sessies en taken duur afgerond op de drempelwaarde voor burst (dus sessies en taken korter dan de drempelwaarde voor burst zijn mogelijk niet zichtbaar). Het wordt aanbevolen een burst drempel niet meer dan 30000 (30s) gebruiken.

### <a name="session-timeout"></a>Sessietime-out

Standaard is een sessie beëindigd 10s na het einde van de laatste activiteit (dat meestal door de start of Back-toets te drukken, door in te stellen van de telefoon niet actief geweest of door te gaan in een andere toepassing te). Dit is om te voorkomen dat een sessie-splitsing elk tijd van de gebruiker afsluiten en wilt teruggaan naar de toepassing zeer snel (dat kunt opgetreden wanneer hij een afbeeldings-, verdergaan controleren een melding, enz.). U kunt deze parameter wijzigen. Hiervoor toevoegen (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Log melden uitschakelen

### <a name="using-a-method-call"></a>Middel van een methode-gesprek

Als u betrokkenheid om te stoppen met het verzenden van Logboeken wilt, kunt u bellen:

            EngagementAgent.getInstance(context).setEnabled(false);

In dit gesprek is permanente: site gebruikmaakt van een gedeelde voorkeurenbestand.

Als wanneer u deze functie belt betrokkenheid actief is, duurt 1 minuut voor de service stoppen. Maar deze starten de service helemaal de volgende keer dat Won't starten u de toepassing.

Kunt u rapportage opnieuw door te bellen met dezelfde functie log inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>-Integratie in uw eigen`PreferenceActivity`

In plaats van oproepen van deze functie kunt u ook deze instelling integreren rechtstreeks in uw bestaande `PreferenceActivity`.

U kunt betrokkenheid om uw voorkeurenbestand (met de gewenste modus) te gebruiken de `AndroidManifest.xml` bestand met `application meta-data`:

-   De `engagement:agent:settings:name` sleutel wordt gebruikt voor de naam van het gedeelde voorkeurenbestand definiëren.
-   De `engagement:agent:settings:mode` toets wordt gebruikt voor het definiëren van de modus van het gedeelde voorkeurenbestand, moet u de dezelfde modus als in uw `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van constante vlaggen in uw code gebruikt, controleert u de totale waarde.

Altijd betrokkenheid gebruiken de `engagement:key` Booleaanse toets binnen het voorkeurenbestand voor het beheren van deze instelling.

Het volgende voorbeeld van `AndroidManifest.xml` ziet u de standaardwaarden:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

U kunt toevoegen een `CheckBoxPreference` in de indeling van uw voorkeur zoals de volgende:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
