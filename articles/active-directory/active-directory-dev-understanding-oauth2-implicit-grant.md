<properties
   pageTitle="Informatie over de impliciete OAuth2 verlenen stroom in Azure Active Directory | Microsoft Azure"
   description="Lees meer over Azure Active Directory-implementatie van de impliciete OAuth2 verlenen stroom, en of het juiste voor uw toepassing is."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Informatie over de OAuth2 impliciete verlenen stroom in Azure AD (Active Directory)

De OAuth2 impliciete verlenen is voor het verlenen met de langste lijst met beveiligingsoverwegingen in de specificatie OAuth2 worden algemeen. En klik nog dat de methode geïmplementeerd door ADAL JS en de presentatie waarin die wordt aangeraden bij het schrijven van beveiligd-WACHTWOORDVERIFICATIE toepassingen is. Wat te bieden heeft? Het is een kwestie van compromissen nodig zijn: en omdat het blijkt, de impliciete verlenen tot stand is de beste manier die u kunt oefenen voor toepassingen die een Web API via JavaScript in een browser gebruiken.

## <a name="what-is-the-oauth2-implicit-grant"></a>Wat is de OAuth2 impliciete verlenen?

De ultieme [OAuth2 autorisatiecode verlenen](https://tools.ietf.org/html/rfc6749#section-1.3.1) , is de autorisatie verlenen waarin twee afzonderlijke eindpunten. Het eindpunt autorisatie wordt gebruikt voor de gebruiker interactie fase, waardoor een autorisatiecode. Het token eindpunt is wordt gebruikt door de client voor de uitwisseling van de code voor een toegangstoken en vaak ook een token voor vernieuwen. Webtoepassingen zijn vereist om hun eigen toepassing referenties naar het eindpunt toegangstoken, zodat de server autorisatie voor de client kan verifiëren.

De [OAuth2 impliciete verlenen](https://tools.ietf.org/html/rfc6749#section-1.3.2) , is er een variant die een client voor informatie over een toegangstoken (en id_token, in het geval van [OpenId verbinding](http://openid.net/specs/openid-connect-core-1_0.html)) zodat rechtstreeks vanuit het eindpunt autorisatie, zonder contact opneemt met het token eindpunt en ook niet de clienttoepassing verifiëren. Deze variant is speciaal ontworpen voor JavaScript gebaseerde toepassingen die worden uitgevoerd in een webbrowser: in de oorspronkelijke OAuth2 specification, tokens in een fragment URI worden geretourneerd. Die ervoor zorgt dat de token bits beschikbaar voor de JavaScript-code in de client, maar deze garandeert dat ze worden niet opgenomen in omleidingen naar de server. Tokens retourneren via de browser wordt omgeleid rechtstreeks vanuit het eindpunt autorisatie. Er wordt ook het voordeel van het verwijderen van een vereisten voor cross origin oproepen die nodig zijn als de JavaScript-toepassing is vereist voor het contact opnemen met het token eindpunt zijn.

Een belangrijk kenmerk van de OAuth2 impliciete verlenen is het feit dat deze nooit afzender vernieuwen tokens naar de klant doorloopt. Zoals we in het volgende gedeelte ziet, die niet echt nodig en is in feite een beveiligingsprobleem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geschikt voor scenario's voor de OAuth2 impliciete verlenen

Als de OAuth2 specification zelf gedeclareerd, heeft het impliciete verlenen om in te schakelen gebruikersagent toepassingen – dat wil zeggen, JavaScript-toepassingen uitgevoerd in een browser is ontwikkeld. De definiëren kenmerk van dergelijke toepassingen is dat JavaScript-code wordt gebruikt voor toegang tot serverbronnen (meestal een Web-API) en voor het bijwerken van de toepassing UX dienovereenkomstig gewijzigd. Bedenk toepassingen zoals Gmail of Outlook Web Access: wanneer u een bericht uit uw Postvak selecteert, wordt alleen het bericht visualisatie deelvenster gewijzigd om weer te geven van de nieuwe selectie, terwijl de rest van de pagina ongewijzigd blijft. Dit is anders dan bij traditionele redirect gebaseerde WebApps, waarbij elke gebruikersinteractie resulteert in een terugposten volledige pagina en een volledige pagina-weergave van de nieuwe serverreactie.

Toepassingen die de methode JavaScript gebaseerd duren voordat de extreme heten toepassingen van één pagina of kuuroorden: het idee is dat deze toepassingen alleen maar een eerste HTML-pagina en de bijbehorende JavaScript, met alle opeenvolgende interacties wordt aangestuurd door Web API-oproepen via JavaScript uitgevoerd. Echter hybride benaderingen, waar de toepassing is voornamelijk terugposten op basis van hoeveelheid werk, maar kunt u incidentele JS-oproepen uitvoeren, zijn niet ongebruikelijk – de discussie die over het gebruik van de impliciete stroom ook relevant zijn voor gebruikers met computers is.

Redirect-toepassingen secure meestal hun aanvragen via cookies echter die methode niet als u ook voor JavaScript-toepassingen werkt. Cookies werken alleen voor het domein dat ze zijn gegenereerd voor, terwijl JavaScript aanroepen mogelijk worden doorgestuurd naar andere domeinen. Ja, die vaak worden de hoofdletters/kleine letters: vergelijken met toepassingen aanroepen van Microsoft Graph API, Office-API, Azure API – alle die zich buiten het domein van waar de toepassing wordt bediend. Een groeiende trend voor JavaScript-toepassingen is dat er geen backend helemaal te vertrouwen 3e partijen Web-API's willen implementeren van hun bedrijfsfunctie 100%.

De voorkeursmethode voor het beveiligen van oproepen naar een Web-API is momenteel gebruik van de OAuth2 vruchtdragende token benadering, waarbij elk gesprek wordt geleverd met een toegangstoken OAuth2. De Web-API wordt het binnenkomende toegangstoken en, als er in deze de benodigde bereiken, deze toegang verleent tot de bewerking. De impliciete stroom biedt een handige om JavaScript-toepassingen voor access tokens voor een Web-API, bieden veel voordelen ten opzichte van cookies:

- Tokens kunnen betrouwbaar worden verkregen zonder dat de hoeven cross origin oproepen – verplicht registratie van de omleiding URI waaraan tokens afzender zijn garandeert geen verplaatsing van tokens
- JavaScript-toepassingen kunnen net zoveel access tokens terwijl ze nodig hebben en voor zo veel Web API's ze doelgerichte – met geen beperking voor domeinen aanvragen
- HTML5-functies zoals sessie of lokale opslag verlenen volledige controle over token caching en beheer van de levensduur dat cookies management ondoorzichtig naar de app
- Access tokens worden niet vatbaar voor meerdere sites verzoek voorkoming (CSRF) aanvallen

Vernieuwen tokens, voornamelijk om beveiligingsredenen wordt niet afgegeven door de stroom impliciete verlenen. Een token vernieuwen wordt niet als nauwkeuriger beperkt als tokens van access, toekennen van de uiterst meer power dus uiterst meer schade toegebracht geval deze is meer informatie. Klik in de impliciete stroom, tokens worden bezorgd in de URL, de kans op pesterijen onderschept is daarom hoger zijn dan in het verlenen van de code autorisatie.

Bedenk wel dat een JavaScript-toepassing om een andere beschikt voor het vernieuwen van access tokens zonder herhaaldelijk prompt om referenties. De toepassing kunt een verborgen iframe uitvoeren nieuwe token aanvragen ten opzichte van het eindpunt autorisatie van Azure AD: zolang de browser nog steeds een actieve sessie heeft (lezen: een sessiecookie heeft) ten opzichte van de Azure AD-domein, de verificatieaanvraag voor het succes hoeven de interactie van de gebruiker kan optreden. 

Dit model Hiermee de JavaScript-toepassing kunnen onafhankelijk verlengen access tokens en zelfs in het bezit van nieuwe bestanden voor een nieuwe API (mits de gebruiker ingestemd eerder voor hen. Hiermee voorkomt u de toegevoegde last van bij het verkrijgen van, beheren en beveiligen van een hoge waarde-onderdeel, zoals een token vernieuwen. Het onderdeel waardoor de stille verlenging mogelijk, de sessie Azure AD cookie worden buiten de toepassing beheerd. Een ander voordeel van deze methode is dat een gebruiker kan zich afmeldt van Azure AD, een van de toepassingen die zijn aangemeld bij Azure AD, uitgevoerd op een van de tabs van de browser. Het resultaat is het verwijderen van de Azure AD-sessiecookie en de JavaScript-toepassing verliest automatisch de mogelijkheid om te verlengen tokens voor de gebruiker ondertekend af.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Is de impliciete verlenen geschikt voor mijn app?

De impliciete verlenen biedt meer risico's dan de andere verleent. De gebieden die u moet aandacht besteden aan worden ook beschreven (Zie bijvoorbeeld [Misbruik van Access Token naar Resource eigenaar nabootsen in impliciete Flow] [ OAuth2-Spec-Implicit-Misuse] en [OAuth 2.0 bedreiging Model en beveiligingsoverwegingen][OAuth2-Threat-Model-And-Security-Implications]). Het hogere risicoprofiel is echter grotendeels vanwege het feit dat is bedoeld als het mogelijk dat toepassingen die actieve code, die door een externe bron naar een browser wordt bediend uitvoeren. Als u van plan bent een beveiligd-WACHTWOORDVERIFICATIE architectuur, hebt u geen back-end-onderdelen of plan bent om aan te roepen een Web API via JavaScript, gebruik van de impliciete stroom voor token aanschaf wordt aanbevolen.

Als uw toepassing een native client is, is een uitstekende aanpassen door de impliciete stroom niet. Het ontbreken van de Azure AD-sessiecookie in de context van een native client ontneemt uw toepassing van de gemiddelden van een lang worden bewaard sessie onderhouden. Wat betekent dat uw toepassing vraagt net zo vaak de gebruiker bij het verkrijgen van access tokens voor nieuwe resources.

Als u een webtoepassing die bestaat uit een back-end en gebruik een API van de backend-code ontwikkelt, is de impliciete stroom ook niet thuishoort. Andere verleent stellen u veel meer. Het verlenen van OAuth2 client referenties bevat bijvoorbeeld de mogelijkheid om te verkrijgen tokens die overeenkomen met de machtigingen die zijn toegewezen aan de toepassing zelf, in plaats van de gebruiker delegaties. Dit betekent dat de client heeft de mogelijkheid om te onderhouden toegang tot het resources even wanneer een gebruiker niet actief in een sessie, enzovoort samen is. Niet alleen die, maar deze verleent geven hoger garanties op beveiliging. Bijvoorbeeld de browser van de gebruiker toegangstokens nooit doorvoer, ze niet worden opgeslagen in de browsegeschiedenis risico, enzovoort. De clienttoepassing kan ook sterke verificatie uitvoeren bij het aanvragen van een token.

## <a name="next-steps"></a>Volgende stappen

- Voor een volledige lijst met bronnen voor ontwikkelaars, met inbegrip van informatie over de protocollen en OAuth2 autorisatie loopt ondersteuning door Azure AD verlenen, verwijst naar de [Azure AD Developer's Guide][AAD-Developers-Guide]
- Lees [hoe u het integreren van een toepassing met Azure AD]  [ ACOM-How-To-Integrate] voor aanvullende diepte op het proces van de integratie van toepassing.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

