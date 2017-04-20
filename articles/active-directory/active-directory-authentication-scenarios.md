
<properties
   pageTitle="Verificatie-scenario's voor Azure AD | Microsoft Azure"
   description="Een overzicht van de vijf meest voorkomende verificatie scenario's voor Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Verificatie-scenario's voor Azure AD

Azure Active Directory (Azure AD) verificatie voor ontwikkelaars vereenvoudigen doordat identiteit als de bron van een service, met ondersteuning voor standaard-protocollen zoals OAuth 2.0 en verbindt u OpenID, evenals openen bibliotheken voor andere platforms om te helpen u kleurcodering aan Snel starten. In dit document, kunt u meer informatie over de verschillende scenario's Azure AD-ondersteunt en ziet u hoe u aan de slag. Deze in de volgende secties verdeeld:

- [Basisbeginselen van verificatie in Azure AD](#basics-of-authentication-in-azure-ad)

- [Op claims in Azure AD-beveiligingstokens](#claims-in-azure-ad-security-tokens)

- [Basisbeginselen van het registreren van een toepassing in Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Toepassingstypen en scenario 's](#application-types-and-scenarios)

  - [Webbrowser aan webtoepassing.](#web-browser-to-web-application)

  - [Eén pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE)](#single-page-application-spa)

  - [De toepassing waarmee Web API](#native-application-to-web-api)

  - [Webtoepassing Web API](#web-application-to-web-api)

  - [Daemon of servertoepassing Web API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Basisbeginselen van verificatie in Azure AD

Als u niet bekend met basisbegrippen van verificatie in Azure AD bent, leest u in deze sectie. U wilt mogelijk anders overslaan omlaag naar [typen toepassingen en -scenario's](#application-types-and-scenarios).

Laten we eens het meest eenvoudige scenario waar identiteit vereist is: een gebruiker in een webbrowser moet worden geverifieerd aan een webtoepassing. Dit scenario wordt beschreven in de sectie [Webbrowser naar webtoepassing](#web-browser-to-web-application) uitgebreider, maar het is een handige beginpunt om te illustreren de mogelijkheden van Azure AD en de werking van de scenario concept. Houd rekening met het volgende diagram voor dit scenario:

![Overzicht van eenmalige aanmelding aan webtoepassing.](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Met het diagram hierboven in gedachten, moet u dit is wat u moet weten over de verschillende onderdelen:

- Azure AD is de id-provider, die verantwoordelijk is voor de identiteit van gebruikers en toepassingen die in de adreslijst van een organisatie aanwezig en uiteindelijk verlenen beveiligingstokens na een geslaagde verificatie van die gebruikers en toepassingen.


- Een toepassing die wil uitbesteden verificatie Azure AD moet worden geregistreerd in Azure AD, waarin wordt geregistreerd en deze identificeert op de app in de adreslijst.


- Ontwikkelaars kunnen de bron openen Azure AD-verificatie-bibliotheken gebruiken om verificatie eenvoudig door het afhandelen van de gegevens van het protocol voor u. Zie [Azure Active Directory verificatie bibliotheken](active-directory-authentication-libraries.md) voor meer informatie.


• Wanneer een gebruiker is geverifieerd, moet de toepassing van de gebruiker beveiligingstoken om ervoor te zorgen dat de verificatie is geslaagd voor de beoogde partijen valideren. Ontwikkelaars kunnen de meegeleverde verificatie-bibliotheken worden afgehandeld validatie van een token van Azure AD, inclusief JSON Web Tokens (JWT) of SAML 2.0 gebruiken. Als u validatie handmatig uitvoeren wilt, raadpleegt u de documentatie [JWT Token-Handler](https://msdn.microsoft.com/library/dn205065.aspx) .


> [AZURE.IMPORTANT] Azure AD gebruikt openbare sleutel cryptografische af wilt melden tokens en controleert u of ze geldig zijn. Zie [Belangrijke informatie over ondertekening toets Rollover in Azure AD](active-directory-signing-key-rollover.md) voor meer informatie over de benodigde logica moet in uw toepassing om ervoor te zorgen dat deze altijd wordt bijgewerkt met de meest recente toetsen.


• De stroom van vergaderverzoeken en antwoorden voor het verificatieproces wordt bepaald door de verificatieprotocol dat is gebruikt, zoals OAuth 2.0, OpenID verbinding kunt maken, WS-Federation of SAML 2.0. Deze protocollen worden in het onderwerp van de [Azure Active Directory verificatieprotocollen](active-directory-authentication-protocols.md) en in de secties hierna uitgebreider besproken.

> [AZURE.NOTE] Azure AD ondersteunt het OAuth-2.0 en OpenID verbinding standaarden uitgebreid waardoor van vruchtdragende tokens, inclusief vruchtdragende tokens weergegeven als JWTs gebruiken. Een dragertoken is een lightweight beveiligingstoken die de "vruchtdragende" toegang tot een beveiligde bron verleent. In deze zin is de "toonder" een partij die het token kunt presenteren. Hoewel een partij moet eerst worden geverifieerd bij Azure AD voor het ontvangen van de dragertoken, als de vereiste stappen zijn niet die u hebt gemaakt voor het beveiligen van de token in overdracht en opslag, kunt u er onderschept en die wordt gebruikt door een onbedoelde feest. Sommige beveiligingstokens beschikken over een ingebouwde methode voor het voorkomen dat onbevoegden ze te gebruiken, wordt aan toonder tokens geen deze methode hebt en moeten worden overgebracht in een beveiligd kanaal zoals transport laag beveiliging (HTTPS). Als een dragertoken wordt verzonden in het wissen, kan een man in de middelste aanval worden gebruikt door een schadelijke partij bij het aanschaffen van het token en deze gebruiken voor een onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer wilt opslaan of caching van vruchtdragende tokens voor later gebruik. Altijd Zorg ervoor dat uw toepassing verzendt en vruchtdragende tokens op een veilige manier opgeslagen. Zie voor meer beveiligingsoverwegingen op vruchtdragende tokens, [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).


Lees de secties hierna voor meer informatie over hoe in Azure AD inrichting werkt en de gebruikelijke scenario's Azure AD ondersteunt nu dat u een overzicht van de basisbeginselen hebt.


## <a name="claims-in-azure-ad-security-tokens"></a>Op claims in Azure AD-beveiligingstokens

Beveiligingstokens uitgegeven door Azure AD bevatten claims of bevestigingen van informatie over het onderwerp dat is geverifieerd. Deze claims kunnen worden gebruikt door de toepassing voor verschillende taken. Ze kunnen bijvoorbeeld kunnen worden gebruikt voor het valideren van het token, van het onderwerp directory tenant identificeren, gebruikersgegevens weergeven, bepalen van het onderwerp autorisatie, enzovoort. Het presenteren in een bepaald beveiligingstoken claims zijn afhankelijk van het type token, het type referentie gebruikt om te verifiëren van de gebruiker en configuratie van de toepassing. Een korte beschrijving van elk type claimen dat door Azure AD is opgegeven in de onderstaande tabel. Raadpleeg [Token ondersteund en claimtypen](active-directory-token-and-claims.md)voor meer informatie.


| Claim | Beschrijving |
|-------|-------------|
| Toepassings-ID | Geeft de toepassing waarin het token wordt gebruikt.
| Publiek | De geadresseerden resource die het token is bedoeld voor aangeeft. |
| Toepassing verificatie Context Class verwijzing | Geeft aan hoe de client is geverifieerde (openbare client versus vertrouwelijke client). |
| Verificatie chatberichten | Records van de datum en tijd waarop de verificatie is opgetreden. |
| Verificatiemethode | Geeft aan hoe het onderwerp van het token is geverifieerd (wachtwoord, certificaat, enzovoort). |
| Voornaam | Biedt de opgegeven naam van de gebruiker als set in Azure AD. |
| Groepen | Object-id's van Azure AD groepen die de gebruiker een lid van is bevat. |
| Identiteitsprovider | Records van de identiteitsprovider die het onderwerp van het token geverifieerd. |
| Uitgegeven aan | De tijd waarop het token is uitgegeven, vaak gebruikt voor token fris records. |
| Uitgever | Geeft de STS die afkomstig van de token, evenals de Azure AD-tenant. |
| Achternaam | Biedt de achternaam van de gebruiker als set in Azure AD. |
| Naam | Biedt een menselijke leesbare waarde die aangeeft van het onderwerp van het token. |
| Object-Id | Bevat een onveranderlijke, unieke id van het onderwerp in Azure AD. |
| Rollen | Bevat beschrijvende namen van Azure AD-toepassingsrollen die de gebruiker is verleend. |
| Bereik | Geeft de machtigingen met de clienttoepassing. |
| Onderwerp | Geeft de hoofdsom waarover het token informatie bevestigingen. |
| Tenant-Id | Bevat een onveranderlijke, unieke id van de tenant directory dat het token uitgegeven. |
| Levensduur van tokens | Hiermee definieert u het tijdsinterval waarbinnen een token geldig is. |
| User Principal Name | Bevat de UPN van het onderwerp. |
| Versie | Het versienummer van het token bevat. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Basisbeginselen van het registreren van een toepassing in Azure AD

Een toepassing die Azure AD-verificatie heeft moet worden geregistreerd in een map. Deze stap heeft betrekking op Azure AD vertellen over uw toepassing, inclusief de URL die waar is gevonden, de URL voor het verzenden van antwoorden na verificatie de URI voor uw toepassing en meer. Deze informatie is vereist voor enkele belangrijke redenen:

- Azure AD moet coördinaten om te communiceren met de toepassing bij het verwerken van eenmalige aanmelding of uitwisselen tokens. Dit omvat het volgende:

  - URI van toepassing-ID: De id van een toepassing. Deze waarde wordt verzonden naar Azure AD tijdens de verificatie om aan te geven welke toepassing de beller wil een token voor. Bovendien is deze waarde in het token opgenomen, zodat de toepassing weet dat deze was het beoogde doel.


  - URL en URI omleiden beantwoorden: bij een web API of webtoepassing, is de URL antwoord de locatie waarnaar Azure AD de verificatieantwoord verzendt, inclusief een token als verificatie gelukt is. Bij een systeemeigen-toepassing is de URI omleiden een unieke id waaraan Azure AD user-agent in een verzoek OAuth 2.0 leiden.


  - Client-ID: De ID voor een toepassing wordt gegenereerd door Azure AD wanneer de toepassing is geregistreerd. Bij het aanvragen van een vergunning code of token, worden de client-ID en de toets verzonden naar Azure AD tijdens de verificatie.


  - Sleutel: De sleutel die naar wordt verzonden samen met een client-ID bij het verifiëren van Azure AD om te bellen van een web API.


- Azure AD moet ervoor zorgen dat de toepassing heeft de vereiste machtigingen voor toegang tot de gegevens van uw adreslijst, andere toepassingen in uw organisatie, enzovoort

Inrichting wordt duidelijkere wanneer u weet dat er zijn twee categorieën van toepassingen die kunnen worden ontwikkeld en geïntegreerd met Azure AD:

- Eén tenant-toepassing: een toepassing voor één tenant is bedoeld voor gebruik in een organisatie. Dit zijn meestal line-of-business (LoB)-toepassingen geschreven door een ontwikkelaar enterprise. Een toepassing voor één tenant alleen moet worden gebruikt door gebruikers in een bepaalde map, en daardoor deze alleen moet worden deze is ingericht in één map. Deze toepassingen zijn meestal door een ontwikkelaar in de organisatie geregistreerd.


- Meerdere tenant-toepassing: een toepassing voor meerdere tenant is bedoeld voor gebruik in veel organisaties niet slechts één organisatie. Dit zijn meestal software-als-een-service (SaaS) toepassingen geschreven door een onafhankelijke softwareleverancier (ISV). Meerdere tenant toepassingen moeten worden deze is ingericht in elke map waar ze worden gebruikt, waarvoor de gebruiker of beheerder toestemming om u te registreren. Dit proces toestemming begint wanneer een toepassing in de map is geregistreerd en toegang tot de API grafiek of een andere web API is verleend. Wanneer een gebruiker of beheerder van een andere organisatie zich registreert voor het gebruik van de toepassing, wordt deze worden weergegeven met een dialoogvenster dat wordt weergegeven welke machtigingen van die de toepassing is vereist. De gebruiker of beheerder kan vervolgens met de toepassing, geeft de toepassing toegang tot de vermelde gegevens, en ten slotte de toepassing registreert in hun directory toestemming. Zie [overzicht van het kader toestemming](active-directory-integrating-applications.md#overview-of-the-consent-framework)voor meer informatie.

Aantal aanvullende overwegingen zich voordoen bij het ontwikkelen van een toepassing voor meerdere tenant in plaats van een toepassing voor één tenant. Bijvoorbeeld als u uw toepassing beschikbaar voor gebruikers in meerdere mappen maken bent, moet u een om te bepalen welke tenant die zich bevinden. Een enkel tenant-toepassing moet alleen om te zoeken in een eigen map voor een gebruiker, terwijl een toepassing meerdere tenant moet een specifieke gebruiker van alle mappen in Azure AD identificeren. Als u wilt uitvoeren van deze taak, biedt Azure AD een algemene verificatie-eindpunt waar een toepassing meerdere tenant aanmeldingsaanvragen, in plaats van een tenant-specifieke-eindpunt kunt verwijzen. Dit eindpunt is https://login.microsoftonline.com/common voor alle mappen in Azure AD, dat een tenant-specifieke-eindpunt mogelijk https://login.microsoftonline.com/contoso.onmicrosoft.com. Het algemene eindpunt is vooral belangrijk u rekening moet houden bij het ontwikkelen van uw toepassing, omdat u de benodigde logica u omgaat met meerdere tenants tijdens het aanmelden, afmelden en token validatie nodig hebt.

Als u momenteel een toepassing één tenant ontwikkelt, maar u wilt deze beschikbaar maken voor veel organisaties, kunt u eenvoudig wijzigingen aanbrengen aan de toepassing en de configuratie in Azure AD zodat u meerdere tenant staat. Daarnaast Azure AD dezelfde ondertekend sleutel gebruikt voor alle tokens in alle mappen of verificatie in een enkel tenant of meerdere tenant-toepassing wanneer u opgenomen.

Elk scenario die in dit document bevat een subsectie waarmee de inrichten vereisten wordt beschreven. Zie voor meer gedetailleerde informatie over het inrichten van een toepassing in Azure AD en de verschillen tussen enkele of meerdere tenant toepassingen, [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md) voor meer informatie. Doorgaan met lezen begrip van de algemene toepassing-scenario's in Azure AD.

## <a name="application-types-and-scenarios"></a>Toepassingstypen en scenario 's

De scenario's beschreven in dit document kan worden ontwikkeld met verschillende talen en platforms. Ze worden alle ondersteund door de voorbeelden van de volledige code die beschikbaar in onze [codevoorbeelden handleiding](active-directory-code-samples.md)of rechtstreeks vanuit de bijbehorende [Github steekproef opslagplaatsen zijn](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Bovendien als uw toepassing een specifiek stuk of segment van een end-to-end-scenario moet, kan in de meeste gevallen die functionaliteit worden toegevoegd onafhankelijk. Als u een bijbehorende toepassing die een web API-oproepen hebt, kunt u bijvoorbeeld eenvoudig een webtoepassing die ook het web API-oproepen toevoegen. In het volgende diagram ziet u deze scenario's en de toepassingstypen, en hoe verschillende onderdelen kunnen worden toegevoegd:

![Toepassingstypen en scenario 's](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Dit zijn de vijf primaire toepassing-scenario's die worden ondersteund door Azure AD:

- [Webbrowser naar webtoepassing](#web-browser-to-web-application): een gebruiker moet aanmelden bij een webtoepassing die is beveiligd met Azure AD.

- [Eén pagina toepassing WACHTWOORDVERIFICATIE](#single-page-application-spa): een gebruiker moet aanmelden bij een bepaalde pagina-toepassing die is beveiligd met Azure AD.

- [De toepassing waarmee Web API](#native-application-to-web-api): een systeemeigen toepassing die compatibel is met een telefoon, tablet of PC moet een gebruiker als u resources uit een web API die is beveiligd met Azure AD te verifiëren.

- [Webtoepassing Web API](#web-application-to-web-api): een webtoepassing moet resources uit een web API die zijn beveiligd met Azure AD verkrijgen.

- [Daemon of servertoepassing Web API](#daemon-or-server-application-to-web-api): een demontoepassing of een servertoepassing zonder gebruikersinterface web moet resources ophalen uit een web API die zijn beveiligd met Azure AD.

### <a name="web-browser-to-web-application"></a>Webbrowser aan webtoepassing.

Dit onderwerp vindt een toepassing die wordt geverifieerd door een gebruiker in een webbrowser aan een webtoepassing. In dit scenario de webtoepassing wordt u omgeleid zodat de browser van de gebruiker ze aanmelden bij Azure AD. Azure AD geeft als resultaat een reactie aanmelden via de browser van de gebruiker, waarin claims over de gebruiker in een beveiligingstoken. Dit scenario ondersteunt aanmelding met behulp van de protocollen WS-Federatie, SAML 2.0 en OpenID verbinding maken.


#### <a name="diagram"></a>Diagram
![Verificatie-mailstroom voor browser aan webtoepassing.](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Beschrijving van Protocol-mailstroom


1. Wanneer een gebruiker bezoekt de toepassing en de behoeften aan te melden, worden ze omgeleid via een aanvraag aanmelden om het eindpunt verificatie in Azure AD.


2. De gebruiker zich aanmeldt op de pagina aanmelden.


3. Als verificatie gelukt is, wordt Azure AD Hiermee maakt u een verificatietoken en geeft als resultaat een antwoord aanmelden op de URL van een van de toepassing antwoord dat is geconfigureerd in de beheerportal Azure. Voor een productietoepassing moet deze URL antwoord HTTPS. Het geretourneerde token bevat claims over de gebruiker en Azure AD die is vereist door de toepassing voor het valideren van het token.


4. De toepassing is gevalideerd met het token met behulp van een openbare ondertekend sleutel en informatie over de uitgever beschikbaar in het document Federatie-metagegevens voor Azure AD. Wanneer de toepassing is gevalideerd met het token, begint Azure AD een nieuwe sessie met de gebruiker. Deze sessie kan de gebruiker toegang tot de toepassing totdat het verloopt.


#### <a name="code-samples"></a>Voorbeelden van programmacode


Zie de voorbeelden van de code voor webbrowser webtoepassing scenario's. En Kijk hier regelmatig--we nieuwe voorbeelden altijd toevoegen. [Webbrowser naar de webtoepassing](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registreren


- Eén Tenant: Als u een toepassing alleen betrekking heeft op uw organisatie, deze moet worden geregistreerd in de adreslijst van uw bedrijf met behulp van de Azure-beheerportal.


- Meerdere Tenant: Als u een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie maakt, deze moet worden geregistreerd in de adreslijst van uw bedrijf, maar ook moet worden geregistreerd in de adreslijst van elke organisatie die van de toepassing gebruikmaakt. Als u de toepassing beschikbaar in de adreslijst, kunt u een aanmeldingsproces opnemen voor uw klanten waarmee ze toe te staan in uw toepassing. Wanneer ze zich aanmeldt voor uw toepassing, ze krijgt een dialoogvenster dat wordt weergegeven welke machtigingen van die de toepassing is vereist, waarna de optie toe te staan. Afhankelijk van de vereiste machtigingen, een beheerder in de andere organisatie mogelijk moet toestemming geven. Wanneer de gebruiker of beheerder akkoord gaat, wordt de toepassing is geregistreerd in hun directory. Zie voor meer informatie [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Verlopen van token

Sessie van de gebruiker verloopt als de levensduur van het token uitgegeven door Azure AD verloopt. Uw toepassing kunt deze periode indien gewenst, zoals gebruikers op basis van een periode van inactiviteit afmelden inkorten. Als de sessie verloopt, wordt de gebruiker gevraagd opnieuw aanmelden.





### <a name="single-page-application-spa"></a>Eén pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE)

Dit onderwerp vindt verificatie voor één pagina toepassing, die wordt gebruikt Azure AD en het OAuth 2.0 impliciete autorisatie verlenen als u wilt beveiligen van het web API terug beëindigen. Eén pagina-toepassingen zijn meestal gestructureerd als een JavaScript presentatie laag (front-end) die wordt uitgevoerd in de browser en Web API back-enddatabase die wordt uitgevoerd op een server en implementeert bedrijfslogica van de toepassing. Zie [Wat is de stroom OAuth2 impliciete verlenen in Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md)meer informatie over de impliciete autorisatie verlenen en help bepaalt u of het juiste voor uw scenario toepassing is.

In dit scenario wanneer de gebruiker zich aanmeldt, de JavaScript voren einde gebruik [Active Directory-verificatie Library voor JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) en de impliciete toestemming verlenen voor een ID-token (id_token) van Azure AD. Het token is opgeslagen in de cache en de client gekoppeld aan het verzoek als de dragertoken bij gesprekken naar de Web-API back-end, die is beveiligd met de middleware OWIN. 
#### <a name="diagram"></a>Diagram

![Eén pagina toepassing-diagram](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van Protocol-mailstroom

1. De gebruiker navigeert naar de webtoepassing.


2. De toepassing geeft als resultaat de front-end van de JavaScript (presentatie layer) naar de browser.


3. De gebruiker start aanmelden, bijvoorbeeld door te klikken op een aanmelden koppeling. De browser stuurt een GET naar het eindpunt van de Azure AD autorisatie voor een token ID aanvragen. Deze aanvraag bevat de URL van de client-ID en beantwoorden in de queryparameters.


4. Azure AD is gevalideerd met de URL antwoord ten opzichte van de geregistreerde antwoord-URL die is geconfigureerd in de beheerportal Azure.


5. De gebruiker zich aanmeldt op de pagina aanmelden.


6. Als verificatie gelukt is, wordt Azure AD Hiermee maakt u een token ID en geretourneerd als een URL-fragment (#) naar de URL van de reactie van de toepassing. Voor een productietoepassing moet deze URL antwoord HTTPS. Het geretourneerde token bevat claims over de gebruiker en Azure AD die is vereist door de toepassing voor het valideren van het token.


7. De JavaScript-client-code die wordt uitgevoerd in de browser haalt de token uit het antwoord wilt gebruiken in het beveiligen van oproepen naar van de toepassing web dat API terug beëindigen.


8. De browser oproepen van de toepassing web API terug eindigt op het toegangstoken in de koptekst autorisatie.

#### <a name="code-samples"></a>Voorbeelden van programmacode


Zie de voorbeelden van de code voor één pagina toepassing WACHTWOORDVERIFICATIE scenario's. Zorg ervoor dat u Kijk hier regelmatig--we nieuwe voorbeelden altijd toevoegen. [Eén pagina toepassing (beveiligd-WACHTWOORDVERIFICATIE)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registreren


- Eén Tenant: Als u een toepassing alleen betrekking heeft op uw organisatie, deze moet worden geregistreerd in de adreslijst van uw bedrijf met behulp van de Azure-beheerportal.


- Meerdere Tenant: Als u een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie maakt, deze moet worden geregistreerd in de adreslijst van uw bedrijf, maar ook moet worden geregistreerd in de adreslijst van elke organisatie die van de toepassing gebruikmaakt. Als u de toepassing beschikbaar in de adreslijst, kunt u een aanmeldingsproces opnemen voor uw klanten waarmee ze toe te staan in uw toepassing. Wanneer ze zich aanmeldt voor uw toepassing, ze krijgt een dialoogvenster dat wordt weergegeven welke machtigingen van die de toepassing is vereist, waarna de optie toe te staan. Afhankelijk van de vereiste machtigingen, een beheerder in de andere organisatie mogelijk moet toestemming geven. Wanneer de gebruiker of beheerder akkoord gaat, wordt de toepassing is geregistreerd in hun directory. Zie voor meer informatie [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

Na het registreren van de toepassing, moet dit worden geconfigureerd OAuth 2.0 impliciete verlenen protocol gebruiken. Dit protocol is standaard uitgeschakeld voor toepassingen. Schakel in het protocol OAuth2 impliciete verlenen voor uw toepassing door downloaden van het manifest van de toepassing van de Azure-beheerportal, de waarde "oauth2AllowImplicitFlow" ingesteld op waar en upload de manifest terug naar de portal. Zie voor gedetailleerde informatie kunt [Inschakelen OAuth 2.0 impliciete verlenen voor één pagina-toepassingen](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Verlopen van token

Wanneer u ADAL.js gebruikt voor het beheren van verificatie met Azure AD, profiteert u diverse functies die een verlopen token vernieuwen, evenals tokens ophalen voor aanvullende web API resources die mogelijk worden aangeroepen door de toepassing te vergemakkelijken. Wanneer de gebruiker is geverifieerd met Azure AD, een sessie beveiligd met een cookie tot stand is gebracht voor de gebruiker tussen de browserversie en Azure AD. Het is belangrijk te weten dat de sessie bestaat tussen de gebruiker en Azure AD en niet tussen de gebruiker en de webtoepassing uitgevoerd op de server. Als een token verloopt, wordt deze sessie in ADAL.js gebruikt naar een andere token stilte te verkrijgen. Dit gebeurt met behulp van een verborgen iFrame verzenden en ontvangen van de aanvraag met het OAuth impliciete verlenen-protocol. ADAL.js kunt ook deze dezelfde methode gebruiken voor stilte access tokens van Azure AD voor andere web API-resources die de toepassing oproepen zo lang maken als deze resources ondersteunen cross-origin resources delen (CORS), worden geregistreerd in de map en alle vereiste toestemming is gegeven door de gebruiker bij het aanmelden.


### <a name="native-application-to-web-api"></a>De toepassing waarmee Web API


Dit onderwerp vindt een bijbehorende toepassing die een web API namens een gebruiker belt. Dit scenario is gebaseerd op het type OAuth 2.0 autorisatie code verlenen met een openbare mailclient gebruikt, zoals wordt beschreven in de sectie 4.1 van de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). De bijbehorende toepassing wordt een toegangstoken voor de gebruiker opgehaald met behulp van het OAuth 2.0-protocol. In dit toegangstoken wordt vervolgens in de aanvraag voor het web API, geautoriseerd van de gebruiker en levert de gewenste resource verzonden.

#### <a name="diagram"></a>Diagram

![Oorspronkelijke toepassing naar de webpagina-API-Diagram](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Verificatie-mailstroom voor de bijbehorende toepassing tot API

#### <a name="description-of-protocol-flow"></a>Beschrijving van Protocol-mailstroom


Als u de bibliotheken AD-verificatie gebruikt, worden de grootste deel van de gegevens van het protocol hieronder beschreven voor u, zoals de browser pop-upvenster, caching van token en afhandeling van vernieuwen tokens afgehandeld.

1. Een pop-, dat de bijbehorende toepassing ervoor zorgt dat een verzoek om naar het eindpunt autorisatie in Azure AD browser gebruiken. Deze aanvraag bevat de client-ID en de omleiding URI van de oorspronkelijke toepassing, zoals wordt weergegeven in de beheerportal en de toepassing-ID-URI voor het web API. Als de gebruiker al is niet aangemeld, worden ze gevraagd te opnieuw aanmelden


2. Azure AD verifieert de gebruiker. Als dit een toepassing voor meerdere tenant is en toestemming vereist is voor het gebruik van de toepassing, de gebruiker moet toestemming als ze nog niet had gedaan. Nadat u hebt toestemming verlenen en na het verificatie is geslaagd, problemen Azure AD een antwoord van de code autorisatie terug naar de clienttoepassing redirect URI.


3. Wanneer Azure AD een antwoord van de code autorisatie terug naar de omleiding URI problemen, wordt de clienttoepassing browser interactie stopt en de autorisatiecode haalt uit het antwoord. Met deze autorisatiecode, verzendt de clienttoepassing een aanvraag naar token Azure AD-eindpunt dat de autorisatiecode bevat, en details over de clienttoepassing (cliënt-ID en omleiden URI) en de gewenste resource (toepassing-ID-URI voor het web API).


4. De autorisatiecode en informatie over de client-toepassing en web API worden gevalideerd door Azure AD. Na het validatie is geslaagd, geeft Azure AD twee tokens: een toegangstoken JWT en een JWT vernieuwen token. Daarnaast Azure AD geeft als resultaat basisinformatie over de gebruiker, zoals de weergegeven naam en een tenant-ID.


5. De clienttoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.


6. Als het toegangstoken verloopt, ontvangt de clienttoepassing een foutbericht weergegeven dat de gebruiker moet opnieuw te identificeren. Als de toepassing een token geldige vernieuwen heeft, kan deze worden gebruikt om een nieuwe toegangstoken zonder dat de gebruiker opnieuw aanmelden. Als het vernieuwen token is verlopen, moet de toepassing interactief nogmaals de gebruiker te verifiëren.


> [AZURE.NOTE] Het vernieuwen token uitgegeven door Azure AD kan worden gebruikt voor toegang tot meerdere resources. Als u een clienttoepassing die daartoe is gemachtigd om te bellen van twee web API's hebt, kan er bijvoorbeeld het token vernieuwen worden gebruikt een om toegang te krijgen op het andere web API ook token.


#### <a name="code-samples"></a>Voorbeelden van programmacode


Zie de voorbeelden van de code voor de toepassing waarmee Web API-scenario's. En Kijk hier regelmatig--we nieuwe voorbeelden altijd toevoegen. [De toepassing waarmee Web API](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registreren


- Eén Tenant: Native de beide-toepassing en het web API moeten zijn geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een reeks machtigingen, die worden gebruikt om de bijbehorende toepassing toegang tot de bronnen te beperken. De clienttoepassing vervolgens selecteert de gewenste machtigingen uit het vervolgkeuzemenu 'Machtigingen aan andere toepassingen' in de beheerportal Azure.


- Meerdere Tenant: Eerst de bijbehorende toepassing alleen ooit geregistreerd in de ontwikkelaar of van publisher-gids. Tweede, de bijbehorende toepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor het werken. Deze lijst met de vereiste machtigingen wordt weergegeven in een dialoogvenster wanneer een gebruiker of beheerder in de doelmap toestemming geeft met de toepassing, waardoor u deze beschikbaar zijn voor hun organisatie. Bepaalde toepassingen vereisen alleen beveiliging op gebruikersniveau machtigingen die iedere gebruiker in de organisatie kunt toestemming om te. Andere toepassingen vragen beheerdersniveau machtigingen, die een gebruiker in de organisatie kan geen toestemming om te. Alleen de beheerder van een map kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder akkoord gaat, wordt alleen het web API is geregistreerd in hun directory. Zie voor meer informatie [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Verlopen van token


Wanneer de bijbehorende toepassing wordt de autorisatiecode gebruikt om een JWT toegang krijgen tot token, ontvangt deze ook een JWT vernieuwen token. Als het toegangstoken verloopt, kan het token vernieuwen worden gebruikt om te verifiëren opnieuw de gebruiker zonder deze opnieuw aanmelden. Deze token vernieuwen vervolgens wordt gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuwe toegangstoken en vernieuwen token.





### <a name="web-application-to-web-api"></a>Webtoepassing Web API


Dit onderwerp vindt een webtoepassing die vereist zijn voor het ophalen van resources uit een web API. In dit scenario zijn er zijn twee identiteitstypen die de webtoepassing gebruiken kunt om te verifiëren en bellen van het web API: een toepassings-id of een gedelegeerd gebruikers-id.

*Toepassings-id:* In dit scenario wordt OAuth 2.0-client referenties verlenen om te verifiëren als de toepassing en toegang tot het web API. Wanneer u een toepassings-id, het web API alleen detecteren kan dat de webtoepassing is bellen, wordt gebruikt als het web ontvangt API geen alle gegevens over de gebruiker. De toepassing ontvangt informatie over de gebruiker, wordt verzonden via de toepassingsprotocol als het niet is ondertekend door Azure AD. Het web API vertrouwt dat de webtoepassing de gebruiker geverifieerd. Om die reden is dit patroon een vertrouwde subsysteem genoemd.

*Gedelegeerde gebruikers-id:* Dit scenario kan worden uitgevoerd op twee manieren: OpenID verbinding en OAuth 2.0 autorisatie code verlenen met een vertrouwelijke client. De webtoepassing wordt een toegangstoken voor de gebruiker, op het web API die de gebruiker geverifieerd aan de webtoepassing en dat de webtoepassing krijgen een gedelegeerd gebruikers-id om te bellen van het web API blijkt opgehaald. In dit toegangstoken wordt in de aanvraag voor het web API, geautoriseerd van de gebruiker en levert de gewenste resource verzonden.

#### <a name="diagram"></a>Diagram

![Webtoepassing aan Web API-diagram](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Beschrijving van Protocol-mailstroom

De toepassings-id en de gedelegeerde gebruiker identiteit typt, worden in de onderstaande stroom besproken. Het belangrijkste verschil hiertussen is dat de gedelegeerde gebruikersidentiteit moet eerst in het bezit van een autorisatiecode voordat de gebruiker kan aanmelden en toegang krijgen tot het web API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met OAuth 2.0-Client referenties verlenen

1. Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de [Webbrowser naar webtoepassing](#web-browser-to-web-application) hierboven).


2. De webtoepassing moet in het bezit van een toegangstoken zodat deze kan op het web API verifiëren en de gewenste resource op te halen. Deze aanbrengt een aanvraag in Azure AD token eindpunt, de referentie, cliënt-ID en-ID-URI API's webtoepassing.


3. Azure AD verifieert de toepassing en geeft als resultaat een JWT-toegangstoken die wordt gebruikt om te bellen van het web API.


4. De webtoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.

##### <a name="delegated-user-identity-with-openid-connect"></a>Gedelegeerd gebruikers-id met OpenID verbinding maken

1. Een gebruiker is aangemeld bij een webtoepassing met Azure AD (Zie de sectie [Webbrowser naar webtoepassing](#web-browser-to-web-application) hierboven). Als de gebruiker van de webtoepassing heeft nog niet ingestemd met het toestaan van de webtoepassing om te bellen van het web API namens, moet de gebruiker toe te staan. De toepassing de machtigingen die er moeten worden weergegeven en als een van de volgende beheerdersrechten, een normale gebruiker in de adreslijst niet kunnen toestemming. Dit proces toestemming geldt alleen voor meerdere tenant-toepassingen, geen afzonderlijke tenant-toepassingen, zoals de toepassing beschikt al over de benodigde machtigingen. Wanneer de gebruiker is aangemeld, ontvangen de webtoepassing een token ID met informatie over de gebruiker, evenals een autorisatiecode.


2. Met de autorisatiecode is uitgegeven door Azure AD, verzendt de webtoepassing een aanvraag naar token Azure AD-eindpunt dat de autorisatiecode bevat, en details over de clienttoepassing (cliënt-ID en omleiden URI) en de gewenste resource (toepassing-ID-URI voor het web API).


3. De autorisatiecode en informatie over de webtoepassing en web API worden gevalideerd door Azure AD. Na het validatie is geslaagd, geeft Azure AD twee tokens: een toegangstoken JWT en een JWT vernieuwen token.


4. De webtoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Gedelegeerd gebruikers-id met OAuth 2.0 autorisatie Code verlenen

1. Een gebruiker is al aangemeld bij een webtoepassing, waarvan verificatiemethode onafhankelijk van Azure AD is.


2. De webtoepassing is een autorisatiecode bij het aanschaffen van een toegangstoken, zodat deze een aanvraag via de browser service aan Azure AD autorisatie eindpunt, leveren van de client-ID en omleiden URI voor de webtoepassing na een succesvolle verificatie vereist. De gebruiker zich aanmeldt bij Azure AD.


3. Als de gebruiker van de webtoepassing heeft nog niet ingestemd met het toestaan van de webtoepassing om te bellen van het web API namens, moet de gebruiker toe te staan. De toepassing de machtigingen die er moeten worden weergegeven en als een van de volgende beheerdersrechten, een normale gebruiker in de adreslijst niet kunnen toestemming. Dit proces toestemming geldt alleen voor meerdere tenant-toepassingen, geen afzonderlijke tenant-toepassingen, zoals de toepassing beschikt al over de benodigde machtigingen.


4. Nadat de gebruiker heeft ingestemd, ontvangt de webtoepassing de autorisatiecode die nodig zijn voor het ophalen van een toegangstoken.


5. Met de autorisatiecode is uitgegeven door Azure AD, verzendt de webtoepassing een aanvraag naar token Azure AD-eindpunt dat de autorisatiecode bevat, en details over de clienttoepassing (cliënt-ID en omleiden URI) en de gewenste resource (toepassing-ID-URI voor het web API).


6. De autorisatiecode en informatie over de webtoepassing en web API worden gevalideerd door Azure AD. Na het validatie is geslaagd, geeft Azure AD twee tokens: een toegangstoken JWT en een JWT vernieuwen token.


7. De webtoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.

#### <a name="code-samples"></a>Voorbeelden van programmacode

Zie de voorbeelden van de code voor webtoepassing Web API-scenario's. En Kijk hier regelmatig--we nieuwe voorbeelden altijd toevoegen. Web [Application naar Web API](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registreren

- Eén Tenant: Voor zowel het toepassings-id en gedelegeerde gebruiker identiteit gevallen, de webtoepassing en de web API moeten worden geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een reeks machtigingen, die worden gebruikt voor het beperken van de webtoepassing toegang tot de bronnen weer te geven. Als een gebruiker gedelegeerde identiteitstype wordt gebruikt, moet de webtoepassing Selecteer de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen aan andere toepassingen' in de beheerportal Azure. Deze stap is niet vereist als het toepassingstype identiteit wordt gebruikt.


- Meerdere Tenant: Eerst de webtoepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor het werken. Deze lijst met de vereiste machtigingen wordt weergegeven in een dialoogvenster wanneer een gebruiker of beheerder in de doelmap toestemming geeft met de toepassing, waardoor u deze beschikbaar zijn voor hun organisatie. Bepaalde toepassingen vereisen alleen beveiliging op gebruikersniveau machtigingen die iedere gebruiker in de organisatie kunt toestemming om te. Andere toepassingen vragen beheerdersniveau machtigingen, die een gebruiker in de organisatie kan geen toestemming om te. Alleen de beheerder van een map kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder akkoord gaat, de webtoepassing en de web API beide geregistreerd in de adreslijst.

#### <a name="token-expiration"></a>Verlopen van token

Als de webtoepassing gebruikmaakt van de autorisatiecode voor een JWT om toegang te krijgen token, ontvangt deze ook een JWT vernieuwen token. Als het toegangstoken verloopt, kan het token vernieuwen worden gebruikt om te verifiëren opnieuw de gebruiker zonder deze opnieuw aanmelden. Deze token vernieuwen vervolgens wordt gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuwe toegangstoken en vernieuwen token.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon of servertoepassing Web API


Dit onderwerp vindt een toepassing daemon of server die vereist zijn voor het ophalen van resources uit een web API. Er zijn twee onderliggend scenario's die van toepassing op deze sectie: een daemon die vereist zijn voor het bellen van een web API, gebaseerd op OAuth 2.0 referenties verlenen clienttype; en een server-toepassing (zoals een web API) dat u bellen een web API wilt, gebaseerd op OAuth 2.0 On-Behalf-Of conceptspecificatie.

Voor het scenario wanneer een daemontoepassing moet bellen van een web API, is het belangrijk voor meer informatie over een aantal dingen. Eerst is interactie van de gebruiker niet mogelijk met een daemon-toepassing, waarvoor u de toepassing heeft een eigen identiteit. Een voorbeeld van een daemontoepassing is een batchtaak, of een service van het besturingssysteem uitgevoerd op de achtergrond. Dit type toepassing een toegangstoken aanvraagt met behulp van de toepassingsidentiteit en presenteren van de client-ID, referentie (wachtwoord of certificaat) en -toepassing te Azure AD-ID-URI. Na de verificatie is geslaagd, wordt in de daemon een toegangstoken ontvangt van Azure AD, die vervolgens wordt gebruikt om te bellen van het web API.

Voor het scenario wanneer een servertoepassing moet bellen van een web API, is het handig een voorbeeld wilt gebruiken. Stel dat een gebruiker op een bijbehorende toepassing geverifieerd, en deze systeemeigen toepassing moet een web API bellen. Azure AD problemen een toegangstoken JWT om te bellen van het web API. Als de web-API bellen van een ander volgende web API moet, kunt deze via de stroom op-namens-of gemachtigde van de gebruikers id en op het tweede laag web API wordt geverifieerd.

#### <a name="diagram"></a>Diagram

![Daemon of servertoepassing aan Web API-diagram](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van Protocol-mailstroom

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Toepassings-id met OAuth 2.0-Client referenties verlenen

1. De servertoepassing moet eerst worden geverifieerd bij Azure AD als zelf, zonder tussenkomst menselijke zoals een interactieve dialoog aanmelding. Deze aanbrengt een aanvraag in Azure AD token eindpunt, bieden de referentie, de client-ID en de toepassing-ID-URI.


2. Azure AD verifieert de toepassing en geeft als resultaat een JWT-toegangstoken die wordt gebruikt om te bellen van het web API.


3. De webtoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Gedelegeerd gebruikers-id met OAuth 2.0 concept-op-namens-van-specificatie

De stroom besproken hieronder wordt ervan uitgegaan dat een gebruiker op een andere toepassing (zoals een bijbehorende toepassing) is geverifieerd, en hun gebruikers-id wordt gebruikt om in het bezit van een toegangstoken op het eerste laag web API.

1. De oorspronkelijke toepassing verzendt het toegangstoken naar het eerste laag web API.


2. De eerste laag web API wordt een aanvraag verzonden naar token Azure AD-eindpunt, leveren van de client-ID en referenties, evenals toegangstoken van de gebruiker. Bovendien de aanvraag is verzonden met een on_behalf_of parameter die aangeeft van het web API nieuwe tokens om te bellen van een volgende web API namens de oorspronkelijke gebruiker vraagt.


3. Azure AD wordt gecontroleerd of het eerste laag web API machtigingen voor toegang tot de tweede laag web API heeft en is gevalideerd met het verzoek, een toegangstoken JWT retourneren een JWT token op het eerste laag web API vernieuwen.


4. Via HTTPS oproepen de eerste laag web API vervolgens de tweede laag web API door de token tekenreeks in de koptekst autorisatie in het verzoek toe te voegen. De eerste laag web API kunt doorgaan met het bellen van de tweede laag web API zo lang maken als de toegangstoken en vernieuwen tokens geldig zijn.

#### <a name="code-samples"></a>Voorbeelden van programmacode

Zie de voorbeelden van de code voor Daemon of servertoepassing Web API-scenario's. En Kijk hier regelmatig--we nieuwe voorbeelden altijd toevoegen. [Server of demontoepassing naar Web API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registreren

- Eén Tenant: Voor zowel het toepassings-id en gedelegeerde gebruiker identiteit gevallen, de toepassing daemon of server moet worden geregistreerd in dezelfde map in Azure AD. Het web API kan worden geconfigureerd om een reeks machtigingen, die worden gebruikt voor het beperken van de daemon of van server toegang tot de bronnen weer te geven. Als een gebruiker gedelegeerde identiteitstype wordt gebruikt, moet de servertoepassing Selecteer de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen aan andere toepassingen' in de beheerportal Azure. Deze stap is niet vereist als het toepassingstype identiteit wordt gebruikt.


- Meerdere Tenant: Eerst de toepassing daemon of server is geconfigureerd om aan te geven van de machtigingen die nodig is voor het werken. Deze lijst met de vereiste machtigingen wordt weergegeven in een dialoogvenster wanneer een gebruiker of beheerder in de doelmap toestemming geeft met de toepassing, waardoor u deze beschikbaar zijn voor hun organisatie. Bepaalde toepassingen vereisen alleen beveiliging op gebruikersniveau machtigingen die iedere gebruiker in de organisatie kunt toestemming om te. Andere toepassingen vragen beheerdersniveau machtigingen, die een gebruiker in de organisatie kan geen toestemming om te. Alleen de beheerder van een map kunt toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder akkoord gaat, worden beide van de web API's in hun directory geregistreerd.

#### <a name="token-expiration"></a>Verlopen van token

Wanneer de eerste toepassing gebruikt de autorisatiecode voor een JWT om toegang te krijgen token, ontvangt deze ook een JWT vernieuwen token. Als het toegangstoken verloopt, kan het token vernieuwen om te verifiëren opnieuw de gebruiker zonder waarschuwing om referenties worden gebruikt. Deze token vernieuwen vervolgens wordt gebruikt voor verificatie van de gebruiker, wat resulteert in een nieuwe toegangstoken en vernieuwen token.

## <a name="see-also"></a>Zie ook

[Handleiding voor ontwikkelaars van de Azure Active Directory](active-directory-developers-guide.md)

[Voorbeelden van de Azure Active Directory-Code](active-directory-code-samples.md)

[Belangrijke informatie over het sleutelrollover aanmelden met Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
