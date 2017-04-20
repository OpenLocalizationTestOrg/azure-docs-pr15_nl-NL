<properties
    pageTitle="Aan de slag met Azure mobiele betrokkenheid voor Cordova/Phonegap"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en Push-meldingen voor Cordova/Phonegap apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Aan de slag met Azure mobiele betrokkenheid voor Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u het gebruiken van Azure Mobile betrokkenheid voor meer informatie over het appgebruik van uw en het verzenden van push-meldingen aan gesegmenteerde gebruikers van een mobiele toepassing ontwikkeld met Cordova.

In deze zelfstudie we maakt u een lege Cordova-app met behulp van Mac en klikt u vervolgens integreren Mobile betrokkenheid SDK. Deze eenvoudige analytics-gegevens worden verzameld en Apple Push Notification-systeem (APNS) gebruiken voor iOS en Google Cloud Messaging (GCM) voor Android push-meldingen ontvangt. We zullen dit dashboard implementeren naar een iOS of Android-apparaat voor het testen van. 

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started)voor meer informatie.

Deze zelfstudie is het volgende nodig:

+ XCode, die u via de Mac App Store installeren kunt (voor het implementeren voor iOS)
+ [Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (voor het implementeren voor Android)
+ Push-meldingen certificaat (.p12) die u vanaf de Apple ontwikkelaar Center voor APNS kan downloaden
+ GCM projectnummer die u vanaf uw Google-Console voor ontwikkelaars voor GCM kan downloaden
+ [Mobiele betrokkenheid Cordova-invoegtoepassing](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] U vindt de broncode en het Leesmij-bestand voor de invoegtoepassing voor Cordova op [Github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Mobiele betrokkenheid van de instelling voor de app Cordova

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Uw app verbinden met de betrokkenheid van de Mobile-end

Deze zelfstudie biedt een 'eenvoudige integratie"dat wil de minimale instellen vereist zeggen voor het verzamelen van gegevens en een push-bericht verzenden. 

We gaan een eenvoudige app maken met Cordova om te laten zien van de integratie:

###<a name="create-a-new-cordova-project"></a>Een nieuw Cordova project maken

1. *Terminal* -venster op uw Mac-computer starten en typ het volgende voorbeeld waarin een nieuw Cordova project uit de standaardsjabloon maakt. Zorg ervoor dat het publicerende profiel dat u uiteindelijk kunt Implementeer de app iOS 'com.mycompany.myapp' wordt gebruikt als de App ID. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. De volgende handelingen uit om uw project voor **iOS** configureren en te worden uitgevoerd in de iOS Simulator uitvoeren:

        $ cordova platform add ios 
        $ cordova run ios

3. De volgende handelingen uit als u wilt configureren van uw project voor **Android** en worden uitgevoerd in de Android emulator uitvoeren. Zorg ervoor dat uw Android SDK Emulator-instellingen het doel als Google APIs (Google Inc.) met de CPU hebben / ABI als Google APIs ARM.  

        $ cordova platform add android
        $ cordova run android

4.  De Cordova Console-Plug-in toevoegen. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met mobiele betrokkenheid backend

1. De invoegtoepassing voor Azure Mobile betrokkenheid Cordova terwijl de waarden van variabelen om te configureren van de invoegtoepassing installeren:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Pictogram voor android hebt bereikt* : de naam van de resource zonder eventuele extensie of drawable voorvoegsel (ex: mynotificationicon), en het pictogrambestand moet worden gekopieerd naar uw android-project (platforms/android/resolutie/drawable)

*iOS hebt bereikt pictogram* : de naam van de resource met de extensie (ex: mynotificationicon.png), en het pictogrambestand moet worden toegevoegd in uw project iOS met XCode (via het Menu bestanden toevoegen)

##<a id="monitor"></a>Realtime-controle inschakelen

1. Bewerk in het project Cordova - **www/js/index.js** om toe te voegen de oproep door naar mobiele betrokkenheid declareren activiteit in een nieuwe één keer de gebeurtenis *deviceReady* wordt ontvangen.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Voer de toepassing:
        
    - **Voor iOS**
    
        In `Terminal` venster start uw app in een nieuw exemplaar van de Simulator door het uitvoeren van de volgende handelingen uit:

            cordova run ios

    - **Voor Android**
        
        In `Terminal` venster start uw app in een nieuw exemplaar van de emulator door het uitvoeren van de volgende handelingen uit:

            cordova run android

3. U kunt de volgende handelingen uit in de Logboeken console zien:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen en app messaging inschakelen

Mobile betrokkenheid kunt u communiceren met uw gebruikers met Pushmeldingen en app messaging in de context van campagnes. Deze module heet bereiken in de Mobile-betrokkenheid-portal.
De volgende secties wordt uw app voor het ontvangen van deze instelling.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Push-referenties configureren voor mobiele betrokkenheid

Als u wilt toestaan de betrokkenheid mobiele Push-meldingen namens u verzenden, moet u toegang tot uw Apple iOS certificaat of GCM Server API-sleutel. 
    
1. Navigeer naar de betrokkenheid van de Mobile-portal. Controleer of dat u zich in de app wordt gebruikt voor dit project en klik op de knop **Engage** onder:
    
    ![][1]
    
2. U wordt terechtkomt op de instellingenpagina in de Portal van uw betrokkenheid. Uit er Klik op de sectie **Systeemeigen Push** :
    
    ![][2]

3. IOS certificaat/GCM Server API-sleutel configureren

    **[iOS]**

    een. Selecteer uw .p12, uploaden en typ uw wachtwoord:
    
    ![][3]

    **[Android]**

    een. Klik op het pictogram edit vóór **API-sleutel** in de sectie GCM instellingen en klik in het pop-upmenu welke taken bevat omhoog, de sleutel van de Server GCM plakken en klik op **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Push-meldingen in de app Cordova inschakelen

Bewerk **www/js/index.js** om toe te voegen van de oproep door naar mobiele betrokkenheid aanvragen push-meldingen en een handler declareren:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>De app uitvoeren

**[iOS]**

1. We XCode voor bouwen en implementeren van de app op het apparaat voor het testen van push-meldingen aangezien iOS alleen push-meldingen naar een werkelijke apparaat kunt gebruiken. Ga naar de locatie waar uw project Cordova wordt gemaakt en navigeer naar de locatie **...\platforms\ios** . Open het bestand systeemeigen .xcodeproj op XCode. 
    
2. Bouw en implementeer de app Cordova naar de iOS-apparaat met het account met de inrichten profiel waarop het certificaat dat u zojuist hebt geüpload naar de betrokkenheid van de Mobile-portal en de App-Id welke overeenkomsten degene die u tijdens het maken van de app Cordova hebt opgegeven. U kunt de *bundel-id* in raadplegen uw **Resources\*-info.plist** bestand in XCode hieraan omhoog. 

3. Ziet u de standaard iOS die wordt weergegeven op uw apparaat zeggen dat de app aanvraagt machtiging om meldingen te verzenden. De machtiging. 

**[Android]**

U kunt de emulator gewoon gebruiken en de Android-app uitvoeren, zoals GCM meldingen worden ondersteund op de Android emulator. 

    cordova run android

##<a id="send"></a>Een bericht verzenden naar uw app

Maak nu een eenvoudige campagne Push-bericht stuurt een push naar uw app wordt weergegeven op het apparaat:

1. Ga naar het tabblad **bereiken** in de portal van uw mobiele betrokkenheid

2. Klik op **Nieuwe aankondiging** om uw campagne push te maken

    ![][6]

3. Geef invoeritems om uw campagne **[Android]** te maken
    
    - Geef een **naam** voor de campagne. 
    - Selecteer het **Type levering** als *eenvoudige* *systeem-melding*
    - Selecteer het **moment van bezorging** als *"elk Time"*
    - Geef een **titel** voor de melding dat de eerste regel in de push.
    - Geef een **bericht** voor de melding die als de berichttekst fungeert. 

    ![][11]

4. Geef invoer als u wilt maken van uw campagne **[iOS]**

    - Geef een **naam** voor de campagne. 
    - Selecteer het **moment van bezorging** als *"niet alleen app"*
    - Geef een **titel** voor de melding dat de eerste regel in de push.
    - Geef een **bericht** voor de melding die als de berichttekst fungeert. 
 
    ![][12]

5. Schuif omlaag en in de sectie inhoud **melding alleen** selecteren

    ![][8]

6. [Optionele] U kunt ook een actie-URL opgeeft. Zorg ervoor dat deze wordt gebruikt een URL-schema dat is opgegeven tijdens het configureren van de invoegtoepassing **AZME\_OMLEIDEN\_URL** variabele bijvoorbeeld *myapp://test*.  

7. U klaar bent met de belangrijkste campagne mogelijke instelling. Nu opnieuw Schuif omlaag en klik op de knop **maken** om op te slaan van de campagne.

8. Ten slotte **activeren** uw campagne
    
    ![][10]

9. U ziet nu een push-bericht op uw apparaat of emulator als onderdeel van de campagne. 

##<a id="next-steps"></a>Volgende stappen
[Overzicht van alle methoden beschikbaar voor communicatie met Cordova Mobile betrokkenheid SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

