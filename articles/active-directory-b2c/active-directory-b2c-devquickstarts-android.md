<properties
    pageTitle="Azure Active Directory-B2C: Een web API bellen vanuit een Android-toepassing | Microsoft Azure"
    description="In dit artikel wordt uitgelegd hoe u een android-apparaat 'takenlijst' maken app die een Node.js web API-oproepen via OAuth 2.0 vruchtdragende tokens. Zowel de Android-app en het web API kunnen Azure Active Directory B2C nu gebruikersidentiteiten beheer en gebruikers verifiëren."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Een web API bellen vanuit een Android-toepassing

> [AZURE.WARNING] Deze zelfstudie moet enkele belangrijke updates specifiek aan het gebruik van ADAL Android voor B2C verwijderen.  We gaan vers instructies voor het gebruik van Azure AD B2C in Android-apps in de volgende week publiceren en het is raadzaam vasthouden uitschakelen tot die tijd.  Maar als u alleen dingen out proberen wilt, je mag rustig gaat u verder met het onderstaande artikel.



Met behulp van Azure Active Directory (Azure AD) B2C, kunt u krachtige selfservice-identiteit beheerfuncties toevoegen aan uw Android-apps en web API's in een paar korte stappen. In dit artikel wordt uitgelegd hoe u een android-apparaat "takenlijst" maken app die een Node.js web API-oproepen via OAuth 2.0 vruchtdragende tokens. Zowel de Android-app en het web API kunnen Azure AD B2C nu gebruikersidentiteiten beheer en gebruikers verifiëren.

Deze snelstartgids is vereist dat er een web API die is beveiligd met DRM Azure AD met B2C volledig werken. We hebt een handtekening voor zowel Node.js kunt gebruiken als .NET gemaakt. In dit overzicht wordt ervan uitgegaan dat in het voorbeeld van de web API Node.js is geconfigureerd. Zie de [Azure AD B2C web API voor Node.js zelfstudie](active-directory-b2c-devquickstarts-api-node.md)voor meer informatie.

Azure AD biedt voor Android-clients die beveiligde bronnen moeten de Active Directory verificatie bibliotheek (ADAL). De enige doel van ADAL is vergemakkelijkt terwijl uw app access tokens krijgen. Om te laten zien hoe makkelijk het is, in deze handleiding we bouw een toepassing van de lijst met Android taak die:

- Access-tokens die een takenlijst API bellen via het [OAuth 2.0 verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx)krijgt.
- Gebruikers takenlijsten krijgt.
- Positief of negatief gebruikers.

> [AZURE.NOTE] Dit artikel besproken niet hoe u implementeert aanmeldingsproblemen, aanmelding en beheer van gebruikersprofielen met behulp van Azure AD B2C. De nadruk op het web API's bellen nadat de gebruiker is geverifieerd. Als u nog niet is gedaan, moet u beginnen met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over de basisbeginselen van Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant. Een map is een container voor al uw gebruikers, apps, groepen en meer. Als u niet een al hebt, [Maak een map B2C](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. De app en de web API worden aangegeven met een enkel **Toepassings-ID** in dit geval omdat ze bestaan uit één logische app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken. Zorg ervoor dat:

- Een **WebApp**opnemen/**web API** in de toepassing.
- Voer `urn:ietf:wg:oauth:2.0:oob` als een **antwoord-URL**. De standaard-URL voor dit voorbeeld is.
- Een **toepassing geheim** voor uw toepassing maken en deze kopiëren. Moet u deze later. Houd er rekening mee dat deze waarde worden [voorafgegaan XML moet](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) voordat u deze gebruiken.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Ook moet u dit later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). Deze app bevat drie identiteit ervaringen: registreren, aanmelden en meld u aan met Facebook.  U moet maken van één beleid van elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies de **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies de **naam weer te geven** en de **Object-ID** -toepassing claims in elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  U hebt de volgende namen beleid later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u bent uw app maken.

Houd er rekening mee dat het gebruik van de beleidsregels die u zojuist hebt gemaakt in dit artikel niet besproken. Meer informatie over de werking van beleidsregels in Azure AD B2C, begint u met de [zelfstudie .NET web app aan de slag](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Maak de steekproef terwijl u door kunt u [het skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **U moet het skelet deze zelfstudie te downloaden.** Vanwege de complexiteit van een volledig functioneel Android toepassing implementeren, heeft het skelet UX-code die wordt uitgevoerd nadat u deze zelfstudie hebt voltooid. Dit is een maateenheid voor de tijd te besparen voor ontwikkelaars. De code UX is niet belangrijkste naar het onderwerp van het B2C toevoegen aan een Android-toepassing.

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Als u wilt maken met Maven, kunt u `pom.xml` op het hoogste niveau.

  1. Volg de stappen in [de sectie vereisten voor het instellen van Maven voor Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Het instellen van een emulator met SDK 21.
  3. Ga naar de hoofdmap waar u de cessies‑retrocessies gekopieerd.
  4. De opdracht uitvoeren `mvn clean install`.
  5. Wijzig de map in de steekproef Quickstart `cd samples\hello`.
  6. De opdracht uitvoeren `mvn android:deploy android:run`.

Hier ziet u de app starten. Voer de gebruikersreferenties testen om te proberen.

Java-archief (oppervlak)-pakketten ook onderworpen naast het pakket Android archief (AAR).

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>De ADAL Android downloaden en deze toevoegen aan uw werkruimte Android Studio

Hebt u opties voor het gebruik van deze bibliotheek in uw project Android:

* U kunt de broncode importeren van de bibliotheek in Eclips en een koppeling in uw toepassing.
* Als u Android Studio gebruikt, kunt u de indeling van het pakket AAR gebruikt en verwijzen naar de binaire bestanden.

### <a name="option-1-binaries-via-gradle-recommended"></a>Optie 1: Binaire bestanden via Gradle (aanbevolen)

U kunt de binaire bestanden openen via het centrale cessies‑retrocessies Maven. Het pakket AAR kan worden opgenomen in uw project in Android Studio (bijvoorbeeld in `build.gradle`) deze manier:

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Optie 2: AAR via Maven

Als u de `m2e` invoegtoepassing in Eclips, kunt u de afhankelijkheid in uw `pom.xml` bestand:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Optie 3: Gegevensbron via cijfer (laatste instantie)

Als u de broncode van de SDK via cijfer, invoeren:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Gebruik de tak **convergentie.**

## <a name="set-up-your-configuration-file"></a>Het configuratiebestand instellen

Gebruik van de configuratie die u hebt ingesteld in de portal B2C configureren voor de Android project eerdere.

Open `helpes/Constants.java` en vul de waarden voor de volgende handelingen uit:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: De bereiken die u doorgeven aan de server die u wilt aanvragen weer van de server wanneer een gebruiker zich aanmeldt. Het voorbeeld B2C u gebruikmaakt van `client_id`. Dit is echter normaal wijzigen naar `read scopes` in de toekomst. In dit document wordt bijgewerkt wanneer die optreedt.
- `ADDITIONAL_SCOPES`: Extra bereiken die u mogelijk wilt gebruiken voor uw toepassing. Ze zijn naar verwachting worden gebruikt in de toekomst.
- `CLIENT_ID`: De toepassings-ID u hebt ontvangen van de portal.
- `REDIRECT_URL`: De omleiding waar u het token moeten worden geboekt terug verwachten.
- `EXTRA_QP`: Geen extra hulpmiddelen u wilt doorgeven naar de server in een URL-codering-indeling.
- `FB_POLICY`: Het beleid dat u aanroept. Dit is de belangrijkste onderdeel voor dit overzicht.
- `EMAIL_SIGNIN_POLICY`: Het beleid dat u aanroept. Dit is de belangrijkste onderdeel voor dit overzicht.
- `EMAIL_SIGNUP_POLICY`: Het beleid dat u aanroept. Dit is de belangrijkste onderdeel voor dit overzicht.

## <a name="add-references-to-android-adal-to-your-project"></a>Verwijzingen naar Android ADAL toevoegen aan uw project

> [AZURE.NOTE]  Een model bedoelingen gebaseerde ADAL voor Android gebruikt om aan te roepen verificatie. Die 'pagina-indeling op' de app om te werken. In dit voorbeeld hele en alle ADAL voor Android, centers over het beheren van die en informatie doorgeven ertussen.

Eerst Android informeren over de indeling van uw toepassing, inclusief de die u wilt gebruiken. Deze die worden verderop in deze zelfstudie in detail beschreven.

Bijwerken van uw project `AndroidManifest.xml` bestand alle uw die worden opgenomen:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Zoals u ziet u vijf activiteiten worden gedefinieerd. Gebruikt u al deze.

- `AuthenticationActivity`: Dit is afkomstig uit ADAL en biedt de webweergave aanmelden.
- `LoginActivity`: Hiermee worden weergegeven in uw beleid aanmelden en de knoppen voor elk beleid.
- `SettingsActivity`: U dit gebruiken om de app-instellingen gedurende runtime te wijzigen.
- `AddTaskActivity`: U hiermee taken toevoegen aan uw REST API die zijn beveiligd met Azure AD.
- `ToDoActivity`: Dit is de belangrijkste activiteit die worden taken weergegeven.

## <a name="create-the-sign-in-activity"></a>Maken van de activiteit aanmelden

Maak een belangrijkste activiteiten en roep deze `LoginActivity`.

Maken van een bestand met de naam `LoginActivity.java`.

U moet de activiteit geïnitialiseerd en enkele knoppen waarmee u uw UI toevoegen. Dit is voor u als u Android code voordat u hebt geschreven.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
U hebt nu gemaakt knoppen die belt uw `ToDoActivity` bedoeling aangeeft (dat u belt ADAL wanneer u niet een token nodig). Ze doen dit met behulp van uw activiteit als een verwijzing en een extra parameter. Deze extra parameter wordt doorgegeven door de `intent.putExtra()` methode. U definieert `"thePolicy"` met behulp van wat u hebt opgegeven in `Constants.java`. Zo weet de bedoeling welk beleid aanroepen tijdens de verificatie.

## <a name="create-the-settings-activity"></a>De instellingen activiteit maken

Dit is een activiteit waarmee wordt gevuld uw UI-instellingen.

Maken van een bestand met de naam `SettingsActivity.java` voor eenvoudige maken, lezen, bijwerken en verwijderen (CRUD).

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>De activiteit toevoegen-taak maken

U kunt deze activiteit een taak toevoegen aan uw REST API-eindpunt.

Maken van een bestand met de naam `AddTaskActivity.java` en de volgende handelingen uit te schrijven.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>De activiteit van de lijst taak maken

Dit is de belangrijkste activiteit. U kunt gebruiken om toegang te krijgen een token van Azure AD voor een beleid en gebruik vervolgens die token om te bellen van de taak REST API-server.

Maken van een bestand met de naam `ToDoActivity.java` en de volgende handelingen uit te schrijven. (De oproepen worden beschreven later.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 U mogelijk opgevallen dat dit is afhankelijk van methoden die nog niet hebt zijn geschreven. De opties omvatten `updateLoggedInUser()`, `clearSessionCookie()`, en `getTasks()`. U kunt schrijven die verderop in deze handleiding. U kunt de fouten in Android Studio voorlopig negeren.

Uitleg van de parameters:

  - `SCOPES`: Vereist, de bereiken die u wilt toegang voor aanvragen. Voor het voorbeeld B2C het resultaat is hetzelfde als `client_id`, maar dit is normaal wijzigen in de toekomst.
  - `POLICY`: Het beleid voor het gewenste wanneer de gebruiker te verifiëren.
  - `CLIENT_ID`: Vereist, afkomstig is van de Azure AD-portal.
  - `redirectUri`: Kan worden ingesteld als de pakketnaam van uw. Het is niet vereist om te worden opgegeven voor de `acquireToken` bellen.
  - `getUserInfo()`: De manier om te zoeken of een gebruiker zich al in de cache. De parameter wordt ook beschreven hoe een gebruiker wordt gevraagd als die gebruiker niet wordt gevonden of als toegangstoken van de gebruiker ongeldig is. Deze methode wordt verderop in deze handleiding worden geschreven.
  - `PromptBehavior.always`: Helpt om te vragen om referenties de cache en cookie overgeslagen.
  - `Callback`: Nadat een autorisatiecode kan worden omgewisseld voor een token genoemd. Er is een object `AuthenticationResult`, die toegangstoken, vervaldatum en ID token gegevens bevat.

> [AZURE.NOTE]  Microsoft Intune app intune company portal vindt u het onderdeel makelaar en dit kan op van de gebruiker apparaat zijn geïnstalleerd. De app biedt eenmalige aanmelding (SSO) access voor alle toepassingen op het apparaat. Ontwikkelaars moeten zorg ervoor dat toestaan voor Intune. ADAL voor Android, wordt de makelaar-account gebruikt als er één gebruikersaccount gemaakt in de verificator. Als u wilt gebruiken de makelaar, de ontwikkelaar moet een speciale registreren `redirectUri` voor de makelaar gebruiken. `redirectUri`bevindt zich in de notatie van msauth://packagename/Base64UrlencodedSignature. U kunt ophalen `redirectUri` voor de app met behulp van het script `brokerRedirectPrint.ps1` of met behulp van het gesprek API `mContext.getBrokerRedirectUri()`. De handtekening die is gerelateerd aan uw ondertekend certificaten vanuit de Google Play store.

 U kunt de makelaar-gebruiker met behulp van overslaan:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Om te kunnen de complexiteit van deze snelstartgids B2C hebt aangegeven we de makelaar in ons voorbeeld overslaan.

Maak vervolgens methoden die het token alleen tijdens de verificatie-oproepen aan de taak API.

In dezelfde `ToDoActivity.java` bestand, de volgende handelingen uit te schrijven.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Ook toevoegen methoden die 'zet' en 'krijgt' `AuthenticationResult` (heeft uw token) aan het globale bestand `Constants`. Hoewel `ToDoActivity.java` gebruikt `sResult` in de stromen, moet u deze methoden toevoegen. Als u dat niet uw andere activiteiten geen toegang tot het token voor het werk (zoals het toevoegen van een taak in `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Maken van een methode om terug te keren een gebruikers-id

ADAL voor Android de gebruiker in de vorm van vertegenwoordigt een `UserIdentifier` object. Hiermee worden beheerd door de gebruiker. U kunt het object te bepalen of de gebruiker wordt gebruikt in uw oproepen worden gedaan. Deze informatie wordt gebruikt, kunt u gebruikmaken van de cache, plaats een nieuw gesprek starten op de server. Als u wilt dat dit gemakkelijker, die we hebben gemaakt de `getUserInfo()` methode die resulteert in `UserIdentifier`. U kunt deze met `acquireToken()`. We hebben ook gemaakt een `getUniqueId()` methode die resulteert in de ID van `UserIdentifier` in de cache.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Methoden schrijven

Sommige methoden waarmee u Schakel cookies en geef vervolgens schrijven `AuthenticationCallback`. Deze methoden worden alleen gebruikt voor de steekproef om ervoor te zorgen dat u in de beginwaarden bent wanneer u belt uw `ToDo` activiteit.

In hetzelfde bestand genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>De taak API bellen

Nadat u uw activiteiten gereed om te trekken van tokens hebt, kunt u uw API voor toegang tot de taak-server.

`getTasks`biedt een matrix die de taken in uw server vertegenwoordigt.

Beginnen met `getTasks`.

In hetzelfde bestand genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Schrijf ook een methode die wordt geïnitialiseerd tabellen op eerste uitvoeren.

In hetzelfde bestand genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

U kunt zien dat deze code is vereist voor enkele extra methoden uitvoeren. Schrijf de volgende.

### <a name="create-an-endpoint-url-generator"></a>Een eindpunt URL genereren maken

Moet u de URL van het eindpunt dat u een verbinding met maken hebt genereren. Doet u dat in hetzelfde class.

**In hetzelfde** genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Houd er rekening mee dat u het toegangstoken aan het vergaderverzoek in de code in het volgende gedeelte besproken toevoegen.

## <a name="write-some-ux-methods"></a>Sommige methoden UX schrijven

Android-apparaat, moet u enkele terugbellen voor het gebruik van de app verwerken. Hierna ziet u een `createAndShowDialog` en `onResume()`. Dit is voor u als u Android code voordat u hebt geschreven.

In hetzelfde bestand genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Vervolgens het beheren van uw terugbellen dialoogvenster.

In hetzelfde bestand genoemd `ToDoActivity.java`, de volgende handelingen uit te schrijven.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

U hebt nu een `ToDoActivity.java` -bestand dat wordt gecompileerd. Het hele project moet nu ook compileren.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Tot slot maken en uitvoeren van de app in Android Studio of Eclips. Registreren of de app aanmelden. Taken voor de gebruiker die zijn aangemeld bij maken. Meld u af en meld u weer aan als een andere gebruiker en maak vervolgens taken voor die gebruiker.

Zoals u ziet dat de taken die opgeslagen per gebruiker op de API zijn, omdat de API haalt de gebruikers id uit het toegangstoken dat deze ontvangt.

Voor verwijzing, in de voltooide voorbeeld [wordt weergegeven als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). U kunt dit ook klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Belangrijke informatie


### <a name="encryption"></a>Versleuteling

ADAL de tokens en store in worden gecodeerd `SharedPreferences` al dan niet standaard. U kunt bekijken de `StorageHelper` class de details kunnen zien. Android geïntroduceerd **AndroidKeyStore voor 4.3(API18)** veilige opslag van persoonlijke sleutels. ADAL gebruikt voor API18 en hoger. Als u ADAL voor onderste SDK versies gebruiken wilt, moet u een geheime sleutel aan te bieden `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Sessiecookies in de weergave

Android webweergave wist niet sessiecookies nadat de app is gesloten. Hiermee kunt u met de volgende code verwerken.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Meer informatie over cookies](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
