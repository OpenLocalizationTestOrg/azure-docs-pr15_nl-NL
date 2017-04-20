<properties
   pageTitle="Het maken van een toepassing die zich iedere gebruiker Azure Active Directory aanmelden kan | Microsoft Azure"
   description="Stap voor stap instructies voor het samenstellen van een toepassing die kunnen Meld u aan een gebruiker van een tenant Azure Active Directory, ook bekend als een toepassing voor meerdere tenant."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Aanmelden met een Azure Active Directory (AD)-gebruiker met het patroon dat in meerdere tenant-toepassing
Als u een Software als een servicetoepassing voor veel organisaties te bieden, kunt u uw toepassing accepteren aanmeldingen van eventuele Azure AD-tenant configureren.  Dit heet in Azure AD waardoor uw toepassing multi-tenant.  Gebruikers in een Azure AD-tenant is mogelijk te melden bij uw toepassing nadat u ermee akkoord dat hun account gebruiken met uw toepassing.  

Als u een bestaande toepassing die heeft een eigen account-systeem, of andere soorten aanmelden van andere aanbieders cloud ondersteunt, het toevoegen van Azure AD aanmelden van een tenant is net zo eenvoudig als het registreren van uw app, sign in code via een OAuth2, OpenID verbinding of SAML toe te voegen en een aanmelden met een Microsoft-knop in uw toepassing plaatsen. Klik op de knop hieronder voor meer informatie over de huisstijl van uw toepassing.

[! [Knop aanmelden] [AAD-aanmeldingsproblemen]][AAD-App-Branding]


In dit artikel wordt ervan uitgegaan dat u al bekend bent met het samenstellen van een toepassing één tenant voor Azure AD.  Als u zich niet, hoofd terug naar de [introductiepagina van de handleiding voor ontwikkelaars] [ AAD-Dev-Guide] en probeert u een van onze snel aan de slag!

Er zijn vier eenvoudige stappen aan uw toepassing converteren naar een Azure AD-app voor meerdere tenant:

1.  Uw registratie van toepassing als u meerdere tenant wilt bijwerken
2.  Bijwerken van uw code aanvragen verzenden naar de/Common eindpunt 
3.  Uw code voor het verwerken van meerdere uitgever waarden bijwerken
4.  Gebruikers en beheerders toestemming begrijpen en wijzigingen van de juiste code

Laten we even naar bij elke stap in detail. U kunt ook rechtstreeks naar [deze lijst met meerdere tenant voorbeelden]gaan[AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registratie moeten meerdere tenant bijwerken
Standaard zijn web app-API registraties in Azure AD één tenant.  U kunt uw registratie meerdere tenant maken door de schakeloptie 'Toepassing is multi-Tenant' op de configuratiepagina van de registratie van uw toepassingen zoeken in de [portal van Azure klassieke] [ AZURE-classic-portal] en ingesteld op "Ja".

Opmerking: Voordat een toepassing kan worden gemaakt als meerdere tenant, vereist Azure AD de App-ID-URI van de toepassing moet uniek zijn. De App-ID-URI is een van de manieren die een toepassing wordt geïdentificeerd in protocolberichten.  Een app één tenant is voldoende voor de App-ID-URI moet uniek zijn binnen die tenant.  Voor een toepassing meerdere tenant moet deze uniek zodat Azure AD de toepassing op alle tenants kan vinden.  Globale uniekheid wordt afgedwongen door het vereisen van de App-ID-URI heeft een hostnaam die overeenkomt met een gecontroleerde domein van de Azure AD-tenant.  Bijvoorbeeld als de naam van uw tenant contoso.onmicrosoft.com en vervolgens een geldige is App-ID-URI zou `https://contoso.onmicrosoft.com/myapp`.  Als uw tenant had een gecontroleerde domein van `contoso.com`, en vervolgens een geldige App-ID-URI zou ook `https://contoso.com/myapp`.  Als u een toepassing als meerdere tenant loopt vast als dit patroon niet u de App-ID-URI volgt.

Native clientregistraties zijn meerdere tenant al dan niet standaard.  U hoeft niet te doen om te maken van een native client registratie multi-tenant-toepassing.

## <a name="update-your-code-to-send-requests-to-common"></a>Uw code om te verzoeken verzenden naar/Common bijwerkt
In een enkel tenant-toepassing, worden aanmelden aanvragen van de tenant aanmelden eindpunt verzonden.   Bijvoorbeeld voor contoso.onmicrosoft.com zou het eindpunt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Aanvragen verzonden naar een tenant eindpunt kunnen Meld u aan gebruikers (of gasten) in die tenant naar toepassingen in die tenant.  Met een meerdere tenant-toepassing weet de toepassing niet vooraf welke tenant de gebruiker bevindt zich uit, zodat u kunt geen aanvragen naar een tenant-eindpunt verzenden.  In plaats daarvan worden aanvragen verzonden naar een eindpunt dat multiplexes over alle Azure AD-tenants:

    https://login.microsoftonline.com/common

Als Azure AD krijgt met een verzoek om op de/Common eindpunt, deze de gebruiker zich aanmeldt en een als gevolg hiervan ontdekt welke tenant afkomstig van de gebruiker is.  De/algemene eindpunt werkt met alle verificatieprotocollen worden ondersteund door Azure AD: OpenID verbinden, OAuth 2.0 SAML 2.0 en WS-Federatie.

Het teken in antwoord op de toepassing vervolgens bevat een token dat staat voor de gebruiker.  De waarde van de uitgever in het token Hiermee wordt een toepassing aan welke tenant afkomstig van de gebruiker is.  Wanneer een antwoord geeft als resultaat van de/Common eindpunt, de uitgever waarde in het token overeen met de tenant van de gebruiker.  Het is belangrijk te weten de/Common eindpunt is niet een tenant en niet een uitgever is, kunt u alleen een multiplexer.  Wanneer u een/Common gebruikt, moet de logica in uw toepassing voor het valideren van tokens worden bijgewerkt om rekening te dit. 

Zoals eerder is vermeld, moeten meerdere tenant toepassingen ook een consistente aanmeldervaring geven voor gebruikers van de Azure AD-toepassing huisstijl richtlijnen te volgen. Klik op de knop hieronder voor meer informatie over de huisstijl van uw toepassing.

[! [Knop aanmelden] [AAD-aanmeldingsproblemen]][AAD-App-Branding]

We even verder kijken bij het gebruik van de/Common eindpunt en de implementatie van uw code uitgebreider.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Uw code voor het verwerken van meerdere uitgever waarden bijwerken
Webtoepassingen en web API's ontvangen en tokens afgeleid van Azure AD valideren.  

> [AZURE.NOTE] Terwijl systeemeigen clienttoepassingen aanvragen en u ontvangt van tokens van Azure AD, doen ze wilt sturen naar API's, waar ze worden gevalideerd.  Systeemeigen toepassingen tokens niet worden gevalideerd en moeten behandeld als ondoorzichtig.

Laten we even naar aan hoe tokens is gevalideerd met een toepassing wordt ontvangen van Azure AD.  Een toepassing voor één tenant vindt gewoonlijk een eindpunt waarde is zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

en dit gebruiken om de URL van een metagegevens (in dit geval OpenID verbinding) te maken zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

twee kritieke delen van gegevens die worden gebruikt voor het valideren van tokens downloaden: de tenant bevindt zich aanmelden toetsen en uitgever waarde.  Elke Azure AD-tenant heeft een unieke uitgever-waarde van het formulier:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

waar is de waarde GUID de versie geschikt naam wijzigen van de tenant-ID van de tenant.  Als u op klikt op de bovenstaande link metagegevens voor `contoso.onmicrosoft.com`, ziet u de waarde van deze uitgever in het document.

Wanneer een toepassing voor één tenant is gevalideerd met een token, controleert de handtekening van het token ten opzichte van de ondertekend sleutels van het metagegevensdocument, en wordt gecontroleerd of de uitgever waarde in het token overeenkomt met het nummer dat in het metagegevensdocument.

Sinds de/Common eindpunt komen niet overeen met een tenant en een uitgever, niet als u de uitgever waarde in de metagegevens voor onderzoeken/algemene er een sjablonen URL in plaats van een werkelijke waarde:

    https://sts.windows.net/{tenantid}/

Daarom een toepassing voor meerdere tenant tokens niet kunt valideren door overeenkomen met de waarde van de uitgever in de metagegevens met de `issuer` waarde in het token.  Een toepassing voor meerdere tenant logica te bepalen welke waarden uitgever geldig zijn en die worden niet nodig, op basis van de tenant-id-gedeelte van de uitgever-waarde.  

Bijvoorbeeld als een toepassing voor meerdere tenant alleen aanmelden bij specifieke tenants die zich hebben aangemeld voor de service kunt, moet dit moet u controleren de uitgever waarde of de `tid` waarde in het token om ervoor te zorgen dat tenant is in de lijst met abonnees claimen.  Als een toepassing voor meerdere tenant alleen personen behandelt en niet nemen alle access op basis van tenants beslissingen, klikt u vervolgens kan worden genegeerd de uitgever waarde helemaal af.

In de tenant van meerdere steekproeven u in de sectie [Gerelateerde inhoud](#related-content) aan het einde van dit artikel vindt, uitgever validatie uitgeschakeld zodat een Azure AD-tenant aan te melden.

Nu eens de gebruikerservaring voor gebruikers die zich wilt met meerdere tenant-toepassingen aanmelden.

## <a name="understanding-user-and-admin-consent"></a>Lidmaatschap gebruiker en beheerder toestemming
Voor een gebruiker te melden bij een toepassing in Azure AD, moet de toepassing worden weergegeven in de tenant van de gebruiker.  Hierdoor wordt de organisatie acties zoals unieke beleidsregels van toepassing als gebruikers van de tenant zich bij de toepassing aanmelden uit te voeren.  Voor een toepassing één tenant is deze registratie eenvoudig; Dit is de die optreedt wanneer u de toepassing in de [portal van Azure klassieke registreren][AZURE-classic-portal].

Voor een toepassing meerdere tenant, worden de oorspronkelijke registratie voor de toepassing opgeslagen in de Azure AD-tenant die worden gebruikt door de ontwikkelaar.  Wanneer een gebruiker van een andere tenant zich aanmeldt bij de toepassing voor de eerste keer Azure AD wordt gevraagd om ze toe te staan de machtigingen die door de toepassingen aangevraagd.  Als ze toestemming, klikt u vervolgens een weergave van de toepassing met de naam van een *service principal* is gemaakt in de tenant van de gebruiker en aanmelden kunt blijven. Een delegatie is ook in de map die de records van de gebruiker akkoord met de toepassing gemaakt. Zie [objecten toepassing en Service Principal objecten] [ AAD-App-SP-Objects] voor meer informatie over van de toepassing-toepassing en ServicePrincipal objecten en hoe ze zich verhouden tot elkaar.

![Toestemming voor niet-gelaagde-app][Consent-Single-Tier] 

Deze toestemming-ervaring wordt beïnvloed door de machtigingen die door de toepassingen aangevraagd.  Azure AD ondersteunt twee soorten machtigingen app alleen-lezen en gedelegeerd:

- Een gedelegeerde machtiging verleent een toepassing de mogelijkheid om op te treden als een gebruiker aangemeld voor een subset van de zaken aan bod de gebruiker kan doen.  Zoals kunt u een toepassing de gedelegeerd toestemming verlenen om te lezen van de agenda van de aangemeld gebruiker.
- Een app-lezen-toegang wordt verleend rechtstreeks naar de identiteit van de toepassing.  Bijvoorbeeld: u kunt een toepassing te verlenen app alleen-lezen machtigingen voor het lezen van de lijst met gebruikers in een tenant en deze kunnen hiervoor ongeacht wie is aangemeld bij de toepassing.

Sommige machtigingen kunnen worden ingestemd met door een gewone gebruiker, terwijl de anderen van een tenantbeheerder toestemming vereisen. 

### <a name="admin-consent"></a>Beheerder toestemming
App alleen-lezen machtigingen vereist altijd toestemming van een tenantbeheerder.  Als uw toepassing wordt gevraagd een machtiging app alleen-lezen en normale gebruiker probeert aan te melden bij de toepassing, is uw toepassing krijgt een foutmelding dat de gebruiker niet kan worden toestemming.

Bepaalde gedelegeerde machtigingen vereist ook toestemming van een tenantbeheerder.  Bijvoorbeeld nodig de mogelijkheid om terug te schrijven naar Azure AD als de gebruiker aangemeld toestemming om een tenantbeheerder.  Als alleen-app-machtigingen, als een gewone gebruiker probeert te melden bij een toepassing die een gedelegeerd machtiging die moeten worden beheerder toestemming, vraagt wordt uw toepassing een foutbericht.  Een machtigingsniveau is al dan niet vereist beheerder toestemming wordt bepaald door de ontwikkelaar die de resource die zijn gepubliceerd en kan worden gevonden in de documentatie voor de resource.  Koppelingen naar informatie over de beschikbare machtigingen voor de API Azure AD Graph en Microsoft Graph-API zijn in de sectie [Gerelateerde inhoud](#related-content) van dit artikel.

Als uw toepassing machtigingen die beheerder toestemming moeten worden gebruikt, moet u beschikken over een beweging in uw toepassing, bijvoorbeeld een knop of de koppeling waar de beheerder de actie kunt wordt gestart.  Het verzoek uw toepassing verzendt voor deze actie is een gebruikelijke OAuth2/OpenID verbinding autorisatieaanvraag, maar bevat die ook de `prompt=admin_consent` tekenreeks queryparameter.  Nadat de beheerder heeft ingestemd en de hoofdsom service wordt gemaakt in de tenant van de klant, latere aanmelden aanvragen hoeft niet het `prompt=admin_consent` parameter.   Aangezien de beheerder heeft besloten dat de gevraagde machtigingen zijn toegestaan, wordt er geen andere gebruikers in de tenant gevraagd om toestemming vanaf dat moment.

De `prompt=admin_consent` parameter kan ook worden gebruikt door de toepassingen die machtigingen die geen beheerder toestemming vereist, maar wilt geven een ervaring waar de tenant-beheerder "zich registreert" aanvragen voor de toepassing één keer en geen andere gebruikers wordt gevraagd om toestemming vanaf dat punt op.

Als beheerder toestemming nodig om een toepassing en de beheerder zich aanmeldt bij de toepassing, maar de `prompt=admin_consent` parameter wordt niet verzonden kan de beheerder is mogelijk wel akkoord met de toepassing, maar ze worden alleen toestemming voor hun gebruikersaccount.  Gewone gebruikers nog steeds kunnen niet aanmelden en akkoord met de toepassing.  Dit is handig als u de tenantbeheerder de mogelijkheid om te verkennen van uw toepassing wilt voordat het feit dat andere gebruikers toegang geven.

Een tenantbeheerder kan de mogelijkheid om normale gebruikers toestemming naar toepassingen uitschakelen.  Als deze functie is uitgeschakeld, is altijd beheerder toestemming vereist voor de toepassing worden ingesteld in de tenant.  Als u testen van de toepassing met toestemming van de normale gebruiker uitgeschakeld wilt, vindt u de schakeloptie configuratie in de Azure AD-tenant bij configuratie van de [Azure klassieke portal][AZURE-classic-portal].

> [AZURE.NOTE] Sommige toepassingen wilt een ervaring waar gewone gebruikers kunnen in eerste instantie toestemming en later de beheerder en vraag machtigingen waarvoor beheerder toestemming kunt gebruikmaakt van de toepassing.  Er is geen manier hiervoor met de registratie van een toepassing voor eenmalige in Azure AD vandaag.  De komende Azure AD v2 eindpunt verzoek machtigingen gedurende runtime,-toepassingen kunt in plaats van registratie keer, zodat dit scenario.  Zie voor meer informatie de [Handleiding voor ontwikkelaars van Azure AD App-Model v2][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Toepassingen van toestemming en met meerdere niveaus
Uw toepassing heeft mogelijk meerdere niveaus, elk vertegenwoordigd door een eigen registratie in Azure AD.  Een systeemeigen die een web API-oproepen, of een webtoepassing oproepen die bijvoorbeeld een web API.  In beide gevallen vraagt de client (systeemeigen app of WebApp) machtigingen om te bellen van de resource (web API).  Voor de client naar worden doorverbonden ingestemd in de tenant van een klant, worden alle resources waaraan deze machtigingen daarom moeten al bestaan in de tenant van de klant.  Als deze voorwaarde niet is voldaan, retourneert Azure AD een fout dat de resource eerst moet worden toegevoegd.

Dit is een probleem als de logische toepassing uit twee of meer toepassing registraties, bijvoorbeeld een afzonderlijke client en een resource bestaat.  Hoe krijgt u de resource in de tenant van de klant eerste?  Azure AD behandelt deze zaak doordat client- en resourcekalenders worden ingestemd in één stap, waar de gebruiker het totaal van de machtigingen aangevraagd door de client en de bron op de pagina toestemming ziet.  Schakel dit gedrag door registratie van de toepassing van de resource-ID van de klant-App als moet bevatten een `knownClientApplications` in het manifest van de toepassing.  Bijvoorbeeld:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Deze eigenschap kan worden bijgewerkt via de resource [van toepassing manifest][AAD-App-Manifest], en wordt uitgelegd in een met meerdere niveaus native client web API steekproef bellen in de sectie [Gerelateerde inhoud](#related-content) aan het einde van dit artikel. Het onderstaande diagram biedt een overzicht van toestemming voor een app met meerdere niveaus:

![Instemming met meerdere niveaus bekende client-app][Consent-Multi-Tier-Known-Client] 

Een soortgelijke zaak gebeurt er als de andere niveaus van een toepassing in verschillende tenants zijn geregistreerd.  Stel de hoofdletters/kleine letters van het samenstellen van een systeemeigen clienttoepassing die de Office 365 Exchange Online-API-oproepen.  Als u wilt de native toepassing is, en later voor de bijbehorende toepassing uitvoeren in de tenant van een klant ontwikkelt, moet de Exchange Online service-hoofdsom aanwezig zijn.  In dit geval heeft de klant Exchange Online kopen voor de service principal moet worden gemaakt in de tenant is ingeschakeld.  In het geval van een API die door een organisatie dan Microsoft is gemaakt, moet de ontwikkelaar van de API zelf een formule voor hun klanten toe te staan hun toepassing in de tenant van een klant, bijvoorbeeld een webpagina die stations toestemming met de regelingen in dit artikel beschreven.  Nadat de hoofdsom voor de service is gemaakt in de tenant, gaat de oorspronkelijke toepassing tokens voor de API.

Het onderstaande diagram biedt een overzicht van toestemming voor een app met meerdere niveaus is geregistreerd in verschillende tenants:

![Toestemming naar meerdere personen app met meerdere niveaus][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Toestemming intrekken
Gebruikers en beheerders kunnen toestemming voor uw toepassing op elk gewenst moment intrekken:

- Gebruikers toegang tot afzonderlijke toepassingen, intrekken door ze te verwijderen uit hun [Access Configuratiescherm toepassingen] [ AAD-Access-Panel] lijst.
- Beheerders van de toegang tot toepassingen, intrekken door ze te verwijderen uit Azure AD met in het gedeelte voor het beheer van Azure AD van de [Azure klassieke portal][AZURE-classic-portal].

Als een beheerder akkoord met een aanvraag voor alle gebruikers in een tenant gaat, intrekken niet gebruikers afzonderlijk toegang.  Alleen de beheerder kan toegang intrekken en alleen voor de gehele toepassing.

### <a name="consent-and-protocol-support"></a>Toestemming en protocolondersteuning
Toestemming wordt ondersteund in Azure AD via het OAuth, OpenID verbinding kunt maken, WS-Federation en SAML protocollen.  De protocollen SAML en WS-Federation bieden geen ondersteuning voor de `prompt=admin_consent` parameter, zodat een beheerder toestemming is alleen mogelijk via OAuth en OpenID verbinding te maken.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Meerdere Tenant-toepassingen en Caching van Access Tokens
Meerdere tenant-toepassingen kunnen access tokens bellen API's die zijn beveiligd met Azure AD ook openen.  Een algemene fout bij het gebruik van de Active Directory verificatie bibliotheek (ADAL) met een toepassing voor meerdere tenant is in eerste instantie aanvragen van een token voor een gebruiker met/Common, ontvangt een antwoord en vraag een latere token voor die dezelfde gebruiker ook/Common gebruikt.  Aangezien het antwoord van Azure AD afkomstig is van een tenant, niet/algemene, ADAL slaat het token als afkomstig van de tenant. De volgende oproep door naar/Common om een toegangstoken voor de gebruiker missers vermelding in de cache en de gebruiker wordt gevraagd opnieuw aanmelden.  Als u wilt voorkomen dat de cache ontbreekt, zorg mislukken volgende oproepen voor een gebruiker al is aangemeld in van de tenant eindpunt worden aangebracht.

## <a name="related-content"></a>Verwante onderwerpen

- [Voorbeelden van meerdere tenant-toepassing][AAD-Samples-MT]
- [Richtlijnen voor toepassingen huisstijl][AAD-App-Branding]
- [Handleiding voor Azure AD-ontwikkelaars][AAD-Dev-Guide]
- [Toepassingsobjecten en Service Principal objecten][AAD-App-SP-Objects]
- [Toepassingen integreren met Azure Active Directory][AAD-Integrating-Apps]
- [Overzicht van het kader toestemming][AAD-Consent-Overview]
- [Microsoft Graph API machtiging bereiken][MSFT-Graph-AAD]
- [Azure AD Graph API machtiging bereiken][AAD-Graph-Perm-Scopes]

Gebruik de onderstaande Disqus opmerkingen sectie om feedback geven en help ons te verfijnen en de inhoud van vorm.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














