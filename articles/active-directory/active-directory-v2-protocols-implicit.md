<properties
    pageTitle="Azure AD v2.0 impliciete stroom | Microsoft Azure"
    description="Building webtoepassingen met behulp van Azure AD v2.0 uitvoering van de impliciete stroom voor één pagina apps."
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

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protocollen - kuuroorden met de impliciete stroom
Met het eindpunt v2.0, kunt u gebruikers zich aanmelden bij uw apps één pagina met persoonlijke en werk-, school-accounts van Microsoft.  Eén pagina en andere apps JavaScript uitvoeren die hoofdzakelijk in een browser nominale een paar interessante uitdagingen als het gaat om verificatie:

- De beveiligingskenmerken van deze apps zijn sterk afwijkt van webtoepassingen traditionele server gebaseerd.
- Veel autorisatie servers & id-providers bieden geen ondersteuning voor CORS aanvragen.
- Volledige pagina browser wordt omgeleid van de app worden vooral agressieve naar de gebruikerservaring af.

Voor deze toepassingen (denken: AngularJS, Ember.js, React.js, enzovoort) Azure AD ondersteunt de stroom OAuth 2.0 impliciete verlenen.  De stroom impliciet wordt beschreven in de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749#section-4.2).  De primaire voordeel is dat kan de app om toegang te krijgen van tokens van Azure AD zonder de uitvoering van een back-endserver referentie exchange.  Hiermee kunt de app Meld u aan de gebruiker en tokens naar andere web API's allemaal in de client krijgen JavaScript-code voor het behoud van sessie.  Er zijn enkele belangrijke beveiligingsoverwegingen rekening te houden bij gebruik van de impliciete stroom - specifiek rond de [client](http://tools.ietf.org/html/rfc6749#section-10.3) en [gebruikersimitatie](http://tools.ietf.org/html/rfc6749#section-10.3).

Als u de impliciete stroom en Azure AD verificatie toevoegen aan uw JavaScript-app gebruiken wilt, is het raadzaam dat u onze bron openen JavaScript-bibliotheek, [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)gebruikt.  Er zijn enkele AngularJS zelfstudies beschikbaar [hier](active-directory-appmodel-v2-overview.md#getting-started) om u aan de slag te helpen.  

Als u liever niet te gebruiken van een bibliotheek in uw app één pagina en protocolberichten Stuur uzelf, volgt u de volgende algemene stappen uit.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protocol-diagram
De hele impliciete aanmelden stroom er ongeveer zo uitziet: elk van de stappen hieronder uitvoerig worden beschreven.

![OpenId verbinding zwembanen](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Verzend het verzoek aanmelden

Als u wilt melden in eerste instantie de gebruiker in uw app, kunt u een autorisatie [OpenID verbinding](active-directory-v2-protocols-oidc.md) aanvraag verzendt en krijgen een `id_token` vanuit het eindpunt v2.0:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Klik op de onderstaande koppeling voor het uitvoeren van deze aanvraag! Na het aanmelden, uw browser wordt omgeleid naar `https://localhost/myapp/` met een `id_token` in de adresbalk.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's tenant.  Zie voor meer details, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| response_type | Vereist | Moet bevatten `id_token` voor OpenID verbinding aanmeldingsproblemen.  Deze eventueel ook de response_type `token`. Met `token` Hier wordt uw app ontvangt een toegangstoken onmiddellijk van het eindpunt autoriseren zonder te hoeven maken van een tweede verzoek om naar het eindpunt autoriseren geven.  Als u de `token` response_type, de `scope` parameter een bereik die aangeeft welk resource voor het verlenen van het token voor moet bevatten. |
| redirect_uri | aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve het url-codering moet zijn. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken.  OpenID verbinding kunt maken, moet deze het bereik opnemen `openid`, die equivalent de machtiging 'Aanmelden u' in de toestemming UI.  (Optioneel) u kunt ook om op te nemen de `email` of `profile` [bereiken](active-directory-v2-scopes.md) voor toegang tot aanvullende gebruikersgegevens.  U mag ook andere bereiken opnemen in deze aanvraag voor het aanvragen van toestemming voor verschillende bronnen.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende token terug naar uw app.  Moeten worden `fragment` voor de impliciete stroom.  |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  Een willekeurig wordt gegenereerd unieke waarde wordt meestal gebruikt voor [voorkoming van meerdere sites verzoek aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de app die u in de resulterende id_token als een claimen opnemen wilt.  De app kunt vervolgens deze waarde om te beperken herhaling van het token-aanvallen controleren.  De waarde is meestal een willekeurig, unieke tekenreeks die kan worden gebruikt om aan te geven van de oorsprong van het verzoek.  |
| prompt | optionele | Geeft het type van de interactie van de gebruiker die is vereist.  De enige geldige waarden op dit moment zijn 'login', 'geen' en 'toestemming'.  `prompt=login`de gebruiker hun referenties invoeren verzocht, negatief maken van eenmalige aanmelding, worden gedwongen.  `prompt=none`is het tegenovergestelde - zorgt ervoor dat dat de gebruiker niet met vragen dan ook wordt gepresenteerd.  Als het verzoek kan niet worden voltooid in stilte via eenmalige aanmelding, kan het eindpunt v2.0 een fout zullen retourneren.  `prompt=consent`activeert het OAuth toestemming dialoogvenster nadat de gebruiker zich aanmeldt, waarin de gebruiker machtigen om de app wordt gevraagd. |
| login_hint | optionele | Kan worden gebruikt om te vullen vooraf het veld gebruikersnaam/e-mail adres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tijd vooraf.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, de gebruikersnaam die al worden opgehaald uit een vorige aanmelden met de `preferred_username` claimen. |
| domain_hint | optionele | Zijn `consumers` of `organizations`.  Als opgenomen, wordt deze het e-mailbericht gebaseerde detectieproces overslaan die gebruiker doorloopt op de pagina, v2.0 aanmelden waardoor een iets meer gestroomlijnd gebruikerservaring.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, door het extraheren van de `tid` claimen uit de id_token.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

De gebruiker wordt nu gevraagd naar hun referenties invoeren en de verificatie te voltooien.  Het eindpunt v2.0 er ook voor zorgen dat de gebruiker heeft ingestemd met de machtigingen zoals aangegeven in de `scope` queryparameter.  Als de gebruiker niet heeft ingestemd met een van deze machtigingen, wordt deze vraag de gebruiker toe te staan de vereiste machtigingen.  Details van [machtigingen, toestemming te geven, en meerdere tenant apps hier worden beschreven](active-directory-v2-scopes.md).

Nadat de gebruiker wordt geverifieerd, en toestemming verleent, retourneert het eindpunt v2.0 een antwoord op uw app in de opgegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord

Een positief antwoord met `response_mode=fragment` en `response_type=id_token+token` er als volgt te werk met regeleinden voor leesbaarheid:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | Opgenomen als `response_type` bevat `token`. Het toegangstoken dat de app is aangevraagd, in dit geval voor de Microsoft Graph.  Het toegangstoken niet kan worden verkregen of moet anders gecontroleerd, deze kan worden behandeld als één tekenreeks. |
| token_type | Opgenomen als `response_type` bevat `token`.  Altijd `Bearer`. |
| expires_in | Opgenomen als `response_type` bevat `token`.  Geeft het aantal seconden dat het token geldig, voor caching van toepassing is. |
| bereik | Opgenomen als `response_type` bevat `token`.  Geeft de scope(s) waarvoor de access_token wordt gelden. |
| id_token | De id_token die de app wordt gevraagd. U kunt de id_token gebruiken om te controleren of de gebruikers id en begin met een sessie met de gebruiker.  Meer details over id_tokens en de inhoud ervan wordt opgenomen in de [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md).  |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |


#### <a name="error-response"></a>Foutmelding
Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat ze correct laten door de app afhandelen:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |

## <a name="validate-the-idtoken"></a>De id_token valideren
Alleen ontvangen een id_token is niet voldoende zijn voor de verificatie van de gebruiker. u moet de handtekening van de id_token valideren en controleer of de claims in het token per vereisten van uw app.  Het eindpunt v2.0 maakt gebruik van [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en openbare sleutel cryptografische af wilt melden tokens en controleert u of ze geldig zijn.

U kunt kiezen voor het valideren van de `id_token` in gewoon code, maar gebruikelijk is om te verzenden de `id_token` op een back endserver en de validatie er uitvoeren.  Nadat u de handtekening van de id_token hebt geverifieerd, zijn er enkele claims, u moet om te bevestigen.  Zie de [v2.0 token verwijzing](active-directory-v2-tokens.md) voor meer informatie, inclusief [Tokens valideren](active-directory-v2-tokens.md#validating-tokens) en [Belangrijke informatie over ondertekening toets Rollover](active-directory-v2-tokens.md#validating-tokens).  Het is raadzaam om die gebruikmaken van een bibliotheek voor parseren en valideren tokens - is ten minste één beschikbaar voor de meeste talen en platforms.
<!--TODO: Improve the information on this-->

U kunt ook voor het valideren van extra claims afhankelijk van uw scenario.  Enkele veelvoorkomende validatie opnemen:

- Ervoor zorgen dat de gebruiker/organisatie zich heeft aangemeld voor de app.
- Ervoor zorgen dat de gebruiker heeft BEGINLETTERS autorisatie/bevoegdheden
- Ervoor zorgen dat een bepaalde sterkte van verificatie is opgetreden, zoals meervoudige verificatie.

Voor meer informatie over de claims in een id_token, raadpleegt u [v2.0 eindpunt token verwijzing](active-directory-v2-tokens.md).

Zodra u hebt de id_token volledig gevalideerd, kunt u beginnen met een sessie met de gebruiker en de claims gebruiken in de id_token voor informatie over de gebruiker in uw app.  Deze informatie kan worden gebruikt voor weergave, records, vergunningen, enzovoort.

## <a name="get-access-tokens"></a>Access tokens krijgen

Nu dat u kunt de gebruiker hebt aangemeld bij uw app één pagina, kunt u access tokens ophalen voor bellen web API's die zijn beveiligd met Azure AD, zoals de [Microsoft Graph](https://graph.microsoft.io).  Zelfs als u al een token met ontvangen de `token` response_type, kunt u deze methode in het bezit van tokens naar aanvullende bronnen zonder dat u moet de gebruiker opnieuw aanmelden omleiden.

In de normale OpenID verbinding maken met/OAuth-stroom, zou u dit doen door een aanvraag naar de v2.0 `/token` eindpunt.  Echter ondersteunt het eindpunt v2.0 geen CORS aanvragen, dus gesprekken voeren AJAX voor het ophalen en vernieuwen tokens afmelden bij de vraag.  U kunt in plaats daarvan de impliciete stroom in een verborgen iframe gebruiken om nieuwe tokens voor andere web API's: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Probeer kopiëren en plakken de onder aanvraag in een browsertabblad! (Vergeet niet voor het vervangen van de `domain_hint` en de `login_hint` waarden met de juiste waarden voor de gebruiker)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn `common`, `organizations`, `consumers`, en id's tenant.  Zie voor meer details, [protocol basisbeginselen](active-directory-v2-protocols.md#endpoints). |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| response_type | Vereist | Moet bevatten `id_token` voor OpenID verbinding aanmeldingsproblemen.  Dit kan ook andere response_types, zoals bevatten `code`. |
| redirect_uri | aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve het url-codering moet zijn. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken.  Voor ophalen tokens, voegt u alle [bereiken](active-directory-v2-scopes.md) u nodig hebt voor de resource van belang.  |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende token terug naar uw app.  Zijn `query`, `form_post`, of `fragment`.  |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  Een willekeurig wordt gegenereerd unieke waarde wordt meestal gebruikt voor voorkoming van meerdere sites verzoek aanvallen.  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de app die u in de resulterende id_token als een claimen opnemen wilt.  De app kunt vervolgens deze waarde om te beperken herhaling van het token-aanvallen controleren.  De waarde is meestal een willekeurig, unieke tekenreeks die kan worden gebruikt om aan te geven van de oorsprong van het verzoek.  |
| prompt | Vereist | Voor vernieuwen & tokens opvragen in een verborgen iframe, moet u `prompt=none` om ervoor te zorgen dat de iframe nu niet op de aanmeldingspagina v2.0 vastloopt, en levert het onmiddellijk. |
| login_hint | Vereist | Voor vernieuwen & tokens opvragen in een verborgen iframe, moet u de gebruikersnaam van de gebruiker opnemen in deze geheugensteun om te kunnen onderscheiden tussen meerdere sessies die kan deze op een bepaald moment in tijd. U kunt de gebruikersnaam ophalen uit een vorige aanmelden met de `preferred_username` claimen. |
| domain_hint | Vereist | Zijn `consumers` of `organizations`.  Voor vernieuwen & tokens opvragen in een verborgen iframe, moet u de domain_hint opnemen in de uitnodiging.  U moet uitpakken de `tid` claimen uit de id_token van een vorige aanmelden om te bepalen welke waarde gebruiken.  Als de `tid` claimen waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`, moet u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Dank aan de `prompt=none` parameter, dit verzoek wordt hetzij slagen of mislukken meteen en terugkeren naar de toepassing.  Een positief antwoord wordt verzonden naar uw app in de opgegeven `redirect_uri`, met behulp van de methode die is opgegeven in de `response_mode` parameter.

#### <a name="successful-response"></a>Positief antwoord
Een positief antwoord met `response_mode=fragment` lijkt op:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| access_token | De token die de app wordt gevraagd. |
| token_type | Altijd `Bearer`. |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |
| expires_in | Hoe lang het toegangstoken geldt (in seconden). |
| bereik | De bereiken waarvoor het toegangstoken geldig is. |

#### <a name="error-response"></a>Foutmelding
Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat de app ze correct kan verwerken.  In het geval van `prompt=none`, een verwachte fout zijn:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |

Als u deze fout in de iframe-aanvraag ontvangt, moet de gebruiker interactief opnieuw aanmelden om op te halen van een nieuwe token.  U kunt in dit geval in een manier die relevant is voor uw toepassing.

## <a name="refreshing-tokens"></a>Tokens vernieuwen

Beide `id_token`s en `access_token`s verloopt na een korte periode, zodat uw app moet worden voorbereid vernieuwen deze tokens regelmatig.  Als u wilt vernieuwen beide soorten token, kunt u uitvoeren het dezelfde verborgen iframe-verzoek boven het gebruik van de `prompt=none` parameter om Azure AD werking te bepalen.  Als u wilt ontvangen een nieuwe `id_token`, moet u gebruiken `response_type=id_token` en `scope=openid`, evenals een `nonce` parameter.


## <a name="send-a-sign-out-request"></a>Een afmelden verzoek verzenden

De OpenIdConnect `end_session_endpoint` wordt momenteel niet ondersteund door het eindpunt v2.0. Dit betekent dat uw app niet kan een aanvraag verzendt naar het eindpunt v2.0 van een gebruiker sessie beëindigd en Schakel cookies instellen door het eindpunt v2.0.
Als u een gebruiker zich, wilt kunt uw app gewoon beëindigen eigen-sessie met de gebruiker en laat de sessie met de v2.0 eindpunt in-Zijtabs met letters.  De volgende keer dat de gebruiker probeert aan te melden, zien ze een pagina 'kiest u account' met hun actief zijn aangemeld bij-account wordt vermeld.
Klik op deze pagina kan de gebruiker zich wilt afmelden bij een account, de sessie met het eindpunt v2.0 kiezen.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->