<properties
    pageTitle="Azure AD .NET Protocol overzicht | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u toegang tot webtoepassingen en web API's in de tenant met behulp van de Azure Active Directory en verbindt u OpenID Autoriseer met HTTP-berichten."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Autoriseer toegang tot webtoepassingen met OpenID verbinding en Azure Active Directory

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) is een eenvoudige identiteit laag gebouwd het OAuth 2.0-protocol. OAuth 2.0 definieert regelingen als u wilt downloaden en gebruiken van **access tokens** voor toegang tot beveiligde bronnen, maar definiëren niet standaard methoden om identiteit informatie te verstrekken. OpenID verbinding implementeert verificatie als een uitbreiding van het OAuth 2.0 autorisatieproces, verstrekken van informatie over de eindgebruiker in de vorm van een `id_token` die wordt gecontroleerd met de identiteit van de gebruiker, evenals biedt eenvoudige profielgegevens over de gebruiker.

OpenID Connect is onze aanbeveling als u een webtoepassing die wordt gehost op een server en via een browser geopend.

## <a name="authentication-flow-using-openid-connect"></a>Verificatie-mailstroom met OpenID verbinden

De belangrijkste stroom aanmeldingsproblemen bevat de volgende stappen uit: elk van deze wordt gedetailleerd hieronder beschreven.

![OpenId verbinding verificatie-mailstroom](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Verzend het verzoek aanmelden

Wanneer uw webtoepassing moet de gebruiker te verifiëren, moet deze de gebruiker om te verwijzen de `/authorize` eindpunt. Deze aanvraag is vergelijkbaar met de eerste zijde van de [Flow van OAuth 2.0 autorisatie Code](active-directory-protocols-oauth-code.md), met enkele belangrijke verschillen:

- De aanvraag moet bevatten het bereik `openid` in de `scope` parameter.
- De `response_type` parameter moet bevatten `id_token`.
- De aanvraag moet bevatten de `nonce` parameter.

Zodat een voorbeeld van een aanvraag er als volgt uit:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | --------------- |
| tenant | Vereist | De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie kan Meld u aan bij de toepassing.  De toegestane waarden zijn tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` of `contoso.onmicrosoft.com` of `common` voor de tenant-onafhankelijke tokens |
| client_id | Vereist | De toepassings-Id toegewezen aan uw app wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de klassieke Azure-Portal. Klik op **Active Directory**, klikt u op de map, klikt u op de toepassing en klik vervolgens op in **configureren** |
| response_type | Vereist | Moet bevatten `id_token` voor OpenID verbinding aanmeldingsproblemen.  Dit kan ook andere response_types, zoals bevatten `code`. |
| bereik | Vereist | Een door spaties gescheiden lijst met bereiken.  OpenID verbinding kunt maken, moet deze het bereik opnemen `openid`, die equivalent de machtiging 'Aanmelden u' in de toestemming UI.  U mag ook andere bereiken opnemen in deze aanvraag voor het aanvragen van toestemming. |
| nonce | Vereist | Een waarde die is opgenomen in de aanvraag, gegenereerd door de app die u in de resulterende opnemen wilt `id_token` als een claimen.  De app kunt vervolgens deze waarde om te beperken herhaling van het token-aanvallen controleren.  De waarde is meestal een willekeurig, unieke tekenreeks of GUID die kan worden gebruikt om aan te geven van de oorsprong van het verzoek.  |
| redirect_uri | aanbevolen | De redirect_uri van de app, waar verificatie antwoorden kunnen worden verzonden en ontvangen door de app.  Dit moet precies overeenkomen met een van de redirect_uris die u in de portal geregistreerd, behalve het url-codering moet zijn. |
| response_mode | aanbevolen | Hiermee geeft u de methode die moet worden gebruikt voor het verzenden van de resulterende authorization_code terug naar uw app.  Ondersteunde waarden zijn `form_post` voor *HTTP formulier posten* of `fragment` voor *URL-fragment*.  Voor webtoepassingen raden gebruiken `response_mode=form_post` voor de meest beveiligde overdracht van tokens in uw toepassing.  
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  Een willekeurig wordt gegenereerd unieke waarde wordt meestal gebruikt voor [voorkoming van meerdere sites verzoek aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |
| prompt | optionele | Geeft het type van de interactie van de gebruiker die is vereist.  De enige geldige waarden op dit moment zijn 'login', 'geen' en 'toestemming'.  `prompt=login`de gebruiker hun referenties invoeren verzocht, negatief maken van eenmalige aanmelding, worden gedwongen.  `prompt=none`is het tegenovergestelde - zorgt ervoor dat dat de gebruiker niet met vragen dan ook wordt gepresenteerd.  Als het verzoek kan niet worden voltooid in stilte via eenmalige aanmelding, kan het eindpunt een fout zullen retourneren.  `prompt=consent`wordt het OAuth toestemming dialoogvenster geactiveerd nadat de gebruiker zich aanmeldt, waarin de gebruiker machtigen om de app wordt gevraagd. |
| login_hint | optionele | Kan worden gebruikt voor vooraf aanvullen het veld gebruikersnaam/e-mail adres van de aanmeldingspagina voor de gebruiker, als u weet dat hun gebruikersnaam tijd vooraf.  Vaak apps deze parameter wordt gebruikt tijdens opnieuw worden geverifieerd, de gebruikersnaam die al worden opgehaald uit een vorige aanmelden met de `preferred_username` claimen. |


De gebruiker wordt nu gevraagd naar hun referenties invoeren en de verificatie te voltooien.

### <a name="sample-response"></a>Antwoord van de steekproef

Een reactie steekproef kan nadat de gebruiker is geverifieerd, er zo uit:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De `id_token` die de app wordt gevraagd. U kunt de `id_token` om te controleren of de gebruikers id en begin met een sessie met de gebruiker.  |
| de staat | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd. Een willekeurig gegenereerde unieke waarde wordt meestal gebruikt voor [voorkoming van meerdere sites verzoek aanvallen](http://tools.ietf.org/html/rfc6749#section-10.12).  De staat, wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |

### <a name="error-response"></a>Foutmelding
Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat ze correct laten door de app afhandelen:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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
|server_error | De server is een onverwachte fout opgetreden. | Probeer het verzoek. Deze fouten kunnen resulteren uit tijdelijke voorwaarden. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker die het antwoord is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable | De server is tijdelijk bezet u omgaat met het verzoek. | Probeer het verzoek. De clienttoepassing mogelijk wordt uitgelegd aan de gebruiker dat het antwoord is vertraagd vanwege een tijdelijke voorwaarde. |
| invalid_resource |De doel-resource is ongeldig omdat deze niet bestaat, Azure AD kan deze niet vinden of deze juist niet is geconfigureerd.| Hiermee geeft u aan dat de resource, indien aanwezig, niet is geconfigureerd in de tenant. De toepassing kan de gebruiker met instructies voor het installeren van de toepassing en toevoegen aan Azure AD gevraagd. |

## <a name="validate-the-idtoken"></a>De id_token valideren

Alleen ontvangen een `id_token` is niet voldoende zijn voor de verificatie van de gebruiker. u moet de handtekening en controleer of de claims in de `id_token` per vereisten van uw app. Het eindpunt Azure AD maakt gebruik van JSON Web Tokens (JWTs) en openbare sleutel cryptografische af wilt melden tokens en controleert u of ze geldig zijn.

U kunt kiezen voor het valideren van de `id_token` in gewoon code, maar gebruikelijk is om te verzenden de `id_token` op een back endserver en de validatie er uitvoeren. Nadat u de handtekening van hebt gevalideerd de `id_token`, er zijn een paar claims, u moet om te bevestigen.

U kunt ook voor het valideren van extra claims afhankelijk van uw scenario. Enkele veelvoorkomende validatie opnemen:

- Ervoor zorgen dat de gebruiker/organisatie zich heeft aangemeld voor de app.
- Ervoor zorgen dat de gebruiker heeft BEGINLETTERS autorisatie/bevoegdheden
- Ervoor zorgen dat een bepaalde sterkte van verificatie is opgetreden, zoals meervoudige verificatie.

Zodra u hebt volledig gevalideerd de `id_token`, u kunt beginnen met een sessie met de gebruiker en gebruiken van de claims in de `id_token` voor informatie over de gebruiker in uw app. Deze informatie kan worden gebruikt voor weergave, records, vergunningen, enzovoort. Lees voor meer informatie over de token typen en claims [Token ondersteund en claimtypen](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Een afmelden verzoek verzenden

Wanneer u ondertekenen van de gebruiker afmelden bij de app wilt, is het niet voldoende wissen van uw app cookies of anderszins einde de sessie met de gebruiker.  U moet de gebruiker ook omleiding de `end_session_endpoint` voor afmelden.  Als u niet kunt doen, kunnen de gebruiker opnieuw worden geverifieerd bij uw app zonder hun referenties opnieuw in te voeren omdat ze een geldige eenmalige aanmelding-sessie met het Azure AD-eindpunt hebben.

U kunt de gebruiker gewoon omleiden de `end_session_endpoint` vermeld in het document metagegevens OpenID verbinding maken:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter | | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | aanbevolen | De URL die de gebruiker wordt omgeleid naar na succesvolle Meld u af.  Als u niet is opgenomen, wordt de gebruiker een algemeen bericht weergegeven.  |

## <a name="token-acquisition"></a>Token overname

Veel WebApps moeten niet alleen Meld u aan de gebruiker in, maar ook toegang tot een webservice namens die gebruiker OAuth gebruiken. Dit scenario combineert OpenID verbinden voor gebruikersverificatie terwijl tegelijk bij het verkrijgen van een `authorization_code` die kunnen worden gebruikt om `access_tokens` met het OAuth autorisatie Code Flow.

## <a name="get-access-tokens"></a>Access Tokens krijgen

Als u wilt in het bezit van access tokens, moet u het verzoek aanmeldingsproblemen dan hierboven wijzigen:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Machtiging bereiken op te nemen in de aanvraag en `response_type=code+id_token`, de `authorize` eindpunt zorgt ervoor dat het dat de gebruiker heeft ingestemd met de machtigingen zoals aangegeven in de `scope` queryparameter en uw app een autorisatiecode voor exchange voor een toegangstoken te retourneren.

### <a name="successful-response"></a>Positief antwoord

Een positief antwoord met `response_mode=form_post` lijkt op:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| id_token | De `id_token` die de app wordt gevraagd. U kunt de `id_token` om te controleren of de gebruikers id en begin met een sessie met de gebruiker. |
| code | De authorization_code die de app wordt gevraagd. De app kunt u de autorisatiecode gebruiken aanvragen van een toegangstoken voor de resource doel. Authorization_codes zijn zeer korte levensduur, meestal ze verlopen na ongeveer 10 minuten. |
| de staat | Als een parameter status is opgenomen in de aanvraag hetzelfde resultaat moet worden weergegeven in de reactie. De app moet controleren of dat de waarden staat in de vergaderverzoeken en antwoorden identiek zijn. |

### <a name="error-response"></a>Foutmelding

Antwoorden van de fout kunnen ook worden verzonden naar de `redirect_uri` zodat ze correct laten door de app afhandelen:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschrijving |
| ----------------------- | ------------------------------- |
| Fout | Een tekenreeks van de fout code die kan worden gebruikt om de soorten fouten die optreden classificeren en kan worden gebruikt om te reageren op fouten. |
| error_description | Een specifiek foutbericht waarmee een ontwikkelaar kunt identificeren de oorzaak van een verificatiefout.  |

Voor een beschrijving van de mogelijke foutcodes en hun actie aanbevolen client, raadpleegt u [foutcodes autorisatie eindpunt fouten](#error-codes-for-authorization-endpoint-errors).

Nadat u een vergunning hebt verbeterd `code` en een `id_token`, u kunt de gebruiker melden en kennis van access tokens namens hen.  Als u wilt melden de gebruiker in, moet u valideren de `id_token` precies zoals hierboven is beschreven. Als u toegangstokens, kunt u de stappen beschreven in onze [documentatie OAuth-protocol](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token).
