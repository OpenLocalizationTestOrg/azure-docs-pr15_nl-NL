<properties
    pageTitle="Azure Active Directory v2.0 Android-app | Microsoft Azure"
    description="Het maken van een Android-app die zich aanmeldt gebruikers met zowel persoonlijke Microsoft-account en werk- of schoolaccounts en oproepen de API Graph met behulp van derden bibliotheken."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Aanmeldingsproblemen toevoegen aan een Android-app met behulp van een derde partij-bibliotheek met behulp van het eindpunt v2.0 Graph-API

Het Microsoft-platform identiteit wordt open standaarden zoals OAuth2 en verbindt u OpenID gebruikt. Ontwikkelaars kunnen een bibliotheek die ze willen integreren met onze services gebruiken. Ontwikkelaars ons platform gebruiken met andere bibliotheken, zodat hebt we een paar walkthroughs zoals dit het configureren van derden bibliotheken verbinding maken met het Microsoft-platform identiteit aantonen geschreven. De meeste bibliotheken waarin [de specificatie RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementeren kunnen koppelen aan het Microsoft-platform identiteit.

Zorg dat de toepassing die deze procedure wordt gemaakt, kunnen gebruikers aanmelden bij hun organisatie en zoek naar zelf in hun organisatie met behulp van de grafiek-API.

Als u geen ervaring met OAuth2 of OpenID verbinden hebt, logisch veel van deze steekproef-configuratie niet voor u. Het is aan te raden wij u [2.0 protocollen - OAuth 2.0 autorisatie Code Flow](active-directory-v2-protocols-oauth-code.md) voor achtergrond lezen.

> [AZURE.NOTE] Sommige functies van onze platform die een expressie in de standaarden OAuth2 of OpenID verbinden, zoals voorwaardelijke toegang en beheer van Intune hebben, moeten u de bron van onze openen Microsoft Azure identiteit bibliotheken gebruiken.

Het eindpunt v2.0 biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen.

> [AZURE.NOTE] Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>De code uit GitHub downloaden
De code voor deze zelfstudie worden [GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) of het skelet klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

U kunt ook gewoon downloaden van de steekproef en meteen aan de slag:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app bij de [portal voor registratie van toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)maken of de gedetailleerde stappen voor [het registreren van een app aan het eindpunt v2.0](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopieer de **Toepassings-Id** die toegewezen aan uw app omdat u deze snel moet.
- De **Mobile** -platform voor de app toevoegen.

> Opmerking: De toepassing registratie-portal vindt u een waarde **URI omleiden** . In dit voorbeeld moet u de standaardwaarde van gebruiken `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Downloaden van de bibliotheek van de derde partij NXOAuth2 en een werkruimte maken

Voor deze procedure gebruikt u de OIDCAndroidLib van GitHub, dat wil een OAuth2-bibliotheek op basis van de code OpenID verbinding van Google zeggen. Deze implementeert het profiel van de bijbehorende toepassing en het eindpunt autorisatie van de gebruiker ondersteunt. Hierna ziet u alle dingen die u moet integreren met het Microsoft-platform identiteit.

Klonen de cessies‑retrocessies OIDCAndroidLib naar uw computer.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Uw omgeving Android Studio instellen

1. Maak een nieuw project van Android Studio en accepteer de standaardinstellingen in de wizard.

    ![Nieuw project maakt in Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Android-apparaten van doeltoepassing](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Een activiteit toevoegen aan mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Instellen van uw projectmodules, verplaatst u de gekloonde cessies‑retrocessies naar de projectlocatie. U kunt ook het project te maken en te klonen rechtstreeks naar de projectlocatie.

    ![Projectmodules](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Open de instellingen van de modules project met behulp van het contextmenu of met behulp van de sneltoets Ctrl + Alt + rimaire + S.

    ![Project modules-instellingen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Verwijder de standaard-app-module omdat u wilt dat alleen de instellingen van de container project.

    ![De standaard-app-module](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Modules uit de gekloonde cessies‑retrocessies importeren naar het huidige project.

    ![Project voor importeren gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![nieuwe module pagina maken](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Herhaal deze stappen voor het `oidlib-sample` module.

7. Controleren of de afhankelijkheden oidclib de `oidlib-sample` module.

    ![oidclib afhankelijkheden van de module oidlib gepaarde steekproeven](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klik op **OK** en wacht totdat gradle synchroniseren.

    Uw settings.gradle ziet er als:

    ![Schermafbeelding van settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Maken van de steekproef-app om ervoor te zorgen dat de steekproef correct kan worden uitgevoerd.

    Niet mogelijk nog gebruiken met Azure Active Directory. We moet eerst enkele eindpunten configureren. Dit is om ervoor te zorgen er geen een problemen Android Studio laten we beginnen met het aanpassen van de steekproef-app.

10. Maken en uitvoeren `oidlib-sample` als het doel in Android Studio.

    ![Voortgang van oidlib gepaarde steekproeven opbouwen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Verwijder de `app ` directory die is gegeven wanneer u de module verwijderd uit het project omdat Android Studio deze voor de veiligheid niet verwijderen.

    ![Bestandsstructuur met de app-gids](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Open het menu **Bewerken configuraties** om het verwijderen van de uitvoeren configuratie die is ook links wanneer u de module hebt verwijderd uit het project.

    ![Configuraties menu Bewerken](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![configuratie van app uitvoeren](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>De eindpunten van de steekproef configureren

Nu de `oidlib-sample` is uitgevoerd, laten we eens enkele eindpunten als u deze werken met Azure Active Directory bewerken.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Uw client configureren door de oidc_clientconf.xml-bestand te bewerken

1. Aangezien u OAuth2 loopt alleen voor het ophalen van een token en de grafiek-API aanroepen gebruikt, stelt u de client OAuth2 alleen doen. OIDC komen in een latere voorbeeld.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Configureer uw klant-ID die u hebt ontvangen van de registratie-portal.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Uw redirect URI met de onderstaande configureren.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Configureer uw bereiken die u nodig hebt bij het openen van de grafiek-API.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

De `User.Read` waarde in `oidc_scopes` kunt u het profiel basis de ondertekende lezen in de gebruiker.
U kunt meer informatie over alle beschikbare bereiken bij [Microsoft Graph machtiging bereiken](https://graph.microsoft.io/docs/authorization/permission_scopes).

Als u uitleg over `openid` of `offline_access` zoals bereiken in OpenID verbinding maken, bekijken [2.0 protocollen - OAuth 2.0 autorisatie Code Flow](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>De eindpunten van de client configureren door de oidc_endpoints.xml-bestand te bewerken

- Open de `oidc_endpoints.xml` -bestand en de volgende wijzigingen aanbrengen:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Deze eindpunten moeten nooit wijzigen als u OAuth2 als uw protocol gebruikt.

> [AZURE.NOTE]
De eindpunten voor `userInfoEndpoint` en `revocationEndpoint` worden momenteel niet ondersteund door Azure Active Directory. Als u deze met de standaardwaarde voor example.com laat, wordt u eraan herinnerd dat ze niet beschikbaar in de steekproef zijn :-)


## <a name="configure-a-graph-api-call"></a>Een grafiek API-aanroep configureren

- Open de `HomeActivity.java` -bestand en de volgende wijzigingen aanbrengen:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Een eenvoudige grafiek API-oproep geeft hier onze informatie.

Dit zijn alle wijzigingen die u moet doen. Voer de `oidlib-sample` -toepassing, en klik op **aanmelden**.

Nadat u hebt geverifieerd, selecteert u de knop **Beveiligde bron aanvragen** om te testen van uw gesprek tot de API Graph.

## <a name="get-security-updates-for-our-product"></a>Beveiligingsupdates voor onze product

We raden u meldingen ontvangen over-incidenten om door te bezoeken het [Security TechCenter](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
