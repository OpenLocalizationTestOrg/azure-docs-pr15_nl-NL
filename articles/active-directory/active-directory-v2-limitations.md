<properties
    pageTitle="Beperkingen voor het eindpunt van v2.0 & beperkingen | Microsoft Azure"
    description="Een lijst met beperkingen & beperkingen aan het eindpunt van de v2.0 Azure AD."
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

# <a name="should-i-use-the-v20-endpoint"></a>Moet ik het eindpunt v2.0 gebruiken?

Als u toepassingen die zijn geïntegreerd met Azure Active Directory maakt, moet u bepalen of de v2.0 eindpunt en verificatie protocollen aan uw wensen voldoet.  Het oorspronkelijke Azure AD-eindpunt wordt nog steeds volledig ondersteund en in sommige respecteert, is meer uitgebreide functionaliteit dan v2.0.  Echter de v2.0 eindpunt [maakt u kennis met grote voordelen](active-directory-v2-compare.md) voor ontwikkelaars die u met het nieuwe programming objectmodel mogelijk lokken.

Wij raden bij gebruik van het eindpunt v2.0 is op dit moment, als volgt:

- Als u ter ondersteuning van Microsoft voor persoonlijke accounts in uw toepassing wilt, kunt u het eindpunt v2.0 moet gebruiken.  Maar zorgen voor meer informatie over de beperkingen die in dit artikel, met name de die betrekking hebben op specifiek als u wilt werken en schoolaccounts.
- Als uw toepassing alleen ondersteunende werk en schoolaccounts vereist, moet u [de oorspronkelijke Azure AD-eindpunten](active-directory-developers-guide.md).

Het eindpunt v2.0 zal na verloop van tijd toenemen tot het verwijderen van de beperkingen die hier worden vermeld, zodat alleen ooit moet u het eindpunt v2.0 gebruiken.  Ondertussen is in dit artikel bedoeld om te bepalen of het eindpunt v2.0 iets voor u.  We blijft in dit artikel na verloop van tijd, zodat de huidige status van het eindpunt v2.0 aansluiten, dus kijk terug om uw vereisten voor ten opzichte van de mogelijkheden v2.0 Herzie bijwerken.

Als u een bestaande app met Azure Active Directory die geen van het eindpunt v2.0 gebruikmaakt hebt, wordt niet nodig helemaal opnieuw beginnen.  In de toekomst zullen we leveren een manier om te schakelen van uw bestaande Azure AD-toepassingen voor gebruik met het eindpunt v2.0.

## <a name="restrictions-on-apps"></a>Beperkingen voor apps
De volgende soorten apps worden momenteel niet ondersteund door het eindpunt v2.0.  Voor een beschrijving van de ondersteunde soorten apps, raadpleegt u [in dit artikel](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Zelfstandige Web API 's
Met het eindpunt v2.0 hebt u de mogelijkheid om te [maken van een Web-API die is beveiligd met OAuth 2.0](active-directory-v2-flows.md#web-apis).  Echter is die Web API alleen mogelijk tokens ontvangt van een toepassing die u de dezelfde toepassings-id deelt.  Samenstellen van een web API die toegankelijk is met een client met een andere toepassing-Id wordt niet ondersteund.  De client die kunnen niet aan te vragen of machtigingen voor het web API verkrijgen.

Het maken van een Web-API waarin tokens afgeleid van een client met dezelfde App-Id, raadpleegt u de voorbeelden v2.0 eindpunt Web API in [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web API op-namens-van-mailstroom
Veel architecturen bevatten een Web-API die vereist zijn voor het bellen van een andere volgende Web API, beide beveiligd door het eindpunt v2.0.  Dit scenario geldt in systeemeigen clients die een backend Web API, die op zijn beurt oproepen een online-service van Microsoft of naar een andere aangepaste ingebouwd web API die ondersteuning biedt voor Azure AD hebben.

Dit scenario kan worden ondersteund met het OAuth 2.0 Jwt vruchtdragende referentie verlenen, ook bekend als de On-Behalf-Of Flow.  Echter is de stroom aan-namens-Of momenteel niet ondersteund voor het eindpunt v2.0.  Om te kijken hoe deze stroom werkt in het algemeen beschikbaar Azure AD service, raadpleegt u de [-op-namens-van-codevoorbeeld op GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Beperkingen voor de app registraties
Alle apps die wilt integreren met het eindpunt v2.0 moeten op dit moment, de registratie van een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Een bestaande Azure AD Microsoft-Account apps worden niet compatibel met het eindpunt v2.0 of worden geregistreerd in een portal naast de nieuwe App registratie Portal apps.  We willen zelf een formule bestaande toepassingen voor gebruik als een app v2.0 inschakelen. Op dit moment echter is er geen migratiepad voor een app naar het eindpunt v2.0.

Apps die zijn geregistreerd in de Portal voor registratie van nieuwe App werkt op dezelfde manier niet ten opzichte van het oorspronkelijke eindpunt van de Azure AD-verificatie.  U kunt echter apps die zijn gemaakt in de Portal van de registratie-App gebruiken om te integreren met succes met het eindpunt van de verificatie in de Microsoft-account, `https://login.live.com`.

Daarnaast bestaan app registraties gemaakt op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uit de volgende beperkingen:

- De **startpagina van** eigenschap, ook bekend als de **URL voor eenmalige aanmelding** wordt niet ondersteund.  Zonder een introductiepagina, wordt deze toepassingen niet worden weergegeven in het deelvenster aan de Office-Apps.
- Slechts twee app geheimen zijn toegestaan per toepassing Id op dit moment.
- Een app te registreren kan alleen worden bekeken en beheerd door een enkel ontwikkelaarsaccount.  Dit kan niet worden gedeeld door meerdere ontwikkelaars.
- Er zijn verschillende beperkingen van de indeling van redirect_uri toegestaan.  Zie de volgende sectie voor meer informatie.

## <a name="restrictions-on-redirect-uris"></a>Beperkingen voor omleiding URI 's
Apps die zijn geregistreerd in de nieuwe Portal voor de registratie van toepassing zijn op dit moment beperkt tot een beperkt aantal redirect_uri waarden.  De redirect_uri voor WebApps en services moeten beginnen met het schema `https`, en alle redirect_uri waarden moeten delen één DNS-domein.  Bijvoorbeeld, is het niet mogelijk om te registreren van een web-app redirect_uris heeft:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Het registratiesysteem verschilt van de hele DNS-naam van de bestaande redirect_uri met de DNS-naam van de redirect_uri die u wilt toevoegen. Het verzoek om toe te voegen van de DNS-naam loopt vast als een van de volgende voorwaarden wordt voldaan:  

- Als het hele DNS-naam van de nieuwe redirect_uri komt niet overeen met de DNS-naam van de bestaande redirect_uri
- Als het hele DNS-naam van de nieuwe redirect_uri niet een subdomein van de bestaande redirect_uri is

Bijvoorbeeld als de app redirect_uri heeft:

`https://login.contoso.com`

Is het mogelijk om toe te voegen:

`https://login.contoso.com/new`

dat komt exact overeen met de naam van de DNS- of:

`https://new.login.contoso.com`

Dit is een DNS-subdomein van login.contoso.com.  Als u wilt een app hebt die login-east.contoso.com heeft en login-west.contoso.com als redirect_uris, moet u moet de volgende redirect_uris toevoegen in volgorde:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

De laatste twee kunnen worden toegevoegd omdat deze subdomeinen van de eerste redirect_uri contoso.com. Deze beperking worden verwijderd in een nieuwe versie.

Als u wilt weten hoe u een app registreren in de nieuwe Portal voor de registratie van toepassing, raadpleegt u [in dit artikel](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Beperkingen voor de services en API 's
Het eindpunt v2.0 momenteel ondersteunt-aanmelding voor een geregistreerd in de nieuwe toepassing registratie-Portal voorwaarde dat deze app worden onderverdeeld in de lijst met [ondersteunde verificatietypen loopt](active-directory-v2-flows.md).  Echter is deze apps alleen mogelijk aan te schaffen OAuth 2.0 access tokens voor een beperkt aantal resources.  Het eindpunt v2.0 verleent slechts access_tokens voor:

- De app die het token gevraagd.  Een app kan een access_token aanschaffen voor zelf, als de logische app uit diverse verschillende onderdelen of lagen bestaat.  Als u wilt dit scenario in actie zien, raadpleegt u onze [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) -zelfstudies.
- De Outlook Mail, agenda en contactpersonen REST API's die allemaal zich op https://outlook.office.com bevinden.  Als u wilt weten hoe u een app die toegang heeft tot deze API's schrijven, raadpleegt u deze zelfstudies [Office aan de slag](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- De Microsoft Graph-API's.  Meer informatie over de Microsoft Graph en alle gegevens die beschikbaar is, gaat u naar [https://graph.microsoft.io](https://graph.microsoft.io).

Geen andere services worden ondersteund op dit moment.  Meer Microsoft Online services wordt, evenals ondersteuning voor uw eigen aangepaste-en-klare Web-API's en -services in de toekomst toegevoegd.

## <a name="restrictions-on-libraries--sdks"></a>Beperkingen voor bibliotheken & SDK 's
Er is op dit moment bibliotheek ondersteuning voor het eindpunt v2.0 vrij beperkt.  Als u het eindpunt v2.0 gebruiken in een productietoepassing wilt, hebt u de volgende opties:

- Als u een webtoepassing maakt, kunt u onze algemeen beschikbaar serverzijde middleware veilig gebruiken uit te voeren aanmelden token gegevensvalidatie.  Hierbij wordt de middleware OWIN openen-ID-verbinding voor ASP.NET en onze NodeJS Passport-invoegtoepassing.  Voorbeelden van de code met onze middleware zijn beschikbaar in onze ook sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .
- Voor andere platforms en voor systeemeigen en mobiele toepassingen, kunt u ook integreren met het eindpunt v2.0 door rechtstreeks verzenden en ontvangen van protocolberichten in uw toepassingscode.  De v2.0 OpenID verbinding en OAuth protocollen [expliciet is beschreven](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van deze een integratie.
- Tot slot kunt u bron openen-ID-verbinding openen en OAuth-bibliotheken kunt integreren met het eindpunt v2.0.  Het protocol v2.0 moet compatibel zijn met veel bron openen protocol bibliotheken zonder de belangrijkste wijzigingen.  De beschikbaarheid van dergelijke bibliotheken varieert per platform- en taalinstellingen en de [Geopende ID verbinding](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) -websites voor het behoud van een lijst met populaire implementaties. Zie [Azure Active Directory (AD) v2.0 en verificatie bibliotheken](active-directory-v2-libraries.md) voor meer informatie en de lijst met geopende bron clientbibliotheken en voorbeelden die met het eindpunt v2.0 zijn getest.

We hebben ook een eerste voorbeeld van de [Bibliotheek voor Microsoft-verificatie (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) voor uitgebracht .NET alleen.  U bent Welkom bij deze bibliotheek in .NET-clients en servers toepassingen uitproberen, maar als deze niet moet worden voorzien van GA hoogwaardig preview bibliotheek ondersteunen.

## <a name="restrictions-on-protocols"></a>Beperkingen voor protocollen
Het eindpunt v2.0 ondersteunt alleen openen ID verbinden & OAuth 2.0.  Echter niet alle functies en mogelijkheden van elke protocol zijn opgenomen in het eindpunt v2.0, waaronder:

- De verbinding van OpenID `end_session_endpoint`, waardoor een app tot het einde van de gebruiker-sessie met het eindpunt v2.0.
- id_tokens uitgegeven door het eindpunt v2.0 bevatten alleen een paarsgewijze id voor de gebruiker.  Dit betekent dat twee verschillende toepassingen verschillende id's voor dezelfde gebruiker ontvangt.  Merk op dat een query uitgevoerd op de Microsoft Graph `/me` eindpunt, is mogelijk om een correlatable-ID voor de gebruiker die kan worden gebruikt in toepassingen.
- id_tokens uitgegeven door het eindpunt v2.0 niet bevatten een `email` claimen voor de gebruiker op dit moment, zelfs als u in het bezit van toestemming van de gebruiker om hun e-mail te bekijken.
- Het eindpunt OpenID verbinding gebruikersgegevens. Het eindpunt gebruikersgegevens is niet geïmplementeerd op het eindpunt v2.0 op dit moment.  Alle profiel gebruikersgegevens zou ontvangt u mogelijk op dit eindpunt is echter verkrijgbaar via het Microsoft Graph `/me` eindpunt.
- Rol & groepclaims.  Op dit moment kan ondersteunt het eindpunt v2.0 geen uitgifte rol of groep claims in id_tokens.

Lees meer informatie over het bereik van protocol functionaliteit worden ondersteund in het eindpunt v2.0, tot en met onze [OpenID verbinden & OAuth 2.0 Protocol voor meer informatie](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Beperkingen voor school & werken-accounts
Er zijn een paar functies specifiek zijn voor Microsoft organisatie/zakelijke gebruikers die nog niet worden ondersteund door het eindpunt v2.0.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Voorwaardelijke toegang op basis van het apparaat, systeemeigen en mobiele apps en het Microsoft Graph
Het eindpunt v2.0 ondersteunt niet nog apparaat verificatie voor mobiele en systeemeigen toepassingen, zoals systeemeigen uitgevoerd op iOS of Android-apps.  Hiermee kan de bijbehorende toepassing uit het bellen van de Microsoft Graph voor bepaalde organisaties blokkeren.  Apparaat-verificatie is vereist als een beheerder Hiermee stelt u een apparaat gebaseerde voorwaardelijke-beleid van een toepassing.  Voor het eindpunt v2.0 is het meest waarschijnlijke geval voor voorwaardelijke toegang op basis van het apparaat een beheerder een beleid instellen op een resource in de Microsoft Graph, zoals de Outlook-API.  Als een beheerder Hiermee stelt u dit beleid en de bijbehorende toepassing een token naar de Microsoft Graph aanvraagt, mislukt het verzoek uiteindelijk omdat apparaat verificatie wordt nog niet ondersteund.  Webtoepassingen die tokens naar de Microsoft Graph, aanvragen echter worden ondersteund wanneer beleidsregels op basis van het apparaat zijn geconfigureerd.  In het web app scenario apparaat verificatie uitgevoerd via de webbrowser van de gebruiker.

Als een ontwikkelaar kan hebt u waarschijnlijk geen via wanneer beleidsregels zijn ingesteld op bronnen van Microsoft Graph, of zelfs weten wanneer dit gebeurt.  Als u een toepassing voor gebruikers van werk en schoolaccount maakt, moet u [het oorspronkelijke Azure AD-eindpunt](active-directory-developers-guide.md) totdat het eindpunt v2.0 apparaat verificatie ondersteunt is ingesteld.  Voor meer informatie over voorwaardelijke toegang op basis van het apparaat, raadpleegt u [in dit artikel](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Geïntegreerde Windows-verificatie voor federatieve tenants
Als u ADAL (en dus in het oorspronkelijke Azure AD-eindpunt) in Windows-toepassingen gebruikt hebt, is het mogelijk dat u profiteren van het verlenen van SAML bevestiging zogenaamde hebt genomen.  Deze verlenen kan gebruikers van federatieve Azure AD tenants stilte verificatie met hun on-premises Active Directory-exemplaar zonder hun referenties invoeren.  Het verlenen van SAML bevestiging is momenteel niet ondersteund op het eindpunt v2.0.
