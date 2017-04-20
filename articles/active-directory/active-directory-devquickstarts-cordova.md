<properties
    pageTitle="Azure AD Cordova aan de slag | Microsoft Azure"
    description="Het maken van een Cordova-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en u belt Azure AD beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure AD integreren met een Apache Cordova-app

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova maakt het mogelijk u HTML5/JavaScript-toepassingen die kunnen worden uitgevoerd op mobiele apparaten als volwaardige systeemeigen toepassingen ontwikkelen.
U kunt met Azure AD enterprise grade verificatie mogelijkheden toevoegen aan uw Cordova-toepassingen. U kunt uw toepassing ondersteuning voor aanmelding op met uw gebruikers AD-accounts, toegang krijgen tot Office 365 en Azure-API en zelfs beveiligen oproepen naar uw eigen aangepaste Web-API verbeteren met behulp van een Cordova Plug Azure AD systeemeigen SDK's voor tekstterugloop op iOS, Android-, Windows Store- en Windows Phone.

In deze zelfstudie we de Apache Cordova-invoegtoepassing voor Active Directory verificatie bibliotheek (ADAL) gebruiken om te verbeteren van een eenvoudige app met de volgende functies:

-   Met een paar regels met code, een AD-gebruiker te verifiëren en een token voor het bellen van de Azure AD Graph-API verkrijgen.
-   Deze token gebruiken om aan te roepen van de grafiek-API voor het opvragen van die map en de resultaten weergeven  
-   Gebruikmaken van de ADAL token cache voor de verificatie aanwijzingen voor de gebruiker te minimaliseren.

Hiervoor moet u:

2. Een toepassing met Azure AD registreren
2. Code toevoegen aan uw app verzoek tokens
3. Code voor het gebruik van de token voor query's in de grafiek-API toevoegen en weergeven van resultaten.
4. Het Cordova implementatieproject maken met alle platforms die u wilt de doelgroep en de invoegtoepassing voor Cordova ADAL en testen van de oplossing in emulators.

## <a name="0--prerequisites"></a>*0. vereisten voor*

Voltooi deze zelfstudie u moet als volgt:

- Een Azure AD-tenant waar u een account met machtigingen app hebt
- Een ontwikkelomgeving die is geconfigureerd voor gebruik van Apache Cordova  

Als u beide al hebt ingesteld, gaat u rechtstreeks naar stap 1.

Als u een Azure AD-tenant niet hebt, kunt u vinden [instructies voor hoe u er een hier](active-directory-howto-tenant.md).

Als u geen Apache Cordova instellen op uw computer, installeer het volgende:

- [Cijfer](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (eenvoudig kan worden geïnstalleerd via NPM pakket manager: `npm install -g cordova`)

Houd er rekening mee dat die op de PC zowel op de Mac. werken moeten

Elk doelplatform heeft verschillende vereisten.

- Maken en uitvoeren/PC met Windows Tablet of telefoon app-versie
    - [Visual Studio 2013 voor Windows met Update 2 of hoger](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express of een andere versie).
- Maken en uitvoeren voor iOS
    -   Xcode 5.x of hoger. Deze http://developer.apple.com/downloads of de [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) downloaden
    -   [ios-sim](https://www.npmjs.org/package/ios-sim) – kunt u iOS-apps in de iOS Simulator vanaf de opdrachtregel starten (eenvoudig kan worden geïnstalleerd via de terminal: `npm install -g ios-sim`)

- Toepassing voor Android kunt maken en uitvoeren
    - Installeer [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger. Zorg ervoor dat `JAVA_HOME` (omgevingsvariabele) correct is ingesteld op basis van de JDK installatiepad (bijvoorbeeld C:\Program Files\Java\jdk1.7.0_75).
    - [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) installeren en toevoegen `<android-sdk-location>\tools` locatie (bijvoorbeeld C:\tools\Android\android-sdk\tools) naar uw `PATH` omgevingsvariabele.
    - Open Android SDK beheren (bijvoorbeeld via terminal: `android`) en installeren
    - *Android 5.0.1 (API 21)* platform SDK
    - *Android SDK opbouwen-hulpmiddelen voor* versie 19.1.0 of hoger
    - *Ondersteuning voor android-bibliotheek* (Extra's)

  Android sdk zelf niet een standaardexemplaar emulator. Maak een nieuwe door te voeren `android avd` uit terminal en *Create...* te selecteren als u wilt uitvoeren van Android-app op emulator. Aanbevolen *Api niveau* is 19 of hoger, Zie AVD Manager (http://developer.android.com/tools/help/avd-manager.html) voor meer informatie over opties voor Android emulator en maken.


## <a name="1--register-an-application-with-azure-ad"></a>*1. een toepassing met Azure AD registreren*

Opmerking: deze __stap is optioneel__. De zelfstudie aangeboden vooraf ingerichte waarden waarmee u de steekproef in actie zien zonder te moeten doen eventuele inrichting in uw eigen tenant is ingeschakeld. Het is echter aanbevolen dat u deze stap uitvoeren en vertrouwd raken met het proces, te zoals deze uitgevoerd moeten worden wanneer u uw eigen toepassingen wilt maken.

Azure AD verleent slechts tokens naar bekende toepassingen. Voordat u Azure AD vanuit uw app gebruiken kunt, moet u een item maken voor dit in uw tenant.  Een nieuwe toepassing registreren in uw tenant,

-   Meld u aan bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer de tenant waar u wilt registreren van de toepassing.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **Systeemeigen clienttoepassing** (ondanks het feit dat Cordova apps HTML gebaseerd zijn, zijn we hier systeemeigen clienttoepassing dus maken `Native Client Application` moet optie selecteren; anders de toepassing werkt niet).
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **URI omleiden** is de URI gebruikt om terug te keren tokens naar uw app. Voer `http://MyDirectorySearcherApp`.

Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties: u kunt vinden in het tabblad **configureren** van de nieuwe app.

Om te kunnen uitvoeren `DirSearchClient Sample`, de zojuist gemaakte app machtiging om query's in de _grafiek van Azure AD-API_:
-   Ga naar de sectie "Machtigingen aan andere toepassingen" tabblad **configureren** .  Voor de toepassing "Azure Active Directory" toevoegen de toegangsmachtiging **de map als de gebruiker die zijn aangemeld bij** onder **Gedelegeerde machtigingen**.  Hiermee schakelt u de toepassing om query's in de grafiek-API voor gebruikers.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. klonen de steekproef app opslagplaats vereist voor de zelfstudie*

Klik vanuit uw shell of de opdrachtregel, typt u de volgende opdracht uit:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. de Cordova-app maken*

Er zijn verschillende manieren voor het maken van Cordova-toepassingen. We gebruiken de Cordova opdrachtregelinterface () in deze zelfstudie.
Klik vanuit uw shell of de opdrachtregel, typ de volgende opdracht uit:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Die de mapstructuur en steigers voor het project Cordova, de inhoud van het project starter kan kopiëren in de submap www maakt.
Verplaatsen naar de nieuwe DirSearchClient-map.

    cd .\DirSearchClient

De invoegtoepassing "witte" lijst, nodig om de grafiek-API aan te roepen toevoegen.

     cordova plugin add cordova-plugin-whitelist

Voeg nu alle platforms moeten worden ondersteund. Om een steekproef werken, moet u ten minste één van de onderstaande opdrachten uitvoeren. Houd er rekening mee dat is niet mogelijk emuleert iOS op Windows of Windows/Windows Phone op een Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Tot slot kunt u de ADAL voor Cordova Plug-in toevoegen aan uw project.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. code toevoegen om te verifiëren van gebruikers en tokens van AAD verkrijgen*

De toepassing die u in deze zelfstudie ontwikkelt, vindt u een zoekfunctie b bEen directory, waar de eindgebruiker kunt typt u de alias van elke gebruiker in de map en enkele eenvoudige kenmerken visualiseren.  Het project starter met de definitie van de eenvoudige gebruikersinterface van de app (in www/index.html) en de steiger die kabels van eenvoudige app gebeurtenis bladert, gebruikersinterface bindingen en resultaten weergeven logica (in www/js/index.js). Het enige wat u bent vergeten, is de uitvoering van identiteit taken logica toevoegen.

Het eerste wat dat u moet doen is in te voeren in uw code de protocol-waarden die worden gebruikt door AAD om uw app en de bronnen die u snel aan te duiden. Deze waarden worden gebruikt om de token aanvragen later te maken. Het fragment onder invoegen op de menubalk van het bestand index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

De `redirectUri` en `clientId` waarden moeten overeenkomen met de waarden die met een beschrijving van uw app in AAD. U vindt die van het tabblad configureren in de Azure-portal, zoals beschreven in stap 1 eerder in deze zelfstudie.
Opmerking: als u ervoor hebt gekozen voor het registreren van een nieuwe app niet in uw eigen tenant is ingeschakeld, kunt u gewoon plakken de vooraf geconfigureerde bovenstaande waarden is - waarmee u om te zien van de steekproef actief is, hoewel u moet altijd uw eigen vermelding maken voor uw apps is bedoeld voor productie.

Vervolgens moet de werkelijke token verzoekcode hebt toegevoegd. Voeg het volgende fragment tussen de `search `en `renderdata `definities.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Laten we bestudeert u deze functie door deze te splitsen in de twee hoofdgebieden.
In dit voorbeeld is ontworpen voor gebruik met een tenant, in plaats om te worden gekoppeld aan een bepaald. Site gebruikmaakt van het eindpunt '/ algemene', waarmee de gebruiker een account in verificatie keer invoeren en stuurt de aanvraag de tenant behoort.
Deze eerste deel van de methode Hiermee wordt gecontroleerd met de ADAL cache om te zien of er al een opgeslagen token bestaat - en, als er, wordt de tenants die het afkomstig is voor het opnieuw geïnitialiseerd ADAL gebruikt. Dit is nodig om te voorkomen extra aanwijzingen, zoals het gebruik van '/ algemene"altijd resulteert in het vragen van de gebruiker moet een nieuw account invoeren.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Het tweede deel van de methode kunt u het verzoek BEGINLETTERS tokewn uitvoeren.
De `acquireTokenSilentAsync` methode wordt gevraagd naar ADAL om terug te keren een token voor de opgegeven resource zonder eventuele UX. weer te geven Dat kan gebeuren als de cache al een geschikte toegangstoken dat is opgeslagen heeft, of als er een token vernieuwen die kan worden gebruikt om een nieuwe toegangstoken zonder shwoing elke vraag.
Als die poging mislukt, we terugschakelen op `acquireTokenAsync` -die de gebruiker om te verifiëren zichtbaar wordt gevraagd.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nu dat we het token hebben, kunnen we ten slotte aanroepen van de grafiek-API en uitvoeren van de zoekopdracht die we horen. Voeg het volgende fragment direct onder de `authenticate` definitie.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
De eerste punt-bestanden die een barebone UX zijn opgegeven voor het invoeren van een gebruiker alias in een tekstvak. Deze methode gebruikt deze waarde om te maken van een query, combineren met het toegangstoken Stuur dit naar de grafiek en de resultaten parseren. De methode renderData, al in het eerste punt-bestand, zorgt de resultaten wilt visualiseren.

## <a name="4-run"></a>*4. uitvoeren*
Uw app is ten slotte gereed om uit te voeren! Het besturingssysteem is heel eenvoudig: zodra de app wordt gestart, voert u in het tekstvak de alias van de gebruiker die u wilt opzoeken - Klik op de knop. U wordt gevraagd voor verificatie. Na een geslaagde verificatie en zoekactie, wordt de kenmerken van de gezochte gebruiker weergegeven. Volgende uitgevoerd wordt gezocht de zonder een prompt met behulp van de aanwezigheid in de cache van het token eerder hebt aangeschaft weer te geven.
De Betonnen stappen voor het uitvoeren van de app varieert per platform.

####<a name="windows-10"></a>Windows 10:

   Tablet-PC:`cordova run windows --archs=x64 -- --appx=uap`

   Mobiel (hiervoor Windows10 mobiele apparaat is verbonden met de PC):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Opmerking__: tijdens de eerste uitvoering die u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie [Developer-licentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) voor meer informatie.

####<a name="windows-81-tabletpc"></a>Tablet-PC met Windows 8.1:

   `cordova run windows`

   __Opmerking__: tijdens de eerste uitvoering die u mogelijk gevraagd aan te melden voor een ontwikkelaarslicentie. Zie [Developer-licentie](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) voor meer informatie.

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Om uit te voeren op verbonden apparaat:`cordova run windows --device -- --phone`

   Klik op standaard emulator uitvoeren:`cordova emulate windows -- --phone`

   Gebruik `cordova run windows --list -- --phone` om alle beschikbare doelen weer te geven en `cordova run windows --target=<target_name> -- --phone` toepassingen op specifieke apparaat of de emulator kunnen uitvoeren (bijvoorbeeld `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Om uit te voeren op verbonden apparaat:`cordova run android --device`

   Klik op standaard emulator uitvoeren:`cordova emulate android`

   __Opmerking__: Controleer of u kunt emulator exemplaar *AVD Manager* gebruiken, zoals deze wordt weergegeven in de sectie *voorwaarden* hebt gemaakt.

   Gebruik `cordova run android --list` om alle beschikbare doelen weer te geven en `cordova run android --target=<target_name>` toepassingen op specifieke apparaat of de emulator kunnen uitvoeren (bijvoorbeeld `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Om uit te voeren op verbonden apparaat:`cordova run ios --device`

   Klik op standaard emulator uitvoeren:`cordova emulate ios`

   __Opmerking__: Zorg ervoor dat u hebt `ios-sim` pakket geïnstalleerd om uit te voeren op emulator. Zie *vereisten voor* de sectie voor meer informatie.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Gebruik `cordova run --help` Zie extra samenstellen en uitvoeren van de opties.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  U kunt nu gaat u naar meer geavanceerde scenario's (ok en interessanter).  U kunt proberen:

[Een Node.js Web API met Azure AD Secure >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
