
<properties
    pageTitle="Azure AD v2.0 OAuth Client referenties stroom | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>v2.0 protocollen - OAuth 2.0 clientreferenties Flow

De [referenties van de client OAuth 2.0 verlenen](http://tools.ietf.org/html/rfc6749#section-4.4), ook wel 'tweearmige OAuth' genoemd kan worden gebruikt voor toegang tot bronnen op het web gehost via de identiteit van een toepassing.  Deze wordt gebruikt voor de server naar server interacties die moeten worden uitgevoerd op de achtergrond zonder de onmiddellijke precense van een eindgebruiker.  Dit soort toepassingen zijn vaak **daemons** of **serviceaccounts**genoemd.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

In de specifiekere drie "driearmige OAuth," is een clienttoepassing gemachtigd voor toegang tot een resource namens een bepaalde gebruiker.  De machtiging is **overgedragen** van de gebruiker met de toepassing, meestal tijdens het proces [toestemming](active-directory-v2-scopes.md) .  Echter in de stroom van de referenties client machtigingen **rechtstreeks** op de toepassing zelf.  Wanneer de app een token aan een resource presenteert, de resource wordt afgedwongen dat de app zelf autorisatie voor een actie - niet dat sommige gebruiker autorisatie heeft uitgevoerd heeft.

## <a name="protocol-diagram"></a>Protocol-diagram
De volledige client referenties stroom er ongeveer zo uitziet: elk van de stappen hieronder uitvoerig worden beschreven.

![Stroom van client-referenties](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Directe autorisatie ophalen 
Er zijn twee manieren dat een app meestal rechtstreekse autorisatie voor toegang tot een resource ontvangt: via een toegangsbeheerlijst aan de resource of toewijzing van de toepassing machtiging in Azure AD.  Er zijn verschillende manieren die een resource wilt toestaan de clients en elke resource-server kan de methode kiezen die het meest geschikt voor de toepassing is.  Deze twee methoden zijn de meest gebruikte in Azure AD en reccommended voor clients en bronnen die u wilt uitvoeren van de client referenties stroom.

### <a name="access-control-lists"></a>Toegangscontrolelijsten
Een bepaalde bron-provider mogelijk een autorisatie voor controle op basis van een lijst van toepassings-id's dat deze bekend is en enkele bepaalde toegangsniveau verleent afdwingen.  Wanneer de resource wordt een token van het eindpunt v2.0 ontvangt, kan het token decoderen en extraheren de van de klant toepassings-ID uit het `appid` en `iss` claims.  Dit kan vervolgens vergelijken dat de toepassing tegen sommige toegangsbeheerlijst (ACL) bijhoudt.  De granulatie en methode van de toegangsbeheerlijst kunnen variëren aanzienlijk van resource aan resource.

Een algemene use-case voor deze ACL's is uitlopers voor een webtoepassing testen of web api.  Het web api mogelijk alleen verlenen een subset van de volledige machtigingen aan verschillende clients.  Om te kunnen complete tests uitvoeren op de api, een testclient, die is gemaakt, maar krijgt tokens afgeleid van het eindpunt v2.0 en verzendt deze tot de api.  De api kunt vervolgens ACL van de testclient toepassings-ID voor volledige toegang tot de api's volledige functionaliteit.  Houd er rekening mee dat als u bijvoorbeeld een lijst van uw service hebt, u moet ervoor dat u niet alleen valideren van de beller `appid`, maar ook valideren die de `iss` van het token is vertrouwde ook.

In dit type verificatie geldt voor daemons en serviceaccounts die moeten toegang hebben tot gegevens het eigendom van consumenten gebruikers met een persoonlijk Microsoft-account.  Het wordt aanbevolen dat u in het bezit van de benodigde vergunning via de toepassing perimssions voor gegevens eigendom van organisaties.

### <a name="application-permissions"></a>Toepassingsmachtigingen
In plaats van ACL's, API's kunt laten zien van een set **machtigingen** die aan een toepassing kan worden verleend.  De machtigingen van een toepassing aan een toepassing wordt verleend door een beheerder van een organisatie en kan alleen worden gebruikt voor toegang tot gegevens van die organisatie en werknemers.  De Microsoft Graph bevat bijvoorbeeld verschillende Toepassingsmachtigingen:

- E-mail lezen in alle postvakken
- Lezen en schrijven van e-mail in alle postvakken
- E-mail verzenden als een gebruiker
- Het lezen van directorygegevens
- [+ meer](https://graph.microsoft.io)

Om te kunnen in het bezit van deze machtigingen in uw app, kunt u de volgende stappen uitvoeren.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de portal van de registratie van de app aanvragen

- Ga in uw toepassing in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of [een app maken](active-directory-v2-app-registration.md) als u nog niet is gedaan.  U moet om ervoor te zorgen dat uw toepassing ten minste één toepassing geheim heeft gemaakt.
- Zoek de sectie **Rechtstreekse machtigingen van toepassing** en de machtigingen die moeten worden uw app toevoegen.
- Controleer of om op te **slaan** de registratie app

#### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: aan de gebruiker te melden bij uw app

Meestal wanneer de toepassingsmachtigingen bouwen van een toepassing die worden gebruikt, moeten de app hebben een/paginaweergave waarmee de beheerder om het goedkeuren van de app-machtigingen.  Deze pagina kunt deel uitmaken van aanmelding stroom van de app, deel uit van de instellingen van de app of een speciale 'koppelen' stroom.  In veel gevallen relevant dat is voor de app weer te geven 'koppelen' weergave alleen nadat een gebruiker heeft aangemeld met een werk of school Microsoft-account.

De gebruiker aanmelden bij de app, kunt u de organziation die de gebruiker hoort voordat het verzoek om goed te keuren voor de toepassingsmachtigingen identificeren.  Terwijl niet strikt noodzakelijk is, kunt u een meer intuïtieve-ervaring maken voor uw organisatie-gebruikers.  Als u wilt melden de gebruiker in, volgt u onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Vraag de machtigingen van een directory-beheerder

Wanneer u klaar met verzoek om machtigingen van de beheerder van het bedrijf bent, kunt u de gebruiker omleiden naar de v2.0 **beheerder toestemming eindpunt**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De directory-tenant die u wilt vraag machtiging uit.  Kan worden verstrekt in guid of de notatie voor de beschrijvende naam.  Als u niet weet welke tenant de gebruiker behoort en laten Meld u aan met een tenant wilt, voert u `common`. |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| redirect_uri | Vereist | De redirect_uri waar u het antwoord wilt sturen voor uw app worden afgehandeld.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve deze geen extra padsegmenten en url moet-codering. |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  De staat wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |

In dit stadium wordt Azure AD afdwingen dat alleen de tenantbeheerder van een mij aanmelden kan het verzoek te voltooien.  De beheerder krijgt het verzoek goed te keuren voor alle machtigingen rechtstreekse toepassing die u hebt aangevraagd voor de app in de registratie-portal. 

##### <a name="successful-response"></a>Positief antwoord
Als de beheerder de machtigingen voor uw toepassing goedkeurt, worden de positief antwoord:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | De directory-tenant die uw toepassing de machtigingen die dit verzoek is in guid-indeling. |
| de staat | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  De staat wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| admin_consent | Wordt ingesteld op `True`. |


##### <a name="error-response"></a>Foutmelding
Als de beheerder de machtigingen voor uw toepassing niet goedkeurt, worden het mislukte antwoord:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een fout.  |

Zodra u een positief antwoord van de inrichting van eindpunt app hebt ontvangen, heeft uw app de rechtstreekse Toepassingsmachtigingen die zij gevraagd ervaring.  U kunt nu verplaatsen naar het aanvragen van een token voor de gewenste resource.

## <a name="get-a-token"></a>Een token ophalen
Nadat u de benodigde vergunning hebt aangeschaft voor uw toepassing, kunt u doorgaan met het verkrijgen van access tokens voor API's.  Als u een van de client token referenties verlenen, verzendt u een POST-aanvraag naar de `/token` v2.0 eindpunt:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| bereik | Vereist | De waarde doorgegeven voor de `scope` parameter in deze aanvraag moet de URI (resource identifier App-ID) van de gewenste resource, aangebracht met de `.default` achtervoegsel.  Zodat het Microsoft Graph-voorbeeld gegeven, de waarde moet `https://graph.microsoft.com/.default`.  Deze waarde wordt het eindpunt v2.0 geïnformeerd dat van alle rechtstreekse Toepassingsmachtigingen die u voor uw app hebt geconfigureerd, deze aangeraden een token voor de regels die betrekking hebben op de gewenste resource. |
| client_secret | Vereist | De toepassing-geheim dat u hebt gegenereerd in de portal Registratie voor de app. |
| grant_type | Vereist | Moet `client_credentials`. | 

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord duurt het formulier:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De gevraagde toegangstoken. De app kunt u deze token gebruiken om de beveiligde resource, zoals een web API te verifiëren. |
| token_type | Geeft de waarde type token. Het enige type dat is Azure AD ondersteunt `Bearer`.  |
| expires_in | Hoe lang het toegangstoken geldt (in seconden). |

#### <a name="error-response"></a>Foutmelding
Een foutmelding duurt het formulier:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Een token gebruiken
Nu dat u een token hebt aangeschaft, kunt u deze token aanvragen aanbrengen in de resource.  Als het token verloopt, herhaalt u gewoon het verzoek om de `/token` eindpunt waar u een vers toegangstoken.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Codevoorbeeld
Als u wilt een voorbeeld van een toepassing dat implementeert die de client_credentials verlenen met de beheerder toestemming eindpunt wordt weergegeven, raadpleegt u onze [v2.0 daemon code steekproef](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
