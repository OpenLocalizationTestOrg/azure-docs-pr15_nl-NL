<properties
    pageTitle="Verificatie en machtiging voor API-Apps in Azure App Service | Microsoft Azure"
    description="Meer informatie over de verificatie en machtiging services die Azure App-Service voor API-Apps biedt."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Verificatie en machtiging voor API-Apps in Azure App-Service

## <a name="overview"></a>Overzicht 

> [AZURE.NOTE] In dit onderwerp worden gemigreerd naar een geconsolideerde [App Service verificatie / autorisatie](../app-service/app-service-authentication-overview.md) onderwerp Web, Mobile en API-Apps behandelt.

Azure-App-Service biedt ingebouwde services voor verificatie en machtiging die [OAuth 2.0](#oauth) en [Verbindt u OpenID](#oauth)implementeren. In dit artikel worden de services en de opties die beschikbaar voor API-Apps in Azure App-Service zijn.

In het volgende diagram ziet u enkele belangrijke kenmerken van de App Service verificatie:

* Deze verwerkt verzoeken API voor oproepen, wat betekent dat het werkt met alle taal en framework wordt ondersteund door de App Service.
* Hebt u verschillende opties voor hoeveel verificatie werk dat u wilt doen in uw eigen code.
* Dit werkt voor zowel de eindgebruiker als de verificatie van de service-account. 
* Vijf identiteitsprovider wordt ondersteund: Azure Active Directory, Facebook, Twitter, Google en Microsoft-Account.
* Deze werkt op dezelfde voor API-Apps, Web Apps en Mobile-Apps.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Taal agnostic

App Service verificatie verwerking gebeurt voordat uw API-app, wat betekent dat de verificatiefuncties werken voor API-apps geschreven in een taal of framework aanvragen hebt bereikt.  Uw API worden gebaseerd op ASP.NET, Java, Node.js of een framework die ondersteuning biedt voor App-Service.

App Service wordt doorgegeven aan de JSON web token (JWT) in de koptekst van de verificatie van een HTTP-aanvraag en code geschreven in een taal of framework kan de benodigde gegevens ophalen uit het token. Daarnaast biedt App Service u gemakkelijker toegang tot de meest gebruikte claims door in te stellen van bepaalde speciale kopteksten, zoals de volgende:

* X-MS-CLIENT-HOOFDSOM-NAME
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
In een .NET-API, kunt u de `Authorize` kenmerk en code op basis van claims voor fijnmazige autorisatie u eenvoudig kunt schrijven omdat in .NET klassen claims informatie voor u is ingevuld.

## <a name="multiple-protection-options"></a>Meerdere beveiligingsopties

App-Service kan voorkomen dat anonieme HTTP-aanvragen kunnen bereiken van uw app API, dit kunt geven op alle aanvragen en valideren tokens voor aanvragen die ze bevatten of kunnen door alle aanvragen zonder dat u geen actie op deze:

1. Alleen geverifieerde aanvragen toestaan om uw API-app hebt bereikt.

    Als een anonieme aanvraag is ontvangen van een browser, App Service worden omgeleid naar een aanmeldingspagina voor verificatie (Azure AD, Google, Twitter, enzovoort) die u kiest. 

    Met deze optie wordt u niet nodig hebt verificatiecode te schrijven helemaal in uw app en autorisatiecode is vereenvoudigd omdat de belangrijkste claims u in de HTTP-headers vindt.

2. Alle aanvragen toestaan om bereiken van uw API-app, maar valideren geverifieerde aanvragen en geven verificatie-informatie in de koptekst van de HTTP.

    Deze optie zorgt ervoor dat u meer flexibiliteit bij het afhandelen van anonieme aanvragen, maar u moet programmacode schrijven of u wilt voorkomen dat anonieme gebruikers uw-API gebruiken. Aangezien de populairste claims worden doorgegeven in de koppen van HTTP-aanvragen, is autorisatiecode relatief eenvoudig.
    
3. Toestaan dat alle aanvragen voor toegang tot de API, geen actie ondernemen in verificatie-informatie in de aanvragen.

    Deze optie blijven de taken van verificatie en machtiging geheel tot aan uw toepassingscode.

Klik in de [portal van Azure](https://portal.azure.com/)die u selecteert de gewenste optie in de **verificatie / autorisatie** blade.

![](./media/app-service-api-authentication/authblade.png)

Opties 1 en 2 voor **Verificatie van de App-Service**inschakelen en kies in de vervolgkeuzelijst **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** **Meld u aan** of **aanvraag toestaan (geen actie)**.  Als u **zich aanmeldt**, die u moet kiezen een verificatieprovider en configureren van die provider.

![](./media/app-service-api-authentication/actiontotake.png)

Lees [hoe u het configureren van uw App-servicetoepassing gebruik van de Azure Active Directory-aanmelding](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)voor gedetailleerde informatie over het configureren van verificatie. Het artikel is van toepassing op API-apps, evenals de mobiele apps en deze koppelingen naar andere artikelen voor de andere verificatieproviders.
 
## <a id="internal"></a>Verificatie van de service-account

Verificatie van App-Service werkt voor interne scenario's zoals voor het bellen van de ene API-toepassing naar een andere API-app. In dit scenario kunt u geen token ophalen met behulp van referenties voor een serviceaccount in plaats van de eindgebruiker referenties. Een serviceaccount wordt ook wel een *service principal* in Azure Active Directory en verificatie via een dergelijke account wordt ook wel een scenario voor de service-naar-service. 

Voor de service-naar-service scenario's, de genoemd API-app beveiligen met behulp van Azure Active Directory en geef een token AAD service principal autorisatie wanneer u de app API belt. Krijgt u een token doordat de client-ID en geheime van de configuratietoepassing AAD-client. Geen speciale Azure alleen-lezen-code is vereist, zoals die worden gebruikt voor het afhandelen van het token Mobile Services Zumo waar. Een voorbeeld van dit scenario werken met ASP.NET-API-apps valt onder de zelfstudie [Service principal verificatie voor API-Apps](app-service-api-dotnet-service-principal-auth.md).

Als u een service-naar-service-scenario verwerken wilt zonder App Service verificatie, kunt u certificaten of basisverificatie. Voor informatie over clientcertificaten in Azure wordt aangegeven, Zie [Hoe naar configureren TLS onderlinge verificatie voor Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Zie voor informatie over basisverificatie in ASP.NET [Verificatie Filters in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Verificatie in een service-account van een App Service logica app aan een API-app is een speciale geval die wordt uitgelegd in [werken met uw aangepaste API die worden gehost op App-Service met logica apps](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobiele clientverificatie

Zie de [documentatie over verificatie voor de mobiele apps](../app-service-mobile/app-service-mobile-ios-get-started-users.md)voor informatie over hoe u omgaat met verificatie van mobiele clients. Verificatie van App-Service werkt op dezelfde manier voor mobiele apps en API-apps.
  
## <a name="more-information"></a>Meer informatie

Zie de volgende bronnen voor meer informatie over verificatie en autorisatie in Azure App-Service:

* [Groeiende App Service verificatie / autorisatie](/blog/announcing-app-service-authentication-authorization/)
* [Het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Inclusief koppelingen voor andere verificatieproviders boven aan de pagina.) 

Zie de volgende bronnen voor meer informatie over het OAuth 2.0, OpenID verbinding en JSON Web Tokens (JWT).

* [Aan de slag met OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Aan de slag met OAuth 2.0") 
* [Inleiding tot OAuth2, OpenID verbinding en JSON Web Tokens (JWT) - PluralSight cursus](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Bouwen en beveiligen van een RESTful API voor meerdere klanten in ASP.NET - PluralSight cursus](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Zie de volgende bronnen voor meer informatie over Azure Active Directory.

* [Azure AD-scenario 's](http://aka.ms/aadscenarios)
* [Azure AD ontwikkelaars handleiding](http://aka.ms/aaddev)
* [Azure AD-voorbeelden](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Volgende stappen

In dit artikel is verificatie en machtiging functies van App-Service die u voor API-apps gebruiken kunt beschreven. De volgende zelfstudie in de ophalen gestart reeks ziet hoe u het implementeren van [gebruikersverificatie in App Service API-Apps](app-service-api-dotnet-user-principal-auth.md).
