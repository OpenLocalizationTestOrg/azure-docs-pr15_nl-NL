<properties
    pageTitle="Secure Push-meldingen met Azure melding Hubs verzenden"
    description="Informatie over het verzenden van secure push-meldingen aan een Android-app van Azure. Voorbeelden van de code is geschreven in Java en C#."
    documentationCenter="android"
    keywords="push-meldingen, push-meldingen, push-berichten, android push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Secure Push-meldingen met Azure melding Hubs verzenden

> [AZURE.SELECTOR]
- [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-apparaat](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started)voor meer informatie.

Ondersteuning voor push-meldingen in Microsoft Azure krijgt u toegang tot een eenvoudig te gebruiken, meerdere platforms, schaal-out push bericht infrastructuur, waardoor enorm eenvoudiger de uitvoering van push-meldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms.

Vanwege wettelijke of beveiligingsbeperkingen, soms een toepassing mogelijk moeten worden opgenomen iets in de melding die niet kan worden overgebracht via de standaard push notification-infrastructuur. Deze zelfstudie wordt beschreven hoe de dezelfde ervaring bereiken door te sturen vertrouwelijke gegevens door een beveiligde, geverifieerde verbinding tussen de client Android-apparaat en de app-end.

Op hoog niveau is de stroom als volgt uit:

1. De app back-enddatabase:
    - Winkels secure nettolading in back-enddatabase.
    - Verzendt de ID van deze melding naar de Android-apparaat (er worden geen gegevens veilig is verzonden).
2. De app op het apparaat, wanneer u de melding ontvangt:
    - De Android-apparaat contact op met de back-enddatabase aanvragen van de beveiligde nettolading.
    - De app kunt u de nettolading weergeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), wordt ervan uitgegaan dat het apparaat een verificatietoken in de lokale opslag, opslaat nadat de gebruiker zich aanmeldt. Dit zorgt ervoor een volledig naadloze oplossing, terwijl het apparaat van de melding secure payload met deze token kunt ophalen. Als uw toepassing slaat geen verificatietokens op het apparaat, of als deze tokens kunnen worden verlopen, moet een algemene melding dat de gebruiker wordt gevraagd naar de app Start de app apparaat bij ontvangst van de push-bericht worden weergegeven. De app wordt geverifieerd door de gebruiker vervolgens en ziet u de melding nettolading.

Deze zelfstudie wordt getoond hoe u secure push-meldingen verzendt. Dit is gebaseerd op de [Hoogte gebruikers](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) zelfstudie, zodat u moet de stappen in deze zelfstudie eerst als u nog niet is gedaan.

> [AZURE.NOTE] Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en configureren van uw melding-hub, zoals beschreven in [Aan de slag met melding Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Het Android-project wijzigen

Nu dat u uw app-back-enddatabase in als u wilt verzenden, alleen de *id* van een push-meldingen hebt gewijzigd, moet u uw Android-app u deze melding voor uw back-enddatabase om op te halen de beveiligd bericht wordt weergegeven voor terugbellen wijzigen.
Als u wilt realiseren, hebt u er om ervoor te zorgen dat uw Android-app hoe om zichzelf te verifiëren met uw back-enddatabase weet wanneer deze het push-meldingen ontvangt.

We wordt nu de stroom *login* wijzigen om de waarde van de verificatie-header opslaan in de gedeelde voorkeuren van de app. Andere soortgelijke regelingen kunnen worden gebruikt voor de opslag van alle verificatietoken (bijvoorbeeld OAuth tokens) die de app gebruiken moet zonder gebruikersreferenties.

1. In uw project Android-app toevoegen de volgende constanten boven aan de klasse **MainActivity** :

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Werk nog steeds in de klas **MainActivity** de `getAuthorizationHeader()` methode bevatten de volgende code:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Voeg de volgende `import` instructies boven aan het bestand **MainActivity** :

        import android.content.SharedPreferences;

Nu wordt we de handler die wordt aangeroepen wanneer het bericht wordt ontvangen wijzigen.

4. In de **MyHandler** klasse wijzigen de `OnReceive()` methode bevatten:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Voegt u de `retrieveNotification()` methode, de tijdelijke aanduiding vervangen `{back-end endpoint}` met het back-enddatabase eindpunt verkregen tijdens het implementeren van uw back-enddatabase:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Deze methode belt uw app-back-end om de inhoud van de melding met de referenties die zijn opgeslagen in de voorkeuren voor gedeelde wordt opgehaald en weergegeven als een normale melding. De melding Hiermee wordt gezocht naar de app-gebruiker precies zoals elke andere push-meldingen.

Houd er rekening mee bij voorkeur voor het geval van ontbrekende verificatie koptekst eigenschap of afkeuring door de back-enddatabase. De specifieke afhandeling van deze zaken afhankelijk voornamelijk van de gebruikerservaring voor de doeltoepassing. Eén optie is om een melding met een algemene prompt voor de gebruiker om te verifiëren om op te halen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>Voer de toepassing

Ga als volgt te werk om de toepassing uitvoert:

1. Zorg ervoor dat **AppBackend** wordt geïmplementeerd in Azure wordt aangegeven. Als Visual Studio, voert u de **AppBackend** Web API-toepassing. Een ASP.NET-webpagina wordt weergegeven.

2. In Eclips, de app niet uitvoeren op een Android-apparaat van de fysieke of de emulator.

3. Voer een gebruikersnaam en wachtwoord in de Android-app UI. Deze kunnen een willekeurige tekenreeks, maar ze moeten hetzelfde resultaat.

4. Klik in de Android-app UI op **aanmelden**. Klik vervolgens op **push verzenden**.
