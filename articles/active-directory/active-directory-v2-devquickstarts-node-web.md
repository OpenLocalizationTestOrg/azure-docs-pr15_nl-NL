<properties
    pageTitle="Azure AD v2.0 NodeJS Web-App | Microsoft Azure"
    description="Het maken van een knooppunt JS-WebApp die zich gebruikers met beide persoonlijk Microsoft-Account en accounts voor werk- of schoolaccount."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Aanmeldingsproblemen toevoegen aan een nodeJS Web App


> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).


Hier gebruiken we Passport aan:

- De gebruiker aan te melden bij de app met Azure AD en het eindpunt v2.0.
- Vindt u informatie over de gebruiker weergeven.
- Meld u aan de gebruiker afmelden bij de app.

**Passport** is verificatie middleware voor Node.js. Zeer flexibel en modulaire, Passport kan worden onopvallend verwijderd voor elk Express- of Resitify webtoepassing. Een uitgebreide reeks strategieën ondersteuning voor verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We dit module hebt geïnstalleerd en voegt u de Microsoft Azure Active Directory `passport-azure-ad` invoegtoepassing.

## <a name="download"></a>Downloaden

De code voor deze zelfstudie worden [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) of het skelet klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

De voltooide toepassing wordt aangeboden aan het einde van deze zelfstudie ook.

## <a name="1-register-an-app"></a>1. register een App
Een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- De **Web** -platform voor de app toevoegen.
- Voer het juiste **URI omleiden**. De omleiding URI geeft aan dat Azure AD waar verificatie antwoorden moeten worden doorgestuurd: de standaardinstelling voor deze zelfstudie is `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. pre-requisities toevoegen aan uw adreslijst

Vanaf de opdrachtregel, mappen naar de hoofdmap als dat niet al er wijzigen en voer de volgende opdrachten:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Daarnaast we gebruiken hebt `passport-azure-ad` in het skelet van de snelstartgids.

- `npm install passport-azure-ad`


Hiermee installeert u de bibliotheken die passport-azure-ad, hangt af van.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. uw app instellen voor het gebruik van de passport-knooppunt-js-strategie
Hier wordt we de middleware Express als u wilt gebruiken het protocol verificatie OpenID verbinding configureren.  Passport worden gebruikt op aanmelden en afmelden verzoeken en informatie over de gebruiker, onder andere sessie van de gebruiker beheren.

-   Als u wilt beginnen, opent u de `config.js` bestand in de hoofdmap van het project en voer waarden in de configuratie van uw app in de `exports.creds` sectie.
    -   De `clientID:` is de **Toepassings-Id** die is toegewezen aan uw app in de registratie-portal.
    -   De `returnURL` is de **URI omleiden** die u hebt ingevoerd in de portal.
    - De `clientSecret` is het geheim die u in de portal gegenereerd.

- De volgende keer opent `app.js` bestand in de hoofdmap van het project en de follwing-oproep om aan te roepen toevoegen de `OIDCStrategy` strategie die wordt geleverd met`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Nadat u hebt die, gebruikt u de strategie we zojuist waarnaar wordt verwezen verwerken van onze login aanvragen

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Een soortgelijke patroon Passport gebruikt voor alle is strategieën (Twitter, Facebook, enzovoort) die voor alle strategie schrijvers worden aangehouden. Bekijken via de strategie ziet u geven we een function() met een token en een gereed als de parameters. De strategie wordt Plichtsgetrouw keert u terug naar ons wanneer deze wordt het werk. Zodra de optie doet wilt we opslaan van de gebruiker en niet het token initialiseren zodat we niet nodig om aan te vragen om deze opnieuw.

> [AZURE.IMPORTANT]
De bovenstaande code gaat elke gebruiker die er gebeurt om te verifiëren met onze server. Dit wordt automatisch registratie genoemd. In productieservers wilt u wouldn't kiesopenbaar als iedereen zonder eerste ze leest u over een registratieproces die u hebt besloten. Dit is meestal het patroon dat u ziet in consumenten-apps die kunnen u met Facebook hebt geregistreerd, maar wordt u aanvullende informatie invullen gevraagd. Als u dit niet een toepassing voor de steekproef hebt, kan hebben we alleen het e-mailbericht opgehaald uit het token object dat wordt geretourneerd en vervolgens te laten invullen aanvullende informatie gevraagd. Aangezien dit een testserver we gewoon de database in het geheugen toevoegen is.

- Vervolgens de methoden, zodat wij ontvangen voor de aangemelde in gebruikers zoals vereist door Passport toevoegen. Dit geldt ook voor serialiseren en het deserialiseren van de gebruiker informatie:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Vervolgens de code voor het laden van de express engine we hebt toegevoegd. Hier ziet u we gebruiken de standaard-/views en /routes patroon dat Express biedt.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Tot slot toevoegen de bericht-routes wordt met de hand uit de werkelijke login aanvragen voor het `passport-azure-ad` engine:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Passport aanmelden en afmelden verzoeken om aan te verlenen Azure AD gebruiken

Uw app is nu geconfigureerd om te communiceren met het v2.0 eindpunt via het protocol OpenID verbinding maken.  `passport-azure-ad`heeft die u hebt gemaakt om ervoor te zorgen alle niet uit details van besteed hier verificatieberichten, tokens afgeleid van Azure AD valideren en onderhouden van gebruikerssessie.  Alle blijft is een manier om te melden, meld u af en aanvullende informatie verzamelen over de gebruiker is aangemeld voor uw gebruikers geven.

- Eerst kunt toevoegen de standaard, login account en meld u af methoden naar onze `app.js` bestand:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Laten we bekijken deze uitgebreid beschreven:
    -   De `/` route worden omgeleid naar de index.ejs weergave doorgeven van de gebruiker in het verzoek (indien aanwezig)
    - De `/account` route wordt eerste ***zorgen dat we worden geverifieerd*** (we implementeren die hieronder) en vervolgens de gebruiker in het verzoek doorgeven zodat we Vind extra informatie over de gebruiker.
    - De `/login` route telefonisch onze verificator azuread-openidconnect uit `passport-azuread` en als dat is mislukt de gebruiker omleiden naar /login
    - De `/logout` wordt gewoon de logout.ejs bellen (en routeren) die cookies opheffen en vervolgens de gebruiker terug te keren terug naar index.ejs


- Voor het laatste onderdeel van `app.js`, laten we toevoegen de methode EnsureAuthenticated die wordt gebruikt in `/account` hierboven.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Tot slot eigenlijk maken de server zelf in `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. maken de weergaven en routes in express om weer te geven van onze gebruiker op de website

We hebben onze `app.js` voltooid. Nu we gewoon moet toevoegen de routes en weergaven die wordt weergegeven de informatie die we aan de gebruiker, evenals de greep aan de `/logout` en `/login` routes we hebt gemaakt.

- Maak de `/routes/index.js` route onder de hoofdmap.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Maak de `/routes/user.js` route onder de hoofdmap

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Deze eenvoudige routes wordt alleen doorgeven langs het verzoek tot onze weergaven, met inbegrip van de gebruiker als deze aanwezig zijn.

- Maak de `/views/index.ejs` weergave onder de hoofdmap. Dit is een eenvoudige pagina die bellen van onze methoden aan- en afmelden en kunnen we pak accountgegevens. Melding dat we de beschikking over de voorwaardelijke `if (!user)` als de gebruiker bevindt zich bewijs we hebben een aangemelde gebruiker wordt doorgegeven door in het verzoek.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Maak de `/views/account.ejs` weergeven onder de hoofdmap, zodat we aanvullende informatie kunt weergeven die `passport-azuread` in het gebruikersverzoek heeft opgeslagen.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Ten slotte, laten we maken dit eruit heel door toe te voegen een lay-out. Maak de ' / views/layout.ejs' weergave onder de hoofdmap

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Tot slot maken en uitvoeren van uw app!

Uitvoeren `node app.js` en Ga naar`http://localhost:3000`


Meld u aan met een persoonlijk Microsoft-Account of een account voor werk of school en ziet u hoe de gebruikers id is doorgevoerd in de lijst /account.  U hebt nu een web-app beveiligd met industriële standaard protocollen die gebruikers met hun persoonlijke en werk-, school account kunnen worden geverifieerd.

##<a name="next-steps"></a>Volgende stappen

Voor een verwijzing naar kunt de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip)of u klonen deze uit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

U kunt nu verplaatsen naar meer geavanceerde onderwerpen.  U kunt proberen:

[Een node.js Secure web api met behulp van het eindpunt v2.0 >>](active-directory-v2-devquickstarts-node-api.md)

Raadpleeg voor meer bronnen:
- [De handleiding voor ontwikkelaars van v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure active directory" tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
