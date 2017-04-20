<properties
    pageTitle="Typen van het eindpunt v2.0 | Microsoft Azure"
    description="De soorten apps en scenario's die worden ondersteund door het eindpunt van de v2.0 Azure AD."
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

# <a name="types-of-apps-for-the-v20-endpoint"></a>Soorten apps voor het eindpunt v2.0
Het eindpunt v2.0 ondersteunt verificatie voor een groot aantal moderne app architecturen, die zijn gebaseerd op de standaard industriestandaardprotocollen [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) en/of [OpenID verbinding maken](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  In dit document een korte beschrijving van de soorten apps die u kunt maken, onafhankelijk van de taal of platform u liever.  Dit kunt u meer informatie over het hoogste niveau scenario's voordat u [meteen in de code](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De basisbeginselen
Elke app waarin het eindpunt v2.0 moet worden geregistreerd bij [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Het registratieproces app verzamelen & enkele waarden toewijzen aan uw app:

- Een **Toepassings-Id** die deze identificeert op uw app
- Een **URI omleiden** die kunnen worden gebruikt om te antwoorden terug naar uw app sturen
- Een paar andere scenario-specifieke waarden.  Informatie voor meer informatie over hoe u [een app registreert](active-directory-v2-app-registration.md).

Zodra geregistreerd, wordt de status van de app communiceert met Azure AD door te sturen van aanvragen naar het eindpunt van de v2.0 Azure Active Directory.  Wij bieden bron openen kaders & bibliotheken waarin de details van deze aanvragen kunt verrichten, of u kunt ook zelf de logica verificatie implementeren door aanvragen voor deze eindpunten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>WebApps
Voor WebApps (.NET, PHP, Java, Ruby, Python, knooppunt, enzovoort) die zijn toegankelijk via een browser, kunt u gebruiker uitvoeren aanmeldingsproblemen met [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  In het OpenID verbinding maken met de web-app ontvangt een `id_token`, een beveiligingstoken dat wordt gecontroleerd met de gebruikers id en vindt u informatie over de gebruiker in de vorm van claims:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

U kunt meer informatie over alle typen van tokens en claims die beschikbaar zijn in een app in de [v2.0 token verwijzing](active-directory-v2-tokens.md).

In WebApps-server duurt de stroom aanmeldingsproblemen verificatie hoog niveau als volgt:

![Afbeelding van de zwembanen Web App](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

De validatie van de id_token met een openbare ondertekend sleutel hebt ontvangen van het eindpunt v2.0 is voldoende om te zorgen dat de gebruikers id en instellen van een sessiecookie die kan worden gebruikt om de gebruiker op aanvragen van de volgende pagina te identificeren.

Dit scenario in actie, bekijkt u een van de web-app aanmelden codevoorbeelden in onze sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .

Naast eenvoudige aanmelden, een server WebApp mogelijk ook moeten toegang hebben tot enkele andere webservice zoals een REST API.  In dit geval kunt de server WebApp deelnemen aan een gecombineerde OpenID verbinden & OAuth 2.0 stroom, met de [stroom van OAuth 2.0 autorisatie-Code](active-directory-v2-protocols.md#oauth2-authorization-code-flow). Dit scenario valt onder onze [onderwerp WebApp-WebAPI aan de slag](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web-API 's
U kunt het eindpunt v2.0 secure webservices, zoals van uw app RESTful Web API.  In plaats van id_tokens en sessie cookies Web API's met OAuth 2.0 access_tokens hun gegevens beveiligen en te verifiëren verzoeken voor oproepen.  De beller van een Web-API wordt een access_token in de koptekst autorisatie van een HTTP-aanvraag:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De Web API kunt vervolgens de access_token gebruiken om te controleren van de beller API-identiteit en informatie over de beller uit claims die op de access_token gecodeerd halen.  U kunt meer informatie over alle typen van tokens en claims die beschikbaar zijn in een app in de [v2.0 token verwijzing](active-directory-v2-tokens.md).

Een Web-API kunt gebruikers de kracht en opt-in/opt-out van bepaalde gegevens of functionaliteit geven door machtigingen, ook bekend als [bereiken](active-directory-v2-scopes.md)weergeeft.  Voor een bellen-app om machtigingen voor een bereik, moet de gebruiker aan de scope toestemming tijdens een stroom.  Het eindpunt v2.0 wordt kunt verrichten waarin de gebruiker wordt gevraagd om toestemming en opnemen van deze machtigingen in alle access_tokens die de Web API ontvangt.  Alle de Web-API moet bang is de access_tokens deze elk gesprek ontvangt valideren en uitvoering van de juiste machtiging controles.

Een Web-API kunt access_tokens ontvangen van alle typen apps, inclusief server-WebApps, desktop en mobiele apps, apps van één pagina, server kant daemons en zelfs andere Web-API's.  De hoogste niveau stroom voor web api-verificatie is als volgt:

![Afbeelding van de zwembanen web API](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Lees meer informatie over authorization_codes, refresh_tokens en de gedetailleerde stappen voor het ophalen van access_tokens van het [OAuth 2.0-protocol](active-directory-v2-protocols-oauth-code.md).

Voor meer informatie over het beveiligen van een web api met OAuth2 access_tokens, raadpleegt u de voorbeelden van web-api-code in onze [sectie aan de slag](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile en systeemeigen apps
Apps die zijn geïnstalleerd op een apparaat, zoals mobiele en bureaublad-apps, moeten vaak toegang hebben tot de backend-services of Web-API's die gegevens op te slaan en diverse functies namens een gebruiker uitvoeren.  Deze apps kunnen aanmelden en autorisatie toevoegen aan de backend-services met de [stroom van OAuth 2.0 autorisatie-Code](active-directory-v2-protocols-oauth-code.md).  

In deze stroom, een de app ontvangt een authorization_code van het eindpunt v2.0 na de gebruiker zich hebt aangemeld waarmee van de app-machtigingen om te bellen backend-services namens de gebruiker die momenteel is aangemeld.  De app kunt vervolgens de authoriztion_code op de achtergrond voor een access_token OAuth 2.0 en een refresh_token uitwisselen.  De app de access_token kunt gebruiken om te verifiëren Web API's in HTTP-aanvragen en de beschikking over de refresh_token zodat u een nieuwe access_tokens wanneer oudere verloopt.

![Afbeelding van de zwembanen systeemeigen-App](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Eén pagina apps (javascript)
Veel moderne apps hebben een één pagina App-WACHTWOORDVERIFICATIE front geschreven hoofdzakelijk in javascript en vaak gebruikt kaders, zoals AngularJS, Ember.js, Durandal, enzovoort.  Het eindpunt van Azure AD v2.0 ondersteunt deze apps met het [OAuth 2.0 impliciete Flow](active-directory-v2-protocols-implicit.md).

In deze stroom, ontvangt de app tokens van de v2.0 Autoriseer eindpunt rechtstreeks, zonder de uitvoering van een uitwisseling van de server naar server backend.  Hiermee kunt alle verificatie logica en afhandeling van de sessie te plaatsen volledig in de javascript-client, zonder uit te voeren extra pagina omleidingen.

![Afbeelding van de zwembanen impliciete stroom](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Dit scenario in actie, bekijkt u een van de voorbeelden van één pagina-app-code in onze sectie [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Daemons/server kant apps
Apps die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde resources, zoals Web-API's.  Deze apps kunnen verifiëren en ophalen met behulp van de app-identiteit (in plaats van een gebruiker gedelegeerd identiteit) met de OAuth 2.0-client tokens referenties stroom.

De app wordt in deze stroom, tokens opgehaald door direct interactief werken met de `/token` eindpunt:

![Afbeelding van de zwembanen daemon App](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Een daemon-app maken, raadpleegt u de client referenties documeenation in onze [Aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie of verwijzen naar [deze .NET-steekproef-app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Huidige beperkingen
Dit soort van apps worden momenteel niet ondersteund door het eindpunt v2.0, maar zijn op de roadmap.  Extra beperkingen en beperkingen voor het eindpunt v2.0 worden beschreven in de [v2.0 beperkingen artikel](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Gekoppelde web API's (op-namens-of)
Veel architecturen bevatten een Web-API die vereist zijn voor het bellen van een andere volgende Web API, beide beveiligd door het eindpunt v2.0.  Dit scenario geldt in systeemeigen clients die een backend Web API, die op zijn beurt een Microsoft Online-service, zoals Office 365 of de grafiek-API aanroept hebben.

Dit gekoppelde Web API-scenario kan worden ondersteund met het OAuth 2.0 Jwt vruchtdragende referentie verlenen, ook bekend als de [On-Behalf-Of Flow](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  De stroom aan-namens-Of is echter niet momenteel geïmplementeerd in het eindpunt v2.0.  Om te kijken hoe deze stroom werkt in het algemeen beschikbaar Azure AD service, raadpleegt u de [-op-namens-van-codevoorbeeld op GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
