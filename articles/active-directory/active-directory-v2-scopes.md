<properties
    pageTitle="Azure AD v2.0 bereiken, machtigingen en toestemming | Microsoft Azure"
    description="Een beschrijving van autorisatie in het Azure AD v2.0 eindpunt, inclusief bereiken, machtigingen en toestemming."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Bereiken, machtigingen en toestemming in het eindpunt v2.0

Apps die met Azure AD integreren Volg een bepaalde autorisatie model waarmee gebruikers om te bepalen hoe een app toegang krijgen tot hun gegevens.  De implementatie v2.0 van dit model autorisatie is bijgewerkt, wijzigen hoe een app moet werken met Azure AD.  Dit onderwerp behandelt de basisconcepten van dit model autorisatie, inclusief bereiken, machtigingen en toestemming.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Bereiken en machtigingen

Azure AD implementeert het [OAuth 2.0](active-directory-v2-protocols.md) autorisatie protocol, dat wil een methode waarmee een 3e partijen-app zeggen voor toegang tot web gehoste resources namens een gebruiker kan worden verleend.  Een web gehoste resource die is geïntegreerd met Azure AD heeft een resource-id of **App-ID-URI**.  Enkele Microsofts web gehoste resources zijn bijvoorbeeld:

- De Office 365 Unified Mail API:`https://outlook.office.com`
- De API Azure AD Graph:`https://graph.windows.net`
- De Microsoft Graph:`https://graph.microsoft.com`

Dit geldt voor 3e partijen bronnen die is geïntegreerd met Azure AD.  Een van deze resources kunt ook een set machtigingen die kunnen worden gebruikt om de functionaliteit van de desbetreffende resource in kleinere stukken verdelen definiëren.  De Microsoft Graph heeft een paar machtigingen gedefinieerd als u bijvoorbeeld:

- Lees de agenda van een gebruiker
- Aan de agenda van een gebruiker schrijven
- E-mail verzenden namens een gebruiker
- [+ meer](https://graph.microsoft.io)

Als u deze machtigingen, kan de resource fijnmazige controle over de gegevens en hoe deze wordt weergegeven in de wereld buiten hebben.  Een 3e partijen-app kunt vervolgens deze machtigingen aanvragen van een eindgebruiker - en de eindgebruiker moet de machtigingen goedkeuren voordat de app namens hen handelen kan.  Door de functionaliteit van de resource in kleinere machtigingensets logische groepen te verdelen, worden 3e partijen apps uitgerust om aan te vragen alleen de specifieke machtigingen die ze nodig hebt bij het uitvoeren van hun recht.  Bovendien kunt eindgebruikers informatie precies hoe een app hun gegevens gebruikt, zodat ze beter u weet dat de app werkt anders met schadelijke bedoelingen.

In Azure AD en OAuth, deze machtigingen worden genoemd **bereiken**.  Ziet u mogelijk ook deze **oAuth2Permissions**genoemd.  Een bereik is in Azure AD weergegeven als een tekenreekswaarde.  Kunt verdergaan met het Microsoft Graph-voorbeeld, is de scope-waarde voor elke machtiging:

- Lees de agenda van een gebruiker:`Calendar.Read`
- Schrijven naar de agenda van een gebruiker:`Mail.ReadWrite`
- E-mail verzenden namens een gebruiker:`Mail.Send`

Een app kunt deze machtigingen aanvragen door het opgeven van de bereiken in aanvragen voor het eindpunt v2.0, zoals hieronder beschreven.

## <a name="openid-connect-scopes"></a>Bereiken OpenId verbinding maken

De implementatie van het OpenID verbinding maken met de v2.0 biedt een paar duidelijke bereiken die niet op een bepaalde resource toegepast worden - `openid`, `email`, `profile`, en `offline_access`.

#### <a name="openid"></a>OpenId

Als een app aanmelden uitvoert met [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow), moet aanvragen de `openid` bereik.  De `openid` bereik in het scherm werk account toestemming als de machtiging 'Aanmelden u' en in het persoonlijke toestemming scherm van een Microsoft-account als de machtiging 'Uw profiel weergeven en verbinding maken met apps en services met uw Microsoft-account' wordt weergegeven.  Met deze machtiging kan een app voor het ontvangen van een unieke id voor de gebruiker in de vorm van de `sub` claimen.  Deze kan ook de app-toegang tot het eindpunt van de info gebruiker.  De `openid` bereik kan ook worden gebruikt bij het v2.0 token eindpunt aan te schaffen id_tokens, die kan worden gebruikt voor het beveiligen van HTTP-oproepen tussen de verschillende onderdelen van een app.

#### <a name="email"></a>E-mail

De `email` reikwijdte kan worden opgenomen langs de `openid` zoekbereik en alle andere.  Deze app toegang tot de primaire e-mailadres van de gebruiker in de vorm van biedt de `email` claimen.  De `email` claimen wordt alleen worden opgenomen in tokens als een e-mailadres is gekoppeld aan de gebruikersaccount, dat wil niet altijd de hoofdletters/kleine letters zeggen.  Als met de `email` reikwijdte, uw app moet worden voorbereid u omgaat met de hoofdletters/kleine letters waarin de `email` claimen bestaat niet in het token.

#### <a name="profile"></a>Profiel

De `profile` reikwijdte kan worden opgenomen langs de `openid` zoekbereik en alle andere.  Deze kan app toegang tot een enorme hoeveelheid informatie over de gebruiker.  Dit omvat, maar het is niet beperkt tot, van de gebruiker voornaam, achternaam, voorkeur gebruikersnaam, object-ID, enzovoort.  Voor een volledige lijst van het profiel vorderingen die voortvloeien uit een bepaalde gebruiker beschikbaar zijn in id_tokens, raadpleegt u de [v2.0 token verwijzing](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

De [ `offline_access` bereik](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) uw app voor toegang tot bronnen namens de gebruiker kan voor een langere periode.  In dit bereik wordt weergegeven in het scherm werk account toestemming als de machtiging "Toegang tot uw gegevens op elk gewenst moment".  Klik in het persoonlijke Microsoft-account toestemming scherm wordt weergegeven als de machtiging "Toegang tot uw info op elk gewenst moment".  Wanneer een gebruiker goedkeurt de `offline_access` reikwijdte, uw app worden ingeschakeld voor het ontvangen van tokens vernieuwen vanuit het v2.0 token-eindpunt.  Vernieuwen tokens zijn lange levensduur en uw app zoals oudere verloopt in het bezit van nieuwe access tokens geven.

Als uw app vraagt niet om de `offline_access` reikwijdte, ontvangt deze geen refresh_tokens.  Dit betekent dat wanneer u een authorization_code in de [OAuth 2.0 autorisatie code stroom](active-directory-v2-protocols.md#oauth2-authorization-code-flow)inwisselen, alleen ontvangt u terug een access_token uit de `/token` eindpunt.  Die access_token blijven geldig voor een korte periode (meestal één uur), maar verloopt uiteindelijk.  AT die tijdstip, uw app moet de gebruiker omleiden terug naar de `/authorize` eindpunt om op te halen van een nieuwe authorization_code.  Tijdens deze omleiding, de gebruiker kan of niet moet hun referenties opnieuw invoeren of mogelijk opnieuw toestemming voor machtigingen, afhankelijk van het het type app.

Voor meer informatie over het ophalen en gebruiken van tokens vernieuwen, verwijst naar de [v2.0 protocol verwijzing](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Toestemming voor individuele gebruiker wordt gevraagd

In een verzoek [OpenID verbinding maken of OAuth 2.0](active-directory-v2-protocols.md) autorisatie voor een app kunt ook zelf aanvragen de machtigingen die deze nodig met behulp van de `scope` queryparameter.  Wanneer een gebruiker zich in een app, zou de app bijvoorbeeld verzenden in een nieuw vergaderverzoek zoals de volgende (met regeleinden voor leesbaarheid):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

De `scope` parameter is een door spaties gescheiden lijst met bereiken die de app aanvraagt.  Elke afzonderlijke scope wordt aangegeven door de bereikwaarde aan van de resource-id (App-ID-URI) toe te voegen.  De bovenstaande aanvraag wordt aangegeven dat de app machtigingen moet voor agenda van de gebruiker e-mail leest en verzendt als de gebruiker.

Nadat de gebruiker hun referenties invoert, wordt het eindpunt v2.0 moeten worden gecontroleerd voor een overeenkomende record van de **gebruiker toestemming**.  Als de gebruiker niet heeft ingestemd met een van de gevraagde machtigingen in het verleden, vraagt het eindpunt v2.0 de gebruiker de gevraagde machtigingen verlenen.  

![Werk Account toestemming schermafbeelding](../media/active-directory-v2-flows/work_account_consent.png)

Wanneer de gebruiker de machtiging goedkeurt, worden de toestemming vastgelegd zodat de gebruiker niet moet opnieuw toestemming op latere aanmeldingen.

## <a name="requesting-consent-for-an-entire-tenant"></a>Toestemming voor een volledige tenant wordt gevraagd

Vaak wanneer een organisatie een licentie of een abonnement op een toepassing koopt, die ze willen volledig inrichten voor hun werknemers.  Als onderdeel van dit proces, kan de bedrijfsbeheerder van een toestemming voor de toepassing fungeert namens elke werknemer verlenen.  Werknemers van die organisatie ervaart door te verlenen toestemming voor een hele tenant, niet het scherm toestemming voor de toepassing.

Vraag toestemming voor alle gebruikers in een tenant door beschikking uw app over de **beheerder toestemming eindpunt**, hieronder beschreven.

## <a name="admin-restricted-scopes"></a>Bereiken administrator beperkt

Bepaalde hoog-bevoegdheid machtigingen in het Microsoft-systeem kunnen worden gemarkeerd als **administrator beperkt**.  Voorbeelden van dergelijke bereiken zijn:

- Lezen van een organizaion directory-gegevens:`Directory.Read`
- Het schrijven van gegevens naar de adreslijst van een organisatie:`Directory.ReadWrite`
- Beveiligingsgroepen lezen in de adreslijst van een organisatie:`Groups.Read.All`

Terwijl een gebruiker consumenten mogelijk een toepassing toegang tot dergelijke gegevens verlenen, organisatie-gebruikers zijn niet beschikbaar voor toegang verlenen tot dezelfde reeks gevoelige bedrijfsgegevens.  Als uw toepassing toegang naar een van deze machtigingen van een organisatie-gebruiker aanvraagt, moet u de gebruiker ontvangt een foutbericht met de mededeling dat zij onbevoegde toe te staan van uw app-machtigingen zijn.

Als uw app vereist is voor toegang tot deze bereiken administrator beperkt voor organisaties, moet u deze aanvragen rechtstreeks vanuit de bedrijfsbeheerder van een ook met de **beheerder toestemming eindpunt**, hieronder beschreven.

Wanneer u een beheerder deze machtigingen via het eindpunt van beheerder toestemming verleent, wordt toestemming verleend voor alle gebruikers in de tenant, zoals hierboven is beschreven.

## <a name="using-the-admin-consent-endpoint"></a>Gebruik van het eindpunt van de toestemming beheerder

Door deze stappen te volgen, kunnen uw app voor het verzamelen van machtigingen voor alle gebruikers in een bepaald tenant, inclusief administrator beperkt bereiken.  Als u wilt zien van een steekproef code die de onderstaande stappen-inventarisaties implementeert, raadpleegt u de [beheerder beperkte bereiken steekproef](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de portal van de registratie van de app aanvragen

- Ga in uw toepassing in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of [een app maken](active-directory-v2-app-registration.md) als u nog niet is gedaan.
- Zoek de sectie **Microsoft Graph machtigingen** en de machtigingen die moeten worden uw app toevoegen.
- Controleer of om op te **slaan** de registratie app

#### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: aan de gebruiker te melden bij uw app

Meestal bij het samenstellen van een toepassing die gebruikmaakt van de beheerder toestemming eindpunt, moeten de app hebben een/paginaweergave waarmee de beheerder om het goedkeuren van de app-machtigingen.  Deze pagina kunt deel uitmaken van aanmelding stroom van de app, deel uit van de instellingen van de app of een speciale 'koppelen' stroom.  In veel gevallen relevant dat is voor de app weer te geven 'koppelen' weergave alleen nadat een gebruiker heeft aangemeld met een werk of school Microsoft-account.

De gebruiker aanmelden bij de app, kunt u de organziation die de beheerder voordat het verzoek om goed te keuren voor de benodigde machtigingen hoort identificeren.  Terwijl niet strikt noodzakelijk is, kunt u een meer intuïtieve-ervaring maken voor uw organisatie-gebruikers.  Als u wilt melden de gebruiker in, volgt u onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

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
| tenant | Vereist | De directory-tenant die u wilt vraag machtiging uit.  Kan worden verstrekt in guid of de notatie voor de beschrijvende naam. |
| client_id | Vereist | De toepassings-Id of de registratie-portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uw app toegewezen. |
| redirect_uri | Vereist | De redirect_uri waar u het antwoord wilt sturen voor uw app worden afgehandeld.  Dit moet precies overeenkomen met een van de redirect_uris die u geregistreerd in de portal. |
| de staat | aanbevolen | Een waarde die is opgenomen in de aanvraag dat ook in het token antwoord wordt geretourneerd.  Een tekenreeks van inhoud die u wilt u kunt werken.  De staat wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek is opgetreden, bijvoorbeeld een pagina of weergave matrixgegevens op. |

In dit stadium wordt Azure AD afdwingen dat alleen de tenantbeheerder van een mij aanmelden kan het verzoek te voltooien.  De beheerder krijgt het verzoek goed te keuren voor alle machtigingen die u hebt aangevraagd voor de app in de registratie-portal. 

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

Zodra u een positief antwoord van het eindpunt van beheerder toestemming hebt ontvangen, heeft uw app de machtigingen die zij gevraagd ervaring.  U kunt nu verplaatsen naar het aanvragen van een token voor de gewenste resource, zoals hieronder beschreven.

## <a name="using-permissions"></a>Gebruik van machtigingen

Nadat de gebruiker akkoord met machtigingen voor de app gaat, kan uw app in access-tokens die van uw app-machtigingen voor toegang tot een bron in sommige capaciteit voorstellen aanschaffen.  Een bepaald toegangstoken kan alleen worden gebruikt voor een enkele resorce, maar elke machtiging die uw app voor de desbetreffende resource is toegekend codering hierin worden.  Als u wilt in het bezit van een toegangstoken, kunt uw app een aanvraag aanbrengen in het v2.0 token eindpunt:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

De resulterende toegangstoken vervolgens in de HTTP-aanvragen voor de resource kan worden gebruikt: deze betrouwbaar geeft aan aan de resource dat uw app beschikt over de juiste machtiging een bepaalde taak uit te voeren.  

Voor meer informatie over het OAuth 2.0-protocol en hoe u in het bezit van access tokens, raadpleegt u [v2.0 eindpunt protocol verwijzing](active-directory-v2-protocols.md).
