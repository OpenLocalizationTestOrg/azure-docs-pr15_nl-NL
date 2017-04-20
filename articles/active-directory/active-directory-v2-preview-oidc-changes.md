<properties
    pageTitle="Wijzigingen in het eindpunt van Azure AD v2.0 | Microsoft Azure"
    description="Een beschrijving van de wijzigingen die worden aangebracht in de app-model v2.0 openbare preview protocollen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Belangrijke Updates voor de verificatie-protocollen v2.0
Aandacht ontwikkelaars! De volgende twee weken, wordt nog een paar updates naar onze v2.0 verificatie-protocollen die wijzigingen voor alle apps die u hebt geschreven in ons voorbeeld periode verbreken kunnen betekenen.  

## <a name="who-does-this-affect"></a>Wie heeft dit invloed op?
Apps die zijn geschreven gebruik van de v2.0 resultaat heeft verificatie eindpunt, geleverd

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Meer informatie over het eindpunt v2.0 vindt u [hier](active-directory-appmodel-v2-overview.md).

Als u een app het eindpunt v2.0 met kleurcodering rechtstreeks naar het protocol v2.0 hebt gemaakt, moet met behulp van onze OpenID verbinding maken of OAuth web middlewares of andere 3e partijen bibliotheken gebruiken om uit te voeren verificatie, u worden voorbereid om te testen van uw projecten en breng zo nodig wijzigingen.

## <a name="who-doesnt-this-affect"></a>Wie heeft geen dit invloed op?
Apps die zijn geschreven ten opzichte van het eindpunt productie Azure AD-verificatie

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Dit protocol is ingesteld op elke dia toegepast en wordt geen problemen wijzigingen.

Bovendien als uw app **alleen** gebruikmaakt van onze ADAL bibliotheek verificatie uitvoeren, hoeft u te wijzigingen aanbrengen.  ADAL heeft uw app uit de wijzigingen beveiligd.  

## <a name="what-are-the-changes"></a>Wat zijn de wijzigingen?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>De waarde x5t verwijderen uit JWT kopteksten
Het eindpunt v2.0 worden JWT tokens uitgebreid, die een koptekstsectie parameters met relevante metagegevens over de token bevatten.  Als u de koptekst van een van onze huidige JWTs decoderen, zou u er ongeveer zo vinden:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De "x5t" en "kind" eigenschappen geven aan waar de openbare sleutel die moet worden gebruikt voor het valideren van het token handtekening, zoals opgehaald uit het eindpunt van de metagegevens OpenID verbinding maken.

De wijziging we hier maken is de eigenschap "x5t" verwijderen.  U mag blijven de dezelfde regelingen gebruiken om tokens te valideren, maar ontstaan in de eigenschap 'kind' om op te halen de juiste openbare sleutel, zoals opgegeven in het protocol OpenID verbinding maken. 

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de waarde x5t.**

### <a name="removing-profileinfo"></a>Profile_info verwijderen
Eerder, het eindpunt v2.0 heeft is een object te retourneren base64-gecodeerde JSON in token antwoorden genoemd `profile_info`.  Bij het aanvragen van een toegangstoken vanuit het eindpunt v2.0 door een aanvraag om deel te sturen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Het antwoord eruit als het volgende JSON-object:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `profile_info` waarde die is opgenomen informatie over de gebruiker aan wie aangemeld bij de app - hun naam weer te geven, voornaam, achternaam, e-mailadres, identificatie, enzovoort.  Hoofdzakelijk, de `profile_info` is gebruikt voor token in cache opslaan en doeleinden weer te geven.

We nu verwijdert de `profile_info` waarde – maar geen probleem, we nog steeds verschaft u deze informatie voor ontwikkelaars in iets anders locatie.  In plaats van `profile_info`, het eindpunt v2.0 nu retourneert een `id_token` in elk token antwoord:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

U kunt decoderen en de id_token om op te halen van dezelfde gegevens die u hebt ontvangen van profile_info parseren.  De id_token is een JSON Web Token (JWT), met inhoud volgens opgegeven OpenID verbinding maken.  De code hiervoor dus moet vergelijkbaar – u gewoon wilt extraheren van de middelste segment (de tekst) van de id_token en base64 dit voor toegang tot het object JSON binnen decoderen.

De volgende twee weken, moet u uw app gegevens van de gebruiker worden opgehaald uit een code de `id_token` of `profile_info`; indien deze is aanwezig.  Op die manier wanneer de wijziging wordt aangebracht uw app kan naadloos verwerken voor de overgang van `profile_info` naar `id_token` zonder onderbreking.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de `profile_info` waarde.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in verwijderen
Dezelfde `profile_info`, we ook verwijdert de `id_token_expires_in` parameter van de antwoorden.  Eerder, het eindpunt v2.0 resultaat een waarde voor `id_token_expires_in` samen met elk antwoord id_token, bijvoorbeeld in een autoriseren antwoord wordt verzonden:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Of in een token antwoord:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

De `id_token_expires_in` waarde betekent het aantal seconden de id_token geldig blijven zou.  Nu kunnen we verwijdert de `id_token_expires_in` volledig waarde.  In plaats daarvan kunt u de standaard OpenID verbinding `nbf` en `exp` vorderingen om te bekijken van de geldigheid van een id_token.  Zie de [v2.0 token verwijzing](active-directory-v2-tokens.md) voor meer informatie over deze vorderingen.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app is niet afhankelijk van de aanwezigheid van de `id_token_expires_in` waarde.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Wijzigen van de claims die het resultaat van de reikwijdte = openid
Deze wijziging wordt de belangrijkste – in feite, heeft dit gevolgen voor vrijwel elke app waarin het eindpunt v2.0.  Veel toepassingen aanvragen verzenden met het v2.0 eindpunt de `openid` bereik, zoals:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Vandaag, wanneer de gebruiker worden toegewezen toestemming voor de `openid` reikwijdte, uw app een enorme hoeveelheid informatie over de gebruiker in de resulterende id_token ontvangt.  Deze claims kunnen opnemen haar naam, voorkeur gebruikersnaam, e-mailadres, object-ID en meer.

In deze update we de gegevens wilt wijzigen dat de `openid` bereik biedt uw app toegang, tot betere comform met de specificatie OpenID verbinding maken.  De `openid` bereik wordt alleen kunt u uw app aan te melden van de gebruiker in en ontvangen van een app-specifieke-id voor de gebruiker in de `sub` claimen van de id_token.  De claims in een id_token met alleen de `openid` bereik verleend worden persoonlijke gegevens.  Voorbeeld id_token claims zijn:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Als u ophalen van persoonlijke gegevens worden verzameld over de gebruiker in uw app wilt, moet uw app aanvullende machtigingen aanvragen van de gebruiker.  We zijn Kennismaking met ondersteuning voor twee nieuwe bereiken van de specificatie OpenID verbinding – de `email` en `profile` bereiken – waarmee u kunt doen.

- De `email` bereik is zeer eenvoudige, maar het biedt uw app toegang tot de primaire e-mailadres van de gebruiker via de `email` claimen in de id_token.  Houd er rekening mee dat het `email` claimen niet altijd in aanwezig zijn id_tokens – deze alleen worden opgenomen als beschikbaar in het gebruikersprofiel.
- De `profile` bereik biedt uw app toegang tot alle andere basisinformatie over de gebruiker – hun naam, de voorkeur gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u uw app op een wijze minimale vrijgeven code: u kunt de gebruiker vragen om alleen de set met gegevens dat uw app is vereist om de werk te doen.  Als u doorgaan met het ophalen van de volledige reeks gebruikersgegevens die uw app momenteel ontvangt wilt, moet u alle drie bereiken opnemen in uw autorisatieaanvragen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Uw app kunt beginnen met het verzenden van de `email` en `profile` onmiddellijk bereiken en het eindpunt v2.0 wordt deze twee bereiken accepteren en begin met het aanvragen van de machtigingen van gebruikers zo nodig.  Echter de wijziging bij de interpretatie van de `openid` bereik pas van kracht een paar weken.

> [AZURE.IMPORTANT] **Uw taak: toevoegen de `profile` en `email` bereiken als uw app vereist is voor informatie over de gebruiker.**  Houd er rekening mee dat ADAL beide machtigingen in aanvragen al dan niet standaard bevat. 

### <a name="removing-the-issuer-trailing-slash"></a>De uitgever volgspaties streep verwijderen.
Eerder, het formulier hebt gemaakt, de uitgever-waarde die wordt weergegeven in tokens vanuit het eindpunt v2.0

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Waar is de guid de tenantId van de Azure AD-tenant waarin het token uitgegeven.  Met deze wijzigingen wordt de uitgever-waarde

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

in beide tokens en in het document discovery OpenID verbinding maken.

> [AZURE.IMPORTANT] **Uw taak: Zorg ervoor dat uw app accepteert uitgever waarde zowel met en zonder een slash tijdens de uitgever validatie.**

## <a name="why-change"></a>Waarom wijzigen?
De primaire motivatie introduceren deze wijzigingen is compatibel met de standaard specificatie van OpenID verbinding maken.  Door op te OpenID verbinding maken met compatibele, hebt u hopelijk te minimaliseren ten verschillen tussen de integratie met services van Microsoft-identiteit en met andere services identiteit in de branche.  We wilt kunnen ontwikkelaars hun favoriete bron openen verificatie-bibliotheken gebruiken zonder dat u moet het wijzigen van de bibliotheken voor verschillende van Microsoft.

## <a name="what-can-you-do"></a>Wat kunt u doen?
Per vandaag, kunt u beginnen met het maken van alle wijzigingen hierboven is beschreven.  Direct moet u:

1.  **Verwijder de afhankelijkheden op de `x5t` koptekst-parameter.**
2.  **De overgang van de juiste manier verwerken `profile_info` naar `id_token` in token antwoorden.**
3.  **Verwijder de afhankelijkheden op de `id_token_expires_in` antwoord parameter.**
3.  **Toevoegen de `profile` en `email` als uw app moet worden eenvoudige gebruikersgegevens naar uw app-bereiken.**
4.  **Uitgever waarden in tokens zowel met en zonder een slash accepteren.**

Met deze wijzigingen, zodat u deze als referentie gebruiken kunt bij het bijwerken van uw code is al onze [v2.0 protocol documentatie](active-directory-v2-protocols.md) bijgewerkt.

Als u meer vragen op het bereik van de wijzigingen hebt, neem je mag rustig contact met ons op Twitter bij @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Hoe vaak protocol wijzigingen plaatsvindt?
Niet doen we voorzien eventuele verder verbreken wordt gewijzigd in de verificatieprotocollen.  We bundelt deze wijzigingen in één release met opzet hebt zodat hoeft u niet leest u over dit type updateproces binnenkort opnieuw elk gewenst moment.  Uiteraard kunt blijft functies toevoegen aan de verificatieservice geconvergeerde v2.0 die u van profiteren kunt, maar deze wijzigingen moeten toevoegingsmiddel en niet-bestaande code einde.

Ten slotte willen we zeggen Hartelijk dank voor het dingen wilt uitproberen gedurende de periode preview.  De inzichten en ervaringen van onze vroege gebruikers zijn bijzonder nuttig tot nu toe en we hopen dat u hebt om uw mening over en ideeën te delen.

Gefeliciteerd kleurcodering!

De identiteit van de Microsoft-afdeling
