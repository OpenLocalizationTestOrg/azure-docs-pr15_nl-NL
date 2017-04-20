<properties
    pageTitle="Push-meldingen toevoegen aan Apache Cordova App met Azure mobiele Apps | Azure App-Service"
    description="Leer hoe u push-meldingen naar uw app Apache Cordova verzenden via Azure Mobile-Apps."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Push-meldingen toevoegen aan uw Apache Cordova-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie toevoegen u push-meldingen aan het project [Apache Cordova snel starten] , zodat een push-bericht wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket push melding extensie. Zie [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie behandelt een Apache Cordova-toepassing die is ontwikkeld met Visual Studio-2015 die wordt uitgevoerd op de Android Emulator Google, een Android-apparaat, een Windows-apparaat en een iOS-apparaat.

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

* Een computer met [Visual Studio-Community-2015] of hogere versies.
* [Visual Studio Tools for Apache Cordova].
* Een [actieve Azure-account](https://azure.microsoft.com/pricing/free-trial/).
* Een voltooid [Apache Cordova snel aan de slag] -project.
* (Android) Een [Google-account] met een gecontroleerde e-mailadres.
* (iOS) Een programma voor ontwikkelaars van Apple-lidmaatschap en een iOS-apparaat (iOS Simulator biedt geen ondersteuning push).
* (Windows) Een Windows Store ontwikkelaars-Account en een Windows-10-apparaat.

##<a name="configure-hub"></a>Een melding hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bekijk een video met de stappen in deze sectie](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject als u wilt verzenden push-meldingen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Uw app Cordova om te ontvangen van push-meldingen wijzigen

U moet controleren of uw project van de app Apache Cordova gereed is voor het verwerken van push-meldingen door te installeren de push-invoegtoepassing voor Cordova plus eventuele platform / regiospecifieke push-services.

#### <a name="update-the-cordova-version-in-your-project"></a>De versie Cordova in uw project te werken.

Het is raadzaam om de client-project te werken naar Cordova 6.1.1 als uw project is geconfigureerd met een oudere versie. Als u het project bijwerken, met de rechtermuisknop op config.xml om de configuratie designer te openen. Selecteer het tabblad Platforms en kies 6.1.1 in het tekstvak **Cordova CLI** .

Kies **maken**en klik vervolgens **Build Solution** aan het project bijwerken.

#### <a name="install-the-push-plugin"></a>De push-invoegtoepassing installeren

Apache Cordova toepassingen verwerken apparaat- of netwerkproblemen mogelijkheden niet standaard.  Deze mogelijkheden worden verstrekt door Plug-ins die zijn gepubliceerd op [npm](https://www.npmjs.com/) of op GitHub.  De `phonegap-plugin-push` invoegtoepassing is gebruikt voor het verwerken van netwerk push-meldingen.

U kunt de push-invoegtoepassing installeren op een van de volgende manieren:

**Vanaf de opdrachtprompt:**

Voer de volgende opdracht uit:

    cordova plugin add phonegap-plugin-push

**Van binnen Visual Studio:**

1.  Open in Solution Explorer de `config.xml` bestand klikt u op **Plug-ins** > **aangepaste** **cijfer** selecteert als de installatiebron, voer vervolgens `https://github.com/phonegap/phonegap-plugin-push` als bron.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klik op de pijl naast de installatiebron.

3. In **SENDER_ID**, als u al een numerieke project-ID voor het project Google ontwikkelaars Console, kunt u toevoegen deze hier. Anders, voer een aanduidingswaarde van de tijdelijke, zoals 777777, en als u hebt samengesteld Android kunt u deze waarde in config.xml later bijwerken.

4. Klik op **toevoegen**.

De push-invoegtoepassing is nu geïnstalleerd.

####<a name="install-the-device-plugin"></a>De invoegtoepassing voor apparaat installeren

Voer dezelfde procedure voor installatie van de push-invoegtoepassing hebt gebruikt, maar de invoegtoepassing voor apparaat vindt u in de lijst met Core Plug-ins (Klik op **Plug-ins** > **Core** om deze te vinden). Moet u deze invoegtoepassing voor de naam van de platform (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Uw apparaat gebruiken voor push op opstart registreren

In eerste instantie nemen we enkele minimale code voor Android. Wij doen later enkele kleine wijzigingen moet worden uitgevoerd op iOS of Windows 10.

1. Een oproep toevoegen aan **registerForPushNotifications** tijdens het terugbellen voor de aanmelding of onder aan de methode **onDeviceReady** :

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    In dit voorbeeld toont **registerForPushNotifications** bellen als verificatie is geslaagd, waarin u wordt aangeraden wanneer met zowel push-meldingen en verificatie in uw app.

2. De nieuwe **registerForPushNotifications** methode bijvoegen:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Vervang in de bovenstaande code `Your_Project_ID` project met de numerieke ID voor de app vanuit de [Google ontwikkelaars Console].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Optioneel) Configureren en uitvoeren van de app op android-apparaat

Hiermee voert u deze sectie om in te schakelen push-meldingen voor Android.

####<a name="enable-gcm"></a>Firebase inschakelen Cloud Messaging

Aangezien we zijn het platform Google Android in eerste instantie hebt samengesteld, moet u de Firebase Cloud Messaging inschakelen. Als u Microsoft Windows-apparaten zijn doelgroepen, zou u op dezelfde manier WNS ondersteuning inschakelen.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>De Mobile-App-end om te verzenden push-aanvragen via van FCM configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Uw app Cordova configureren voor Android

In uw app Cordova config.xml openen en te vervangen `Your_Project_ID` project met de numerieke ID voor de app vanuit de [Google ontwikkelaars Console].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Open index.js en bijwerken van de code als u wilt gebruiken uw numerieke project-ID.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Uw Android-apparaat voor foutopsporing USB configureren

Voordat u uw toepassing op uw Android-apparaat implementeren kunt, moet u USB-foutopsporing inschakelen.  De volgende stappen uitvoeren op uw Android-telefoon:

1. Ga naar **Instellingen** > **over telefoon**en vervolgens op het **nummer van de Build** totdat de modus voor ontwikkelaars is ingeschakeld (ongeveer 7 maal).

2. Terug in **Instellingen** > **Opties voor ontwikkelaars** **USB foutopsporing**inschakelen en vervolgens uw Android-telefoon verbinden met uw ontwikkeling PC met een USB-kabel.

We hebben getest dit maken met een Google Nexus 5 X-apparaat met Android 6.0 (Boterbloem).  De technieken zijn echter algemene via een modern Android release.

#### <a name="install-google-play-services"></a>Google Play Services installeren

De push-invoegtoepassing is afhankelijk van Android Google Play Services voor push-meldingen.  

1.  Klik op **Hulpmiddelen voor** **Visual Studio**, > **Android** > **Android SDK Manager**, vouw de map **Extra's** uit en schakel het selectievakje in om ervoor te zorgen dat elk van de volgende SDK's is geïnstalleerd.
    * Android 2.3 of hoger
    * Google-bibliotheek revisie 27 of hoger
    * Google Play Services 9.0.2 of hoger

2.  Klik op **Pakketten installeren** en wacht totdat de installatie te voltooien.

De huidige vereist bibliotheken worden vermeld in de [documentatie van phonegap-Plug-push-installatie].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Test push-meldingen in de app op android-apparaat

U kunt nu test push-meldingen door de app wordt uitgevoerd en het invoegen van items in de tabel TodoItem. U kunt dit doen vanuit hetzelfde apparaat of een tweede apparaat, zo lang maken als u de dezelfde backend gebruikt. Test uw Cordova-app op de Android-platform in een van de volgende manieren:

- **Op een fysiek apparaat:**  
Uw Android-apparaat toevoegen aan de computer met een USB-kabel.  In plaats van **Google Android Emulator**, selecteert u **apparaat**. Visual Studio wordt implementeren van de toepassing op het apparaat en worden uitgevoerd.  Vervolgens kunt u interactief werken met de toepassing op het apparaat.  
Uw ontwikkeling te verbeteren.  Delen van toepassingen, zoals [Mobizen] van een scherm kunt u helpen bij het ontwikkelen van een Android-toepassing door het projecteren van uw Android scherm met een webbrowser op uw PC.

- **Klik op een Android emulator:**  
Er zijn extra configuratiestappen wanneer u zich in een emulator vereist.

    Zorg ervoor dat u naar implementeren of foutopsporing op een virtueel apparaat waarop Google APIs instellen als het doel, zoals hieronder wordt weergegeven in de manager Android virtuele apparaat (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Als u wilt gebruiken, een snellere x86 emulator, u [het stuurprogramma HAXM installeren](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) en configureren van de emulator gebruiken.

    Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**en volg de aanwijzingen voor het toevoegen van een bestaande Google-account bij het apparaat (wordt aangeraden met behulp van een bestaand account, in plaats van een nieuw account maken).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    De takenlijst van-app als voordat u uitvoeren en voeg een nieuw item van de taak. Schakel ditmaal wordt een meldingspictogram voor een weergegeven in het systeemvak. U kunt de melding voor vellen om weer te geven van de volledige tekst van de melding openen.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Optioneel) Configureren en uitvoeren op iOS

In dit gedeelte is voor het uitvoeren van het project Cordova op apparaten met iOS. U kunt dit gedeelte overslaan als u niet met iOS-apparaten werkt.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Installeren en uitvoeren van de iOS-agent remotebuild op een Mac of cloud-service

Voordat u een Cordova-app voor iOS gebruik van Visual Studio uitvoeren kunt, leest u over de stappen in de [installatiehandleiding voor iOS](http://taco.visualstudio.com/en-us/docs/ios-guide/) installeren en uitvoeren van de remotebuild-agent.

Zorg ervoor dat u de app voor iOS kunt maken. De stappen in de installatiehandleiding voor zijn vereist voor iOS van Visual Studio maken. Als u nog geen een Mac, kunt u kunt maken voor gebruik van de remotebuild-agent op een service zoals MacInCloud iOS. Zie [uw iOS-app in de cloud uitvoeren](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/)voor meer informatie.

>[AZURE.NOTE] XCode 7 of hoger is vereist voor het gebruik van de push-invoegtoepassing op iOS.

####<a name="find-the-id-to-use-as-your-app-id"></a>Zoek de ID wilt gebruiken als uw App-ID

Zoek voordat u uw app voor push-meldingen openen config.xml in uw app Cordova hebt geregistreerd de `id` kenmerkwaarde in het element widget en kopieer deze voor later gebruik. In de volgende XML-bestand, de ID is `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Gebruik deze id later, wanneer u een App-ID op van Apple developer portal maken. (Als u een andere App-id hebt op de developer portal maken en u wilt gebruiken die, moet u een paar extra stappen uitvoeren verderop in deze zelfstudie deze ID in config.xml wijzigen. De ID in het element widget moet overeenkomen met de App-ID in de developer portal.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor push-meldingen op van Apple developer portal registreren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Bekijk een video met dezelfde stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Azure als u wilt verzenden push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Controleer of uw App-ID overeenkomt met uw Cordova-app

Als de App-ID die u hebt gemaakt in uw Account van Apple ontwikkelaars al overeenkomt met de ID van het element widget in config.xml, kunt u deze stap overslaan. Als de id's niet overeenkomen, echter de volgende stappen uitvoeren:

1. Verwijder de map platforms uit het project.

2. Verwijder de map Plug-ins uit het project.

3. Verwijder de map node_modules uit het project.

4. Werk het kenmerk id van het element widget in config.xml gebruik van de App-ID die u hebt gemaakt in uw Apple-Account voor ontwikkelaars.

5. Bouw die tabellen opnieuw uw project.

#####<a name="test-push-notifications-in-your-ios-app"></a>Test push-meldingen in uw iOS-app

1. Zorg ervoor dat **iOS** is geselecteerd als het implementatiedoel in Visual Studio, en kies **apparaat** uitvoeren op uw verbonden iOS-apparaat.

    U kunt uitvoeren op een iOS-apparaat dat is verbonden met uw PC met iTunes. De iOS Simulator biedt geen ondersteuning voor push-meldingen.

2. Druk op de knop **uitvoeren** of **F5** in Visual Studio om het project bouwen en start de app op een iOS-apparaat en klik op **OK** om pushmeldingen te accepteren.

    >[AZURE.NOTE] U moet expliciet push-meldingen uit uw app accepteren. Deze aanvraag vindt alleen plaats voor de eerste keer is dat de app wordt uitgevoerd.

3. Typ een taak in de app en klik vervolgens op het plusteken (+) pictogram.

4. Controleer of dat een melding wordt ontvangen en klik op OK om te sluiten van de melding.

##<a name="optional-configure-and-run-on-windows"></a>(Optioneel) Configureren en uitvoeren in Windows

In dit gedeelte is voor het uitvoeren van het project Apache Cordova-app op Windows 10-apparaten (de push-invoegtoepassing voor PhoneGap wordt ondersteund in Windows 10). U kunt dit gedeelte overslaan als u niet met Windows-apparaten werkt.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Uw Windows-app voor push-meldingen hebt geregistreerd met WNS

Als u de opties Store in Visual Studio, selecteert u het doel van een Windows in de lijst oplossing Platforms, zoals **Windows-x64** of **Windows-x86** ( **Windows-/ platform** voor push-meldingen voorkomen).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Bekijk een video met dezelfde stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>De melding hub voor WNS configureren

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Uw app Cordova ter ondersteuning van Windows push-meldingen configureren

Open de ontwerpfunctie voor configuratie (met de rechtermuisknop op config.xml en selecteer **Ontwerpfunctie voor weergaven**), selecteer het tabblad **Windows** en kies **Windows 10** onder **Windows doel-versie**.

>[AZURE.NOTE] Als u een Cordova-versie vóór Cordova 5.1.1 (6.1.1 aanbevolen) gebruikt, moet u ook de mailpop kunnen vlag ingesteld op waar in config.xml.

Ter ondersteuning van push meldingen in uw standaard (foutopsporing) wordt gemaakt, open build.json-bestand. Kopieer de configuratie 'release' de configuratie van uw foutopsporing.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Nadat de update, ziet de voorgaande code er als volgt.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

De app maken en controleer of dat er geen fouten. U client-app moet nu registreren voor de meldingen van de Mobile-App-end. Herhaal deze sectie voor elk project Windows in uw oplossing.

####<a name="test-push-notifications-in-your-windows-app"></a>Test push-meldingen in uw Windows-app

Zorg dat een Windows-platform is geselecteerd als het implementatiedoel, zoals **Windows-x64** of **Windows-x86**in Visual Studio. Kies de app op een PC met Windows 10 hostingprovider Visual Studio uitgevoerd, **Lokale computer**.

Druk op de knop uitvoeren voor het bouwen van het project en start de app.

Typ een naam voor een nieuwe todoitem in de app en klik vervolgens op het plusteken (+) pictogram toe te voegen.

Controleren of een melding is ontvangen wanneer het item wordt toegevoegd.

##<a name="next-steps"></a>Volgende stappen

* Lees meer over [Melding Hubs] voor meer informatie over push-meldingen.
* Als u dit nog niet hebt gedaan, kunt u de zelfstudie door [Verificatie toe te voegen] aan uw app Apache Cordova verder.

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET-Server SDK]
* [Node.js Server SDK]

<!-- URLs -->
[Verificatie toevoegen]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova snel aan de slag]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-account]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google-ontwikkelaars Console]: https://console.developers.google.com/home/dashboard
[phonegap-Plug-push-installatie documentatie]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio-Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Melding Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
