<properties
    pageTitle="Azure AD v2.0 token verwijzing | Microsoft Azure"
    description="De soorten tokens en claims dat door het eindpunt v2.0"
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="v20-token-reference"></a>v2.0 token verwijzing

Het eindpunt v2.0 Hiermee plaatst u diverse soorten beveiligingstokens in de verwerking van elke [verificatie stroom](active-directory-v2-flows.md). In dit document wordt beschreven voor de indeling, de beveiligingskenmerken en de inhoud van elk type token.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Soorten tokens

Het eindpunt v2.0 ondersteunt het [OAuth 2.0 autorisatie protocol](active-directory-v2-protocols.md), welke maakt gebruik van zowel access_tokens als refresh_tokens.  Ondersteunt ook verificatie en aanmelden via [OpenID verbinding maken](active-directory-v2-protocols.md#openid-connect-sign-in-flow), waarin maakt u kennis met een derde type token, de id_token.  Elk van deze tokens wordt weergegeven als een 'dragertoken'.

Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. In deze zin is de "toonder" een partij die het token kunt presenteren. Hoewel een partij moet eerst worden geverifieerd bij Azure AD voor het ontvangen van de dragertoken, als de vereiste stappen zijn niet die u hebt gemaakt voor het beveiligen van de token in overdracht en opslag, kunt u er onderschept en die wordt gebruikt door een onbedoelde feest. Sommige beveiligingstokens beschikken over een ingebouwde methode voor het voorkomen dat onbevoegden ze te gebruiken, wordt aan toonder tokens geen deze methode hebt en moeten worden overgebracht in een beveiligd kanaal zoals transport layer beveiliging (HTTPS). Als een dragertoken wordt verzonden in het wissen, kan een man in de middelste aanval worden gebruikt door een schadelijke partij bij het aanschaffen van het token en deze gebruiken voor een onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer wilt opslaan of caching van vruchtdragende tokens voor later gebruik. Altijd Zorg ervoor dat uw app verzendt en vruchtdragende tokens op een veilige manier opgeslagen. Zie voor meer beveiligingsoverwegingen op vruchtdragende tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).

Veel van de tokens uitgegeven door het eindpunt v2.0 worden geïmplementeerd als Json Web Tokens of JWTs.  Een JWT is een compacte, URL geschikt van het overbrengen van gegevens tussen twee partijen.  De gegevens in JWTs worden genoemd 'claims' of bevestigingen van informatie over de vruchtdragende en het onderwerp van het token.  De claims in JWTs zijn JSON objecten codering en serienummer voor de overdracht.  Aangezien de JWTs uitgegeven door het eindpunt v2.0 zijn aangemeld, maar niet gecodeerd, kunt u de inhoud van een JWT eenvoudig controleren voor foutopsporing. Voor meer informatie over JWTs, kunt u verwijzen naar de [JWT specificatie](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens zijn een soort aanmeldingsproblemen beveiligingstoken dat uw app ontvangt tijdens de verificatie met [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Ze worden weergegeven als [JWTs](#types-of-tokens)en claims die u gebruiken kunt voor het ondertekenen van de gebruiker in uw app bevatten.  U kunt de claims gebruiken in een id_token zoals naar eigen inzicht: vaak ze worden gebruikt voor het weergeven van de accountgegevens of beslissingen access besturingselement in een app.  Het eindpunt v2.0 problemen slechts één type id_token, waarvoor een consistente set claims ongeacht het type gebruiker die heeft aangemeld.  Dat is zegt dat de indeling en de inhoud van de id_tokens zal de geldt ook voor beide persoonlijk Microsoft-Account-gebruikers en accounts voor werk- of schoolaccount.

Id_tokens zijn aangemeld, maar niet op dit moment worden gecodeerd.  Wanneer uw app een id_token ontvangt, moet deze [de handtekening valideren](#validating-tokens) om te bewijzen van het token authenticiteit en een paar claims in het token om te bewijzen de geldigheid valideren.  De claims gevalideerd door een app varieert naargelang scenario vereisten, maar er zijn enkele [veelvoorkomende claimen validatie](#validating-tokens) die uw app moet worden uitgevoerd in elke scenario.

Volledige informatie over de claims in id_tokens hieronder, evenals de id_token van een steekproef.  Houd er rekening mee dat de claims in id_tokens niet in een bepaalde volgorde worden geretourneerd.  Daarnaast nieuwe claims wordt toegevoegd aan id_tokens op elk gewenst moment in tijd - uw app beter niet verbreken, zoals nieuwe claims worden geïntroduceerd.  De onderstaande lijst bevat de claims die uw app betrouwbaar op het moment van deze schrijven kunt interpreteren.  Desgewenst nog meer details vindt u in de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Voorbeeld id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Voor de oefening, kunt u proberen de claims in de steekproef id_token controleren door in [calebb.net](https://calebb.net)te plakken.

#### <a name="claims-in-idtokens"></a>Op claims in id_tokens
| Naam | Claimen | Van Voorbeeldwaarde | Beschrijving |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Publiek | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Geeft de geadresseerde van het token.  In id_tokens is het publiek-Id van uw app-toepassing, zoals die zijn toegewezen aan uw app in de portal van de registratie van de app.  Uw app moet deze waarde valideren en het token negeren als deze niet overeenkomen. |
| Uitgever | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Geeft de beveiliging token service (STS) dat wordt gemaakt en geeft als resultaat het token, evenals de Azure AD-tenant waarin de gebruiker is geverifieerd.  Uw app moet de uitgever claimen om ervoor te zorgen dat de token is afkomstig van het eindpunt v2.0 valideren.  Dit moet ook het gedeelte guid van de claim gebruiken voor het Beperk het aantal tenants dat u zich aanmeldt bij de app is toegestaan.  De guid die aangeeft van de gebruiker is een gebruiker consumenten van Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Uitgegeven aan | `iat` | `1452285331` | De tijd waarop het token is uitgegeven, dat wordt aangeduid in epoche tijd. |
| Verlooptijd | `exp` | `1452289231` | Het tijdstip waarop het token ongeldig is, wordt weergegeven in epoche tijd.  Uw app moet deze claim gebruiken om te controleren of de geldigheid van de levensduur van tokens.  |
| Niet eerder | `nbf` | `1452285331` |  Het tijdstip waarop het token geldig is, wordt weergegeven in epoche tijd. Dit is meestal hetzelfde als de tijd uitgifte.  Uw app moet deze claim gebruiken om te controleren of de geldigheid van de levensduur van tokens.  |
| Versie | `ver` | `2.0` | De versie van de id_token, zoals gedefinieerd door Azure AD.  Voor het eindpunt v2.0, de waarde is `2.0`. |
| Tenant-Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Een guid dat staat voor het Azure AD tenant die afkomstig is van de gebruiker.  Voor werk en school-accounts is de guid de onveranderlijke tenant-ID van de organisatie die de gebruiker behoort.  Voor persoonlijke accounts is de waarde is `9188040d-6c67-4c5b-b112-36a304b66dad`.  De `profile` bereik is vereist om te kunnen deze claim ontvangen. |
| Code Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | De code-hash is opgenomen in id_tokens alleen wanneer de id_token is uitgegeven samen met een OAuth 2.0 autorisatie-code.  Dit kan worden gebruikt voor het valideren van de echtheid van een autorisatiecode.  Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie op de uitvoering van deze validatie. |
| Access Token Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | De access token hash is opgenomen in id_tokens alleen wanneer de id_token is uitgegeven samen met een OAuth 2.0-toegangstoken.  Dit kan worden gebruikt voor het valideren van de echtheid van een toegangstoken.  Zie de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html) voor meer informatie op de uitvoering van deze validatie. |
| Nonce | `nonce` | `12345` | De nonce is een strategie voor herhaling van het token-aanvallen te beperken.  Uw app een nonce kunt opgeven in een verzoek autorisatie met behulp van de `nonce` queryparameter.  De waarde die u in het verzoek opgeeft wordt worden weergegeven in van de id_token `nonce` claim, niet gewijzigd.  Hiermee kunt uw app om te controleren of de waarde ten opzichte van de waarde die het opgegeven op de aanvraag, die van de app-sessie worden gekoppeld aan een bepaald id_token.  Uw app moet deze validatie tijdens de validatie id_token uitvoeren. |
| Naam | `name` | `Babe Ruth` | De claim biedt een menselijke leesbare waarde die aangeeft van het onderwerp van het token. Deze waarde is niet gegarandeerd uniek is veranderlijke en is ontworpen voor gebruik alleen ter informatie weergegeven.  De `profile` bereik is vereist om te kunnen deze claim ontvangen. |
| E-mail | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Het primaire e-mailadres dat is gekoppeld aan de gebruikersaccount, indien aanwezig.  De waarde is veranderlijke en na verloop van tijd voor een bepaalde gebruiker kan veranderen.  De `email` bereik is vereist om te kunnen deze claim ontvangen. |
| Voorkeur gebruikersnaam | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | De primaire gebruikersnaam die wordt gebruikt om aan te geven van de gebruiker in het eindpunt v2.0.  Mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een opgegeven notatie.  De waarde is veranderlijke en na verloop van tijd voor een bepaalde gebruiker kan veranderen.  De `profile` bereik is vereist om te kunnen deze claim ontvangen. |
| Onderwerp | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | De hoofdsom waarover het token gegevens, zoals de gebruiker van een app bevestigingen. Wordt deze waarde onveranderlijke en kan niet opnieuw worden toegewezen of opnieuw kan worden gebruikt, zodat deze kan worden gebruikt om uit te voeren autorisatie controles veilig, zoals wanneer het token wordt gebruikt voor toegang tot een resource. Omdat het onderwerp altijd aanwezig zijn in de tokens de Azure AD-problemen is, het is raadzaam deze waarde in een algemene autorisatiesysteem gebruiken. |
| Object-id | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | De object-Id van het account voor werk- of schoolaccount in het Azure AD-systeem.  Deze claim wordt niet uitgevoerd voor persoonlijke Microsoft-accounts.  De `profile` bereik is vereist om te kunnen deze claim ontvangen. |


## <a name="access-tokens"></a>Access tokens

Access tokens uitgegeven door het eindpunt v2.0 zijn op dit moment alleen verbruikbare door Microsoft-Services.  Uw apps hoeft niet te validatie of controle van access tokens voor een van de momenteel ondersteunde scenario's uitvoeren.  U kunt access tokens behandelen als volledig ondoorzichtig - ze zijn alleen tekenreeksen die uw app naar Microsoft in HTTP-aanvragen doorgeven kunt.

Het eindpunt v2.0 wordt in de nabije toekomst, de mogelijkheid om uw app access tokens ontvangen van andere clients introduceren.  Op dat moment worden deze gegevens worden bijgewerkt met de gegevens die uw app moet access token gegevensvalidatie en andere vergelijkbare taken uitvoeren.

Wanneer u een toegangstoken bij het eindpunt v2.0 aanvraagt, retourneert het eindpunt v2.0 ook enkele metagegevens over de toegangstoken voor verbruik van uw app.  Deze informatie bevat de verlooptijd van het toegangstoken en de bereiken waarvoor geldig is.  Hierdoor uw app om uit te voeren Intelligente caching van access tokens zonder te hoeven openen parseren het toegangstoken zelf.

## <a name="refresh-tokens"></a>Tokens vernieuwen

Vernieuwen tokens zijn beveiligingstokens waarin uw app kunt nieuw in het bezit van toegang tot tokens in een stroom OAuth 2.0.  Kan de app om te bereiken langdurige toegang tot bronnen namens een gebruiker zonder tussenkomst door de gebruiker.

Vernieuwen tokens zijn meerdere resource.  Dat wil zeggen worden dat een vernieuwen token ontvangen tijdens een token-aanvraag voor een resource kan ingewisseld voor access tokens aan een volledig andere resource.

Vernieuwen in een token antwoord ontvangen, uw app moet aanvragen en krijgen de `offline_acesss` bereik.   Voor meer informatie over de `offline_access` bereik, verwijst naar het [hier toestemming & bereiken artikel](active-directory-v2-scopes.md).

Tokens vernieuwen, en altijd zullen zijn, volledig ondoorzichtig naar uw app.  Ze kunnen worden uitgegeven door het eindpunt van Azure AD v2.0 en alleen worden gecontroleerd en door het eindpunt v2.0 beschouwd.  Ze zijn lange levensduur, maar de app niet verwacht dat een token vernieuwen voor een periode duurt moet worden geschreven.  Vernieuwen tokens kunnen zijn ongeldig gemaakt op elk moment voor tal van redenen.  De enige manier voor uw app weet ik of een token vernieuwen geldig is is als u wilt deze door een token aanvraag naar het eindpunt v2.0 inwisselen.

Wanneer u een token vernieuwen voor een nieuwe toegangstoken inwisselen (en als uw app is verleend de `offline_access` bereik), ontvangt u een nieuwe vernieuwen token in de token reactie.  U moet het token NET zijn uitgegeven vernieuwen opslaan die u in het verzoek te vervangen.  Hiermee wordt garanderen dat uw tokens vernieuwen geldig voor het zo lang mogelijk blijven.

## <a name="validating-tokens"></a>Tokens valideren

De alleen token validatie die uw apps moeten moet uitvoeren, is op dit moment, id_tokens valideren.  Als u wilt een id_token valideren, moet uw app van de id_token handtekening zowel de claims in de id_token valideren.

<!-- TODO: Link -->
Wij bieden bibliotheken en voorbeelden van de code die wordt aangegeven hoe u omgaat met eenvoudig token validatie - de onderstaande informatie is gewoon opgegeven voor mensen die u wilt meer informatie over het onderliggende proces.  Er zijn ook verschillende 3e partijen bron openen bibliotheken beschikbaar zijn voor JWT validatie - is ten minste één optie voor vrijwel elke platform & uitfaden er taal.

#### <a name="validating-the-signature"></a>De handtekening valideren
Een JWT bevat drie segmenten, welke worden gescheiden door de `.` teken.  Het eerste segment is bekend als de **koptekst**, de tweede als de **hoofdtekst**en de derde als de **handtekening**.  Het segment handtekening kan worden gebruikt voor het valideren van de echtheid van de id_token zodat deze kan worden vertrouwd door uw app.

Id_Tokens hebben aangemeld met industriële standaard asymmetrische coderingsalgoritmen, zoals RSA 256. De kop van de id_token bevat informatie over de sleutel en versleuteling methode gebruikt voor het ondertekenen van het token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

De `alg` claim geeft aan dat het algoritme dat is gebruikt om aan te melden toegangstoken, terwijl de `kid` claim wordt aangegeven dat de bepaalde openbare sleutel waarmee u zich de token aan te melden.

Op elk gewenst moment in tijd, het eindpunt v2.0 Meld u aan een id_token met een van een bepaalde reeks paren openbare en persoonlijke sleutels.  Het eindpunt v2.0 Hiermee draait u de mogelijke set toetsen periodiek, zodat uw app u omgaat met deze belangrijke wijzigingen automatisch moet worden geschreven.  Een redelijk frequentie wilt controleren op updates voor de openbare sleutels die worden gebruikt door het eindpunt v2.0 is bijna 24 uur.

U kunt in het bezit van de ondertekend belangrijke gegevens die nodig zijn voor het valideren van de handtekening met behulp van de metagegevens OpenID verbinding document zich bevindt:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Probeer deze URL in een browser!

Dit metagegevensdocument is een handige stukjes gegevens, zoals de locatie van de verschillende eindpunten vereist voor de uitvoering van verificatie OpenID verbinding maken met JSON-object.  

Bevat ook een `jwks_uri`, waardoor de locatie van de set van openbare sleutels gebruikt voor het ondertekenen van tokens.  De JSON-document te vinden op de `jwks_uri` bevat alle gegevens van de openbare sleutel op die bepaald moment wordt gebruikt.  Uw app de beschikking over de `kid` claimen in de koptekst JWT om te selecteren welk openbare sleutel in dit document aan te melden een bepaalde token is gebruikt.  Deze kan handtekeningvalidatie met de juiste openbare sleutel en de algoritme van de opgegeven uitvoeren.

Het valideren van de handtekening is buiten het bereik van dit document - bibliotheken met veel bron openen er zijn beschikbaar voor het zodat u dat doen indien nodig.

#### <a name="validating-the-claims"></a>De claims valideren
Wanneer uw app een id_token bij aanmelding ontvangt, moet deze ook enkele controles met de vorderingen uitvoeren in de id_token.  Deze bevatten, maar zijn niet beperkt tot:

- Het **publiek** claim - om te bevestigen dat de id_token is bedoeld om te worden opgegeven naar uw app.
- Het **Niet voor** en **Verlooptijd** claims - om te bevestigen dat de id_token niet is verlopen.
- De **uitgever** claim - om te bevestigen dat de token daadwerkelijk naar uw app door het eindpunt v2.0 uitgegeven is.
- De **Nonce** - als een herhaling van het token aanval risicobeperking.
- en meer...

Voor een volledige lijst met claimen validatie die uw app moet uitvoeren, raadpleegt u de [specificatie OpenID verbinding maken](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details van de verwachte waarden voor deze claims worden opgenomen boven in de [sectie id_token](#id_tokens).


## <a name="token-lifetimes"></a>Token levensduur

De volgende token levensduur worden gegeven zuiver voor uw lidmaatschap, terwijl ze helpen kunnen ontwikkelen en om apps op.  Uw apps niet moeten worden geschreven naar een van deze levensduur ongewijzigd verwachten: zij kunnen en op elk gewenst moment wordt gewijzigd in tijd.

| Token | Levensduur | Beschrijving |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (werk of school-accounts) | 1 uur | Id_Tokens zijn meestal geldig is gedurende een uur.  Uw web-app kunt gebruik deze dezelfde levensduur in een eigen sessie onderhouden met de gebruiker (aanbevolen) of kiest u de levensduur van een volledig andere sessie.  Als uw app moet u een nieuwe id_token, worden deze gewoon nodig om te maken van een nieuwe aanmeldingsproblemen verzoek om de v2.0 Autoriseer eindpunt.  Als de gebruiker een geldige browsersessie met het eindpunt v2.0 heeft, kunnen ze niet worden telkens opnieuw in te voeren hun referenties. |
| Id_Tokens (persoonlijke accounts) | 24 uur | Id_Tokens voor persoonlijke accounts zijn meestal geldig 24 uur.  Uw web-app kunt gebruik deze dezelfde levensduur in een eigen sessie onderhouden met de gebruiker (aanbevolen) of kiest u de levensduur van een volledig andere sessie.  Als uw app moet u een nieuwe id_token, worden deze gewoon nodig om te maken van een nieuwe aanmeldingsproblemen verzoek om de v2.0 Autoriseer eindpunt.  Als de gebruiker een geldige browsersessie met het eindpunt v2.0 heeft, kunnen ze niet worden telkens opnieuw in te voeren hun referenties. |
| Access Tokens (werk of school-accounts) | 1 uur | Zoals aangegeven in token antwoorden als onderdeel van de token metagegevens. |
| Access Tokens (persoonlijke accounts) | 1 uur | Zoals aangegeven in token antwoorden als onderdeel van de token metagegevens.  Access_tokens uitgegeven namens persoonlijke accounts mogelijk geconfigureerd voor de levensduur van een andere, maar één uur is meestal de hoofdletters/kleine letters. |
| Vernieuwen Tokens (werk of school-account) | Maximaal 14 dagen | Een token één vernieuwen geldt voor een maximum van 14 dagen.  Echter de token vernieuwen mogelijk ongeldig op elk gewenst moment voor een aantal redenen, zodat uw app probeert en gebruikt u een token vernieuwen totdat deze mislukt of totdat de app wordt vervangen door een nieuwe vernieuwen token moet blijven.  Een token vernieuwen wordt ook ongeldig als het al 90 dagen geleden hun referenties door de gebruiker heeft ingevoerd. |
| Vernieuwen Tokens (persoonlijke accounts) | Maximaal 1 jaar | Een token één vernieuwen geldt voor maximaal 1 jaar.  Echter de token vernieuwen mogelijk ongeldig op elk gewenst moment voor een aantal redenen, zodat uw app blijven moet proberen en gebruiken van een token vernieuwen totdat deze niet meer. |
| Autorisatiecodes (werk of school-accounts) | 10 minuten | Autorisatiecodes zijn opzet tijdelijk en direct moeten worden ingewisseld voor access_tokens en refresh_tokens wanneer ze worden ontvangen. |
| Autorisatiecodes (persoonlijke accounts) | 5 minuten | Autorisatiecodes zijn opzet tijdelijk en direct moeten worden ingewisseld voor access_tokens en refresh_tokens wanneer ze worden ontvangen.  Autorisatiecodes uitgegeven namens persoonlijke accounts zijn ook eenmalig gebruik. |
