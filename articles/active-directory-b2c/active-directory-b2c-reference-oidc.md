<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Het bouwen van webtoepassingen met behulp van de Azure Active Directory-implementatie van het protocol verificatie OpenID verbinding maken."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory-B2C: Web aanmelden met OpenID verbinding maken

OpenID Connect is een protocol voor verificatie, gebouwd OAuth 2.0, die kunnen worden gebruikt om gebruikers veilig aan te melden bij webtoepassingen.  U kunt uitbesteden aanmelding, aanmelden met behulp van de Azure Active Directory (Azure AD) B2C-implementatie van OpenID Connect, en andere identiteitsbeheer-ervaringen in uw webtoepassingen Azure AD. Deze handleiding wordt uitgelegd hoe u kunt doen in een, taal ongeacht de landinstelling. Dit wordt beschreven hoe berichten verzenden en ontvangen HTTP zonder een van onze open source-bibliotheken.

[OpenID verbinding](http://openid.net/specs/openid-connect-core-1_0.html) breidt het OAuth 2.0 *autorisatie* protocol voor gebruik als een *verificatie* -protocol. Hiermee kunt u eenmalige aanmelding met behulp van OAuth uitvoeren. Deze maakt u kennis met het concept van een `id_token`, namelijk een beveiligingstoken waarmee de client om te controleren of de identiteit van de gebruiker en verkrijgen van eenvoudige profielgegevens over de gebruiker.

Omdat deze zich OAuth 2.0 uitstrekt, kunt u eveneens apps veilig verkrijgen **access_tokens**. U kunt access_tokens voor toegang tot bronnen die zijn beveiligd met een [autorisatie-server](active-directory-b2c-reference-protocols.md#the-basics). Wordt aangeraden OpenID verbinding maken als u een webtoepassing die wordt gehost op een server en toegankelijk via een browser maakt. Als u toevoegen van identiteitsbeheer naar uw mobiele of bureaublad toepassingen wilt via Azure AD B2C, moet u [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) in plaats van OpenID verbinding maken.

Azure AD B2C breidt de standaard protocol OpenID verbinding maken als u wilt meer dan eenvoudige verificatie en machtiging doen. Deze maakt u kennis met de [**parameter van het beleid**](active-directory-b2c-reference-policies.md), waarmee u OpenID verbinding maken met voegt zoals gebruikerservaringen toe aan uw app--aanmelding, aanmelden en beheer van gebruikersprofielen. Hier leert u hoe u met OpenID verbinding en beleid elk van deze ervaringen implementeren in uw webtoepassingen. Ook leert u hoe u access_tokens voor toegang tot web API's.

Het onderstaande voorbeeld HTTP-aanvragen wordt gebruikt voor ons voorbeeld B2C directory, **fabrikamb2c.onmicrosoft.com**, evenals onze steekproef toepassing **https://aadb2cplayground.azurewebsites.net** en beleid. U bent gratis om uit te proberen de aanvragen uzelf met behulp van deze waarden, of kunt u deze vervangen door uw eigen.
Leer hoe u [uw eigen B2C tenant, de toepassing, en het beleid](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Verificatie-aanvragen verzenden
Wanneer uw web-app moet de gebruiker te verifiëren en uitvoeren van een beleid, kan de gebruiker worden doorgestuurd de `/authorize` eindpunt. Dit is het interactieve deel van de stroom, waar de gebruiker zal daadwerkelijk actie ondernemen, afhankelijk van het beleid.

In dit verzoek de client geeft aan dat de machtigingen die zijn vereist voor het ophalen van de gebruiker in de `scope` parameter en het beleid uitvoeren de `p` parameter. Drie voorbeelden hieronder vermeld (met regeleinden voor leesbaarheid), elk met een ander beleid. Zodat u een beetje voor elk verzoek om een werking, probeer het verzoek plakken in een browser en uitvoeren.

#### <a name="use-a-sign-in-policy"></a>Gebruik een beleid aanmelden

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Een aanmelding beleid gebruiken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Gebruik een profiel bewerken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Vereist | De toepassing-ID die de [portal van Azure](https://portal.azure.com/) die zijn toegewezen aan uw app. |
| response_type | Vereist | Het antwoordtype, waaronder moet `id_token` voor OpenID verbinding maken. Als uw web-app ook tokens moet voor het bellen van een web API, kunt u `code+id_token`, zoals we hier hebt gedaan. |
| redirect_uri | Aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app. Dit moet precies overeenkomen met een van de redirect_uris die u in de portal hebt geregistreerd, behalve dat het URL-codering moet zijn. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging Aanmelden bij de gebruiker en gegevens over de gebruiker in de vorm van **id_tokens** (nog niet alles op deze later in dit artikel) ophalen. De `offline_access` bereik is optioneel voor Webapps. Hiermee wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet. |
| response_mode | Aanbevolen | De methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app. U kunt werken 'query', 'form_post' of 'fragment'.  'form_post' geschikt is voor de beste beveiliging. |
| de staat | Aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd. Een tekenreeks van inhoud die u wilt u kunt werken. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor voorkoming van meerdere sites verzoek aanvallen. De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld de pagina op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die u in de resulterende id_token als een claimen opnemen wilt. De app kunt vervolgens deze waarde om te beperken herhaling van het token-aanvallen controleren. De waarde is meestal een willekeurig, unieke tekenreeks die kan worden gebruikt om aan te geven van de oorsprong van het verzoek. |
| p | Vereist | Het beleid die wordt uitgevoerd. Dit is de naam van een beleid die is gemaakt in uw tenant B2C. De waarde van beleid moet beginnen met ' b2c\_1\_". Meer informatie over het beleid in [Extensible beleidskader](active-directory-b2c-reference-policies.md). |
| prompt | Optionele | Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde op dit moment is 'aanmelding', waardoor de gebruiker hun referenties invoeren verzocht. Eenmalige aanmelding pas van kracht. |

De gebruiker wordt nu gevraagd om de werkstroom van het beleid te voltooien. Hierbij kan de gebruiker die hun gebruikersnaam en wachtwoord in te voeren aanmelden met een sociale identiteit, zich registreert voor de map of een ander getal van stappen uit, afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker het beleid voltooit, Azure AD retourneert een antwoord op uw app in de opgegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van de bovenstaande gevallen, onafhankelijk van het beleid dat is uitgevoerd.

Een positief antwoord met `response_mode=fragment` eruitziet:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De id_token die de app wordt gevraagd. U kunt de id_token gebruiken om te controleren of de gebruikers id en begin met een sessie met de gebruiker. Meer details over id_tokens en de inhoud ervan worden opgenomen in de [Azure AD B2C token verwijzing](active-directory-b2c-reference-tokens.md). |
| code | De authorization_code dat de app is aangevraagd, als u hebt gebruikt `response_type=code+id_token`. De app kunt u de autorisatiecode gebruiken aanvragen van een access_token voor een resource doel. Authorization_codes zijn erg lang te worden bewaard. Normaal gesproken verlopen ze na ongeveer 10 minuten. |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |

Fout antwoorden kunnen ook worden verzonden naar de `redirect_uri` zodanig dat ze correct laten door de app afhandelen:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout. |
| de staat | Zie de volledige beschrijving in de vorige tabel. Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |


## <a name="validate-the-idtoken"></a>De id_token valideren
Alleen ontvangen een id_token is niet genoeg voor de verificatie van de gebruiker--af van de id_token handtekening valideren en controleer of de claims in het token aan de hand van de vereisten van uw app. Azure AD B2C gebruikt [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en openbare sleutel cryptografische af wilt melden tokens en controleert u of ze geldig zijn.

Zijn er veel open source bibliotheken die beschikbaar zijn voor het valideren van JWTs, afhankelijk van de taal van voorkeur. Het is raadzaam om deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie hier is handig in uitzoeken hoe u goed gebruikt die bibliotheken.

Azure AD B2C heeft een eindpunt metagegevens OpenID verbinden, waarmee een app kunt u informatie over Azure AD B2C gedurende runtime ophalen. Deze informatie bevat eindpunten, token-inhoud en token-ondertekening toetsen. Er is een document JSON metagegevens voor elk beleid in uw tenant B2C. Bijvoorbeeld het metagegevensdocument voor de `b2c_1_sign_in` beleid in de `fabrikamb2c.onmicrosoft.com` bevindt zich op:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit document configuratie is de `jwks_uri`, waarvan de waarde voor hetzelfde beleid zou:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Klik in bestel om te bepalen welk beleid is gebruikt voor het ondertekenen van een id_token (en waar u kunt de metagegevens van ophalen), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claimen in de id_token. Voor informatie over hoe u de claims van een id_token parseren, raadpleegt u [Azure AD B2C token verwijzing](active-directory-b2c-reference-tokens.md). Uw andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u het verzoek verzenden en klikt u vervolgens decoderen om te bepalen welk beleid is gebruikt. Een methode is perfect geldig.

Nadat u het metagegevensdocument vanuit het eindpunt van de metagegevens OpenID verbinding hebt aangeschaft, kunt u de RSA 256 openbare sleutels (die zich op dit eindpunt) voor het valideren van de handtekening van de id_token. Er zijn mogelijk meerdere toetsen weergegeven op dit eindpunt op elk gewenst moment in time, elk die wordt aangeduid met een `kid`. De kop van de id_token bevat ook een `kid` claimen, waarmee wordt aangegeven welke van deze toetsen is gebruikt voor het ondertekenen van de id_token. Zie de [Azure AD B2C token verwijzing](active-directory-b2c-reference-tokens.md) voor meer informatie, inclusief de gedeelten over [tokens valideren](active-directory-b2c-reference-tokens.md#validating-tokens) en [belangrijke informatie over het ondertekenen van sleutelrollover](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Nadat u de handtekening van de id_token hebt geverifieerd, zijn er verschillende claims die u nodig hebt om te verifiëren, bijvoorbeeld:

- U moet valideren de `nonce` claimen om te voorkomen dat herhaling van het token-aanvallen. De waarde moet liggen wat u hebt opgegeven in het verzoek aanmelden.
- U moet valideren de `aud` claimen om ervoor te zorgen dat de id_token is uitgegeven voor de app. De waarde moet de toepassings-ID van de app.
- U moet valideren de `iat` en `exp` vorderingen om ervoor te zorgen dat de id_token niet is verlopen.

Er zijn ook enkele meer validatie die u uitvoeren moet, in de [OpenID verbinding Core specificatie](http://openid.net/specs/openid-connect-core-1_0.html)in detail beschreven.  U kunt ook voor het valideren van extra claims, afhankelijk van uw scenario. Enkele veelvoorkomende validatie opnemen:

- Ervoor zorgen dat de gebruiker/organisatie zich heeft aangemeld voor de app.
- Ervoor zorgen dat de gebruiker de juiste autorisatie/bevoegdheden heeft.
- Ervoor zorgen dat een bepaalde sterkte van verificatie is opgetreden, zoals Azure meervoudige verificatie.

Voor meer informatie over de claims in een id_token, raadpleegt u [Azure AD B2C token verwijzing](active-directory-b2c-reference-tokens.md).

Nadat u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker en de claims gebruiken in de id_token voor informatie over de gebruiker in uw app. Deze informatie kan worden gebruikt voor weergave, records, autorisatie en dergelijke.

## <a name="get-a-token"></a>Een token ophalen
Als de behoeften van uw web-app moet is alle beleidsregels uitvoeren, kunt u de volgende secties overslaan. Deze secties zijn alleen van toepassing op web-apps die u wilt aanbrengen oproepen naar een web API geverifieerd en ook zijn beveiligd met DRM Azure AD B2C.

U kunt de authorization_code die u hebt aangeschaft inwisselen (met behulp van `response_type=code+id_token`) voor een token op de gewenste resource door te sturen een `POST` verzoek om deel te de `/token` eindpunt. Momenteel is de enige resource die u kunt ook zelf aanvragen een token voor uw app eigen backend web API. De overeenkomst voor het aanvragen van een token aan uzelf is van uw app cliënt-ID gebruiken als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| p | Vereist | Het beleid dat is gebruikt voor het ophalen van de autorisatiecode. U kunt een ander beleid niet gebruiken in dit verzoek. Houd er rekening mee dat u deze parameter aan de **queryreeks**, niet in de hoofdtekst van bericht toevoegen. |
| client_id | Vereist | De toepassing-ID die de [portal van Azure](https://portal.azure.com/) die zijn toegewezen aan uw app. |
| grant_type | Vereist | Het type verlenen, die moet `authorization_code` voor de stroom van de code autorisatie. |
| bereik | Aanbevolen | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging Aanmelden bij de gebruiker en gegevens over de gebruiker in de vorm van **id_tokens**ophalen. Dit kan worden gebruikt om tokens naar uw app eigen backend web API, waarmee wordt aangeduid met dezelfde toepassing ID als de client. De `offline_access` bereik wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet. |
| code | Vereist | De authorization_code die u in de eerste zijde van de stroom hebt aangeschaft. |
| redirect_uri | Vereist | De redirect_uri van de toepassing waarop u de authorization_code hebt ontvangen. |
| client_secret | Vereist | De toepassing-geheim dat u in de [portal van Azure](https://portal.azure.com/)gegenereerd. Deze toepassing geheim is een onderdeel belangrijke beveiliging. U moet deze veilig opslaan op de server. U moet ook handelingen kunt verrichten voor het omwisselen van deze geheim client op basis van periodieke. |

Een positief token antwoord eruitziet:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| not_before | Het tijdstip waarop het token wordt beschouwd als geldige, in epoche tijd. |
| token_type | De waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende. |
| access_token | De ondertekende JWT token gevraagde. |
| bereik | De bereiken dat het token geldt voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd waarop de access_token (in seconden geldt). |
| refresh_token | Een refresh_token OAuth 2.0. De app met deze token kunt u in het bezit van extra tokens als het huidige token is verlopen.  Refresh_tokens zijn lang mee, en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden. Voor meer details, raadpleegt u de [B2C token verwijzing](active-directory-b2c-reference-tokens.md). Opmerking u moet gebruikt hebt het bereik `offline_access` voor de verificatie en de token aanvragen een refresh_token ontvangen. |

Fout antwoorden eruitziet:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout. |

## <a name="use-the-token"></a>Gebruik het token
Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen aan uw backend API's met webonderdelen door op te nemen in de `Authorization` kop:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Het token vernieuwen
De id_tokens zijn lang te worden bewaard. Nadat ze verlopen als u wilt doorgaan wordt toegang tot bronnen, moet u deze vernieuwen. U kunt doen door in te dienen andere `POST` verzoek om deel te de `/token` eindpunt. Schakel ditmaal bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| p | Vereist | Het beleid dat is gebruikt voor het ophalen van de oorspronkelijke refresh_token. U kunt een ander beleid niet gebruiken in dit verzoek. Houd er rekening mee dat u deze parameter aan de **queryreeks**, niet in de hoofdtekst van bericht toevoegen. |
| client_id | Vereist | De toepassing-ID die de [portal van Azure](https://portal.azure.com/) die zijn toegewezen aan uw app. |
| grant_type | Vereist | Het type verlenen, die moet `refresh_token` voor deze arm van de stroom van de code autorisatie. |
| bereik | Aanbevolen | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging Aanmelden bij de gebruiker en gegevens over de gebruiker in de vorm van **id_tokens**ophalen. Dit kan worden gebruikt om tokens naar uw app eigen backend web API, waarmee wordt aangeduid met dezelfde toepassing ID als de client. De `offline_access` bereik wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet. |
| redirect_uri | Aanbevolen | De redirect_uri van de toepassing waarop u de authorization_code hebt ontvangen. |
| refresh_token | Vereist | De oorspronkelijke refresh_token die u in de tweede zijde van de stroom hebt aangeschaft. Opmerking u moet gebruikt hebt het bereik `offline_access` voor de verificatie en de token aanvragen een refresh_token ontvangen. |
| client_secret | Vereist | De toepassing-geheim dat u in de [portal van Azure](https://portal.azure.com/)gegenereerd. Deze toepassing geheim is een onderdeel belangrijke beveiliging. U moet deze veilig opslaan op de server. U moet ook handelingen kunt verrichten voor het omwisselen van deze geheim client op basis van periodieke. |

Een positief token antwoord eruitziet:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| not_before | Het tijdstip waarop het token wordt beschouwd als geldige, in epoche tijd. |
| token_type | De waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende. |
| access_token | De ondertekende JWT token gevraagde. |
| bereik | De bereiken dat het token geldt voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd waarop de access_token (in seconden geldt). |
| refresh_token | Een refresh_token OAuth 2.0. De app met deze token kunt u in het bezit van extra tokens als het huidige token is verlopen.  Refresh_tokens zijn lang mee, en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden. Voor meer details, raadpleegt u de [B2C token verwijzing](active-directory-b2c-reference-tokens.md). |

Fout antwoorden eruitziet:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout. |


## <a name="send-a-sign-out-request"></a>Een afmelden aanvraag verzendt

Wanneer u ondertekenen van de gebruiker afmelden bij de app wilt, is het niet voldoende wissen van uw app cookies of anderszins einde de sessie met de gebruiker. U moet de gebruiker ook omleiden naar Azure AD af te melden. Als u niet kunt doen, kan de gebruiker mogelijk om andere referenties voor uw app zonder hun referenties opnieuw in te voeren. Dit komt doordat ze een geldige eenmalige aanmelding-sessie met Azure AD hebben.

U kunt de gebruiker gewoon omleiden de `end_session_endpoint` die in hetzelfde OpenID verbinding maken met metagegevensdocument beschreven in de eerdere sectie 'Valideren de id_token' wordt weergegeven:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| p | Vereist | Het beleid dat u gebruiken wilt voor het ondertekenen van de gebruiker afmelden bij uw toepassing. |
| post_logout_redirect_uri | Aanbevolen | De URL die de gebruiker wordt omgeleid naar na succesvolle afmelden. Als deze niet voorkomt, worden de gebruiker weergegeven een algemeen bericht door Azure AD B2C.  |

> [AZURE.NOTE]
    Terwijl de gebruiker waarmee de `end_session_endpoint` enkele van de status van de gebruiker eenmalige aanmelding met Azure AD B2C wordt gewist, ondertekent de gebruiker afmelden bij de sessie voor sociale identiteit provider (IDP) van de gebruiker geen. Als de gebruiker selecteert de dezelfde IDP tijdens een latere aanmeldingsproblemen, worden ze worden geverifieerd, zonder hun referenties invoeren. Als een gebruiker wil zich wilt afmelden bij uw B2C-toepassing, betekent dit niet noodzakelijkerwijs dat ze willen geheel afmelden bij de Facebook-account. Echter voor lokale accounts, de sessie beëindigd correct.

## <a name="use-your-own-b2c-tenant"></a>Uw eigen tenant B2C gebruiken

Als u proberen deze aanvragen voor uzelf wilt, moet u eerst deze drie stappen uitvoeren en vervolgens de voorbeeldwaarden hierboven vervangen door uw eigen:

- [Een tenant B2C maken](active-directory-b2c-get-started.md)en gebruik de naam van uw tenant in de aanvragen.
- [Een toepassing maken](active-directory-b2c-app-registration.md) voor informatie over een toepassings-ID en een redirect_uri. U wilt opnemen een **web app/web api** in uw app, en eventueel een **toepassing geheim**maken.
- [Maak uw beleidsregels](active-directory-b2c-reference-policies.md) voor de namen van uw beleid.
