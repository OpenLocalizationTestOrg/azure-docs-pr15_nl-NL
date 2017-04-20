<properties
    pageTitle="Azure AD AngularJS aan de slag | Microsoft Azure"
    description="Het maken van een hoek JS één pagina-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en Azure AD-oproepen beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>AngularJS één pagina Apps gebruiken met Azure AD beveiligen

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD kunt u eenvoudig en eenvoudig kunt aanmelden in afmelden en secure OAuth-API oproepen naar uw apps één pagina.  U kunt uw app bij het verifiëren van gebruikers met hun Active Directory-accounts en eventuele web API die zijn beveiligd met Azure AD, zoals de Office 365-API of de Azure-API gebruiken.

Voor javascript-toepassingen uitgevoerd in een browser, biedt Azure AD de Active Directory-verificatie Library of adal.js.  De enige doel van Adal.js in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen.  U kunt zien hoe gemakkelijk het is, bouw hier we een toepassing AngularJS takenlijst die:

- De gebruiker kunt zich aanmelden bij de app met Azure AD als de identiteitsprovider.
- Geeft vindt u informatie over de gebruiker.
- Veilig oproepen van de app om te doen lijst API vruchtdragende tokens uit AAD gebruiken.
- De gebruiker afmelden bij de app wordt ondertekend.

Als u wilt de volledige werken-toepassing maakt, moet u:

2. Uw toepassing met Azure AD registreren.
3. ADAL installeren en configureren van de beveiligd-WACHTWOORDVERIFICATIE.
5. Gebruik ADAL u pagina's in de beveiligd-WACHTWOORDVERIFICATIE beveiligt.

Om gestart, [de app-skelet downloaden](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  U moet ook een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren.  Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directorysearcher-application"></a>*1. de toepassing DirectorySearcher registreren*
Als u wilt uw app bij het verifiëren van gebruikers en tokens krijgen inschakelen, moet u eerst registreren in uw Azure AD-tenant:

-   Meld u aan bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven.
    -   De **Uri omleiden** is locatie waaraan AAD tokens zullen retourneren.  De standaardlocatie voor dit voorbeeld is`https://localhost:44326/`
-   Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke **ID van de Client**.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad **configureren** .
- De stroom van OAuth-impliciete Adal.js gebruikt om te communiceren met Azure AD.  U moet de impliciete stroom inschakelen voor uw toepassing door:
    - Download manifest van de toepassing door te klikken op **Bestandenlijst beheren**.
    - Open het manifest en zoek naar de `oauth2AllowImplicitFlow` eigenschap. De waarde instellen op `true`.
    - Slaat & manifest van de toepassing door nogmaals te **Beheren bestandenlijst** klikken op uploaden.

## <a name="2-install-adal--configure-the-spa"></a>*2. ADAL installeren en configureren van de beveiligd-WACHTWOORDVERIFICATIE*
U kunt nu dat u een toepassing in Azure AD hebt, adal.js installeren en uw identiteit-gerelateerde programmacode schrijven.

-   Beginnen met het adal.js toevoegen aan het TodoSPA project met de Console Package Manager:
  - [Adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) downloaden en toe te voegen aan de `App/Scripts/` project directory.
  - Downloaden van [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) en toe te voegen aan de `App/Scripts/` project directory.
  - Laden van elk script vóór het einde van de `</body>` in `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Voor een van de beveiligd-WACHTWOORDVERIFICATIE backend naar doen lijst-API accepteren tokens afgeleid van de browser, moet de backend configuratie-informatie over de registratie van de app. Open in het project TodoSPA `web.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie om de waarden die u in de Portal Azure invoert aan te geven.  Uw code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `ida:Tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    -   De `ida:Audience` moet de **Client-ID** van de toepassing die u hebt gekopieerd in de portal.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. ADAL gebruiken voor het beveiligen van pagina's in de beveiligd-WACHTWOORDVERIFICATIE*
Adal.js heeft zijn gebouwd om te integreren met AngularJS route en http providers, waarmee u beveiligde afzonderlijke weergaven in uw beveiligd-WACHTWOORDVERIFICATIE.

- In `App/Scripts/app.js`, in de module adal.js weer:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- U kunt nu geïnitialiseerd de `adalProvider` met de configuratiewaarden van de registratie van uw toepassing, ook in `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Nu u beveiligt de `TodoList` weergeven in de app, slechts één regel met code nodig is - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

U hebt nu een toepassing voor de beveiligde één pagina met de mogelijkheid gebruikers aanmelden en aan toonder token beveiligde aanvragen verlenen aan de backend-API.  Wanneer een gebruiker de `TodoList` koppeling adal.js worden automatisch omgeleid naar Azure AD voor het aanmelden indien nodig.  Bovendien adal.js automatisch aan te koppelen een access_token ajax verzoeken die zijn verzonden naar de backend van de toepassing.  De bovenstaande is absoluut minimale nodig voor het maken van een beveiligd-WACHTWOORDVERIFICATIE met adal.js - maar er zijn een aantal andere functies die in kuuroorden handig zijn:

- U kunt functies definiëren in uw controllers die adal.js roepen expliciet probleem aanmelden en afmelden aanvragen.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- U kunt ook om gebruikersgegevens te presenteren in de gebruikersinterface van de app.  De adal service al is toegevoegd aan de `userDataCtrl` controller, zodat u toegang hebt tot de `userInfo` object in de bijbehorende weergave `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Er zijn ook veel scenario's waarin u weten wilt als de gebruiker is aangemeld of niet.  U kunt ook de `userInfo` object om deze informatie te verzamelen.  Bijvoorbeeld in `index.html` kunt u de "Login" of "Afmelden"-knop op basis van de verificatiestatus weergeven:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Gefeliciteerd! Uw Azure AD zijn geïntegreerd met één pagina App is voltooid.  U kunt gebruikers verifiëren, veilig bellen de backend met OAuth 2.0 en basisinformatie over de gebruiker.  Als u nog niet is gedaan, is nu de tijd aan het vullen van uw tenant met enkele gebruikers.  Voer uw naar doen lijst beveiligd-WACHTWOORDVERIFICATIE en meld u aan met een van deze gebruikers.  Voeg taken toe aan de gebruikers moet u doen vermelden, meld u af en meld u weer aan.

Adal.js kunt u heel gemakkelijk al deze algemene identiteit functies opnemen in uw toepassing.  Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  U kunt nu gaat u naar extra scenario's.  U kunt proberen:

[Een CORS Web API bellen vanuit een beveiligd-WACHTWOORDVERIFICATIE >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
