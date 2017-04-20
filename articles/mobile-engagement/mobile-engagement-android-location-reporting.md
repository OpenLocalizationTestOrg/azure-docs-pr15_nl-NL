<properties
    pageTitle="Locatie voor rapportage voor Android SDK Azure mobiele betrokkenheid"
    description="Wordt beschreven hoe u de locatie voor rapportage voor Azure Mobile betrokkenheid Android SDK configureren"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Locatie voor rapportage voor Android SDK Azure mobiele betrokkenheid

> [AZURE.SELECTOR]
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md)

Dit onderwerp wordt beschreven hoe u locatie voor rapportage voor uw Android-toepassing.

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Locatie-rapportage

Als u locaties om te worden gerapporteerd wilt, moet u een paar regels configuratie toevoegen (tussen de `<application>` en `</application>` labels).

### <a name="lazy-area-location-reporting"></a>Rapportage van fikse gebied locatie

Rapportage van fikse gebied locatie, kunt rapportage van het land, regio en plaats die is gekoppeld aan apparaten. Dit type locatie rapportage wordt alleen gebruikt voor netwerklocaties (gebaseerd op de cel-ID of WIFI). Het gebied van het apparaat wordt gerapporteerd maximaal eenmaal per sessie. De GPS is nooit gebruikt en kan dus dit type locatie rapport lage invloed heeft op de kleine.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in een bereik campagnes.

U inschakelen fikse gebied locatie rapportage met behulp van de configuratie die eerder is vermeld in deze procedure:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook een machtiging locatie opgeven. In deze code wordt ``COARSE`` machtiging:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Als uw app vereist, kunt u ``ACCESS_FINE_LOCATION`` in plaats daarvan.

### <a name="real-time-location-reporting"></a>Realtime locatie rapportage

Realtime locatie rapportage, kunt de breedte en hoogte die is gekoppeld aan apparaten rapportage. Standaard wordt dit type locatie rapportage alleen netwerklocaties, op basis van de cel-ID of WIFI gebruikt. De rapportage is alleen actief wanneer de toepassing wordt uitgevoerd op voorgrond (bijvoorbeeld tijdens een sessie).

Realtime locaties zijn *niet* gebruikt om te berekenen van statistieken. De enige doel is toe te staan dat het gebruik van realtime geografische-fencing \<bereik-publiek-geofencing\> criterium in een bereik campagnes.

Toevoegen om in te schakelen realtime locatie rapportage, een regel met code waar u de verbindingsreeks betrokkenheid instellen in de activiteit van het startprogramma voor apps. Het resultaat ziet er als volgt uit:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS gebaseerd rapportage

Realtime locatie rapportage alleen standaard in locaties op basis van een netwerk. Als u wilt inschakelen van het gebruik van GPS gebaseerde locaties, die uiterst nauwkeuriger zijn, de configuratieobject te gebruiken:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtiging toevoegen als ontbreken:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Rapportage-achtergrond

Standaard is realtime locatie rapportage alleen actief wanneer de toepassing wordt uitgevoerd op voorgrond (bijvoorbeeld tijdens een sessie). Als u wilt de melden ook op de achtergrond inschakelen, gebruikt u deze configuratieobject:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wanneer de toepassing wordt uitgevoerd op de achtergrond, alleen op basis van een netwerk locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Als de gebruiker opnieuw is opgestart diens apparaat, wordt het rapport van de locatie achtergrond is gestopt. Als u automatisch opnieuw bij het opstarten wordt gestart, kunt u deze code toevoegen.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

U moet ook de volgende machtiging toevoegen als ontbreken:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M-machtigingen

Beginnen met Android M, sommige machtigingen worden beheerd gedurende runtime en goedkeuring van een gebruiker nodig.

Als u snel Android API niveau 23, zijn de machtigingen runtime zich al dan niet standaard voor nieuwe app-installaties uitgeschakeld. Anders zijn ze standaard ingeschakeld.

U kunt-/ uitschakelen die machtigingen via het menu apparaat-instellingen. Het uitschakelen van machtigingen ten opzicht van het systeemmenu beëindigd de achtergrondprocessen van de toepassing en het systeemgedrag van een, waarna heeft geen invloed op de mogelijkheid voor het ontvangen van push op achtergrond.

In de context van Mobile betrokkenheid locatie rapportage, worden de machtigingen die moeten worden goedgekeurd gedurende runtime:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Machtigingen aanvragen van de gebruiker met een standaard-dialoogvenster. Als de gebruiker goedkeurt, Vertel ``EngagementAgent`` deze wijziging in rekening realtime uitvoeren. De wijziging wordt anders kan de volgende keer dat de gebruiker de toepassing start verwerkt.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
