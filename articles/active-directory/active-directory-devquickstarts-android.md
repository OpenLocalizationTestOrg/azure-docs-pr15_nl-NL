<properties
    pageTitle="Azure AD-Android aan de slag | Microsoft Azure"
    description="Het maken van een Android-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en u belt Azure AD beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Azure AD integreren met een Android-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Als u een bureaubladtoepassing ontwikkelt, kunt Azure AD u eenvoudige en duidelijke voor u te verifiëren van uw gebruikers met behulp van hun Active Directory-accounts.  Bovendien kunt uw toepassing veilig eventuele web API die zijn beveiligd met Azure AD, zoals de Office 365-API of de API Azure in beslag neemt.

Voor Android-clients die nodig hebt voor toegang tot beveiligde bronnen biedt Azure AD de Active Directory-verificatie Library of ADAL.  De enige doel van ADAL in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen.  U kunt zien hoe gemakkelijk het is, gaat hier we maken een toepassing Android takenlijst die:

-   Haalt toegang tot tokens voor het bellen van een taak lijst API met het [OAuth 2.0 verificatie-protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   De takenlijst van een gebruiker krijgt
-   Positief of negatief gebruikers af.

Als u wilt beginnen, moet u een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren.  Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

> [AZURE.TIP] Probeer het voorbeeld van onze nieuwe [developer portal](https://identity.microsoft.com/Docs/Android) waarmee u kunt aan de slag met Azure Active Directory in slechts een paar minuten!  De developer portal wordt u begeleid bij het proces van het registreren van een app en Azure AD integreren in uw code.  Wanneer u klaar bent, hebt u een eenvoudige toepassing die gebruikers in uw tenant en een backend waarmee u kunnen tokens accepteren en gevalideerd kan verifiëren. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Stap 1: Downloaden en de Node.js REST API doen steekproef Server wordt uitgevoerd

In dit voorbeeld is geschreven werkt ten opzichte van onze bestaande voorbeeld voor het samenstellen van een enkel tenant taak REST API voor Microsoft Azure Active Directory in. Dit is een spelen voor de werkbalk Snel starten.

Informatie over hoe u dit instelt, vindt u hier onze bestaande voorbeelden:

* [Microsoft Azure Active Directory steekproef REST API-Service voor Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Stap 2: Uw Web API registreert bij uw Microsoft Azure AD-Tenant

**Wat heb ik nu doen?**

*Microsoft Active Directory ondersteunt het toevoegen van twee typen toepassingen. Web API's die services bieden aan gebruikers en toepassingen (hetzij op het web of een toepassing wordt uitgevoerd op een apparaat) dat toegang deze Web-API's tot. In deze stap schrijft u de Web-API die u lokaal voor het testen van dit voorbeeld worden uitgevoerd. Normaal zou dit Web API een REST-service die u wilt dat een app voor toegang tot functionaliteit biedt. Een eindpunt kunt beveiligen met een Microsoft Azure Active Directory!*

*Hier wordt uitgegaan van u de taak REST API hierboven vermelde registreert, maar dit werkt voor Web API's kunt u Azure Active Directory om te beveiligen.*

Stappen voor het registreren van een Web-API met Microsoft Azure AD

1. Meld u aan bij de [portal van Azure management](https://manage.windowsazure.com).
2. Klik op Active Directory in de linkerpagina nav.
3. Klik op de map tenant waar u wilt registreren van de steekproef-toepassing.
4. Klik op het tabblad toepassingen.
5. Klik in het vel, klikt u op toevoegen.
6. Klik op "Toevoegen een toepassing het ontwikkelen van mijn organisatie".
7. Voer een beschrijvende naam voor de select-toepassing, bijvoorbeeld "TodoListService", "Web API voor Web Application en/of" en klik op volgende.
8. Voor de URL van eenmalige aanmelding, voert u de basis-URL voor de steekproef, die standaard is `https://localhost:8080`.
9. Voer voor de App-ID-URI, `https://<your_tenant_name>/TodoListService`, vervangt `<your_tenant_name>` met de naam van uw Azure AD-tenant.  Klik op OK om het registratieproces te voltooien.
10. Klik op het tabblad configureren van uw toepassing terwijl u nog steeds in de portal Azure.
11. **De waarde van de Client-ID en reserveer kopiëren**, moet u dit later tijdens het configureren van uw toepassing.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Stap 3: De steekproef Android Native Client-toepassing registreren

Uw webtoepassing registreren, is de eerste stap. Vervolgens moet u Azure Active Directory informeren over uw toepassing. Hiermee kan de toepassing om te communiceren met de zojuist geregistreerde Web-API

**Wat heb ik nu doen?**  

*Zoals hierboven, ondersteunt Microsoft Azure Active Directory toe te voegen twee soorten toepassingen. Web API's die services bieden aan gebruikers en toepassingen (hetzij op het web of een toepassing wordt uitgevoerd op een apparaat) dat toegang deze Web-API's tot. In deze stap schrijft u de toepassing in dit voorbeeld. U moet dit doen in de volgorde van deze toepassing kunnen verzoek voor toegang tot de API Web die u zojuist hebt geregistreerd. Azure Active Directory weigert zelfs zodat uw toepassing om te vragen om aanmeldingsproblemen, tenzij deze geregistreerd! Die uitmaakt deel van de beveiliging van het model.*

*Hier wordt uitgegaan van u in dit voorbeeld-toepassing bovengenoemde registreert, maar dit werkt voor een app die u ontwikkelt.*

**Waarom heb ik een toepassing en een Web-API plaatsen in een tenant?**

*Als u mogelijk hebt raden, kunt u een app die toegang heeft tot een externe API dat is geregistreerd in Azure Active Directory van een andere tenant kan maken. Als u dat doet, wordt uw klanten gevraagd naar toestemming voor het gebruik van de API in de toepassing. Het deel achter de nette is, Active Directory-verificatie-bibliotheek voor iOS zorgt voor deze toestemming voor u! Terwijl we onmiddellijk aan meer geavanceerde functies, ziet u dat dit is een belangrijk onderdeel van het werk dat nodig is voor toegang tot de suite van Microsoft APIs van Azure en Office, evenals andere providers, service. Voorlopig, omdat u uw Web API en de toepassing onder dezelfde tenant geregistreerd ziet u geen vragen om toestemming. Dit is meestal de hoofdletters/kleine letters als u een toepassing alleen betrekking heeft op uw eigen bedrijf ontwikkelt.*

1. Meld u aan bij de [portal van Azure management](https://manage.windowsazure.com).
2. Klik op Active Directory in de linkerpagina nav.
3. Klik op de map tenant waar u wilt registreren van de steekproef-toepassing.
4. Klik op het tabblad toepassingen.
5. Klik in het vel, klikt u op toevoegen.
6. Klik op "Toevoegen een toepassing het ontwikkelen van mijn organisatie".
7. Voer een beschrijvende naam voor de toepassing, bijvoorbeeld "TodoListClient-Android', schakelt"systeemeigen clienttoepassing', en klik op volgende.
8. Voer voor de URI omleiden, `http://TodoListClient`.  Klik op voltooien.
9. Klik op het tabblad configureren van de toepassing.
10. De Client-ID-waarde en deze kopiëren Reserveer, moet u dit later tijdens het configureren van uw toepassing.
11. In 'Machtigingen aan andere toepassingen', klikt u op 'Toepassing toevoegen'.  Selecteer "Overig" in de vervolgkeuzelijst 'Weergeven' en klik op het bovenste selectievakje is ingeschakeld.  Zoek en klik op de TodoListService en klik op het vinkje onder als de toepassing wilt toevoegen.  "Toegang TodoListService" selecteren in de vervolgkeuzelijst 'De machtigingen gedelegeerde' en sla de configuratie.



Als u wilt maken met Maven, kunt u de pom.xml op hoogste niveau

  * Klonen deze cessies‑retrocessies in naar een map van uw keuze:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Volg de stappen in [de sectie Prerequests voor het instellen van uw maven voor android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Emulator met 19 SDK Setup
  * Ga naar de hoofdmap waar u de cessies‑retrocessies gekopieerd
  * De opdracht uitvoeren: mvn wissen.Control installeren
  * Wijzig de map in de steekproef snel aan de slag: cd samples\hello
  * De opdracht uitvoeren: android mvn: android: klaar implementeren
  * U ziet nu app starten
  * Voer de gebruikersreferenties testen om te proberen!

Oppervlak-pakketten ook onderworpen naast het pakket aar.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Stap 4: De ADAL Android downloaden en deze toevoegen aan uw werkruimte Eclips

We hebt aangebracht gemakkelijk kunt u hebt meerdere opties voor het gebruik van deze bibliotheek in uw Android project:

* U kunt de broncode deze bibliotheek importeren in Eclips en een koppeling in uw toepassing.
* Als Android Studio gebruikt, kunt u *aar* pakket opmaken gebruikt en verwijzen naar de binaire bestanden.

####<a name="option-1-source-zip"></a>Optie 1: Bron Zip

Als u wilt een kopie van de broncode downloaden, klikt u op "Downloaden ZIP" aan de rechterkant van de pagina of klik op [hier](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Optie 2: Bron via cijfer

Als u wilt de broncode van de SDK via cijfer alleen het volgende typt:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Optie 3: Binaire bestanden via Gradle

U kunt de binaire bestanden openen via het centrale cessies‑retrocessies Maven. AAR pakket kan als volgt worden opgenomen in uw project in AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Optie 4: aar via Maven

Als u de invoegtoepassing voor m2e in Eclips gebruikt, kunt u de afhankelijkheid in uw bestand pom.xml opgeven:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Optie 5: oppervlak-pakket in libs map
U kunt het bestand oppervlak krijgen van de cessies‑retrocessies maven en neerzetten in de map *libs* in uw project. Moet u de vereiste bronnen aan uw project ook kopiëren, aangezien de oppervlak-pakketten ze niet bevatten.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Stap 5: Verwijzingen naar Android ADAL toevoegen aan uw project


2. Verwijzing naar een aan uw project en geef deze op als een Android-bibliotheek. Als u niet zeker hoe weet u dit doet, [Klik hier voor meer informatie] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. De project-afhankelijkheid voor foutopsporing in naar de projectinstellingen van uw toevoegen

4. Bijwerken van uw project AndroidManifest.xml dat moet worden opgenomen:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Een exemplaar van AuthenticationContext bij uw belangrijkste activiteiten maken. De details van dit gesprek zijn buiten het bereik van dit Leesmij-bestand, maar u kunt een goede begin openen door de [Android Native clientvoorbeeld](https://github.com/AzureADSamples/NativeClient-Android)kijken. Hieronder volgt een voorbeeld:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Opmerking: mContext is een veld in uw activiteiten

8. Kopieer deze codeblok voor het verwerken van het einde van AuthenticationActivity nadat de gebruiker referenties invoert en ontvangt autorisatiecode:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Als u vragen om een token, wilt definiëren u een terugbellen

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Tot slot vragen om een token terugbellen gebruiken:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Uitleg van de parameters:

  * Resource is vereist en is de resource die u probeert te openen.
  * Clientid is vereist en afkomstig is van de AzureAD Portal.
  * U kunt redirectUri instellen als uw pakketnaam. Deze is niet verplicht voor het gesprek acquireToken worden toegekend.
  * PromptBehavior helpt om te vragen om referenties cache en cookie overgeslagen.
  * Terugbellen wordt aangeroepen nadat autorisatiecode kan worden omgewisseld voor een token.

  De terugbellen krijgt een object met AuthenticationResult waarvoor accesstoken, datum verlopen, en idtoken info.

Optioneel: **acquireTokenSilent**

U kunt bellen **acquireTokenSilent** worden afgehandeld caching en token vernieuwen. Als u ook de versie voor de synchronisatie krijgen. Gebruikers-id worden geaccepteerd als parameter.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Makelaar**: app intune van Microsoft Intune Company portal, vindt u in het onderdeel makelaar. ADAL wordt gebruik het makelaar-account, als er één gebruikersaccount is gemaakt voor deze verificator en ontwikkelaars kiezen niet op deze overslaat. Ontwikkelaars kan de gebruiker makelaar met overslaan:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 De ontwikkelaar moet speciale redirectUri voor gebruik van de makelaar registreren. RedirectUri bevindt zich in de notatie van msauth://packagename/Base64UrlencodedSignature. U kunt uw redirecturi opvragen voor de app met het script "brokerRedirectPrint.ps1" of API gesprek mContext.getBrokerRedirectUri gebruiken. Handtekening is gekoppeld aan uw ondertekend certificaten.

 Huidige makelaar model is voor één gebruiker. AuthenticationContext biedt API methode als u de gebruiker makelaar.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Makelaar gebruiker wordt geretourneerd als account geldig is.

 Uw app-manifest machtigingen gebruikt AccountManager accounts nodig hebt: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


In dit scenario wordt gebruikt, hebt u wat u moet wel integreren met Azure Active Directory. Voor meer voorbeelden van deze werken, gaat u naar de AzureADSamples / bibliotheek op GitHub.

## <a name="important-information"></a>Belangrijke informatie

### <a name="customization"></a>Aanpassing

Bibliotheek projectresources kunnen worden overschreven door de resources voor uw toepassing. Dit gebeurt als het bouwen van uw app. Daarom kunt u verificatie activiteit indeling de gewenste manier aanpassen. U nodig hebt om ervoor te zorgen dat de id van de besturingselementen die ADAL uses(Webview).

### <a name="broker"></a>Makelaar

Onderdeel van de makelaar wordt geleverd met Microsoft Intune van app intune Company portal. Account wordt gemaakt in Account Manager. Accounttype is 'com.microsoft.workaccount'. Staat één SSO-account. Deze maakt SSO cookie voor deze gebruiker na het voltooien van apparaat uitdaging voor een van de apps.

### <a name="authority-url-and-adfs"></a>Certificeringsinstantie Url en ADFS

ADFS wordt niet herkend als STS, dus u hoeft te schakelen van exemplaar discovery en ONWAAR laat lopen AuthenticationContext constructor.

Certificeringsinstantie url moet worden STS exemplaar en de naam van de tenant: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Query's uitvoeren cache items

ADAL biedt standaardcache in SharedPreferences met enkele eenvoudige cache queryfuncties. U kunt de huidige cache openen via AuthenticationContext met:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Als u wilt aanpassen, kunt u ook uw implementatie cache opgeven.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL biedt een optie om de prompt gedrag opgeeft. PromptBehavior.Auto verschijnt UI als vernieuwen token ongeldig is en gebruikersreferenties vereist zijn. PromptBehavior.Always wordt het gebruik van de cache overslaan en altijd weergeven UI.

### <a name="silent-token-request-from-cache-and-refresh"></a>Stille token verzoek om van cache en vernieuwen

Deze methode gebruikt geen UI pop omhoog en een activiteit niet vereist. Wordt geretourneerd token uit de cache indien beschikbaar. Als token is verlopen, wordt deze probeert te vernieuwen. Als het vernieuwen token is verlopen of is mislukt, wordt AuthenticationException geretourneerd.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

U kunt ook bellen met deze methode synchronisatie aanbrengen. U kunt null naar terugbellen instellen of acquireTokenSilentSync gebruiken.

### <a name="diagnostics"></a>Diagnostische hulpprogramma 's

Hier volgen de primaire bronnen van gegevens voor de diagnose van problemen:

+ Uitzonderingen
+ Logboeken
+ Netwerk sporen

Ook rekening mee dat correlatie-id's centraal naar de diagnostische hulpprogramma's in de bibliotheek. U kunt instellen dat uw correlatie id's op basis per aanvraag als u wilt relateren van een ADAL aanvragen met andere bewerkingen in uw code. Als u geen een correlatie-id en vervolgens ADAL een willekeurig een and alle genereert worden berichten in het logboek en netwerk oproepen tijdstempel met de correlatie-id. De self gegenereerd id wijzigingen weergeven op elk verzoek om een.

#### <a name="exceptions"></a>Uitzonderingen

Dit is duidelijk de eerste diagnostic. We proberen te bieden handig foutberichten. Als u dat niet is handig vinden Rapporteer op een probleem en laat het ons weten. Geef apparaatgegevens zoals model en SDK # ook.

#### <a name="logs"></a>Logboeken

De bibliotheek log-mailberichten die u gebruiken kunt om op te sporen problemen wilt genereren, kunt u configureren. U kunt logboekregistratie configureren door de volgende oproep een terugbellen die ADAL worden gebruikt door te leveren uit elk logboekbericht zoals deze wordt gegenereerd configureren.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Berichten kunnen worden geschreven in een aangepaste logboekbestand, zoals hieronder wordt weergegeven. Er is geen standaard manier om Logboeken vanaf een apparaat. Er zijn enkele services die u met deze helpen kunnen. U kunt ook uw eigen, zoals het bestand verzenden naar een server voor voorraad.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Logboekregistratieniveaus

+ Error(Exceptions)
+ Warn(Warning)
+ Info (ter informatie)
+ Uitgebreid (meer informatie)

U instellen het niveau voor logboekregistratie als volgt:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Alle berichten in het logboek zijn verzonden naar logcat naast elke aangepaste log terugbellen.
U gaat log naar een bestand formulier logcat als belog wordt weergegeven:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Als u meer voorbeelden over adb opdrachten: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Netwerk sporen

U kunt diverse hulpprogramma's gebruiken om vast te leggen het HTTP-verkeer dat ADAL wordt gegenereerd.  Dit is vooral handig als u bekend met het OAuth-protocol bent of als u moet diagnostische gegevens naar Microsoft of andere kanalen ondersteuning.

Fiddler is de eenvoudigste hulpmiddel voor het traceren van HTTP.  Gebruik de volgende koppelingen om snel aan de correct record ADAL netwerkverkeer in te stellen.  Om te kunnen handig zijn is het nodig zijn voor het configureren van fiddler of een ander hulpmiddel zoals Jeroen, niet-versleutelde SSL-verkeer opnemen.  Opmerking: Sporen gegenereerd op deze manier kunnen veel bevoegdheden gegevens zoals access tokens, gebruikersnamen en wachtwoorden bevatten.  Als u productierekeningen gebruikt, u deze sporen niet delen met 3e partijen.  Als u een spoor aan iemand opgeven om te ondersteuning krijgen moet, moet u het probleem met een tijdelijke rekening met de gebruikersnamen en wachtwoorden die u niet erg om delen vindt reproduceren.

+ [Bij het instellen van Fiddler voor Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Regels voor Fiddler configureren voor ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialoogvenster modus
de methode acquireToken zonder activiteit ondersteunt dialoogvenster.

### <a name="encryption"></a>Versleuteling

ADAL versleutelt de tokens en store in SharedPreferences al dan niet standaard. U kunt de klasse StorageHelper kunt u de details bekijken. Android geïntroduceerd AndroidKeyStore voor 4.3(API18) veilige opslag van persoonlijke sleutels. ADAL gebruikt voor API18 en hoger. Als u ADAL voor onderste SDK versies gebruiken wilt, moet u geheime sleutel bij AuthenticationSettings.INSTANCE.setSecretKey opgeven

### <a name="oauth2-bearer-challenge"></a>Oauth2 vruchtdragende uitdaging

AuthenticationParameters klasse biedt functionaliteit als u de authorization_uri vanuit Oauth2 vruchtdragende uitdaging.

### <a name="session-cookies-in-webview"></a>Sessiecookies in webweergave

Android webweergave wist niet sessiecookies nadat de app is gesloten. U kunt dit verwerken met het onderstaande voorbeeldcode:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Meer informatie over cookies: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Resource-negeren

De ADAL bibliotheek bevat Engelse tekenreeksen voor de volgende twee ProgressDialog-berichten.

Uw toepassing moet overschrijven gelokaliseerde tekenreeksen zijn desgewenst.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM-dialoogvenster
Adal versie 1.1.0 ondersteunt NTLM-dialoogvenster dat wordt verwerkt via onReceivedHttpAuthRequest gebeurtenis van WebViewClient. Dialoogvenster lay-out en tekenreeksen kunnen worden aangepast.

### <a name="cross-app-sso"></a>Cross-app eenmalige aanmelding
Informatie over [het inschakelen van eenmalige aanmelding van cross-app op android-apparaat met ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
