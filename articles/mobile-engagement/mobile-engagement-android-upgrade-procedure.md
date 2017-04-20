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


#<a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

U moet mogelijk verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 1.4.0 naar 1.6.0 moet u eerst de procedure 'van 1.4.0 naar 1.5.0"Volg vervolgens de procedure 'van 1.5.0 naar 1.6.0' bijvoorbeeld.

Ongeacht de versie die u een van upgrade, die u hebt voor het vervangen van de `mobile-engagement-VERSION.jar` door het nieuwe bereik.

##<a name="from-420-to-421"></a>Uit 4.2.0 naar 4.2.1

Deze stap daadwerkelijk op alle versies van de SDK kan worden uitgevoerd, is het een verbetering beveiliging als u bereik activiteiten integreren.

U moet nu toevoegen `exported="false"` op alle bereik activiteiten.

Bereik activiteiten ziet er nu als volgt op uw `AndroidManifest.xml`:

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

##<a name="from-400-to-410"></a>Uit 4.0.0 naar 4.1.0

Het SDK nu greep nieuwe machtiging model Android M.

Als u locatie functies gebruiken of Lees [dit gedeelte](mobile-engagement-android-integrate-engagement.md#android-m-permissions)totaalbeeld meldingen.

Naast het nieuwe machtigingsniveau model ondersteunen we nu configureren locatie functies gedurende runtime.
We nog steeds compatibel zijn met de manifest parameters voor locatie, maar deze nu afgeschaft. Als u wilt gebruiken runtime configuratie, verwijderen in de volgende secties van uw ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

en Lees [Deze bijgewerkte procedure](mobile-engagement-android-integrate-engagement.md#location-reporting) runtime configuratie in plaats daarvan wordt gebruikt.

##<a name="from-300-to-400"></a>Uit 3.0.0 naar 4.0.0

### <a name="native-push"></a>Systeemeigen push

Systeemeigen push (GCM/ADM) is nu ook gebruikt voor in de app-meldingen zodat u de systeemeigen push-referenties voor elk gewenst type push campaign moet configureren.

Als dit nog niet Volg [deze procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Integratie van het bereik is gewijzigd in ``AndroidManifest.xml``.

Deze vervangen:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Door

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Er is mogelijk een scherm laden nu wanneer u op een aankondiging (met tekst/webinhoud) of een peiling klikt.
U moet dit voor deze campagnes om te werken in 4.0.0 toevoegen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Resources

De nieuwe insluiten `res/layout/engagement_loading.xml` bestand in uw project.

##<a name="from-240-to-300"></a>Uit 2.4.0 naar 3.0.0

Het volgende beschreven hoe u het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app mogelijk gemaakt door Azure Mobile betrokkenheid. Als u vanuit een eerdere versie migreert, neem contact op met de Capptain-website om te migreren naar 2.4.0 eerst en pas de volgende procedure.

>[AZURE.IMPORTANT] Capptain en Mobile betrokkenheid zijn niet dezelfde services en het migreren van de client-app in de onderstaande procedure alleen worden gemarkeerd. Migreren van de SDK in de app migreert geen gegevens van de Capptain-servers met de betrokkenheid van de Mobile-servers.

### <a name="jar-file"></a>OPPERVLAK-bestand

Vervang `capptain.jar` door `mobile-engagement-VERSION.jar` in uw `libs` map.

### <a name="resource-files"></a>Bronbestanden

Elke resource-bestand dat wordt geleverd (voorafgegaan door `capptain_`) te worden vervangen door de nieuwe bestanden (voorafgegaan door `engagement_`).

Als u de bestanden hebt aangepast, hebt u uw aanpassingen aan de nieuwe bestanden, **alle id's in de resource-bestanden zijn ook hernoemd**opnieuw toepassen.

### <a name="application-id"></a>Toepassings-ID

Betrokkenheid wordt nu een verbindingsreeks gebruikt voor het configureren van de SDK-id's zoals de toepassings-id.

U moet gebruiken `EngagementAgent.init` methode in uw activiteiten starten als volgt:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven op Azure-Portal.

Verwijder eventuele doorschakelen naar `CapptainAgent.configure` als `EngagementAgent.init` vervangt deze methode.

De `appId` kan niet meer worden geconfigureerd met `AndroidManifest.xml`.

Verwijder dit gedeelte uit uw `AndroidManifest.xml` als u deze hebt:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java-API

Elke aanroep voor alle klassen Java van onze SDK heeft moet worden gewijzigd; bijvoorbeeld `CapptainAgent.getInstance(this)` naam moet worden gewijzigd `EngagementAgent.getInstance(this)`, `extends CapptainActivity` naam moet worden gewijzigd `extends EngagementActivity` enzovoort...

Als u zijn geïntegreerd met bestanden met gebruikersvoorkeuren die standaard agent, de standaardbestandsnaam is nu `engagement.agent` en de sleutel `engagement:agent`.

Wanneer u web aankondigingen maakt, de Javascript-binder is nu `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Een groot aantal wijzigingen is er gebeurd er, de service niet meer wordt gedeeld en een groot aantal ontvangers kunnen niet worden geëxporteerd meer.

De declaratie van de service is nu veel eenvoudiger; verwijdert de domeinbeheerpagina van filteren en alle metagegevens hierin en voegt u `exportable=false`.

Plus alles gewijzigd betrokkenheid gebruiken.

Deze ziet er nu zo:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Als u testen Logboeken inschakelen wilt, wordt de metagegevens nu is verplaatst naar de toepassing-code en heeft een andere naam gekregen:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Alle andere metagegevens zijn alleen hernoemd, volgt de volledige lijst (met de naam van de cursus alleen de kleuren die u gebruikt):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play en SmartAd bijhouden van wijzigingen is verwijderd uit SDK die u zojuist moet u deze verwijderen zonder teruglegging:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

De activiteiten van het bereik worden nu als volgt gedefinieerd:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Als u aangepaste bereik activiteiten hebt, moet u alleen de domeinbeheerpagina van acties zodat deze overeenkomen met een wijzigen `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` of `com.microsoft.azure.engagement.reach.intent.action.POLL`.

De broadcast ontvangers hebt gekregen, plus we nu toevoegen `exported=false`. Hier is de volledige lijst van de ontvangers met de nieuwe specificatie, (van de naam van de cursus alleen de kleuren die u gebruikt):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Het bijhouden van de ontvanger is verwijderd, zodat u hoeft te verwijderen in deze sectie:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Opmerking die de declaratie van uw implementatie van de broadcast ontvanger **EngagementMessageReceiver** zijn gewijzigd in de `AndroidManifest.xml`. Dit komt omdat de API voor het verzenden en willekeurige XMPP berichten verwijderen uit willekeurige XMPP entiteiten en de API verzenden en ontvangen van berichten tussen apparaten zijn verwijderd. Dus moet u ook aan de volgende terugbellen verwijderen uit uw **EngagementMessageReceiver** -implementatie:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

en

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Verwijder vervolgens elke oproep op **EngagementAgent** voor:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

en

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard configuratie kan worden beïnvloed door rebranding, de regels zijn nu ziet eruit als:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
