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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory-B2C: OAuth 2.0 autorisatie code stroom

Het OAuth 2.0 autorisatie code verlenen kan worden gebruikt in apps die op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web API's zijn geïnstalleerd. Met behulp van de Azure Active Directory (Azure AD) B2C uitvoering van OAuth 2.0, kunt u beheertaken voor aanmelding, aanmelden en andere identiteit toevoegen aan uw mobiele en bureaublad-apps. Deze handleiding is bedoeld voor onafhankelijke taal. Dit wordt beschreven hoe berichten verzenden en ontvangen HTTP zonder een van onze open source-bibliotheken.

<!-- TODO: Need link to libraries -->

De stroom van OAuth 2.0 autorisatie code wordt beschreven in de [sectie 4.1 van het OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). U kunt deze verificatie en machtiging uitvoeren in de meeste app typen, inclusief [WebApps](active-directory-b2c-apps.md#web-apps) en [apps standaard geïnstalleerd](active-directory-b2c-apps.md#mobile-and-native-apps). Apps veilig verkrijgen **access_tokens**, die kan worden gebruikt voor toegang tot de resources die zijn beveiligd met een [autorisatie server](active-directory-b2c-reference-protocols.md#the-basics)kunt.

Deze handleiding gericht op een bepaalde versie van het OAuth 2.0 autorisatie code stroom--**openbare-clients**. Een openbare client is een clienttoepassing die niet kan overgelaten worden aan de integriteit van een geheime wachtwoord veilig te behouden. Dit geldt ook voor mobiele apps, bureaublad-apps en vrijwel elke toepassing die wordt uitgevoerd op een apparaat en moet access_tokens verkrijgen. Als u Identiteitsbeheer toevoegen aan een web-app wilt met behulp van Azure AD B2C, moet u [OpenID verbinding](active-directory-b2c-reference-oidc.md) in plaats van OAuth 2.0.

Azure AD B2C breidt de standaard die OAuth 2.0 doorloopt om uit te voeren meer dan eenvoudige verificatie en machtiging. Deze maakt u kennis met de [**parameter van het beleid**](active-directory-b2c-reference-policies.md), waarmee u kunt het OAuth 2.0 gebruiken voor het toevoegen van gebruikerservaringen aan uw app, is het zoals aanmelding, aanmelden en beheer van gebruikersprofielen. Hier leert OAuth 2.0 en beleidsregels voor elk van deze ervaringen implementeren in uw eigen toepassingen gebruiken. Ook leert hoe u access_tokens voor toegang tot web API's.

Het onderstaande voorbeeld HTTP-aanvragen wordt gebruikt voor ons voorbeeld B2C directory, **fabrikamb2c.onmicrosoft.com**, evenals onze steekproef-toepassing en beleid. U kunt het aanvragen van uzelf met behulp van deze waarden bent, of kunt u deze vervangen door uw eigen.
Leer hoe u [uw eigen B2C directory, -toepassing, en beleid](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. een autorisatiecode ophalen
De stroom van de code autorisatie begint met de client die de gebruiker waarmee de `/authorize` eindpunt. Dit is het interactieve deel van de stroom, waar de gebruiker wordt pas actie. In dit verzoek de client geeft aan dat de machtigingen die zijn vereist voor het ophalen van de gebruiker in de `scope` parameter en het beleid uitvoeren de `p` parameter. Drie voorbeelden hieronder vermeld (met regeleinden voor leesbaarheid), elk met een ander beleid.

#### <a name="use-a-sign-in-policy"></a>Gebruik een beleid aanmelden

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Een aanmelding beleid gebruiken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Gebruik een profiel bewerken

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Vereist | De toepassing-ID die de [portal van Azure](https://portal.azure.com) die zijn toegewezen aan uw app. |
| response_type | Vereist | Het antwoordtype, waaronder moet `code` voor de stroom van de code autorisatie. |
| redirect_uri | Vereist | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app. Dit moet precies overeenkomen met een van de redirect_uris die u in de portal hebt geregistreerd, behalve dat het URL-codering moet zijn. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. Gebruik de client-ID van het bereik wordt aangegeven dat uw app moet een **toegangstoken** die kan worden gebruikt voor uw eigen service of het web API, voorgesteld door het dezelfde client-ID.  De `offline_access` bereik wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet.  U kunt ook de `openid` aanbevolen draaitabellen op het aanvragen van een **id_token** van Azure AD B2C. |
| response_mode | Aanbevolen | De methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app. U kunt werken 'query', 'form_post' of 'fragment'.
| de staat | Aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd. Een tekenreeks van inhoud die u wilt u kunt werken. Een willekeurig wordt gegenereerd unieke waarde wordt meestal gebruikt voor voorkoming van meerdere sites verzoek aanvallen. De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld de pagina op of het beleid wordt uitgevoerd. |
| p | Vereist | Het beleid die wordt uitgevoerd. Dit is de naam van een beleid dat wordt gemaakt in uw adreslijst B2C. De waarde van beleid moet beginnen met ' b2c\_1\_". Meer informatie over het beleid in [Extensible beleidskader](active-directory-b2c-reference-policies.md). |
| prompt | Optionele | Het type van de interactie van de gebruiker die is vereist. De enige geldige waarde op dit moment is 'aanmelding', waardoor de gebruiker hun referenties invoeren verzocht. Eenmalige aanmelding pas van kracht. |

De gebruiker wordt nu gevraagd om de werkstroom van het beleid te voltooien. Hierbij kan de gebruiker die hun gebruikersnaam en wachtwoord in te voeren aanmelden met een sociale identiteit, zich registreert voor de map of een ander getal van stappen uit, afhankelijk van hoe het beleid wordt gedefinieerd.

Nadat de gebruiker het beleid voltooit, Azure AD retourneert een antwoord op uw app in de opgegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van de bovenstaande gevallen, onafhankelijk van het beleid dat is uitgevoerd.

Een positief antwoord waarin `response_mode=query` lijkt op:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| code | De authorization_code die de app wordt gevraagd. De app kunt u de autorisatiecode gebruiken aanvragen van een access_token voor een resource doel. Authorization_codes zijn erg lang te worden bewaard. Normaal gesproken verlopen ze na ongeveer 10 minuten. |
| de staat | Zie de volledige beschrijving in de vorige tabel. Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de aanvraag en het antwoord identiek zijn. |

Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodanig dat ze correct laten door de app afhandelen:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee u kunt een ontwikkelaar om aan te geven van de oorzaak van een verificatiefout. |
| de staat | Zie de volledige beschrijving in de eerste tabel in deze sectie. Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de aanvraag en het antwoord identiek zijn. |


## <a name="2-get-a-token"></a>2. een token ophalen
Nu dat u een authorization_code hebt aangeschaft, kunt u inwisselen de `code` voor een token op de gewenste resource door te sturen een `POST` verzoek om deel te de `/token` eindpunt. In Azure AD B2C is de enige resource die u kunt ook zelf aanvragen een token voor uw app eigen backend web API. De overeenkomst die wordt gebruikt voor het aanvragen van een token aan uzelf is van uw app cliënt-ID gebruiken als het bereik:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| p | Vereist | Het beleid dat is gebruikt voor het ophalen van de autorisatiecode. U kunt een ander beleid niet gebruiken in dit verzoek. Houd er rekening mee dat u deze parameter aan de *queryreeks*, niet in de hoofdtekst van bericht toevoegen. |
| client_id | Vereist | De toepassing-ID die de [portal van Azure](https://portal.azure.com) die zijn toegewezen aan uw app. |
| grant_type | Vereist | Het type verlenen, die moet `authorization_code` voor de stroom van de code autorisatie. |
| bereik | Reccommended | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. Gebruik de client-ID van het bereik wordt aangegeven dat uw app moet een **toegangstoken** die kan worden gebruikt voor uw eigen service of het web API, voorgesteld door het dezelfde client-ID.  De `offline_access` bereik wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet.  U kunt ook de `openid` aanbevolen draaitabellen op het aanvragen van een **id_token** van Azure AD B2C. |
| code | Vereist | De authorization_code die u in de eerste zijde van de stroom hebt aangeschaft. |
| redirect_uri | Vereist | De redirect_uri van de toepassing waarop u de authorization_code hebt ontvangen. |

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
| not_before | Het tijdstip waarop het token wordt beschouwd als geldige, epoche tijdig. |
| token_type | De waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende. |
| access_token | De ondertekende JSON Web Token (JWT) token gevraagde. |
| bereik | De bereiken dat het token geldt voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd waarop de token geldig (in seconden is). |
| refresh_token | Een refresh_token OAuth 2.0. De app met deze token kunt u in het bezit van extra tokens als het huidige token is verlopen. Refresh_tokens zijn lang mee, en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden. Voor meer details, raadpleegt u de [B2C token verwijzing](active-directory-b2c-reference-tokens.md). |

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
| error_description | Een specifiek foutbericht waarmee u kunt een ontwikkelaar om aan te geven van de oorzaak van een verificatiefout. |

## <a name="3-use-the-token"></a>3. Gebruik de token
Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen aan uw backend API's met webonderdelen door op te nemen in de `Authorization` kop:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. het token vernieuwen
Access & Id tokens zijn lang te worden bewaard. Nadat ze verlopen als u wilt doorgaan wordt toegang tot bronnen, moet u deze vernieuwen. U kunt doen door in te dienen andere `POST` verzoek om deel te de `/token` eindpunt. Ditmaal bieden de `refresh_token` in plaats van de `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Vereist? | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| p | Vereist | Het beleid dat is gebruikt voor het ophalen van de oorspronkelijke refresh_token. U kunt een ander beleid niet gebruiken in dit verzoek. Houd er rekening mee dat u deze parameter aan de *queryreeks*niet in de hoofdtekst van bericht toevoegen. |
| client_id | Aanbevolen | De toepassing-ID die de [portal van Azure](https://portal.azure.com) die zijn toegewezen aan uw app. |
| grant_type | Vereist | Het type verlenen, die moet `refresh_token` voor deze arm van de stroom van de code autorisatie. |
| bereik | Aanbevolen | Een door spaties gescheiden lijst met bereiken. Een bereikwaarde één geeft aan Azure AD beide van de machtigingen die worden aangevraagd. Gebruik de client-ID van het bereik wordt aangegeven dat uw app moet een **toegangstoken** die kan worden gebruikt voor uw eigen service of het web API, voorgesteld door het dezelfde client-ID.  De `offline_access` bereik wordt aangegeven dat uw app een **refresh_token** voor lange levensduur toegang tot resources moet.  U kunt ook de `openid` aanbevolen draaitabellen op het aanvragen van een **id_token** van Azure AD B2C. |
| redirect_uri | Optionele | De redirect_uri van de toepassing waarop u de authorization_code hebt ontvangen. |
| refresh_token | Vereist | De oorspronkelijke refresh_token die u in de tweede zijde van de stroom hebt aangeschaft. |

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
| access_token | De ondertekende JSON Web Token (JWT) token gevraagde. |
| bereik | De bereiken dat het token geldt voor, die kunnen worden gebruikt voor caching van tokens voor later gebruik. |
| expires_in | De lengte van de tijd waarop de token geldig (in seconden is). |
| refresh_token | Een refresh_token OAuth 2.0. De app met deze token kunt u in het bezit van extra tokens als het huidige token is verlopen. Refresh_tokens zijn lang mee, en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden. Voor meer details, raadpleegt u de [B2C token verwijzing](active-directory-b2c-reference-tokens.md). |

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

## <a name="use-your-own-b2c-directory"></a>Het telefoonboek van uw eigen B2C gebruiken

Als u proberen deze aanvragen voor uzelf wilt, moet u eerst deze drie stappen uitvoeren en vervolgens de voorbeeldwaarden hierboven vervangen door uw eigen:

- [Een map B2C maken](active-directory-b2c-get-started.md) en gebruik de naam van de map in de aanvragen.
- [Een toepassing maken](active-directory-b2c-app-registration.md) voor informatie over een toepassings-ID en een redirect_uri. U wilt een **native client** opnemen in uw app.
- [Maak uw beleidsregels](active-directory-b2c-reference-policies.md) voor de namen van uw beleid.
