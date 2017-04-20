<properties
    pageTitle="Push-meldingen verzenden naar Chrome-apps gebruiken met Azure melding Hubs | Microsoft Azure"
    description="Informatie over het gebruik van Azure melding Hubs push-meldingen verzenden naar een Chrome-App."
    services="notification-hubs"
    keywords="mobiele push-meldingen, push-meldingen, push-meldingen, push-meldingen van chrome"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Push-meldingen verzenden naar Chrome-apps gebruiken met Azure melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In dit onderwerp ziet u hoe u push-meldingen naar een Chrome-App, dat wordt weergegeven in het kader van de Google Chrome-browser verzenden via Azure melding Hubs. In deze zelfstudie gaan we een Chrome-app die push-meldingen ontvangt met behulp van [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/)maken. 

>[AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F)voor meer informatie.

De zelfstudie begeleidt u bij deze basisstappen voor het inschakelen van push-meldingen:

* [Google Cloud Messaging inschakelen](#register)
* [De melding hub configureren](#configure-hub)
* [Uw App Chrome verbinden met de melding hub](#connect-app)
* [Een push-bericht verzenden naar uw App Chrome](#send)
* [Extra functionaliteit en mogelijkheden](#next-steps)

>[AZURE.NOTE] Chrome app push-meldingen worden niet algemene browser meldingen: deze uniek zijn voor het uitbreiden van de browser model (Zie [Overzicht van de Chrome-Apps] voor meer informatie). Naast de browser bureaublad uitvoeren Chrome apps via Apache Cordova Mobile (Android en iOS). Zie [Chrome Apps Mobile] voor meer informatie.

Configureren GCM en Azure melding Hubs is gelijk aan het configureren voor Android, aangezien [Google Cloud Messaging voor Chrome] is afgeschaft en de dezelfde GCM nu zowel Android-apparaten en Chrome exemplaren ondersteunt.

##<a id="register"></a>Google Cloud Messaging inschakelen

1. Navigeer naar de [Google Cloud Console] -website, meld u aan met de referenties van uw Google-account en klik vervolgens op de knop **Project maken** . Geef een **Naam van het Project**en klik vervolgens op de knop **maken** .

    ![Google Cloud Console - Project maken][1]

2. Maak een notitie van het **Project getal** op de pagina **projecten** voor het project dat u zojuist hebt gemaakt. U kunt dit als de **GCM afzender-ID** in de Chrome-App met GCM registreren.

    ![Google Cloud-Console - projectnummer][2]

3. In het linkerdeelvenster, klik op **API's & auth**, schuif naar beneden en klik op de wisselknop om in te schakelen **Google Cloud Messaging voor Android**. U hoeft niet te **Google Cloud Messaging voor Chrome**inschakelen.

    ![Google Cloud-Console - Server-toets][3]

4. Klik in het linkerdeelvenster op **referenties** > **Maken van nieuwe sleutel** > **Sleutel Server** > **maken**.

    ![Google Cloud-Console - referenties][4]

5. Maak een notitie van de server **API-sleutel**. Configureert u deze in de hub melding vervolgens als u wilt deze push-meldingen verzenden naar GCM inschakelen.

    ![Google Cloud-Console - API-sleutel][5]

##<a id="configure-hub"></a>De melding hub configureren

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. Selecteer in het blad **Instellingen** **Notification Services** en **Google (GCM)**. Voer de API-sleutel en opslaan.

&emsp;&emsp;![Azure melding Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Uw App Chrome verbinden met de melding hub

Uw hub melding is nu geconfigureerd voor het werken met GCM en u de verbindingstekenreeksen registreren van uw app als u wilt zowel ontvangen en versturen van push-meldingen hebt. LK

###<a name="create-a-new-chrome-app"></a>Een nieuwe Chrome-App maken

Het onderstaande voorbeeld is gebaseerd op de [Chrome App GCM steekproef] en gebruikt het beste om een Chrome-toepassing te maken. We worden de stappen die specifiek betrekking heeft op Azure melding Hubs gemarkeerd. 

>[AZURE.NOTE] Het is raadzaam dat u de bron voor deze App Chrome uit [Chrome App melding Hub voorbeeld]downloaden.

De Chrome-App is gemaakt via JavaScript en u een van uw voorkeur word editors voor het maken van deze kunt gebruiken. Hieronder ziet u hoe deze App Chrome eruitziet.

![Google Chrome-App][15]

1. Een map maken en noem deze `ChromePushApp`. De naam is natuurlijk willekeurige: als u noem deze iets anders, controleert u of u vervangt door het pad tussen de vereiste code segmenten.

2. Download de [cryptografische-js-bibliotheek] in de map die u in de tweede stap hebt gemaakt. Deze bibliotheekmap bevat twee submappen: `components` en `rollups`.

3. Maak een `manifest.json` bestand. Alle Chrome-Apps worden ondersteund door een manifest bestand met de app-metagegevens en, vooral is van belang dat alle machtigingen die zijn toegewezen aan de app wanneer de gebruiker is geïnstalleerd.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Kennisgeving de `permissions` element, geeft aan dat deze App Chrome push-meldingen ontvangen van GCM. De Azure melding Hubs URI waar de Chrome-App wordt bellen REST registreren moet ook opgeven.
    Ons voorbeeld-app wordt ook gebruikt voor een pictogrambestand, `gcm_128.png`, die wordt vindt u in de bron die opnieuw uit de oorspronkelijke GCM voorbeeld gebruikt wordt. U kunt dit ook voor een afbeelding die voldoet aan de [criteria pictogram](https://developer.chrome.com/apps/manifest/icons).

4. Maken van een bestand met de naam `background.js` met de volgende code:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Dit is het bestand dat verschijnt de Chrome-appvenster HTML (**register.html**) en definieert ook de handler **messageReceived** voor het verwerken van de binnenkomende push-meldingen.

5. Maken van een bestand met de naam `register.html` -Hiermee definieert u de gebruikersinterface van de Chrome-App. 

   >[AZURE.NOTE] In dit voorbeeld wordt **CryptoJS v3.1.2**gebruikt. Als u een andere versie van de bibliotheek hebt gedownload, controleert u of u goed vervangt door de versie in de `src` pad.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Maken van een bestand met de naam `register.js` met de onderstaande code. Dit bestand Hiermee geeft u het script achter `register.html`. Chrome-Apps toestaan niet inline uitvoering, zodat u moet een afzonderlijke back-script maken voor de gebruikersinterface.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Het bovenstaande script heeft de volgende belangrijke parameters:
    - **Window.OnLoad** Hiermee definieert u de knop terwijl u klikt op gebeurtenissen van de twee knoppen in de gebruikersinterface. Een registreert bij GCM en de andere gebruikt de registratie-ID die wordt geretourneerd na registratie met GCM registreren bij Azure melding Hubs.
    - **updateLog** is de functie waarmee wij u omgaat met eenvoudige logboekregistratie mogelijkheden.
    - **registerWithGCM** is de eerste knop terwijl u klikt op handler, waardoor u de `chrome.gcm.register` bellen naar GCM registreren van het huidige exemplaar van de Chrome-App.
    - **registerCallback** is de functie voor terugbellen die wordt aangeroepen wanneer de registratie-oproep GCM geeft als resultaat.
    - **registerWithNH** is de tweede knop terwijl u klikt op handler, bij melding Hubs registreert. Dit wordt `hubName` en `connectionString` (die de gebruiker is opgegeven) en de melding Hubs registratie REST API-oproep crafts.
    - **splitConnectionString** en **generateSaSToken** zijn hulpprogramma's waarmee de JavaScript-implementatie van een SA's token maakproces, die moet worden gebruikt in alle REST API-oproepen. Zie [Algemene begrippen](http://msdn.microsoft.com/library/dn495627.aspx)voor meer informatie.
    - **sendNHRegistrationRequest** is de functie die u aanbrengt in de REST van een HTTP bellen Azure melding Hubs.
    - **registrationPayload** Hiermee definieert u de registratie XML-nettolading. Zie [Registratie NH REST API maken]voor meer informatie. We bijwerken de registratie-ID in deze met wat wij van GCM ontvangen.
    - **client** is een exemplaar van **XMLHttpRequest** die we gebruiken om de HTTP POST-aanvraag te starten. Opmerking die we bijwerken de `Authorization` koptekst met `sasToken`. Succesvolle afronding van dit gesprek wordt dit exemplaar van de Chrome-App met Azure melding Hubs registreren.


De algehele mapstructuur voor dit project moet er dan ongeveer als volgt:     ![Google Chrome-App - mapstructuur][21]

###<a name="set-up-and-test-your-chrome-app"></a>Instellen en testen van de Chrome-App

1. Open de Chrome-browser. Open **Chrome extensies** en inschakelen van de **modus voor ontwikkelaars**.

    ![Google Chrome - modus voor ontwikkelaars inschakelen][16]

2. Klik op **uitgepakt extensie laden** en navigeer naar de map waar u de bestanden hebt gemaakt. U kunt desgewenst ook de **Chrome-Apps en uitbreidingen ontwikkelaars hulpmiddel**gebruiken. Dit programma is een Chrome-App in zelf (hebt geïnstalleerd vanaf de webwinkel Chrome) en biedt geavanceerde mogelijkheden voor foutopsporing voor de ontwikkeling van de Chrome-Apps.

    ![Google Chrome - uitgepakt extensie laden][17]

3. Als de Chrome-App is gemaakt zonder fouten, ziet u de Chrome-App wordt weergegeven.

    ![Google Chrome - weergave van Chrome-App][18]

4. Het **Projectnummer** dat u eerder hebt u vanuit de **Google Cloud Console** als de afzender-ID, en klik op **registeren GCM**. Ziet u het bericht **registratie met GCM is geslaagd.**

    ![Google Chrome - Chrome App aanpassing][19]

5. Voer uw **Melding hubnaam** en de **DefaultListenSharedAccessSignature** die u eerder van de portal gekregen en klikt u op **registreren met Azure melding Hub**. Ziet u het bericht **melding de registratie van de Hub is geslaagd!** en de details van de registratie-antwoord waarin de Azure melding Hubs registratie-id 's

    ![Google Chrome - melding Hub Details opgeven][20]  

##<a name="send"></a>Een bericht verzenden naar uw Chrome-App

Voor testdoeleinden ontvangt ons Chrome push-meldingen met behulp van een .NET console toepassing. 

>[AZURE.NOTE] U kunt push-meldingen met melding Hubs verzenden van een end via onze openbare <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST API interface</a>. Bekijk onze [documentatie portal](https://azure.microsoft.com/documentation/services/notification-hubs/) voor meer voorbeelden van meerdere platforms.

1. Selecteer in het menu **bestand** in Visual Studio, **Nieuw** en klik vervolgens op **Project**. Klik onder **Visual C#**, op **Windows** en **Console-toepassing**, en klik vervolgens op **OK**.  Hiermee maakt u een nieuw project van console-toepassing.

2. Klik op **Bibliotheek Package Manager** en klik vervolgens op **Package Manager-Console**in het menu **Extra** . Hiermee worden de Package Manager-Console weergegeven.

3. Voer de volgende opdracht in het consolevenster:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de SDK Azure Service Bus met het <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet-pakket</a>.

4. Open `Program.cs` en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

5. In de `Program` klasse, voegt u de volgende methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Vervang de `<hub name>` tijdelijke aanduiding met de naam van de hub melding die wordt weergegeven in de [portal](https://portal.azure.com) in uw blade melding Hub. Vervang de tijdelijke aanduiding voor tekenreeks verbinding ook met de verbindingsreeks genoemd `DefaultFullSharedAccessSignature` die u in de sectie melding hub configuratie hebt gekregen.

    >[AZURE.NOTE] Zorg ervoor dat u de verbindingsreeks met **volledige** toegang, access niet **beluisteren gebruikt** . De verbindingsreeks **beluisteren** toegang verleent geen machtigingen voor het verzenden van push-meldingen.

5. Toevoegen van de volgende oproepen in de `Main` methode:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Zorg ervoor dat Chrome wordt uitgevoerd en voert u de consoletoepassing.

7. U moet de volgende melding pop-upvenster op uw bureaublad zien.

    ![Google Chrome - melding][13]

8. U kunt alle meldingen voor uw ook bekijken met behulp van het venster Chrome berichten op de taakbalk (in Windows) als Chrome wordt uitgevoerd.

    ![Google Chrome - meldingen lijst][14]

>[AZURE.NOTE] U hoeft niet te de Chrome-App wordt uitgevoerd of openen in de browser (Hoewel de Chrome-browser zelf moet worden uitgevoerd). U kunt ook een gecombineerde weergave van alle meldingen voor uw openen in het venster Chrome berichten.

## <a name="next-steps"> </a>Vervolgstappen

Meer informatie over melding Hubs in [Systeemvak Hubs overzicht].

Als u wilt afstemmen op specifieke gebruikers, raadpleegt u de zelfstudie [Azure melding Hubs hoogte gebruikers] . 

Als u uw gebruikers door rente groepen segmenten wilt, kunt u de zelfstudie [Azure melding Hubs belangrijk nieuws] volgen.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App melding Hub steekproef]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud-Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Melding Hubs overzicht]: notification-hubs-push-notification-overview.md
[Overzicht van de chrome-Apps]: https://developer.chrome.com/apps/about_apps
[Voorbeeld van de App GCM chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-Apps Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Registratie NH REST API maken]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[cryptografische-js-bibliotheek]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging voor Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure melding Hubs moet gebruikers laten weten]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure melding Hubs laatste nieuws]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
