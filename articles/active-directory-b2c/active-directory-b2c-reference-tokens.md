<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="De soorten tokens uitgegeven in de Azure Active Directory B2C."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token verwijzing

Azure Active Directory (Azure AD) B2C genereert diverse soorten beveiligingstokens zoals elke [verificatie stroom](active-directory-b2c-apps.md)worden verwerkt. In dit document wordt beschreven voor de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="types-of-tokens"></a>Soorten tokens

Azure AD B2C ondersteunt het [OAuth 2.0 autorisatie protocol](active-directory-b2c-reference-protocols.md), welke maakt gebruik van access tokens zowel vernieuwen tokens. Ondersteunt ook verificatie en aanmelden via [OpenID verbinding maken](active-directory-b2c-reference-protocols.md), waarin maakt u kennis met een derde type token: het ID-token. Elk van deze tokens wordt weergegeven als een dragertoken.

Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. De toonder is een partij die het token kunt presenteren. Azure AD moet een partij eerst worden geverifieerd voordat deze kan een dragertoken ontvangen. Maar als de vereiste stappen worden niet geopend secure de token in overdracht en opslag, kan deze kan worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens hebben een ingebouwde methode voor het voorkomen dat onbevoegden ze te gebruiken, maar vruchtdragende tokens ik heb geen deze methode. Zij moeten worden overgebracht in een beveiligd kanaal, zoals transport laag beveiliging (HTTPS).

Als een dragertoken buiten een beveiligd kanaal wordt verzonden, een schadelijke partij een aanval man in het midden in het bezit van het token en kunt gebruiken om toegang te krijgen onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes toepassen wanneer vruchtdragende tokens worden opgeslagen of in cache opslaan voor later gebruik. Altijd Zorg ervoor dat uw app verzendt en vruchtdragende tokens op een veilige manier opgeslagen.

Zie voor extra beveiligingsoverwegingen op vruchtdragende tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens die Azure AD B2C problemen worden geïmplementeerd als JSON web tokens (JWTs). Een JWT is een compacte, URL veilige methode voor het overbrengen van gegevens tussen twee partijen. JWTs bevatten informatie bekend als claims. Hierna ziet u bevestigingen van informatie over de toonder en het onderwerp van het token. De claims in JWTs zijn JSON-objecten die worden gecodeerd en serienummer voor de overdracht. Omdat de JWTs uitgegeven door Azure AD B2C zijn ondertekend maar niet versleuteld, kunt u de inhoud van een JWT voor foutopsporing deze eenvoudig controleren. Verschillende hulpmiddelen die beschikbaar zijn die dit, inclusief [calebb.net](http://calebb.net)kunt doen. Raadpleeg voor meer informatie over JWTs, [JWT specificaties](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID tokens

Een token ID is een soort beveiligingstoken dat uw app van de Azure AD-B2C ontvangt `authorize` en `token` eindpunten. ID tokens worden weergegeven als [JWTs](#types-of-tokens), en ze bevatten claims die u gebruiken kunt om gebruikers in uw app te identificeren. Wanneer de ID tokens worden opgehaald uit de `authorize` eindpunt, worden vaak gebruikt gebruikers webtoepassingen aanmelden. Wanneer de ID tokens worden opgehaald uit de `token` eindpunt, ze kunnen worden verzonden in HTTP-aanvragen tijdens de communicatie tussen twee onderdelen van de dezelfde toepassing of service. U kunt de claims gebruiken in een token ID wens naar. Ze worden meestal gebruikt om weer te geven van de accountgegevens of beslissingen access besturingselement in een app.  

ID tokens bent aangemeld, maar ze zijn niet gecodeerd op dit moment. Wanneer uw app of API ontvangt een token ID, moet deze [de handtekening valideren](#token-validation) om te bewijzen dat de token authentiek is. Uw app of API moet u ook een paar claims in het token om te bewijzen dat dit geldig is valideren. Afhankelijk van de vereisten scenario de claims gevalideerd door een app kunnen variëren, maar uw app enkele [veelvoorkomende claimen validatie](#token-validation) in elke scenario moet uitvoeren.

#### <a name="sample-id-token"></a>Voorbeeld-ID token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Access tokens

Een toegangstoken is ook een soort beveiligingstoken dat uw app van de Azure AD-B2C ontvangt `authorize` en `token` eindpunten. Access tokens worden ook weergegeven als [JWTs](#types-of-tokens), en ze bevatten claims die u gebruiken kunt om gebruikers in uw webservices en API's te identificeren.

Access tokens bent aangemeld, maar ze zijn niet versleutelde op dit moment- en lijkt veel op id tokens.  Access tokens moeten worden gebruikt voor toegang tot webservices en API's en voor het identificeren en de gebruiker in deze services te verifiëren.  Ze bieden echter niet een bevestiging van vergunning aan deze services.  Dat wil zeggen dat de `scp` claimen in access tokens niet beperken of anderszins vertegenwoordigen de toegang van het onderwerp van het token is verleend.

Wanneer uw API een toegangstoken ontvangt, moet deze [de handtekening valideren](#token-validation) om te bewijzen dat de token authentiek is. Uw API moet een paar claims in het token om te bewijzen dat dit geldig is ook valideren. Afhankelijk van de vereisten scenario de claims gevalideerd door een app kunnen variëren, maar uw app enkele [veelvoorkomende claimen validatie](#token-validation) in elke scenario moet uitvoeren.

### <a name="claims-in-id--access-tokens"></a>Op claims in ID & Access Tokens

Wanneer u Azure AD B2C gebruikt, hebt u fijnmazige controle over de inhoud van uw tokens. U kunt [beleidsregels](active-directory-b2c-reference-policies.md) voor het verzenden van bepaalde sets met gebruikersgegevens in claims die uw app is vereist voor de bewerkingen configureren. Deze claims standaardeigenschappen zoals van de gebruiker kunnen opnemen `displayName` en `emailAddress`. Ze kunnen ook [aangepaste kenmerken](active-directory-b2c-reference-custom-attr.md) die u in uw adreslijst B2C definiëren kunt bevatten. Elke-ID en access token die u ontvangt een bepaalde set van claims-beveiliging bevat. Uw toepassingen kunnen u deze claims gebruiken om te verifiëren veilig gebruikers en aanvragen.

Houd er rekening mee dat de gegevens in de ID tokens niet in een bepaalde volgorde worden geretourneerd. Bovendien kunnen u nieuwe claims geïntroduceerd in ID tokens op elk gewenst moment. Uw app moet niet verbreken als nieuwe claims worden geïntroduceerd. Hier volgen de claims die u moet bestaan in-ID en access tokens uitgegeven door Azure AD B2C. Eventuele aanvullende claims wordt bepaald door de beleidsregels. Oefening kunt u proberen de claims in het voorbeeld-ID-token controleren door in [calebb.net](http://calebb.net)te plakken. Meer informatie vindt u in de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html).

| Naam | Claimen | Van Voorbeeldwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Publiek | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Een doelgroep claim kunt u de geadresseerde van het token identificeren. Voor Azure AD B2C is het publiek-ID van uw app-toepassing, zoals die zijn toegewezen aan uw app in de portal van de registratie van de app. Uw app moet deze waarde valideren en het token negeren als deze niet overeenkomen. |
| Uitgever | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Deze claim geeft de beveiliging token service (STS) dat wordt gemaakt en geeft als resultaat het token. Ook worden de Azure AD-map waarin de gebruiker is geverifieerd. Uw app moet de uitgever claimen om ervoor te zorgen dat de token is afkomstig van het eindpunt v2.0 valideren. |
| Uitgegeven aan | `iat` | `1438535543` | Dit verzoek is de tijd waarop het token is uitgegeven, dat wordt aangeduid in epoche tijd. |
| Verlooptijd | `exp` | `1438539443` | De verlooptijd claim is het tijdstip waarop het token ongeldig is, wordt weergegeven in epoche tijd. Uw app moet deze claim gebruiken om te controleren of de geldigheid van de levensduur van tokens.  |
| Niet eerder | `nbf` | `1438535543` | Dit verzoek is het tijdstip waarop het token geldige, weergegeven in epoche tijd wordt. Dit is meestal hetzelfde als de tijd die het token is uitgegeven. Uw app moet deze claim gebruiken om te controleren of de geldigheid van de levensduur van tokens.  |
| Versie | `ver` | `1.0` | Dit is de versie van de token ID, zoals gedefinieerd door Azure AD. |
| Code hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een hash code wordt opgenomen in een token ID alleen wanneer de token is uitgegeven samen met een OAuth 2.0 autorisatie-code. Een hash code kan worden gebruikt voor het valideren van de echtheid van een autorisatiecode. Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het uitvoeren van deze validatie. |
| Access token hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Een access-token-hash is opgenomen in een token ID alleen wanneer de token is uitgegeven samen met een OAuth 2.0-toegangstoken. Een access-token-hash kan worden gebruikt voor het valideren van de echtheid van een toegangstoken. Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie over het uitvoeren van deze validatie. |
| Nonce | `nonce` | `12345` | Een nonce is een strategie die wordt gebruikt voor het aantal herhaling van het token-aanvallen. Uw app een nonce kunt opgeven in een verzoek autorisatie met behulp van de `nonce` queryparameter. De waarde die u in het verzoek opgeeft worden verzonden niet wordt gewijzigd de `nonce` claimen van een ID-token alleen. Hiermee kunt uw app om te controleren of de waarde ten opzichte van de waarde die het opgegeven op de aanvraag, die van de app-sessie worden gekoppeld aan een bepaald ID-token. Uw app moet deze validatie tijdens de ID token validatie uitvoeren. |
| Onderwerp | `sub` | `Not supported currently. Use oid claim.` | Dit is een principal waarover het token gegevens, zoals de gebruiker van een app bevestigingen. Deze waarde onveranderlijke is en niet kan worden toegewezen of opnieuw kan worden gebruikt. Dit kan worden gebruikt om uit te voeren autorisatie controles veilig, zoals wanneer het token wordt gebruikt voor toegang tot een resource. Echter is de claim onderwerp nog niet geïmplementeerd in de Azure AD-B2C. U moet uw beleid als u wilt opnemen van de object-ID configureren `oid` claimen en gebruik de waarde naar gebruikers identificeren, in plaats van het onderwerp verzoek om autorisatie gebruiken. |
| Verificatie context class verwijzing | `acr` | `b2c_1_sign_in` | Dit is de naam van het beleid dat is gebruikt voor het ophalen van het ID-token.  |
| Verificatie-tijd | `auth_time` | `1438535543` | Dit verzoek is de tijd waarop een gebruiker laatste ingevoerde referenties, dat wordt aangeduid in epoche tijd. |


### <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen tokens beveiligingstokens die uw app gebruiken kunt bij het aanschaffen van nieuwe ID tokens en toegang tot tokens in een stroom OAuth 2.0 zijn. Ze uw app langdurige toegang bieden tot resources namens gebruikers zonder interactie met deze gebruikers.

Voor het ontvangen van een vernieuwing token een token antwoord wordt uw app moet aanvragen de `offline_acesss` bereik. Voor meer informatie over de `offline_access` bereik, verwijst naar de [Azure AD B2C protocol verwijzing](active-directory-b2c-reference-protocols.md).

Tokens vernieuwen, en altijd zullen zijn, volledig ondoorzichtig naar uw app. Ze kunnen worden uitgegeven door Azure AD en worden gecontroleerd en alleen op basis van Azure AD beschouwd. Ze lange levensduur zijn, maar de app niet moet worden geschreven met de verwachting die een token vernieuwen voor een bepaalde periode duurt. Vernieuwen tokens kunnen zijn ongeldig gemaakt op elk moment voor tal van redenen. De enige manier voor uw app weet ik of een token vernieuwen geldig is is als u wilt deze door een token aanvraag naar Azure AD inwisselen.

Wanneer u een token vernieuwen voor een nieuwe token inwisselen (en als uw app is verleend de `offline_access` bereik), ontvangt u een nieuwe vernieuwen token in de token reactie. Sla het token NET zijn uitgegeven vernieuwen. Moet vervangen door deze het vernieuwen token die u eerder hebt gebruikt in het verzoek. Hiermee kunt garanderen dat uw tokens vernieuwen geldig voor het zo lang mogelijk blijven.

## <a name="token-validation"></a>Token gegevensvalidatie

Een token valideren, moet uw app de handtekening en de claims van het token controleren.

Veel bron openen bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van de voorkeurstaal. Het is raadzaam dat u deze opties verkennen in plaats van uw eigen validatielogica implementeren. De informatie in deze handleiding kunt u informatie over het gebruik correct die bibliotheken.

### <a name="validate-the-signature"></a>De handtekening valideren
Een JWT bevat drie segmenten, gescheiden door de `.` teken. Het eerste segment wordt de **koptekst**, de tweede is de **hoofdtekst**en de derde is de **handtekening**. Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van het token zodat deze kan worden vertrouwd door uw app.

Azure AD B2C tokens hebben aangemeld met behulp van de standaard-asymmetrische coderingsalgoritmen, zoals RSA 256. De kop van het token bevat informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

De `alg` claimen geeft aan dat het algoritme dat is gebruikt voor het ondertekenen van het token. De `kid` claimen geeft aan dat de bepaalde openbare sleutel waarmee u zich de token aan te melden.

Op elk gewenst kan Azure AD Meld u aan een token met behulp van een van een bepaalde reeks paren openbare en persoonlijke sleutels. Azure AD Hiermee draait u de mogelijke set toetsen regelmatig, zodat uw app u omgaat met deze belangrijke wijzigingen automatisch moet worden geschreven. Een redelijk frequentie wilt controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD is elke 24 uur.

Azure AD B2C heeft een metagegevens OpenID verbinding eindpunt. Hiermee kunt apps kunt u informatie over Azure AD B2C gedurende runtime ophalen. Deze informatie bevat eindpunten, token-inhoud en token-ondertekening toetsen. Het telefoonboek van uw B2C bevat een document JSON metagegevens voor elk beleid. Bijvoorbeeld de metagegevensdocument voor de `b2c_1_sign_in` beleid in de `fabrikamb2c.onmicrosoft.com` bevindt zich op:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`de map B2C is gebruikt voor het verifiëren van de gebruiker, en `b2c_1_sign_in` is het beleid kon u in het bezit van het token. Om te bepalen welk beleid is gebruikt voor het ondertekenen van een token (en waar kunt u voor het ophalen van de metagegevens), hebt u twee opties. Eerst de naam van het beleid is opgenomen in de `acr` claimen in het token. U kunt claims afmelden bij de hoofdtekst van het JWT parseren door base 64 decoderen van de hoofdtekst en het deserialiseren van de JSON-tekenreeks die het resultaat is. De `acr` claimen moet de naam van het beleid dat is gebruikt voor het verlenen van het token.  Uw andere optie is voor het coderen van het beleid in de waarde van de `state` parameter wanneer u het verzoek verzenden en klikt u vervolgens decoderen om te bepalen welk beleid is gebruikt. Beide methoden is ongeldig.

Het metagegevensdocument is een JSON-object dat aantal handige gegevens bevat. Het gaat hierbij om de locatie van de eindpunten vereist verificatie OpenID verbinding uitvoeren. Ook de opties omvatten `jwks_uri`, aan te melden tokens die resulteert in de locatie van de set van openbare sleutels die worden gebruikt. Dat locatie hier is opgegeven, maar het beste kunt u de locatie dynamisch ophalen met behulp van het metagegevensdocument en parseren `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

De JSON-document zich bevindt op deze URL bevat alle gegevens van de openbare sleutel gebruikt op een bepaald moment. Uw app de beschikking over de `kid` claimen in de koptekst JWT de openbare sleutel in de JSON-document dat wordt gebruikt voor het ondertekenen van een bepaalde token selecteren. Dit kan handtekeningvalidatie uitvoeren met behulp van de juiste openbare sleutel en de algoritme van de opgegeven.

Een beschrijving van het uitvoeren van handtekening gegevensvalidatie is buiten het bereik van dit document. Veel bron openen bibliotheken zijn beschikbaar om u te helpen met dit als u deze nodig hebt.

### <a name="validate-the-claims"></a>De claims valideren
Wanneer uw app of API ontvangt een token ID, moet deze ook verschillende controles met de vorderingen uitvoeren in het ID-token. Deze opnemen, maar zijn niet beperkt tot:

- Het **publiek** claimen: Hiermee wordt gecontroleerd dat het ID-token is bedoeld om te worden opgegeven naar uw app.
- De claims **niet vóór** en **Verlooptijd** : dit bevestigen dat het ID-token niet is verlopen.
- De **uitgever** claimen: Hiermee wordt gecontroleerd of het token naar uw app is uitgegeven door Azure AD.
- De **nonce**: dit is een strategie voor herhaling van het token aanval risicobeperking.

Raadpleeg de [specificatie OpenID verbinding maken](https://openid.net)voor een volledige lijst validatie die uw app moet uitvoeren. Details van de verwachte waarden voor deze claims worden opgenomen in de voorgaande [token sectie](#types-of-tokens).  

## <a name="token-lifetimes"></a>Token levensduur

De volgende token levensduur worden te bevorderen van uw kennis gegeven. Ze kunnen u helpen bij het ontwikkelen en fouten opsporen in apps. Houd er rekening mee dat uw apps niet naar een van deze levensduur ongewijzigd verwachten moeten worden geschreven. Ze kunnen en wordt gewijzigd.  U vindt meer informatie over het aanpassen van token levensduur in Azure AD B2C [hier](active-directory-b2c-token-session-sso.md).

| Token | Levensduur | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| ID tokens | Eén uur | ID tokens zijn meestal geldig voor een uur. Uw web-app kunt deze levensduur gebruiken om een eigen sessies onderhouden met gebruikers (aanbevolen). U kunt ook de levensduur van een andere sessie. Als uw app een nieuwe ID token ophalen moet, moet deze gewoon Azure AD van een nieuwe vergaderverzoek aanmeldingsproblemen aanbrengen. Als een gebruiker een geldige browsersessie met Azure Active Directory heeft, kan die gebruiker niet worden telkens opnieuw in te voeren referenties. |
| Tokens vernieuwen | Maximaal 14 dagen | Een token één vernieuwen geldt voor een maximum van 14 dagen. Echter een token vernieuwen mogelijk ongeldig op elk gewenst moment voor een aantal redenen. Uw app moet blijven probeert te gebruiken van een token vernieuwen totdat het verzoek mislukt, of uw app vervangt de token vernieuwen met een nieuwe record.  Een token vernieuwen kan ook ongeldig als 90 dagen is verstreken sinds de gebruiker referenties voor het laatst ingevoerd. |
| Autorisatiecodes | Vijf minuten | Autorisatiecodes zijn met opzet hebt tijdelijk. Ze moeten worden ingewisseld direct voor tokens van access, ID tokens of vernieuwen tokens wanneer ze worden ontvangen. |
