<properties
    pageTitle="Aan de slag met Azure melding Hubs met Baidu | Microsoft Azure"
    description="In deze zelfstudie leert u hoe u met Azure melding Hubs push-meldingen voor Android-apparaten met Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Aan de slag met melding Hubs Baidu gebruiken

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

Baidu cloud push is een Chinese cloudservice die u gebruiken kunt om te verzenden push-meldingen voor mobiele apparaten. Deze service is vooral handig in China, waar push-meldingen aan Android te bieden aan de complexe is vanwege de aanwezigheid van verschillende app winkels en push-services, naast de beschikbaarheid van Android-apparaten die niet meestal met GCM (Google Cloud Messaging verbonden zijn).

##<a name="prerequisites"></a>Vereisten voor

Deze zelfstudie is het volgende nodig:

+ Android SDK (we wordt ervan uitgegaan dat u wordt Eclips), die u van de <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-site downloaden kunt</a>
+ [Android SDK mobiele Services]
+ [Android SDK Baidu Push]

>[AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F)voor meer informatie.


##<a name="create-a-baidu-account"></a>Een Baidu-account maken

Als u wilt gebruiken Baidu, moet u een Baidu-account hebt. Als u al een hebt, meld u aan bij de [Baidu-portal] en gaat u verder met de volgende stap. Anders, ziet u de onderstaande instructies voor het maken van een Baidu-account.  

1. Ga naar de [Baidu-portal] en klik op de koppeling**登录**(**Login**). Klik op**立即注册**om het account registratie-proces te starten.

    ![][1]

2. Voer de vereiste gegevens, telefoon/e-mailadres, wachtwoord en verificatiecode, en klik op **aanmelden**.

    ![][2]

3. U ontvangt een e-mailbericht naar het e-mailadres dat u hebt ingevoerd met een koppeling om uw Baidu-account te activeren.

    ![][3]

4. Meld u aan bij uw e-mailaccount, opent u de Baidu activering voor e-mail en klik op de activeringskoppeling om uw Baidu-account te activeren.

    ![][4]

Nadat u een geactiveerde Baidu-account hebt, moet u zich aanmelden bij de [portal Baidu].

##<a name="register-as-a-baidu-developer"></a>Registreren als een ontwikkelaar Baidu

1. Wanneer u zich hebt aangemeld bij de [portal Baidu], klikt u op**更多 >>** (**meer**).

    ![][5]

2. Schuif omlaag in de sectie**站长与开发者服务 (Webmasterfuncties en ontwikkelaars Services)** en klikt u op**百度开放云平台**(**Baidu cloud platform openen**).

    ![][6]

3. Klik op de volgende pagina op**开发者服务**(**Ontwikkelaars Services**) in de rechterbovenhoek.

    ![][7]

4. Op de volgende pagina, klikt u in het menu aan de rechterbovenhoek op**注册开发者**(**Ontwikkelaars geregistreerd**).

    ![][8]

5. Voer uw naam, een beschrijving en een mobiel telefoonnummer voor het ontvangen van een tekstbericht verificatie en klik op**送验证码**(**Verificatiecode verzenden**). Houd er rekening mee dat voor internationale telefoonnummers opgeeft, moet u de landcode haakjes. Bijvoorbeeld voor een getal van de Verenigde Staten, dient **(1) 1234567890**.

    ![][9]

6. U moet vervolgens een bericht tekst met een verificatiecode, zoals wordt weergegeven in het volgende voorbeeld:

    ![][10]

7. Voer de verificatiecode van het bericht in**验证码**(**bevestigingscode**).

8. Ten slotte, voert u de registratie ontwikkelaars door de overeenkomst Baidu accepteren en te klikken op**提交**(**verzenden**). De volgende pagina wordt weergegeven op de registratie is voltooid:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Een project Baidu cloud push maken

Wanneer u een project Baidu cloud push maakt, ontvangt u uw app-ID, API-sleutel en geheime sleutel.

1. Wanneer u zich hebt aangemeld bij de [portal Baidu], klikt u op**更多 >>** (**meer**).

    ![][5]

2. Schuif omlaag in de sectie**站长与开发者服务**(**Webmasterfuncties en ontwikkelaars Services**) en klikt u op**百度开放云平台**(**Baidu cloud platform openen**).

    ![][6]

3. Klik op de volgende pagina op**开发者服务**(**Ontwikkelaars Services**) in de rechterbovenhoek.

    ![][7]

4. Klik op de volgende pagina op**云推送**(**Cloud Push**) in de sectie met**云服务**(**Cloudservices**).

    ![][12]

5. Als u een geregistreerde ontwikkelaar bent, ziet u**管理控制台**(**Management Console**) op het bovenste menu. Klik op**开发者服务管理**(**ontwikkelaars Management Service**).

    ![][13]

6. Klik op de volgende pagina op**创建工程**(**Project maken**).

    ![][14]

7. Naam van een toepassing en klik op**创建**(**maken**).

    ![][15]

8. Na het maken van een Baidu cloud push-project is geslaagd ziet u een pagina met **toepassings-id**, **API-sleutel**en **Geheim-toets**. Noteer de API en geheime sleutel, die we later gebruiken.

    ![][16]

9. Het project voor push-meldingen configureren door te klikken op**云推送**(**Cloud Push**) in het linkerdeelvenster.

    ![][31]

10. Klik op de volgende pagina op de knop**推送设置**(**Push-instellingen**).

    ![][32]  

11. Klik op de configuratiepagina toevoegen de pakketnaam dat u wordt gebruiken in uw Android project in het veld**应用包名**(**pakket van toepassing**), en klik vervolgens op**保存设置**(**Opslaan**).  

    ![][33]

Ziet u de**保存成功!** (**zijn opgeslagen!**) bericht.

##<a name="configure-your-notification-hub"></a>De melding hub configureren

1. Meld u aan bij de [Klassieke Azure-Portal]en klik vervolgens op **+ Nieuw** onderaan in het scherm.

2. Klik op **App-Services**, klik op **Service-Bus**, klik op **Melding Hub**en klik vervolgens op **Snelle maken**.

3. Geef een naam voor de **Melding-Hub**, selecteert u de **regio** en de **Namespace** waar de hub van deze melding wordt gemaakt, en klik op **een nieuwe melding Hub maken**.  

    ![][17]

4. Klik op de naamruimte waarin u uw hub melding hebt gemaakt en klik vervolgens op **Melding Hubs** aan de bovenkant.

    ![][18]

5. Selecteer de melding hub die u hebt gemaakt en klik vervolgens op **configureren** in het bovenste menu.

    ![][19]

6. Schuif omlaag naar de sectie **baidu-instellingen voor meldingen** en voer de API en geheime sleutel die u hebt aangeschaft bij de console Baidu eerder voor uw project Baidu cloud push. Klik op **Opslaan**.

    ![][20]

7. Klik op het tabblad **Dashboard** aan de bovenkant voor de melding hub en klik vervolgens op **Weergave-verbindingsreeks**.

    ![][21]

8. Noteer de **DefaultListenSharedAccessSignature** en **DefaultFullSharedAccessSignature** vanuit het venster **Toegangsgegevens voor de verbinding** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de melding hub

1. Maak een nieuw Android project in Eclips ADT, (**bestand** > **Nieuw** > **Android Application-Project**).

    ![][23]

2. Voer een **Naam-toepassing** en zorg ervoor dat de **Minimum vereist SDK** versie is ingesteld op **API-16: Android 4.1**.

    ![][24]

3. Klik op **volgende** en doorloop de wizard totdat het venster **Activiteit maken** wordt weergegeven. Zorg ervoor dat **Lege activiteit** is geselecteerd en ten slotte selecteert u **klaar bent** om te maken van een nieuwe toepassing voor Android.

    ![][25]

4. Zorg ervoor dat het **Project bouwen doel** juist is ingesteld.

    ![][26]

5. De melding-hubs-0.4.jar-bestand downloaden via het tabblad **bestanden** van de [Melding-Hubs-Android-SDK op Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Voeg het bestand naar de map **libs** van uw project Eclips en vernieuw de map *libs* .

6. Downloaden en pak de [Baidu Push Android SDK], open de map **libs** en kopieer het **pushservice-x.y.z** oppervlak-bestand en de **armeabi** & **mips** mappen in de map **libs** van uw Android-toepassing.

7. Open het bestand **AndroidManifest.xml** van uw Android project en de machtigingen die voor de Baidu SDK vereist zijn toevoegen.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. De eigenschap **android: naam** toevoegen aan uw **toepassing** -element in **AndroidManifest.xml**, *yourprojectname* (bijvoorbeeld **com.example.BaiduTest**) vervangen. Zorg ervoor dat deze projectnaam overeenkomt met de naam die u in de console Baidu geconfigureerd.

        <application android:name="yourprojectname.DemoApplication"

9. Voeg de volgende configuratie binnen het toepassingselement na de **. MainActivity** activiteit element, worden vervangen door *yourprojectname* (bijvoorbeeld **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Voeg een nieuwe klasse **ConfigurationSettings.java** aan het project.

    ![][28]

    ![][29]

10. Voeg de volgende code toe:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Stel de waarde van **API_KEY** met wat u hebt opgehaald uit de Baidu cloud project eerder, **NotificationHubName** met de naam van de melding hub in de klassieke Azure-Portal en **NotificationHubConnectionString** met DefaultListenSharedAccessSignature in de klassieke Azure-Portal.

11. Een nieuwe klasse **DemoApplication.java**toevoegen en voeg de volgende code toe:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Een andere nieuwe klasse **MyPushMessageReceiver.java**toevoegen en voeg de volgende code toe. Dit is de klasse die de push-meldingen die afkomstig zijn van de Baidu push-server verwerkt.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Open **MainActivity.java**en voeg de volgende met de methode **onCreate** :

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. De volgende importinstructies boven openen:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Meldingen verzenden naar uw app


U kunt snel testen meldingen ontvangen in uw app door te sturen van meldingen in de [Portal van Azure](https://portal.azure.com/) met de knop **Testen verzenden** op de melding-hub, zoals wordt weergegeven in het volgende scherm.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-meldingen worden meestal verzonden in een back-enddatabase-service zoals Mobile Services of met behulp van een compatibele bibliotheek. U kunt ook de REST API rechtstreeks naar de meldingsberichten verzenden als een bibliotheek niet beschikbaar voor uw back-enddatabase is gebruiken.

In deze zelfstudie we Houd het eenvoudig en alleen demonstreren testen van de client-app door meldingen met behulp van de .NET SDK voor melding hubs in een consoletoepassing in plaats van een back-end-service te verzenden. Het is raadzaam de zelfstudie [Gebruik melding Hubs push-meldingen aan gebruikers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) als de volgende stap voor het verzenden van meldingen van een ASP.NET-end. De volgende methoden kunnen echter worden gebruikt voor het verzenden van meldingen:

* **REST API Interface**: kunt u de melding op elk backend-platform met de [REST API interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)ondersteunen.

* **Microsoft Azure melding Hubs .NET SDK**: In het Nuget pakket Manager voor Visual Studio [- Installatiepakket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)uitvoeren.

* **Node.js** : [het gebruik van de melding Hubs uit Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobile-Apps**: Zie voor een voorbeeld van hoe u meldingen van een end Azure App Service Mobile-Apps die geïntegreerd met melding Hubs verzendt, [push-meldingen toevoegen aan uw mobiele app](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: Zie voor een voorbeeld van hoe u meldingen verzendt met behulp van de REST API's, "Het gebruik van de melding Hubs uit Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Optioneel) Meldingen verzenden vanuit een .NET-console-app.

In dit gedeelte leert u verzendt een melding met een .NET-console-app.

1. Maak een nieuwe Visual C# consoletoepassing:

    ![][30]

2. Klik in het venster Package Manager Console instellen met de **project standaard** naar uw nieuwe console-toepassingsproject en klik vervolgens in het consolevenster Voer de volgende opdracht uit:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee voegt u een verwijzing naar de Azure melding Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Open het bestand **Program.cs** en voeg de volgende instructie gebruiken:

        using Microsoft.Azure.NotificationHubs;

4. In uw `Program` klasse, voegt u de volgende methode en *DefaultFullSharedAccessSignatureSASConnectionString* en *NotificationHubName* vervangen door de waarden die u hebt.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Voeg de volgende regels in uw methode **Main** :

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Uw app testen

Als u wilt testen deze app met een werkelijke telefoon, alleen verbinding maken met de telefoon naar uw computer met behulp van een USB-kabel. Hiermee wordt uw app naar de bijgevoegde telefoon geladen.

Klik op **uitvoeren**als u wilt testen van deze app met de emulator, klik op de bovenste werkbalk Eclips en selecteer uw app. Hiermee start de emulator, en vervolgens wordt geladen en wordt de app wordt uitgevoerd.

De app de 'een gebruikers-id' en 'channelId' opgehaald uit de Baidu Push notification-service en registreert met de hub melding.

Als u wilt verzenden van een melding test kunt u het tabblad foutopsporing van de klassieke Azure-Portal. Als u de .NET-console-toepassing voor Visual Studio hebt gemaakt, drukt u op de toets F5 in Visual Studio om de toepassing wordt uitgevoerd. De toepassing stuurt een melding die wordt weergegeven in het bovenste systeemvak van uw apparaat of emulator.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Android SDK mobiele Services]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Android SDK Baidu Push]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure klassieke Portal]: https://manage.windowsazure.com/
[Baidu-portal]: http://www.baidu.com/
