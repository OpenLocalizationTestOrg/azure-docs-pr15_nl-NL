<properties
   pageTitle="Handleiding voor Azure Active Directory-ontwikkelaars | Microsoft Azure"
   description="Dit artikel bevat een uitgebreide handleiding voor ontwikkelaars-georiënteerd resources voor Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Handleiding voor Azure Active Directory-ontwikkelaars

## <a name="overview"></a>Overzicht
Als een identiteitsbeheer als een platform service (IDMaaS) biedt Azure Active Directory (AD) een efficiënte manier om het identiteitsbeheer integreren in hun toepassingen ontwikkelaars. De volgende artikelen vindt u overzichten op implementatie en de belangrijkste functies van Azure AD. Het is raadzaam dat u in de volgorde lezen of naar het [aan de slag](#getting-started) gaan als u klaar bent om te verdiepen.


1. [De voordelen van Azure AD-integratie](./develop/active-directory-how-to-integrate.md): Ontdek waarom integratie met Azure AD biedt de beste oplossing voor secure aanmelden en machtiging.

1. [Azure AD-verificatie-scenario's](active-directory-authentication-scenarios.md): profiteren van vereenvoudigd verificatie in Azure AD te leveren aanmelding in uw toepassing.

1. [Integratie van toepassingen met Azure AD](active-directory-integrating-applications.md): meer informatie over het toevoegen, bijwerken en verwijderen van toepassingen van Azure AD en voor de bestaande huisstijl richtlijnen voor geïntegreerde apps.

1. [Azure AD Graph API](active-directory-graph-api.md): de Azure AD Graph-API gebruiken via programmacode toegang tot Azure AD via REST API-eindpunten. De API Azure AD Graph is ook toegankelijk is via [Microsoft Graph](https://graph.microsoft.io/). Microsoft Graph biedt een geïntegreerd API waarmee toegang tot meerdere Microsoft-cloudservice API's, via een enkele REST API-eindpunt en met een enkel toegangstoken.

1. [Azure AD-verificatie bibliotheken](active-directory-authentication-libraries.md): eenvoudig worden gebruikers geverifieerd om toegangstokens verkrijgen met behulp van Azure AD-verificatie bibliotheken voor .NET, JavaScript, doel-C, Android en meer.


## <a name="getting-started"></a>Aan de slag

Deze zelfstudies zijn afgestemd op meerdere platforms en kunnen u snel starten ontwikkelen met Azure Active Directory. Als een vereiste moet u [een Azure Active Directory-tenant ophalen](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobile en PC-toepassing snel aan de slag handleidingen

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android-apparaat](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android-apparaat](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windows Universal](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Rechtstreeks te integreren met OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web application snel aan de slag handleidingen

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID verbinding maken](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Rechtstreeks te integreren met OpenID verbinding maken](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Web API snel aan de slag handleidingen

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Query's uitvoeren de directory-handleiding

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph-API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Uitleg

Deze artikelen wordt uitgelegd hoe u specifieke taken uitvoeren met behulp van Azure Active Directory:

- [Een Azure AD-tenant ophalen](active-directory-howto-tenant.md)
- [Meld u aan elke Azure AD-gebruiker met het patroon dat in meerdere tenant-toepassing](active-directory-devhowto-multi-tenant-overview.md)
- Cross-app SSO met ADAL, klik op [Android](active-directory-sso-android.md) en op apparaten met [iOS](active-directory-sso-ios.md) inschakelen
- [Controleer uw toepassing AppSource gecertificeerd voor Azure AD](active-directory-devhowto-appsource-certified.md)
- [Lijst van uw toepassing in de galerie met Azure AD-toepassing](active-directory-app-gallery-listing.md)
- [WebApps voor Office 365 aan het Dashboard verkoper indienen](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Meer informatie over de manifest van de Azure Active Directory-toepassing](active-directory-application-manifest.md)
- [Meer informatie over de bestaande huisstijl richtlijnen voor het aanmelden en app acquisition knoppen in de clienttoepassing](active-directory-branding-guidelines.md)
- [Voorbeeld: Het maken van de apps die Meld u aan gebruikers met zowel persoonlijke & werk of school-account](active-directory-appmodel-v2-overview.md)
- [Voorbeeld: Het maken van de apps die zich registreren en aanmelden op consumenten](../active-directory-b2c/active-directory-b2c-overview.md)
- [Preview: token levensduur configureren in Azure AD](active-directory-configurable-token-lifetimes.md) via PowerShell. Zie [beleid bewerkingen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) en het [beleid entiteit](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) voor meer informatie over het configureren van via de API Azure AD Graph.

## <a name="reference"></a>Verwijzing

Deze artikelen voorzien in een verwijzing foundation REST en verificatie bibliotheek API's, protocollen fouten, codevoorbeelden en eindpunten.  

###  <a name="support"></a>Ondersteuning
- [Labels vragen](http://stackoverflow.com/questions/tagged/azure-active-directory): oplossingen van de Azure Active Directory zoeken op de stapel overloop door te zoeken naar de labels [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) en [adal](http://stackoverflow.com/questions/tagged/adal).
- Zie de [woordenlijst voor ontwikkelaars van Azure AD](active-directory-dev-glossary.md) voor definities van de veelgebruikte voorwaarden voor het ontwikkelen van toepassingen en -integratie.

### <a name="code"></a>Code

- [Azure Active Directory open source bibliotheken](http://github.com/AzureAD): de eenvoudigste manier om te zoeken van een bibliotheek bron is met behulp van onze [lijst bibliotheek](active-directory-authentication-libraries.md).

- [Voorbeelden van Azure Active Directory](https://github.com/azure-samples?query=active-directory): de eenvoudigste manier om te navigeren van de lijst met voorbeelden is met behulp van de [index van voorbeelden van programmacode](active-directory-code-samples.md).

- [Active Directory verificatie bibliotheek (ADAL) voor .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - documentatie is beschikbaar voor zowel [de meest recente versie van de belangrijkste](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) als de [de vorige belangrijke versie](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph-API

- [Graph API-verwijzing](https://msdn.microsoft.com/library/azure/hh974476.aspx): overzicht van de REST van de Azure Active Directory Graph API. [De interactieve Graph API verwijzing ervaring weergeven](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph API machtiging bereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 machtiging bereiken die worden gebruikt voor het toegangsbeheer dat een app met directory-gegevens in een tenant heeft.

### <a name="authentication-and-authorization-protocols"></a>Verificatie en machtiging protocollen

- [Toets Rollover ondertekenen in Azure AD](active-directory-signing-key-rollover.md): meer informatie over Azure AD sleutelrollover cadence en het bijwerken van de sleutel voor de meest voorkomende toepassing-scenario's ondertekenen.

- [OAuth 2.0-protocol: met het verlenen van de code autorisatie](active-directory-protocols-oauth-code.md): U kunt het OAuth 2.0-protocol autorisatie code verlenen, gebruiken om toegang tot webtoepassingen Autoriseer en Web-API's in uw Azure Active Directory-tenant.

- [OAuth 2.0-protocol: informatie over de impliciete verlenen](active-directory-dev-understanding-oauth2-implicit-grant.md): meer informatie over het verlenen impliciete autorisatie en of het juiste voor uw toepassing is.

- [OAuth 2.0-protocol: Service-Service oproepen gebruiken clientreferenties](active-directory-protocols-oauth-service-to-service.md): de referenties voor de OAuth 2.0-Client verlenen toestemming geeft dat een webservice (een vertrouwelijke client) een eigen referenties gebruiken om te verifiëren bij het aanroepen van een andere webservice, in plaats van een gebruiker simuleren. In dit scenario de client is meestal een middelste laag webservice, een daemon-service of website.

- [OpenID verbinding 1.0-protocol: aanmelden en verificatie](active-directory-protocols-openid-connect-code.md): de OpenID verbinding 1.0-protocol breidt de mogelijkheden van OAuth 2.0 voor gebruik als een verificatieprotocol. Een clienttoepassing kunt ontvangen van een id_token als u wilt het aanmeldingsproces beheren of de stroom van de code autorisatie voor het ontvangen van een id_token en de machtiging code verbeteren.

- [Op SAML 2.0 protocol verwijzing](active-directory-saml-protocol-reference.md): het SAML 2.0-protocol kan toepassingen een functionaliteit voor eenmalige aanmelding aan hun gebruikers te verstrekken.

- [WS-Federation 1.2 protocol](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory WS-Federation 1.2 aan de hand van de Web Services Federatie versie 1.2 specificatie ondersteunt. Voor meer informatie over het document van de metagegevens Federatie, raadpleegt u [Federatie metagegevens](active-directory-federation-metadata.md).

- [Ondersteunde typen voor token en claimen](active-directory-token-and-claims.md): U kunt deze handleiding gebruiken om te begrijpen en de claims in de tokens SAML 2.0 en JSON Web Tokens (JWT) als resultaat.

## <a name="videos"></a>Video 's

### <a name="build"></a>Opbouwen

Deze presentaties overzicht over het ontwikkelen van apps met behulp van de luidsprekers van de Azure Active Directory-functie die rechtstreeks in het team voor technische werken. De presentaties duidelijk fundamentele onderwerpen, inclusief IDMaaS, verificatie, identiteitsfederatie en eenmalige aanmelding.

- [Microsoft-id: Status van de Europese Unie en toekomstige richting](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identiteitsbeheer als een service voor moderne toepassingen](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Moderne webtoepassingen met Azure Active Directory ontwikkelen](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Moderne systeemeigen toepassingen met Azure Active Directory ontwikkelen](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure vrijdag
[Azure vrijdag](https://azure.microsoft.com/documentation/videos/azure-friday/) is een terugkerende vrijdag videoreeks 1:1 die gewijd aan te brengen u korte (10 – 15 minuten) met experts op allerlei Azure onderwerpen analyseren.  Gebruik de functie Services Filter op de pagina om alle Azure Active Directory-video's weer te geven.

- [Azure identiteit 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure identiteit 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure identiteit 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Sociale

- [Active Directory-teamblog](http://blogs.technet.com/b/ad/): de meest recente ontwikkeling van de wereld van Azure Active Directory.

- [Teamblog van Azure Active Directory Graph](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory-informatie die specifiek is voor de grafiek-API.

- [Cloud identiteit](http://www.cloudidentity.net): gedachten op identiteitsbeheer als een service, uit een principal Azure Active Directory PM.  

- [Azure Active Directory op Twitter](https://twitter.com/azuread): aankondigingen van de Azure Active Directory in 140 tekens bevatten.

## <a name="windows-server-on-premises-development"></a>Windows Server on-premises implementatie ontwikkelen
Voor hulp bij het gebruik van Windows Server en Active Directory Federation Services (ADFS) ontwikkeling, raadpleegt u:

- [AD FS-scenario's voor ontwikkelaars](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): biedt een overzicht van AD FS-onderdelen en hoe deze werkt, met meer informatie over de ondersteunde verificatie/autorisatie scenario's.
- [AD FS-scenario's](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): een lijst met de artikelen overzicht, die stapsgewijze instructies bieden over het implementeren van de gerelateerde verificatie/autorisatie loopt.
