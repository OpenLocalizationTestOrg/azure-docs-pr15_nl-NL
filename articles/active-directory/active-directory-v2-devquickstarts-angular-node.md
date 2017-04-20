<properties
    pageTitle="Azure AD v2.0 AngularJS aan de slag | Microsoft Azure"
    description="Het maken van een hoek JS één pagina-app die zich aanmeldt gebruikers met beide persoonlijk Microsoft-accounts en werk- of schoolaccount accounts."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Aanmelden bij een AngularJS één pagina app - NodeJS toevoegen

In dit artikel zullen we Meld u aan met Microsoft mogelijk gemaakt-accounts toevoegen aan een AngularJS-app met het eindpunt van de v2.0 Azure Active Directory. het eindpunt v2.0 kunt u een één-integratie in uw app uit te voeren en verifiëren van gebruikers met persoonlijke en werk-, school-accounts.

In dit voorbeeld is een eenvoudige takenlijst één pagina app waarin taken opgeslagen in een backend REST API, geschreven in NodeJS en beveiligd met OAuth vruchtdragende tokens afgeleid van Azure AD.  De app AngularJS wilt onze bron openen JavaScript verificatie bibliotheek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) gebruiken voor de hele aanmelden proces verwerken en in het bezit van tokens voor het bellen van de REST API.  Hetzelfde patroon kan worden toegepast als u wilt verifiëren bij andere REST API's, zoals de [Microsoft Graph](https://graph.microsoft.com) of de Azure resourcemanager API's.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="download"></a>Downloaden

Als u wilt beginnen, moet u downloaden en installeren van [node.js](https://nodejs.org).  Vervolgens kunt u kopiëren of [gedownload](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) skelet app:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

De skelet app bevat de standaardtekst-code voor een eenvoudige AngularJS-app, maar ontbreekt alle van de identiteit-gerelateerde onderdelen.  Als u niet volgen wilt, kunt u in plaats daarvan kopiëren of [downloaden](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) de voltooide steekproef.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Een app hebt geregistreerd

Eerst een app maken in de [Portal voor registratie van App](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- De **Web** -platform voor de app toevoegen.
- Voer het juiste **URI omleiden**. De standaardindeling voor dit voorbeeld is `http://localhost:8080`.
- Laat het selectievakje **Impliciete Flow toestaan** is ingeschakeld. 

Kopiëren naar beneden de **Toepassings-ID** die is toegewezen aan uw app, moet u deze binnenkort. 

## <a name="install-adaljs"></a>Adal.js installeren
Als u wilt beginnen, Ga naar project u gedownload en adal.js installeren.  Als er [bower](http://bower.io/) is geïnstalleerd, kunt u alleen deze opdracht uitvoeren.  Voor eventuele problemen afhankelijkheid versie, kiest u een nieuwere versie.
```
bower install adal-angular#experimental
```

U kunt ook handmatig downloaden [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) en [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Als beide bestanden toevoegen de `app/lib/adal-angular-experimental/dist` directory.

Open het project in uw favoriete teksteditor nu en laden adal.js aan het einde van de Paginahoofdtekst:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>De REST API instellen

Terwijl we dingen die u instelt bent, kunt de back-end REST API starten (Engelstalig).  In een opdrachtprompt door alle benodigde pakketten te installeren door te voeren (Zorg ervoor dat u bent in de bovenste map van het project):

```
npm install
```

Nu openen `config.js` en vervang het `audience` waarde:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

De REST API gebruikt deze waarde om deze uit de hoek-app op AJAX-aanvragen ontvangt tokens te valideren.  Opmerking dat deze eenvoudige REST API worden opgeslagen gegevens in het geheugen - zodat ieder afstemmen met de server stoppen, gaan verloren alle eerder gemaakte taken.

Dit is altijd gaan we besteedt bespreekt de werking van de REST API.  Je mag rustig Maak rond de code, maar als u meer informatie wilt over het beveiligen van web API's met Azure AD, neemt u [in dit artikel](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Aanmeldingsadres gebruikers in
Tijd aan sommige identiteit programmacode schrijven.  U mogelijk al opgevallen dat adal.js bevat een provider AngularJS, die geschikt aangepast met hoek routeren regelingen is.  Begin met het toevoegen van de adal module bij de app:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

U kunt nu geïnitialiseerd de `adalProvider` met uw toepassings-ID:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Uitstekende, adal.js bevat nu alle informatie die nodig is om te beveiligen van uw app en meld u aan gebruikers.  Als u aanmelden voor een bepaalde route in de app, is een kwestie van één regel met code:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Nu wanneer een gebruiker de `TodoList` koppeling adal.js worden automatisch omgeleid naar Azure AD voor aanmelding indien nodig.  U kunt ook expliciet aanmelden en afmelden aanvragen door aan te roepen adal.js in uw domeincontrollers verzenden:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Gebruikersgegevens weergeven
Nu dat de gebruiker is aangemeld, moet u waarschijnlijk voor toegang tot de verificatiegegevens ondertekend in van de gebruiker in uw toepassing.  Adal.js beschrijft deze informatie voor u in de `userInfo` object.  Voor toegang tot dit object in een weergave, moet u eerst adal.js toevoegen aan het bereik van de hoofdsite van de corresponderende controller uit:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

En u kunt rechtstreeks adres van de `userInfo` object in uw weergave: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

U kunt ook de `userInfo` object om te bepalen als aan de gebruiker is aangemeld of niet.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>De REST API bellen
Ten slotte, is het tijd om enkele tokens en bellen van de REST API als u wilt maken, lezen, bijwerken en verwijderen van taken.  Ook raden wat?  U hoeft te doen *een ding*.  Adal.js wordt automatisch kunt verrichten ophalen, caching en tokens vernieuwen.  Dit wordt ook kunt verrichten deze tokens toevoegen aan uitgaande AJAX-aanvragen die u naar de REST API verzendt.  

Hoe werkt dit? Is alles met behulp van de magie van [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), waarmee adal.js transformeren uitgaande en binnenkomende HTTP-berichten.  Bovendien adal.js wordt ervan uitgegaan dat alle aanvragen aan hetzelfde domein verzenden als het venster tokens bedoeld voor de ID van dezelfde als de app AngularJS moet gebruiken.  Dit is de reden waarom we de dezelfde toepassings-ID in de hoek app en in de NodeJS REST API gebruikt.  Uiteraard kunt u kunt dit probleem negeren en vertel adal.js om tokens voor andere REST API's zo nodig - maar voor dit eenvoudige scenario posities wordt uitvoeren.

Hier volgt een fragment dat wordt weergegeven hoe makkelijk het is om te verzoeken met vruchtdragende tokens verzenden vanaf Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gefeliciteerd!  Uw app Azure AD-geïntegreerde één pagina is voltooid.  Verdergaan, strik duren.  U kunt gebruikers verifiëren, veilig bellen de backend REST API met OpenID verbinden en basisinformatie over de gebruiker.  Afmelden bij het vak biedt ondersteuning voor elke gebruiker met een persoonlijk Microsoft-Account of een werk-, school-account van Azure AD.  Geef de app uitproberen door te voeren:

```
node server.js
```

Ga in een browser naar `http://localhost:8080`.  Meld u aan met een persoonlijk Microsoft-account of een werk-, school-account.  Taken toevoegen aan de takenlijst van de gebruiker en meld u af.  Probeer aan te melden met het andere type account. Als u een Azure AD-tenant werk-, school-gebruikers, maken nodig [leren hoe u een hier](active-directory-howto-tenant.md) (dit is gratis).

Om te blijven leren over de het eindpunt v2.0, terug naar onze [v2.0-handleiding voor ontwikkelaars](active-directory-appmodel-v2-overview.md).  Raadpleeg voor meer bronnen:

- [Azure-voorbeelden op GitHub >>](https://github.com/Azure-Samples)
- [Azure AD in stapel overloop >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD-documentatie op [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
