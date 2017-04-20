

<properties
    pageTitle="Azure AD v2.0 OAuth autorisatie Code stroom | Microsoft Azure"
    description="Building webtoepassingen met Azure AD-implementatie van het OAuth 2.0 verificatie-protocol."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0 protocollen - OAuth 2.0 autorisatie Code stroom

Het OAuth 2.0 autorisatie code verlenen kan worden gebruikt in apps die op een apparaat toegang te krijgen tot beveiligde bronnen, zoals web API's zijn geïnstalleerd.  De app-model v2.0 implementatie van OAuth 2.0 gebruikt, kunt u aanmelden en API toegang tot uw mobiele en bureaublad-apps toevoegen.  Deze handleiding is taal-onafhankelijke en wordt beschreven hoe u berichten verzenden en ontvangen HTTP zonder een van onze open source-bibliotheken.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

De stroom van OAuth 2.0 autorisatie code wordt beschreven in in de [sectie 4.1 van het OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749).  Deze wordt gebruikt voor verificatie en machtiging uitvoeren in de meeste app typen, inclusief [WebApps](active-directory-v2-flows.md#web-apps) en [apps standaard geïnstalleerd](active-directory-v2-flows.md#mobile-and-native-apps).  Apps veilig verkrijgen access_tokens die kunnen worden gebruikt voor toegang tot de resources die zijn beveiligd met het eindpunt v2.0 kunt.  

## <a name="protocol-diagram"></a>Protocol-diagram
Op hoog niveau, de hele verificatie stroom voor een toepassing native/mobile enigszins zien er zo uit:

![OAuth Auth Code stroom](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Een autorisatiecode aanvragen
De stroom van de code autorisatie begint met de client die de gebruiker waarmee de `/authorize` eindpunt.  In dit verzoek geeft de client aan de machtigingen die deze moeten ophalen van de gebruiker:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Klik op de onderstaande koppeling voor het uitvoeren van deze aanvraag! Na het aanmelden, uw browser wordt omgeleid naar `https://localhost/myapp/` met een `code` in de adresbalk.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's tenant.  Zie voor meer details, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| response_type | Vereist | Moet bevatten `code` voor de stroom van de code autorisatie. |
| redirect_uri | aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve het url-codering moet zijn.  Voor systeemeigen en mobiele apps, moet u de standaardwaarde van `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| bereik | Vereist | Een door spaties gescheiden lijst met [bereiken](active-directory-v2-scopes.md) die u wilt dat de gebruiker toe te staan.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende token terug naar uw app.  Kan `query` of `form_post`.  |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [voorkoming van meerdere sites verzoek aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| prompt | optionele | Geeft het type van de interactie van de gebruiker die is vereist.  De enige geldige waarden op dit moment zijn 'login', 'geen' en 'toestemming'.  `prompt=login`de gebruiker hun referenties invoeren verzocht, negatief maken van eenmalige aanmelding, worden gedwongen.  `prompt=none`is het tegenovergestelde - zorgt ervoor dat dat de gebruiker niet met vragen dan ook wordt gepresenteerd.  Als het verzoek kan niet worden voltooid in stilte via eenmalige aanmelding, kan het eindpunt v2.0 een fout zullen retourneren.  `prompt=consent`activeert het OAuth toestemming dialoogvenster nadat de gebruiker zich aanmeldt, waarin de gebruiker machtigen om de app wordt gevraagd. |
| login_hint | optionele | Kan worden gebruikt om te vullen vooraf het veld gebruikersnaam/e-mail adres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tijd vooraf.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, de gebruikersnaam die al worden opgehaald uit een vorige aanmelden met de `preferred_username` claimen. |
| domain_hint | optionele | Zijn `consumers` of `organizations`.  Als opgenomen, wordt deze het e-mailbericht gebaseerde detectieproces overslaan die gebruiker doorloopt op de pagina, v2.0 aanmelden waardoor een iets meer gestroomlijnd gebruikerservaring.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, door het extraheren van de `tid` uit een vorige aanmeldingsproblemen.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

De gebruiker wordt nu gevraagd naar hun referenties invoeren en de verificatie te voltooien.  Het eindpunt v2.0 er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen zoals aangegeven in de `scope` queryparameter.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, wordt deze vraag de gebruiker toe te staan de vereiste machtigingen.  Details van [machtigingen, toestemming te geven, en meerdere tenant apps hier worden beschreven](active-directory-v2-scopes.md).

Nadat de gebruiker wordt geverifieerd, en toestemming verleent, retourneert het eindpunt v2.0 een antwoord op uw app in de opgegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord met `response_mode=query` lijkt op:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| code | De authorization_code die de app wordt gevraagd. De app kunt u de autorisatiecode gebruiken aanvragen van een toegangstoken voor de resource doel.  Authorization_codes zijn zeer korte levensduur, meestal ze verlopen na ongeveer 10 minuten. |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |

#### <a name="error-response"></a>Foutmelding
Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat ze correct laten door de app afhandelen:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Foutcodes autorisatie eindpunt fouten

De volgende tabel beschrijft de verschillende foutcodes die kunnen worden geretourneerd in de `error` -parameter van de reactie van de fout.

| Foutcode | Beschrijving | Clientactie |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Oplossen en verzend het verzoek opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test.|
| unauthorized_client | De clienttoepassing is niet toegestaan aanvragen van een autorisatiecode. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet wordt toegevoegd aan van de gebruiker Azure AD-tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| ACCESS_DENIED | De eigenaar van de resource is geweigerd toestemming | De clienttoepassing kan de gebruiker die dit kan niet worden voortgezet tenzij de gebruiker akkoord gaat melden. |
| unsupported_response_type | De server autorisatie biedt geen ondersteuning voor het antwoordtype in het verzoek. | Oplossen en verzend het verzoek opnieuw. Dit is een ontwikkeling fout wordt meestal onderschept tijdens de eerste test.|
|server_error | De server is een onverwachte fout opgetreden. | Probeer het verzoek. Deze fouten kunnen resulteren uit tijdelijke voorwaarden. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable | De server is tijdelijk bezet u omgaat met het verzoek. | Probeer het verzoek. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke voorwaarde. |
| invalid_resource |De doel-resource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze juist niet is geconfigureerd.| Hiermee geeft u aan dat de resource, indien aanwezig, niet is geconfigureerd in de tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |

## <a name="request-an-access-token"></a>Een toegangstoken aanvragen
Nu dat u een authorization_code hebt aangeschaft en toestemming hebt gekregen van de gebruiker, kunt u inwisselen de `code` voor een `access_token` naar de gewenste resource, door te sturen een `POST` verzoek om deel te de `/token` eindpunt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probeer deze aanvraag uitvoert in Postman! (Vergeet niet voor het vervangen van de `code`)     [ ![Uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's tenant.  Zie voor meer details, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| grant_type | Vereist | Moet `authorization_code` voor de stroom van de code autorisatie. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken.  De bereiken aangevraagd in dit gedeelte moet gelijk aan of een deel van de bereiken in de eerste zijde is aangevraagd.  Als de bereiken die in deze aanvraag reeks meerdere resource-servers, retourneert het eindpunt v2.0 een token voor de resource die is opgegeven in het eerste bereik.  Voor een gedetailleerde uitleg van bereiken, raadpleegt u [machtigingen, toestemming te geven, en bereiken](active-directory-v2-scopes.md).  |
| code | Vereist | De authorization_code die u in de eerste zijde van de stroom hebt aangeschaft.   |
| redirect_uri | Vereist | De dezelfde redirect_uri waarde die is gebruikt voor het ophalen van de authorization_code. |
| client_secret | vereist voor WebApps | De toepassing geheim die u in de portal van de registratie van de app hebt gemaakt voor de app.  Deze moet niet worden gebruikt in een systeemeigen app, omdat client_secrets betrouwbaar op apparaten kan niet worden opgeslagen.  Dit is vereist voor het WebApps en web API's waarvoor de mogelijkheid om op te slaan de client_secret veilig op de server. |

#### <a name="successful-response"></a>Positief antwoord
Een positief token antwoord eruitziet:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De gevraagde toegangstoken. De app kunt u deze token gebruiken om de beveiligde resource, zoals een web API te verifiëren. |
| token_type | Geeft de waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende  |
| expires_in | Hoe lang het toegangstoken geldt (in seconden). |
| bereik | De bereiken waarvoor de access_token geldig is. |
| refresh_token |  Een OAuth 2.0 vernieuwen token. De app de beschikking over deze token in het bezit van extra access tokens als het huidige toegangstoken is verlopen.  Refresh_tokens zijn lange levensduur en kunnen worden gebruikt om toegang tot bronnen voor langere tijd behouden.  Voor meer details, raadpleegt u de [v2.0 token verwijzing](active-directory-v2-tokens.md).  |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De app kunnen base64Url decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft aangemeld. De app kan de waarden in de cache en waar ze worden weergegeven, maar deze mag niet volledig vertrouwen worden voor alle autorisatie en beveiligingsgrenzen.  Zie voor meer informatie over id_tokens de [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Foutmelding
Fout antwoorden eruitziet:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |
| error_codes | Een lijst met STS specifieke foutcodes die in diagnostische hulpprogramma's kunnen helpen.  |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens.  |
| correlation_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens over de onderdelen. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Foutcodes voor token-eindpunt fouten

| Foutcode | Beschrijving | Clientactie |
|------------|-------------|---------------|
| invalid_request | Protocolfout, zoals een ontbrekende vereiste parameter. | Los en de aanvraag opnieuw indienen |
| invalid_grant | De autorisatiecode ongeldig is of is verlopen. | Voer een nieuwe aanvraag naar de `/authorize` eindpunt |
| unauthorized_client | De geverifieerde client is niet geautoriseerd gebruik van dit type met toestemming verlenen. | Dit gebeurt meestal wanneer de clienttoepassing is niet geregistreerd in Azure AD of niet wordt toegevoegd aan van de gebruiker Azure AD-tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| invalid_client | Clientverificatie is mislukt. | De referenties van de client zijn niet geldig. De beheerder van de toepassing bijgewerkt om op te lossen, de referenties. |
| unsupported_grant_type | De server autorisatie biedt geen ondersteuning voor het type van toestemming verlenen. | Wijzig het type verlenen in het verzoek. Dit soort fouten alleen tijdens de ontwikkeling moet worden uitgevoerd en worden gedetecteerd tijdens de eerste test. |
| invalid_resource | De doel-resource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze juist niet is geconfigureerd. | Hiermee geeft u aan dat de resource, indien aanwezig, niet is geconfigureerd in de tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |
| interaction_required | Het verzoek is vereist voor interactie met de gebruiker. Bijvoorbeeld, is een stap extra verificatie vereist. | Probeer het verzoek met dezelfde resource. |
| temporarily_unavailable | De server is tijdelijk bezet u omgaat met het verzoek. | Probeer het verzoek. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke voorwaarde.|

## <a name="use-the-access-token"></a>Gebruik het toegangstoken
Nu dat u hebt gekregen een `access_token`, kunt u het token in aanvragen voor Web-API's door op te nemen in de `Authorization` kop:

> [AZURE.TIP] Het uitvoeren van deze verzoek in Postman! (Vervangen de `Authorization` koptekst eerste)     [ ![Uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Het toegangstoken vernieuwen
Access_tokens kort zijn gehouden en u ze hebt vernieuwd nadat ze verlopen als u wilt doorgaan bronnen.  U kunt doen door in te dienen andere `POST` verzoek om deel te de `/token` eindpunt, lang mits de `refresh_token` in plaats van de `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probeer deze aanvraag uitvoert in Postman! (Vergeet niet voor het vervangen van de `refresh_token`)     [ ![Uitvoeren in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | -------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's tenant.  Zie voor meer details, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| grant_type | Vereist | Moet `refresh_token` voor deze arm van de stroom van de code autorisatie. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken.  De bereiken aangevraagd in dit gedeelte moet gelijk aan of een deel van de bereiken in de oorspronkelijke authorization_code verzoek arm verzocht.  Als de bereiken die in deze aanvraag reeks meerdere resource-servers, retourneert het eindpunt v2.0 een token voor de resource die is opgegeven in het eerste bereik.  Voor een gedetailleerde uitleg van bereiken, raadpleegt u [machtigingen, toestemming te geven, en bereiken](active-directory-v2-scopes.md).  |
| refresh_token | Vereist | De refresh_token die u in de tweede zijde van de stroom hebt aangeschaft.   |
| redirect_uri | Vereist | De dezelfde redirect_uri waarde die is gebruikt voor het ophalen van de authorization_code. |
| client_secret | vereist voor WebApps | De toepassing geheim die u in de portal van de registratie van de app hebt gemaakt voor de app.  Deze moet niet worden gebruikt in een systeemeigen app, omdat client_secrets betrouwbaar op apparaten kan niet worden opgeslagen.  Dit is vereist voor het WebApps en web API's waarvoor de mogelijkheid om op te slaan de client_secret veilig op de server. |

#### <a name="successful-response"></a>Positief antwoord
Een positief token antwoord eruitziet:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De gevraagde toegangstoken. De app kunt u deze token gebruiken om de beveiligde resource, zoals een web API te verifiëren. |
| token_type | Geeft de waarde type token. Het enige type die ondersteuning biedt voor Azure AD is vruchtdragende  |
| expires_in | Hoe lang het toegangstoken geldt (in seconden). |
| bereik | De bereiken waarvoor de access_token geldig is. |
| refresh_token |  Een nieuwe OAuth 2.0 vernieuwen token. Met deze token recent overgenomen vernieuwen om ervoor te zorgen dat uw tokens vernieuwen blijven geldig voor het zo lang mogelijk moet u het oude vernieuwen token vervangen.  |
| id_token | Een niet-ondertekende JSON Web Token (JWT). De app kunnen base64Url decoderen de segmenten van deze token informatie opvragen over de gebruiker die heeft aangemeld. De app kan de waarden in de cache en waar ze worden weergegeven, maar deze mag niet volledig vertrouwen worden voor alle autorisatie en beveiligingsgrenzen.  Zie voor meer informatie over id_tokens de [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Foutmelding
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |
| error_codes | Een lijst met STS specifieke foutcodes die in diagnostische hulpprogramma's kunnen helpen.  |
| tijdstempel | De tijd waarop de fout is opgetreden. |
| trace_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens.  |
| correlation_id | Een unieke id voor de aanvraag die u kunt bij het oplossen van diagnostische gegevens over de onderdelen. |

Voor een beschrijving van de foutcodes en de actie aanbevolen client, raadpleegt u [foutcodes voor token-eindpunt fouten](#error-codes-for-token-endpoint-errors).
