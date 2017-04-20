<properties
   pageTitle="Woordenlijst voor ontwikkelaars van Azure Active Directory | Microsoft Azure"
   description="Een lijst met voorwaarden voor veelgebruikte Azure Active Directory ontwikkelaars concepten en functies."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory-woordenlijst voor ontwikkelaars
In dit artikel bevat definities voor een deel van de belangrijkste Azure Active Directory (AD) ontwikkelaars begrippen, die handig zijn als u meer informatie over het ontwikkelen van toepassingen voor Azure AD.

## <a name="access-token"></a>toegangstoken
Een type [beveiligingstoken](#security-token) uitgegeven door een [server autorisatie](#authorization-server)en gebruikt door een [clienttoepassing](#client-application) voor toegang tot een [bronserver is beveiligd](#resource-server). Meestal in de vorm van een [JSON Web Token (JWT)][JWT], het token bevat de machtiging voor de client door de [eigenaar van de resource](#resource-owner), voor een aangevraagde toegangsniveau. Het token bevat alle van toepassing [op claims](#claim) over het onderwerp, zodat de clienttoepassing gebruiken als referentie bij het openen van een bepaalde bron. Hierdoor wordt ook dat u de eigenaar van de resource om weer te geven van referenties naar de klant.

Access tokens worden ook wel "Gebruiker + App" of "App alleen', afhankelijk van de referenties die wordt weergegeven genoemd. Bijvoorbeeld: wanneer een clienttoepassing gebruikt de:

- ["Autorisatiecode" autorisatie verlenen](#authorization-grant), de eindgebruiker wordt geverifieerd door eerst als de eigenaar van de resource, delegeren van autorisatie voor de client voor toegang tot de resource. De client verifieert daarna bij het verkrijgen van het toegangstoken. Het token kan soms worden genoemd specifieker als "Gebruiker + App" token, staat dit voor de gebruiker die gemachtigd de clienttoepassing en de toepassing.
- ["Clientreferenties" toestemming verlenen](#authorization-grant), de client biedt de enige verificatie, werking zonder verificatie/autorisatie voor de resource-eigenaar, zodat het token kan ook worden genoemd als een token "Alleen App".

Raadpleeg [referentie voor Azure AD Token] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="application-manifest"></a>toepassingsmanifest  
Een functie van de [Azure klassieke portal][AZURE-classic-portal], die een JSON-weergave van de identiteit de configuratie van toepassing, die wordt gebruikt als voor het bijwerken van de bijbehorende [toepassing] om oplevert[ AAD-Graph-App-Entity] en [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteiten. Zie [Wat is de manifest van de Azure Active Directory-toepassing] [ AAD-App-Manifest] voor meer informatie.

## <a name="application-object"></a>Application-object  
Wanneer u register/update een toepassing in de [portal van Azure klassieke][AZURE-classic-portal], de portal wordt gemaakt/updates zowel een application-object en een bijbehorende [service principal object](#service-principal-object) voor die tenant. De toepassing object *definieert* de toepassing bevindt zich identiteit configuratie globaal (in alle tenants waar deze toegang heeft), middel van een sjabloon van waaruit de bijbehorende service principal of meer objecten zijn *afgeleid* voor gebruik lokaal tijdens runtime (in een specifieke tenant).

Zie [-toepassing en Service Principal objecten] [ AAD-App-SP-Objects] voor meer informatie.

## <a name="application-registration"></a>registratie van toepassing  
Als u wilt toestaan dat een toepassing integreren met en delegeren identiteit toegangsbeheer functies en Azure AD, moet deze zijn geregistreerd bij een Azure AD- [tenant](#tenant). Wanneer u uw toepassing met Azure AD registreert, u hebt opgenomen de configuratie van een identiteit voor uw toepassing, zodat het integreren met Azure AD en het gebruik van functies, zoals:

- Krachtige beheer van eenmalige aanmelding met Azure AD-identiteitsbeheer en [Verbindt u OpenID] [ OpenIDConnect] protocol-implementatie
- Brokered toegang tot [beveiligde resources](#resource-server) met [clienttoepassingen](#client-application), via Azure AD OAuth 2.0 [autorisatie server](#authorization-server) -implementatie
- [Framework toestemming](#consent) voor het beheer van de clienttoegang tot beveiligde bronnen, op basis van de resource eigenaarsautorisatie.

Zie [integratie toepassingen met Azure Active Directory] [ AAD-Integrating-Apps] voor meer informatie.

## <a name="authentication"></a>verificatie
De act van een partij voor legitieme referenties, waarin u de basis voor het maken van een beveiligings-principal moet worden gebruikt voor identiteits- en toegangsbeheer moeilijker. Tijdens een [OAuth2 autorisatie verlenen](#authorization-grant) bijvoorbeeld is de partij verificatie invullen de rol van [de eigenaar van de resource](#resource-owner) of [clienttoepassing](#client-application), afhankelijk van het verlenen gebruikt.

## <a name="authorization"></a>autorisatie
De act van het toekennen van de een geverifieerde beveiliging principal machtiging wilt hebben. Er zijn twee gevallen van primaire gebruiken in het programmeren Azure AD-model:

- Tijdens een stroom [OAuth2 autorisatie verlenen](#authorization-grant) : wanneer u de [eigenaar van de resource](#resource-owner) verleent autorisatie met de [clienttoepassing](#client-application), zodat u de client van de eigenaar van de resource informatiebronnen.
- Tijdens de resource toegang tot de client: zoals geïmplementeerd door de [resource-server](#resource-server), met de [claimen](#claim) waarden weergeven in het [toegangstoken](#access-token) voor toegang tot het toegangsbeheer op basis van deze.

## <a name="authorization-code"></a>Autorisatiecode
Een korte levensduur "token" geleverd met een [clienttoepassing](#client-application) door het [eindpunt van de autorisatie](#authorization-endpoint), als onderdeel van de stroom "autorisatiecode" een van de vier OAuth2 [autorisatie worden toegewezen](#authorization-grant). De code wordt geretourneerd met de clienttoepassing in antwoord op de verificatie van een [eigenaar van de resource](#resource-owner), die aangeeft dat u dat de eigenaar van de resource heeft gedelegeerd autorisatie voor toegang tot de gevraagde bronnen. Als onderdeel van de stroom, is de code later hebt ingewisseld voor een [toegangstoken](#access-token).

## <a name="authorization-endpoint"></a>autorisatie eindpunt
Een van de eindpunten van de [autorisatie server](#authorization-server)gebruikt om te communiceren met de [eigenaar van de resource](#resource-owner) kan alleen worden aangeboden een [vergunning verlenen](#authorization-grant) tijdens een vergunning OAuth2 geïmplementeerd stroom verlenen. Afhankelijk van de autorisatie verlenen stroom gebruikt, kan de werkelijke verlenen verstrekt variëren, wordt een [autorisatiecode](#authorization-code) of het [beveiligingstoken](#security-token)inclusief.

Zie de OAuth2-specificatie [autorisatie verlenen typen] [ OAuth2-AuthZ-Grant-Types] en [autorisatie eindpunt] [ OAuth2-AuthZ-Endpoint] secties en de [specificatie van OpenIDConnect] [ OpenIDConnect-AuthZ-Endpoint] voor meer informatie.

## <a name="authorization-grant"></a>autorisatie verlenen
Een referentie dat staat voor het [van de eigenaar van de resource](#resource-owner) [autorisatie](#authorization) voor toegang tot de beveiligde bronnen, naar een [clienttoepassing](#client-application)verleend. Een clienttoepassing de beschikking over een van de [vier verlenen typen gedefinieerd door het kader van de autorisatie OAuth2] [ OAuth2-AuthZ-Grant-Types] voor een verlenen, afhankelijk van de client type/vereisten: "autorisatie code grant", "clientreferenties verlenen", "impliciete verlenen" en "resource eigenaar wachtwoordreferenties verlenen". De referentie geretourneerd naar de klant is een [toegangstoken](#access-token), of een [autorisatiecode](#authorization-code) (uitgewisseld later voor een toegangstoken), afhankelijk van het type autorisatie verlenen gebruikt.

## <a name="authorization-server"></a>autorisatie server
Zoals gedefinieerd door het [OAuth2 autorisatie Framework][OAuth2-Role-Def], de server die verantwoordelijk is voor het verlenen van toegang tokens naar de [client](#client-application) nadat u succes verifiëren van de [eigenaar van de resource](#resource-owner) en het verkrijgen van haar toestemming hebt. Een [clienttoepassing](#client-application) omgaat met de server autorisatie gedurende runtime via de [autorisatie](#authorization-endpoint) en [token](#token-endpoint) eindpunten, volgens de OAuth2 gedefinieerd [autorisatie worden toegewezen](#authorization-grant).

Voor integratie van Azure AD-toepassingen, implementeert Azure AD de rol van de server autorisatie voor Azure AD-toepassingen en Microsoft-service API's, bijvoorbeeld [Microsoft Graph-API's][Microsoft-Graph].

## <a name="claim"></a>claimen
Een [beveiligingstoken](#security-token) bevat claims, waarmee bevestigingen over een entiteit (zoals een [clienttoepassing](#client-application) of [de eigenaar van de resource](#resource-owner)) aan een andere entiteit (zoals de [bronserver](#resource-server)). Op claims zijn naam/waardeparen die doorsturen feiten over het token onderwerp (bijvoorbeeld de beveiligings-principal die door de [autorisatie server](#authorization-server)is geverifieerd). Het presenteren in een bepaald token claims zijn afhankelijk van verschillende variabelen, met inbegrip van het type token, het type referentie gebruikt om te verifiëren van het onderwerp, de configuratie van toepassingen, enzovoort.

Raadpleeg [referentie voor token Azure AD] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="client-application"></a>clienttoepassing  
Zoals gedefinieerd door het [OAuth2 autorisatie Framework][OAuth2-Role-Def], een toepassing die beveiligde resource aanvragen namens de [eigenaar van de resource](#resource-owner). De term "client" betekent niet een bepaalde hardware implementatie kenmerken (bijvoorbeeld opgeven of de toepassing wordt uitgevoerd op een server, een bureaublad of op andere apparaten).  

Een clienttoepassing [autorisatie](#authorization) aanvraagt aan de eigenaar van een resource voor het deelnemen aan een stroom [OAuth2 toestemming verlenen](#authorization-grant) en API's / gegevens namens van de eigenaar van de resource kan openen. De OAuth2 autorisatie Framework [definieert twee soorten clients][OAuth2-Client-Types], 'vertrouwelijk' en 'openbare', op basis van de mogelijkheid van de client om vertrouwelijk van de referenties. Toepassingen kunnen implementeren een [webclient (vertrouwelijke)](#web-client) die wordt uitgevoerd op een webserver, een [native client (openbaar)](#native-client) zijn geïnstalleerd op een apparaat of een [gebruiker-agent - clientapparaten (openbaar)](#user-agent-based-client) die wordt uitgevoerd in de browser van een apparaat.

## <a name="consent"></a>toestemming
Het proces van een [eigenaar van de resource](#resource-owner) voor het toekennen van autorisatie met een [clienttoepassing](#client-application), specifieke [machtigingen](#permissions) voor toegang tot beveiligde bronnen, namens de eigenaar van de resource. Afhankelijk van de machtigingen die door de client aangevraagd, wordt een beheerder of de gebruiker gevraagd om te toestemming om toegang te krijgen tot hun gegevens organisatie/afzonderlijke respectievelijk. Houd er rekening mee in een scenario voor [meerdere tenant](#multi-tenant-application) van de toepassing [service principal](#service-principal-object) ook is vastgelegd in de tenant van de consenting gebruiker.

## <a name="id-token"></a>ID token
Een [Verbinding OpenID] [ OpenIDConnect-ID-Token] [beveiligingstoken](#security-token) verstrekt door een [van de server autorisatie](#authorization-server) [autorisatie eindpunt](#authorization-endpoint), waarin [claims](#claim) die betrekking hebben op de verificatie van een gebruiker [de eigenaar van de resource](#resource-owner). Als een toegangstoken, worden ook ID tokens weergegeven als een digitaal ondertekend [JSON Web Token (JWT)][JWT]. In tegenstelling tot een toegangstoken echter een ID-token claims niet worden gebruikt voor doeleinden die betrekking hebben op toegang tot bronnen en specifiek toegangsbeheer.

Raadpleeg [referentie voor token Azure AD] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="multi-tenant-application"></a>meerdere tenant-toepassing
Een categorie van [clienttoepassing](#client-application) waarmee aanmelden en [toestemming](#consent) door gebruikers deze is ingericht in elke Azure AD- [tenant](#tenant), inclusief tenants behalve het gedeelte waar de klant is geregistreerd. Daarentegen zou een toepassing die is geregistreerd als één-tenant, kunnen alleen aanmeldingen van deze is ingericht in dezelfde tenant als de gebruikersaccounts waar de toepassing is geregistreerd. [Native client](#native-client) -toepassingen zijn meerdere tenant al dan niet standaard, terwijl de [web](#web-client) -clienttoepassingen hebt de mogelijkheid te kiezen tussen één en meerdere tenant.

Lees [hoe u zich in een Azure AD-gebruiker met het patroon dat in meerdere tenant-toepassing] [ AAD-Multi-Tenant-Overview] voor meer informatie.

## <a name="native-client"></a>native client
Een type [clienttoepassing](#client-application) die standaard is geïnstalleerd op een apparaat. Aangezien alle code wordt uitgevoerd op een apparaat, wordt een 'openbare' client vanwege dat voor de opslag van referenties privé/vertrouwelijk. Zie [OAuth2 clienttypen en profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="permissions"></a>machtigingen
Een [clienttoepassing](#client-application) krijgt toegang tot een [bronserver](#resource-server) door toestemmingsaanvragen declareren. Twee typen zijn beschikbaar:

- "Gedelegeerde" machtigingen die toegang onder [bereik gebaseerde](#scopes) aanvragen gedelegeerde toestemming van de die zijn aangemeld bij [de eigenaar van de resource](#resource-owner), aan de resource tijdens runtime als ["scp" claims](#claim) in van de klant [toegangstoken](#access-token)worden weergegeven.
- "" Toepassingsmachtigingen, die [op basis van rollen](#roles) toegang onder de clienttoepassing referenties/identiteit aanvragen, worden als het ['rollen' vorderingen](#claim) in van de klant-toegangstoken aan iedere resource tijdens runtime weergegeven.

Ze ook toepassen tijdens het [toestemming](#consent) geven de beheerder of de eigenaar van de resource de mogelijkheid om de clienttoegang tot bronnen in de tenant verlenen/weigeren.

Toestemmingsaanvragen zijn geconfigureerd op de "toepassingen" / "" tabblad configureren in de [portal van Azure klassieke][AZURE-classic-portal], onder 'De machtigingen voor andere toepassingen', door te selecteren de gewenste "gedelegeerde machtigingen' en 'de toepassingsmachtigingen' (de laatste hiervoor lidmaatschap van de rol van globale beheerder). Omdat een [openbare client](#client-application) kan geen referenties behouden, kunt deze alleen gedelegeerde machtigingen aanvragen terwijl een [vertrouwelijke client](#client-application) de mogelijkheid om te aanvraag voor gedelegeerd en Toepassingsmachtigingen heeft. Van de klant [application-object](#application-object) slaat de opgegeven machtigingen in de [eigenschap requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>de eigenaar van de resource
Zoals gedefinieerd door het [OAuth2 autorisatie Framework][OAuth2-Role-Def], een entiteit kunnen verlenen van toegang tot een beveiligde bron. Wanneer de eigenaar van de resource van een persoon is, deze heet een eindgebruiker. Bijvoorbeeld wanneer een [clienttoepassing](#client-application) wil van een gebruiker postvak openen via de [Microsoft Graph-API][Microsoft-Graph], er machtiging eigenaar van de bron van het Postvak moeten.

## <a name="resource-server"></a>resource-server
Zoals gedefinieerd door het [OAuth2 autorisatie Framework][OAuth2-Role-Def], een server dat hosts resources, kan accepteren en reageren op beveiligde resource aanvragen beveiligd met [clienttoepassingen](#client-application) die een [toegangstoken](#access-token)presenteren. Ook bekend als een beveiligde bronserver of resource toepassing.

Een resource-server worden API's en toegang tot de beveiligde bronnen tot en met [bereiken](#scopes) en [rollen](#roles), met het OAuth 2.0 autorisatie Framework. Voorbeelden hiervan zijn de Azure AD Graph-API die toegang biedt tot gegevens van Azure AD-tenant en de Office 365-API die toegang bieden tot gegevens zoals e-mail en agenda. Deze zijn ook toegankelijk via de [Microsoft Graph-API][Microsoft-Graph].  

Net als een clienttoepassing configuratie van de identiteit van de toepassing van de resource tot stand is gebracht via [registratie](#application-registration) in een Azure AD-tenant, waarin u de toepassing en de service principal object. Sommige Microsoft geleverde API's, zoals de API Azure AD Graph hebt vooraf service principes beschikbaar in alle tenants worden gemaakt tijdens het inrichten geregistreerd.

## <a name="roles"></a>rollen
Zoals [bereiken](#scopes), rollen om er zelf een toe voor een [resource-server](#resource-server) voor de toegang tot de beveiligde bronnen. Er zijn twee typen: een ' gebruikersrol "implementeert Rolgebaseerd toegangsbeheer voor gebruikers/groepen waarvoor toegang tot de resource, terwijl een beheerdersrol"toepassing"implementeert hetzelfde is voor [clienttoepassingen](#client-application) die toegang nodig hebben.

Rollen resource gedefinieerde tekenreeksen zijn (bijvoorbeeld 'onkosten fiatteur', "Alleen-lezen", "Directory.ReadWrite.All"), in de [portal van Azure klassieke] beheerde[ AZURE-classic-portal] via een van de resource [manifest van de toepassing](#application-manifest), en die zijn opgeslagen in van de resource [appRoles eigenschap][AAD-Graph-Sp-Entity]. De portal van Azure klassieke wordt ook gebruikt voor gebruikers toewijzen aan "gebruikersrollen" en configureren van client [machtigingen voor een toepassing](#permissions) voor toegang tot een beheerdersrol "toepassing".

Zie voor gedetailleerde informatie over de rollen van de toepassing die worden aangeboden door Azure AD Graph API, [Graph API machtiging bereiken][AAD-Graph-Perm-Scopes]. Zie voor een voorbeeld stapsgewijze implementatie [Rolgebaseerd toegangsbeheer in de cloud-toepassingen met Azure AD][Duyshant-Role-Blog].

## <a name="scopes"></a>bereiken
Net als [rollen](#roles)zelf bereiken in een formule voor een [resource-server](#resource-server) voor de toegang tot de beveiligde bronnen. Bereiken worden gebruikt om te implementeren [bereik gebaseerde] [ OAuth2-Access-Token-Scopes] toegangsbeheer voor een [clienttoepassing](#client-application) die toegang heeft gekregen gedelegeerd aan iedere resource door de eigenaar.

Bereiken resource gedefinieerde tekenreeksen (bijvoorbeeld "Mail.Read", "Directory.ReadWrite.All"), beheerd in de [portal van Azure klassieke] zijn[ AZURE-classic-portal] via een van de resource [manifest van de toepassing](#application-manifest), en die zijn opgeslagen in van de resource [oauth2Permissions eigenschap][AAD-Graph-Sp-Entity]. De portal van Azure klassieke wordt ook gebruikt voor het configureren van client-toepassing [gedelegeerde machtigingen](#permissions) voor toegang tot een bereik.

Een aanbevolen oefening-naamgevingsconventie, is het gebruik van een notatie "resource.operation.constraint". Zie voor een gedetailleerde beschrijving van de bereiken die worden aangeboden door Azure AD Graph API, [Graph API machtiging bereiken][AAD-Graph-Perm-Scopes]. Voor bereiken die worden aangeboden door Office 365-services, raadpleegt [Office 365-API machtigingen verwijzing][O365-Perm-Ref].

## <a name="security-token"></a>beveiligingstoken
Een ondertekend document met claims, zoals een OAuth2 token of SAML 2.0 bevestiging. Voor een OAuth2 [toestemming verlenen](#authorization-grant), een [toegangstoken](#access-token) (OAuth2) en een [ID Token](OpenID Connect) typen beveiligingstokens zijn, die beide worden geïmplementeerd als een [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>service principal object
Wanneer u register/update een toepassing in de [portal van Azure klassieke][AZURE-classic-portal], de portal wordt gemaakt/updates zowel een [application-object](#application-object) en een bijbehorende service principal object voor die tenant. De toepassing object *definieert* configuratie van de identiteit van de toepassing globaal (in alle tenants waar de bijbehorende toepassing toegang is verleend), en is de sjabloon waaruit de bijbehorende service principal of meer objecten zijn *afgeleid* voor gebruik lokaal tijdens runtime (in een specifieke tenant).

Zie [-toepassing en Service Principal objecten] [ AAD-App-SP-Objects] voor meer informatie.

## <a name="sign-in"></a>aanmelden
Het proces van een [clienttoepassing](#client-application) initiëren eindgebruiker verificatie en gerelateerde provincie, voor het verkrijgen van een [beveiligingstoken](#security-token) en een bereik van de toepassingssessie aan deze staat instellen vastleggen. Beschrijft onderdelen zoals gebruikersprofielinformatie kunt opnemen, en informatie afgeleid van token claims.

De functie aanmeldingsproblemen van een toepassing wordt meestal gebruikt voor het implementeren van eenmalige aanmelding (SSO). Dit kan ook worden voorafgegaan door een functie "aanmelding", als het ingangspunt voor een eindgebruiker toegang te krijgen tot een toepassing (na het eerste aanmeldingsproblemen). De aanmelding functie wordt gebruikt om te verzamelen en extra staat die specifiek zijn voor de gebruiker behouden, en mogelijk [toestemming van de gebruiker](#consent).

## <a name="sign-out"></a>afmelden
Het proces van het niet verifiëren een eindgebruiker loskoppelen van de gebruikersstatus die is gekoppeld aan de sessie [clienttoepassing](#client-application) bij het [aanmelden](#sign-in)

## <a name="tenant"></a>tenant
Een exemplaar van een Azure AD-directory is een Azure AD-tenant genoemd. Biedt een aantal functies, waaronder:

- een registerservice voor geïntegreerde toepassingen
- verificatie van gebruikersaccounts en geregistreerde toepassingen
- REST eindpunten vereist ter ondersteuning van verschillende protocollen inclusief OAuth2 en SAML, inclusief het [eindpunt van de autorisatie](#authorization-endpoint), [token eindpunt](#token-endpoint) en het "algemene" eindpunt gebruikt door [meerdere tenant-toepassingen](#multi-tenant-application).

Een tenant is ook gekoppeld aan een Azure AD of Office 365-abonnement tijdens het inrichten van het abonnement, identiteit en toegangsbeheer functies bieden voor het abonnement. [Hoe kom ik aan een Azure Active Directory-tenant] Zie[ AAD-How-To-Tenant] voor meer informatie over de verschillende manieren waarop u toegang kan krijgen tot een tenant. Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory] [ AAD-How-Subscriptions-Assoc] voor meer informatie over de relatie tussen abonnementen en een Azure AD-tenant.

## <a name="token-endpoint"></a>token eindpunt
Een van de eindpunten geïmplementeerd door de [server autorisatie](#authorization-server) ter ondersteuning van OAuth2 [autorisatie worden toegewezen](#authorization-grant). Afhankelijk van het verlenen, deze kan worden gebruikt om in het bezit van een [toegangstoken](#access-token) (en de gerelateerde "vernieuwen" token) aan een [client](#client-application)of [ID token](#ID-token) gebruikt in combinatie met de [Verbinding van OpenID] [ OpenIDConnect] protocol.

## <a name="user-agent-based-client"></a>Gebruiker-agent-clients
Een type [clienttoepassing](#client-application) die code gedownload van een webserver en wordt uitgevoerd binnen een gebruikersagent (bijvoorbeeld een webbrowser), zoals een enkele pagina toepassing WACHTWOORDVERIFICATIE. Aangezien alle code wordt uitgevoerd op een apparaat, wordt een 'openbare' client vanwege dat voor de opslag van referenties privé/vertrouwelijk. Zie [OAuth2 clienttypen en profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="user-principal"></a>UPN
Net zoals die een principal serviceobject wordt gebruikt voor een toepassingsexemplaar, een user principal-object is een ander type beveiligings-principal, waarmee een gebruiker. De Azure AD Graph [entiteit gebruiker] [ AAD-Graph-User-Entity] definieert het schema voor een object, met inbegrip van de gebruiker-gerelateerde eigenschappen zoals en achternaam, UPN, lidmaatschap van directory een rol, enzovoort. Hierdoor wordt de configuratie van de gebruiker identiteit voor Azure AD tot stand brengen van een UPN tijdens runtime. De UPN wordt gebruikt voor een geverifieerde gebruiker voor eenmalige aanmelding, delegeren [toestemming](#consent) opnemen, zodat wijze het toegangsbeheer, enzovoort.

## <a name="web-client"></a>WebClient
Een type [clienttoepassing](#client-application) die alle code op een webserver, en kunnen uitvoert fungeren als een 'vertrouwelijk' client door de referenties veilig opslaan op de server. Zie [OAuth2 clienttypen en profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
De [handleiding voor Azure AD ontwikkelaars] [ AAD-Dev-Guide] is van de portal wilt gebruiken voor de ontwikkeling van alle Azure AD Verwante onderwerpen, inclusief een overzicht van de [integratie van toepassingen] [ AAD-How-To-Integrate] en de basisbeginselen van [Azure AD-verificatie en de ondersteunde verificatietypen scenario's][AAD-Auth-Scenarios].

Gebruik de volgende sectie voor Disqus opmerkingen om feedback geven en help ons te verfijnen en de inhoud van vorm.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
