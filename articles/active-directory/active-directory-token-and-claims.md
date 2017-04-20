 <properties
   pageTitle="Azure AD-Token verwijzing | Microsoft Azure"
   description="Een handleiding voor het begrijpen en evaluatie van de claims in de SAML 2.0 en JSON Web Tokens (JWT) tokens uitgegeven door Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD token verwijzing

Azure Active Directory (Azure AD) genereert diverse soorten beveiligingstokens in de verwerking van elke stroom verificatie. In dit document wordt beschreven voor de indeling, de beveiligingskenmerken en de inhoud van elk type token.

## <a name="types-of-tokens"></a>Soorten tokens

Azure AD ondersteunt het [OAuth 2.0 autorisatie protocol](active-directory-protocols-oauth-code.md), welke maakt gebruik van zowel access_tokens als refresh_tokens.  Ondersteunt ook verificatie en aanmelden via [OpenID verbinding maken](active-directory-protocols-openid-connect-code.md), waarin maakt u kennis met een derde type token, de id_token.  Elk van deze tokens wordt weergegeven als een 'dragertoken'.

Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. In deze zin is de "toonder" een partij die het token kunt presenteren. Hoewel verificatie met Azure AD is vereist om te kunnen een dragertoken ontvangen, moet men de token, om te voorkomen dat onderschept door een onbedoelde partij beveiligen. Omdat vruchtdragende tokens geen een ingebouwde om om te voorkomen dat onbevoegden ze te gebruiken, moeten zij worden overgebracht in een beveiligd kanaal zoals transport laag beveiliging (HTTPS). Als een dragertoken wordt verzonden in het wissen, kan een man in de middelste aanval worden gebruikt in het bezit van het token en zo onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer wilt opslaan of caching van vruchtdragende tokens voor later gebruik. Altijd Zorg ervoor dat uw app verzendt en vruchtdragende tokens op een veilige manier opgeslagen. Zie voor meer beveiligingsoverwegingen op vruchtdragende tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens uitgegeven door Azure AD worden geïmplementeerd als JSON Web Tokens of JWTs.  Een JWT is een compacte, URL veilige methode voor het overbrengen van gegevens tussen twee partijen.  De gegevens in JWTs worden genoemd 'claims' of bevestigingen van informatie over de vruchtdragende en het onderwerp van het token.  De claims in JWTs zijn JSON objecten codering en serienummer voor de overdracht.  Aangezien de JWTs uitgegeven door Azure AD zijn aangemeld, maar niet gecodeerd, kunt u de inhoud van een JWT eenvoudig controleren voor foutopsporing.  Er zijn verschillende hulpmiddelen beschikbaar voor dit doet, zoals [jwt.calebb.net](http://jwt.calebb.net). Voor meer informatie over JWTs, kunt u verwijzen naar de [JWT specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens zijn een soort aanmeldingsproblemen beveiligingstoken dat uw app ontvangt tijdens de verificatie met [OpenID verbinden](active-directory-protocols-openid-connect-code.md).  Ze worden weergegeven als [JWTs](#types-of-tokens)en claims die u gebruiken kunt voor het ondertekenen van de gebruiker in uw app bevatten.  U kunt de claims gebruiken in een id_token zoals naar eigen inzicht: vaak ze worden gebruikt voor het weergeven van de accountgegevens of beslissingen access besturingselement in een app.

Id_tokens zijn aangemeld, maar niet op dit moment worden gecodeerd.  Wanneer uw app een id_token ontvangt, moet deze [de handtekening valideren](#validating-tokens) om te bewijzen van het token authenticiteit en een paar claims in het token om te bewijzen de geldigheid valideren.  De claims gevalideerd door een app varieert naargelang scenario vereisten, maar er zijn enkele [veelvoorkomende claimen validatie](#validating-tokens) die uw app moet worden uitgevoerd in elke scenario.

Zie de volgende sectie voor informatie op id_tokens claims, evenals de id_token van een steekproef.  Houd er rekening mee dat de claims in id_tokens niet in een bepaalde volgorde worden geretourneerd.  Daarnaast nieuwe claims wordt toegevoegd aan id_tokens op elk gewenst moment in tijd - uw app beter niet verbreken, zoals nieuwe claims worden geïntroduceerd.  De volgende lijst bevat de claims die uw app betrouwbaar op het moment van deze schrijven kunt interpreteren.  Desgewenst nog meer details vindt u in de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Voorbeeld id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Voor de oefening, kunt u proberen de claims in de steekproef id_token controleren door in [calebb.net](http://jwt.calebb.net)te plakken.

#### <a name="claims-in-idtokens"></a>Op claims in id_tokens

| JWT claimen | Naam | Beschrijving |
|-----------|------|-------------|
| `appid`| Toepassings-ID | De toepassing waarin het token wordt gebruikt voor toegang tot een resource wordt geïdentificeerd. De toepassing kan fungeren als zelf of namens een gebruiker. De toepassings-ID vertegenwoordigt meestal een application-object, maar dit kan ook een service principal object vertegenwoordigen in Azure AD. <br><br> **Voorbeeld JWT waarde**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Publiek | De geadresseerde van het token. De toepassing die u het token ontvangt moet Controleer of de waarde van het publiek juist is en alle tokens die is bedoeld voor een ander publiek negeren. <br><br> **Voorbeeld SAML waarde**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Voorbeeld JWT waarde**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Toepassing verificatie Context Class verwijzing | Geeft aan hoe de client is geverifieerd. Voor een openbare mailclient gebruikt is de waarde 0. Als de client-ID en geheim client worden gebruikt, is de waarde 1. <br><br> **Voorbeeld JWT waarde**: <br> `"appidacr": "0"`|
| `acr`| Verificatie Context Class verwijzing | Geeft aan hoe het onderwerp is geverifieerd, in plaats van de client in de toepassing verificatie Context Class verwijzing claimen. De waarde '0' geeft aan de verificatie van de eindgebruiker niet aan de vereisten van ISO/IEC 29115. <br><br> **Voorbeeld JWT waarde**: <br> `"acr": "0"`|
| | Verificatie chatberichten | Records van de datum en tijd waarop de verificatie is opgetreden. <br><br> **Voorbeeld SAML waarde**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Verificatiemethode | Wordt aangegeven hoe het onderwerp van het token is geverifieerd. <br><br> **Voorbeeld SAML waarde**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Voorbeeld JWT waarde**:`“amr”: ["pwd"]` |
| `given_name`| Voornaam | Biedt de eerste of 'krijgt"naam van de gebruiker, zoals ingesteld op het gebruikersobject Azure AD. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"given_name": "Frank"` |
| `groups`| Groepen | Biedt de object-id's waarmee het groepslidmaatschap van het onderwerp. Deze waarden zijn unieke (Zie Object-ID) en veilig kunnen worden gebruikt voor het beheren van access, zoals het afdwingen van machtiging voor toegang tot een resource. De groepen die zijn opgenomen in de groepen claimen zijn geconfigureerd per toepassing, via de eigenschap "groupMembershipClaims" van manifest van de toepassing. Een waarde van null-waarden worden alle groepen uitsluiten, een waarde van "SecurityGroup" bevat alleen Active Directory-beveiligingsgroep lidmaatschappen en een waarde van "Alle" kan bevatten beveiligingsgroepen en distributielijsten van Office 365. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Identiteitsprovider | Records van de identiteitsprovider die het onderwerp van het token geverifieerd. Deze waarde is gelijk aan de waarde van de uitgever claim tenzij het gebruikersaccount zich in een andere tenant dan de uitgever. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | De tijd waarop het token is uitgegeven opgeslagen. Dit wordt vaak gebruikt token fris meten. <br><br> **Voorbeeld SAML waarde**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Voorbeeld JWT waarde**: <br> `"iat": 1390234181` |
| `iss` | Uitgever | Geeft de beveiliging token service (STS) dat wordt gemaakt en geeft als resultaat het token. In de tokens die Azure AD retourneert, wordt de uitgever sts.windows.net. De GUID in de waarde van de uitgever claim is de ID van de tenant van de Azure AD-map. De ID van de tenant is een onveranderlijke en betrouwbare id van de map. <br><br> **Voorbeeld SAML waarde**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Voorbeeld JWT waarde**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Achternaam | Biedt de achternaam, achternaam, of de achternaam van de gebruiker, zoals gedefinieerd in het gebruikersobject Azure AD. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"family_name": "Miller"` |
| `unique_name`| Naam | Biedt een menselijke leesbare waarde die aangeeft van het onderwerp van het token. Deze waarde is niet gegarandeerd uniek zijn binnen een tenant en is ontworpen voor gebruik alleen ter informatie weergegeven. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Object-ID | Een unieke id van een object bevat in Azure AD. Deze waarde onveranderlijke is en niet kan worden toegewezen of opnieuw kan worden gebruikt. De object-ID gebruiken om een object in query's kunt Azure AD te identificeren. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Rollen | Hiermee geeft u alle rollen van de toepassing die het onderwerp is verleend direct en indirect via groepslidmaatschap en kan worden gebruikt om af te dwingen Rolgebaseerd toegangsbeheer. Toepassingsrollen zijn gedefinieerd op basis van het per toepassing, via de `appRoles` eigenschap van manifest van de toepassing. De `value` eigenschap van de toepassingsrol van elke is de waarde die wordt weergegeven in de rollen claimen. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Bereik | Geeft de imitatie machtigingen met de clienttoepassing. Standaard de machtiging is `user_impersonation`. De eigenaar van de beveiligde bron kunt extra waarden registreren in Azure AD. <br><br> **Voorbeeld JWT waarde**: <br> `"scp": "user_impersonation"`|
| `sub` |Onderwerp| Geeft de hoofdsom waarover het token gegevens, zoals de gebruiker van een toepassing bevestigingen. Wordt deze waarde onveranderlijke en kan niet opnieuw worden toegewezen of opnieuw kan worden gebruikt, zodat deze kan worden gebruikt om uit te voeren autorisatie controles veilig. Omdat het onderwerp altijd aanwezig zijn in de tokens de Azure AD-problemen is, het is raadzaam deze waarde in een algemene autorisatiesysteem gebruiken. <br> `SubjectConfirmation`is niet een claimen. Dit wordt beschreven hoe het onderwerp van het token is geverifieerd. `Bearer`Geeft aan dat het onderwerp is bevestigd door hun bezit van het token. <br><br> **Voorbeeld SAML waarde**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Voorbeeld JWT waarde**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Tenant-ID | Een onveranderlijke, herbruikbare-id waarmee de directory-tenant dat het token uitgegeven. U kunt deze waarde gebruiken voor toegang tot tenant / regiospecifieke directory resources in een toepassing voor meerdere tenant. Bijvoorbeeld, kunt u deze waarde om aan te geven van de tenant in een oproep tot de API Graph. <br><br> **Voorbeeld SAML waarde**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Voorbeeld JWT waarde**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Levensduur van tokens | Hiermee definieert u het tijdsinterval waarbinnen een token geldig is. De service die is gevalideerd met het token moet controleren of de huidige datum binnen de levensduur van tokens, anders die moet deze het token negeren. De service mogelijk voor maximaal vijf minuten buiten het bereik van de levensduur van tokens voor eventuele verschillen tussen de kloktijd ("tijd scheefheid") tussen Azure AD en de service. <br><br> **Voorbeeld SAML waarde**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Voorbeeld JWT waarde**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| User Principal Name | De gebruikersnaam van de UPN opgeslagen.<br><br> **Voorbeeld JWT waarde**: <br> `"upn": frankm@contoso.com`|
| `ver`| Versie | Het versienummer van het token opgeslagen. <br><br> **Voorbeeld JWT waarde**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Access tokens

Access tokens zijn op dit moment alleen verbruikbare door Microsoft-Services.  Uw apps hoeft niet te validatie of controle van access tokens voor een van de momenteel ondersteunde scenario's uitvoeren.  U kunt access tokens behandelen als volledig ondoorzichtig - ze zijn alleen tekenreeksen die uw app naar Microsoft in HTTP-aanvragen doorgeven kunt.

Wanneer u een toegangstoken aanvraagt, retourneert Azure AD ook enkele metagegevens over de toegangstoken voor verbruik van uw app.  Deze informatie bevat de verlooptijd van het toegangstoken en de bereiken waarvoor geldig is.  Hierdoor uw app om uit te voeren Intelligente caching van access tokens zonder te hoeven openen parseren het toegangstoken zelf.

## <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen tokens-beveiligingstokens die uw app gebruiken kunt om nieuwe access tokens in een stroom OAuth 2.0 zijn.  Kan de app om te bereiken langdurige toegang tot bronnen namens een gebruiker zonder tussenkomst door de gebruiker.

Vernieuwen tokens zijn meerdere resource, hetgeen betekent dat ze kunnen worden ontvangen tijdens een token-aanvraag voor een resource, maar hebt ingewisseld voor access tokens aan een volledig andere resource. Als meerdere resource opgeven, stelt u de `resource` parameter in de aanvraag voor de desbetreffende resource.

Vernieuwen tokens zijn geheel ondoorzichtig naar uw app. Ze zijn lange levensduur, maar de app niet verwacht dat een token vernieuwen voor een periode duurt moet worden geschreven.  Vernieuwen tokens kunnen zijn ongeldig gemaakt op elk moment voor tal van redenen.  De enige manier voor uw app weet ik of een token vernieuwen geldig is is geprobeerd aan inwisselen deze door een token aanvraag naar token Azure AD-eindpunt.

Wanneer u een token vernieuwen voor een nieuwe toegangstoken inwisselen, ontvangt u een nieuwe vernieuwen token in het token antwoord.  U moet het token NET zijn uitgegeven vernieuwen opslaan die u in het verzoek te vervangen.  Hiermee wordt garanderen dat uw tokens vernieuwen geldig voor het zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Tokens valideren

De alleen token validatie die uw client-app moet moet uitvoeren, is op dit moment, id_tokens valideren.  Als u wilt een id_token valideren, moet uw app van de id_token handtekening zowel de claims in de id_token valideren.

Wij bieden bibliotheken en voorbeelden van code zien hoe u omgaat met eenvoudig token validatie, als u wilt meer informatie over het onderliggende proces.  Er zijn ook verschillende bibliotheken van de bron openen van derden beschikbaar voor JWT validatie, ten minste één optie voor vrijwel elke platform en taal. Voor meer informatie over Azure AD-verificatie bibliotheken en voorbeelden van code, raadpleegt u [bibliotheken van Azure AD-verificatie](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>De handtekening valideren

Een JWT bevat drie segmenten, welke worden gescheiden door de `.` teken.  Het eerste segment is bekend als de **koptekst**, de tweede als de **hoofdtekst**en de derde als de **handtekening**.  Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van de id_token zodat deze kan worden vertrouwd door uw app.

Id_Tokens hebben aangemeld met industriële standaard asymmetrische coderingsalgoritmen, zoals RSA 256. De kop van de id_token bevat informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

De `alg` claimen geeft aan dat het algoritme dat is gebruikt om aan te melden toegangstoken, terwijl de `x5t` claimen geeft aan dat de bepaalde openbare sleutel waarmee u zich de token aan te melden.

Op elk gewenst moment in tijd, Azure AD Meld u aan een id_token met een van een bepaalde reeks paren openbare en persoonlijke sleutels. Azure AD Hiermee draait u de mogelijke set toetsen periodiek, zodat uw app u omgaat met deze belangrijke wijzigingen automatisch moet worden geschreven.  Een redelijk frequentie wilt controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD is elke 24 uur.

U kunt in het bezit van de ondertekend belangrijke gegevens die nodig zijn voor het valideren van de handtekening met behulp van de metagegevens OpenID verbinding document zich bevindt:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Probeer deze URL in een browser!

Dit metagegevensdocument is een handige stukjes gegevens, zoals de locatie van de verschillende eindpunten vereist voor de uitvoering van verificatie OpenID verbinding maken met JSON-object.  

Bevat ook een `jwks_uri`, waardoor de locatie van de set van openbare sleutels gebruikt voor het ondertekenen van tokens.  De JSON-document te vinden op de `jwks_uri` bevat alle gegevens van de openbare sleutel op die bepaald moment wordt gebruikt.  Uw app de beschikking over de `kid` claimen in de koptekst JWT om te selecteren welk openbare sleutel in dit document aan te melden een bepaalde token is gebruikt.  Deze kan handtekeningvalidatie met de juiste openbare sleutel en de algoritme van de opgegeven uitvoeren.

Het valideren van de handtekening is buiten het bereik van dit document - bibliotheken met veel bron openen er zijn beschikbaar voor het zodat u dat doen indien nodig.

#### <a name="validating-the-claims"></a>De claims valideren

Wanneer uw app een id_token bij aanmelding ontvangt, moet deze ook enkele controles met de vorderingen uitvoeren in de id_token.  Deze bevatten, maar zijn niet beperkt tot:

  - Het **publiek** claim - om te bevestigen dat de id_token is bedoeld om te worden opgegeven naar uw app.
  - Het **Niet voor** en **Verlooptijd** claims - om te bevestigen dat de id_token niet is verlopen.
  - De **uitgever** claim - om te bevestigen dat de token daadwerkelijk naar uw app door Azure AD uitgegeven is.
  - De **Nonce** - te verminderen van een aanval herhaling van het token.
  - en meer...

Voor een volledige lijst claimen validatie die uw app moet uitvoeren, raadpleegt u de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details van de verwachte waarden voor deze claims worden opgenomen in de voorgaande [sectie id_token](#id-tokens) sectie.

## <a name="sample-tokens"></a>Voorbeeld van Tokens

Deze sectie bevat voorbeelden van SAML en JWT tokens die Azure AD geeft als resultaat. Deze voorbeelden kunnen u de gegevens in de context te zien.
Op SAML Token

Dit is een voorbeeld van een typisch SAML-token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - gebruikersimitatie

Dit is een voorbeeld van een typisch JSON web token (JWT) gebruikt in een stroom autorisatie code verlenen.
Naast het claims bevat het token een versienummer in **ver** en **appidacr**, de verificatie context class verwijzing, waarmee wordt aangegeven hoe de client is geverifieerd. Voor een openbare mailclient gebruikt is de waarde 0. Als een client-ID of geheim client is gebruikt, is de waarde 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Verwante onderwerpen
- Zie de Azure AD Graph [beleid bewerkingen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en het [beleid entiteit](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), voor meer informatie over het beheren van de levensduur van tokens beleid via de API Azure AD Graph.
- Zie voor meer informatie en voorbeelden voor het beheren van beleid via PowerShell-cmdlets, inclusief voorbeelden, [configureerbare token levensduur in Azure AD](active-directory-configurable-token-lifetimes.md). 
